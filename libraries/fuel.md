## Dependencies Gradle

```
dependencies {
    ...
    compile 'com.github.kittinunf.fuel:fuel:2.2.0'
}
```

## Implementation

```kt
class PaymentsResponseException(cause: Throwable?) : Exception(cause)

class PaymentsClient {
    fun sendInfo(info: String, retrycount: Int = 0) {
        val httpAsync = "${configuration.url}/log"
            .httpPost()
            .header("Content-Type", "application/json")
            .body(
                """{
                    |'some': ${info},
                    |}""".trimMargin()
            )
            .responseString { _, _, result ->
                when (result) {
                    is Result.Failure -> {
                        if (retrycount >= configuration.maxRetries) {
                            throw PaymentsResponseException(result.getException())
                        }
                        validatePayment(payment, retrycount + 1)
                    }
                    is Result.Success -> {
                        println(result)
                    }
                }
            }

        httpAsync.join()
    }
}

```