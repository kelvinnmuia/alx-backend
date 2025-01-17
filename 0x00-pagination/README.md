# 0x00. Pagination
## The Domains/Concepts covered in this project: `Back-end`

This project introduced me to implementing pagination in backend development, a crucial technique for managing and delivering large datasets efficiently. I learned how to design and implement pagination strategies, such as limit-offset and cursor-based pagination, to optimize API performance and enhance user experience.

**0. Simple helper function**

Write a function named `index_range` that takes two integer arguments `page` and `page_size`.

The function should return a tuple of size two containing a start index and an end index corresponding to the range of indexes to return in a list for those particular pagination parameters.

Page numbers are 1-indexed, i.e. the first page is page 1.

```
bob@dylan:~$ cat 0-main.py
#!/usr/bin/env python3
"""
Main file
"""

index_range = __import__('0-simple_helper_function').index_range

res = index_range(1, 7)
print(type(res))
print(res)

res = index_range(page=3, page_size=15)
print(type(res))
print(res)

bob@dylan:~$ ./0-main.py
<class 'tuple'>
(0, 7)
<class 'tuple'>
(30, 45)
bob@dylan:~$
```

  * [0-simple_helper_function.py](./0-simple_helper_function.py)
