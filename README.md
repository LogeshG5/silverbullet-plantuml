# Silver Bullet plug for PlantUML diagrams

This plug adds basic [PlantUML](https://www.plantuml.com) support to Silver Bullet.

## Installation

The plug is installed like any other plug using SpaceLua. Just add `ghr:LogeshG5/silverbullet-plantuml` to the plugs array in your CONFIG page.

```space-lua
config.set {
  plugs = {
  "ghr:LogeshG5/silverbullet-plantuml"
  }
}
```

Run `Plugs: Update` command and off you go!

## Configuration

There are four types of configuration possible

1. [Remote Server](#1-remote-server-configuration)
2. [Docker Server](#2-docker-server-configuration)
3. [Local Server](#3-local-server-configuration)
4. [Script](#4-script-configuration)

### 1. Remote Server Configuration

Add this to your `SETTINGS.md`

```space-lua
config.set("plantuml", {serverurl="https://plantuml.com/plantuml"})
```

This configuration uses the offical PlantUML server to generate the diagram. If you do not want to send the data to PlantUML server check other configuration options.

### 2. Docker Server Configuration

Deploy your own PlantUML server with the [offical plantuml/plantuml-server](https://hub.docker.com/r/plantuml/plantuml-server) Docker image.

Use one of the following commands:

```bash
docker run -d -p 8080:8080 plantuml/plantuml-server:jetty
docker run -d -p 8080:8080 plantuml/plantuml-server:tomcat
```

Add this to your `SETTINGS.md`

```space-lua
config.set("plantuml", {serverurl="http://{ip or hostname}"})
```

> **Note**
> You might want to have a reverse proxy such as [Traefik](https://doc.traefik.io/traefik/), or [Caddy](https://caddyserver.com/) in front of the PlantUML container.

### 3. Local Server Configuration

[PlantUML](https://plantuml.com/download) needs to be installed in your machine at e.g., `/usr/local/bin/plantuml.jar`. Ensure you have the JDK installed on your system.

Launch a local PlantUML [http server](https://plantuml.com/picoweb).

```bash
java -jar /usr/local/bin/plantuml.jar -picoweb:8080
```

Add this to your `SETTINGS.md`

```space-lua
config.set("plantuml", {serverurl="http://localhost:8080"})
```

This configuration uses the local PlantUML server to generate the diagram. This doesn't send the data to PlantUML server. You will have to keep the local server running always.

### 4. Script Configuration

[PlantUML](https://plantuml.com/download) needs to be installed in your machine at e.g., `/usr/local/bin/plantuml.jar`. Ensure you have the JDK installed on your system.

The configured script is run with plantuml data encoded in base64 format as argument.

Create a helper script that decodes the input data and generate diagram. Copy the below contents to a script at e.g., `/usr/local/bin/gen_plantuml_svg`

Helper scripts for Linux & Windows can be found in [scripts](scripts) directory. Take a note to update the plantuml.jar paths.

```bash
#!/bin/bash
echo -e $1 | base64 -d | java -jar /usr/local/bin/plantuml.jar -tsvg -pipe
```

Make it an executable by running the following command

```bash
chmod a+x /usr/local/bin/gen_plantuml_svg
```

In your `SETTINGS.md` configure the path to the generator.

```space-lua
config.set("plantuml", {generator="/usr/local/bin/gen_plantuml_svg"})
```

This helper script is needed as I couldn't get to call the plantuml.jar directly from this plugin.

## Use

Put a plantuml block in your markdown:

````
```plantuml
@startuml
Alice -> Bob: Hi!
@enduml
```
````

And move your cursor outside of the block to live preview it!
