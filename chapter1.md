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
**s** Indicates standard out


```
$ welcome to GCP detection
$ starting GCP detection
$ STAGE: GCP detection
$ PROGRESS: 0
$ PROGRESS: 50
$ PROGRESS: 100
$ RESPONSE: { gcp: "gcp1", file:"DJI001.JPG", location:[400, 555] }
$ Please send feedback
> { gcp: "gcp1", file:"DJI001.jpg", location:[450, 555] }
$ Response: { gcp: "gcp3", file: "DJI003.JPG", location: [300, 330] }
$ Please send feedback
> { gcp: "gcp3", file:"DJI001.jpg", location:[330, 300] }
... repeat until feedback has been sent for every gcp file. 
$ STAGE: GCP File Generation
$ Response: { gcp: "/home/code/gcp_list.txt", feedback: {} }
$ EXIT: 0
```

There are several things of note. 
1. Any output printed that does not begin with a whitelisted prefix is ignored by the automation script gcpfind_server, and is purely for user input.
 1.1 The whitelisted prefixes are
 - STAGE: name of the stage: Stage represents where gcpfind is currently at
 - PROGRESS: number from 0..100 representing the progress at this stage: Progress indicates the progress of the current stage
 - RESPONSE: json response: Response notifies gcpfind_server that data should be read in.
 - WAITING: : Waiting notifies gcpfind_server that data should be sent in
 - EXIT: return code: 0 = SUCCESS xxx= FAILURE: Exit the program immediately
2. 
2. During the "GCP detection stage" RESPONSE: and PROGRESS: commands may be intermixed (Progress: 0, Response: {}, Progress: 10, Response: {})
3. The WAIT: command does not stall the program. It just indicates that something may be written to STDIN. 

#### gcpfind_server