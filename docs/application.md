# Application
The **Application** class, _limberframework.foundation.application.Application_, is responsible for registering the available services, creating instances of services when needed, and monitoring the instances, i.e. establishing the service container.
An example of how to establish a new instance of **Application** is shown below:

```python
from limberframework.foundation.application import Application

app = Application()
```

> **Application** inherits from the **FastAPI** class, _fastapi.applications.FastAPI_, and, therefore, receives its functionality as well.

## Services
**Services** provide functionality to the web application and can be registered with **Application** using a **Service Provider**, _limberframework.support.service_providers.ServiceProvider_.
A **Service Provider** has a register() method which is called by **Application**.
This method should bind services to the service container using **Application's** bind() method, passing a closure that will be called to create the service when required.
The closure will have access to **Application**, and all services that have been registered with **Application**, as an argument.
An example is shown below:

```python
from limberframework.foundation.application import Application
from limberframework.database.connections import Connection, make_connection
from limberframework.support.service_provider import ServiceProvider

# Create a service provider that provides the database service.
class DatabaseServiceProvider(ServiceProvider):
    def register(self) -> None:
        # Closure to create the database service when required.
        def register_database(app: Application):
            return make_connection(app['config']['database'])

        # Bind the service to the service container.
        self.app.bind('database', register_database)

# Create an instance of the database service provider.
database_service_provider = DatabaseServiceProvider(app)

# Register the services provided by the database
# service provider with the service container.
app.register(database_service_provider)
```

The database service can be requested from **Application** as follows:

```python
database_service = app.make('database')
# or
database_service = app['database']
```

### Singletons
A service can be register with **Application** as a singleton, i.e. only having one instance of the service in existence at any time.
In this case, **Application** will monitor all created instances of the service and provide an existing instance if available.
Setting a service to a singleton can be achieved by setting the `singleton` argument of **Application's** bind() method to `True`.

```python
# Bind the database service as a singleton to the service container.
self.app.bind('database', register_database, singleton=True)
```

### Deferred
**Application** will pre-load services on start-up of the web application to reduce overhead when handling a request.
To prevent a service from being pre-loaded, for example when a fresh instance of a service is required for each request, the service can be set to deferred.
This can be achieved by setting the `defer` argument of **Application's** bind() method to `True`.

```python
# Bind the database service to the service container and defer loading.
self.app.bind('database', register_database, defer=True)
```

### Custom Service
If you would like to create a service and register it with **Application**, establish a new service provider that inherits from **ServiceProvider**, _limberframework.support.service_providers.ServiceProvider_, and implements the required methods.
Also, to let **Application** know that the service is available, add it to the list of service providers in  app's configuration, _limber.config.app.service\_providers_.

```python
service_providers = [
    'limberframework.authentication.authentication_service_provider.AuthServiceProvider',
    'limberframework.cache.cache_service_provider.CacheServiceProvider',
    'limberframework.config.config_service_provider.ConfigServiceProvider',
    'limberframework.database.database_service_provider.DatabaseServiceProvider',
    'custompackage.custom.custom_service_provider.CustomServiceProvider'
]
```
