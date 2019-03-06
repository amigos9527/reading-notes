### 构造方法

------

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | :----------------------------------------------------------- |
| **String**()                                                 | 初始化一个新创建的 String 对象，使其表示一个空字符序列。     |
| **String**(byte[] bytes)                                     | 通过使用平台的默认字符集解码指定的 byte 数组，构造一个新的 `String`。 |
| **String**(byte[] bytes, int offset, int length)             | 通过使用平台的默认字符集解码指定的 byte 子数组，构造一个新的 `String`。 |
| **String**(byte[] bytes, int offset, int length, Charset charset) | 通过使用指定的 charset 解码指定的 byte 子数组，构造一个新的 String。 |
| **String**(byte[] bytes, int offset, int length, String charsetName) | 通过使用指定的字符集解码指定的 byte 子数组，构造一个新的 String。 |
| **String**(byte[] bytes, String charsetName)                 | 通过使用指定的 charset 解码指定的 byte 数组，构造一个新的 String。 |
| **String**(char[] value)                                     | 分配一个新的 String，使其表示字符数组参数中当前包含的字符序列。 |
| **String**(int[] codePoints, int offset, int count)          | 分配一个新的 String，它包含 Unicode 代码点数组参数一个子数组的字符。 |
| **String**(String original)                                  | 初始化一个新创建的 String 对象，使其表示一个与参数相同的字符序列；换句话说，新创建的字符串是该参数字符串的副本。 |
| **String**(StringBuffer buffer)                              | 分配一个新的字符串，它包含字符串缓冲区参数中当前包含的字符序列。 |
| **String**(StringBuilder builder)                            | 分配一个新的字符串，它包含字符串生成器参数中当前包含的字符序列。 |



### 常用方法

------

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| char **charAt**(int index)                                   | 返回指定索引处的 char 值。                                   |
| int **compareTo**(String anotherString)                      | 按字典顺序比较两个字符串。                                   |
| int **compareToIgnoreCase**(String str)                      | 按字典顺序比较两个字符串，不考虑大小写。                     |
| String **concat**(String str)                                | 将指定字符串连接到此字符串的结尾。                           |
| boolean **contains**(CharSequence s)                         | 当且仅当此字符串包含指定的 char 值序列时，返回 true。        |
| boolean **contentEquals**(CharSequence cs)                   | 将此字符串与指定的 CharSequence 比较。                       |
| boolean **contentEquals**(StringBuffer sb)                   | 将此字符串与指定的 StringBuffer 比较。                       |
| static String **copyValueOf**(char[] data)                   | 返回指定数组中表示该字符序列的 String。                      |
| static String **copyValueOf**(char[] data, int offset, int count) | 返回指定数组中表示该字符序列的 String。                      |
| boolean **endsWith**(String suffix)                          | 测试此字符串是否以指定的后缀结束。                           |
| boolean **equals**(Object anObject)                          | 将此字符串与指定的对象比较。                                 |
| boolean **equalsIgnoreCase**(String     anotherString)       | 将此 String 与另一个 String 比较，不考虑大小写。             |
| static String **format**(Locale l, String format, Object... args) | 使用指定的语言环境、格式字符串和参数返回一个格式化字符串。   |
| static String **format**(String format, Object... args)      | 使用指定的格式字符串和参数返回一个格式化字符串。             |
| byte[] **getBytes**()                                        | 使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。 |
| byte[] **getBytes**(Charset charset)                         | 使用给定的 charset 将此 String 编码到 byte 序列，并将结果存储到新的 byte 数组。 |
| byte[] **getBytes**(String charsetName)                      | 使用指定的字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。 |
| void **getChars**(int srcBegin, int srcEnd, char[] dst, int dstBegin) | 将字符从此字符串复制到目标字符数组。                         |
| int **indexOf**(int ch)                                      | 返回指定字符在此字符串中第一次出现处的索引。                 |
| int **indexOf**(int ch, int fromIndex)                       | 返回在此字符串中第一次出现指定字符处的索引，从指定的索引开始搜索。 |
| int **indexOf**(String str)                                  | 返回指定子字符串在此字符串中第一次出现处的索引。             |
| int **indexOf**(String str, int fromIndex)                   | 返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始。 |
| String **intern**()                                          | 返回字符串对象的规范化表示形式。                             |
| boolean **isEmpty**()                                        | 当且仅当 length() 为 0 时返回 true。                         |
| int **lastIndexOf**(int ch)                                  | 返回指定字符在此字符串中最后一次出现处的索引。               |
| int **lastIndexOf**(int ch, int fromIndex)                   | 返回指定字符在此字符串中最后一次出现处的索引，从指定的索引处开始进行反向搜索。 |
| int **lastIndexOf**(String str)                              | 返回指定子字符串在此字符串中最右边出现处的索引。             |
| int **lastIndexOf**(String str, int fromIndex)               | 返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索。 |
| int **length**()                                             | 返回此字符串的长度。                                         |
| boolean **matches**(String regex)                            | 此字符串是否匹配给定的正则表达式。                           |
| String **replace**(char oldChar, char newChar)               | 返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的。 |
| String **replace**(CharSequence target, CharSequence replacement) | 使用指定的字面值替换序列替换此字符串所有匹配字面值目标序列的子字符串。 |
| String **replaceAll**(String regex, String replacement)      | 使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串。 |
| String **replaceFirst**(String regex, String replacement)    | 使用给定的 replacement 替换此字符串匹配给定的正则表达式的第一个子字符串。 |
| String[] **split**(String regex)                             | 根据给定正则表达式的匹配拆分此字符串。                       |
| String[] **split**(String regex, int limit)                  | 根据匹配给定的正则表达式来拆分此字符串。                     |
| boolean **startsWith**(String prefix)                        | 测试此字符串是否以指定的前缀开始。                           |
| boolean **startsWith**(String prefix, int toffset)           | 测试此字符串从指定索引开始的子字符串是否以指定前缀开始。     |
| String **substring**(int beginIndex)                         | 返回一个新的字符串，它是此字符串的一个子字符串。             |
| String **substring**(int beginIndex, int endIndex)           | 返回一个新字符串，它是此字符串的一个子字符串。               |
| char[] **toCharArray**()                                     | 将此字符串转换为一个新的字符数组。                           |
| String **toLowerCase**()                                     | 使用默认语言环境的规则将此 `String` 中的所有字符都转换为小写。 |
| String **toLowerCase**(Locale locale)                        | 使用给定 Locale 的规则将此 String 中的所有字符都转换为小写。 |
| String **toUpperCase**()                                     | 使用默认语言环境的规则将此 String 中的所有字符都转换为大写。 |
| String **toUpperCase**(Locale locale)                        | 使用给定 Locale 的规则将此 String 中的所有字符都转换为大写。 |
| String **trim**()                                            | 返回字符串的副本，忽略前导空白和尾部空白。                   |
| static String **valueOf**(Object obj)                        | 返回 Object 参数的字符串表示形式。                           |

