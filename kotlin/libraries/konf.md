## Dependencies Gradle

```
dependencies {
    ...
    compile 'com.uchuhimo:konf:0.22.1'
}
```

## Implementation

```kt

object toggles : ConfigSpec() {
    val someToggle by required<Boolean>()
}

object FileConfig {
    val config = Config {
        addSpec(toggles)
    }
        .enable(Feature.OPTIONAL_SOURCE_BY_DEFAULT)
        .from.hocon.resource("application.conf")
        .from.hocon.resource("${System.getProperty("config.env")}.conf")
        .from.env()
}

object ToggleConfiguration {
    val someToggle = FileConfig.config[toggles.someToggle]
}

```