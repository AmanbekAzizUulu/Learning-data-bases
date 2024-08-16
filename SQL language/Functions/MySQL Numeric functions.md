### 14.6.2 Mathematical Functions

**Table 14.10 Mathematical Functions**

|Name|Description|
|---|---|
|[`ABS()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_abs)|Return the absolute value|
|[`ACOS()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_acos)|Return the arc cosine|
|[`ASIN()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_asin)|Return the arc sine|
|[`ATAN()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_atan)|Return the arc tangent|
|[`ATAN2()`, `ATAN()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_atan2)|Return the arc tangent of the two arguments|
|[`CEIL()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_ceil)|Return the smallest integer value not less than the argument|
|[`CEILING()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_ceiling)|Return the smallest integer value not less than the argument|
|[`CONV()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_conv)|Convert numbers between different number bases|
|[`COS()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_cos)|Return the cosine|
|[`COT()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_cot)|Return the cotangent|
|[`CRC32()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_crc32)|Compute a cyclic redundancy check value|
|[`DEGREES()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_degrees)|Convert radians to degrees|
|[`EXP()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_exp)|Raise to the power of|
|[`FLOOR()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_floor)|Return the largest integer value not greater than the argument|
|[`LN()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_ln)|Return the natural logarithm of the argument|
|[`LOG()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_log)|Return the natural logarithm of the first argument|
|[`LOG10()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_log10)|Return the base-10 logarithm of the argument|
|[`LOG2()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_log2)|Return the base-2 logarithm of the argument|
|[`MOD()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_mod)|Return the remainder|
|[`PI()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_pi)|Return the value of pi|
|[`POW()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_pow)|Return the argument raised to the specified power|
|[`POWER()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_power)|Return the argument raised to the specified power|
|[`RADIANS()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_radians)|Return argument converted to radians|
|[`RAND()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_rand)|Return a random floating-point value|
|[`ROUND()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_round)|Round the argument|
|[`SIGN()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_sign)|Return the sign of the argument|
|[`SIN()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_sin)|Return the sine of the argument|
|[`SQRT()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_sqrt)|Return the square root of the argument|
|[`TAN()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_tan)|Return the tangent of the argument|
|[`TRUNCATE()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_truncate)|Truncate to specified number of decimal places|

  

All mathematical functions return `NULL` in the event of an error.

