# Composer
NodeJS package for generating docker-compose.yml file for containers from a JSON document.

## About the package

The main goal of this repository to create a small lightweight npm package, which generates docker-compose.yml from a JSON file.


The JSON has a very easy ruleset, it use the docker-compose.yml reference for the available elements. This version support commands from the docker-compose.yml reference that have values as only one line (string, number), array, or object (one level)

- build (one level)
- cap_add
- cap_drop
- command
- cgroup_parent
- container_name
- devices
- depends_on
- dns
- dns_search
- tmpfs
- entrypoint
- env_file
- environment
- expose
- extends
- external_links
- extra_hosts
- image
- labels
- links
- logging (one level)
- log_driver
- log_opt
- net
- network_mode
- networks
- pid
- ports
- security_opt
- stop_signal
- volumes
- volume_driver
- volumes_from
- cpu_shares,cpu_quota, cpuset
- domainname, hostname, ipc, mac_address
- mem_limit, memswap_limit, privileged
- read_only, restart, shm_size, stdin_open
- tty, user, working_dir



You can find and read the complete docker-compose.yml reference on the next url: [Compose Reference](https://docs.docker.com/compose/yml/)

## Release Notes

You can read the changelist: [here](https://github.com/tudvari/docker-composer/blob/master/ReleaseNotes.md)

## Rules of the JSON

You should use the followed rules for the JSON file, which is the only argument for this package.

- ports : This element is array, example syntax:

  ports : [ 'PORT_NUMBER1','PORT_NUMBER2']

- environment : This element is a subdocument, you can describe it as the followed style:

  { "ENVIRONMENT_VARIABLE1" : "VARIABLE_VALUE"}

- extra_hosts : Same a the envorinment element:

  { "HOST_NAME1" : "IP_ADDRESS", "HOST_NAME2" : "IP_ADDRESS"}

- image : This element is very simple:

  image : REGISTRY_HOST:REGISTRY_PORT/REGISTRY_USER/IMAGE_NAME:VERSION

- command : Simple key value pair.

  command : NEW_COMMAND

- expose : Array element, example:

  expose: [ PORT_NUMBER1, PORT_NUMBER2]

- dns: Array of the DNS Servers

  dns : ['DNS_SERVER_IP','DNS_SERVER_IP']

- dns_search: Array of the DNS Search Servers

  dns_search : ["DNS_SEARCH_SERVER1","DNS_SEARCH_SERVER2"]

- mem_limit: Maximum memory usage of the container

  mem_limit: number[unit], where unit = b,k,m,g or inf = infinity

- memswap_limit: Maximum swap memory usage of the container

  memswap_limit: number[unit], where unit = b,k,m,g or inf = infinity

- cpu_shares: Proportion of CPU cycles

  cpu_shares: number (default: 1024, 0 will be ignored)

- volumes : Array element, example:

  volumes: [ "/var/test", "var/test:/opt/test"]

## Usage

```javascript

  var composer = require('docker-composer') ;
    .
    .
    .
  composer.generate(inputJSON,function(err,result){
    .
    .
    .
  }) ;
```


## Full Example
Input JSON:

```json
{
    "service": {
        "environment": {
            "WARPER_PORT": "1234",
            "WARPER_IP"  : "127.0.0.1"
        },
        "extra_hosts": {
            "amqphost": "10.0.0.4",
            "mongohost": "10.0.0.4"
        },
        "ports": [
            "3456",
            "8692"
        ],
        "image" : "tudvari.com:5000/warper/ms_config:LATEST",
        "dns" : [
          "8.8.8.8",
          "127.0.0.1"
        ],
        "dns_search" : [
          "search1.example.com",
          "search2.example.com"
        ],
        "mem_limit" : "25M",
        "memswap_limit": "128k",
        "cpu_shares" : 43

    },
    "service2": {
        "environment": {
            "WARPER_PORT": "1234",
            "WARPER_IP": "127.0.0.1"
        },
        "extra_hosts": {
            "amqphost": "10.0.0.4",
            "mongohost": "10.0.0.4"
        },
        "ports": [
            "3456",
            "8692"
        ]
    }
}```

Result:
```yml
service:
  environment:
   -WARPER_PORT:1234
   -WARPER_IP:127.0.0.1
  extra_hosts:
   -amqphost:10.0.0.4
   -mongohost:10.0.0.4
  ports:
   -3456
   -8692
  image: tudvari.com:5000/warper/ms_config:LATEST
  dns:
   -8.8.8.8
   -127.0.0.1
  dns_search:
   -search1.example.com
   -search2.example.com
  mem_limit: 25M
  memswap_limit: 128k
  cpu_shares: 43
service2:
  environment:
   -WARPER_PORT:1234
   -WARPER_IP:127.0.0.1
  extra_hosts:
   -amqphost:10.0.0.4
   -mongohost:10.0.0.4
  ports:
   -3456
   -8692
```




## Future plans

I would like give much more power into this package. The final goal is, push all possibility into this package, which the docker-compose already has. Feel free to help me, in this mission. :-)

## Metrics

[![Code Climate](https://codeclimate.com/github/tudvari/composer/badges/gpa.svg)](https://codeclimate.com/github/tudvari/composer)
[![Test Coverage](https://codeclimate.com/github/tudvari/composer/badges/coverage.svg)](https://codeclimate.com/github/tudvari/composer/coverage)
[![Build Status](https://travis-ci.org/tudvari/docker-composer.svg?branch=master)](https://travis-ci.org/tudvari/docker-composer)
