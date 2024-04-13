## Notions:
- #### Script - A Python file which is run like `python mysctupt.py`
- #### Package - A directory full of Python code to be imported e.g. numpy
- #### Subpackage -- A smaller package inside a package e.g. `numpy.random`
- #### Module - A Python file indise a package whcih store the package code 
- #### Directory tree of a package
![[1712955270407.png]]

# Documentation  

## help() function

![[Pasted image 20240412165708.png]]

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
