
# definitions

This directory contains definition files that are to be used to build containers.

Using the `bootstrap` feature, containers can be created in a stackable manner.

In this case, for instance, creating a container with deal.II and other ASPECT dependencies installed takes a significantly longer time than just installing ASPECT after the dependencies are in place. For this reason, it is more efficient, practically, to break this build process down by first building a `base-dealii` container which contains all ASPECT dependencies (including a compatible MPI and also deal.II) which can then be bootstrapped to create an `additive-aspect` container which layers ASPECT in an additive manner on top of the `base-dealii` container. The `base-dealii` container can be leveraged to build new ASPECT containers rapidly and iteritively.

