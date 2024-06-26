
> [!IMPORTANT]
> On June 26 2024, Linux Foundation announced the merger of its financial services umbrella, the Fintech Open Source Foundation ([FINOS](https://finos.org)), with OS-Climate, an open source community dedicated to building data technologies, modeling, and analytic tools that will drive global capital flows into climate change mitigation and resilience; OS-Climate projects are in the process of transitioning to the [FINOS governance framework](https://community.finos.org/docs/governance); read more on [finos.org/press/finos-join-forces-os-open-source-climate-sustainability-esg](https://finos.org/press/finos-join-forces-os-open-source-climate-sustainability-esg)

# osc-dm-mesh-srv - Ecosystem Platform Execution Environment

osc-dm-mesh-srv is the execution environment for Broda Group
Software's Ecosystem Platform, and is responsible for starting all
components in a Docker Compose run-time environment.

Full documentation is available in in the
[osc-dm-mesh-doc](https://github.com/brodagroupsoftware/osc-dm-mesh-doc)
repo.

This application interacts with other applications. You can run
the full set of applications by following instructions in the
[osc-dm-mesh-doc](https://github.com/brodagroupsoftware/osc-dm-mesh-doc)
repo.

The remaining sections explain how to Dockerize the application
as well as providing a few developer notes.

## Prerequisites

Docker and Docker Compose must be available.

All Docker images must be available locally or in an
accessible repository.

## Setting up your Environment

Some environment variables are used by various coded and scripts.
Setup your environment as follows (note that "source" is used)
~~~~
source ./bin/environment.sh
~~~~

## Ecosystem Platform Configuration

Our data mesh is composed of several components:
- Core services, including a "Ecosystem Platform Registry" service
and a user experience/interface (UX) to query the Ecosystem Platform Registry
- A command line interface (CLI) that supports publishers (those
that create and register data products), subscribers
- Data Products, including a sample called "GISS Temperature" and
and a user experience/interface (UX) to visualize elements of
the GISS Temperature data

EXTERNAL/INTERNAL ports:
- Marketplace UX:  25000/3000
- Proxy server: 20000/8000
- Registrar:  21000/8000
- Monitor:  21000/8000
- Search: 23000/8000
- Geospatial Index: 30000/3000
- ETCD: 2379/2379

Data product ports are defined by their instance,
starting with 24000, increnting by 1, for example:
- Data Product - RMI: 24000/3000
- Data Product - PUDL: 24001/8000


## Running the Ecosystem Platform

The following command will start all Ecosystem Platform components:
~~~~
$PROJECT_DIR/app/startd.sh
~~~~

The following docker engvironment will be started:
- Registry service: Ports (external/internal): 25000/8000
- LLM service: Ports (external/internal): 26000/8001
- Agent service(s): Ports (external/internal): 27000/8002
- ETCD services: Ports (external/internal): 30000-2/2379
- SQLlite services: Ports (external/internal): 31000/2379
~~~~
docker ps

CONTAINER ID   IMAGE                                         COMMAND                  CREATED          STATUS          PORTS                               NAMES
8459f2167afa   brodagroupsoftware/bgssrv-llm:latest          "python3 /app/bgssrv…"   12 seconds ago   Up 10 seconds   0.0.0.0:26000->8001/tcp             docker-bgssrv-llm-1
6f68e2289795   brodagroupsoftware/bgssrv-dmregistry:latest   "python3 /app/bgssrv…"   12 seconds ago   Up 10 seconds   0.0.0.0:25000->8000/tcp             docker-bgssrv-dmregistry-1
4400f3676e43   brodagroupsoftware/bgssrv-dmagent:latest      "python3 /app/bgssrv…"   12 seconds ago   Up 10 seconds   0.0.0.0:27000->8002/tcp             docker-bgssrv-dmagent-1
2a07dfb594ba   bitnami/etcd:3                                "/opt/bitnami/script…"   12 seconds ago   Up 10 seconds   2380/tcp, 0.0.0.0:30001->2379/tcp   docker-bgssrv-etcd-2-1
c8c8bd718e54   bitnami/etcd:3                                "/opt/bitnami/script…"   12 seconds ago   Up 10 seconds   2380/tcp, 0.0.0.0:30000->2379/tcp   docker-bgssrv-etcd-1-1
b5c0ac57228c   bitnami/etcd:3                                "/opt/bitnami/script…"   12 seconds ago   Up 10 seconds   2380/tcp, 0.0.0.0:30002->2379/tcp   docker-bgssrv-etcd-3-1
~~~~

## License

Use of this source code is governed by an MIT-style
license that can be found in the LICENSE file or at
https://opensource.org/licenses/MIT.
