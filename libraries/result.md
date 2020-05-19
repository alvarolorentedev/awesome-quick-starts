## Dependencies Gradle

```
dependencies {
    ...
    compile 'com.github.kittinunf.result:result:3.0.0'
}
```

## Implementation

```kt
class Something {
    fun doSomenthing() {
        Result.of<Unit, Exception> {
                if (ToggleConfiguration.nameOfToggle) {
                    return "ok"
                }
                else {
                    throw Exception()
                }
            }.fold(
                {
                    println("all fine")
                },
                {
                    println("there was an exception")
                }
            )
    }
}

```