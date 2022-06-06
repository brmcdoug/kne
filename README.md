# kne

Sample configs and a pb.txt file for launching a six router XRd topology using Google's delightful KNE tool: https://github.com/google/kne

Repo also includes a modified cisco.go file to accommodate XRd ENV and setup script changes which occurred between 7.6.x and 7.7.x releases.

Use these instructions if delploying with XRd 7.7.x or later 

Instructions:

1. Pull XRd docker .tgz then load into docker and kind:
```
docker load -i xrd-container-x64.dockerv1.tgz-7.7.1.26I 
kind load docker-image localhost/xrd:7.7.1.26I --name=kne
```

2. Install and setup kne: https://github.com/google/kne/blob/main/docs/setup.md

2. Before the "cd kne_cli" and "go install" step replace this file: https://github.com/google/kne/blob/main/topo/node/cisco/cisco.go with the cisco.go file in this repo: 

3. Continue following kne setup

4. Replace kne examples/3node-withtraffic.pb.txt with the 6node-xrd.pb.txt file: 


