# MySQL - Numeric Functions

**Name**|**Description**
:-----:|:-----:
ABS()|Returns the absolute value of numeric expression.
ACOS()|Returns the arccosine of numeric expression. Returns NULL if the value is not in the range -1 to 1.
ASIN()|Returns the arcsine of numeric expression. Returns NULL if value is not in the range -1 to 1
ATAN()|Returns the arctangent of numeric expression.
ATAN2()|Returns the arctangent of the two variables passed to it.
BIT\_AND()|Returns the bitwise AND all the bits in expression.
BIT\_COUNT()|Returns the string representation of the binary value passed to it.
BIT\_OR()|Returns the bitwise OR of all the bits in the passed expression.
CEIL()|Returns the smallest integer value that is not less than passed numeric expression
CEILING()|Returns the smallest integer value that is not less than passed numeric expression
CONV()|Converts numeric expression from one base to another.
COS()|Returns the cosine of passed numeric expression. The numeric expression should be expressed in radians.
COT()|Returns the cotangent of passed numeric expression.
DEGREES()|Returns numeric expression converted from radians to degrees.
EXP()|Returns the base of the natural logarithm (e)  raised to the power of passed numeric expression.
FLOOR()|Returns the largest integer value that is not greater than passed numeric expression.
FORMAT()|Returns a numeric expression rounded to a number of decimal places.
GREATEST()|Returns the largest value of the input expressions.
INTERVAL()|Takes multiple expressions exp1, exp2 and exp3 so on.. and returns 0 if exp1 is less than exp2, returns 1 if exp1 is less than exp3 and so on.
LEAST()|Returns the minimum-valued input when given two or more.
LOG()|Returns the natural logarithm of the passed numeric expression.
LOG10()|Returns the base-10 logarithm of the passed numeric expression.
MOD()|Returns the remainder of one expression by diving by another expression.
OCT()|Returns the string representation of the octal value of the passed numeric expression. Returns NULL if passed value is NULL.
PI()|Returns the value of pi
POW()|Returns the value of one expression raised to the power of another expression
POWER()|Returns the value of one expression raised to the power of another expression
RADIANS()|Returns the value of passed expression converted from degrees to radians.
ROUND()|Returns numeric expression rounded to an integer. Can be used to round an expression to a number of decimal points
SIN()|Returns the sine of numeric expression given in radians.
SQRT()|Returns the non-negative square root of numeric expression.
STD()|Returns the standard deviation of the numeric expression.
STDDEV()|Returns the standard deviation of the numeric expression.
TAN()|Returns the tangent of numeric expression expressed in radians.
TRUNCATE()|Returns numeric exp1 truncated to exp2 decimal places. If exp2 is 0, then the result will have no decimal point.

## ABS(X)
The ABS() function returns the absolute value of X. Consider the following example −

```sql
mysql> SELECT ABS(2);
+---------------------------------------------------------+
|                          ABS(2)                         |
+---------------------------------------------------------+
|                            2                            |
+---------------------------------------------------------+
```

```sql
mysql> SELECT ABS(-2);
+---------------------------------------------------------+
|                          ABS(2)                         |
+---------------------------------------------------------+
|                            2                            |
+---------------------------------------------------------+
```

## ACOS(X)
This function returns the arccosine of X. The value of X must range between .1 and 1 or NULL will be returned. Consider the following example −

```sql
mysql> SELECT ACOS(1);
+---------------------------------------------------------+
|                          ACOS(1)                        |
+---------------------------------------------------------+
|                         0.000000                        |
+---------------------------------------------------------+
```

## ASIN(X)
The ASIN() function returns the arcsine of X. The value of X must be in the range of .1 to 1 or NULL is returned.

```sql
mysql> SELECT ASIN(1);
+---------------------------------------------------------+
|                          ASIN(1)                        |
+---------------------------------------------------------+
|                     1.5707963267949                     |
+---------------------------------------------------------+
```

## ATAN(X)
This function returns the arctangent of X.

```sql
mysql> SELECT ATAN(1);
+---------------------------------------------------------+
|                         ATAN(1)                         |
+---------------------------------------------------------+
|                   0.78539816339745                      |
+---------------------------------------------------------+
```

## ATAN2(Y,X)
This function returns the arctangent of the two arguments: X and Y. It is similar to the arctangent of Y/X, except that the signs of both are used to find the quadrant of the result.

```sql
mysql> SELECT ATAN2(3,6);
+---------------------------------------------------------+
|                        ATAN2(3,6)                       |
+---------------------------------------------------------+
|                    0.46364760900081                     |
+---------------------------------------------------------+
```

