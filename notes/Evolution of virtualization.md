---
created: 2025-11-21
title: Evolution of virtualization
tags:
  - history
  - virtualiation
  - containerzation
aliases:
---
1. bare metal
	- hellish dependency conflicts
	- low utilization efficiency
	- large blast radius
	- slow start up & shut down speed (minutes)
	- very slow provisioning & decommissioning (hours to days)
2. [[Віртуальні машини|virtual machines]]
	- no dependency conflicts
	- better utilization efficiency
	- small blast radius
	- faster startup and shutdown (minutes)
	- faster  provisioning & decommissioning (seconds)
3. [[container]]
	- no dependency conflicts
	- even better utilization efficiency
	- small blast radius
	- even faster startup and shutdown (seconds)
	- even faster [[провіжінг|provisioning]] & decommissioning (seconds)
	- lightweight enough to use in development
4. virtual machines + containers + orchestrators