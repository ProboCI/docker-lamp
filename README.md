# Probo Image Builder 2000
This  project was created to use a Dockerfile and automated build on the Docker Hub to create Docker images for Probo.CI.
This project also uses Docker image inheritance to build automated image builds from upstream repos on the Docker Hub.

## Image Testing
Docker images should be tested to ensure that all advertised services and installed applications are available and working properly on each image.

Below you can find commands to build each image tag manually for testing purposes.

Once tested, changes to the Dockerfile can be pushed to the Docker Hub to kick off a new automated build.

Images can be built manually from the root of the repository using the -f parameter to specify the Dockerfile to build the image from.

**Note:** There are some files doubled up that I would have liked to make some sort of shared files, but due to the way that the Docker Hub works to generated automated builds I don't think it will work the way I wanted.

### Build Ubuntu Images
Currently we are only building Ubuntu images, but this automated format of building images on the Docker Hub will allow us to extend our image library to other Linux distributions and other types of images.

#### Ubuntu 14.04
Below you can find the commands to manually build the following Ubuntu 14.04 images that are compatible with Probo.CI:
  - `proboci/lamp:ubuntu14.04-apache2.4-mysql5.5-php5.6`
  - `proboci/lamp:ubuntu14.04-apache2.4-mysql5.5-php7.0`
  - `proboci/lamp:ubuntu14.04-apache2.4-mysql5.5-php7.1`
  - `proboci/lamp:ubuntu14.04-apache2.4-mysql5.5-php7.2`

**Note:** These images will be rebuilt automatically when changes are pushed to the master branch.

#### Apache 2.4, MySQL 5.5, PHP 5.6

    docker build -t proboci/lamp:ubuntu14.04-apache2.4-mysql5.5-php5.6 -f ubuntu/14.04/mysql/5.5/php/5.6/Dockerfile .

#### Apache 2.4, MySQL 5.5, PHP 7.0

    docker build -t proboci/lamp:ubuntu14.04-apache2.4-mysql5.5-php7.0 -f ubuntu/14.04/mysql/5.5/php/7.0/Dockerfile .

#### Apache 2.4, MySQL 5.5, PHP 7.1

    docker build -t proboci/lamp:ubuntu14.04-apache2.4-mysql5.5-php7.1 -f ubuntu/14.04/mysql/5.5/php/7.1/Dockerfile .

#### Apache 2.4, MySQL 5.5, PHP 7.2

    docker build -t proboci/lamp:ubuntu14.04-apache2.4-mysql5.5-php7.2 -f ubuntu/14.04/mysql/5.5/php/5.6/Dockerfile .

#### Ubuntu 16.04
Below you can find the commands to manually build the following Ubuntu 16.04 images that are compatible with Probo.CI:
  - `proboci/lamp:ubuntu16.04-apache2.4-mysql5.7-php7.0`
  - `proboci/lamp:ubuntu16.04-apache2.4-mysql5.7-php7.1`
  - `proboci/lamp:ubuntu16.04-apache2.4-mysql5.7-php7.2`

**Note:** These images will be rebuilt automatically when changes are pushed to the master branch.

##### Apache 2.4, MySQL 5.7, PHP 7.0

    docker build -t proboci/lamp:ubuntu16.04-apache2.4-mysql5.7-php7.0 -f ubuntu/16.04/mysql/5.7/php/7.0/Dockerfile .

##### Apache 2.4, MySQL 5.7, PHP 7.1

    docker build -t proboci/lamp:ubuntu16.04-apache2.4-mysql5.7-php7.1 -f ubuntu/16.04/mysql/5.7/php/7.1/Dockerfile .

##### Apache 2.4, MySQL 5.7, PHP 7.2

    docker build -t proboci/lamp:ubuntu16.04-apache2.4-mysql5.7-php7.2 -f ubuntu/16.04/mysql/5.7/php/7.2/Dockerfile .
