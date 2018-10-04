Installation Methods
====================

Docker & ffmpeg
---------------

The Alpine-based image is available on
`Docker Hub <https://hub.docker.com/r/braindoctor/clustercode/>`_.

Windows & Handbrake
-------------------

#.  Download the windows-zip from the Releases page.
#.  Extract the contents to a folder of your choice, but leave the structure
    intact.
#.  Edit ``config/clustercode.properties`` to your needs. Be aware that new
    releases ship the default config, so you need to migrate the changes in the
    files manually.
#.  Install Handbrake 1.0.x from https://handbrake.fr/
#.  Download and install the HandbrakeCLI from
    https://handbrake.fr/downloads2.php and paste it in the install directory of
    Handbrake (e.g. ``C:\Program Files\HandBrake\HandBrakeCLI.exe``)
#.  Place your video library in a
    `Priority directory <https://github.com/ccremer/clustercode/wiki/Storage-Settings#media-input-directory>`_
    in your root input directory.
#.  Download nginx for Windows from http://nginx.org/en/download.html (mainline
    version) and extract the contents in the root folder of ``clustercode``.
#.  Rename the ``nginx-1.x.x`` folder to ``nginx`` (remove the version number)
#.  Overwrite ``nginx/conf/nginx.conf`` with the one that comes in the zip file.
#.  Start clustercode by double-clicking ``clustercode.cmd``
#.  Start clustercode-admin by double-clicking ``start-clustercode-admin.cmd``

Note: To terminate, click on X of the terminal window. But also make sure that
``HandBrakeCLI.exe`` is not running in the Task Manager, it might not get
terminated properly.

You should end up with a structure like this (not all files shown)::

    clustercode
    │   clustercode.cmd
    │   clustercode.jar
    │   log4j2.xml
    │   start-clustercode-admin.cmd
    │   stop-clustercode-admin.cmd
    ├───clustercode-admin
    │   │   index.html
    │   └───static
    │       │   favicon.png
    │       │   swagger.html
    │       │   swagger.json
    │       ├───css
    │       ├───fonts
    │       ├───img
    │       └───js
    ├───config
    │       clustercode.properties
    │       log4j2-debug.xml
    │       tcp.xml
    │       udp.xml
    ├───nginx
    │   │   nginx.exe
    │   ├───conf
    │   │       nginx.conf
    │   ├───contrib
    │   ├───docs
    │   ├───html
    │   ├───logs
    │   └───temp
    ├───profiles
    │       default.ffmpeg
    │       default.handbrake
    │       x265.ffmpeg
    │       x265.handbrake
    └───tmp
            README.md
