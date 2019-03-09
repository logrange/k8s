## Logrange k8s installation

1. add Logrange helm repo:<br/>
`helm repo add logrange https://raw.githubusercontent.com/logrange/k8s/master/generic/charts/_repo/`

2. install Logrange storage:<br/>
`helm install logrange/logrange`

3. install Logrange collector agent:<br/>
`helm install logrange/collector`

4. install Logrange cli command:<br/>
`curl -s https://raw.githubusercontent.com/logrange/logrange/master/build/install | bash`

5. run Logrange cli command:<br/>
`./lr shell --server-addr=logrange.kube-system.svc.cluster.local:9966`

Done...

