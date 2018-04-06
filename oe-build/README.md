### Using the Docker file

## Build the docker image from source

To build this container from source, navigate to this directory and run:

    $ sudo docker build -t ettusresearch/oe-build .

## Building an SD card image using the Docker container

Navigate to a location with sufficient disk space. There, create a new directory
called 'build' and make it writable by everyone:

   $ mkdir build && chmod 777 build

Then run the Docker container:

    $ docker run -i -t -v $(pwd)/build:/home/oe-builder/build:rw,z ettusresearch/oe-build /bin/bash

Note the order of the naming above might vary by docker version, sometimes it might need to be:

    $ docker run -i -t ettusresearch/oe-build -v $(pwd)/build:/home/oe-builder/build:rw,z /bin/bash

After running the previous command, you will be inside the container. First,
configure your build environment:

    $ TEMPLATECONF=`pwd`/meta-ettus/conf/sulfur source ./oe-core/oe-init-build-env ./build ./bitbake

Then, you can invoke bitbake to build the image:

    $ bitbake

