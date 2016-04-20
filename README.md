# Islandora CLAW: All in One Docker Image

[![Docker Stars](https://img.shields.io/docker/stars/islandora/claw-all-in-one.svg)](https://hub.docker.com/r/islandora/claw-all-in-one/)
[![Docker Pulls](https://img.shields.io/docker/pulls/islandora/claw-all-in-one.svg)](https://hub.docker.com/r/islandora/claw-all-in-one/)
[![Image Size](https://img.shields.io/imagelayers/image-size/islandora/claw-all-in-one/latest.svg)](https://imagelayers.io/?images=islandora/claw-all-in-one:latest)
[![Image Layers](https://img.shields.io/imagelayers/layers/islandora/claw-all-in-one/latest.svg)](https://imagelayers.io/?images=islandora/claw-all-in-one:latest)

### Introduction

This is a single image will all the dependencies required to run Islandora CLAW.

It's provided for the purposes of easing testing and development, but is not intended for production use. 

### Install

Please see the [install
documentation](https://github.com/Islandora-CLAW/claw-docker/blob/master/docs/install-guide.md)
in our main Docker repository.

### Usage

#### Kitematic

With [Kitematic](https://kitematic.com/) you can download this Image from Docker Hub.

Please read the
[Kitematic documentation](https://docs.docker.com/kitematic/userguide/) for more
details.

If using Kitematic solely you must provide the *required*
[Environment Variables](#environment_variables) documented below.

#### Docker Compose

If using Docker Compose, you first must generate the environment variables
files, by running the command ```./commands/generate-env-files``` documented
below.

Once the files have been generated simply run:

```docker-compose up```

To run in the foreground.

Please refer to our [Docker User
Guide](https://github.com/Islandora-CLAW/claw-docker/blob/master/docs/docker-user-guide.md)
for more information on using Docker Compose.

### Includes

* PHP 5.6.17
* Java 8
* Drupal 7.43
* Tomcat 7.0.68
* Fedora 4.4.0
* Solr 4.10.3
* Blazegraph 1.5.1
* TODO Karaf 4.0.4

### Build Arguments

| Variable           | Required |                                                      Default |
|--------------------|----------|--------------------------------------------------------------|
| TOMCAT_VERSION     | no       |                                                       7.0.68 |
| MAVEN_VERSION      | no       |                                                        3.3.9 |
| TOMCAT_VERSION     | no       |                                                       7.0.68 |
| FEDORA_VERSION     | no       |                                                        4.4.0 |
| BLAZEGRAPH_VERSION | no       |                                                        1.5.1 |
| SOLR_VERSION       | no       |                                                       4.10.3 |
| SOLR_DEPENDENCIES  | no       | log4j-1.2.17.jar slf4j-api-1.7.6.jar slf4j-log4j12-1.7.6.jar |
| KARAF_VERSION      | no       |                                                        4.0.4 |
| DRUSH_VERSION      | no       |                                                        8.0.2 |
| DRUPAL_VERSION     | no       |                                                         7.43 |

**SOLR_DEPENDENCIES** are the jar files to copy from ```example/lib/ext``` into
```$CATALINA_HOME/lib```.

**Example:**
```bash
docker build --build-arg "TOMCAT_VERSION=7.0.68" -t islandora/claw-all-in-one .
```

### Environment Variables

| Variable                        | Required | Default                                                                                                                                                                                                                                                |
|---------------------------------|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| TOMCAT_ADMIN_USER               | no       | admin                                                                                                                                                                                                                                                  |
| TOMCAT_ADMIN_PASSWORD           | yes      |                                                                                                                                                                                                                                                        |
| JAVA_OPTS                       | no       |                                                                                                                                                                                                                                                        |
| CATALINA_OPTS                   | no       | -server -XX:+CMSClassUnloadingEnabled -Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom" -Dfcrepo.home=/mnt/fedora-data -Dcom.bigdata.rdf.sail.webapp.ConfigParams.propertyFile=/opt/tomcat/webapps/bigdata/WEB-INF/RWStore.properties" |
| KARAF_OPTS                      | no       | -Xms128M -Xmx512M -XX:+UnlockDiagnosticVMOptions -XX:+UnsyncloadClass                                                                                                                                                                                  |
| MYSQL_ROOT_USER                 | no       | root                                                                                                                                                                                                                                                   |
| MYSQL_ROOT_PASSWORD             | yes      |                                                                                                                                                                                                                                                        |
| DRUPAL_SITE_NAME                | no       | Islandora CLAW                                                                                                                                                                                                                                         |
| DRUPAL_SITE_EMAIL               | no       | webmaster@localhost.com                                                                                                                                                                                                                                |
| DRUPAL_SITE_LOCALE              | no       | en-us                                                                                                                                                                                                                                                  |
| DRUPAL_SITE_ACCOUNT_NAME        | no       | admin                                                                                                                                                                                                                                                  |
| DRUPAL_SITE_ACCOUNT_PASSWORD    | yes      |                                                                                                                                                                                                                                                        |
| DRUPAL_SITE_ACCOUNT_EMAIL       | no       | webmaster@localhost.com                                                                                                                                                                                                                                |
| DRUPAL_SITE_DB_NAME             | no       | drupal_default                                                                                                                                                                                                                                         |
| DRUPAL_SITE_DB_USER             | no       | drupal                                                                                                                                                                                                                                                 |
| DRUPAL_SITE_DB_PASSWORD         | yes      |                                                                                                                                                                                                                                                        |
| DRUPAL_SITE_DB_REPLACE_EXISTING | no       | no                                                                                                                                                                                                                                                     |

**DRUPAL_SITE_DB_REPLACE:** If set to 'yes', the Drupal database will be replaced on start-up. 

**Example (foreground, random host port mapping, auto-remove, interactive shell):**
```bash
docker run --rm -ti -P  \
                    -e "TOMCAT_ADMIN_PASSWORD=your_super_secure_password" \
                    -e "MYSQL_ROOT_PASSWORD=your_super_secure_password" \
                    -e "DRUPAL_SITE_ACCOUNT_PASSWORD=your_super_secure_password" \
                    -e "DRUPAL_SITE_DB_PASSWORD=your_super_secure_password" \
                    islandora/claw-all-in-one ash
```

### Commands

For convenience a number of commands are provided in the [commands](/commands)
folder.

| Command            | Arguments                                              | Defaults | Notes                                                    |
|--------------------|--------------------------------------------------------|----------|----------------------------------------------------------|
| build              | none                                                   |          | Build this image with the default settings.              |
| drush              | [drush documentation](http://www.drush.org/en/master/) |          | Execute a drush command within the container             |
| karaf              | none                                                   |          | Connect to the Karaf client.                             |
| shell              | none                                                   |          | Connect to the container and open an interactive shell.  |
| exec               | any valid command                                      |          | Executes the given command in the container.             |
| generate-env-files | none                                                   |          | Generates the .env files required to run docker-compose. |

**N.B.** The command above assume you used ```docker-compose``` to start the
container or you have given the container the name **islandora-claw**.

### Maintainers/Sponsors

* UPEI
* discoverygarden inc.
* LYRASIS
* McMaster University
* University of Limerick
* York University
* University of Manitoba
* Simon Fraser University
* PALS
* American Philosophical Society
* common media inc.

Current maintainers:

* [Nigel Banks](https://github.com/nigelgbanks)

## Development

If you would like to contribute, please get involved with the [Islandora Fedora 4 Interest Group](https://github.com/Islandora/Islandora-Fedora4-Interest-Group). We love to hear from you!

If you would like to contribute code to the project, you need to be covered by an Islandora Foundation [Contributor License Agreement](http://islandora.ca/sites/default/files/islandora_cla.pdf) or [Corporate Contributor Licencse Agreement](http://islandora.ca/sites/default/files/islandora_ccla.pdf). Please see the [Contributors](http://islandora.ca/resources/contributors) pages on Islandora.ca for more information.

### License

[MIT](https://opensource.org/licenses/MIT)
