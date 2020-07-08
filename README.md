[![Build Status](https://travis-ci.org/microservices-demo/front-end.svg?branch=master)](https://travis-ci.org/microservices-demo/front-end)
[![](https://images.microbadger.com/badges/image/weaveworksdemos/front-end.svg)](http://microbadger.com/images/weaveworksdemos/front-end "Get your own image badge on microbadger.com")


Front-end app
---
Front-end application written in [Node.js](https://nodejs.org/en/) that puts together all of the microservices under [microservices-demo](https://github.com/microservices-demo/microservices-demo).


# Setting up Dev Env

Platform : Ubuntu 18.04


### Common Setup
```

# Install Pre reqs
sudo apt-get update  
sudo apt-get install git wget curl build-essential -yq

# Install Nodejs with NPM
cd /tmp
wget -c https://nodejs.org/dist/v4.8.6/node-v4.8.6-linux-x64.tar.xz
sudo tar -xzf node-v4.8.6-linux-x64.tar.xz -C /usr/local --strip-components=1

ln -s /usr/local/bin/node /usr/local/bin/nodejs


# validation  
node --version  
nodejs --version  
npm --version  

```


### Clone the source and install dependencies

```
cd /opt
git clone https://github.com/microservices-demo/front-end.git
cd front-end/
yarn install
```


##### Launching the App

The front-end service can be launched with npm using the following command.

```
export NODE_ENV=production
npm start
```


Service will launch on port **8079**


## Production Deployment

Follow **common steps** from above section.  

  * Create release directories
```
mkdir -p /opt/apps/frontend/releases

```

  * Download the [latest artifact from this page](https://github.com/udbc/front-end/releases) to the releases directory created above
e.g.
```
cd /opt/apps/frontend/releases
wget -c https://github.com/udbc/front-end/archive/1.0.1.tar.gz
```
  * Extract the artifact

```
cd /opt/apps/frontend/releases
tar -xzf 1.0.1.tar.gz

ls

front-end-1.0.1

```

  * Create a symlink */opt/frontend* pointing to the latest release

e.g.
```
ln -s /opt/apps/frontend/releases/front-end-1.0.1 /opt/frontend
```

  * Install Dependencies and start the app

```
cd /opt/frontend
npm install
npm start
```

#### Init Script

You could also use the init script available at path  **scripts/frontend** .  You could copy this script to /etc/init.d so that you could start and stop service as,

```
service frontend start
service frontend stop

```


#### App Configurations

Frontend connects with all the backend services using endpoint configurations. These configs are in a file api/endpoints.js

Sample of which is given below. 

```
module.exports = {
  catalogueUrl:  util.format("http://catalogue%s", domain),
  tagsUrl:       util.format("http://catalogue%s/tags", domain),
  cartsUrl:      util.format("http://carts%s/carts", domain),
  ordersUrl:     util.format("http://orders%s", domain),
  customersUrl:  util.format("http://user%s/customers", domain),
  addressUrl:    util.format("http://user%s/addresses", domain),
  cardsUrl:      util.format("http://user%s/cards", domain),
  loginUrl:      util.format("http://user%s/login", domain),
  registerUrl:   util.format("http://user%s/register", domain),
};

```
===========================

# Build

## Dependencies

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Version</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="https://docker.com">Docker</a></td>
      <td>>= 1.12</td>
    </tr>
    <tr>
      <td><a href="https://docs.docker.com/compose/">Docker Compose</a></td>
      <td>>= 1.8.0</td>
    </tr>
    <tr>
      <td><a href="gnu.org/s/make">Make</a>&nbsp;(optional)</td>
      <td>>= 4.1</td>
    </tr>
  </tbody>
</table>

## Node

`npm install`

## Docker

`make test-image`

## Docker Compose

`make up`

# Test

**Make sure that the microservices are up & running**

## Unit & Functional tests:

```
make test
```

## End-to-End tests:

To make sure that the test suite is running against the latest (local) version with your changes, you need to manually build
the image, run the container and attach it to the proper Docker networks.
There is a make task that will do all this for you:

```
make dev
```

That will also tail the logs of the container to make debugging easy.
Then you can run the tests with:

```
make e2e
```

# Run

## Node

`npm start`

## Docker

`make server`

# Use

## Node

`curl http://localhost:8081`

## Docker Compose

`curl http://localhost:8080`

# Push

`GROUP=weaveworksdemos COMMIT=test ./scripts/push.sh`
