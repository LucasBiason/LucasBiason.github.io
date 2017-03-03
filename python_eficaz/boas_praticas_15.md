
Why Is It Important to Write Documentation or Inline Comments?

Many developers think they are saving time by avoiding writing comments with a semi-obfuscated code. Or, they think avoiding docstrings will help them meet deadlines. Stay assured within a short period of time you’ll hate yourself when you will not remember what and why you did something in the way you did while reading your own code.

In the future, probably you will leave the company, and your code will haunt all the members of your team who will come across this zombie-like code. There is just no excuse to not write a documentation. Writing a doc-strings and comments on complex code sections don’t take that too much time. The other way to approach writing your code is to name your functions, methods and variables to reflect the purpose of the component, making them “self-documented”.

Here is a nice guide on how to properly documente your Python code.

http://docs.python-guide.org/en/latest/writing/documentation/
https://docs.python.org/2/library/doctest.html

Properly Structure Your Repository

Most developers, no matter the language, will begin a new project by settings up a code repository and some form of version control.

While endless debates can (and do) take place arguing about which version control system is best or where to host your project repositories, no matter what you decide to use, it’s critical to focus on structuring your project and subsequent repository in a suitable fashion.

For Python, in particular, there are a few key components that should exist within your repository, and you should take the time to generate each of these, at least in a basic skeletal form, before actual coding begins.

    License – [root]: This file should be the first thing you add to your project. If you’re uncertain, sites like choosealicense.com can help you decide.
    README – [root]: Whether you choose Markdown, reStructuredText, or even just plain text as your format, getting a basic README file in place can help you describe your project, define its purpose, and outline the basic functionality.
    Module Code – [root] or [/module]: Of course this is all for naught if you don’t have any new or worthwhile code to create. That said, be sure to place your actual code inside a properly-named sub-directory (e.g. /module), or for a single-file project, within root itself.
    setup.py – [root]: A basic setup.py script is very common in Python projects, allowing Distutils to properly build and distribute the modules your project needs. See the official documentation for more information on setup.py.
    requirements.txt – [root]: While not all projects utilize a requirements.txt file, it can be used to specify development modules and other dependencies that are required to work on the project properly. See the official PIP page for details.
    Documentation – [/docs]: Documentation is a key component to a well-structured project, and typically it is placed in the /docs directory.
    Tests – [/tests]: Just like documentation, tests also typically exist in most projects, and should be placed in their own respective directory.

All told, this means a basic Python repository structure might look something like this:
docs/conf.py
docs/index.rst
module/__init__.py
module/core.py
tests/core.py
LICENSE
README.rst
requirements.txt
setup.py
	
  Create Consistent Documentation

As much as it can feel like a burden at times, proper documentation throughout the lifecycle of a project is a cornerstone to clean code. Thankfully, the Python community has made this process fairly painless, and it involves the use of three simple tools and concepts: reStructredText, Docstrings, and Sphinx.

reStructredText is a simple plain text markup syntax. It is commonly used for in-line documentation, which in the case of Python, allows for documentation to be generated on-the-fly. The Quickstart Guide provides a few simple examples of reStructuredText in action.

While using reStructuredText is the first step to proper documentation, it’s critical to understand how to properly generate Docstrings. A Docstring is simply a documentation string that appears prior to every module, class, or method within your Python code.

Once every component of your code contains a properly formatted Docstring using reStructuredText markdown, you can move onto using Sphinx, which is the go-to tool to generate Python documentation from existing reStructuredText.Sphinx allows documentation to easily be exported into a variety of formats, including beautiful HTML, for near-automatic online documentation pages. With virtually no effort, your project documentation can even be added to the prominent ReadTheDocs documentation repository after every code commit.
