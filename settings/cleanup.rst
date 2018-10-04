Cleanup Settings
----------------

Variables
^^^^^^^^^

.. csv-table::
   :header: "Key", "Type", "Default", "Description"

    CC_CLEANUP_STRATEGY, Ordered Enum, ``STRUCTURED_OUTPUT MARK_SOURCE CHOWN``,	The enabled cleanup strategies.
    CC_CLEANUP_OVERWRITE, Boolean, ``true``, Whether to overwrite existing files in the /output dir.
    CC_CLEANUP_CHOWN_GROUPID, Integer, ``0``, The group id when changing file owner (Linux).
    CC_CLEANUP_CHOWN_USERID, Integer, ``0``, The user id when changing file owner (Linux).
    CC_CLEANUP_MARK_SOURCE_DIR,	String, ``/input/done``, The directory in which the done files will be created.

Enabled Cleanup Strategies
^^^^^^^^^^^^^^^^^^^^^^^^^^

Specify a space-separated list of strategies that should be applied after
encoding. You should specify at least ``MARK_SOURC`` and ``UNIFIED_OUTPUT``,
otherwise clustercode will not behave as you'd expect.

Overwrite
^^^^^^^^^

This flag controls whether existing files should be overwritten. If set to
``false``, a timestamp in the form of ``yyyy-MM-dd.HH-mm-ss`` will be added
before the file extension.

Cleanup Strategies
^^^^^^^^^^^^^^^^^^

Unified Output Directory
""""""""""""""""""""""""

Specify ``UNIFIED_OUTPUT`` to put all converted files into ``/output`` without
any subdirectories.

Structured Output Directory
"""""""""""""""""""""""""""

Specify ``STRUCTURED_OUTPUT`` to recreate the input directory tree on the output
root folder. Examples:

-   ``/input/0/movie1.mp4`` -> /output/movie.mp4``
-   ``/input/1/movies/movie2.mkv -> ``/output/movies/movie2.mkv``
-   ``/input/1/movies/action/movie3.avi -> ``/output/movies/action/movie3.mp4``

The file extensions will be according to the profile.

Mark Source
"""""""""""

Specify ``MARK_SOURCE`` to create the dummy file in the input folder next to the
source file. If this file exists, clustercode knows that the media file has
already been processed and can be skipped in the next scan. Examples:

-   For ``/input/0/movie1.mp4`` a file named ``/input/0/movie1.mp4.done`` will
    be created.
-   For ``/input/0/series/sgnikiV/ep1.mp4`` a file named
    ``/input/0/series/sgnikiV/ep1.mp4.done`` will be created.

Mark Source Dir
"""""""""""""""

Specify ``MARK_SOURCE_DIR`` to create a dummy file similar to "Mark Source", but
in its own directory specified by setting key ``CC_CLEANUP_MARK_SOURCE_DIR``.
This strategy will leave your input folders as they are. You could then mount
the input folders as read-only, provided you change the default value for
``CC_CLEANUP_MARK_SOURCE_DIR`` (which is ``/input/done``). Examples:

-   For ``/input/0/movie1.mp4`` a file named ``/input/done/0/movie1.mp4.done``
    will be created.
-   For ``/input/0/series/sgnikiV/ep1.mp4`` a file named
    ``/input/done/0/series/sgnikiV/ep1.mp4.done`` will be created.

Delete Source
"""""""""""""

Specify ``DELETE_SOURCE`` as a strategy to delete the source files in the input
directory. Obviously clustercode cannot find the file on the next scan and will
thus not be processed a second time.

.. warning:: Only use this option if you trust your ffmpeg profile or you have a
    backup.

Chown
"""""

.. warning:: This strategy only works with Linux under root user!

Specify ``CHOWN`` to change owner of the output file. The file will be changed
with ``chown CC_CLEANUP_CHOWN_USERID:CC_CLEANUP_CHOWN_GROUPID``.
