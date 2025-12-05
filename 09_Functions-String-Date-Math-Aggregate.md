# MySQL: Functions, String, Date, Math, Aggregate

- Function is a stored program that returns something.

> **NOTE:** MySQL follows 1 based indexing.

## String Functions:
1. **CONCAT(str1,str2,..strN):** it concatenates the strings and return single string.
```sql
 SELECT CONCAT('Hello', ' ', 'World');
```
2. **LENGTH(str):** it returns the length of the string in bytes.
```sql
 SELECT LENGTH('Hello');
```
3. **UPPER(str):** it returns the string in uppercase.
```sql
 SELECT UPPER('Hello');
```
4. **LOWER(str):** it returns the string in lowercase.
```sql
 SELECT LOWER('Hello');
```
5. **TRIM([LEADING|TRAILING|BOTH] [remstr] FROM str):** it removes the leading, trailing or both leading and trailing characters from the string str. If remstr is not specified, it removes the leading and trailing spaces.
```sql
 SELECT TRIM('    SQL    '); -- 'SQL'
 SELECT TRIM(BOTH 'x' FROM 'xxxSQLxxx'); -- 'SQL'
 SELECT TRIM(TRAILING 'x' FROM 'xxxSQLxxx'); -- 'xxxSQL'
 SELECT TRIM(LEADING 'x' FROM 'xxxSQLxxx'); -- 'SQLxxx'
```
6. **SUBSTRING(str, pos, len):** it returns the substring from the string str starting from position pos and of length len.
```sql
 SELECT SUBSTRING('Hello', 1, 3); -- 'Hel'
```
7. **LOCATE(str, str1, pos):** it returns the position of the string str in the string str1 starting from position pos. If pos is not specified, it starts from 1.
```sql
 SELECT LOCATE('Hello', 'Hello World'); -- 1
 SELECT LOCATE('Hello', 'Hello World Hello World', 2); -- 13
```
8. **REPLACE(str, from_str, to_str):** it returns the string str after replacing all occurrences of from_str with to_str.
```sql
 SELECT REPLACE('Hello World', 'World', 'MySQL'); -- 'Hello MySQL'
```
9. **REVERSE(str):** it returns the string str in reverse order.
```sql
 SELECT REVERSE('Hello'); -- 'olleH'
```
10. **LEFT(str, len):** it returns the leftmost len characters from the string str.
```sql
 SELECT LEFT('Hello', 3); -- 'Hel'
```
11. **RIGHT(str, len):** it returns the rightmost len characters from the string str.
```sql
 SELECT RIGHT('Hello', 3); -- 'llo'
```
12. **REPEAT(str, count):** it returns the string str repeated count times.
```sql
 SELECT REPEAT('Hello', 3); -- 'HelloHelloHello'
```
13. **ASCII(str):** it returns the ASCII value of the first character of the string str.
```sql
 SELECT ASCII('Hello'); -- 72 as incase of String the ASCII value will be of 1st character in the String
```
14. **FIELD(str, str1, str2,..strN):** it returns the position of the string str in the list of strings str1, str2,..strN. If str is not found, it returns 0.
```sql
 SELECT FIELD('Hello', 'Hello', 'World'); -- 1
```
15. **CHAR_LENGTH(str):** it returns the length of the string str in characters.
```sql
 SELECT CHAR_LENGTH('Hello'); -- 5

 SELECT CHAR_LENGTH('Café'); -- 4 it treats multi byte character as single unit
 SELECT LENGTH('Café'); -- 5 as it counts bytes and this contains multi byte character
```
16. **SOUNDEX(str):** it returns the soundex value of the string str. It is used to find similar sounding words and it returns 4-character code representing how the word is sounding.
```sql
 SELECT SOUNDEX('Rupert'), SOUNDEX('ROBERT'); -- 'R163', 'R163'
```
> **NOTE:** Soundex is best for English Language and case of mismatching is very high.

