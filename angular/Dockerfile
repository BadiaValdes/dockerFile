# PRODUCTION DOCKERFILE
# ---------------------
# This Dockerfile allows to build a Docker image of the Angular application
# and based on a NodeJS 14 image. The multi-stage mechanism allows to build
# the application in a "installer" stage and then create a production
# image containing the required dependencies and the JS build files.

# For manual build you can execute:
# docker build . --target=production --name=aisecurity-frontend:1.0.0

#----------------------
#   Building stage
#----------------------

FROM node:16 AS installer

ENV TZ=America/Havana \
	COMPOSE_HTTP_TIMEOUT=200 \
	SASS_BINARY_PATH=/home/node/app/linux-x64-83_binding.node

WORKDIR /home/node/app

#COPY .npmrc /root/.npmrc
COPY package*.json ./

RUN yarn global add @angular/cli
# uncomment the line below and add node_modules to .dockerignore
RUN yarn config set ignore-engines true \
&& yarn install --network-timeout 1000000 --no-optional \
&& yarn cache clean --force

COPY . .

#----------------------
#   Building stage
#----------------------

FROM installer AS building
RUN ["yarn", "build:prod"]

#----------------------
#   Production stage
#----------------------

FROM nginx:alpine as production

COPY --from=building /home/node/app/dist /var/www/html
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
