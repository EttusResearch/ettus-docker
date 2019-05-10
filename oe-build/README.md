### Using the Docker file

## Build the docker image from source

To build this container from source, navigate to this directory and run:

	$ sudo docker build -t ettusresearch/oe-build .

## Building an SD card image using the Docker container

Navigate to a location with sufficient disk space. There, create a new directory
called `oe-builder`, and make it writable by everyone. Also copy the default
user files like .bashrc:

	$ mkdir oe-builder && chmod 777 oe-builder && cp -r /etc/skel/. oe-builder

Then run the Docker container:

	$ docker run -i -t -v $(pwd)/oe-builder:/home/oe-builder:rw,z ettusresearch/oe-build /bin/bash

Note the order of the naming above might vary by Docker version, sometimes it
might need to be:

	$ docker run -i -t ettusresearch/oe-build -v $(pwd)/oe-builder:/home/oe-builder:rw,z /bin/bash

After running the previous command, you will be inside the container.
The first time you run the Docker container, you need to setup the layers.
There are various ways to do this, the easiest is to refer to a branch or tag
on the oe-manifests repository, and have it instantiate the defaults. Here, we
grab the defaults from the `sumo` branch:

	$ repo init -u git://github.com/EttusResearch/oe-manifests.git -b sumo
	$ repo sync

Recent versions of meta-ettus contain a script that will automate building a
filesystem for you. For example, if you want to build an N310 filesystem, run
the following command from your home directory:

	$ ./meta-ettus/contrib/build_imgs_package.sh n3xx v3.14.0.0

Note this will not only build the filesystem, but it will also zip it up in the
same way files are packaged for shipping with `uhd_image_downloader`.

If you prefer a more manual process, the following steps will guide you:

Before compiling, configure your build environment:

	$ TEMPLATECONF=$(pwd)/meta-ettus/<MACHINELAYER>/conf
	$ source ./oe-core/oe-init-build-env ./build ./bitbake

If you get a warning regarding chmod, you can ignore it.

This will setup the following configuration files inside the `conf` directory:

- `local.conf` - this file includes the local user configurations for the  build environment like the Distribution (`DISTRO`) and the target hardware (`MACHINE`) to be used
- `bblayers.conf`- this file includes the list of layers to be used

`<MACHINELAYER>` can be either of the following:

- `meta-neon-mender` for E320 (with Mender support)
- `meta-sulfur-mender` for N3xx series (with Mender support)

More information on the configuration options and the required layers is provided in the `meta-ettus/README` file ([the latest version is available here](https://github.com/EttusResearch/meta-ettus)). It lists the supported values for `MACHINE` which look something like this:

    MACHINE="ni-sulfur-rev6-mender" # for N3xx
    MACHINE="ni-neon-rev2-mender" # for E320


Then, you can invoke bitbake to build the image:

	$ bitbake developer-image

To build the according SDK, run

	$ bitbake developer-image -cpopulate_sdk

Note that "developer-image" is the name of the build target.