Settings
========

Storage
^^^^^^^

Variables
*********

.. csv-table::
   :header: "Key", "Type", "Default", "Description"

    CC_MEDIA_INPUT_DIR, String, ``/input``, The root path of your input folder. See below.
    CC_MEDIA_OUTPUT_DIR, String, ``/output``, The root path of your output folder.
    CC_PROFILE_DIR, String, ``/profiles``, The root path of your profiles directory.
    CC_TRANSCODE_TEMP_DIR, String, ``/var/tmp/clustercode``, The location of the temporary files during conversion.

Media Input directory
*********************

clustercode recursively scans this folder for new video files that need
encoding. But, don't just mount your whole library here! clustercode supports
prioritized tasks, meaning that you can control the order of the conversions.
But to find out which order, you need to create **subdirectories** with the
number as priority and put your videos in here. Examples:


-   ``/input/movie1.mp4`` **will not** work!
-   ``/input/0/movie1.mp4`` **will** work.
-   ``/input/1/movie2.mkv`` will be converted before ``/input/0/movie1.mp4``

Now if you don't care about the order, just mount everything in
``/input/0/your-movie-lib-here``. But here is another example, which might make
sense.


-   ``/input/0/movies/loopdaeD.mkv``
-   ``/input/1/series/sgnikiV/season1/ep1.mp4``
