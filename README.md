# Hello Java Agent Buildpack

This project shows how to add the SignalFx Java Tracing Agent (APM) to a Cloud Foundry app, using a custom builpack.

## Notes for Developers

Add this buildpack to your manifest:

```yaml
---
applications:
- name: myapp
  memory: 1G
  random-route: true
  path: target/myapp.jar
  buildpacks:
  - https://github.com/ecointet/signalfx-agent-buildpack.git
  - https://github.com/cloudfoundry/java-buildpack.git
  env:
    SIGNALFX_SERVICE_NAME: myapp
    SIGNALFX_ENDPOINT_URL: "http://localhost:8080/v1/trace"
```

This buildpack is actually relying on the official
[Cloud Foundry Java Buildpack](https://github.com/cloudfoundry/java-buildpack)
ability to support multiple buildpacks.
You need to include this buildpack before the Java Buildpack.

This buildpack defines an extension which is read by the Java Buildpack
when the droplet is being built.

A standard `supply` script will automatically download the Java agent
from its repository, and then will generate a file `config.yml` which
will be read by the Java Buildpack to add the Java agent to the command
line.

As soon as you start your app, you should see this log entry:
```text
2019-05-25T23:43:50.058+02:00 [APP/PROC/WEB/0] [OUT] Applying signalfx-javaagent buildpack...!
```

## Notes for Operators

You can download the last stable release from this GitHub (signalfx-apm-buildpack.zip) and upload it to your Cloud Foundry Platform.

`cf create-buildpack signalfx-apm-buildpack signalfx-apm-buildpack-1-0.zip 10`
`cf buildpacks`