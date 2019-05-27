# Hello Java Agent Buildpack

This project shows how to add a custom Java agent to a Cloud Foundry app,
using a custom builpack.

Add this buildpack to your manifest:
```yaml
---
applications:
- name: myapp
  memory: 1G
  random-route: true
  path: target/myapp.jar
  buildpacks:
  - https://github.com/alexandreroman/hello-javaagent-buildpack.git
  - https://github.com/cloudfoundry/java-buildpack.git
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
2019-05-25T23:43:50.058+02:00 [APP/PROC/WEB/0] [OUT] Hello world, from Java Agent!
```

## Contribute

Contributions are always welcome!

Feel free to open issues & send PR.

## License

Copyright &copy; 2019 [Pivotal Software, Inc](https://pivotal.io).

This project is licensed under the [Apache Software License version 2.0](https://www.apache.org/licenses/LICENSE-2.0).
