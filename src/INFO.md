
# src

This directory contains the ASPECT source code to be used in the builds.

The `aspect` link dictates the specific version to be used in the build, and this link can be adjusted on-the-fly as necessary. Using this linking methods enables the user to efficiently manage multiple ASPECT source code variations, whether they are different versions or source code modifications of the same version.

The `download_aspect` script simplifies the download process for the user. The user can also provide the script with the `link` argument to specifically link that source code version:

    $ ./download_aspect         # to download and extract ASPECT source code
    $ ./download_aspect link    # to download, extract, and link ASPECT source code

The specific ASPECT version to be downloaded is determined by the `ASPECT_VERSION` in the `download_aspect` script and can be modified as needed.

The ASPECT source code and tar files are not tracked by git to facilitate clean builds for new repo instances.

