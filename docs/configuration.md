# Configuration
The **config** folder holds a variety of settings to configure Limber and its services.

## .ini
Several **.ini** files containing configuration settings are placed in _limber/config_.
These settings will then be read and stored in the service container, using the **config** service, for easy access throughout the project.

### Custom Configurations
To establish new configurations, create a new config **.ini** file in _limber/config_ and it will be read by the config service automatically.

```ini
[test]
version = 1
```

Now the **test** section will be read from the **.ini** file when the web application starts and stored in the config service.

```python
test_config = config_service.get_section("test")
test_config["version"] # 1
```