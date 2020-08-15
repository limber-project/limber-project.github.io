# Database
[SQLAlchemy](https://www.sqlalchemy.org) is used to connect and interact with the DBMS.
The **database service provider** allows access to SQLAlchemy and registers several services with the service container to help with accessing the database, each are discussed below.

## Connection Service
A connection with the database can be established by requesting the **db.connection** service from the service container, i.e. Application.
This will create a new [Engine](https://docs.sqlalchemy.org/en/13/core/connections.html#sqlalchemy.engine.Engine) instance which can be used to interact with the database.

## Session Service
A session with the database can be established by requesting the **db.session** service from the service container, i.e. Application.
This will create a new [Session](https://docs.sqlalchemy.org/en/13/orm/session_basics.html#getting-a-session) instance which can be used to interact with the database.

## Alembic
Limber uses [Alembic](https://alembic.sqlalchemy.org/en/latest/) to version the database using migration scripts.
These migration scripts are stored under the **database/migrations** folder and the Alembic configuration file can be found at the root of the project, _alembic.ini_.

> If the location of the migration scripts is changed, update the **script_location** setting in _alembic.ini_ so that Alembic knows where to find them.

To create a new migration script, use the ```alembic revision``` command from the root of the project.
More information about creating a migration script can be found [here](https://alembic.sqlalchemy.org/en/latest/tutorial.html#create-a-migration-script).

## Models
Models allow for an association between a database table and Python class to be made and can be found under the **app/models** folder.

### Create a New Model
To establish a new model, create a new class that inherits from **Model**, _limberframework.database.models.Model_.

```python
from limberframework.database.models import Model

class Book(Model):
    pass
```

> **Model** is set as the [declarative base](https://docs.sqlalchemy.org/en/13/orm/extensions/declarative/api.html) for SQLAlchemy.

Next, describe the table that the class will be associated with using [SQLAlchemy notation](https://docs.sqlalchemy.org/en/13/orm/tutorial.html#create-a-schema).

```python
from limberframework.database.models import Model
from sqlalchemy import Column, Integer, String

class Book(Model):
    id = Column(Integer, primary_key=True)
    name = Column(String)
```

Finally, register the model in **config/app.py** under the models list.

```python
models = [
    'limber.app.models.user',
    'limber.app.models.book'
]
```

### Soft Delete
To prevent a record from being permanently removed from the database, the _soft\_delete_ attribute, along with the _delete\_at_ attribute, can be set on a model.

```python
class Book(Model):
    id = Column(Integer, primary_key=True)
    name = Column(String)
    deleted_at = Column(DateTime, nullable=True)
    soft_delete = True
```

Now, when the delete() method is called on Book, the deleted_at field will be updated with the current time and queries will filter out this model.