## Math Functions:
17. **ABS(num):** it returns the absolute value of the number num.
```sql
 SELECT ABS(-10); -- 10
```
18. **CEIL(num):** it returns the smallest integer greater than or equal to num.
```sql
 SELECT CEIL(10.1); -- 11
```
19. **FLOOR(num):** it returns the largest integer less than or equal to num.
```sql
 SELECT FLOOR(10.1); -- 10
```
20. **ROUND(num, decimals):** it returns the number num rounded to decimals decimal places.
```sql
 SELECT ROUND(10.123,2); -- 10.12
```
21. **TRUNCATE(num, decimals):** it returns the number num truncated to decimals decimal places.
```sql
 SELECT TRUNCATE(10.1234, 3); -- 10.123
```
22. **POW(base, exp):** it returns the number base raised to the power of exp.
```sql
 SELECT POW(2, 3); -- 8
```

23. **MOD(num1, num2):** it returns the remainder of num1 divided by num2.
```sql
 SELECT MOD(10, 3); -- 1
```
24. **SQRT(num):** it returns the square root of the number num.
```sql
 SELECT SQRT(10); -- 3.1622776601683795
 -- For negative number we have to use ABS
 SELECT SQRT(ABS(-225)); -- 15
```
25. **EXP(num):** it returns the exponential of the number *num*, and *num* should be less than 700 else it iwll give `out of range` error.
```sql
 SELECT EXP(1); -- 2.718281828459045
```
26. **LOG(base, num):** it returns the logarithm of the number *num* with base *base*.
```sql
 SELECT LOG(10, 100); -- 2
```
27. **LOG10(num):** it returns the base 10 logarithm of the number *num*.
```sql
 SELECT LOG10(100); -- 2
```
28. **LOG(num):** it returns the natural logarithm of the number *num*.
```sql
 SELECT LOG(100); -- 4.605170185988092
```
> **NOTE:** The `LOG` of negative number of negative number is `UNDEFINED` so we use `ABS` to avoid such error.
29. **RAND():** it returns a random float value between 0 and 1.
```sql
 SELECT RAND(); -- 0.123456789
```
30. **SIN(num):** it returns the sine of the number *num* in radians.
```sql
 SELECT SIN(1); -- 0.8414709848078965
```
31. **COS(num):** it returns the cosine of the number *num* in radians.
```sql
 SELECT COS(1); -- 0.5403023058681398
```
32. **TAN(num):** it returns the tangent of the number *num* in radians.
```sql
 SELECT TAN(1); -- 1.5574077246549023
```
33. **PI():** it returns the value of pi.
```sql
 SELECT PI(); -- 3.141592653589793
```
34. **DEGREES(num):** it converts the number *num* from radians to degrees.
```sql
 SELECT DEGREES(1); -- 57.29577951308232
```
35. **RADIANS(num):** it converts the number *num* from degrees to radians.
```sql
 SELECT RADIANS(1); -- 0.017453292519943295
```
36. **BIT_AND(col):** it returns the bit AND of all rows of the column *col*.
```sql
 SELECT BIT_AND(price) FROM products;
```
37. **BIT_OR(col):** it returns the bit OR of all rows of the column *col*.
```sql
 SELECT BIT_OR(price) FROM products;
```
38. **BIT_XOR(col):** it returns the bit XOR of all rows of the column *col*.
```sql
 SELECT BIT_XOR(price) FROM products;
```
> **NOTE:** In case of bitwise operations if values are in floating point then they are converted to integer before performing the operation.

