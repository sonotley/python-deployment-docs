# Writing tests

## Use pytest
Unit testing is an area I'm still learning. I initially based my tests on [this excellent article](https://blog.miguelgrinberg.com/post/how-to-write-unit-tests-in-python-part-1-fizz-buzz), particularly [part 2](https://blog.miguelgrinberg.com/post/how-to-write-unit-tests-in-python-part-2-game-of-life). However I found that two of its key recommendations, inheriting from `unittest.TestCase` and use of the `parameterized` package, seemed to make things harder when I came to do more complicated tests. For example it was difficult to find any help on how to use `unittest.mock.patch` (or my current choice, `pytest-mock`, which wraps this into `mocker`) with parameterized. Sticking within the pytest package has made things easier in this regard.

Therefore, my current approach to unit tests is to write bare functions (no classes) with plain `assert` statements. If I need to parameterize a function I use `pytest.parametrize` and if I need to patch something I use `mocker` from pytest-mock. For other mocking I use the classic `unittest.MagicMock`.

Where possible I try to write code that avoids the need for patching during tests. I use dependency injection or callbacks such that if function A needs to call function B, function B is passed to function A as an argument. For example, the code below makes is easy to have multiple `data_getters` perhaps one for disk one for database, one for remote API etc. but it also makes it very easy to pass a mock getter for unit testing. Of course I don't do this absolutely everywhere, just in places where is helps.

``` python linenums="1"
def read_data_from_disk(selection):
    # some code to find the data etc

def get_data(data_getter: Callable):
    # lots of other stuff
    selection = input("Select your data: ")
    data = data_getter(selection)
    # lots more stuff

get_data(read_data_from_disk)
```

## Test dependencies

To let me quickly run my tests while developing I like to install my test dependencies into my dev environment using `poetry add --dev`. This adds them to `pyproject.toml` show shown.

```ini linenums=1
[tool.poetry.dev-dependencies]
pytest = "^6.2.5"
pytest-cov = "^2.12.1"
pytest-mock = "^3.6.1"
```