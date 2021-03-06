# Tekkers Mongo 
## Instructions 
### Running the Mongo Container 
`docker run -v ~/dev/tekkers/data:/data --name tekkers-mongo -d mongo mongod --smallfiles`
`docker run -it --link tekkers-mongo:mongo --rm mongo sh -c 'exec mongo "$MONGO_PORT_27017_TCP_ADDR:$MONGO_PORT_27017_TCP_PORT/test"'`

### Running a Linked Node.js Container 
`docker run -it --name node -v "$(pwd)":/data --link mongo:mongo -w /data -p 8082:8082 node bash`


### Deployd dockerfile
``` 
FROM dockerfile/nodejs-bower-grunt

# Install MongoDB.
RUN \
  apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10 && \
  echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' > /etc/apt/sources.list.d/mongodb.list && \
  apt-get update && \
  apt-get install -y mongodb-org && \
  rm -rf /var/lib/apt/lists/*

RUN npm install deployd -g

# Define mountable directories.
#VOLUME ["/data"]

# Define working directory.
WORKDIR /home

# Expose ports.
#   - 27017: process
#   - 28017: http
#EXPOSE 27017
#EXPOSE 28017

EXPOSE 2403
CMD dpd create test && cd test && dpd 
```
