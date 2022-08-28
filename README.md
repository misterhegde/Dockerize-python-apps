## Documentation

Docker:

- For the base images, **`alpine`** has not been used to prevent compatibility issues. Since most python apps are built to work on Debian systems, they need recompilation to work on alpine. Therefore **`slim`** version of the image has been used.
- Base image digest has been added to the Dockerfiles to ensure that everytime the images get built, the same underlying OS and library versions are used which provide a deterministic build.
- A multi-stage build process for the Dockerfiles. A virtualenv (isolation inside of already isolated container) is created so that the app can use the required dependencies only, thereby also reducing the image sizes.
- WSGI for production. [Gunicorn](https://docs.gunicorn.org/en/stable/index.html) is used.
- The Docker applications are run with least privileges. A user named **`python`**, is created so that with this user the processes can run with least privileges. All the files copied will also be owned by the created user and not the default root user created by Docker.
- A healthcheck is included in the Dockerfiles and also in the k8s deployment file to make sure the applications are working as expected.
- The built Dockerfiles are scanned using [Snyk](https://snyk.io/) to scan vulnerabilities in the images.

Kubernetes:
- A single deployment file is created which launches 2 replicas and 2 containers (app-a and app-b) in each pod.
- To connect to the containers, NodePort is being used. This single service file will communicate to the containers through localhost over seperate ports.


Taskfile:

To build and scan the apps seperately:

```bash
task build-app-a
task build-app-b
```

To deploy the apps in local k8s cluster (docker desktop is used here):

```bash
task launch
```

To destory the apps in the k8s cluster:

```bash
task destroy
```