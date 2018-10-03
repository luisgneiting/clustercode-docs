.. Clustercode documentation master file, created by
   sphinx-quickstart on Wed Oct  3 08:23:00 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to Clustercode's documentation!
=======================================

.. image:: _static/webui.png

.. toctree::
   :maxdepth: 2
   :caption: Contents:
   :numbered:
   :hidden:

   installation
   netdata
   settings/storage.rst

Overview
========

Let's look at the graphic below. It shows two nodes and how they are working
together on automatically pickup new tasks and make sure that they are the only
ones processing it.

.. image:: _static/overview.png


The procedure in detail
^^^^^^^^^^^^^^^^^^^^^^^

#.  Start up the node and connect to a cluster. If the node is the first in the
    cluster, it will become master. Though that doesn't affect media selection
    or encoding.

#.  Scan the ``input storage``. In this directory are the media sources located
    in a special pattern. More on that later.

#.  One of:

    a)  Select a media file. If the media is already processed, it won't be
        picked again.
    b)  Wait a few minutes in case no media has been found and go back to step
        ``2``.

#.  Check if any constraints would prevent this media selection.

#.  Select a profile that matches the media. A profile contains the information
    on how to encode the media file. Basically they are ``ffmpeg`` options.

#.  Ask the cluster if the selected media is already being converted. It it is,
    go back to step ``3a``.

#.  Encode the selected media using ``ffmpeg`` in a temporary folder.

#.  Perform a cleanup according to the selected cleanup strategy. Move the file
    to the output storage.

#.  Go back to step ``2``.

The state machine of a node should help understanding it.

.. image:: _static/statemachine.png

