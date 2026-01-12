
# run

Use this directory for your projects.

The `launch_sb_shell` script simplifies launching a sandbox container in writable mode. Simply run the following:

    $ ./launch_sb_shell ../containers/<mycontainer.sb>

Note that this only works for sandbox containers since images (.sif) cannot be opened in writable mode. To launch a shell in normal mode, use

    $ apptainer shell ../containers/<mycontainer.sif>

