# docker-images-example-apache

Example of a web application to use with startx s2i builder [startx/sv-apache](https://hub.docker.com/r/startx/sv-apache) or [sv-apache centos](https://hub.docker.com/r/startx/sv-apache) using [source-to-image](https://github.com/openshift/source-to-image).

## Docker OS Images : APACHE

Startx apache is a base container used for web services and applications published in [startx Dockerhub registry](https://hub.docker.com/u/startx/sv-apache). 
[Read startx apache image guideline](https://github.com/startxfr/docker-images/blob/master/Services/apache/README.md) for more information on how to use this image.

## Running this example in OKD (Openshift)

- Create a openshift project
  ```bash
  oc new-project startx-example-apache
  ```
- Add build template to the project service catalog
  ```bash
  oc create -f https://raw.githubusercontent.com/startxfr/docker-images/master/Services/apache/openshift-template-build.yml
  ```
- start a new application (build and run)
  ```bash
  oc process -f startx-apache-build-template \
      -p APP_NAME=myapp \
  | oc create -f -
  ```

## Running this example with s2i

### sample application

```bash
# Build the application
s2i build https://github.com/startxfr/docker-images-example-apache startx/sv-apache startx-apache-sample
# Run the application
docker run --rm -d -p 8777:8080 startx-apache-sample
```

### Personalized application

- Create a project directory
  ```bash
  git clone https://github.com/startxfr/docker-images-example-apache.git apache-myapp
  cd apache-myapp
  ```
- Create a personalized page
  ```bash
  cat << "EOF"
  <html><head></head><body><h1>My Web Application</h1></body></html>
  EOF > index.html
  ```
- Build your current code with startx image as an s2i builder
  ```bash
  s2i build . startx/sv-apache:latest startx-apache-myapp
  ```
- Run a sample application and test it
  ```bash
  docker run --rm -d -p 8777:8080 startx-apache-myapp
  ```
