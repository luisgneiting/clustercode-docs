Concept
-------

Clustercode 2.0 is a major overhaul since version 1. While the functionality is
mostly the same, version 1 suffers from several issues. It is not possible to
parallelize tasks as one might expect. You only benefit from having multiple
media files in the queue, otherwise you might as well run a single node.

Version 2 tries overcome this limitation by using Ffmpeg's segmentation feature.
The main deployment type is now Docker Swarm and Kubernetes. Of course it still
runs on a single Docker host, but it is now easier to scale across several
nodes.

Component: Master
^^^^^^^^^^^^^^^^^

In clustercode 1.x, the clustering was done by scaling the master node itself.
It relied on JGroups as the messaging framework. Properly configured, each
master was independent enough to scan, transcode and cleanup a media file. They
only shared the current state so that no two nodes would do the same.

In 2.x, this is no longer the case. There is now only one master intendet. It
handles the scanning of media files, parsing profiles and cleanup (for now).
Transcoding is handed off to the worker components.

Component: Compute Worker
^^^^^^^^^^^^^^^^^^^^^^^^^

Roles
^^^^^

There are 2 roles of the worker artifact: compute and shovel.

* Compute: A compute node is designated for ffmpeg transcoding. Nodes with high
  CPU capacities are beneficial for compute workers as they directly influence
  the time required for processing media files. The more compute nodes there
  are, the faster is the processing time.
* Shovel: A shovel node is mainly tasked with moving files around. It is there
  to free compute nodes from IO heavy jobs, such as splitting and merging the
  media files. This enables compute nodes to continue on other media files.
  Usually 1 shovel node is enough for most environments.

It is usually enough to deploy 1 shovel and as many compute workers as you have
nodes.

Component: Frontend
^^^^^^^^^^^^^^^^^^^

The frontend is an nginx server which contains the static assets. The data
requests are proxied to the master.

Component: Messaging
^^^^^^^^^^^^^^^^^^^^

RabbitMQ is used as the message broker that glues together the master and worker
nodes.

Component: Database
^^^^^^^^^^^^^^^^^^^

Processed jobs are stored in CouchDB. This allows a historical view of the jobs
including the output of the ffmpeg process. Through the API, users may freely
requeue the jobs or delete existing ones.
