## Notions:
- #### Script - A Python file which is run like `python mysctupt.py`
- #### Package - A directory full of Python code to be imported e.g. numpy
- #### Subpackage -- A smaller package inside a package e.g. `numpy.random`
- #### Module - A Python file indise a package whcih store the package code 
- #### Directory tree of a package
![1712955270407](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/785d47f6-64e2-46a4-9bbb-65c3f21fe485)


# Documentation  

## help() function
![Pasted image 20240412165708](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/1b165606-a7f4-4b4d-8649-c594e51a04bb)


## Function documentation
```
def count_words(filepath, words_list):
	""" ... """

defcount_words(filepath, words_list):
	"""Count the total number of times these words appear."""
```


## Documentation style

```
# Google documentation style

"""Summary line.
Extended description of function.

Args: 
arg1 (int): Description of arg1 
arg2 (str): Description of arg2
```

```
# NumPy style

"""Summary line. 
Extended description of function. 

Parameters 
---------- 
arg1 : int 
	Description of arg1 ...
```

```
# reStructured text style

"""Summary line. 
Extended description of function. 
:param arg1: Description of arg1 
:type arg1: int 
:param arg2: Description of arg2 
:type arg2: str
```

```
# Epytext style

"""Summary line. 
Extended description of function. 

@type arg1: int 
@param arg1: Description of arg1 
@type arg2: str 
@param arg2: Description of arg2
```

### NumPy documentation style -- popular 
![[1712972402655.png]]

## Documentation templates and style translation -- pyment 
```
pyment -w -o numpydoc textanalysis.py
```

![[1712972548811.png]]

![[1712972599156.png]]

### Package and module documentation 
![[1712973173916.png]]

```
# Absolute import
from mysklearn.utils import MyException
# Relative import
from ..utils import MyException

# Relative import cheat sheet
# From current directory, import module
from . import module
# From one directory up, import module
from .. import module
# From module in current directory, import function
from .module import function
# From subpackage one directory up, from module in that subpackage, import function
from ..subpackage.module import function
```

#  Installing your own package

To install packages, need the ==setup.py==

![[1712977202898.png]]

### Inside setup.py
```
# Import required functions
from setuptools import setup
# Call setup function
setup( 
	author="James Fulton", 
	description="A complete package for linear regression.", 
	name="mysklearn", 
	version="0.1.0",
	packages=find_packages(include=["mysklearn", "mysklearn.*"])，
)
# version number = (major number) . (minor number) . (patch number)
```
## Dependency
-- check dependency and package version from the package history or release notes
![1713016387572 1](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/fd6cfa20-1d5b-497d-a04e-41906bb3a10d)
```
# Call setup function
setup( 
	...
	install_requires=[
		'pandas>=1.0', 
		'scipy==1.1', 
		'matplotlib>=2.2.1,<3']
)
# version number = (major number) . (minor number) . (patch number)
```
##  Including licences and writing READMEs
### README format
```
Markdown (commonmark)
-- Contained in README.md file
-- Simpler
-- Used in this course and in the wild

# reStructuredText
-- Contained in README.rst file
-- More complex
-- Also common in the wild
```

### MANIFEST.in
-- Lists all the extra files to include in your package distribution.
![1713017713669](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/9c47ba6a-0b9f-4bac-8055-c141cf4310ec)

## Publish the package
When upload the package, it is a package distribution:
* Distribution package: a bundled version of my package which is ready to install
* Source distribution: a distribution package which is mostly my source code
* Wheel distribution: a distribution package which has been processed to make it dater to install

### Build distributions
```
# sdist = source distribution
# bdist_wheel = wheel distribution
python setup.py sdist bdist_wheel
```
![1713018392102](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/8dac7b66-b772-4473-94cc-bb277d38d1de)

### upload packages

```
# upload diostribution to PyPI
twine upload dist/*

# Upload distribution to TestPyPI
twine upload -r testpypi dist/*
```

# Test packages
## Write tests

```
# function for test
def get_ends(x):
"""Get the first and last element in a list"""
	return x[0], x[-1]

# test
def test_get_ends():
	assert get_ends([1,5,39,0]) == (1,0)
	assert get_ends(['n','e','r','d']) == ('n','d')
test_get_ends()

# Running pytest 
# pytest looks inside the test directory
# It looks for modules like test_modulename.py
# It looks for functions like test_functionname()
# It runs these functions and shows output
```
![1713019277379](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/6f885005-3a60-4d99-82fc-f081095c3345)
![1713019299424](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/8e48e8d9-7d58-4269-b178-310dc5ec33c2)
![1713019347128](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/6b1b5d21-6a9f-44a4-8bc9-141a77e80f43)

## test packages with different environment

-- Install all allowed python version in the setup.py
-- Run **tox**
![1713020638443](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/0ded02d2-79e1-4288-9497-e5b840674329)

```
# content og tox.ini
# Headings are surrounded by squarebrackets [...]
# To test Python version X.Y add pyXY to envlist
# The versions of Python you test need to beinstalled already
# The commands parameter lists the terminalcommands tox will run
# The commands list can be any commandswhich will run from the terminal, like ls, cd, echo etc

[tox]
envlist = py27, py35, py36, py37

[testenv]
deps = pytest
commands = 
	pytest 
	echo "run more commands" 

```

## Keeping your package stylish -- flake8

```
flake8 features.py

--ignire
# # noqa:E222
```
![1713238288444](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/ca6c70ec-8a72-4e08-93bc-18a349dc1617)




