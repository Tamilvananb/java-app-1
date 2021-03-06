# chart-java-app

A generic java app chart that can be used to deploy java applications to your kubernetes cluster via helm. It's completely templatized and has a sample values.yaml file which looks like the following:

```yaml
javaApp:
  namespace: default
  name: appName
  version: 1.0.0
  type: jar
  #serviceAccount: serviceaccount-name 
  group: com.stackator
  deployment:
  # podAnnotations:
  # - key: myannotationkey
  #   value: myannotationvalue
    minReadySeconds: 5
    replicaCount: 2
    revisionHistoryLimit: 2
    probes:
      liveness:
        path: /health
        port: 8080
        scheme: HTTP
        initialDelaySeconds: 180
      readiness:
        path: /health
        port: 8080
        scheme: HTTP
        initialDelaySeconds: 10
  service:
    #ingressPath: /vis/
    #rewriteTarget: /
    ingressClass: external-ingress
    forceSslRedirect: true
    prometheusPort: 9779
    prometheusScrape: true
    expose: true
    additionalIngressAnnotations:
      - key: monitor.stakater.com/enabled
        value: true
    # ports:
    #   - name: http
    #     port: 8080
    #     protocol: TCP
    #     targetPort: 8080
  image:
    name: imageName
    tag: "tag"
    pullPolicy: IfNotPresent

  # rollingUpdateStrategy:
  #   strategy:
  #     rollingUpdate:
  #       maxSurge: 1
  #       maxUnavailable: 1
  #     type: RollingUpdate

  # mounts:
  #   secrets:
  #   - mountName: mountName
  #     mountPath: /some/path
  #     secretName: some-secret
  #   - mountName: mountName2
  #     mountPath: /some/path2
  #     secretName: some-secret2
  #     readOnly: true
  #   hostPaths:
  #   - mountName: name
  #     mountPath: /path
  #     hostPath: /host/path
  
  # env:
  # - name: key1
  #   value: value1
  # - name: key2
  #   value: value2
  # - name: key3
  #   value: value3

  # ports:
  # - containerPort: 8080
  #   name: http
  #   protocol: TCP
  # - containerPort: 9779
  #   name: prometheus
  #   protocol: TCP
  # - containerPort: 8778
  #   name: jolokia
  #   protocol: TCP
    
  imagePullSecrets:
    - image-pull-secret
```

You can comment / uncomment parts of values to customize this chart according to the app.

## Installing

Run `helm install --name app-name java-app -f values.yaml`
