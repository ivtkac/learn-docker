---
created: 2025-11-22
title: Docker usages
tags:
  - docker
aliases:
---
- [[СКБД|Databases]]:
	- **volume** to persistent data: (data persists beyond the container)
	- **bind** for additional config (config files)
	- **environment variables**: runtime behavior (like admin password)
- Interactive test environment:
	- [[Операційна система|OS]] (`docker run -it --rm ubuntu:22.04`)
	- Programming runtimes (`docker run -it --rm python:3.11.1`)
- CLI Utilities
- Improving the ergonomics