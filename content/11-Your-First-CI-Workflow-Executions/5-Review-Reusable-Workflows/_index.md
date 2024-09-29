---
title : "Review Reusable Workflows"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 11.5 </b> "
---

Both the Release and Rollback workflows leverage other workflows as reusable jobs for efficiency:

- The *Validate version format* job is ideal for reuse in both workflows to ensure consistency when handling version inputs.
- The *Release* and *Rollback* jobs, which interact with AWS ECS and AWS CodeDeploy for deploying and rolling back project versions, can be streamlined into a reusable component through the Deploy workflow.

![0001](/images/11/5/0001.svg?featherlight=false&width=100pc)

![0002](/images/11/5/0002.svg?featherlight=false&width=100pc)

I follow a naming convention where reusable workflows are prefixed with *wc-\*.yml*, where *wc* stands for [workflow_call](https://docs.github.com/en/actions/sharing-automations/reusing-workflows).

Check out *.github/workflows/wc-deploy.yml* file.

```yml
#  This workflow will be called in release and rollback workflows
name: Deploy

on:
  workflow_call:
    inputs:
      rollback:
        description: "Set to true to perform a rollback deployment."
        default: false
        type: boolean
      download-artifact-name:
        description: "Name of the artifact to download for deployment."
        default: ""
        type: string
      download-artifact-path:
        description: "Path to store the downloaded artifact."
        default: ""
        type: string

      aws-region:
        description: "AWS region where resources are deployed."
        default: us-east-1
        type: string
      role-to-assume:
        description: "AWS IAM role to assume for deployment."
        required: true
        type: string

      ecr-repository:
        description: "ECR repository name where the Docker image is stored."
        required: true
        type: string
      image-tag:
        description: "Tag of the Docker image to deploy."
        default: latest
        type: string
      task-definition:
        description: "ECS task definition used for deployment."
        required: true
        type: string
      container-name:
        description: "Name of the container within the ECS task."
        required: true
        type: string
      ecs-cluster:
        description: "Name of the ECS cluster for deployment."
        required: true
        type: string
      ecs-service:
        description: "Name of the ECS service to update."
        required: true
        type: string

      codedeploy-application:
        description: "AWS CodeDeploy application name."
        required: true
        type: string
      codedeploy-application-group:
        description: "AWS CodeDeploy deployment group name."
        required: true
        type: string

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest
    env:
      allow-download-artifact: ${{ !inputs.rollback && inputs.download-artifact-name != '' && inputs.download-artifact-path != '' }}
    steps:
      - if: ${{ fromJSON(env.allow-download-artifact) }}
        name: Download artifact
        uses: actions/download-artifact@v4
        with:
          name: ${{ inputs.download-artifact-name }}
          path: ${{ inputs.download-artifact-path }}

      - if: ${{ fromJSON(env.allow-download-artifact) }}
        name: Load image
        run: docker load --input ${{ inputs.download-artifact-path }}/${{ inputs.download-artifact-name }}.tar

      - name: Checkout appspec.yml
        uses: actions/checkout@v4
        with:
          sparse-checkout: appspec.yml
          sparse-checkout-cone-mode: false

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-region: ${{ inputs.aws-region }}
          role-to-assume: ${{ inputs.role-to-assume }}

      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v2

      - name: Create image name
        id: image
        env:
          ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
          ECR_REPOSITORY: ${{ inputs.ecr-repository }}
          IMAGE_TAG: ${{ inputs.image-tag }}
        run: |
          echo "name=$ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG" >> $GITHUB_OUTPUT

      - if: ${{ !inputs.rollback }}
        name: Tag image for releasing
        run: docker tag localbuild/prepared-image:latest ${{ steps.image.outputs.name }}

      - if: ${{ !inputs.rollback }}
        name: Push Docker image to Amazon ECR
        run: docker push ${{ steps.image.outputs.name }}

      - if: ${{ inputs.rollback }}
        name: Pull Docker image from Amazon ECR
        run: docker pull ${{ steps.image.outputs.name }}

      - name: Download task definition ${{ inputs.task-definition }}
        run: |
          aws ecs describe-task-definition \
          --task-definition ${{ inputs.task-definition }} \
          --query taskDefinition > task-definition.json

      - name: Fill in the new image ID in the Amazon ECS task definition
        id: task-def
        uses: aws-actions/amazon-ecs-render-task-definition@v1
        with:
          task-definition: task-definition.json
          container-name: ${{ inputs.container-name }}
          image: ${{ steps.image.outputs.name }}

      - name: Deploy Amazon ECS task definition
        uses: aws-actions/amazon-ecs-deploy-task-definition@v1
        with:
          cluster: ${{ inputs.ecs-cluster }}
          service: ${{ inputs.ecs-service }}
          task-definition: ${{ steps.task-def.outputs.task-definition }}
          wait-for-service-stability: true
          codedeploy-appspec: appspec.yml
          codedeploy-application: ${{ inputs.codedeploy-application }}
          codedeploy-deployment-group: ${{ inputs.codedeploy-application-group }}
```

This GitHub Actions workflow is designed to manage deployments to AWS ECS and integrates rollback capabilities. It is intended to be invoked by other workflows (e.g., Release and Rollback workflows) through the *workflow_call* event trigger, which allows input parameters to be passed to it.


#### Events

**workflow_call**: it is called by other workflows, typically during a release or rollback process, with required inputs.

#### Job

**Deploy**

- Purpose: release or roll back the project based on conditions.
- Steps:
  - Downloads a deployment artifact only if not in rollback mode and if artifact inputs are provided.
  - Loads the downloaded Docker image into the Docker engine from the artifact.
  - Fetches the *appspec.yml* file, which is crucial for CodeDeploy to handle the deployment.
  - Configure AWS credentials.
  - Login to Amazon ECR.
  - Create image name.
  - Tag and Push Docker Image (for new releases).
  - Pull Docker image (for rollbacks).
  - Updates the ECS task definition with the new Docker image ID.
  - Deploys the updated ECS task definition to the specified ECS cluster and service. Uses AWS CodeDeploy for blue/green deployment, utilizing *appspec.yml* and linking it to the provided CodeDeploy application and deployment group.

Check out *.github/workflows/wc-validate-version-format.yml* file.

```yml
#  This workflow will be called in release and rollback workflows
name: Validate format of semantic version

on:
  workflow_call:
    inputs:
      version:
        description: "Specify semantic version tag of the Docker image to check."
        required: true
        type: string

jobs:
  validate-version-format:
    name: Validate semantic version format
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          sparse-checkout: |
            .github
          sparse-checkout-cone-mode: false

      - name: Create path to scripts
        id: scripts
        run: |
          echo "path=./.github/scripts" >> $GITHUB_OUTPUT

      - name: Validate format of semantic version ${{ inputs.version }}
        run: |
          chmod +x -R ./.github/scripts
          ${{ steps.scripts.outputs.path }}/validate-version-format.sh ${{ inputs.version }}
```

This GitHub Actions workflow is designed to validate the format of a semantic version string (for example, versions like 1.0.0) that is passed in as an input.

#### Events

**workflow_call**: it is called by other workflows, typically during a release or rollback process, with the required input *version*.

#### Job

**validate-version-format**

- Purpose: ensure that the version tag provided as an input follows the semantic versioning (semver) format.
- Steps:
  - Checkout the code.
  - This step creates a reference to the path where the scripts (specifically, *validate-version-format.sh*) are located within the *.github/scripts* directory.
  - This step runs a custom shell script to validate the format of the *version* input.

<!-- You now explore the reusable workflows.

#### "Validate format of semantic version" Workflow

Check out *.github/workflows/wc-validate-version-format.yml* file.

```
#  This workflow will be called in release and rollback workflows
name: Validate format of semantic version

on:
  workflow_call:
    inputs:
      version:
        description: "Specify semantic version tag of the Docker image to check."
        required: true
        type: string

jobs:
  validate-version-format:
    name: Validate semantic version format
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          sparse-checkout: |
            .github
          sparse-checkout-cone-mode: false

      - name: Create path to scripts
        id: scripts
        run: |
          echo "path=./.github/scripts" >> $GITHUB_OUTPUT

      - name: Validate format of semantic version ${{ inputs.version }}
        run: |
          chmod +x -R ./.github/scripts
          ${{ steps.scripts.outputs.path }}/validate-version-format.sh ${{ inputs.version }}
```

#### "Deploy" Workflow -->