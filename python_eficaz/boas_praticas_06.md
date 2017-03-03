
Testing

Write Unit Tests and Doctests.

Avoiding the writing of automated tests just for the sake of shipping quickly is a bad practice. Sooner or later, a bug will hit the surface, and it will happen on the production server, resulting with customer’s downtime. Just because of a “completely” manually-tested new feature, which will break something “almost” unrelated. Maybe after many sleepless nights of the development team, the bug will be found. But it will be too late.

Consequences can be catastrophic, a company can even lose millions and can be out of business.

Maybe this whole mess could be simply avoided if the developer would use best practices of writing unit tests and doctests. And after implementing a newly written feature, he would have run the tests once across the whole project.

The online book Dive into Python 3 has an excellent introduction on unit-testing. Another good start is in The Hitchhiker’s Guide to Python!.


Strive for 100% code coverage, but don't get obsess over the coverage score.
General testing guidelines

    Use long, descriptive names. This often obviates the need for doctrings in test methods.
    Tests should be isolated. Don't interact with a real database or network. Use a separate test database that gets torn down or use mock objects.
    Prefer factories to fixtures.
    Never let incomplete tests pass, else you run the risk of forgetting about them. Instead, add a placeholder like assert False, "TODO: finish me".

Unit Tests

    Focus on one tiny bit of functionality.
    Should be fast, but a slow test is better than no test.
    It often makes sense to have one testcase class for a single class or model.

import unittest
import factories

class PersonTest(unittest.TestCase):
    def setUp(self):
        self.person = factories.PersonFactory()

    def test_has_age_in_dog_years(self):
        self.assertEqual(self.person.dog_years, self.person.age / 7)

Functional Tests

Functional tests are higher level tests that are closer to how an end-user would interact with your application. They are typically used for web and GUI applications.

    Write tests as scenarios. Testcase and test method names should read like a scenario description.
    Use comments to write out stories, before writing the test code.

import unittest

class TestAUser(unittest.TestCase):

    def test_can_write_a_blog_post(self):
        # Goes to the her dashboard
        ...
        # Clicks "New Post"
        ...
        # Fills out the post form
        ...
        # Clicks "Submit"
        ...
        # Can see the new post
        ...

Notice how the testcase and test method read together like "Test A User can write a blog post".
https://docs.python.org/2/library/doctest.html
https://docs.python.org/2/library/unittest.html

###### Links úteis:

============================
:arrow_left: [Voltar](https://github.com/LucasBiason/PadroesPython/blob/master/python_eficaz/boas_praticas.md)
