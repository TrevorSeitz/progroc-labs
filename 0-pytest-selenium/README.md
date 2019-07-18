# ProgRoc Lab 0 - Pytest, Selenium

## Overview

This lab will focus on creating automated browser regressions against a preexisting website such as Google or Facebook. We will write code using [Python] and instrument browsers using [Selenium].  [Pytest] will be used to organize and execute test cases.

This lab is cross-platform and can be run on Windows, Linux, and OSX. The instructions will focus on OSX and Linux; equivalent Windows instructions are left out. The author apologizes profusely in advance.

[Python]: https://www.python.org/
[Selenium]: <https://www.seleniumhq.org/>
[Pytest]: https://docs.pytest.org/en/latest/

## Prerequisite Knowledge

- Understand CSS selectors such as class and ID
- Basic working knowledge of HTML
- Basic understanding of [XPath syntax](https://www.w3schools.com/xml/xpath_syntax.asp)

Python knowledge is _not_ required, but would certainly help. Hopefully the exercises will enable you to pick up on syntax as you work!

## Setup

### Browser Setup

For simplicity and uniformity, we will be instrumenting [Google Chrome](https://www.google.com/chrome/) with Selenium, so please install that on your system if you haven't already.

### Python Setup

First we will setup a sandboxed Python environment and install the requisite packages: Selenium Python bindings and Pytest.

There are a few different ways to setup a sandboxed Python environment for development:

- [venv](https://docs.python.org/3/library/venv.html)
- [Pipenv](https://docs.python-guide.org/dev/virtualenvs/)
- conda using [Miniconda]
  - conda can also be installed using [Anaconda](https://www.anaconda.com/)
  - Anaconda is a much larger install which comes with a lot of packages

In this lab, we will use [Miniconda] because of its cross-platform compatibility and simplicity.

[Miniconda]: https://docs.conda.io/en/latest/miniconda.html

### Miniconda Setup

1. [Download and Install Miniconda](https://docs.conda.io/en/latest/miniconda.html)
  - Ensure `conda` is added to your `PATH` and accessible from your terminal. Modify your `.bashrc` or `.bash_profile` if necessary (Linux, OSX)
2. [Create a Python 3.7 environment](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)
  1. Create: `conda create -n lab0 python=3.7`
  2. Activate: `conda activate lab0`
    - On Linux: `source activate lab0`
3. Install the required packages:
  `pip install pytest selenium`

### Setup Chromedriver

Webdrivers are the executables which puppet their respective browsers. To instrument Chrome, we will be using [chromedriver].

1. Install the Selenium bindings for the language of your choice
  - `conda activate lab0`
    - `conda info --envs` to list environments
2. Install [chromedriver]
  - Create a working directory and navigate to it
    - `mkdir ~/code/lab0`
    - `cd ~/code/lab0`
  - Download [chromedriver] to your working directory e.g. `~/code/lab0`
    - Optional: install the drivers to your `PATH`
      `sudo cp chromedriver /usr/local/bin`

Chromedriver needs to be accessible via the Python scripts running Selenium, hence it has to either reside in the same directory as the scripts or in the system `PATH`.

For more in-depth instructions, view the [Selenium Python Installation Guide](https://selenium-python.readthedocs.io/installation.html#introduction).

### Setup Selenium Server

_If you are having trouble with this part, please skip it._

The [Remote WebDriver] is the Selenium webdriver client which interfaces with the Selenium standalone server. A single Selenium standalone server can be used to instrument multiple types of drivers.

To use a Selenium standalone server:

1. Install the [Selenium server](https://www.seleniumhq.org/download/)
2. Run the Selenium server and point it to the desired driver service using the appropriate commandline options
  - `java -Dwebdriver.chrome.driver="/path/to/chromedriver" -jar selenum-standalone-server.jar`
  - [How to start Selenium server](https://github.com/lmc-eu/steward/wiki/Selenium-server-&-browser-drivers#3-start-selenium-server)
3. Use the Selenium bindings to connect to the standalone server instance and pass desired driver configuration options
   - The Selenium server will create the appropriate browser instance

[Remote WebDriver]: https://seleniumhq.github.io/docs/remote.html

## Part 1: Selenium Basics

Selenium is a cross-platform project written in Java which can instrument Firefox (gecko), Chrome, and Safari browsers using their driver interfaces. Besides its native Java bindings, Selenium has bindings to many languages, including Python and Ruby.

There are a few ways to run Selenium:

1. Instrumenting a driver directly
2. Having a Selenium standalone server instrument a driver
3. Setting up a simple Selenium grid to instrument a driver (beyond the scope of lab)

### Exercise 1

Use the chromedriver (not the Selenium server) to navigate to google and search for something.

1. Create a new python file in your working directory
  `~/code/lab0/test_google.py`
2. Read the [Selenium Python Getting Started Guide](https://selenium-python.readthedocs.io/getting-started.html).
3. Follow the examples in the guide and create a script which:
  - Navigates to google.com
  - Clicks the search bar
  - Enters a search term of your choice
  - Clicks the search button
  - Closes the Selenium chromedriver window

_Note:_ To run your script, you can either `./test_google.py` or `python test_google.py`. Make sure your conda environment is activated!

#### _Hints_

- Right-click and inspect elements to get their class and ID
- Prefer to identify elements by ID

### Exercise 2

Update your simple google script to

- Find the search input field using an XPath selector
  - Use a relative XPath instead of absolute!
- Send the `ENTER` key to search

#### _Hints_

- Right-click and inspect elements, then right-click the HTML chunk > Copy > Copy XPath
- Refer back to the [Selenium Python Getting Started Guide](https://selenium-python.readthedocs.io/getting-started.html)
- Refer to [basic XPath syntax](https://www.w3schools.com/xml/xpath_syntax.asp) and pay attention to the attribute selectors e.g. `[@attr="something"]`

### Exercise 2a

_If you could not setup Selenium server earlier, please skip this._

Run the Selenium standalone server and update your script to utilize the [Remote WebDriver].

[Remote WebDriver]: https://selenium-python.readthedocs.io/getting-started.html#using-selenium-with-remote-webdriver

## Part 2: Pytest

Pytest is a framework designed for writing unit tests, but can be used as a generic test executive. We will utilize some of its basic features to organize our Selenium automation into browser regression tests.

One of the strengths of pytest is its flexibility: tests can be written in the style of xUnit classes and methods, or it can leverage depedency injection in Python for a more functional approach.

It is extensible, offering hooks for customization, and can easily render reports to junit XML, HTML, and JSON.

### Exercise 3

Convert the simple google script into a pytest function (avoid xUnit style classes and methods). Call it something meaningful, like `test_perform_google_search`. Note that the prefix `test_` is necessary before all functions which are tests.

Run the test using `pytest` from the command line. Remember to have your environment activated. Your script _must_ be named with the format `test_<something>.py` in order for the default scrape settings to work.

The [pytest introduction guide] and [official pytest documentation] will make this task a breeze.

[pytest introduction guide]: http://pythontesting.net/framework/pytest/pytest-introduction/
[official pytest documentation]: https://docs.pytest.org/en/latest/

### Exercise 4

Follow the [pytest documentation] to create a module-level fixture which navigates to Google. Inject this fixture as an argument to your test function. Your test function should now only enter a search term and perform the search. The "navigate to Google" code should be self-contained within the fixture.

In a pytest fixture, setup and teardown code is specified together:
```python
@pytest.fixture(scope="module")
def some_fixture():
    # logic for setup
    yield
    # logic for teardown
```

Use `yield` in the fixture to indicate where the fixture setup code ends and place the logic to close the chromedriver window after the `yield`.

A quick debrief on fixture scopes:
- Session-level: run once during the entire execution of pytest
- Module-level: run once within each module that uses the fixture
- Function-level: run before _each test_ function

[pytest documentation]: https://docs.pytest.org/en/latest/fixture.html

### Exercise 5

Our test doesn't actually "test" anything (yet) because it cannot fail!

We're going to use a pytest plugin now. Go ahead and install [pytest-dependency](https://github.com/RKrahl/pytest-dependency).

    pip install pytest-dependency

1. Move your search term into a constant at the top of the file `SEARCH_TERM = "<your search term>"` and update your search test to use this
2. Create a new test function, `test_assert_title_contains_search`
  - Mark this test to be dependent on the previous `test_perform_google_search`
  - You don't need to inject the module-level fixture for this part, since it depends on the previous test
3. Use Selenium to scrape the title in your new test
4. Assert that the term you searched for is present in the title
5. Run ONLY your new assertion test from the commandline -- the dependency should automatically be run!
  `pytest test_google.py::test_assert_title_contains_search`

_Note:_ The pytest-dependency plugin is not necessary if you always run all the tests. Pytest tries to run tests in the order they are found within source code. However, there are many situations where you may only want to run one specific test.

### Exercise 6

We will setup our project to save test results to HTML and jUnit XML. This way, a continuous integration tool such as Gitlab or Jenkins can archive the test result artifact(s). In addition, suites like Jenkins offer the ability to parse jUnit XML to display passes and failures directly within their interface.

By parsing these results, posting metrics, and utilizing a visualization library e.g. using statsd or Datadog for metrics and Grafana or even matplotlib for rendering, you can create powerful visualizations regarding the health of your project(s).

To start, go ahead and install `pytest-html` for rendering HTML reports:

    pip install pytest-html

1. Create the file `conftest.py` in your project. This configuration file is automatically executed by pytest each run, and allows you to utilize pytest hooks.
2. Add hooks in `conftest.py` to integrate the docstrings into the HTML report. Docstrings are triple-quote strings directly following a test function `def`.
  ```python
  import pytest
  from py.xml import html

  @pytest.mark.optionalhook
  def pytest_html_results_table_header(cells):
      """Update HTML report table headers."""
      cells.insert(2, html.th("Description"))
      cells.insert(1, html.th("Time", class_="sortable time", col="time"))
      cells.pop()


  @pytest.mark.optionalhook
  def pytest_html_results_table_row(report, cells):
      """Add description and time columns to HTML report."""
      cells.insert(2, html.td(report.description))
      cells.insert(1, html.td(datetime.utcnow(), class_="col-time"))
      cells.pop()


  @pytest.mark.hookwrapper
  def pytest_runtest_makereport(item, call):
      """Add test docstrings as descriptions in the HTML report."""
      outcome = yield
      report = outcome.get_result()
      report.description = str(item.function.__doc__)
  ```
3. Add docstrings to your test functions.
4. Use `pytest -h` to view commandline options for pytest and check the `--junit-xml` option.
5. Run `pytest --html=report.html --self-contained-html --junit-xml=junit.xml` and verify that `report.html` and `junit.xml` are created. The `--self-contained-html` indicates to create an HTML report with inline styling.
6. To always run without having to specify these settings, create a `pytest.ini` file with default arguments to be passed to pytest. Refer to the [pytest configuration documentation] and use:
  ```
  [pytest]
  addopts = --html=report.html --self-contained-html --junit-xml=junit.xml
  ```

[pytest configuration documentation]: https://docs.pytest.org/en/latest/customize.html

