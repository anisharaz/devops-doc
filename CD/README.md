# Continous Deployment (CD)
CD is the process of deploying the code to the production server automatically. This is done by the CI/CD pipeline. The pipeline is triggered when the code is pushed to the repository. The pipeline will build the code, run the tests, and deploy the code to the production server. This process is automated and reduces the manual intervention required to deploy the code to the production server. This ensures that the code is deployed to the production server quickly and efficiently.

The CD pipelines are configured using tools like Jenkins,Github Actions,Gitlab CI/CD, etc. These tools provide a way to define the pipeline stages and steps required to deploy the code to the production server. The pipeline can be configured to run the tests, build the code, and deploy the code to the production server.

## CD using github actions
Github actions is a CI/CD tool provided by Github. It allows you to define the CI/CD pipeline using a YAML file at directory `.github/workflows/pipeline.yaml` at the root of the project. The name of Yml file inside workflows directory can be any thing. Here is an example of a github actions pipeline that builds the code, runs the tests, and deploys the code to the production server:

> [!IMPORTANT]
> Docker container and docker compose is used in this CD pipeline to deploy the code to the production server. The docker container is built and pushed to the docker hub. The docker-compose file is used to deploy the code to the production server. Check this Doc for details [Here](/Docker)

```yaml
name: Push & Deploy 

on:
  push:
    branches:
      - "master"

jobs:
  docker_build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Set up QEMU
        uses: docker/setup-qemu-action@v3
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}
      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: aaraz/resourcesite:latest
          platforms: linux/amd64,linux/arm64
  deploy_to_server:
    runs-on: ubuntu-latest
    needs: docker
    steps:
      - name: Deploy
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SERVER_KEY }}
          script: |
            cd directory_of_compose_file && docker-compose down && docker-compose pull && docker-compose up -d
```
### Breakdown of the pipeline
- The pipeline is triggered when the code is pushed to the master branch. It is defined using the `on` keyword. Find More Docs [here](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#on)
- The pipeline has two jobs: `docker_build` and `deploy_to_server`. The `docker_build` job builds the code and pushes the docker image to the docker hub. The `deploy_to_server` job deploys the code to the production server.
- The `docker_build` job uses the pre written action to build and push the docker image to the docker hub.

    > The `actions/checkout@v4` action is used to clone the code from the repository to the runner.

    > The `docker/setup-qemu-action@v3` action is used to set up QEMU for the runner.

    > The `docker/setup-buildx-action@v3` action is used to set up Docker Buildx for the runner.

    > The `docker/login-action@v3` action is used to login to the docker hub.
    >> The `DOCKER_USERNAME` and `DOCKER_PASSWORD` are the secrets that are defined in the repository settings at `repository -> settings -> Secrets and Variable -> Actions`. These secrets are used to login to the docker hub.

    > The `docker/build-push-action@v5` action is used to build and push the docker image to the docker hub.
    >> The `push` is set to true to push the image to the docker hub, set this `false` if you want to just test the build process. The `tags` are the tags for the docker image. The `platforms` are the platforms on which the docker image will run.

- The `deploy_to_server` deploy the just built container to the production server.

    > The `appleboy/ssh-action@master` action is used to ssh into the production server.
    >> The `SERVER_HOST` (i.e ip address of server), `SERVER_USER` (i.e name of user to connect with), and `SERVER_KEY` (i.e ssh key for the user) are the secrets that are defined in the repository settings at `repository -> settings -> Secrets and Variable -> Actions`. These secrets are used to connect to the production server.The `script` is the commands that is run on the production server to deploy the code.
    >> - `cd directory_of_compose_file` is the directory where the docker-compose file is located.
    >> - `docker-compose down` stops the running containers.
    >> - `docker-compose pull` pulls the latest image from the docker hub.
    >> - `docker-compose up -d` again starts the service with new images.
 

