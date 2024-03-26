# Building a Data Pipeline

## Using Python Virtual Environment Strategy

We will operate the pipeline in a virtual environment:

```
export VIRTUAL_ENV=airflow.env
python3 -m venv $VIRTUAL_ENV
source $VIRTUAL_ENV/bin/activate
```

Install Airflow

```
export AIRFLOW=$(pwd)
export AIRFLOW_VERSION=2.8.4
export PYTHON_VERSION="$(python --version | cut -d " " -f 2 | cut -d "." -f 1-2)"
export CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"

pip install "apache-airflow==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"
```

Start up airflow in standalone (non-production) mode

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

Using the provided `docker-compose.yml` file to start up the containers

```
mkdir dags/ logs/ plugins/ config/
docker-compose up -d
```

Let's create your first DAG

```python
#
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.
"""
### Tutorial Documentation
Documentation that goes along with the Airflow tutorial located
[here](https://airflow.apache.org/tutorial.html)
"""
from __future__ import annotations

# [START tutorial]
# [START import_module]
import textwrap
from datetime import datetime, timedelta

# The DAG object; we'll need this to instantiate a DAG
from airflow.models.dag import DAG

# Operators; we need this to operate!
from airflow.operators.bash import BashOperator

# [END import_module]


# [START instantiate_dag]
with DAG(
    "bash_demo",
    # [START default_args]
    # These args will get passed on to each operator
    # You can override them on a per-task basis during operator initialization
    default_args={
        "depends_on_past": False,
        "email": ["airflow@example.com"],
        "email_on_failure": False,
        "email_on_retry": False,
        "retries": 1,
        "retry_delay": timedelta(minutes=5),
        # 'queue': 'bash_queue',
        # 'pool': 'backfill',
        # 'priority_weight': 10,
        # 'end_date': datetime(2016, 1, 1),
        # 'wait_for_downstream': False,
        # 'sla': timedelta(hours=2),
        # 'execution_timeout': timedelta(seconds=300),
        # 'on_failure_callback': some_function, # or list of functions
        # 'on_success_callback': some_other_function, # or list of functions
        # 'on_retry_callback': another_function, # or list of functions
        # 'sla_miss_callback': yet_another_function, # or list of functions
        # 'trigger_rule': 'all_success'
    },
    # [END default_args]
    description="A simple tutorial DAG",
    schedule=timedelta(days=1),
    start_date=datetime(2021, 1, 1),
    catchup=False,
    tags=["example"],
) as dag:
    # [END instantiate_dag]

    # t1, t2 and t3 are examples of tasks created by instantiating operators
    # [START basic_task]
    t1 = BashOperator(
        task_id="print_date",
        bash_command="date",
    )

    t2 = BashOperator(
        task_id="sleep",
        depends_on_past=False,
        bash_command="sleep 5",
        retries=3,
    )

    # [END basic_task]

    # [START documentation]
    t1.doc_md = textwrap.dedent(
        """\
    #### Task Documentation
    You can document your task using the attributes `doc_md` (markdown),
    `doc` (plain text), `doc_rst`, `doc_json`, `doc_yaml` which gets
    rendered in the UI's Task Instance Details page.
    ![img](http://montcs.bloomu.edu/~bobmon/Semesters/2012-01/491/import%20soul.png)
    **Image Credit:** Randall Munroe, [XKCD](https://xkcd.com/license.html)
    """
    )

    dag.doc_md = __doc__  # providing that you have a docstring at the beginning of the DAG; OR
    dag.doc_md = """
    This is a documentation placed anywhere
    """  # otherwise, type it like this
    # [END documentation]

    # [START jinja_template]
    templated_command = textwrap.dedent(
        """
    {% for i in range(5) %}
        echo "{{ ds }}"
        echo "{{ macros.ds_add(ds, 7)}}"
    {% endfor %}
    """
    )

    t3 = BashOperator(
        task_id="templated",
        depends_on_past=False,
        bash_command=templated_command,
    )
    # [END jinja_template]

    t1 >> [t2, t3]
# [END tutorial]
```

Put this file into `dags/first_dag/bash_flow.py`

Now you should be able to enable and run the workflow through the GUI.

