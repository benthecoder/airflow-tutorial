# Airflow Tutorial

- [Airflow Tutorial](#airflow-tutorial)
  - [Setup in local](#setup-in-local)
  - [Setup in Docker](#setup-in-docker)
  - [References](#references)

## Setup in local

check your python version

```bash
    python3 --version
```

Create a virtual environment

```bash
    python3 -m venv py_env
```

activate

```bash
source py_env/bin/activate
```

Install airflow

From the [Quick Start](https://airflow.apache.org/docs/apache-airflow/stable/start.html#) of airflow docs

```bash
# Airflow needs a home. `~/airflow` is the default, but you can put it
# somewhere else if you prefer (optional)
export AIRFLOW_HOME=.

# Install Airflow using the constraints file
AIRFLOW_VERSION=2.5.1
PYTHON_VERSION="$(python3 --version | cut -d " " -f 2 | cut -d "." -f 1-2)"
CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"
pip install "apache-airflow==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"

# The Standalone command will initialise the database, make a user,
# and start all components for you.
airflow standalone
```

If you're on mac and you get a gcc error, do `xcode-select --install`

Visit localhost:8080 in the browser and use the admin account details shown on the terminal to login.

Enable the example_bash_operator dag in the home page

```bash
    # run your first task instance
    airflow tasks run example_bash_operator runme_0 2015-01-01
    # run a backfill over 2 days
    airflow dags backfill example_bash_operator \
        --start-date 2015-01-01 \
        --end-date 2015-01-02
```

Running manually

```bash
airflow db init

airflow users create \
    --username admin \
    --firstname Peter \
    --lastname Parker \
    --role Admin \
    --email spiderman@superhero.org

airflow webserver --port 8080

airflow scheduler
```

## Setup in Docker

Based on the [docs](https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html)

Side: If you want to update docker-compose on mac

I had to uninstall it

```bash
    sudo rm /usr/local/lib/docker/cli-plugins/docker-compose
```

Install with brew

```bash
    brew install docker-compose
```

and symlink it

```bash
    mkdir -p ~/.docker/cli-plugins
    ln -sfn /opt/homebrew/opt/docker-compose/bin/docker-compose ~/.docker/cli-plugins/docker-compose
```

if you want to check where it was installed

```bash
    docker info --format '{{range .ClientInfo.Plugins}}{{if eq .Name "compose"}}{{.Path}}{{end}}{{end}}'
```

Check if you have enough memory

```bash
    docker run --rm "debian:bullseye-slim" bash -c 'numfmt --to iec $(echo $(($(getconf _PHYS_PAGES) * $(getconf PAGE_SIZE))))'
```

Fetch docker compose yaml

```bash
    curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.5.1/docker-compose.yaml'
```

## References

- [Airflow Tutorial for Beginners - Full Course in 2 Hours 2022 - YouTube](https://www.youtube.com/watch?v=K9AnJ9_ZAXE&list=PLwFJcsJ61oujAqYpMp1kdUBcPG0sE0QMT&index=2)
- [coder2j/airflow-docker: Source code of the Apache Airflow Tutorial for Beginners on YouTube Channel Coder2j (https://www.youtube.com/c/coder2j)](https://github.com/coder2j/airflow-docker)
