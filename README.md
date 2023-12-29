# Dockerized Jenkins Pipeline
## Prerequisites:
- Jenkins should have the GitHub and SSH Agent plugins installed.
- Docker should be installed on the Jenkins server or Agent (if used) and the remote server.

## Pipeline Environment Variables:
- `DOCKER_HUB_REPO`: Docker Hub repository name.
- `DOCKER_HUB_USERNAME`: Docker Hub username.
- `DOCKER_HUB_PASSWORD`: Docker Hub password. note: better use Jenkins credentials.
- `GITHUB_REPO_SSH_URL`: SSH URL of the private GitHub repository.
- `GITHUB_CREDENTIALS_ID`: Jenkins credentials ID for GitHub access.
- `GITHUB_BRANCH`: GitHub repository branch to build.
- `REMOTE_SERVER_ADDRESS`: IP address of the remote server.
- `REMOTE_SERVER_USERNAME`: SSH username for the remote server.


## Usage:
1. Configure Jenkins with the required plugins and credentials.
2. Create a Jenkins pipeline job and paste this script into the pipeline definition.
3. Update the environment variables and ensure that necessary dependencies are met.

**Note:**
- Uncomment the "post" section for cleanup tasks after the build is complete.

Feel free to customize the script according to your project's needs and security best practices.


This Jenkins pipeline script automates Dockerization and the deployment of a Dockerized web application from a private GitHub repository to a remote server. The pipeline includes the following stages:

## 1. Clone from GitHub
   - Clones the private project repository from GitHub using SSH.

## 2. Build and Push Docker Image
   - Logs in to Docker Hub.
   - Pulls the official `httpd` Docker image.
   - Creates a Dockerfile to add custom content (copies `index.html` to the Apache web server's root).
   - Builds a new Docker image with a version tag.
   - Tags the image for Docker Hub, including a `latest` tag.
   - Pushes the new Docker image to Docker Hub.
   - Logs out from Docker Hub.

## 3. Checks if Docker is installed on Remote Server. If not, installs Docker on Remote Server
   - Uses SSH credentials for authentication.
   - Adds the remote server to the known hosts.
   - Executes a Docker installation script on the remote server.
   - Adds the Jenkins user to the `docker` group.

## 4. Runs Docker Container on Remote Server
   - Uses SSH credentials for authentication.
   - Logs in to Docker Hub on the remote server.
   - Checks for existing processes on port 80. (debug purposes)
   - Stops and removes an existing Docker container named `httpd-container`, so it won't restrict from starting newly built container.
   - Stops Nginx on the remote server. (debug purposes)
   - Pulls the Docker image from Docker Hub.
   - Runs a Docker container named `httpd-container` on port 80.

