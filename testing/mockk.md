## Dependencies Gradle

```
dependencies {
    ...
    testCompile group: 'org.junit.jupiter', name: 'junit-jupiter-api', version: '5.6.2'
    testRuntimeOnly group: 'org.junit.jupiter', name: 'junit-jupiter-engine', version: '5.6.2'
    testImplementation 'io.mockk:mockk:1.10.0'
    ...
}
```

## Implementation

```kt

class SomeClass{
    fun doSomething(): String {}
}
class SavePaymentsProcessorTest {
    @MockK
    private lateinit var someClass: SomeClass

    @BeforeEach
    fun setup() {
        someClass = mockk(relaxed = true)
    }

    @Test
    fun shouldReturnSomething() {
        val subject = Subject(someClass)
        every { someClass.doSomething() } return hello
        subject.run()
        Assertions.assertEquals(result?.value, "hello")
        verify { someClass.doSomething() }
    }
}


```