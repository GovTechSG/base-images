
# Oracle, Ruby, Node
Base image for bgp-react application.

Consists of
- oracle 12.2.0.1.0
- ruby 2.3.1
- nodejs 7.1.0
- npm 

## Building it

```bash
docker build -t nexus-docker-publish.ship.gov.sg/bgp/ror-base:12.2-2.3.1-7.1.0 .
```

## Caveats

### Why use oraclelinux:7-slim?

- Alpine is not supported by oracle instant client. https://stack
- 
- 
- overflow.com/questions/53263972/oracle-on-alpine-linux

- It has the smallest image size next to alpine. https://blogs.oracle.com/developers/put-your-containers-on-a-diet-with-oracle-linux
