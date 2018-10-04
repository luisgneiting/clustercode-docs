Cluster Settings
----------------

Variables
^^^^^^^^^

.. csv-table::
   :header: "Key", "Type", "Default", "Description"

    CC_CLUSTER_NAME, String, ``clustercode``, The cluster name.
    CC_CLUSTER_JGROUPS_CONFIG, String, ``config/tcp.xml``, The path to the JGroups config file.
    CC_CLUSTER_JGROUPS_PREFER_IPV4, Boolean, ``true``, Whether IPv4 is preferred. Otherwise IPv6 is used.
    CC_CLUSTER_JGROUPS_BIND_PORT, Integer, ``7600``, The port to bind JGroups
    CC_CLUSTER_JGROUPS_BIND_ADDR, String, ``SITE_LOCAL``, The IP address to bind JGroups.
    CC_CLUSTER_JGROUPS_EXT_ADDR, String, ``-``, The address under which other nodes can find the local one.
    CC_CLUSTER_JGROUPS_TCP_INITIAL_HOSTS, String, ``localhost[7600]``, The initial hosts to check for an existing cluster.
    CC_CLUSTER_JGROUPS_HOSTNAME, String, <empty>, Override the hostname for the cluster.
    CC_CLUSTER_TASK_TIMEOUT, Integer, ``24``, The amount of hours to remove tasks from the cluster.
    CC_ARBITER_NODE, Boolean, ``false``, If the local node should run as an arbiter node.

JGroups
^^^^^^^

The cluster exchanges information and tasks via the
`JGroups <http://jgroups.org/>`_ framework. The whole networking depends on it.
The above config works with non-Swarm mode in Docker. If you have Swarm that is
capable of multicasting then you might need to read more on JGroups either
`here <http://jgroups.org/manual4/index.html>`_ and/or
`here <https://github.com/belaban/jgroups-docker#overview>`_ .

Initial Hosts
^^^^^^^^^^^^^

When clustercode starts, it tries to contact this list of hosts and connect to
them. You can specify multiple IPv4, IPv6 or DNS hostnames in the form of
``node-1[7600],node-2[7600]`` in ``CC_CLUSTER_JGROUPS_TCP_INITIAL_HOSTS``. Don't
specify localhost anymore.

External Address
^^^^^^^^^^^^^^^^

When JGroups creates a socket in Docker, it will be on ``eth0``. ``eth0`` is a
virtual NIC inside the container. So other cluster nodes cannot connect to this
virtual address. Instead, they need to contact the physical machine on the
specified port. Docker then routes the traffic to the correct container. So if
your Docker node is reachable on IP ``10.10.10.45``, you need to set
``CC_CLUSTER_JGROUPS_EXT_ADDR`` to ``10.10.10.45``. This may or may not be
needed in Docker Swarm mode, this is not tested.

Arbiter Node
^^^^^^^^^^^^

If set to ``true``, the local node will not convert any video files whatsoever,
but rather partake in the cluster communication. If you have two nodes and there
happens to be a split-brain, it is hard to recover from it. If you have a 3rd
node but it's not powerful enough for encoding then you can still add it to
cluster in arbiter mode. With three cluster members, a split-brain can be
prevented if one node is down. Use this to provide a quorum.
