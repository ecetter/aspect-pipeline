# ASPECT Pipeline

This repository provides a container-based workflow for [ASPECT](https://aspect.geodynamics.org), enabling source code customization, large-scale parallel builds, and reproducible execution on HPC systems using Slurm, specifically [ICDS@PSU's](https://icds.psu.edu) [Roar Collab cluster](https://icds.psu.edu/services/roar/roar-collab-cluster). The workflow is built around Apptainer/Singularity containers and is designed to combine developer flexibility with portability, performance, and reproducibility.

The core idea is:

Download and edit ASPECT source code -> build containers with automated jobs -> run and benchmark simulations reproducibly

---

## Key Features

- Containerized ASPECT builds using Apptainer
- Direct ASPECT source code customization
- Automated and parallelized container builds for HPC systems
- Portable and reproducible execution environments
- Dedicated workspace for simulations and experiments
- Automated testing and benchmarking using ASPECT cookbooks
- Clean separation of concerns between build, runtime, and testing


## Repository Structure

    .
    ├── build/
    │   └── Submission scripts to configure and build containers
    │
    ├── containers/
    │   └── Built Apptainer container images (.sif or .sb)
    │
    ├── definitions/
    │   └── Apptainer definition (.def) files
    │
    ├── run/
    │   └── User workspace for simulations and projects
    │
    ├── src/
    │   └── ASPECT source code used during container builds
    │
    ├── test/
    │   ├── Link to ASPECT cookbooks
    │   └── Submission scripts for automated tests and benchmarks
    │
    └── README.md


## Workflow Overview

1. **Download and Modify ASPECT Source Code**
   Users download ASPECT source code automatically using the provided `src/download_aspect` script.
   Users edit or extend ASPECT directly in the `src/` directory.
   These changes are automatically incorporated during the container build process.
   Multiple source code variants may be saved at any time, and the `aspect` soft link dictates the source code used during the build process.

2. **Build Containers**
   Submission scripts in the `build/` directory configure and build Apptainer containers.  
   Builds are designed to run efficiently on HPC systems and can be parallelized.  
   Resulting container images are stored in `containers/`.

3. **Test and Benchmark**  
   The `test/` directory links to ASPECT cookbooks.  
   Automated submission scripts run selected cookbook examples.  
   These tests are used for validation and benchmarking, including parallel scaling studies.

4. **Run Simulations**
   The `run/` directory serves as the primary user-facing workspace.  
   Users set up projects, input files, and output directories here.  
   Containers are executed from this location to perform simulations.


## System Requirements

The host system must have Apptainer/Singularity installed.

The host system must utilize Slurm as the job scheduler for build and test scripts. The build and test scripts are configured to run as-is on Roar Collab (RC), but the resource directives and script configuration blocks near the top of each script can be easily altered to run on other HPC systems.

To enable efficient parallelization, Intel HPC Toolkit version 2025.3 must be installed on the host system. The key reason for this requirement is that the MPI version within the container must be compatible with, or more simply must match, the version of MPI on the host. This pipeline is specifically configured to use Intel HPC Toolkit version 2025.3 since this is the MPI on the host and the initial container bootstrap utilizes this MPI version. If the host MPI differs, then the container bootstrapping and definition files can be reasonably reconfigured to match the host MPI instead.


## Directory Details

### src/

Contains the ASPECT source code used during automated container builds.

- Modify existing ASPECT components
- Add custom physics, material models, or solvers
- Experiment with new or experimental features

Any changes in this directory are automatically picked up during container builds as dictated by the `aspect` soft link.


### definitions/

Holds Apptainer definition files that define the container environment.

- Specifies the base operating system
- Defines compilers, MPI libraries, and dependencies
- Captures the ASPECT build configuration

This directory is the authoritative description of the runtime environment.


### build/

Contains HPC submission scripts for building containers.

- Intended to be run through a scheduler (Slurm) as a job
- Supports parallel container builds
- Enables automated rebuilds after source code changes

Build logic is intentionally separated from user workflows.


### containers/

Stores the built Apptainer container images.

- Portable across systems
- Suitable for large-scale parallel execution
- Reproducible representations of a given ASPECT source state


### test/

Automated testing and benchmarking environment.

- Includes a link to ASPECT cookbooks
- Provides submission scripts for running cookbook examples

Used to validate container builds and measure parallel performance and scaling.


### run/

Primary user workspace for simulations and experiments.

- Create project directories
- Manage input files and outputs
- Treat as a scratch or project space

This directory is independent of the container build infrastructure.


## Why This Workflow?

This pipeline combines the flexibility of source-based ASPECT development with the convenience, portability, and reproducibility of containers:

- Eliminates dependency drift across systems
- Simplifies collaboration and sharing
- Enables rapid iteration on ASPECT source code
- Scales from small test runs to leadership-class HPC systems


## Getting Started (High-Level)

1. Download, modify, and link ASPECT source code in `src/`
2. Submit container build job from `build/`
3. Execute automated benchmarks from `test/`
4. Set up and run simulations in `run/`


## Intended Audience

- ASPECT developers and contributors
- Geodynamics and geothermal researchers
- HPC users requiring reproducible workflows
- Research teams collaborating across multiple systems


## License and Attribution

ASPECT is developed by the ASPECT community.  
This repository provides a container-based workflow and does not replace or redistribute ASPECT licensing terms.

Please cite ASPECT appropriately when using this workflow in publications.


## Contributing

Contributions are welcome. 
Please submit issues or pull requests to improve portability, performance, or usability.
If this project receives interest to be configured for other HPC systems, a more streamline configuration process can be implemented in future versions.

