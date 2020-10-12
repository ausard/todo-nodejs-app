
#####################################
#       FRONTEND ENVIRONMENT        #
#####################################

FROM node:lts-alpine
WORKDIR /app
COPY . .
RUN yarn install && yarn cache clean
CMD ["yarn", "start"]