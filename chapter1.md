# Technical Specification

## Introduction

The project is divided into 3 processes

* gcpfind.py - CLI that finds gcps in an image list.
* gcpfind\_server - Program \(written in go\) that spawns gcpfind.py processes to handle request. 
* gcpfind\_culster - Cluster of nodes running the gcpfind\_server. 

Let's describe how these pieces work from a bottom-up perspective.

#### gcpfind.py

A typical run of gcpfind.py looks like this:

**$** Indicates standard out  
**&gt;** Indicates standard out

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
1. Any output printed that does not begin with a whitelisted prefix is ignored by the automation script gcpfind\_server, and is purely for user input.

2. The whitelisted prefixes are

* **STAGE**: _&lt;name of the stage&gt; _: Stage represents where gcpfind is currently at
* **PROGRESS**: _&lt;number from 0..100 representing the progress at this stage&gt;_: Progress indicates the progress of the current stage
* **RESPONSE**: _&lt;json response&gt;_ : Response notifies gcpfind\_server that data should be read in.
* **WAITING**:_&lt;&gt;_ : Waiting notifies gcpfind\_server that data should be sent in
* **EXIT**: _&lt;return code: 0 = SUCCESS xxx= FAILURE&gt;_: Exit the program immediately
  1. During the "GCP detection stage" RESPONSE: and PROGRESS: commands may be intermixed \(Progress: 0, Response: {}, Progress: 10, Response: {}\)
  2. The WAIT: command does not stall the program. It just indicates that something may be written to STDIN. 

#### gcpfind\_server



