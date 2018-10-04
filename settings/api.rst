API Settings
------------

Variables
^^^^^^^^^

.. csv-table::
   :header: "Key", "Type", "Default", "Description"

    CC_REST_API_ENABLED, Boolean, ``true``, (De)Activate the REST API.
    CC_REST_API_PORT, Integer, ``7700``, The port to connect to the internal API.

Enabling the REST API
^^^^^^^^^^^^^^^^^^^^^

If you enable the API, you can navigate to ``your.host:8080/static/swagger.html``
and retrieve the API documentation. You can also get ``/static/swagger.json``
and upload it to Swagger to generate a client in your preferred programming
language. It's quite awesome!

Some resources:

*   ``/api/v1/status``: Retrieve the status of the current node.
*   ``/api/v1/tasks``: Get a list of tasks that are in the cluster.
*   ``/api/v1/progress``: Get the percentage of the task done.
*   ``/api/v1/progress/ffmpeg``: Get a more detailed report of the most recent
    ffmpeg output.

Only change the api port if you reconfigure nginx (or any other proxy server) as
well.
