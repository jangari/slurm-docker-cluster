# Intersect Slurm Container

Aidan Wilson <aidan.wilson@intersect.org.au>
2025-03-29

## Background

Built on a [Docker environment by giovtorres](https://github.com/giovtorres/slurm-docker-cluster.git) that has been tweaked for Intersect's requirements for delivering training courses in HPC using Slurm. Changes from that repo are as follows:

- Adds a login node (from [whophil](https://github.com/whophil/slurm-docker-cluster/commit/c45c2647137c5e4cd6cc077d4c3c7353b88bf260))
- Exposes port 22 allowing users to SSH directly into the cluster (for this to work, the host machine's sshd port must be changed)
- Installs various packages, such as `environment-modules`, `rsync`, `mailx`, `nano`, and so on, to emulate a training environment
- A modulefiles volume that is bind mounted from the host
- A shared `/home` volume so that all nodes can access it
- timezone synchronisation across all nodes from the host
- A scratch space that is bind mounted from the host
- Expands to 8 compute nodes
- a `mailrc` that points to the nectar relay

## Usage

- `docker-compose build` to build all images required for the cluster
- `docker-compose up -d` to start the cluster
- `docker-compose down -v` to stop the cluster and destroy the volumes (important to remove old home dirs to restart with new courses)

If any changes are made to the `Dockerfile`, the `Dockerfile-login`, or the `docker-entrypoint.sh` script, you will need to run `docker-compose build` to re-build the image.

As with giovtorres' repo, live changes to the `slurm.conf` or `slurmdbd.conf` files can be applied using the `update_slurmfiles.sh` script.

A separate repo, `sacctmgr_docker`, is used to create and manage system users, slurm accounts and associations.


