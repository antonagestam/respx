# Mocking HTTPX

To mock out `HTTPX`, use the `respx.mock` decorator / context manager.


## Using the Decorator

``` python
import httpx
import respx


@respx.mock
def test_something():
    request = respx.get("https://foo.bar/", content="foobar")
    response = httpx.get("https://foo.bar/")
    assert request.called
    assert response.status_code == 200
    assert response.text == "foobar"
```

## Using the Context Manager

``` python
import httpx
import respx


with respx.mock:
    request = respx.get("https://foo.bar/", content="foobar")
    response = httpx.get("https://foo.bar/")
    assert request.called
    assert response.status_code == 200
    assert response.text == "foobar"
```

## Async Support

Both decorator and context manager supports async contexts and the `HTTPX` async client.

``` python
@respx.mock
async def test_something():
    ...
    async with httpx.AsyncClient() as client:
        response = await client.get(...)
```

``` python
async with respx.mock:
    ...
    async with httpx.AsyncClient() as client:
        response = await client.get(...)
```

## Advanced Usage

Use `respx.mock` *without* parentheses for **global** scope, or *with* parentheses for **local** scope and configurable checks.
For more details on checks, see RESPX [Built-in Assertions](api.md#built-in-assertions).

!!! note "NOTE"
    You can also start and stop mocking `HTTPX` manually, by calling `respx.start()` and `respx.stop()`.