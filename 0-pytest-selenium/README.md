# ProgRoc Lab 0 - Pytest, Selenium

## Overview

This lab will focus on creating automated browser regressions against a preexisting website such as Google or Facebook. We will write code using [Python] and instrument browsers using [Selenium].  [Pytest] will be used to organize and execute test cases.

This lab is cross-platform and can be run on Windows, Linux, and OS X.

[Python]: https://www.python.org/
[Selenium]: <https://www.seleniumhq.org/>
[Pytest]: https://docs.pytest.org/en/latest/

## Download Repo



## Python Setup

First we will setup a sandboxedPython environment to install the requisite packages: Selenium Python bindings and Pytest.

There are a few different ways to setup a sandboxed Python environment for development:

- [venv](https://docs.python.org/3/library/venv.html)
- [Pipenv](https://docs.python-guide.org/dev/virtualenvs/)
- conda using [Miniconda](https://docs.conda.io/en/latest/miniconda.html)
  - conda can also be installed using [Anaconda](https://www.anaconda.com/)
  - Anaconda is a much larger install which comes with a lot of packages

In this lab, we will use Miniconda because of its cross-platform nature and simplicity.

### Miniconda Setup

1. [Download and Install Miniconda](https://docs.conda.io/en/latest/miniconda.html)
   - Ensure `conda` is added to your `PATH` and accessible from your terminal. Modify your `.bashrc` or `.bash_profile` if necessary (Linux, OSX)
2. [Create a Python 3.7 environment](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)
   1. Create: `conda create -n progroc-lab0 python=3.7`
   2. Activate: `conda activate progroc-lab0`
      - On Linux: `source activate progroc-lab0`
3. Install the required packages

## Selenium

Selenium is a cross-platform project written in Java which can instrument Firefox (gecko), Chrome, and Safari browsers using their native driver interfaces. Besides its native Java bindings, Selenium provides bindings to many languages, including Python and Ruby.

In this lab, we will setup and run Selenium in three different ways using Python:

1. Instrumenting a driver directly
2. Having a Selenium standalone server instrument a driver
3. Setting up a simple Selenium grid to instrument a driver

#### Approach 1: WebDrivers

The simplest way to get running using Selenium is to:

1. Install the Selenium bindings for the language of your choice
2. Make sure the browser you want to test with is installed (Chrome, Firefox, etc.)
3. Install the appropriate driver service (chromedriver, geckodriver, etc.)
4. Use the Selenium bindings to create an instance of the appropriate driver with the desired options and instrument a brower instance

#### Approach 2: Remote WebDriver and Selenium Server

The [Remote WebDriver] is the Selenium webdriver client which interfaces with the Selenium standalone server. Selenium standalone servers can be run singularly and used to instrument various driver instance, or multiple server instances can be configured to run as a Selenium Grid.

To use a Selenium standalone server:

1. Complete the steps for instrumenting the desired browser using a webdriver with Selenium bindings in the language of your choice
2. Install a Selenium server
3. Run the Selenium server and point it to the desired driver service
4. Use the Selenium bindings to connect to the standalone server instance and pass desired driver configuration options
   - The Selenium server will create the appropriate browser instance

[Remote WebDriver]: https://seleniumhq.github.io/docs/remote.html

#### Approach 3: Selenium Grid

The [Selenium Grid] operates using a node-hub model. The SeleniumGrid is used to parallelize testing across multiple browsers and OSes. The hub server delegates tasks to nodes which can instrument different browsers running on different host OSes.

This process abstracts the test endpoints from the client. Rather than requiring the client to connect directly to different servers, the client only needs to connect to one server for all its testing.

To 

[Selenium Grid]: https://www.seleniumhq.org/docs/07_selenium_grid.jsp

##### Aside: Cloud Services

Cloud services such as [Sauce Labs] setup provide remote Selenium Grids as SaaS (software as a service). These services help QA engineers focus on test development rather than maintaining Selenium infrastructure, which can be rather involved. This reduces maintenance costs and hopefully enables companies to improve product quality and testing.

[Sauce Labs]: https://saucelabs.com

### Lab Overview



For more information, review  the [official Selenium introduction].

[Selenium]: https://www.seleniumhq.org/
[Official Selenium Introduction]: https://www.seleniumhq.org/docs/01_introducing_selenium.jsp
[Sauce Labs]: https://saucelabs.com/

### Pytest

Pytest is a framework designed for writing unit tests, but can be used as a generic test executive. We will use some of its basic features to save our Selenium automation so we can re-run them later.

One of the strengths of pytest is its flexibility: tests can be written in the style of xUnit  classes and methods, or it can leverage depedency injection in Python for a more functional approach. 

It is extensible, offering hooks for customization, and can easily render reports to junit XML, HTML, and JSON.