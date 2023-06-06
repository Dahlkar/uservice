# uService
A async microservice framework for Python. Inspired by [FastAPI](https://github.com/tiangolo/fastapi) and [Nameko](https://github.com/nameko/nameko).


## Requirements

Python 3.10+

## Example

``` python
from uservice import Service, EventPublisher, Depends
from typing import Annotated
from pydantic import BaseModel


class Payload(BaseModel):
    foo: int
    bar: str


publisher = EventPublisher()


service_a = Service(name="service_a")


@service_a.rpc()
def dispatch(
        input: Payload,
        publish: Annotated[Callable, Depends(publisher)],
) -> NoReturn:
    publish("event", payload)


service_b = Service(name="service_b")


@service_b.event_handler("service_a", "event")
def handle(
        payload: Payload,
) -> NoReturn:
    print(payload)
```

