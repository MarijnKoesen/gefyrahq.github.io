---
layout: page
title: Try it with Docker Desktop
permalink: /try-it-docker-desktop-osx/
nav_order: 5
---
# Try it on MacOS
This example is really similar to the try it yourself guide.
Howeever, Docker Desktop's Kubernetes comes with the specialities
which must be taken care of to make this guide work.

**Note:** Gefyra works the same for different Kubernetes distributions.
Docker Desktop is missing a natively installed ingress which must
be added to the cluster to get things working.

You can easily try Gefyra yourself following this small example.
{: .fs-6 .fw-300 }

## Prerequisites
1. Follow the [installation](https://gefyra.dev/installation) for your preferred platform.

2. Start the Kubernetes cluster that comes with Docker Desktop.
#[Screenshot]
4. Apply some workload, for example from the testing directory of this repo:  
`kubectl apply -f testing/workloads/hello.yaml`
Check out this workload running under: http://hello.127.0.0.1.nip.io:8080/    

## Running Gefyra
4. Set up Gefyra with `gefyra up`
5. Run a local Docker image with Gefyra in order to  make it part of the cluster.
   1. Build your Docker image with a local tag, for example from the testing directory:
   `cd testing/images/ && docker build -f Dockerfile.local . -t pyserver`
   2. Execute Gefyra's run command:    
   `gefyra run -i pyserver -N mypyserver -n default`
   3. _Exec_ into the running container and look around. You will find the container to run within your Kubernetes cluster.  
   `docker exec -it mypyserver bash`  
   `wget -O- hello-nginx` will print out the website of the cluster service _hello-nginx_ from within the cluster.
7. Create a bridge in order to intercept the traffic to the cluster application with the one running locally:    
`gefyra bridge -N mypyserver -n default --deployment hello-nginxdemo --port 80:8000 --container-name hello-nginx -I mypybridge`    
Check out the locally running server comes up under: http://hello.127.0.0.1.nip.io:8080/  
8. List all running _bridges_:  
`gefyra list --bridges`
9. _Unbridge_ the local container and reset the cluster to its original state: 
`gefyra unbridge -N mypybridge`
Check out the initial response from: http://hello.127.0.0.1.nip.io:8080/  

## Cleaning up
9. Remove Gefyra's components from the cluster with `gefyra down`
10. Optional: Reset the cluster on Docker Desktop.