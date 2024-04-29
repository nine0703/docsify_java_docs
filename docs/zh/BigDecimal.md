## BigDecimal常用API

### 构造函数

-   **BigDecimal(String val)**: 构造一个`BigDecimal`，其值由`String`参数给出。这是首选构造方法，因为它可以防止由于浮点数的精度问题而发生的意外。

    ```
    使用的str类型构造
    ```

-   **BigDecimal(double val)**: 构造一个`BigDecimal`，其值由`double`参数给出。但是，由于`double`的精度问题，这通常不是推荐的方法。

    ```java
    BigDecimal bd = new BigDecimal(<double>)
    ```

-   **BigDecimal(BigInteger val)**: 构造一个`BigDecimal`，其值由`BigInteger`参数给出。

### 数学运算

-   **add(BigDecimal augend)**: 返回`this`加`augend`的结果。

    ```java
    返回的是一个添加后的对象，比如
    BigDecimal sum = new BigDecimal(<str>)
    sum = sum.add(str)  // sum本体添加str字符串的数据后，返回一个相同类型的，添加后的对象
    ```

-   **subtract(BigDecimal subtrahend)**: 返回`this`减`subtrahend`的结果。

-   **multiply(BigDecimal multiplicand)**: 返回`this`乘`multiplicand`的结果。

-   **divide(BigDecimal divisor, int scale, RoundingMode roundingMode)**: 返回`this`除以`divisor`的结果，其中`scale`指定结果的小数点后的位数，`roundingMode`指定舍入模式。

### 比较

-   **compareTo(BigDecimal val)**: 比较`this`和`val`。如果`this`大于、等于或小于`val`，则返回正数、零或负数。

    ```
    可以用于比较数据是否相同
    if ( val.compareTo(val2) == 0 )
    ```

-   **equals(Object x)**: 检查`this`是否等于`x`。

### 转换

-   **doubleValue()**: 将`BigDecimal`转换为`double`。
-   **floatValue()**: 将`BigDecimal`转换为`float`。
-   **longValue()**: 将`BigDecimal`转换为`long`。
-   **intValue()**: 将`BigDecimal`转换为`int`。
-   **toBigInteger()**: 将`BigDecimal`转换为`BigInteger`。
-   **toString()**: 将`BigDecimal`转换为`String`。

### 其他常用方法

-   **setScale(int newScale, RoundingMode roundingMode)**: 设置`BigDecimal`的小数点后的位数，并指定舍入模式。
-   **abs()**: 返回`this`的绝对值。
-   **negate()**: 返回`this`的相反数。
-   **pow(int n)**: 返回`this`的`n`次幂。
-   **round(newScale, roundingMode)**: 使用指定的舍入模式将`BigDecimal`舍入到指定的小数位数。

请注意，这只是`BigDecimal`类的一小部分常用方法。`BigDecimal`类提供了丰富的功能来处理精确的浮点数运算，包括各种舍入模式、比例缩放等。为了获得更完整和详细的信息，建议查阅Java官方文档或`BigDecimal`类的源代码。