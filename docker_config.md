# Docker

- [About](#about)
- [Install](#install)
- [NodeJS](#nodejs)
- [MongoDB](#MongoDB)
- [PostgreSQL](#PostgreSQL)

## About

This is for sharing the Docker containers I use with simple examples.

## Install

- installation on [Windows](https://docs.docker.com/docker-for-windows/install/) (i recomend using [Windows Subsystem for Linux 2](https://docs.microsoft.com/en-us/windows/wsl/wsl2-kernel))
- installation on [Linux](https://docs.docker.com/engine/install/ubuntu/)

## NodeJS

### Build a docker image for a NodeJS

- Create a **`dockerfile`**

#### Using NPM

```dockerfile
# Use the official lightweight Node.js 12 image.
# https://hub.docker.com/_/node
FROM node:12-slim

# Create and change to the app directory.WORKDIR /usr/app
WORKDIR /usr/app

# Copy application dependency manifests to the container image.
# Copying this first prevents re-running yarn install on every code change.
COPY package.json ./

# Install production dependencies.
RUN npm install --only=production

# Copy local code to the container image.
COPY . .

# Enviroment variable
ENV PORT=3000

# Expose port
EXPOSE 3000

# Run the web service on container startup.
CMD ["npm", "dev"]
```

#### Using Yarn

```dockerfile
# Use the official lightweight Node.js 12 image.
# https://hub.docker.com/_/node
FROM node:12-slim

# Create and change to the app directory.
WORKDIR /usr/app

# Copy application dependency manifests to the container image.
# Copying this first prevents re-running yarn install on every code change.
COPY package.json ./

# Install production dependencies.
RUN yarn install --only=production

# Copy local code to the container image.
COPY . .

# Enviroment variable
ENV PORT=3000

# Expose port
EXPOSE 3000

# Run the web service on container startup.
CMD ["yarn", "dev"]
```

### Ignore node_modules on your docker image

- create a **`.dockerignore`**

```dockerignore
node_modules

npm-debug.log
yarn-debug.log
yarn-error.log
```

### Building the container

- Finally let's build the container using [docker-compose](https://docs.docker.com/compose/).

- `docker build -t <username>/<app_name> .`  

**_NOTE!_** in `<username>`  you should use your Hub username and .

### Running the container

- `docker run -p 3000:3000 -d <username>/<app_name>`  
or
- `docker run -p 3000:3000 <image_id>`

## MongoDB

- `docker pull mongo`  
- `docker run --name mongodb -p 27017:27017 -d mongo`

### Remote connection to MongoDB on Docker

- host: `127.0.0.1`
- port: `27017`


## PostgreSQL

- `docker pull postgres`
- `docker run --name postgresql -e POSTGRES_PASSWORD=postgres -p 5432:5432 -d postgres`  
**_NOTE!_** change the username and password.

### get the IP address with

#### Windows

- `docker inspect postgresql | findstr "IPAddress"`  

#### Linux

- `docker inspect postgresql | grep "IPAddress"`  

#### Remote connection to PostgreSQL on Docker

- host: `127.0.0.1`
- port: `5432`
  
#### PSQL

- `docker pull governmentpaas/psql`  
- `docker run -it --rm postgres psql -h <Postgres-IP> -p 5432 -U postgres`  
**_NOTE!_** change `<Postgres-IP>` to the IP you want to connect.
