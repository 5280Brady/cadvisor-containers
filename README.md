# Container Advisor
Repository is used for building the cAdvisor binary that is used to
monitor container performance into a docker image.  

cAdvisor is built using the [google/cadvisor](https://github.com/google/cadvisor) repository on GitHub.

## Prerequisite
* Docker installed

## Using Pre-built Image on Dockerhub
If you aren't looking to build and just want to use a working
instance of cAdvisor, you can pull it from the dockerhub.

* https://hub.docker.com/r/5280brady/cadvisor
   * Tags correspond to version of cAdvisor
     * latest
     * arm64-v0.47.2
     * amd64-v0.47.2
  ```
  docker pull 5280brady/cadvisor
  ```

## Using the Repository
* Clone
  ```commandline
  git clone https://github.com/5280Brady/cadvisor-containers.git
  cd cadvisor-containers/docker
  ```
  Builds need to be done on the architecture you wish to run it on.

### How to use
Assumption is made the repository has been cloned previously and you are
working in the `docker` directory.
* Build the image
  ```commandline
  docker build -t <image_name>:<image_tag> .
  ```
* Run the image
  ```
  docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --volume=/dev/disk/:/dev/disk:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  <image_name>:<image_tag>
  ```
  * Update the <image_name> and <image_tag> with what you used to
  build the image.

  ### Alternative use
  Run using `docker compose` command

  **_Note:_** Be sure to use the yaml file that coresponds to the architecture
  of the machine the docker image is being built and run on.

  * Build and run using docker compose (ARM)
    ```
    docker compose -f docker-compose-arm.yml up -d
    ```
  * Build and run using docker compose (AMD)
    ```
    docker compose -f docker-compose-amd.yml up -d
    ```
  * Stop containers using docker compose
    ```
    docker compose down
    ```
