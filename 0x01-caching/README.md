# 0x01. Caching
## The Domains/Concepts covered in this project: `Back-end`

This project introduced me to caching in backend development, a critical technique for improving application performance and reducing server load. I learned how to implement caching strategies using tools like Redis, configure cache expiration policies, and optimize data retrieval for faster and more efficient responses.

## More Info
**Parent class** `BaseCaching`
All your classes must inherit from `BaseCaching` defined below:

```
$ cat base_caching.py
#!/usr/bin/python3
""" BaseCaching module
"""

class BaseCaching():
    """ BaseCaching defines:
      - constants of your caching system
      - where your data are stored (in a dictionary)
    """
    MAX_ITEMS = 4

    def __init__(self):
        """ Initiliaze
        """
        self.cache_data = {}

    def print_cache(self):
        """ Print the cache
        """
        print("Current cache:")
        for key in sorted(self.cache_data.keys()):
            print("{}: {}".format(key, self.cache_data.get(key)))

    def put(self, key, item):
        """ Add an item in the cache
        """
        raise NotImplementedError("put must be implemented in your cache class")

    def get(self, key):
        """ Get an item by key
        """
        raise NotImplementedError("get must be implemented in your cache class")
```

**0. Basic dictionary**

Create a class `BasicCache` that inherits from `BaseCaching` and is a caching system:

  * You must use `self.cache_data` - dictionary from the parent class `BaseCaching`
  * This caching system doesn’t have limit
  * `def put(self, key, item):`
    * Must assign to the dictionary `self.cache_data` the `item` value for the key `key`.
    * If `key` or `item` is `None`, this method should not do anything.
  * `def get(self, key):`
    * Must return the value in `self.cache_data` linked to `key`.
    * If `key` is `None` or if the `key` doesn’t exist in `self.cache_data`, return `None`.

```
guillaume@ubuntu:~/0x01$ cat 0-main.py
#!/usr/bin/python3
""" 0-main """
BasicCache = __import__('0-basic_cache').BasicCache

my_cache = BasicCache()
my_cache.print_cache()
my_cache.put("A", "Hello")
my_cache.put("B", "World")
my_cache.put("C", "ALX")
my_cache.print_cache()
print(my_cache.get("A"))
print(my_cache.get("B"))
print(my_cache.get("C"))
print(my_cache.get("D"))
my_cache.print_cache()
my_cache.put("D", "School")
my_cache.put("E", "Battery")
my_cache.put("A", "Street")
my_cache.print_cache()
print(my_cache.get("A"))

guillaume@ubuntu:~/0x01$ ./0-main.py
Current cache:
Current cache:
A: Hello
B: World
C: ALX
Hello
World
ALX
None
Current cache:
A: Hello
B: World
C: ALX
Current cache:
A: Street
B: World
C: ALX
D: School
E: Battery
Street
guillaume@ubuntu:~/0x01$ 
```

  * [0-basic_cache.py](./0-basic_cache.py)
