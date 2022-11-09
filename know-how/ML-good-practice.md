## Good practices for ML projects
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