## BIT_AND(expression)
The BIT_AND function returns the bitwise AND of all bits in expression. The basic premise is that if two corresponding bits are the same, then a bitwise AND operation will return 1, while if they are different, a bitwise AND operation will return 0. The function itself returns a 64-bit integer value. If there are no matches, then it will return 18446744073709551615. The following example performs the BIT_AND function on the PRICE column grouped by the MAKER of the car −

```sql
mysql> SELECT 
      MAKER, BIT_AND(PRICE) BITS
      FROM CARS GROUP BY MAKER
+---------------------------------------------------------+
|                 MAKER           BITS                    |
+---------------------------------------------------------+
|                CHRYSLER         512                     |
|                  FORD          12488                    |
|                 HONDA           2144                    |
+---------------------------------------------------------+
```

## BIT_COUNT(numeric_value)
The BIT_COUNT() function returns the number of bits that are active in numeric_value. The following example demonstrates using the BIT_COUNT() function to return the number of active bits for a range of numbers −

```sql
mysql> SELECT
      BIT_COUNT(2) AS TWO,
      BIT_COUNT(4) AS FOUR,
      BIT_COUNT(7) AS SEVEN
+-----+------+-------+
| TWO | FOUR | SEVEN |
+-----+------+-------+
|  1  |   1  |   3   |
+-----+------+-------+
```

## BIT_OR(expression)
The BIT_OR() function returns the bitwise OR of all the bits in expression. The basic premise of the bitwise OR function is that it returns 0 if the corresponding bits match, and 1 if they do not. The function returns a 64-bit integer, and, if there are no matching rows, then it returns 0. The following example performs the BIT_OR() function on the PRICE column of the CARS table, grouped by the MAKER −

```sql
mysql> SELECT 
      MAKER, BIT_OR(PRICE) BITS
      FROM CARS GROUP BY MAKER
+---------------------------------------------------------+
|                MAKER           BITS                     |
+---------------------------------------------------------+
|               CHRYSLER        62293                     |
|                FORD           16127                     |
|                HONDA          32766                     |
+---------------------------------------------------------+
```

## CEIL(X)
## CEILING(X)
This function returns the smallest integer value that is not smaller than X. Consider the following example −

```sql
mysql> SELECT CEILING(3.46);
+---------------------------------------------------------+
|                     CEILING(3.46)                       |
+---------------------------------------------------------+
|                          4                              |
+---------------------------------------------------------+
```

```sql
mysql> SELECT CEIL(-6.43);
+---------------------------------------------------------+
|                      CEIL(-6.43)                        |
+---------------------------------------------------------+
|                          -6                             |
+---------------------------------------------------------+
```

## CONV(N,from_base,to_base)
The purpose of the CONV() function is to convert numbers between different number bases. The function returns a string of the value N converted from from_base to to_base. The minimum base value is 2 and the maximum is 36. If any of the arguments are NULL, then the function returns NULL. Consider the following example, which converts the number 5 from base 16 to base 2 −

```sql
mysql> SELECT CONV(5,16,2);
+---------------------------------------------------------+
|                      CONV(5,16,2)                       |
+---------------------------------------------------------+
|                          101                            |
+---------------------------------------------------------+
```

## COS(X)
This function returns the cosine of X. The value of X is given in radians.

```sql
mysql>SELECT COS(90);
+---------------------------------------------------------+
|                         COS(90)                         |
+---------------------------------------------------------+
|                  -0.44807361612917                      |
+---------------------------------------------------------+
```

## COT(X)
This function returns the cotangent of X. Consider the following example −

```sql
mysql>SELECT COT(1);
+---------------------------------------------------------+
|                          COT(1)                         |
+---------------------------------------------------------+
|                    0.64209261593433                     |
+---------------------------------------------------------+
```

## DEGREES(X)
This function returns the value of X converted from radians to degrees.

```sql
mysql>SELECT DEGREES(PI());
+---------------------------------------------------------+
|                      DEGREES(PI())                      |
+---------------------------------------------------------+
|                       180.000000                        |
+---------------------------------------------------------+
```

## EXP(X)
This function returns the value of e (the base of the natural logarithm) raised to the power of X.

```sql
mysql>SELECT EXP(3);
+---------------------------------------------------------+
|                         EXP(3)                          |
+---------------------------------------------------------+
|                       20.085537                         |
+---------------------------------------------------------+
```

