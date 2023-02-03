# kne

Note: this stuff worked as of about August/September 2022. Hopefully it still does, but the warranty has expired :)

Sample configs and a pb.txt file for launching a six router XRd topology (plus attached workload "VMs" using Google's delightful KNE tool: https://github.com/google/kne


![6node-xrd](https://github.com/brmcdoug/kne/blob/main/6node-xrd-topo.png)

Repo also includes a modified cisco.go file to accommodate XRd ENV and setup script changes which occurred between 7.6.x and 7.7.x releases.

Use these instructions if delploying with XRd 7.7.x or later 

Instructions:

1. Pull XRd docker .tgz then load into docker and kind:
```
docker load -i xrd-container-x64.dockerv1.tgz-7.8.1
docker tag localhost/ios-xr:7.7.1.26I localhost/xrd:7.8.1
kind load docker-image localhost/xrd:7.8.1 --name=kne
```

2. Install and setup kne: https://github.com/google/kne/blob/main/docs/setup.md

2. Before the "cd kne_cli" and "go install" step replace this file: https://github.com/google/kne/blob/main/topo/node/cisco/cisco.go with the cisco.go file in this repo:
 
https://github.com/brmcdoug/kne/blob/main/cisco.go

3. Continue following kne setup

4. Replace kne examples/3node-withtraffic.pb.txt with the 6node-xrd.pb.txt file: 

https://github.com/brmcdoug/kne/blob/main/6node-xrd.pb.txt

5. run "kne_cli create <6node-xrd.pb.txt>"

6. check containers:

```
kubectl get pods -n xrd
kubectl logs -n xrd r25
etc.
```

7. Access to XRd CLI and attached "VM" containers

```
kubectl exec -it -n xrd r25 /pkg/bin/xr_cli.sh
kubectl exec -it -n xrd vm-1 /bin/sh
etc.
```
8. The config files in this repo will a setup user/pw combo of cisco/cisco123 on all XRd's

Notes:
XRd may take a few minutes to come up and provide a username/pw prompt. It may also take a few minutes for interfaces to come up
As of this writing SSH to the XRd nodes isn't working...we hope to get that fixed soon

My first deploy attempt failed due to lack of inotify resources. Deploy worked after bumping inotify up to 65k:
```
cat /proc/sys/fs/inotify/max_user_watches
echo fs.inotify.max_user_watches=65536 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p

cat /proc/sys/fs/inotify/max_user_instances
echo fs.inotify.max_user_instances=65536 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```


