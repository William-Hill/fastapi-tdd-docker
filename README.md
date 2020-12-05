# FastAPI TDD Docker Tutorial

![Continuous Integration and Delivery](https://github.com/william-hill/fastapi-tdd-docker/workflows/Continuous%20Integration%20and%20Delivery/badge.svg?branch=master)

## Build and Launch Containers

```bash
    docker-compose up -d --build
```

## Shut down Containers

```bash
    docker-compose down -v
```

## View Logs

```bash
    docker-compose logs web
```

## Access Database

```bash
    docker-compose exec web-db psql -U postgres

    postgres=# \c web_dev
    postgres=# \q
```

## Update Database Schema

```bash
    docker-compose exec web python app/db.py
```

## Run Tests

```bash
    # normal run
    $ docker-compose exec web python -m pytest

    # disable warnings
    $ docker-compose exec web python -m pytest -p no:warnings

    # run only the last failed tests
    $ docker-compose exec web python -m pytest --lf

    # run only the tests with names that match the string expression
    $ docker-compose exec web python -m pytest -k "summary and not test_read_summary"

    # stop the test session after the first failure
    $ docker-compose exec web python -m pytest -x

    # enter PDB after first failure then end the test session
    $ docker-compose exec web python -m pytest -x --pdb

    # stop the test run after two failures
    $ docker-compose exec web python -m pytest --maxfail=2

    # show local variables in tracebacks
    $ docker-compose exec web python -m pytest -l

    # list the 2 slowest tests
    $ docker-compose exec web python -m pytest --durations=2

    #Run unit tests in parallel
    $ docker-compose exec web pytest -k "unit" -n auto
```

## View API Docs

```bash
    http://localhost:8002/docs
```

## Build container for Heroku

```bash
    docker build -f project/Dockerfile.prod -t registry.heroku.com/young-springs-76642/web ./project
```

## Run Heroku container locally

```bash
    docker run -d --name fastapi-tdd -e PORT=8765 -e DATABASE_URL=sqlite://sqlite.db -p 5003:8765 registry.heroku.com/young-springs-76642/web:latest
```

## Push Container to Heroku Registry

```bash
    docker push registry.heroku.com/young-springs-76642/web:latest

```

## Release the image

```bash
    heroku container:release web
```

## View the app

https://young-springs-76642.herokuapp.com/ping/

## View the API docs

http://young-springs-76642.herokuapp.com/docs

## Apply Database Migrations on Heroku

```bash
    heroku run python app/db.py -a young-springs-76642
```

## Add mock summary to database on Heroku

```bash
    http --json POST https://young-springs-76642.herokuapp.com/summaries/ url=https://testdriven.io
```

## Run test coverage

```bash
    docker-compose exec web python -m pytest --cov="." --cov-report html
```

## Run Black code formatter

```bash
    docker-compose exec web black .
```

## Run flake8 code linter

```bash
    docker-compose exec web flake8 .
```

## Sort dependencies

```bash
    docker-compose exec web /bin/sh -c "isort ./**/*.py"
```

## Build the image for GitHub

```bash
    docker build -f project/Dockerfile.prod -t docker.pkg.github.com/william-hill/fastapi-tdd-docker/web:latest ./project
```

## Authenticate to GitHub packages

```bash
    docker login docker.pkg.github.com -u william-hill -p <TOKEN>
```

## Push the image to the Docker registry on GitHub Packages

```bash
    docker push docker.pkg.github.com/william-hill/fastapi-tdd-docker/web:latest
```
