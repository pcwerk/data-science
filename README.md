This project is designed to help students to quickly get into data science.  Before getting started, students should first clone this repository to their local computer.  As with all open learning and open source platforms, pull requests are glad accepted.

# Data-Science Process

## Data Acquisition

The idea behind data collection (or data acquisition) is for the students to collect their data in _any_ way they can.  To this extent, we do not place any restrictions (nor provide specific guidance) on how data can be acquired.

Some general guidance on data acquisition:
- Acquire as much data as possible (volume)
- Acquire as frequently as possible (velocity)
- Acquire from multiple sources (diversity)

Also, note that as part of the data acquisition stage, we really should consider the importance of data cleansing.  As a quick refresher, data cleansing is the process which we normalize (make everything uniform, prune out unnecessary information, reshape structure) data.

## Data Analysis

### Quantitative (Statistical) Analysis

The use of statistics to support your claim is the most clear and direct method for performing data analytics.  This involves collecting and interpreting data with numbers and graphs to identify patterns and trends.

- Descriptive Statistics - breakdown and summarize data as given
- Inferential Statistics - projection of small sampling into larger population

For descriptive statistics, students should consider two baseline approaches:

- Central computation 
- Spread computation

### Qualitative (Non-Statistical) Analysis

A non-statistical approach involves generic information and uses text, sound and other forms of media to do so.  In other words, you can be somewhat creative.

## Data Visualization


## Tools

### Docker

Containers technology is a must-have tool set for data science.  The most popular (and probably the only) containers technology is `docker`.  To install docker:

- Windows follow [this guide](https://docs.docker.com/desktop/install/windows-install/)
- Linux follow [this guide](https://docs.docker.com/desktop/install/linux-install/)

For Linux users, note that depending your distribution, you will have to follow the specific instructions for Ubuntu, RedHat (Alma Linux), or Fedora.  Generally speaking, Ubuntu is the most ready-to-use distribution.

In addition to docker, we recommend that you install [docker-compose](https://docs.docker.com/compose/install/).  Most of the project example code provided here will involve `docker-compose`.  Again, follow the specific instruction for your platform.

### Airflow

To build a data processing pipeline, students should consider [Apache Airflow](https://airflow.apache.org).  Note that setting up Airflow can be challenging, so please carefully read the [README.md](airflow/README.md) in the `airflow` folder.  This document provides detail instruction on how to setup and use Apache Airflow.

### Streamlit

We recommend students to use `streamlit` as a data visualization tool.  Getting started with streamlit is relatively simple. Inside the `streamlit` folder, you will find `docker-compose.yml` and `app/app.py`.  

To run streamlit, run `docker-compose up` and point your browser to `http://localhost:8501`.

To tweak with the visualization code, modify `app.py` and the files in folders `app/demo_echarts` and `app/demopyecharts`.

Source: https://github.com/andfanilo/streamlit-echarts

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

First create the three folders `dags`, `plugins`, `config`, and `storage`

```
mkdir dags/ logs/ plugins/ config/ storage/
chmod 777 dags logs plugins config storage
```

Next start up your airflow application

```
docker-compose up --build
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



# Data-Science Education

## Mathematics Background
- [Probability for Data Science & Machine Learning](https://youtu.be/sEte4hXEgJ8) introduction to probability
- [Statistics for Data Science & Machine Learning](https://youtu.be/tcusIOfI_GM) introduction to statistics

## Tools for Software Development
- [Git Tutorial for Beginners: Learn Git in 1 Hour](https://youtu.be/8JJ101D3knE) Git version control
- [Docker Tutorial for Beginners](https://youtu.be/pTFZFxd4hOI) Introduction to Docker

## Abstract Computer Science

- [The Most Beautiful Program Ever Written](https://www.youtube.com/watch?v=OyfBQmvr2Hc) with William Byrd

## Clojure

- [Clojure Tutorial](https://www.youtube.com/watch?v=ciGyHkDuPAE) with Derek Banas
- [Clojure Crash Course](https://www.youtube.com/watch?v=ZkJcVCW9GqY) with Kelvin Mai
- [Clojure in a Nutshell](https://www.youtube.com/watch?v=C-kF25fWTO8) with James Trunk

## Golang

- [Learn Go](https://www.codecademy.com/learn/learn-go) Learn how to use Go (Golang), an open-source programming language supported by Google!
- [Developing a RESTful API with Go and Gin](https://go.dev/doc/tutorial/web-service-gin) is tutorial introduces the basics of writing a RESTful web service API with Go and the Gin Web Framework (Gin)

## Python

### Introduction to Python
- [Python for Beginners - Learn Python in 1 Hour](https://youtu.be/kqtD5dpn9C8) soft introduction to programming
- [Python Tutorial - Python Full Course for Beginners](https://youtu.be/_uQrJ0TkZlc) for newbies
- [Learn Python Programming - Python Course](https://youtu.be/f79MRyMsjrQ) for experienced programmers
- [Python Programming](https://youtu.be/N4mEzFDjqtA) entire Python language in one video

### Python for Statistics
- [Statistics Tutorial with Python](https://youtu.be/YCPYNXtwKAc)
- [Statistics Tutorial 2 Real World Example](https://youtu.be/ger_Won5sRQ)

### Python and Data Science
- [Python Machine Learning Tutorial (Data Science)](https://youtu.be/7eh4d6sabA0)
- [Data Analysis with Python - Full Course for Beginners (NumPy, Pandas, Matplotlib, Seaborn)](https://youtu.be/r-uOLxNrNk8)

### Python Tools and Libraries
- [NumPy Tutorial 2021](https://youtu.be/8Y0qQEh7dJg) numerical computations
- [Pandas Tutorial 2021](https://youtu.be/PcvsOaixUh8) data manipulation
- [Matplotlib Tutorial 2021](https://youtu.be/wB9C0Mz9gSo) creating visuals

