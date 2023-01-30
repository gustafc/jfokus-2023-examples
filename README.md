# jfokus-2023-examples

This repo has some examples and links relevant to my talk [Faster builds and
slimmer images with Docker multistage builds](https://www.jfokus.se/talks/1379)
at the 2023 installment of the [JFokus](https://www.jfokus.se/) conference.

## The examples

- [`example-node-app`](./example-node-app/README.md) is a very full-featured
  example building a React app and packaging it in an [nginx](http://nginx.org/)
  container. This is the project I demo in the talk.
- [`example-mvn-app`](./example-mvn-app/README.md) is a simple approach to a
  cache-friendly and fairly "feature complete" Maven build - you can build a
  container running a simple Spring Boot app, and export test reports.

## Links

- [Mejsla](https://mejsla.se/) - where I work
- [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) -
  this document _probably_ has the answers to most technical questions you have
  about your Docker builds.
