# docker-phabricator
--------------------

This image configures a [Phabricator](http://phabricator.org) installation that runs within nginx. The install is configured
using the `script.pre` file (example at `sample/script.pre`), which contains the settings that can be found in the
[configuration documentation](https://secure.phabricator.com/book/phabricator/article/advanced_configuration/). Feel free to add
more configuration settings in that file. Please be sure to mark this file as an executable.

### To run this image:

  ```
    docker run -p 22:22 -p 22280:22280 -p 80:80 -p 443:443 -v /path/to/config:/config -v /path/to/repo/storage:/srv/repo --name=phabricator ajagnanan/docker-phabricator
  ```

A docker-compose file is included as an example and can also be used to run the container locally.

  ```
    docker-compose up
  ```

#### What do these parameters do?

  ```
    -p 22:22 = forward the host's SSH port to Phabricator for repository access
    -p 22280:22280 = forward the host's 22280 port for the notification server
    -p 80:80 -p 443:443 = http and https ports to bind to on the host
    -v path/to/config:/config = map the configuration from the host to the container
    -v path/to/repo/storage:/srv/repo = map the repository storage from the host to the container
    --name phabricator = the name of the container
    ajagnanan/docker-phabricator = the name of the image
  ```

### Enabling SSL

To enable SSL, place `cert.pem` and `cert.key` files alongside `script.pre`.  The Docker container will automatically
detect the presence of the certificates and configure nginx to run with SSL enabled.

### Linking to a DB container

If you are running MySQL in a Docker container (e.g. using the `mysql/mysql` container), you can configure the `script.pre` file like so to use the linked MariaDB container:

```
    ./bin/config set mysql.host "$LINKED_MYSQL_PORT_3306_TCP_ADDR"
    ./bin/config set mysql.port "$LINKED_MYSQL_PORT_3306_TCP_PORT"
```

### SSH / Login

    ```
    Username: root
    Password: linux
    Port: 24
    ```
CVS repository hosting is done on port `22`.

### Extra information

  - Make sure you configure Phabricator to be backed by an external MySQL database.
  - Map a directory from the host for repository storage. Containers are volatile unless using a tools like [Flocker](https://clusterhq.com)

*** Image based on [hachque's phabricator configuration](https://github.com/hach-que-docker/phabricator) with some extra information.
