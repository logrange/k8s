### Logrange k8s installation

#### Script:

Launch installation script:<br/>
`curl -s http://get.logrange.io/k8s/install | bash`

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
`curl -s http://get.logrange.io/install | bash`

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
       // <REMOTE_SYSLOG>    : syslog server hostname/ip to where logs to be forwarded, 
       //                      e.g. "log-collector.kube-system.svc.cluster.local"
    
        {
          "Name": "<NAME>",
          "Pipe": {
            "From": "",
            "Filter": ""
          },
          "Sink": {
            "Type": "syslog", 
            "Params": {
              "Protocol": "tcp",
              "RemoteAddr": "<REMOTE_SYSLOG>:514",
              "TlsCAFile": "",
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
