### Logrange k8s installation

Logrange k8s installation requires [Helm](https://helm.sh/) to be installed. The installation can be done either via [script](#Script:) or [manually](#Manual:). There are 4 key components which are installed during the installation: 

* **lr-aggregator:** streaming database where all the logs are stored/aggregated after they are gathered from the nodes.

* **lr-collector:** logs collecting agent, runs on every node, collects logs from all the containers and sends them to the aggregator.

* **lr-forwarder:** logs forwarding agent, used if you need to forward logs from your k8s cluster to some 3rd party system/application.

* **lr-configs:** all the configurations/settings on which other components rely on.

The components come preconfigured for k8s cluster logs gathering, so no special configuration is needed, until you have some very custom k8s installation and/or requirements.

After key components installation is done, [install](#Logrange CLI installation) our **CLI tool**, to make different kind of requests using **Logrange Query Language (LQL),** which is powerful and very SQL like!

#### Script:

Launch install script:<br/>
`curl -s http://get.logrange.io/k8s/install | bash -s -- --version v0.0.4`

Launch uninstall script:<br/>
`curl -s http://get.logrange.io/k8s/install | bash -s -- --uninstall [--wipe]`

#### Manual:

Execute steps in order:

1. add Logrange helm repo:<br/>
`helm repo add logrange http://get.logrange.io/k8s/helm/ && helm repo update`

2. install Logrange configs:<br/>
`helm install logrange/lr-configs`

3. install Logrange storage:<br/>
    a) `kubectl label nodes <NODE_NAME> logrange.io/node=`<br/>
    b) `helm install logrange/lr-aggregator`

4. install Logrange collector agent:<br/>
`helm install logrange/lr-collector`

5. install Logrange forwarding agent:<br/>
`helm install logrange/lr-forwarder`

### Logrange CLI installation

Execute steps in order:

1. install Logrange cli command:<br/>
`curl -s http://get.logrange.io/install | bash -s -- lr -d /usr/local/bin`

2. run Logrange shell:<br/>
`lr shell --server-addr=lr-aggregator.kube-system.svc.cluster.local:9966`

### Log forwarding to 3rd party system

Execute steps in order:

1. Open 'lr-forwarder' ConfigMap for edit:<br/>
`kubectl edit configmap lr-forwarder --namespace=kube-system`

2. Insert into `Workers` array, one or more blocks like shown below (substitute values in brackets):
    ```javascript
    
       //    
       // Note: Only syslog protocol is supported for for now...
       //
       // <NAME>             : name of your forwarder, e.g. "forwarder1"
       // <REMOTE_SYSLOG>    : syslog server hostname/ip to where logs to be forwarded
       //
    
        {
          "Name": "<NAME>",
          "Pipe": {
            "Name": "logrange.pipe=__default__",
          },
          "Sink": {
            "Type": "syslog",                      // syslog only
            "Params": {
              "Protocol": "tcp",                   // tcp, udp or tls (requires non-empty 'TlsCAFile')
              "RemoteAddr": "<REMOTE_SYSLOG>:514",
              "TlsCAFile": "",                     // required if 'Protocol' is 'tls'
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
