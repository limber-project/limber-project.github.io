# Installation
This page discusses how to setup a new Limber project.

## Prerequisites
[Poetry](https://python-poetry.org) is required to manage the packages that are needed by Limber.
View the [Poetry website](https://python-poetry.org/docs/) for information on how to install Poetry.

## Setup
Enter the commands below in a terminal to establish a new Limber project.

```bash
# Retrieve a fresh instance of Limber
git clone https://github.com/limber-project/limber.git
cd limber

# Install required packages
poetry shell
poetry install

# Run web server
uvicorn limber.main:app
```

> If you receive an error installing psycopg2 with the message 'ld: library not found for -lssl', view the following [stack overflow discussion](https://stackoverflow.com/questions/26288042/error-installing-psycopg2-library-not-found-for-lssl) to find out how to add openssl to your environment for compiling psycopg2.
