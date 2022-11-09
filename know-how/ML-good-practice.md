# Good practices for ML projects

## Coding principles
Essential best practices from software development to improve the way we do coding in ML projects can be sumarized in five principles:

* __Organize code into reusable units__: We should avoid code duplication and extract functionality into distinct modules, classes, and functions. The code we write for our projects should be loosely coupled and highly cohesive. This means that units of code should serve a specific function and that we should be able to change those units without affecting other ones.
* __Use Git for version control__: Git is the industry standard for version control. We should use it to track changes to our code so that we can compare versions over time and refer back to previous versions when needed.
* __Follow style guides__: Following pre-determined style guides makes reading just a little easier. It becomes less troublesome to revisit code we have written in the past or to comprehend code written by our teammates.
* __Make dependencies and requirements explicit__: Software evolves over time. To ensure our code will still work in the future, we should specify which versions of dependencies and requirements to use. We can do so in a dedicated requirements file where we list all dependencies and their versions.
* __Testing:__ We should test our code to ensure it does what it is supposed to do. Moreover, we should test our code to ensure it doesn't do anything beyond what we expect it to do.

## Data Science Project Structure

he blueprint file structure follows the following pattern


        ├── data               <- The data folder used by default by the dockerized environment
        │
        ├── docker             <- docker files injections used to build the environment
        │
        ├── docs               <- Sphinx documentation of the project
        │
        ├── notebooks          <- Jupyter notebooks used by the dockerized environment
        │
        ├── awesome_project    <- Source code location of this project.
        │   ├── __init__.py    <- Makes it a Python module
        │   │
        │   ├── commands       <- Command scripts that will allow you to use your project with command lines
        │   │   └── cmd.py
        │   │
        │   ├── features       <- Contains the scripts that will handle feature explorations and engineering
        │   │
        │   ├── models         <- Contains the scripts that will handle the models
        │   │
        │   ├── operator       <- Contains the scripts that will handle data processing
        │   │   ├── data.py
        │   │   ├── dataframe.py
        │   │   └── generator.py
        │   │
        │   └── tests          <- Contains the unit tests
        │       └── operator
        │           ├── data.py
        │           ├── dataframe.py
        │           └── generator.py
        │
        ├── Dockerfile         <- Build file used by Docker to build the project environment
        │
        ├── LICENSE
        │
        ├── Makefile           <- Makefile to run commands over the project
        │
        ├── README.md
        │
        ├── requirements.txt   <- The requirements file that lists all the package dependencies to install
        │
        ├── setup.cfg          <- Configuration file used to fill the setup.py file
        │
        ├── setup.py           <- Setup file to install the project as a pip package
        │
        └── .gitignore         <- Avoid pushing tmp files that should never exist on Github
