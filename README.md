# Documentation Guid

This guide will explain how to do the reStructuredText (rst) documentation using Sphinx.

To get start with documentation involves three main steps:

1. Setup python virtual environment
2. Install Sphinx and other dependencies
3. Build documentation

## Setup python virtual environment


Install the python virtual environment using the following command
```
pip3 install virtualenv
```
Create a directory for the documentation and navigate to that directory
```
    mkdir doc
    cd doc
```
Create virtual environment using the following command
```
    python3 -m virtualenv venv
```
The above command will create the virtual environment with the name **venv** in the current folder.

Activate the virtual environment using the following command.
```
source venv/bin/activate
```
The above steps will setup the virtual environment

## Install Sphinx and other dependencies


Install `sphinx` in the virtual environment::
```
    pip3 install sphinx
```

Install the required html theme using the following command. 

```
pip install sphinx_rtd_theme
```
## Build documentation

clone the documentation template using the following command. 
