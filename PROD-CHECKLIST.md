# Production Checklist

This is a checklist for software being prepared for use in production environments.

## Code
- [ ] Dependencies are managed and versioned using a dependency management tool such as [`dep`](https://github.com/golang/dep), [`composer`](https://getcomposer.org/), [`npm`](https://www.npmjs.com/), etc.
  * For services, all vendor dependencies should be checked in to code repository.
  * For libraries, only dependency configuration and lock files should be checked in.
- [ ] Logger is configured to output structured logs to support forensic analysys and metric gathering (ELK, etc.).
- [ ] All file / network handlers are explicitly closed immediately when no longer needed.

### Middleware
- [ ] Circuit breakers are available if appropriate.
- [ ] Rate-limiting is implemented if appropriate.
- [ ] Request-body size limits are implemented if appropriate.
- [ ] Sensible metric recording is implemented (error-rate, response-time, etc.). A [prometheus](https://prometheus.io/) endpoint is ideal.

### Language specific:
#### Golang
- [ ] Services are compiled with `-buildmode=pie`
  * https://en.wikipedia.org/wiki/Address_space_layout_randomization
  * https://access.redhat.com/blogs/766093/posts/1975793
  * https://eklitzke.org/position-independent-executables

## Daemons
### Startup and shutdown
- [ ] Exit early and loudly (`panic`, etc.) when a unrecoverable error occurs (database not found, invalid system configuration, etc.).
- [ ] Signals are caught and handled, graceful-shutdown is implemented.

## Services
### APIs
- [ ] CORS is properly configured.
- [ ] API response data models a uniform output format.

## Devops
### Docker
- [ ] A multi-stage `Dockerfile` is used to produce the most minimal image possible (within reason).
- [ ] Docker image does not include software or packages not required for the service to function (`vim`, etc.).

### Kubernetes
- [ ] Liveness and readiness checks are configured.
- [ ] Horizontal pod autoscalling is defined appropriately.

## Deployment
### Preparation
- [ ] Verify the production configuration is valid and correct.
- [ ] Service is load-tested and/or benchmarked.
- [ ] All build dependencies should be available (cached, hosted, etc.) within the build environment so 3rd party outages do not prevent successful builds.

### Network
- [ ] SSL is properly configured (validate with [SSL Labs](https://www.ssllabs.com/)).
- [ ] Private services are only accessible via the private network.
- [ ] A reasonable timeout is configured for all network requests.

#

_[inspiration](https://github.com/bahlo/go-production-checklist)_
