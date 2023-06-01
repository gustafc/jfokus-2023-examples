# Example Node project

This directory contains an out-of-the-box React app, and a fully fleshed
`Dockerfile` which builds an Nginx image containing the built HTML/JS/CSS/etc
files.

To show how it's done, most steps require you to have mount an `NPMRC` secret,
although it doesn't actually need any credentials or other NPM settings (since
it uses the public NPM registry). Since there's no reason to mount your real
`.npmrc` file, just create an empty environment variable called `NPMRC`:

```
export NPMRC=
```

Once this is done, you can try the following invocations:

- **To build the web server:** \
  `docker build  . --secret=id=NPMRC --target=web-server` \
  ... then start a container: \
  `docker run --rm -p8080:80 sha256:...` \
  _Et voilà_, you can now see the app in all its glory at http://localhost:8080
- **To run the tests and extract the test reports:** \
  `docker build . --secret=id=NPMRC --target=report-artifacts --output type=local,dest=test-reports` \
  Test reports are now available in `./test-reports/unit-tests.txt` regardless
  of whether tests passed or not.
- **To use an existing image as a "remote" cache:** \
  First of all, make sure you have set the environment variable
  `DOCKER_BUILDKIT=1`. Then, build the image you want to use as a cache source: \
  `docker build  . --secret=id=NPMRC --target=web-server --build-arg BUILDKIT_INLINE_CACHE=1 --tag example-node-app:1.0` \
  Now, run `docker builder prune -a` to wipe the build cache completely, and
  build a new image based on the previous build: \
  `docker build  . --secret=id=NPMRC --target=web-server --build-arg BUILDKIT_INLINE_CACHE=1 --tag example-node-app:1.1 --cache-from=example-node-app:1.0` \
  If you haven't changed any files, every build step should be cached, and the
  build essentially becomes a no-op.
