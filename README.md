# Laravel-docker
Docker development environment for Laravel projects with Docker Compose.

## General information
The project contains the essentials for Laravel projects. It has a `workspace` container which you can use to run php and node commands.

The following containers are available:
 - Workspace
 - Apache2
 - PHP-FPM
 - MariaDB

In the root of the project you can find an `env-example` file with environment variables and comments with info about them.

## Installation
0) Add to your project as a submodule or simply `git clone` 
1) Make a copy of `env-example` to create a `.env` file
2) Edit the `.env` file to your liking
3) Make sure to edit the variable `COMPOSE_PROJECT_NAME` if you have multiple projects with Laravel-docker
4) Run `docker-compose up`

## Usage
To issue `composer`, `artisan` or `npm` commands, you can use the `workspace` container after the following:
```
docker-compose exec --user=wsuser workspace bash
```
