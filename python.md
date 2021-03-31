# Some useful Python notes

## Class decorators

Useful Medium post [here](https://medium.com/swlh/three-decorators-commonly-used-in-python-custom-classes-acc34a145dcf)

* **`@classmethod`**: a class method as a factory method that **creates an instance object**, as an **alternative** to the default constructor method, as defined in the `__init__` function.

* **`@staticmethod`**: **utility functions** that are eused elsewhere within the class to perform some function. They are not provided the `self` argument

* **`@property`**: returns useful data about the instance of the object (`len()` might be one if it wasn't a magic method, because it provides info about the instance but doesn't perform any real action)
    * Also has a `setter` method which allows one to overwrite the property

## Class magic methods

## Useful things

Print a value with rounded decimals:
* Change the underlying data: `round(loss)`
* Keep the underlying data but print a rounded value: `f"{loss:.2f}.png"` - where the `:.2f` is the key
