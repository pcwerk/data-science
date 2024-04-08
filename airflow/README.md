# Building a Data Pipeline

The Apache Airflow is an open source python project designed to help data scientist to quickly and efficiently build out data pipelines.  When working with `airflow` you have two options:

1. Using native python within an [virtual environment](#using-python-virtual-environment-strategy) _or_
2. Using [docker containers](#using-containers-strategy)

This document describes both approaches.

## Using Python Virtual Environment Strategy

We will operate the pipeline in a virtual environment on Linux (or Mac). THe process for Windows will be slightly different.

First prepare the variables and location where the virtual environment will reside:

```
export VIRTUAL_ENV=airflow.env
python3 -m venv $VIRTUAL_ENV
source $VIRTUAL_ENV/bin/activate
```

Next, install Airflow:

```
export AIRFLOW=$(pwd)
export AIRFLOW_VERSION=2.8.4
export PYTHON_VERSION="$(python --version | cut -d " " -f 2 | cut -d "." -f 1-2)"
export CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"

pip install "apache-airflow==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"
```

Finally, to start up airflow in standalone (non-production) mode

```
airflow standalone
```

You should see the following:

```
webserver  | [2024-03-26 11:11:01 -0700] [3281111] [INFO] Starting gunicorn 21.2.0
webserver  | [2024-03-26 11:11:01 -0700] [3281111] [INFO] Listening at: http://0.0.0.0:8080 (3281111)
webserver  | [2024-03-26 11:11:01 -0700] [3281111] [INFO] Using worker: sync
webserver  | [2024-03-26 11:11:01 -0700] [3281232] [INFO] Booting worker with pid: 3281232
webserver  | [2024-03-26 11:11:01 -0700] [3281233] [INFO] Booting worker with pid: 3281233
webserver  | [2024-03-26 11:11:01 -0700] [3281234] [INFO] Booting worker with pid: 3281234
webserver  | [2024-03-26 11:11:01 -0700] [3281235] [INFO] Booting worker with pid: 3281235
standalone | Airflow is ready
standalone | Login with username: admin  password: 5Q3cQKVvQMZVeWbT
standalone | Airflow Standalone is for development purposes only. Do not use this in production!
```

Note the username `admin` and the passowrd `5Q3cQKVvQMZVeWbT`.  This is needed to get into the airflow server.  Point your browser to http://localhost:8080 and login with the password show above.

![airflow](resources/airflow.png)

## Using Containers Strategy

The recommended approach is to use containers.  Using the provided `docker-compose.yml` file as a starting point. 

First create the three folders `dags`, `plugins`, and `config`

```
mkdir dags/ logs/ plugins/ config/
chmod 777 dags logs pluggins config
```

Next start up your airflow application

```
docker-compose up -d
```

Now point your browser to `http://localhost:8080` and you should see something similar to this

![airflow](resources/airflow.png)

Note to view your containers activitities:

```
docker-compose logs -f
```

Are you should see something like this

```
airflow-init-1       | [2024-04-06T14:32:17.733+0000] {override.py:1769} INFO - Created Permission View: can read on View Menus
airflow-init-1       | [2024-04-06T14:32:17.737+0000] {override.py:1820} INFO - Added Permission can read on View Menus to role Admin
airflow-init-1       | [2024-04-06T14:32:17.746+0000] {override.py:1769} INFO - Created Permission View: menu access on Resources
airflow-init-1       | [2024-04-06T14:32:17.749+0000] {override.py:1820} INFO - Added Permission menu access on Resources to role Admin
airflow-init-1       | [2024-04-06T14:32:17.774+0000] {override.py:1769} INFO - Created Permission View: can read on Permission Views
airflow-init-1       | [2024-04-06T14:32:17.778+0000] {override.py:1820} INFO - Added Permission can read on Permission Views to role Admin
airflow-init-1       | [2024-04-06T14:32:17.788+0000] {override.py:1769} INFO - Created Permission View: menu access on Permission Pairs
airflow-init-1       | [2024-04-06T14:32:17.792+0000] {override.py:1820} INFO - Added Permission menu access on Permission Pairs to role Admin
airflow-init-1       | [2024-04-06T14:32:19.049+0000] {override.py:1458} INFO - Added user airflow
airflow-init-1       | User "airflow" created with role "Admin"
airflow-init-1       | 2.8.4
```

## Working with the DAGS

DAGS or direct acyclic graphs are essentially Airflow control files.  Data pipelines are written in python and follows a structure.  In the `bigdata` folder, there are two example DAGS.  Let's get started!

### Bash Executor

Let's take a look at the `bigdata/first_dag/bash_flow.py` file. There are currently three tasks: `t1`, `t2`, and `t3`.  Let's add it to the running instance of Airflow:

```
mkdir dags/first_dag
cp bigdata/first_dag/bash_flow.py dags/first_dag/bash_flow.py
```
Enable the dag and run it.  Let us now add a fourth task:

```python
    t4 = BashOperator(
        task_id="print_date_again",
        bash_command="date",
    )
```

Don't forget to also add in the flow assignment

```python
    t1 >> [t2, t3] >> t4
```    

### Python Executor

For a python executor (or running python code), you should do something similar by copying `bigdata/second_dag/python_flow.py` to the `dags/second_dag` folder.

The example provided is rather simplistic. However, you can copy your entire data acquisition code base into the same DAG folder.


