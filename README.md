This README aims to explain the **Inception** project from 42, how the stack is organized, and how to build, run, and extend it.

***

## Overview

Inception is a 42 school DevOps project where you set up a small Docker-based infrastructure using Dockerfiles and docker-compose.
The mandatory stack includes NGINX (TLS), WordPress with PHP-FPM, and MariaDB, all running in separate containers orchestrated with docker-compose.

***

## Project goals

- Learn containerization concepts by building all images from your own Dockerfiles (no pre-built images for main services).
- Design a reproducible, isolated environment using docker-compose, custom networks, and named volumes to persist data.
- Secure the setup with TLS, non-root users, and environment variables stored outside the repository (e.g. `.env`, secrets).

***

## Stack architecture

- **NGINX**: Reverse proxy handling HTTPS (TLS) termination and serving static files, forwarding PHP requests to WordPress/PHP-FPM.
- **WordPress + PHP-FPM**: Application container running WordPress with PHP-FPM, configured via environment variables for DB access and initial admin user.
- **MariaDB**: Database container initialized with a dedicated WordPress database, user, and password stored in secrets or env vars.

***

## Repository layout

A common layout for this project is:

```txt
inception/
├── Makefile
└── srcs/
    ├── docker-compose.yml
    ├── .env            # ignored in Git
    └── requirements/
        ├── nginx/
        │   ├── Dockerfile
        │   └── conf/...
        ├── wordpress/
        │   ├── Dockerfile
        │   └── tools/...
        └── mariadb/
            ├── Dockerfile
            └── tools/...
```

This structure separates compose orchestration, environment configuration, and per-service Dockerfiles and configs.

***

## Usage

- Build and start the whole stack with a single command, typically `make` or `make up`, which wraps `docker-compose up --build -d`.
- Stop and clean containers, networks, and volumes with `make down` or a dedicated `clean` rule that calls `docker-compose down -v` and removes generated data.
- Access the WordPress site through the configured domain (e.g. `https://login.42.fr`) once all containers are up and healthy.

