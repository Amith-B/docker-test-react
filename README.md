# Create build

`docker build -t docker-test-react .`

# Start container:

## below command will not work because the code is replaced with current code and node_modules is removed from container

`docker run -it --rm -p 3000:3000 -v ${PWD}:/usr/src/app docker-test-react`

## below command will work because we are explicity specifing to use node_modules from container volumn itself, this copies the node_modules from container to PWD

`docker run -it --rm -p 3000:3000 -v ${PWD}:/usr/src/app -v /usr/src/app/node_modules docker-test-react`

1. `-it` starts the container in interactive mode.
1. `--rm` removes the container and volumes after the container exits.
1. `-v ${PWD}:/usr/src/app` mounts the code into the container at "/usr/src/app"
1. `-v /usr/src/app/node_modules`
   Since we want to use the container version of the "node_modules" folder, we configured another volume: -v /usr/src/app/node_modules. We should now be able to remove the local "node_modules" flavor.
1. `-p 3001:3000` exposes port 3000 to other Docker containers on the same network (for inter-container communication) and port 3000 to the host.

# Start container by modifing the existing CMD

## This replaces the `["npm", "start"]` with `npm run build`

`docker run -it --rm -p 3000:3000 -v ${PWD}:/usr/src/app -v /usr/src/app/node_modules docker-test-react npm run build`

### By default the ENTRYPOINT is "node", so below command will return the node version inside container

`docker run -it --rm -p 3000:3000 -v ${PWD}:/usr/src/app -v /usr/src/app/node_modules docker-test-react -v`

# Docker compose instead of a long docker command

create docker-componse.yml with all necessary args and values and execute `docker-compose up -d` or `docker-componse down -v`

1. `up -d` - run in detach mode
1. `down -v` - remove volumn attached to the container when container stops

# Enter shell of the build:

`docker run -it --name temp-container docker-test-react sh`
