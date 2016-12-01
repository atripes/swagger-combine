   * [Introduction](#introduction)
   * [Repos used](#repos-used)
   * [Building image](#building-image)
   * [Usage](#usage)
      * [Running against locally hosted servers](#running-against-locally-hosted-servers)
      * [Running against docker environment](#running-against-docker-environment)
   * [References](#references)

# Introduction
It is difficult to combine multiple [swagger](swagger.io) outputs into a single format. This project is designed to help.

For instance, you have multiple REST endpoints defined in swagger that is aggregated through a [gateway-api](http://microservices.io/patterns/apigateway.html). You want to expose the combined REST interfaces as a single swagger definition. You can expose each swagger definition, but that's not how they're ultimately used. 

This project creates a [Docker](docker.com) image combines multiple [swagger](swagger.io) definitions into one. The resulting container can be run on the localhost or in a docker environment (via docker-compose for example).

This image will also live on [dockerhub.com/u/hipposareevil/swagger-combine](https://hub.docker.com/u/hipposareevil/swagger-combine).

# Repos used
This uses code from the following github repositories:
Repo | Purpose
--- | ---
[swagger-yaml](https://github.com/idlerun/swagger-yaml) | Node.js project that combines yaml together
[swagger-ui](https://github.com/swagger-api/swagger-ui) | Swagger-ui the serves the final result

# Building image
Clone this repository and then run:

```
> docker build -t swagger-combine .
```

# Usage
To run against the swagger petstore:
```
> docker run -p 8080:8080 swagger-combine http://petstore.swagger.io/v2/swagger.json
```

The commandline will accept multiple URLs (with yaml files):

```
> docker run -p 8080:8080 swagger-combine URL_1/swagger.yaml URL_2/swagger.yaml
```

## Running against locally hosted servers
Say you are running a local server on port 8765 with 2 endpoints and want to combine those:
* localhost:8080/foo/swagger.yaml
* localhost:8080/bar/myswagger.yml

First obtain your IP address:
```
on ubuntu> ip route get 8.8.8.8 | awk '{print $NF; exit}'
10.1.2.3
on mac> ip route get 8.8.8.8 | awk '{print $NF; exit}'
10.1.2.3
```

Then run docker image:
```
> docker run -p 8765:8080 swagger-combine 10.1.2.3:8080/foo/swagger.yaml 10.1.2.3:8080/bar/myswagger.yml
```

Open browser to [localhost:8765](localhost:8765/) and the combined yaml will be shown in the swagger-ui.

## Running against docker environment
Say you are running two micro-services (exposed on port 8080) behind a gateway-api and want to combine both REST endpoints. These are running in docker containers on the network *my_network*. 

Launch the swagger ui via:
```
> docker run -p 8765:8080 --network my_network swagger-combine serviceone:8080/swagger.yaml servicetwo:8080/swagger.yaml
```

Open browser to [localhost:8765](localhost:8765/) and the combined yaml will be shown in the swagger-ui.



# References

* Docker image to combine static yaml files: [docker swagger-yaml](https://hub.docker.com/r/hipposareevil/swagger-yaml/)
* [swagger-yaml project](https://github.com/idlerun/swagger-yaml)
* [swagger-ui project](https://github.com/swagger-api/swagger-ui)