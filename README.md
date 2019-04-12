### Logrange k8s installation

#### Script:

Launch installation script:<br/>
`curl -s https://raw.githubusercontent.com/logrange/k8s/master/generic/install | bash`

#### Manual:

Execute steps in order:

1. add Logrange helm repo:<br/>
`helm repo add logrange https://raw.githubusercontent.com/logrange/k8s/master/generic/charts/_repo/`

2. install Logrange storage:<br/>
    a) `kubectl label nodes <NODE_NAME> logrange.io/node=`<br/>
    b) `helm install logrange/logrange`

3. install Logrange collector agent:<br/>
`helm install logrange/collector`

4. install Logrange forwarding agent:<br/>
`helm install logrange/forwarder`

### Log forwarding to 3rd party system

Execute steps in order:

1. Open 'forwarder' ConfigMap for edit:<br/>
`kubectl edit configmap forwarder --namespace=kube-system`

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
          "Stream": {
            "Source": "",
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
