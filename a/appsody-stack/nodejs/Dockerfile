#Install the app dependencies in a full Node docker image
FROM registry.access.redhat.com/ubi8/nodejs-12

USER root

# Install OS updates
RUN yum -y update \
&& echo 'Finished installing dependencies' \
&& yum -y install python2 \
&& ln -s $(which python2) /usr/local/bin/python

# Install user-app dependencies
WORKDIR "/project/user-app"
COPY ./package*.json ./
RUN npm install --production
WORKDIR "/project/user-app/node_modules"

# Copy the dependencies into a slim Node docker image
FROM registry.access.redhat.com/ubi8/nodejs-12

USER root

# Install OS updates
RUN yum -y update \
&& echo 'Finished installing dependencies'\
&& yum -y install python2 \
&& ln -s $(which python2) /usr/local/bin/python

RUN groupadd node && useradd -r -g node node  -m -d /home/node/

# Copy the project
COPY --chown=node:node . /project

# Copy all dependencies
COPY --chown=node:node --from=0 /project/user-app/node_modules /project/user-app/node_modules

WORKDIR "/project/user-app"

ENV NODE_ENV production

USER node

CMD ["npm", "start"]
