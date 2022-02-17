## BoNeS Sample Java application

This is a sample Java application for the BoNeS Platform. It will encompass all the
best practices that BoNeS aims to incorporate.

This application will contain features and integrations which will eventually be thoroughly documented
and templated.

This application tries to abide by the [Organisation Manifesto](https://github.com/nexmoinc/manifesto).

### Technologies used
This application will try to steer away from library choices. That said, some basic technologies which comply
with the BoNeS standards and are used are the following:

- [Micronaut](https://micronaut.io/) - Modern Java Multipurpose Framework
- [Netty](https://netty.io/) - Non blocking high performance TCP and UDP server. Used by Micronaut.
- [jib](https://github.com/GoogleContainerTools/jib) - OCI compliant containerising tool that does not require a Dockerfile

### ECR Login
The base JDK image is Amazon Corretto. You need to login to Amazon ECR to be able to pull it. Follow the
instructions [here](https://github.com/corretto/corretto-docker).

### Build and Run
The project contains a Makefile for all the usual activities:

- `make clean` - Cleans the build folders
- `make build` - Builds the project and runs the unit tests
- `make verify` - Builds the project and runs all the tests (including integration tests)
- `make package` - Creates a local Docker image for the project
- `make run-local` - Runs the service locally
- `make dev` - Runs the service in a local Kubernetes cluster and port-forwards local and debug ports
