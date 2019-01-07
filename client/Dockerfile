# base image
FROM node:9.6.1

# set working directory
RUN mkdir -p /usr/client
WORKDIR /usr/client

# add `/usr/src/app/node_modules/.bin` to $PATH
ENV PATH /usr/client/node_modules/.bin:$PATH

# install and cache app dependencies
COPY package.json /usr/client/package.json
RUN npm install
RUN npm install -g @angular/cli@1.7.1

# add app
COPY . /usr/client

# start app
CMD ng serve --configuration=production --host 0.0.0.0 --disableHostCheck true
