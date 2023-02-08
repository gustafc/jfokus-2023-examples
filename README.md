# jfokus-2023-examples

This repo has some examples and links relevant to my talk [Faster builds and
slimmer images with Docker multistage builds](https://www.jfokus.se/talks/1379)
at the 2023 installment of the [JFokus](https://www.jfokus.se/) conference.

## Bug in the Jfokus presentation

If you watched the presentation you may have noticed that the demo of
`--cache-from` didn't work out the way the presenter had hoped 🙂

This was because you need to set the environment variable `DOCKER_BUILDKIT` for
this feature to work:

```
# On *nix shells:
export DOCKER_BUILDKIT=1

# On Windows/cmd:
set DOCKER_BUILDKIT=1
```

Once this is set, you can use any image built with `--build-arg
BUILDKIT_INLINE_CACHE=1` can be used as a target for `--cache-from`.

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
