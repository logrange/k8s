### Logrange k8s installation

Logrange k8s installation requires [Helm](https://helm.sh/) to be installed. The installation can be done either via [script](#script) or [manually](#manual). There are 4 key components which are installed during the installation: 

* **lr-aggregator:** streaming database where all the logs are stored/aggregated after they are gathered from the nodes.

* **lr-collector:** logs collecting agent, runs on every node, collects logs from all the containers and sends them to the aggregator.

* **lr-forwarder:** logs forwarding agent, used if you need to forward logs from your k8s cluster to some 3rd party system/application.

* **lr-configs:** all the configurations/settings on which other components rely on.

The components come preconfigured for k8s cluster logs gathering, so no special configuration is needed, until you have some very custom k8s installation and/or requirements.

After key components installation is done, [install](#logrange-cli-installation) our **CLI tool**, to make different kind of requests using **Logrange Query Language (LQL),** which is powerful and very SQL like!

#### Script:

Install:<br/>
```
$ curl -s http://get.logrange.io/k8s/install | bash -s -- --version v0.1.1
```

Uninstall:<br/>
```
$ curl -s http://get.logrange.io/k8s/install | bash -s -- --uninstall
```

_Note:_ Script uninstall works only if you did "script install" otherwise you should do uninstall [manually](#manual).

#### Manual:

Install:<br/>
```
$ helm repo add logrange http://get.logrange.io/k8s/helm/
$ helm repo update
$ helm install logrange/lr-configs 
$ helm install logrange/lr-aggregator
$ helm install logrange/lr-collector
$ helm install logrange/lr-forwarder
```

Uninstall:<br/>
```
$ helm list
$ helm delete <release name>
```

### Logrange CLI installation

```
$ curl -s http://get.logrange.io/install | bash -s -- lr -d /usr/local/bin
$ lr shell --server-addr=lr-aggregator.kube-system.svc.cluster.local:9966
```

### Log forwarding to 3rd party system

Execute steps in order:

1. Open `lr-forwarder` ConfigMap for edit:<br/>
```
$ kubectl edit configmap lr-forwarder --namespace=kube-system
```
2. Insert into `Workers` array, one or more blocks like shown below (substitute values in brackets):
```javascript
{
  "Name": "<NAME>",                          // name of your forwarder, e.g. "forwarder1"
  "Pipe": {
    "Name": "logrange.pipe=__default__",
  },
  "Sink": {
    "Type": "syslog",                        // for now only "syslog" is supported
    "Params": {
      "Protocol": "tcp",                     // "tcp", "udp" or "tls" (requires non-empty "TlsCAFile")
      "RemoteAddr": "<REMOTE_SYSLOG>:514",   // syslog server hostname/ip to where logs to be forwarded
      "TlsCAFile": "",                       // required if "Protocol" is "tls"
      "MessageSchema" : {
        "Facility": "local6",
        "Severity": "info",
        "Hostname": "{vars:pod}",
        "Tags": "{vars}",
        "Msg": "{msg}"
      }
    } 
  }
}
 ```
