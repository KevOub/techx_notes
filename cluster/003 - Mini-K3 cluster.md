## The Flow

### What I did
1) Download Raspberry Pi Image from raspberry pi website
2) Boot into it and use `sudo raspi-config`
	1) modify the `hostname`, `enable SSH`, and install a backdoor SSH private key :)
3) Used the following dd command to create a bootable image this from `cluster00`
	```bash
	sudo pv -B 32m /dev/sdb | dd of=$HOME/Desktop/raspberry.img bs=512 conv=sparse 
	```
4) To use the dd image:
    ```bash
	sudo dd if=raspberry.iso of=/dev/sdb bs=1024k status=progress
	```
	
### Post install process
1) Change hostname using `sudo raspi-config`
2) Add the following line to `/boot/cmdline.txt` to allow docker. Otherwise, it runs into weird issues
	1) `cgroup_memory=1 cgroup_enable=memory`
3) Use the following command to build the `master` / `main` node of the kubernetes cluster
	 ```bash
	 k3sup install --ip 10.47.11.128 --user pi --ssh-key $HOME/.ssh/techx_rsa.pub
	 ```
	 [From here](https://github.com/alexellis/k3sup)
4) Check kubernetes cluster with:
```bash
export KUBECONFIG=/home/kevin/programming/techx/kube00/kubeconfig # path to kubeconfig
kubectl config set-context default
kubectl get node -o wide  
```
6) Install compose
https://kubernetes.io/docs/tasks/configure-pod-container/translate-compose-kubernetes/
7) Install dashboard
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.3.1/aio/deploy/recommended.yaml                  
```
8) Get token for access (That way, you can login with this token)
```bash
$TOKEN=$(kubectl -n kube-system describe secret default| awk '$1=="token:"{print $2}')    
```
9) Set the token to be admin
```bash
kubectl config set-credentials kubernetes-admin --token="${TOKEN}"        
```
10) Finally, run `kubectl proxy` and goto  `http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/` for the service