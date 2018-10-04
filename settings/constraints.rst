Constraints
-----------

You can control which media files will get accepted and which not.

Variables
^^^^^^^^^

.. csv-table::
   :header: "Key", "Type", "Default", "Description"

    CC_CONSTRAINTS_ACTIVE, Unordered Enum, ``FILE_SIZE``, List of enabled constraints.
    CC_CONSTRAINT_FILE_MIN_SIZE, Integer, ``150``, Do not include video files smaller than this value in MB.
    CC_CONSTRAINT_FILE_MAX_SIZE, Integer, ``0``, Do not include video files larger than this value in MB.
    CC_CONSTRAINT_TIME_BEGIN, String, ``08:00``, Do not start converting video files before this time of day.
    CC_CONSTRAINT_TIME_STOP, String, ``16:00``, Do not start converting video files after this time of day.
    CC_CONSTRAINT_FILE_REGEX, String, <empty>, Do not include video files matching this regular expression.

Enabling Constraints
^^^^^^^^^^^^^^^^^^^^

Specify a space-separated list of constraints that should be active during media
scans. If you specify ``ALL``, all constraints will be used. Use ``NONE`` to not
enable any constraints at all. If one of the constraints does not accept the
video file, it will be skipped.

File Size Constraint
""""""""""""""""""""

The ``FILE_SIZE`` constraint can be used to filter out small samples or videos
that are not necessary to convert to a better codec. Be sure to set positive
numbers. To disable, do not specify ``FILE_SIZE`` in ``CC_CONSTRAINTS_ACTIVE``.

Time Constraint
"""""""""""""""

This ``TIME`` constraint can be used to disable conversion e.g. during
nighttime. So if you specify a stop time of ``16:00``, a video file will still
begin encoding at ``15.59``. The encoding process will not get cancelled, even
after ``16:00``. If you like to stop encoding at ``20:00``, you need to figure
out the average encoding time of your videos and subtract it from ``20:00``. For
example if encoding takes 1.5 hours, specify ``18:30``. Bear in mind that the
times are localized in your time zone, but in Docker that might still be UTC. If
you only want to encode during nighttime, switch the start and stop times.

File Name Constraint
""""""""""""""""""""

You can skip video files that match a specific regular expression with the
``FILE_NAME`` constraint. Test your regex in an online tool, such as
https://regexr.com