## FLOOR(X)
This function returns the largest integer value that is not greater than X.

```sql
mysql>SELECT FLOOR(7.55);
+---------------------------------------------------------+
|                       FLOOR(7.55)                       |
+---------------------------------------------------------+
|                           7                             |
+---------------------------------------------------------+
```

## FORMAT(X,D)
The FORMAT() function is used to format the number X in the following format: ###,###,###.## truncated to D decimal places. The following example demonstrates the use and output of the FORMAT() function −

```sql
mysql>SELECT FORMAT(423423234.65434453,2);
+---------------------------------------------------------+
|             FORMAT(423423234.65434453,2)                |
+---------------------------------------------------------+
|                   423,423,234.65                        |
+---------------------------------------------------------+
```

## GREATEST(n1,n2,n3,..........)
The GREATEST() function returns the greatest value in the set of input parameters (n1, n2, n3, a nd so on). The following example uses the GREATEST() function to return the largest number from a set of numeric values −

```sql
mysql>SELECT GREATEST(3,5,1,8,33,99,34,55,67,43);
+---------------------------------------------------------+
|           GREATEST(3,5,1,8,33,99,34,55,67,43)           |
+---------------------------------------------------------+
|                            99                           |
+---------------------------------------------------------+
```

## INTERVAL(N,N1,N2,N3,..........)
The INTERVAL() function compares the value of N to the value list (N1, N2, N3, and so on ). The function returns 0 if N < N1, 1 if N < N2, 2 if N <N3, and so on. It will return .1 if N is NULL. The value list must be in the form N1 < N2 < N3 in order to work properly. The following code is a simple example of how the INTERVAL() function works −

```sql
mysql>SELECT INTERVAL(6,1,2,3,4,5,6,7,8,9,10);
+---------------------------------------------------------+
|             INTERVAL(6,1,2,3,4,5,6,7,8,9,10)            |
+---------------------------------------------------------+
|                             6                           |
+---------------------------------------------------------+
```

## INTERVAL(N,N1,N2,N3,..........)
The INTERVAL() function compares the value of N to the value list (N1, N2, N3, and so on ). The function returns 0 if N < N1, 1 if N < N2, 2 if N <N3, and so on. It will return .1 if N is NULL. The value list must be in the form N1 < N2 < N3 in order to work properly. The following code is a simple example of how the INTERVAL() function works −

```sql
mysql>SELECT INTERVAL(6,1,2,3,4,5,6,7,8,9,10);
+---------------------------------------------------------+
|            INTERVAL(6,1,2,3,4,5,6,7,8,9,10)             |
+---------------------------------------------------------+
|                           6                             |
+---------------------------------------------------------+
```
Remember that 6 is the zero-based index in the value list of the first value that was greater than N. In our case, 7 was the offending value and is located in the sixth index slot.

## LEAST(N1,N2,N3,N4,......)
The LEAST() function is the opposite of the GREATEST() function. Its purpose is to return the least-valued item from the value list (N1, N2, N3, and so on). The following example shows the proper usage and output for the LEAST() function −

```sql
mysql>SELECT LEAST(3,5,1,8,33,99,34,55,67,43);
+---------------------------------------------------------+
|            LEAST(3,5,1,8,33,99,34,55,67,43)             |
+---------------------------------------------------------+
|                            1                            |
+---------------------------------------------------------+
```

## LOG(X)
## LOG(B,X)
The single argument version of the function will return the natural logarithm of X. If it is called with two arguments, it returns the logarithm of X for an arbitrary base B. Consider the following example −

```sql
mysql>SELECT LOG(45);
+---------------------------------------------------------+
|                          LOG(45)                        |
+---------------------------------------------------------+
|                         3.806662                        |
+---------------------------------------------------------+
```

```sql
mysql>SELECT LOG(2,65536);
+---------------------------------------------------------+
|                       LOG(2,65536)                      |
+---------------------------------------------------------+
|                        16.000000                        |
+---------------------------------------------------------+
```

## LOG10(X)
This function returns the base-10 logarithm of X.

```sql
mysql>SELECT LOG10(100);
+---------------------------------------------------------+
|                      LOG10(100)                         |
+---------------------------------------------------------+
|                       2.000000                          |
+---------------------------------------------------------+
```

## MOD(N,M)
This function returns the remainder of N divided by M. Consider the following example −

```sql
mysql>SELECT MOD(29,3);
+---------------------------------------------------------+
|                        MOD(29,3)                        |
+---------------------------------------------------------+
|                            2                            |
+---------------------------------------------------------+
```

