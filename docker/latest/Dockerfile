
FROM node:6

# Home directory for Node-RED application source code.
RUN mkdir -p /usr/src/node-red

# User data directory, contains flows, config and nodes.
RUN mkdir /data

WORKDIR /usr/src/node-red

# Add node-red user so we aren't running as root.
RUN useradd --home-dir /usr/src/node-red --no-create-home node-red \
    && chown -R node-red:node-red /data \
    && chown -R node-red:node-red /usr/src/node-red

USER node-red

# package.json contains Node-RED NPM module and node dependencies
COPY package.json /usr/src/node-red/
RUN npm install

# make symlincs for wnrUI 2.0
#RUN cd /usr/src/node-red/node_modules/wnr-ui
WORKDIR /usr/src/node-red/node_modules/wnr-ui
RUN ln -s ../../node_modules node_modules
WORKDIR /usr/src/node-red

# User configuration directory volume
EXPOSE 1880

# Environment variable holding file path for flows configuration
ENV FLOWS=/flows/flows.json

CMD ["npm", "start", "--", "--settings", "/settings/settings.js"]
