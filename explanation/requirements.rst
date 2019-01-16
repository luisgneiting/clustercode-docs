Requirements
------------

This is loosely summary of the requirements that clustercode should fulfill.

Parallelizaion
^^^^^^^^^^^^^^

Transcoding video files is usually a very CPU intensive task. Clustercode should
be able to speed up encoding tasks by scaling horizontaly.

To achieve this, it uses Ffmpeg to divide a file into chunks and creating
so-called task slices. Those slices will be picked up by a compute worker node.

When task slices have been completed, we can use ffmpeg again to merge the
chunks back into the resulting file.

Scaling
^^^^^^^

Compute nodes should be able to scale up at any time, without configuration of
the peer instances. It should be as easy as
`kubectl scale --replicas=5 deployment/clustercode-compute`. New instances
should aumatically pick up one of the remaining task slices and transcode it.

History
^^^^^^^

Clustercode should maintain a history of already transcoded files, so that they
do not get rescheduled for processing. The 1.x approach for having a *.done file
in the input pollutes the file system. On the other hand, it was very easy for
users to re-schedule media files by just deleting the magic file.

The new approach is to use a database in which the completed jobs are stored.
An API and a front enable the deletion of jobs.

Installation
^^^^^^^^^^^^

Clustercode is intended to be run on Docker. If it runs on Docker, it should
also run on Kubernetes. Moreover, it should support various hardware, mainly
x86 and ARM architecture. This would allow users to run a Raspberry Pi cluster
if needed.

Fault-Tolerance
^^^^^^^^^^^^^^^

In Kubernetes world, one has to expect that any instance might get terminated at
any random time. A node might get rebooted, pods evicted and moved, a storage
node might loose network connectivity. Clustercode needs to cope with these
unreliabilities, up to a certain degree.

Should any worker instance fail for some reason or terminated, the task slice
will be re-queued, so that another instance might still do it. A fail might be
caused by broken storage mounts due to network hiccups.

The master keeps track of which tasks have been added to the queue. Since tasks
are split into slices, the messaging component is responsible for keeping track
which worker received which messages and acknowledged them. The master is not
required to validate whether the messaging component is working correctly or
mitigate certain race conditions. Though the system as a whole should be able to
cold start from nothing.

Integrity
^^^^^^^^^

By using microservices, NoSQL database and filesystem, the system is not exactly
ACID compliant. Meaning there is a chance that a job could be marked as
completed in the database, but it actually failed to write to disk. For now,
this is acceptable as long as it is reasonably easy to delete the job and re-
queue it. The only loss would be time.

Another case is the opposite: The encoded result has been saved to disk, but the
database is not reachable. The worst that can happen is a re-queue, overwriting
the already existing file. Which should yield the same result, but it is just
a loss of time.

Usability
^^^^^^^^^

There are two entrypoints to operate clustercode from the user perspective. The
first being the filesystem, the user needs to respect some rules about
structuring the input and output directories. The file scanner in clustercode
will try to figure out which settings need to be applied when encoding the media
files. Specifying these settings should be as flexible as possible, where the
user can freely edit them without relying on clustercode to provide these.

The second entrypoint is the frontend. In clustercode 1.x it was possible to
view multiple tasks in a single session, if multiple nodes were processing
files. In 2.x, because of the parallelization there is now only 1 task active,
divided into several slices. The current task can be cancelled, but more
features are not yet planned.

Performance
^^^^^^^^^^^

There are no special requirements to performance to the internals of the
software. The bottleneck is always ffmpeg itself (heavy Disk and CPU loads).

As for the frontend, there are not thoudsands of requests expected from users.
So there is no need to setup a cluster of masters and frontends for the sake of
handling load.

Security
^^^^^^^^

As mentioned before, security requirements are almost none. Because of the
flexibility it is possible to supply 'bad' parameters to ffmpeg. There may be
more vulnerabilities (database, messaging, ...), so use Clustercode at your own
risk.
