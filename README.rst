################################
pl-tensorflowapp-sample
################################


Abstract
********

| Sample Tensorflow application plugin for ChRIS Project.
| The application is a digit-identification application based on MNIST data.
| The intent is to make it easy for future users to take this sample and use it as the starting point to build your own plugin.

Build
*****

.. note::
  Make sure you have 's2i' installed where you are building.

On a Fedora system the command would be:

.. code-block:: bash

  sudo dnf install s2i

Build The Base Container:
=========================

From the root directory of the git repository, run the following command to build the container:

.. code-block:: bash

    make build

The default image built will be named:

``pl-tensorflowapp-sample-centos-python3``

This default container image is a CentOS image that includes Python3.

.. note::
  See 'Makefile' if you'd like to use a different IMAGE_NAME



S2I Build & Training
====================

Run the s2i command below to train the model and build the sample application plugin

``s2i build <source-location> <builder-image-name> <output-image-name>``

Example Command: (Run from root of project repo)

.. code-block:: bash

  s2i build . pl-tensorflowapp-sample-centos-python3 tensorflowapp-sample-centos

If you'd like see additional information when building, append the --loglevel <loglevel_value>

.. code-block:: bash

  s2i build . pl-tensorflowapp-sample-centos-python3 tensorflowapp-sample-centos --loglevel 5


The output of the above command is a container named:
``tensorflowapp-sample-centos``

Run
***

Using ``docker run``
====================

Start Docker Service

.. code-block:: bash

    sudo systemctl start docker


Start Docker daemon at boot (Optional)

.. code-block:: bash

    sudo systemctl enable docker


Make sure your user is in the docker group if you want to run the docker command as a non-root user

.. code-block:: bash

    sudo groupadd docker && sudo gpasswd -a ${USER} docker && sudo systemctl restart docker
    newgrp docker


Run our MNIST model and make an inference
=========================================

.. code-block:: bash

  docker run tensorflowapp-sample-centos

If everything went well, you should see output similar to this:

::

  Inference Test:
   Inference value of test Image is :  1
   Creating new file... /opt/app-root/src/output/mnist-inference


Debugging
=========

If you'd like to load the container in interactive mode and poke around via bash use the follow command:

.. code-block:: bash

  docker run -it tensorflowapp-sample-centos /bin/bash
