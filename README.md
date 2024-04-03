This project is designed to help students to quickly get into data science.  Before getting started, students should first clone this repository to their local computer.  As with all open learning and opensource platforms, pull requests are glad accepted.

# Data-Science Tools

## Docker

Containers technology is a must-have toolset for data science.  The most popular (and probably the only) containers technology is `docker`.  To install docker:

- Windows follow [this guide](https://docs.docker.com/desktop/install/windows-install/)
- Linux follow [this guide](https://docs.docker.com/desktop/install/linux-install/)

For Linux users, note that depending your distribution, you will have to follow the specific instructions for Ubuntu, RedHat (Alma Linux), or Fedora.  Generally speaking, Ubuntu is the most ready-to-use distribution.

In addition to docker, we recommend that you install [docker-compose](https://docs.docker.com/compose/install/).  Most of the project example code provided here will involve `docker-compose`.  Again, follow the specific instruction for your platform.

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

### Qantitative (Statistical) Analysis

The use of stastistics to support your claim is the most clear and direct method for performing data analytics.  This involves collecting and interpreting data with numbers and graphs to identify patterns and trends.

- Descriptive Statistics - breakdown and summarize data as given
- Inferential Statistics - projection of small sampling into larger population

For descriptive statistics, students should consider two baseline approaches:

- Central computation 
- Spread computation

### Qualitative (Non-Statistical) Analysis

A non-statistical approach involves generic information and uses text, sound and other forms of media to do so.  In otherwords, you can be somewhat creative.

## Data Visualization



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