## Date Functions:
39. **NOW():** it returns the current date and time.
```sql
 SELECT NOW();
```
40. **CURDATE():** it returns the current date.
```sql
 SELECT CURDATE();
```
41. **CURTIME():** it returns the current time.
```sql
 SELECT CURTIME();
```
42. **YEAR(date):** it returns the year of the date.
```sql
 SELECT YEAR('2022-12-05'); -- 2022
```
43. **MONTH(date):** it returns the month of the date.
```sql
 SELECT MONTH('2022-12-05'); -- 12
```
44. **DAY(date):** it returns the day of the date.
```sql
 SELECT DAY('2022-12-05'); -- 5
```
45. **HOUR(date):** it returns the hour of the date.
```sql
 SELECT HOUR('2022-12-05 12:00:00'); -- 12
```
46. **MINUTE(date):** it returns the minute of the date.
```sql
 SELECT MINUTE('2022-12-05 12:00:00'); -- 0
```
47. **SECOND(date):** it returns the second of the date.
```sql
 SELECT SECOND('2022-12-05 12:00:00'); -- 0
```
48. **DATE_ADD(date, INTERVAL value unit):** it returns the date after adding the interval to the date.
```sql
 SELECT DATE_ADD('2022-12-05', INTERVAL 1 DAY); -- 2022-12-06
```
49. **DATE_SUB(date, INTERVAL value unit):** it returns the date after subtracting the interval from the date.
```sql
 SELECT DATE_SUB('2022-12-05', INTERVAL 1 DAY); -- 2022-12-04
```
50. **DATE_FORMAT(date, format):** it returns the date in the format specified.
```sql
 SELECT DATE_FORMAT('2022-12-05', '%Y-%m-%d'); -- 2022-12-05
```
> **NOTE:** The format can be any of the following:
> - %Y: Year with century as a decimal number.
> - %y: Year without century as a decimal number.
> - %m: Month as a zero-padded decimal number.
> - %c: Month as a decimal number (1-12).
> - %d: Day of the month as a zero-padded decimal number.
> - %e: Day of the month as a decimal number (1-31).
> - %H: Hour (24-hour clock) as a zero-padded decimal number.
> - %h: Hour (12-hour clock) as a zero-padded decimal number.
> - %i: Minutes as a zero-padded decimal number.
> - %s: Seconds as a zero-padded decimal number.
> - %p: AM or PM.
> - %W: Weekday as a full name.
> - %a: Weekday as an abbreviation.
> - %w: Weekday as a decimal number (0-6).
> - %D: Day of the month with English suffix (e.g., 1st, 2nd, 3rd, 4th).
> - %M: Month as a full name.
> - %b: Month as an abbreviation.
> - %j: Day of the year as a zero-padded decimal number.
> - %U: Week of the year as a zero-padded decimal number (1-53).
> - %u: Week of the year as a decimal number (1-53).
> - %c: Month as a decimal number (1-12).
> - %x: Year with century as a decimal number.
> - %y: Year without century as a decimal number.
> - %X: Year with century as a decimal number.
> - %Y: Year with century as a decimal number.
> - %z: Timezone offset in the format +HHMM or -HHMM.
> - %Z: Timezone name.
> - %%: Literal % character.

51. **DATE_DIFF(date1, date2):** it returns the difference of days between the two dates.
```sql
 SELECT DATE_DIFF('2022-12-05', '2022-12-04'); -- 1
```
> **NOTE:** Standard format for date is *YYYY-mm-dd*. If result is positive in `DATE_DIFF` then it means that `date1` is after `date2` and if result is negative then it means that `date1` is before `date2`.
52. **UNIX_TIMESTAMP(date):** it returns the Unix timestamp of the date i.e., number of seconds passed since 1970-01-01 00:00:00 till now also known as `EPOCH` time.
```sql
 SELECT UNIX_TIMESTAMP(); -- number of seconds passed since 1970-01-01 00:00:00 till now
 SELECT UNIX_TIMESTAMP('2022-12-05'); -- 1670227200
```
53. **FROM_UNIXTIME(timestamp, optional_format):** it returns the human readable date time from the Unix timestamp, optional format can be used to format the date.
```sql
 SELECT FROM_UNIXTIME(1670227200); -- 2022-12-05
 SELECT FROM_UNIXTIME(1670227200, '%Y-%m-%d %H:%i:%s'); -- 2022-12-05 00:00:00
```
## Aggregate Functions:
- there are the functions that are used to perform calculations on multiple rows of data and return a single summarized value.
- These are commonly used with `GROUP BY` and `HAVING` clauses to group rows.
54. **COUNT(col):** it returns the number of rows in the column *col*.
```sql
 SELECT COUNT(*) FROM table_name;
```
55. **SUM(col):** it returns the sum of all values in the column *col*.
```sql
 SELECT SUM(col) FROM table_name;
```
56. **AVG(col):** it returns the average of all values in the column *col*.
```sql
 SELECT AVG(col) FROM table_name;
```
57. **MAX(col):** it returns the maximum value in the column *col*.
```sql
 SELECT MAX(col) FROM table_name;
```
58. **MIN(col):** it returns the minimum value in the column *col*.
```sql
 SELECT MIN(col) FROM table_name;
```
