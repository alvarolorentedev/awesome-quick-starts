## Dependencies Gradle

```
dependencies {
    ...
    testCompile group: 'org.junit.jupiter', name: 'junit-jupiter-api', version: '5.6.2'
    testRuntimeOnly group: 'org.junit.jupiter', name: 'junit-jupiter-engine', version: '5.6.2'
    testCompile 'com.github.tomakehurst:wiremock-standalone:2.26.3'
}
```

## Implementation

```kt

internal class Test {
    private val port = 8997
    private val url = "localhost"
    private val wiremock = WireMockServer(WireMockConfiguration.wireMockConfig().port(port))
    private lateinit var subject: Client

    @BeforeEach
    fun beforeEach() {
        wiremock.start()
        wiremock.resetAll()
        WireMock.configureFor(url, port)
        val logConfiguration: LogConfiguration = mockk(relaxed = true)
        every { logConfiguration.url } returns "http://localhost:8997"
        subject = Client(logConfiguration)
    }

    @AfterEach
    fun afterEach() {
        wiremock.stop()
    }

    
    @Test
    fun shouldSendRequestToValidatePayment() {
        val data = "some"

        wiremock.stubFor(
            WireMock.post("/some")
                .withHeader("Content-Type", WireMock.equalTo("application/json"))
                .withRequestBody(
                    WireMock.equalTo(
                        """{
                        |'some': ${data}, 
                        |}""".trimMargin()
                    )
                )
                .inScenario("Scenario")
                .willReturn(WireMock.ok())
        )

        paymentsClient.validatePayment(payment)

        wiremock.verify(
            1,
            WireMock.postRequestedFor(WireMock.urlEqualTo("/some"))
                .withHeader("Content-Type", WireMock.equalTo("application/json"))
                .withRequestBody(
                    WireMock.equalTo(
                        """{
                        |'some': ${data}, 
                        |}""".trimMargin()
                    )
                )
        )
    }
}

```