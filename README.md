# Remove Specific Existing Docker Container and Image

GitHub Action to **remove a specific existing Docker container** and its associated **Docker image** from a remote VM.

## Example :

```yaml
on:
  push: main
jobs:
  buildAndRunImage:
    name: Build, Push, and Cleanup Docker Container
    runs-on: ubuntu:latest
    steps:
      - name: checkout the repo
        uses: actions/checkout@v4
      - name: login to the docker hub
        uses: docker/login-action@v3
        with:
          username: ${{secrets.DOCKERHUB_USERNAME}}
          password: ${{secrets.DOCKERHUB_PASSWORD}}
      - name: build and push to docker hub
        uses: docker/build-push-action@v6
        with:
          context: .
          file: ./Dockerfile # Update the correct path if necessary
          push: true
          tags: your-dockerhub-username/your-image-name:latest
      - name: Remove Existing Container and Image
        uses: Lovegupta112/cleanup-docker-container-img@v1.0.0
        with:
          SSH_PRIVATE_KEY: ${{secrets.SSH_PRIVATE_KEY}}
          HOST_IP: ${{secrets.HOST_IP}}
          CONTAINER_NAME: ${{secrets.BACKEND_CONTAINER_NAME}}
      - name: start the container #start your container 
        run: 
          docker run -d -p 8080:8080 \ 
          -e DATABASE_URL=${{secrets.DATABASE_URL}} \
          --name ${{secrets.BACKEND_CONTAINER_NAME}} \
           your-dockerhub-username/your-image-name:latest
           
```
##  Inputs: 

 1. **SSH_PRIVATE_KEY  &  HOST_IP  ( Required )** :  for accessing the remote vm there is need of ssh private key and iP address of the remote VM (i.g. ubuntu@13.45.67.45) .
 
 2.  **CONTAINER_NAME  ( Required )** :  Name of the container to be removed (along with its associated image).
  
