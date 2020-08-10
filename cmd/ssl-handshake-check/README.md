## SSL Handshake Status Check

The *SSL Handshake Check* checks that SSL certificates are valid and a TLS handshake can be completed successfully against the domain/port specified.

The check runs every 5 minutes (spec.runInterval), with a check timeout set to 3 minutes (spec.timeout). If the check
does not complete within the given timeout it will report a timeout error on the status page.

#### SSL Expiry Check Kube Spec:
```yaml
apiVersion: comcast.github.io/v1
kind: KuberhealthyCheck
metadata:
  name: ssl-handshake-check
  namespace: kuberhealthy
spec:
  runInterval: 5m
  timeout: 3m
  podSpec:
    containers:
      - env:
          # Domain name env variable must be updated to the domain on which you wish to check the SSL for
          - name: DOMAINNAME
            value: "google.com"
          # If not using default SSL port of 443, port name env variable must be updated  
          - name: PORT
            value: "443"
        image: kuberhealthy/ssl-handshake-check:v1.0.0
        imagePullPolicy: IfNotPresent
        name: main
        resources:
          requests:
            cpu: 10m
            memory: 50Mi
```

#### How-to

To implement the SSL Handshake Check with Kuberhealthy, update the spec sheet to the domain name and port number you wish to test and run:
`kubectl apply -f https://raw.githubusercontent.com/Comcast/kuberhealthy/v2.3.0/cmd/ssl-handshake-check/ssl-handshake-check.yaml`

 Make sure you are using the latest release of Kuberhealthy 2.3.0.