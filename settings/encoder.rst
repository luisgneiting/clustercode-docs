Encoder Settings
----------------

Variables
^^^^^^^^^

.. csv-table::
   :header: "Key", "Type", "Default", "Description"

    CC_TRANSCODE_CLI, String, ``/usr/bin/ffmpeg``, The path to the encoder binary.
    CC_TRANSCODE_IO_REDIRECTED, Boolean, ``false``, Redirects encoder output to the java process (combined output).
    CC_TRANSCODE_DEFAULT_FORMAT, String, ``.mkv``, The default container file format if not overridden by a profile.
    CC_TRANSCODE_TYPE, Enum, ``FFMPEG``, "The type of encoder."

CLI and encoder type
^^^^^^^^^^^^^^^^^^^^

Following are supported:

-   ``FFMPEG``
-   ``HANDBRAKE``

With ``CC_TRANSCODE_CLI`` you can set the path, but ``CC_TRANSCODE_TYPE``
determines the expected encoder type and is needed for parsing progress data.

IO Redirection
^^^^^^^^^^^^^^

When ffmpeg is invoked, by default the output is not displayed in the console of
the clustercode process. You can enable it to debug problems with a profile or
video file, however, the REST API will not be able to get the progress
information.

Default file format
^^^^^^^^^^^^^^^^^^^

This is the file extension of converted files. Be aware that you need to specify
the container format in the profiles.
