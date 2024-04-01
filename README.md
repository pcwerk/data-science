Data Science Tool Kit.  This project is designed to help students to quickly get into data science.

# Data-Science Tools

## Docker

Containers technology is a must-have toolset for data science.  The most popular (and probably the only) containers technology is `docker`.  To install docker:

- Windows follow [this guide](https://docs.docker.com/desktop/install/windows-install/)
- Linux follow [this guide](https://docs.docker.com/desktop/install/linux-install/) -- note that depending your Linux distribution, you will have to follow the specific instructions for Ubuntu, RedHat (Alma Linux), or Fedora.

In addition to docker, we recommend that you install [docker-compose](https://docs.docker.com/compose/install/).  Most of the project example code provided here will involve the `docker-compose`.  Again, follow the specific instruction for your platform.

## Airflow

To build a data processing pipeline, students should consider [Apache Airflow](https://airflow.apache.org).  Note that setting up Airflow can be challenging, so please carefully read the [README.md](airflow/README.md) in the `airflow` folder.  This document provides detail instruction on how to setup and use Apache Airflow.

## Streamlit

We recommend students to use `streamlit` as a data visualization tool.  Getting started with streamlt is relatively simple. Inside the `streamlit` folder, you will find `docker-compose.yml` file and several examples: `streamlit_app.py.*`.  

To run streamlit, run `docker-compose up` and point your browser to `http://localhost:8501`.

To tweak with the visualization code, modify `streamlit_app.py`.  You can refer to the other `streamlit_app.py.*` files, which have been providedas reference.

# Data-Science Process

## Data Acquisition

The idea behind data collection (or data acquisition) is for the students to collect their data in _any_ way they can.  To this extent, we do not place any restrictions (nor provide specific guidance) on how data can be acquired.

Some general guidance on data acquisition:
- Acquire as much data as possible (volume)
- Acquire as frequently as possible (velocity)
- Acquire from multiple sources (diversity)

Also, note that as part of the data acquisition stage, we really should consider the importance of data cleansing.  As a quick refresher, data cleansing is the process which we normalize (make everything uniform, prune out uncessary information, reshape structure) data.

## Data Analysis

### Statistical Analysis

### Other Analytical Methods

## Data Visualization