## OCT(N)
The OCT() function returns the string representation of the octal number N. This is equivalent to using CONV(N,10,8).

```sql
mysql>SELECT OCT(12);
+---------------------------------------------------------+
|                        OCT(12)                          |
+---------------------------------------------------------+
|                          14                             |
+---------------------------------------------------------+
```

## PI()
This function simply returns the value of pi. MySQL internally stores the full double-precision value of pi.

```sql
mysql>SELECT PI();
+---------------------------------------------------------+
|                          PI()                           |
+---------------------------------------------------------+
|                        3.141593                         |
+---------------------------------------------------------+
```

## POW(X,Y)
## POWER(X,Y)
These two functions return the value of X raised to the power of Y.

```sql
mysql> SELECT POWER(3,3);
+---------------------------------------------------------+
|                       POWER(3,3)                        |
+---------------------------------------------------------+
|                           27                            |
+---------------------------------------------------------+
```

## RADIANS(X)
This function returns the value of X, converted from degrees to radians.

```sql
mysql>SELECT RADIANS(90);
+---------------------------------------------------------+
|                      RADIANS(90)                        |
+---------------------------------------------------------+
|                       1.570796                          |
+---------------------------------------------------------+
```

## ROUND(X)
## ROUND(X,D)
This function returns X rounded to the nearest integer. If a second argument, D, is supplied, then the function returns X rounded to D decimal places. D must be positive or all digits to the right of the decimal point will be removed. Consider the following example −

```sql
mysql>SELECT ROUND(5.693893);
+---------------------------------------------------------+
|                    ROUND(5.693893)                      |
+---------------------------------------------------------+
|                           6                             |
+---------------------------------------------------------+
```

```sql
mysql>SELECT ROUND(5.693893,2);
+---------------------------------------------------------+
|                   ROUND(5.693893,2)                     |
+---------------------------------------------------------+
|                          5.69                           |
+---------------------------------------------------------+
```

## SIGN(X)
This function returns the sign of X (negative, zero, or positive) as .1, 0, or 1.

```sql
mysql>SELECT SIGN(-4.65);
+---------------------------------------------------------+
|                      SIGN(-4.65)                        |
+---------------------------------------------------------+
|                          -1                             |
+---------------------------------------------------------+
```

```sql
mysql>SELECT SIGN(0);
+---------------------------------------------------------+
|                         SIGN(0)                         |
+---------------------------------------------------------+
|                            0                            |
+---------------------------------------------------------+
```

```sql
mysql>SELECT SIGN(4.65);
+---------------------------------------------------------+
|                         SIGN(4.65)                      |
+---------------------------------------------------------+
|                             1                           |
+---------------------------------------------------------+
```

## SIN(X)
This function returns the sine of X. Consider the following example −

```sql
mysql>SELECT SIN(90);
+---------------------------------------------------------+
|                        SIN(90)                          |
+---------------------------------------------------------+
|                        0.893997                         |
+---------------------------------------------------------+
```

## SQRT(X)
This function returns the non-negative square root of X. Consider the following example −

```sql
mysql>SELECT SQRT(49);
+---------------------------------------------------------+
|                         SQRT(49)                        |
+---------------------------------------------------------+
|                            7                            |
+---------------------------------------------------------+
```

## STD(expression)
## STDDEV(expression)
The STD() function is used to return the standard deviation of expression. This is equivalent to taking the square root of the VARIANCE() of expression. The following example computes the standard deviation of the PRICE column in our CARS table −

```sql
mysql>SELECT STD(PRICE) STD_DEVIATION FROM CARS;
+---------------------------------------------------------+
|                      STD_DEVIATION                      |
+---------------------------------------------------------+
|                       7650.2146                         |
+---------------------------------------------------------+
```

## TAN(X)
This function returns the tangent of the argument X, which is expressed in radians.

```sql
mysql>SELECT TAN(45);
+---------------------------------------------------------+
|                         TAN(45)                         |
+---------------------------------------------------------+
|                        1.619775                         |
+---------------------------------------------------------+
```

## TRUNCATE(X,D)
This function is used to return the value of X truncated to D number of decimal places. If D is 0, then the decimal point is removed. If D is negative, then D number of values in the integer part of the value is truncated. Consider the following example −

```sql
mysql>SELECT TRUNCATE(7.536432,2);
+---------------------------------------------------------+
|                   TRUNCATE(7.536432,2)                  |
+---------------------------------------------------------+
|                         7.53                            |
+---------------------------------------------------------+
```