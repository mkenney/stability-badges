# Production Checklist

This is a checklist for software going into a production environment. Inspired by https://github.com/bahlo/go-production-checklist.

## Startup and shutdown
- [ ] Exit early (`panic`) on fatal errors (e.g. database not found or configuration invalid).
- [ ] Migrate your database on startup.
- [ ] Graceful-shutdown is implemented / handled.

## Code
- [ ] Use a tool ([Dep](https://github.com/golang/dep), [Composer](https://getcomposer.org/), [NPM](https://www.npmjs.com/), etc.) to manage project dependencies. Dependencies should be checked in to service repositories, libraries should only incldue the dependency configuration and lock files.
- [ ] Logger is configured to output outputs machine-readable logs, to be processed later (e.g. with ELK).
- [ ] A reasonable timeout is configures for all external request (HTTP, RPC, etc.).
- [ ] All file / network handlers are explicitly closed.

## Middleware
- [ ] Implement circuit breaker if needed.
- [ ] Implement rate-limiting if needed.
- [ ] Set up sensible metrics (error-rate, response-time, etc.). A [prometheus](https://prometheus.io/) endpoint is ideal.
- [ ] Enforce body-size-limits if appropriate.

## Preparation
- [ ] Service is load-tested and/or benchmarked.
- [ ] Verify that your production configuration is valid (this should be the only thing different to staging, so make sure it’s correct).

## Deployment

### Docker / k8s
- [ ] Use a minimal multi-stage `Dockerfile`.

### Kubernetes


### Golang
- [ ] Compile your service(s) w/ `-buildmode=pie` (position independent executables).

### Network
- [ ] SSL is configured to get an A+ on [SSL Labs](https://www.ssllabs.com/).
