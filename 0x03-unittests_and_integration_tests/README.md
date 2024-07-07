# Unit and Integration Tests

This project focuses on learning to write unit tests and integration tests using Python 3.

## Tasks to Complete

+ [x] 0. **Parameterize a unit test**<br/>The [test_utils.py](test_utils.py) module includes the following:
  + Understand the `utils.access_nested_map` function by experimenting with it in the Python console.
  + Write the initial unit test for `utils.access_nested_map`.
  + Create a `TestAccessNestedMap` class inheriting from `unittest.TestCase`.
  + Implement the `TestAccessNestedMap.test_access_nested_map` method to ensure it returns the expected results.
  + Use `@parameterized.expand` to test the function with these inputs:
    ```python
    nested_map={"a": 1}, path=("a",)
    nested_map={"a": {"b": 2}}, path=("a",)
    nested_map={"a": {"b": 2}}, path=("a", "b")
    ```
  + Use `assertEqual` to verify the function's output matches the expected results.
  + Ensure the test method's body is no longer than 2 lines.

+ [x] 1. **Parameterize a unit test for exceptions**<br/>The [test_utils.py](test_utils.py) module includes the following:
  + Implement the `TestAccessNestedMap.test_access_nested_map_exception` method. Use the `assertRaises` context manager to check that a `KeyError` is raised for the following inputs (use `@parameterized.expand`):
    ```python
    nested_map={}, path=("a",)
    nested_map={"a": 1}, path=("a", "b")
    ```
  + Verify that the exception message is as expected.

+ [x] 2. **Mock HTTP calls**<br/>The [test_utils.py](test_utils.py) module includes the following:
  + Understand the `utils.get_json` function.
  + Create a `TestGetJson` class inheriting from `unittest.TestCase`.
  + Implement the `TestGetJson.test_get_json` method to test that `utils.get_json` returns the expected result.
  + Use `unittest.mock.patch` to patch `requests.get`, returning a `Mock` object with a `json` method that returns `test_payload`. Parameterize the `test_payload` and `test_url` with the following inputs:
    ```python
    test_url="http://example.com", test_payload={"payload": True}
    test_url="http://holberton.io", test_payload={"payload": False}
    ```
  + Test that the mocked `get` method is called exactly once (per input) with `test_url` as the argument.
  + Verify that the output of `get_json` matches `test_payload`.

+ [x] 3. **Parameterize and patch**<br/>The [test_utils.py](test_utils.py) module includes the following:
  + Understand the `utils.memoize` decorator.
  + Create a `TestMemoize` class inheriting from `unittest.TestCase` with a `test_memoize` method.
  + Inside `test_memoize`, define the following class:
    ```python
    class TestClass:

        def a_method(self):
            return 42

        @memoize
        def a_property(self):
            return self.a_method()
    ```
  + Use `unittest.mock.patch` to mock `a_method`. Verify that calling `a_property` twice returns the correct result but `a_method` is called only once using `assert_called_once`.

+ [x] 4. **Parameterize and patch as decorators**<br/>The [test_client.py](test_client.py) module includes the following:
  + Understand the `client.GithubOrgClient` class.
  + Create a `TestGithubOrgClient` class inheriting from `unittest.TestCase` and implement the `test_org` method.
  + Test that `GithubOrgClient.org` returns the correct value.
  + Use `@patch` to ensure `get_json` is called once with the expected argument but not executed.
  + Use `@parameterized.expand` to parameterize the test with the following `org` examples:
    ```python
    google
    abc
    ```
  + Ensure no external HTTP calls are made.

+ [x] 5. **Mocking a property**<br/>The [test_client.py](test_client.py) module includes the following:
  + Understand how `memoize` turns methods into properties and how to mock a property.
  + Implement the `test_public_repos_url` method to unit-test `GithubOrgClient._public_repos_url`.
  + Use `patch` as a context manager to patch `GithubOrgClient.org` to return a known payload.
  + Verify that the result of `_public_repos_url` matches the expected one based on the mocked payload.

+ [x] 6. **More patching**<br/>The [test_client.py](test_client.py) module includes the following:
  + Implement `TestGithubOrgClient.test_public_repos` to unit-test `GithubOrgClient.public_repos`.
  + Use `@patch` to mock `get_json` to return a chosen payload.
  + Use `patch` as a context manager to mock `GithubOrgClient._public_repos_url` and return a chosen value.
  + Verify that the list of repos matches the expected payload.
  + Ensure the mocked property and `get_json` are called once.

+ [x] 7. **Parameterize**<br/>The [test_client.py](test_client.py) module includes the following:
  + Implement `TestGithubOrgClient.test_has_license` to unit-test `GithubOrgClient.has_license`.
  + Parameterize the test with the following inputs:
    ```python
    repo={"license": {"key": "bsd-3-clause"}}, license_key="bsd-3-clause"
    repo={"license": {"key": "bsl-1.0"}}, license_key="bsd-3-clause"
    ```
  + Also parameterize the expected returned value.

+ [x] 8. **Integration test: fixtures**<br/>The [test_client.py](test_client.py) module includes the following:
  + Create the `TestIntegrationGithubOrgClient` class inheriting from `unittest.TestCase` and implement `setUpClass` and `tearDownClass`.
  + Use `@parameterized_class` to decorate and parameterize the class with fixtures found in [fixtures.py](fixtures.py):
    ```python
    org_payload, repos_payload, expected_repos, apache2_repos
    ```
  + In `setUpClass`, mock `requests.get` to return example payloads found in the fixtures.
  + Use `patch` to start a patcher named `get_patcher` and use `side_effect` to ensure `requests.get(url).json()` returns the correct fixtures based on the anticipated `url` values.
  + Implement `tearDownClass` to stop the patcher.

+ [x] 9. **Integration tests**<br/>The [test_client.py](test_client.py) module includes the following:
  + Implement the `test_public_repos` method to test `GithubOrgClient.public_repos`.
  + Ensure the method returns the expected results based on the fixtures.
  + Implement `test_public_repos_with_license` to test `public_repos` with the argument `license="apache-2.0"` and ensure the result matches the expected value from the fixtures.
