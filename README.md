# SQLPlus Query Metrics - PARR Tools

## Overview
This project was developed to provide an automated method to capture performance metrics for a suite of Oracle queries using a docker container to execute them with SQL\*Plus.  This project provides a method to capture query metrics in a variety of configurations for flexibility and allows a user to define multiple queries and define the SQL\*Plus connection string to determine which Oracle database instance to execute the queries on.  This project is forked from the [SQLPlus Query Metrics project](https://picgitlab.nmfs.local/centralized-data-tools/sqlplus-query-metrics).

## Resources
-   SQLPlus Query Metrics - PARR Tools Version Control Information:
    -   URL: git@picgitlab.nmfs.local:query-metrics/sqlplus-query-metrics-parr-tools.git
    -   Version: 1.0 (Git tag: parr_tools_sqlplus_query_metrics_v1.0)
    -   Forked repository (upstream)
        -   [SQLPlus Query Metrics README](https://picgitlab.nmfs.local/centralized-data-tools/sqlplus-query-metrics/-/blob/main/README.md?ref_type=heads)
        -   SQLPlus Query Metrics Version Control Information:
            -   URL: git@picgitlab.nmfs.local:centralized-data-tools/sqlplus-query-metrics.git
            -   Version: 1.0 (Git tag: sqlplus_query_metrics_v1.0)

## Scenarios
-   There are three different scenarios implemented by the docker project:
    -   Local - this scenario deploys the docker container to a local docker host and connects to a local Oracle database
    -   Remote - this scenario deploys the docker container to a remote docker host and connects to a remote Oracle database
    -   Hybrid - this scenario deploys the docker container to a local docker host and connects to a remote Oracle database

## Repository Configuration
-   Update each of the SQL calling scripts (e.g. [query_metrics_calling_script.hybrid.sql](./docker/src/SQL/query_metrics_calling_script.hybrid.sql) for the hybrid scenario) to change the V_DB_NAME variable to the name of the database that is being queried.
    -   Update the [README.md](./README.md) file to change the volume names, document title heading, and setup procedure accordingly
    -   Update each of the docker project preparation bash scripts (e.g. [prepare_docker_project.local.sh](./deployment_scripts/prepare_docker_project.local.sh) for the local scenario) to change the git URL to the URL for the forked repository and optionally to change the deployment folder path.
        -   Update each of the docker project build and deploy bash scripts (e.g. [build_deploy_project.local.sh](./deployment_scripts/build_deploy_project.local.sh) for the local scenario) to change the deployment folder path if it was changed in the docker project preparation bash scripts.
    -   Optional updates:
        -   Update the [query_metrics_export.sql](./docker/src/SQL/query_metrics_export.sql) SQL script to specify a repository-specific output .csv file name  
        -   Update the [run_query_metrics.sh](./docker/src/run_query_metrics.sh) bash script to specify a repository-specific output .csv file name  
        -   Update the docker compose runtime configuration file [docker-compose.prod.yml](./docker/docker-compose.prod.yml) to specify repository-specific the volume names and container name
        -   Update the docker compose configuration file [docker-compose.yml](./docker/docker-compose.yml) to specify the volume names and image/container names for the forked project

## Setup Procedure
-   Execute the appropriate docker preparation script stored in the [deployment_scripts](./deployment_scripts) folder to prepare the docker container for deployment in a new working directory
    -   For example use the [prepare_docker_project.local.sh](./deployment_scripts/prepare_docker_project.local.sh) bash script to prepare the Local docker container for deployment in the c:/docker/sqlplus-query-metrics-parr-tools-local folder
-   Update the DB_credentials.sql file in the appropriate new working directory to specify the Oracle SQL*Plus database connection string (e.g. c:/docker/sqlplus-query-metrics-parr-tools-local/docker/src/SQL/credentials/DB_credentials.sql) for the local scenario

## Building/Running Container
-   Execute the appropriate build and deploy script for the given scenario (e.g. [build_deploy_project.remote.sh](./deployment_scripts/build_deploy_project.remote.sh) for the remote scenario)

## Checking Results
-   Open the docker volume sqlplus-query-metrics-parr-tools-logs to view the log files for the different executions of the docker container
-   Open the docker volume sqlplus-query-metrics-parr-tools-data to view the exported data files for the different queries
    -   Open the parr-tools-query-metrics.csv to view the metrics that were captured for each query execution
