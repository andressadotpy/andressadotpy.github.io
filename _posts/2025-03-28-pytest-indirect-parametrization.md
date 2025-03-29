---
layout: post
title: Passing parameters to a fixture
subtitle: Something cool I learned at work today
tags: [Python, Pytest, Test]
---

Just when I thought parametrization with Pytest couldn't get any cooler: :boom:, I learned about using parametrization to make pytest fixtures more dynamic and flexible.

I am writing a pytest plugin to use in a new test automation. In our team, we usually ensure that each test has its own user associated with it because re-using users might introduce too much instability. So I started to experiment with fixtures to understand how we could setup and login as different users in each test.

At first, I thought the only way to do it was creating multiple chained fixtures. One fixture to setup a user, another fixture to add stuff to a user (to make it different) and one login fixture for each different type of user. Something like:

```python
@pytest.fixture
def setup_user():
    # Code to setup a user

@pytest.fixture
def add_stuff_1_to_user(setup_user):
    # Code to add stuff 1 to user

@pytest.fixture
def login_as_user_with_stuff_1(add_stuff_1_to_user, setup_user):
    # Code to login as user with stuff 1

@pytest.fixture
def add_stuff_2_to_user(setup_user):
    # Code to add stuff 2 to user

@pytest.fixture
def login_as_user_with_stuff_2(add_stuff_2_to_user, setup_user):
    # Code to login as user with stuff 2
```

And inside a test would be something like:

```python
def test_if_user_can_access_that_page(login_as_user_with_stuff_1):
    # Code to test in here

def test_if_another_user_can_access_this(login_as_user_with_stuff_2):
    # Code to test in here
```

Sure you can see why this could go wrong really fast :satisfied:

I wanted **one login fixture** and one only. So I needed a generic fixture that could add different stuff to a user by receiving the parameters to add to each user from the test function.

## @pytest.mark.parametrize and indirect=True

From the [documentation](https://docs.pytest.org/en/stable/example/parametrize.html#indirect-parametrization):
> Using the `indirect=True` parameter when parametrizing a test allows to parametrize a test with a fixture receiving the values before passing them to a test

This means: instead of using `@pytest.mark.parametrize` like we usually do, to pass values for a test, we can **pass the values as input to a fixture**, giving me the possibility to have one generic fixture receiving parameters and adding this parameters to a user to change the user.

So I could have something like:

```python
@pytest.fixture
def setup_user():
    print("Setting up a user.")

@pytest.fixture
def add_stuff_to_user(setup_user, request):
    stuff = request.param if hasattr(request, 'param') else None
    print(f"This is the stuff: {stuff}")

@pytest.fixture
def login(setup_user, add_stuff_to_user):  
    print("Logging in")

# Example test using the login fixture with different stuff being passed
@pytest.mark.parametrize('add_stuff_to_user', ['STUFF1', 'STUFF2', 'STUFF3'], indirect=True)
def test_login_with_different_skus(login):
    1 == 1 

# Example test using the login fixture without any stuff being added
@pytest.mark.parametrize('add_stuff_to_user', [None], indirect=True)
def test_login_without_sku(login):
    assert 2 == 2    
```

Running these tests to check what is being called we can see:

```python
test_fixtures.py Setting up a user.
This is the stuff: STUFF1
Logging in
.Setting up a user.
This is the stuff: STUFF2
Logging in
.Setting up a user.
This is the stuff: STUFF3
Logging in
.Setting up a user.
This is the stuff: None
Logging in

```

## Conclusion

In pytest, the `indirect=True` parameter in the `@pytest.mark.parametrize` decorator indicates that the values provided for the parameter should not be passed directly to the test function. Instead, these values should be used to call the corresponding fixture with those values as input.

When `indirect=True` is used, pytest will treat the parameter name as a reference to a fixture and pass the value to that fixture. The fixture can then use the value as needed, and the result of the fixture will be passed to the test function.
