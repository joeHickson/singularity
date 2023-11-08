This is a sandbox for developing containers to run cylc/rose on different bases. Containers are ultimately going to be used in Singularity but some are pure docker as interim / pre-build steps.

Useful dev commands:

Build, launch and enter test docker container:
docker build -t test/rhel:0.1 .
docker run -dit -l rhel --name rhel test/rhel:0.1
docker exec -it rhel bin/bash


Building singularity builds using docker container version of singularity:
docker run -dit --privileged -l singularity --name singularity -v /workspaces/singularity/rhel:/mnt/buildfiles quay.io/singularity/singularity:v4.0.1 build /mnt/buildfiles/cylc_rhel.sif /mnt/buildfiles/cylc_rhel.def

loop for retrying above (monitor `htop`` or `docker ps` for job completion, re-run `docker logs singularity` to check job output):
docker container rm singularity && docker run -dit -l singularity --privileged --name singularity -v /workspaces/singularity/rhel:/mnt/buildfiles quay.io/singularity/singularity:v4.0.1 build /mnt/buildfiles/cylc_rhel.sif /mnt/buildfiles/cylc_rhel.def && docker logs singularity