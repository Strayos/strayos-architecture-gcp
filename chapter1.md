# Technical Specification

## Introduction

The project is divided into 3 processes
- gcpfind.py - CLI that finds gcps in an image list.
- gcpfind_server - Program (written in go) that spawns gcpfind.py processes to handle request. 
- gcpfind_culster - Cluster of nodes running the gcpfind_server. 

Let's describe how these pieces work from a bottom-up perspective.

#### gcpfind.py

A typical run of gcpfind.py looks like this:

**$** Indicates standard out
**>** Indicates standard out


```
$ welcome to GCP detection
$ starting GCP detection
$ STAGE: GCP detection
$ PROGRESS: 0
$ PROGRESS: 50
$ PROGRESS: 100
$ RESPONSE: { gcp: "gcp1", file:"DJI001.JPG", location:[400, 555] }

```