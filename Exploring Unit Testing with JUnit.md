# Overview
There are two types of a test - unit test & system level test.
Junit is for unit testing.

# First Unit Test
Tests are created under the `test` directory. The `test` directory has the same structure as `src` directory.
```java
public class MyMathTest {
    @Test
    public void test() {
        MyMath math = new MyMath();
        int[] numbers = {1, 7, 2};
        int result = math.calculateSum(numbers);
        int expectedResult = 5;
        assertEquals(expectedResult, result);
    }
}
```

```java
package com.azmalakhtar.junit;
import static org.junit.jupiter.api.Assertions.*;
import org.junit.Test;

public class MyMathTest {
    private MyMath math = new MyMath();
    @Test
    public void calculateSum_ThreeMemberArray() {
        assertEquals(10, math.calculateSum(new int[]{1, 7, 2}));
    }

    @Test
    public void calculateSum_EmptyArray() {
        assertEquals(0, math.calculateSum(new int[]{}));
    }
}
```

```java
package com.azmalakhtar.junit;

public class MyMath {
    public int calculateSum(int[] numbers) {
        int sum = 0;
        for (int number : numbers) {
            sum += number;
        }
        return sum;
    }
}
```

`@AfterEach`
`@BeforeEach`
`@BeforeAll`
`@AfterAll`

---
### References
- [Master Spring Boot 3 & Spring Framework 6 with Java](Master%20Spring%20Boot%203%20&%20Spring%20Framework%206%20with%20Java.md)