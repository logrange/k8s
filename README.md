## Logrange k8s installation

1. add Logrange helm repo:<br/>
`helm repo add logrange https://raw.githubusercontent.com/logrange/k8s/master/generic/charts/_repo/`

2. install Logrange storage:<br/>
`helm install logrange/logrange`

3. install Logrange collector agent:<br/>
`helm install logrange/collector`

4. install Logrange cli command:<br/>
`curl https://raw.githubusercontent.com/logrange/logrange/master/build/install | bash`

Done...

