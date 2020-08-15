# Configuration
The **config** folder holds a variety of settings to configure Limber and its services.

## .env
A **.env** file containing configuration settings can be placed at the root of the project.
These settings will then be read and stored in the service container, using the **config** service, for easy access throughout the project.

### Custom Configurations
To establish new configurations, create a new config class that inherits from _limberframework.config.config.BaseConfig_.

```python
from limberframework.config.config import BaseConfig

class RoutesConfig(BaseConfig):
    pass
```

Inside of the new config class, list the available settings as attributes using [Pydantic notation](https://pydantic-docs.helpmanual.io/usage/settings/).

```python
from typing import Optional
from limberframework.config.config import BaseConfig
from pydantic import Field

class RoutesConfig(BaseConfig):
    version: Optional[str] = Field(..., env='ROUTES_VERSION')
```

> _limberframework.config.config.BaseConfig_ inherits from _pydantic.BaseSettings_, therefore it receives its functionality as well.

Finally, register the new config class with the service container in _main.py_, _limber.main_.

```python
from limber.config.routes_config import RoutesConfig

...

# Load configurations
app['config']['app'] = AppConfig().dict()
app['config']['auth'] = auth
app['config']['cache'] = CacheConfig().dict()
app['config']['cors'] = cors
app['config']['database'] = DatabaseConfig().dict()
app['config']['routes'] = RoutesConfig().dict()

...
```

Now **ROUTES_VERSION** will be read from the _.env_ file when the web application starts and stored in the service container.

```python
app['config']['routes']['version']
```