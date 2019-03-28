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

### Logrange CLI installation

Execute steps in order:

1. install Logrange cli command:<br/>
`curl -s https://raw.githubusercontent.com/logrange/logrange/master/build/install | bash`

2. run Logrange shell:<br/>
`lr shell --server-addr=logrange.kube-system.svc.cluster.local:9966`

### Log forwarding to 3rd party system

Execute steps in order:

1. Open 'forwarder' ConfigMap for edit:<br/>
`kubectl edit configmap forwarder --namespace=kube-system`

2. Insert into `Workers` array, one or more blocks like shown below (substitute values in brackets):
    ```json
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
