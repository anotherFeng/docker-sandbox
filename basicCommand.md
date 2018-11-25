`docker ps`
  get all running containers

`docker build .`
  find the Dockerfile in current directory and build an image

`docker run <optional-flag(arg)> <image_id> <arg>`

##Running test
`docker run <image_id> npm run test`
  over write CMD on dockerfile and run your custom entry command

`docker exec -it <container_id> npm run test`
  another way to run test dynamically, while having a container running, 
  you can exec -it into the container and run test internally.
  alternatively, you may also create another service inside docker-compose config for test running.
=========================
tests:
  build: .
  volumes:
    - /app/node_modules
    - .:/app
  command: ["npm", "run", "test"]
=========================

and finally, you can always exec -it into running container, and use shell to run command
possibly the most flexible way to run test.

##Hot reload
`docker run -p 3000:3000 -v /app/node_modules -v $(pwd):/app <image_id>`
  first -v flag determine which folder you dont want referrence back to local
  second -v deteremine which folder you want to referrence
  in this case since node_modules is inside app folder, 
  we can assume everything inside container will delegate its lookup to local copy except for node_modules

you may also use docker-compose to accomplish the above:
=========================
version: '3'
services:
  react-app:
    build: .
    port:
      - "3000:3000"
    volumes:
      - /app/node_modules
      - .:/app
=========================

if dockerfile is not named `Dockerfile`
=========================
    build:
      context: .
      dockerfile: Dockerfile.dev
=========================

`docker build -t dockerhub-name/container-name:version .`
  create a tag name in place of image id

`docker build -f <dockerfile-name> .`
  build container using the docker file you specificed

`docker exec -it <container-name> sh`
  run shell command inside this container

docker run -p 8080:80 1d03032f2936

docker push dockerhub-name/container-name

kubectl apply -f <file name>
kubectl delete -f <file name>
kubectl get pods
kubectl get pods -o wide
kubectl get deployments

kubectl describe <object type> <object name> 
`object name is optional`

kubectl set image <object_type>/<object_name> <container_name> = <new image to use>
kubectl set image deployment/client-deployment client=stephengrider/multi-client:v5


imperative: manual config
declarative: auto config