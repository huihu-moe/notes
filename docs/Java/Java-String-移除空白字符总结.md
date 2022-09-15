# String 中移除空白字符总结

参考[从String中移除空白字符的多种方式！？差别竟然这么大！](https://mp.weixin.qq.com/s/Du2huBEkI7IR3noPeK_67g)

- trim() : 删除字符串开头和结尾的空格。
- strip() : 删除字符串开头和结尾的空格。
- stripLeading() : 只删除字符串开头的空格
- stripTrailing() : 只删除字符串的结尾的空格
- replace() : 用新字符替换所有目标字符
- replaceAll() : 将所有匹配的字符替换为新字符。此方法将正则表达式作为输入，以标识需要替换的目标子字符串
- replaceFirst() : 仅将目标子字符串的第一次出现的字符替换为新的字符串

trim移除的空白字符指的是指ASCII值小于或等于32的任何字符(' U+0020 ')。

strip相比trim方法，适用范围更广。因为trim方法只能针对ASCII值小于等于32的字符进行移除，但是根据Unicode标准，除了ASCII中的字符以外，还是有很多其他的空白字符的。

strip使用Character类中的isWhitespace(int)方法。该方法使用unicode来标识空格字符。

| trim                        | strip                                                           |
|-----------------------------|-----------------------------------------------------------------|
| Java 1                      | Java 11                                                         |
| Ascii                       | Unicode                                                         |
| 删去Ascii值小于等于32的字符 | 根据Unicode删除所有空白字符，使用Character.isWhitespace方法判断 |