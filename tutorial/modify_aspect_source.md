
# Modify ASPECT Source Code

This tutorial provides a demonstration of how to modify the ASPECT (version 3.0.0) source code and subsequently use that modified ASPECT installation for in a conatiner. In this case, let "mod-aspect-3.0.0" indicate the name of the directory that contains the modified version of the source code. It's useful to use descriptive names for specific changes.

NOTE: Make sure that this tutorial is completed within a compute session on a compute node.

Navigate to the `aspect-pipeline/src` directory and create a new copy of the ASPECT source code. Assuming `aspect-pipeline/src/aspect-3.0.0.tar.gz` is already downloaded to this directory, simply run the following to create the new source code tree to be modified:

```bash
cd aspect-pipeline/src
SOURCE_CODE_VARIANT="mod-aspect-3.0.0"
tar -xzf aspect-3.0.0.tar.gz -C ${SOURCE_CODE_VARIANT} --strip-components=1
```

Now a pristine copy of the aspect-3.0.0 source code is available in a directory "mod-aspect-3.0.0" where the source code modification can be made.

For this case, the `shear_bands_modified.cc` file in the `aspect-pipeline/tutorial` directory represents the desired source code change. The goal is to replace the `mod-aspect-3.0.0/benchmarks/shear_bands/shear_bands.cc` file with this updated file. Now make this switch, but also keep copies of the modified and original versions as well for posterity.

```bash
cp ../tutorial/shear_bands_modified.cc ${SOURCE_CODE_VARIANT}/benchmarks/shear_bands/
cp ${SOURCE_CODE_VARIANT}/benchmarks/shear_bands/shear_bands.cc ${SOURCE_CODE_VARIANT}/benchmarks/shear_bands/shear_bands_original.cc
cp ${SOURCE_CODE_VARIANT}/benchmarks/shear_bands/shear_bands_modified.cc ${SOURCE_CODE_VARIANT}/benchmarks/shear_bands/shear_bands.cc
```

For more information on writing your plug-ins, read the [How to write a plugin](https://aspect-documentation.readthedocs.io/en/latest/user/extending/write-a-plugin.html) section of the ASPECT documentation.

For this modification to take effect, ASPECT must now be built within a container using this updated source code. Redirect the `aspect` link in `aspect-pipeline/src` to point to the modified source code. This `aspect` link dictates the source code to be used during the build process.

```bash
rm aspect
ln -s mod-aspect-3.0.0 aspect
```

Navigate to the `aspect-pipeline/build` directory and copy the `build_additive-aspect.submit` script, renaming it according to your new custom variant.

```bash
cd ../build
cp build_additive-aspect.submit build_mod-aspect.submit
```

Edit the new build script (`build_mod-aspect.submit`) simply by adding a `TAG` (changing the line `TAG=""` instead to `TAG="-mod"`) to indicate that this container build contains the modified ASPECT build. Then launch the job to build the container.

```bash
sbatch build_mod-aspect.submit
```