- [``ABS(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_abs)
    
    Returns the absolute value of _`X`_, or `NULL` if _`X`_ is `NULL`.
    
    The result type is derived from the argument type. An implication of this is that [`ABS(-9223372036854775808)`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_abs) produces an error because the result cannot be stored in a signed `BIGINT` value.
    
    ```sql
    mysql> SELECT ABS(2);
            -> 2
    mysql> SELECT ABS(-32);
            -> 32
    ```
    
    This function is safe to use with [`BIGINT`](https://dev.mysql.com/doc/refman/8.4/en/integer-types.html "13.1.2 Integer Types (Exact Value) - INTEGER, INT, SMALLINT, TINYINT, MEDIUMINT, BIGINT") values.
    
- [``ACOS(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_acos)
    
    Returns the arc cosine of _`X`_, that is, the value whose cosine is _`X`_. Returns `NULL` if _`X`_ is not in the range `-1` to `1`, or if _`X`_ is `NULL`.
    
    ```sql
    mysql> SELECT ACOS(1);
            -> 0
    mysql> SELECT ACOS(1.0001);
            -> NULL
    mysql> SELECT ACOS(0);
            -> 1.5707963267949
    ```
    
- [``ASIN(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_asin)
    
    Returns the arc sine of _`X`_, that is, the value whose sine is _`X`_. Returns `NULL` if _`X`_ is not in the range `-1` to `1`, or if _`X`_ is `NULL`.
    
    ```sql
    mysql> SELECT ASIN(0.2);
            -> 0.20135792079033
    mysql> SELECT ASIN('foo');
    
    +-------------+
    | ASIN('foo') |
    +-------------+
    |           0 |
    +-------------+
    1 row in set, 1 warning (0.00 sec)
    
    mysql> SHOW WARNINGS;
    +---------+------+-----------------------------------------+
    | Level   | Code | Message                                 |
    +---------+------+-----------------------------------------+
    | Warning | 1292 | Truncated incorrect DOUBLE value: 'foo' |
    +---------+------+-----------------------------------------+
    ```
    
- [``ATAN(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_atan)
    
    Returns the arc tangent of _`X`_, that is, the value whose tangent is _`X`_. Returns _`NULL`_ if _`X`_ is `NULL`
    
    ```sql
    mysql> SELECT ATAN(2);
            -> 1.1071487177941
    mysql> SELECT ATAN(-2);
            -> -1.1071487177941
    ```
    
- [``ATAN(_`Y`_,_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_atan2), [``ATAN2(_`Y`_,_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_atan2)
    
    Returns the arc tangent of the two variables _`X`_ and _`Y`_. It is similar to calculating the arc tangent of ``_`Y`_ / _`X`_``, except that the signs of both arguments are used to determine the quadrant of the result. Returns `NULL` if _`X`_ or _`Y`_ is `NULL`.
    
    ```sql
    mysql> SELECT ATAN(-2,2);
            -> -0.78539816339745
    mysql> SELECT ATAN2(PI(),0);
            -> 1.5707963267949
    ```
    
- [``CEIL(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_ceil)
    
    [`CEIL()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_ceil) is a synonym for [`CEILING()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_ceiling).
    
- [``CEILING(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_ceiling)
    
    Returns the smallest integer value not less than _`X`_. Returns `NULL` if _`X`_ is `NULL`.
    
    ```sql
    mysql> SELECT CEILING(1.23);
            -> 2
    mysql> SELECT CEILING(-1.23);
            -> -1
    ```
    
    For exact-value numeric arguments, the return value has an exact-value numeric type. For string or floating-point arguments, the return value has a floating-point type.
    
- [``CONV(_`N`_,_`from_base`_,_`to_base`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_conv)
    
    Converts numbers between different number bases. Returns a string representation of the number _`N`_, converted from base _`from_base`_ to base _`to_base`_. Returns `NULL` if any argument is `NULL`. The argument _`N`_ is interpreted as an integer, but may be specified as an integer or a string. The minimum base is `2` and the maximum base is `36`. If _`from_base`_ is a negative number, _`N`_ is regarded as a signed number. Otherwise, _`N`_ is treated as unsigned. [`CONV()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_conv) works with 64-bit precision.
    
    `CONV()` returns `NULL` if any of its arguments are `NULL`.
    
    ```sql
    mysql> SELECT CONV('a',16,2);
            -> '1010'
    mysql> SELECT CONV('6E',18,8);
            -> '172'
    mysql> SELECT CONV(-17,10,-18);
            -> '-H'
    mysql> SELECT CONV(10+'10'+'10'+X'0a',10,10);
            -> '40'
    ```
    
- [``COS(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_cos)
    
    Returns the cosine of _`X`_, where _`X`_ is given in radians. Returns `NULL` if _`X`_ is `NULL`.
    
    ```sql
    mysql> SELECT COS(PI());
            -> -1
    ```
    
- [``COT(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_cot)
    
    Returns the cotangent of _`X`_. Returns `NULL` if _`X`_ is `NULL`.
    
    ```sql
    mysql> SELECT COT(12);
            -> -1.5726734063977
    mysql> SELECT COT(0);
            -> out-of-range error
    ```
    
- [``CRC32(_`expr`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_crc32)
    
    Computes a cyclic redundancy check value and returns a 32-bit unsigned value. The result is `NULL` if the argument is `NULL`. The argument is expected to be a string and (if possible) is treated as one if it is not.
    
    ```sql
    mysql> SELECT CRC32('MySQL');
            -> 3259397556
    mysql> SELECT CRC32('mysql');
            -> 2501908538
    ```
    
- [``DEGREES(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_degrees)
    
    Returns the argument _`X`_, converted from radians to degrees. Returns `NULL` if _`X`_ is `NULL`.
    
    ```sql
    mysql> SELECT DEGREES(PI());
            -> 180
    mysql> SELECT DEGREES(PI() / 2);
            -> 90
    ```
    
- [``EXP(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_exp)
    
    Returns the value of _e_ (the base of natural logarithms) raised to the power of _`X`_. The inverse of this function is [`LOG()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_log) (using a single argument only) or [`LN()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_ln).
    
    If _`X`_ is `NULL`, this function returns `NULL`.
    
    ```sql
    mysql> SELECT EXP(2);
            -> 7.3890560989307
    mysql> SELECT EXP(-2);
            -> 0.13533528323661
    mysql> SELECT EXP(0);
            -> 1
    ```
    
- [``FLOOR(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_floor)
    
    Returns the largest integer value not greater than _`X`_. Returns `NULL` if _`X`_ is `NULL`.
    
    ```sql
    mysql> SELECT FLOOR(1.23), FLOOR(-1.23);
            -> 1, -2
    ```
    
    For exact-value numeric arguments, the return value has an exact-value numeric type. For string or floating-point arguments, the return value has a floating-point type.
    
- [``FORMAT(_`X`_,_`D`_)``](https://dev.mysql.com/doc/refman/8.4/en/string-functions.html#function_format)
    
    Formats the number _`X`_ to a format like `'#,###,###.##'`, rounded to _`D`_ decimal places, and returns the result as a string. For details, see [Section 14.8, “String Functions and Operators”](https://dev.mysql.com/doc/refman/8.4/en/string-functions.html "14.8 String Functions and Operators").
    
- [`HEX(N_or_S)`](https://dev.mysql.com/doc/refman/8.4/en/string-functions.html#function_hex)
    
    This function can be used to obtain a hexadecimal representation of a decimal number or a string; the manner in which it does so varies according to the argument's type. See this function's description in [Section 14.8, “String Functions and Operators”](https://dev.mysql.com/doc/refman/8.4/en/string-functions.html "14.8 String Functions and Operators"), for details.
    
- [``LN(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_ln)
    
    Returns the natural logarithm of _`X`_; that is, the base-_e_ logarithm of _`X`_. If _`X`_ is less than or equal to 0.0E0, the function returns `NULL` and a warning “Invalid argument for logarithm” is reported. Returns `NULL` if _`X`_ is `NULL`.
    
    ```sql
    mysql> SELECT LN(2);
            -> 0.69314718055995
    mysql> SELECT LN(-2);
            -> NULL
    ```
    
    This function is synonymous with [``LOG(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_log). The inverse of this function is the [`EXP()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_exp) function.
    
- [``LOG(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_log), [``LOG(_`B`_,_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_log)
    
    If called with one parameter, this function returns the natural logarithm of _`X`_. If _`X`_ is less than or equal to 0.0E0, the function returns `NULL` and a warning “Invalid argument for logarithm” is reported. Returns `NULL` if _`X`_ or _`B`_ is `NULL`.
    
    The inverse of this function (when called with a single argument) is the [`EXP()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_exp) function.
    
    ```sql
    mysql> SELECT LOG(2);
            -> 0.69314718055995
    mysql> SELECT LOG(-2);
            -> NULL
    ```
    
    If called with two parameters, this function returns the logarithm of _`X`_ to the base _`B`_. If _`X`_ is less than or equal to 0, or if _`B`_ is less than or equal to 1, then `NULL` is returned.
    
    ```sql
    mysql> SELECT LOG(2,65536);
            -> 16
    mysql> SELECT LOG(10,100);
            -> 2
    mysql> SELECT LOG(1,100);
            -> NULL
    ```
    
    [``LOG(_`B`_,_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_log) is equivalent to [``LOG(_`X`_) / LOG(_`B`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_log).
    
- [``LOG2(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_log2)
    
    Returns the base-2 logarithm of ``_`X`_``. If _`X`_ is less than or equal to 0.0E0, the function returns `NULL` and a warning “Invalid argument for logarithm” is reported. Returns `NULL` if _`X`_ is `NULL`.
    
    ```sql
    mysql> SELECT LOG2(65536);
            -> 16
    mysql> SELECT LOG2(-100);
            -> NULL
    ```
    
    [`LOG2()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_log2) is useful for finding out how many bits a number requires for storage. This function is equivalent to the expression [``LOG(_`X`_) / LOG(2)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_log).
    
- [``LOG10(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_log10)
    
    Returns the base-10 logarithm of _`X`_. If _`X`_ is less than or equal to 0.0E0, the function returns `NULL` and a warning “Invalid argument for logarithm” is reported. Returns `NULL` if _`X`_ is `NULL`.
    
    ```sql
    mysql> SELECT LOG10(2);
            -> 0.30102999566398
    mysql> SELECT LOG10(100);
            -> 2
    mysql> SELECT LOG10(-100);
            -> NULL
    ```
    
    [``LOG10(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_log10) is equivalent to [``LOG(10,_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_log).
    
- [``MOD(_`N`_,_`M`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_mod), [``_`N`_ % _`M`_``](https://dev.mysql.com/doc/refman/8.4/en/arithmetic-functions.html#operator_mod), [``_`N`_ MOD _`M`_``](https://dev.mysql.com/doc/refman/8.4/en/arithmetic-functions.html#operator_mod)
    
    Modulo operation. Returns the remainder of _`N`_ divided by _`M`_. Returns `NULL` if _`M`_ or _`N`_ is `NULL`.
    
    ```sql
    mysql> SELECT MOD(234, 10);
            -> 4
    mysql> SELECT 253 % 7;
            -> 1
    mysql> SELECT MOD(29,9);
            -> 2
    mysql> SELECT 29 MOD 9;
            -> 2
    ```
    
    This function is safe to use with [`BIGINT`](https://dev.mysql.com/doc/refman/8.4/en/integer-types.html "13.1.2 Integer Types (Exact Value) - INTEGER, INT, SMALLINT, TINYINT, MEDIUMINT, BIGINT") values.
    
    [`MOD()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_mod) also works on values that have a fractional part and returns the exact remainder after division:
    
    ```sql
    mysql> SELECT MOD(34.5,3);
            -> 1.5
    ```
    
    [``MOD(_`N`_,0)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_mod) returns `NULL`.
    
- [`PI()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_pi)
    
    Returns the value of π (pi). The default number of decimal places displayed is seven, but MySQL uses the full double-precision value internally.
    
    Because the return value of this function is a double-precision value, its exact representation may vary between platforms or implementations. This also applies to any expressions making use of `PI()`. See [Section 13.1.4, “Floating-Point Types (Approximate Value) - FLOAT, DOUBLE”](https://dev.mysql.com/doc/refman/8.4/en/floating-point-types.html "13.1.4 Floating-Point Types (Approximate Value) - FLOAT, DOUBLE").
    
    ```sql
    mysql> SELECT PI();
            -> 3.141593
    mysql> SELECT PI()+0.000000000000000000;
            -> 3.141592653589793000
    ```
    
- [``POW(_`X`_,_`Y`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_pow)
    
    Returns the value of _`X`_ raised to the power of _`Y`_. Returns `NULL` if _`X`_ or _`Y`_ is `NULL`.
    
    ```sql
    mysql> SELECT POW(2,2);
            -> 4
    mysql> SELECT POW(2,-2);
            -> 0.25
    ```
    
- [``POWER(_`X`_,_`Y`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_power)
    
    This is a synonym for [`POW()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_pow).
    
- [``RADIANS(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_radians)
    
    Returns the argument _`X`_, converted from degrees to radians. (Note that π radians equals 180 degrees.) Returns `NULL` if _`X`_ is `NULL`.
    
    ```sql
    mysql> SELECT RADIANS(90);
            -> 1.5707963267949
    ```
    
- [``RAND([_`N`_])``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_rand)
    
    Returns a random floating-point value _`v`_ in the range `0` <= _`v`_ < `1.0`. To obtain a random integer _`R`_ in the range _`i`_ <= _`R`_ < _`j`_, use the expression [``FLOOR(_`i`_ + RAND() * (_`j`_``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_floor) − ``_`i`_))``. For example, to obtain a random integer in the range the range `7` <= _`R`_ < `12`, use the following statement:
    
    ```sql
    SELECT FLOOR(7 + (RAND() * 5));
    ```
    
    If an integer argument _`N`_ is specified, it is used as the seed value:
    
    - With a constant initializer argument, the seed is initialized once when the statement is prepared, prior to execution.
        
    - With a nonconstant initializer argument (such as a column name), the seed is initialized with the value for each invocation of [`RAND()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_rand).
        
    
    One implication of this behavior is that for equal argument values, [``RAND(_`N`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_rand) returns the same value each time, and thus produces a repeatable sequence of column values. In the following example, the sequence of values produced by `RAND(3)` is the same both places it occurs.
    
    ```sql
    mysql> CREATE TABLE t (i INT);
    Query OK, 0 rows affected (0.42 sec)
    
    mysql> INSERT INTO t VALUES(1),(2),(3);
    Query OK, 3 rows affected (0.00 sec)
    Records: 3  Duplicates: 0  Warnings: 0
    
    mysql> SELECT i, RAND() FROM t;
    +------+------------------+
    | i    | RAND()           |
    +------+------------------+
    |    1 | 0.61914388706828 |
    |    2 | 0.93845168309142 |
    |    3 | 0.83482678498591 |
    +------+------------------+
    3 rows in set (0.00 sec)
    
    mysql> SELECT i, RAND(3) FROM t;
    +------+------------------+
    | i    | RAND(3)          |
    +------+------------------+
    |    1 | 0.90576975597606 |
    |    2 | 0.37307905813035 |
    |    3 | 0.14808605345719 |
    +------+------------------+
    3 rows in set (0.00 sec)
    
    mysql> SELECT i, RAND() FROM t;
    +------+------------------+
    | i    | RAND()           |
    +------+------------------+
    |    1 | 0.35877890638893 |
    |    2 | 0.28941420772058 |
    |    3 | 0.37073435016976 |
    +------+------------------+
    3 rows in set (0.00 sec)
    
    mysql> SELECT i, RAND(3) FROM t;
    +------+------------------+
    | i    | RAND(3)          |
    +------+------------------+
    |    1 | 0.90576975597606 |
    |    2 | 0.37307905813035 |
    |    3 | 0.14808605345719 |
    +------+------------------+
    3 rows in set (0.01 sec)
    ```
    
    [`RAND()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_rand) in a `WHERE` clause is evaluated for every row (when selecting from one table) or combination of rows (when selecting from a multiple-table join). Thus, for optimizer purposes, [`RAND()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_rand) is not a constant value and cannot be used for index optimizations. For more information, see [Section 10.2.1.20, “Function Call Optimization”](https://dev.mysql.com/doc/refman/8.4/en/function-optimization.html "10.2.1.20 Function Call Optimization").
    
    Use of a column with [`RAND()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_rand) values in an `ORDER BY` or `GROUP BY` clause may yield unexpected results because for either clause a [`RAND()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_rand) expression can be evaluated multiple times for the same row, each time returning a different result. If the goal is to retrieve rows in random order, you can use a statement like this:
    
    ```sql
    SELECT * FROM tbl_name ORDER BY RAND();
    ```
    
    To select a random sample from a set of rows, combine `ORDER BY RAND()` with `LIMIT`:
    
    ```sql
    SELECT * FROM table1, table2 WHERE a=b AND c<d ORDER BY RAND() LIMIT 1000;
    ```
    
    [`RAND()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_rand) is not meant to be a perfect random generator. It is a fast way to generate random numbers on demand that is portable between platforms for the same MySQL version.
    
    This function is unsafe for statement-based replication. A warning is logged if you use this function when [`binlog_format`](https://dev.mysql.com/doc/refman/8.4/en/replication-options-binary-log.html#sysvar_binlog_format) is set to `STATEMENT`.
    
- [``ROUND(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_round), [``ROUND(_`X`_,_`D`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_round)
    
    Rounds the argument _`X`_ to _`D`_ decimal places. The rounding algorithm depends on the data type of _`X`_. _`D`_ defaults to 0 if not specified. _`D`_ can be negative to cause _`D`_ digits left of the decimal point of the value _`X`_ to become zero. The maximum absolute value for _`D`_ is 30; any digits in excess of 30 (or -30) are truncated. If _`X`_ or _`D`_ is `NULL`, the function returns `NULL`.
    
    ```sql
    mysql> SELECT ROUND(-1.23);
            -> -1
    mysql> SELECT ROUND(-1.58);
            -> -2
    mysql> SELECT ROUND(1.58);
            -> 2
    mysql> SELECT ROUND(1.298, 1);
            -> 1.3
    mysql> SELECT ROUND(1.298, 0);
            -> 1
    mysql> SELECT ROUND(23.298, -1);
            -> 20
    mysql> SELECT ROUND(.12345678901234567890123456789012345, 35);
            -> 0.123456789012345678901234567890
    ```
    
    The return value has the same type as the first argument (assuming that it is integer, double, or decimal). This means that for an integer argument, the result is an integer (no decimal places):
    
    ```sql
    mysql> SELECT ROUND(150.000,2), ROUND(150,2);
    +------------------+--------------+
    | ROUND(150.000,2) | ROUND(150,2) |
    +------------------+--------------+
    |           150.00 |          150 |
    +------------------+--------------+
    ```
    
    [`ROUND()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_round) uses the following rules depending on the type of the first argument:
    
    - For exact-value numbers, [`ROUND()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_round) uses the “round half away from zero” or “round toward nearest” rule: A value with a fractional part of .5 or greater is rounded up to the next integer if positive or down to the next integer if negative. (In other words, it is rounded away from zero.) A value with a fractional part less than .5 is rounded down to the next integer if positive or up to the next integer if negative.
        
    - For approximate-value numbers, the result depends on the C library. On many systems, this means that [`ROUND()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_round) uses the “round to nearest even” rule: A value with a fractional part exactly halfway between two integers is rounded to the nearest even integer.
        
    
    The following example shows how rounding differs for exact and approximate values:
    
    ```sql
    mysql> SELECT ROUND(2.5), ROUND(25E-1);
    +------------+--------------+
    | ROUND(2.5) | ROUND(25E-1) |
    +------------+--------------+
    | 3          |            2 |
    +------------+--------------+
    ```
    
    For more information, see [Section 14.24, “Precision Math”](https://dev.mysql.com/doc/refman/8.4/en/precision-math.html "14.24 Precision Math").
    
    The data type returned by `ROUND()` (and [`TRUNCATE()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_truncate)) is determined according to the rules listed here:
    
    - When the first argument is of any integer type, the return type is always [`BIGINT`](https://dev.mysql.com/doc/refman/8.4/en/integer-types.html "13.1.2 Integer Types (Exact Value) - INTEGER, INT, SMALLINT, TINYINT, MEDIUMINT, BIGINT").
        
    - When the first argument is of any floating-point type or of any non-numeric type, the return type is always [`DOUBLE`](https://dev.mysql.com/doc/refman/8.4/en/floating-point-types.html "13.1.4 Floating-Point Types (Approximate Value) - FLOAT, DOUBLE").
        
    - When the first argument is a [`DECIMAL`](https://dev.mysql.com/doc/refman/8.4/en/fixed-point-types.html "13.1.3 Fixed-Point Types (Exact Value) - DECIMAL, NUMERIC") value, the return type is also `DECIMAL`.
        
    - The type attributes for the return value are also copied from the first argument, except in the case of `DECIMAL`, when the second argument is a constant value.
        
        When the desired number of decimal places is less than the scale of the argument, the scale and the precision of the result are adjusted accordingly.
        
        In addition, for `ROUND()` (but not for the [`TRUNCATE()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_truncate) function), the precision is extended by one place to accommodate rounding that increases the number of significant digits. If the second argument is negative, the return type is adjusted such that its scale is 0, with a corresponding precision. For example, `ROUND(99.999, 2)` returns `100.00`—the first argument is `DECIMAL(5, 3)`, and the return type is `DECIMAL(5, 2)`.
        
        If the second argument is negative, the return type has scale 0 and a corresponding precision; `ROUND(99.999, -1)` returns `100`, which is `DECIMAL(3, 0)`.
        
    
- [``SIGN(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_sign)
    
    Returns the sign of the argument as `-1`, `0`, or `1`, depending on whether _`X`_ is negative, zero, or positive. Returns `NULL` if _`X`_ is `NULL`.
    
    ```sql
    mysql> SELECT SIGN(-32);
            -> -1
    mysql> SELECT SIGN(0);
            -> 0
    mysql> SELECT SIGN(234);
            -> 1
    ```
    
- [``SIN(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_sin)
    
    Returns the sine of _`X`_, where _`X`_ is given in radians. Returns `NULL` if _`X`_ is `NULL`.
    
    ```sql
    mysql> SELECT SIN(PI());
            -> 1.2246063538224e-16
    mysql> SELECT ROUND(SIN(PI()));
            -> 0
    ```
    
- [``SQRT(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_sqrt)
    
    Returns the square root of a nonnegative number _`X`_. If _`X`_ is `NULL`, the function returns `NULL`.
    
    ```sql
    mysql> SELECT SQRT(4);
            -> 2
    mysql> SELECT SQRT(20);
            -> 4.4721359549996
    mysql> SELECT SQRT(-16);
            -> NULL
    ```
    
- [``TAN(_`X`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_tan)
    
    Returns the tangent of _`X`_, where _`X`_ is given in radians. Returns `NULL` if _`X`_ is `NULL`.
    
    ```sql
    mysql> SELECT TAN(PI());
            -> -1.2246063538224e-16
    mysql> SELECT TAN(PI()+1);
            -> 1.5574077246549
    ```
    
- [``TRUNCATE(_`X`_,_`D`_)``](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_truncate)
    
    Returns the number _`X`_, truncated to _`D`_ decimal places. If _`D`_ is `0`, the result has no decimal point or fractional part. _`D`_ can be negative to cause _`D`_ digits left of the decimal point of the value _`X`_ to become zero. If _`X`_ or _`D`_ is `NULL`, the function returns `NULL`.
    
    ```sql
    mysql> SELECT TRUNCATE(1.223,1);
            -> 1.2
    mysql> SELECT TRUNCATE(1.999,1);
            -> 1.9
    mysql> SELECT TRUNCATE(1.999,0);
            -> 1
    mysql> SELECT TRUNCATE(-1.999,1);
            -> -1.9
    mysql> SELECT TRUNCATE(122,-2);
           -> 100
    mysql> SELECT TRUNCATE(10.28*100,0);
           -> 1028
    ```
    
    All numbers are rounded toward zero.
    
    The data type returned by `TRUNCATE()` follows the same rules that determine the return type of the `ROUND()` function; for details, see the description for [`ROUND()`](https://dev.mysql.com/doc/refman/8.4/en/mathematical-functions.html#function_round).