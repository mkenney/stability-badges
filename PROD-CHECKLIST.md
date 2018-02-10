# Productionisation Checklist

This is a checklist things to think about when [productionizing software](https://en.wikipedia.org/wiki/Productionisation).

## Documentation

- [ ] A README.md file is included in the repository with a high-level project description:
  * purpose of the service or library
  * links to production support pages
  * deployment instructions
  * high-level usage instructions
  * links to service / library documentation
- [ ] A wiki site is available containing further details:
  * troubleshooting and production-support information
  * detailed usage instructions
  * service / library documentation
  * examples
  * flowchart(s) (`graphviz`, `seqdiag`, etc.) of data / process flow

## Code
- [ ] Dependencies are managed and versioned using a dependency management tool such as [`dep`](https://github.com/golang/dep), [`composer`](https://getcomposer.org/), [`npm`](https://www.npmjs.com/), etc.
  * For services, all vendor dependencies should be checked into the code repository.
  * For shared libraries, only dependency configuration and lock files should be checked in.
- [ ] Logger is configured to output structured logs to support forensic analysys and metric gathering (ELK, etc.).
- [ ] All file / network handlers are explicitly closed immediately when no longer needed.
- [ ] Garbage collection concerns are handled appropriately:
  * object references are not cached unnecessarily
  * event loops release object references at the _end_ of each iteration
  * local storage is minimal and contains only application data
  * limits exist for number of cached route changes
  * etc.

### Middleware
- [ ] Circuit breakers are available if appropriate.
- [ ] Rate-limiting is implemented if appropriate.
- [ ] Request-body size limits are implemented if appropriate.
- [ ] Sensible metric recording is implemented (error-rate, response-time, etc.). A [prometheus](https://prometheus.io/) endpoint is preferred.

### Language specific:
#### Golang
- [ ] Services are compiled with `-buildmode=pie`
  * https://en.wikipedia.org/wiki/Address_space_layout_randomization
  * https://access.redhat.com/blogs/766093/posts/1975793
  * https://eklitzke.org/position-independent-executables

## Daemons
### Startup and shutdown
- [ ] `init.d` or equivalent scripts or commands are implemented.
- [ ] Exit early and loudly (`panic`, etc.) when a unrecoverable error occurs (database not found, invalid system configuration, etc.).
- [ ] Signals are caught and handled, graceful-shutdown is implemented.

## Services
### APIs
- [ ] CORS is properly configured.
- [ ] Uniform response data models are enforced.

## Devops
### Docker
- [ ] A multi-stage `Dockerfile` is used to produce minimal images.
- [ ] Images do not include software or packages not required for the service to function (`vim`, `ssh`, etc.).

### Kubernetes
- [ ] Liveness and readiness checks are configured.
- [ ] Horizontal pod autoscalling is defined appropriately.
- [ ] Deployment `strategy` provides 0 downtime for upgrades.

### Helm
- [ ] ...

## Deployment
### Preparation
- [ ] Verify all production configurations are valid and correct.
- [ ] Service is load-tested and/or benchmarked and a scaling method is defined.
- [ ] All build dependencies should be available (cached, hosted, etc.) within the build environment so 3rd party outages do not prevent successful builds.

### Network
- [ ] SSL is properly configured (validate with [SSL Labs](https://www.ssllabs.com/)).
- [ ] Private services are not accessible outside their private network.
- [ ] A reasonable timeout is defined for all network requests.

#

_[inspiration](https://github.com/bahlo/go-production-checklist)_
