---
created: 2025-11-22
title: General principles for docker
tags:
  - docker
  - principle
aliases:
---
> [!quote] Make it work, make it secure, make it fast

S - security, B - build speed, C - clarity

- (SBC) pin specific versions
	- (SBC) base images (either major+minor or SHA256 hash)
	- (SC) system dependencies
	- (SC) application dependencies
- (SB) use small + secure base images
- (BC) protect the layer cache
	- (B) order commands by frequency of change
	- (B) `COPY` dependency requirements file -> install deps -> copy remaninig source code
	- (B) Use cache mounts
	- (B) Use `COPY --link`
	- (BC) Combine steps that are always linked (use heredocs to improve tidiness)

> [!quote] Make it work, make it SECURE, make it fast

- (SC) be explicit
	- (C) set working directory with `WORKDIR`
	- (C) indicate standard port with `EXPOSE`
	- (SC) Set default environment variables with `ENV`
- (SBC) Avoid unnecessary files
	- (SBC) Use .dockerignore
	- (SBC) Copy specific files
	- (S) Use non-root USER
- (SBC) Install only production dependencies
- (S) Avoid leaking sensitive information
- (SB) Leverage multi-stage builds