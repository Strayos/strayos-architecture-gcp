# Technical Specification

## Introduction

The project is divided into 3 processes
- gcpfind.py - CLI that finds gcps in an image list.
- gcpfind_server - Program (written in go) that spawns gcpfind.py processes to handle request. 
- gcpfind_culster - Cluster of nodes running the gcpfind_server. 


