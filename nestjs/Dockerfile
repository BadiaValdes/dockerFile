# PRODUCTION DOCKERFILE
# ---------------------
# This Dockerfile allows to build a Docker image of the NestJS application
# and based on a NodeJS 18 image. The multi-stage mechanism allows to build
# the application in a "builder" stage and then create a production
# image containing the required dependencies and the JS build files.

# For manual build you can execute:
# docker build . --target=deployment --name=ms-security:1.0.0

#----------------------
#   Building stage
#----------------------

FROM docker.uclv.cu/node:18 AS builder

ENV TZ=America/Havana
ENV COMPOSE_HTTP_TIMEOUT=200
ARG NODE_ENV=dev
ENV NODE_ENV=${NODE_ENV}

RUN yarn global add @nestjs/cli

WORKDIR /home/node/app
# COPY --chown=node:node .npmrc /home/node/.npmrc
RUN yarn global add @nestjs/cli

#COPY --chown=node:node yarn.lock .

COPY package*.json ./

EXPOSE ${APP_PORT}

# uncomment the line below and add node_modules to .dockerignore
RUN yarn config set strict-ssl false \
&& yarn config set ignore-engines true \
&& yarn install --no-optional \
&& yarn cache clean --force

COPY . .


#----------------------
#   Deployment stage
#----------------------

FROM docker.uclv.cu/node:18-alpine AS deployment

ARG NODE_ENV=prod
ENV NODE_ENV=${NODE_ENV}

WORKDIR /home/node/app

COPY package*.json ./
COPY yarn.lock ./

COPY --from=builder /home/node/app/dist ./
COPY --from=builder /home/node/app/package* ./
COPY --from=builder /home/node/app/yarn.lock ./

RUN yarn install --prod

COPY . .

EXPOSE ${APP_PORT}

CMD ["node", "dist/main"]
