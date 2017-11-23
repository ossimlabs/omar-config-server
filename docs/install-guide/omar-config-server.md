# OMAR Config Server

## Source Location
https://github.com/ossimlabs/omar-config-server

## Purpose

The omar-config-server is responsible for handing out configurations to each OMAR application by managing a repository of application configuration files. These files can be gathered from each application's installation guides.

## Installation in Openshift

**Assumption:** The omar-config-server docker image is pushed into the OpenShift server's internal docker registry and available to the project.

The OMAR Config Server can be deployed into OpenShift without any special volumes or secrets.

The server can be deployed into one of two modes:

1. Native - The server will serve application configurations from a local file system. These configurations can be attached via a ConfigMap to */home/omar/configs*
2. Remote - The server will serve application configurations from a remote Git service. Under this mode the remote Git repository can be initialized with optional environment variables.

### Environment variables

|Variable|Value|
|------|------|
|SPRING_CLOUD_CONFIG_SERVER_GIT_URI|Remote Git location|
|GIT_USER_NAME|Remote Git username|
|GIT_PASSWORD|Remote Git password|
|SPRING_PROFILES_ACTIVE|'native' or 'remote'|

By default, no environment variables are required. If none are specified the SPRING_PROFILES_ACTIVE is assumed to be 'native'

### An Example DeploymentConfig

```yaml
apiVersion: v1
kind: DeploymentConfig
metadata:
  annotations:
    openshift.io/generated-by: OpenShiftNewApp
  creationTimestamp: null
  generation: 1
  labels:
    app: omar-openshift
  name: omar-config-server
spec:
  replicas: 1
  selector:
    app: omar-openshift
    deploymentconfig: omar-config-server
  strategy:
    activeDeadlineSeconds: 21600
    resources: {}
    rollingParams:
      intervalSeconds: 1
      maxSurge: 25%
      maxUnavailable: 25%
      timeoutSeconds: 600
      updatePeriodSeconds: 1
    type: Rolling
  template:
    metadata:
      annotations:
        openshift.io/generated-by: OpenShiftNewApp
      creationTimestamp: null
      labels:
        app: omar-openshift
        deploymentconfig: omar-config-server
    spec:
      containers:
      - env:
        - name: GIT_USER_NAME
          value: auser
        - name: GIT_PASSWORD
          value: apassword
        - name: SPRING_PROFILES_ACTIVE
          value: remote
        image: 172.30.181.173:5000/o2/omar-config-server@sha256:8d8d8dd70494308cc46e6ccd4c62a6af0795251d204d34975dbe57c8df866b64
        imagePullPolicy: Always
        livenessProbe:
          failureThreshold: 3
          initialDelaySeconds: 60
          periodSeconds: 10
          successThreshold: 1
          tcpSocket:
            port: 8888
          timeoutSeconds: 1
        name: omar-config-server
        ports:
        - containerPort: 8888
          protocol: TCP
        readinessProbe:
          failureThreshold: 3
          initialDelaySeconds: 30
          periodSeconds: 10
          successThreshold: 1
          tcpSocket:
            port: 8888
          timeoutSeconds: 1
        resources: {}
        terminationMessagePath: /dev/termination-log
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      securityContext: {}
      terminationGracePeriodSeconds: 30
  test: false
  triggers:
  - type: ConfigChange
  - imageChangeParams:
      automatic: true
      containerNames:
      - omar-config-server
      from:
        kind: ImageStreamTag
        name: omar-config-server:latest
        namespace: o2
    type: ImageChange
status:
  availableReplicas: 0
  latestVersion: 0
  observedGeneration: 0
  replicas: 0
  unavailableReplicas: 0
  updatedReplicas: 0
```
