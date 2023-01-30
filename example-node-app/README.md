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
  `docker build  --secret=id=NPMRC --target=report-artifacts --output type=local,dest=test-reports` \
  Test reports are now available in `./test-reports/unit-tests.txt` regardless
  of whether tests passed or not.
