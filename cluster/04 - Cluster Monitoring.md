```markdown

So I downloaded Carlos' [cluster-monitoring](https://github.com/carlosedp/cluster-monitoring) project to the Pi and deployed it instead:

1.  Log into the master Pi: `ssh pirate@turing-master`
2.  Switch to the root user: `sudo su`
3.  Install prerequisite tools: `apt-get update && apt-get install -y build-essential golang`
4.  Clone the project: `git clone https://github.com/carlosedp/cluster-monitoring.git`
5.  Edit the `vars.jsonnet` file, tweaking the IP addresses to servers in your cluster, and enabling the `k3s` option and `armExporter`.
6.  Run `make vendor && make` (this takes a while)
7.  Run the final commands in the [Quickstart for K3s](https://github.com/carlosedp/cluster-monitoring#quickstart-for-k3s) guide to `kubectl apply` manifests, and wait for everything to roll out.

Once everything has started successfully, you should be able to access Prometheus, Alertmanager, and Grafana at the URLs you can retrieve via:


`kubectl get ingress -n monitoring`


```
