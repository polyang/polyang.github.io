---
title: Java String compareTo方法详解
categories: Java
tags:
  - Java基础
  - String
  - compareTo
abbrlink: 928514d2
date: 2022-12-11 19:53:47
description: 带你了解一下String中的compareTo方法。揭露那些不为人知的代码！
---
### 解释
compareTo方法来自Comparable接口，String实现了该接口，具体实现如下：

```java
/**
     * Compares two strings lexicographically.
     * The comparison is based on the Unicode value of each character in
     * the strings. The character sequence represented by this
     * {@code String} object is compared lexicographically to the
     * character sequence represented by the argument string. The result is
     * a negative integer if this {@code String} object
     * lexicographically precedes the argument string. The result is a
     * positive integer if this {@code String} object lexicographically
     * follows the argument string. The result is zero if the strings
     * are equal; {@code compareTo} returns {@code 0} exactly when
     * the {@link #equals(Object)} method would return {@code true}.
     * <p>
     * This is the definition of lexicographic ordering. If two strings are
     * different, then either they have different characters at some index
     * that is a valid index for both strings, or their lengths are different,
     * or both. If they have different characters at one or more index
     * positions, let <i>k</i> be the smallest such index; then the string
     * whose character at position <i>k</i> has the smaller value, as
     * determined by using the &lt; operator, lexicographically precedes the
     * other string. In this case, {@code compareTo} returns the
     * difference of the two character values at position {@code k} in
     * the two string -- that is, the value:
     * <blockquote><pre>
     * this.charAt(k)-anotherString.charAt(k)
     * </pre></blockquote>
     * If there is no index position at which they differ, then the shorter
     * string lexicographically precedes the longer string. In this case,
     * {@code compareTo} returns the difference of the lengths of the
     * strings -- that is, the value:
     * <blockquote><pre>
     * this.length()-anotherString.length()
     * </pre></blockquote>
     *
     * @param   anotherString   the {@code String} to be compared.
     * @return  the value {@code 0} if the argument string is equal to
     *          this string; a value less than {@code 0} if this string
     *          is lexicographically less than the string argument; and a
     *          value greater than {@code 0} if this string is
     *          lexicographically greater than the string argument.
     */
    public int compareTo(String anotherString) {
        int len1 = value.length;
        int len2 = anotherString.value.length;
        int lim = Math.min(len1, len2);
        char v1[] = value;
        char v2[] = anotherString.value;

        int k = 0;
        while (k < lim) {
            char c1 = v1[k];
            char c2 = v2[k];
            if (c1 != c2) {
                return c1 - c2;
            }
            k++;
        }
        return len1 - len2;
    }
```
根据上面的代码，我们可以知道String的compareTo是单个字符依次进行比较的，于是可以得出如下结论：
1、如果两个字符串的长度一样，从下标为0的位置开始，依次比较每个位置上字符的Unicode码值的大小，如果相等，则继续比较下一个位置，否则直接返回两者Unicode码值的差值，如果所有位置的字符的Unicode码值都相等，则返回两个字符串的长度差值，即0;
2、如果两个字符串的长度不一样，从下标为0的位置开始，依次比较每个位置上字符的Unicode码值的大小，如果相等，则继续比较下一个位置，否则直接返回两者Unicode码值的差值，如果长度更短的字符串的所有位置都毕竟比较完了，发现都是相等，则返回两个字符串的长度差值（<font color='red'>注意：这里有一个需要避坑的地方，千万不要拿长度不一样的数字字符串去compareTo比较，比如"2".compareTo("15")，你可能期望返回-1，实际却返回1</font>）。

### 实例
```java
    String str1 = "ABC";
    String str2 = "ABC";
    String str3 = "ABD";
    String str4 = "ABCD";
    String str5 = "ABCDE";
    String str6 = "你好";
    String str7 = "大家好";
    String str8 = "2";
    String str9 = "15";

    System.out.println("ABC与ABC比较：" + str1.compareTo(str2));
    System.out.println("ABC与ABD比较：" + str1.compareTo(str3));
    System.out.println("ABC与ABCD比较：" + str1.compareTo(str4));
    System.out.println("ABC与ABCDE比较：" + str1.compareTo(str5));
    System.out.println("你好与大家好比较：" + str6.compareTo(str7));// 返回你和大的Unicode 差值
    System.out.println("2与15比较：" + str8.compareTo(str9));
```
输出结果为：
```java
ABC与ABC比较：0
ABC与ABD比较：-1
ABC与ABCD比较：-1
ABC与ABCDE比较：-2
你好与大家好比较：-2503
2与15比较：1
```