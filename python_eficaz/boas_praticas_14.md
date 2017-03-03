
   
Feel the Power of the Python Logic Operands.

Python’s logical operations don’t return just Boolean values True or False, they can also return the actual value of the operations.

This power of the Python logic operands can be used to speed up development and to increase code readability. In the following example, object will be fetched from the cache, or if it’s missed in the cache it will be fetched from the database:

# check_in_cache returns object or None
def get_obj(): 
return check_in_cache() or pull_from_db()

A quick explanation of the provided example: it will first try to get an object from cache (check_in_cache() function). If it doesn’t return an object and returns a None instead, it will get it from the database (pull_from_db() function). Written in this way is better than the following code snippet, written in a standard way:

def get_obj():
result = check_in_cache()
if result is None:
  		result = pull_from_db()
	return result

Our first code example solves a problem in one line of the code, which is better than four lines of code from the second code example. Not to mention the first code example is more expressive and readable.

Just one thing to watch for - you should be aware of returning objects with logical equivalent of False, like an empty lists for example. If check_in_cache() function returns such an object, it will be treated as missing, and will cause your app to call a pull_from_db function. So, in cases where your functions could be returning these kind of objects, consider using additional explicit is None check.

   for number in numbers:
       result.setdefault(number, 0)  # this is clearer
       result[number] += 1
   return result

We can also use the dictionary to manipulate lists:

>>> characters = {'a': 1, 'b': 2}
>>> characters.items() // return a copy of a dictionary’s list of (key, value) pairs (https://docs.python.org/2/library/stdtypes.html#dict.items)
[('a', 1), ('b', 2)]

>>> characters = [['a', 1], ['b', 2], ['c', 3]]
>>> dict(characters) // return a new dictionary initialized from a list (https://docs.python.org/2/library/stdtypes.html#dict)
{'a': 1, 'b': 2, 'c': 3}

If necessary, it is easy to change a dictionary form by switching the keys and values – changing {key: value} to a new dictionary {value: key} – also known as inverting the dictionary:

>>> characters = {'a': 1, 'b': 2, 'c': 3}
>>> invert_characters = {v: k for k, v in characters.iteritems()}
>>> invert_characters
{1: 'a', 2: 'b', 3: 'c'}

The final tip is related to exceptions. Developers should watch out for exceptions. One of the most annoying is the KeyError exception. To handle this, developers must first check whether or not a key exists in the dictionary.

>>> character_occurrences = {'a': [], ‘b’: []}
>>> character_occurrences[‘c’]
KeyError: 'c'
>>> if ‘c’ not in character_occurrences:
        character_occurrences[‘c’] = []
>>> character_occurrences[‘c’]
[]
>>> try:
        print character_occurrences[‘d’]
    except: 
        print “There is no character `d` in the string”

However, to achieve clean and easily testable code, avoiding exceptions catch and conditions is a must. So, this is cleaner and easier to understand if the defaultdict, in collections, is used.

>>> from collections import defaultdict
>>> character_occurrences = defaultdict(list)
>>> character_occurrences['a']
[]
>>> character_occurrences[‘b’].append(10)
>>> character_occurrences[‘b’].append(11)
>>> character_occurrences[‘b’]
[10, 11]
