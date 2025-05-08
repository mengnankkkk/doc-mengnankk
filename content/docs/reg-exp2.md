---
title: 正则表达式 进阶
categories:
  - java
  
abbrlink: 2701
date: 2024.10.7
tags: 
   - 正则表达式
---

# 非贪婪匹配

给定一个字符串表示的数字，判断该数字末尾`0`的个数。例如：

- `"123000"`：3个`0`
- `"10100"`：2个`0`
- `"1001"`：0个`0`

可以很容易地写出该正则表达式：`(\d+)(0*)`

这是因为正则表达式默认使用贪婪匹配：任何一个规则，它总是尽可能多地向后匹配，因此，`\d+`总是会把后面的`0`包含进来。

```java
(\\d+?)(0*)

```

这样就代表非贪婪匹配，不会让\\d少匹配，0*多匹配

```
(\d??)(9*)
```

我们再来看这个正则表达式`(\d??)(9*)`，注意`\d?`表示匹配0个或1个数字，后面第二个`?`表示非贪婪匹配，因此，给定字符串`"9999"`，匹配到的两个子串分别是`""`和`"9999"`，因为对于`\d?`来说，可以匹配1个`9`，也可以匹配0个`9`，但是因为后面的`?`表示非贪婪匹配，它就会尽可能少的匹配，结果是匹配了0个`9`

# 搜索和替换

使用正则表达式分割字符串可以实现更加灵活的功能。`String.split()`方法传入的正是正则表达式。我们来看下面的代码：

```
"a b c".split("\\s"); // { "a", "b", "c" }
"a b  c".split("\\s"); // { "a", "b", "", "c" }
"a, b ;; c".split("[\\,\\;\\s]+"); // { "a", "b", "c" }
```

## 搜索

```
import java.util.regex.*;

public class Main {
    public static void main(String[] args) {
        String s = "the quick brown fox jumps over the lazy dog.";
        Pattern p = Pattern.compile("\\wo\\w");
        Matcher m = p.matcher(s);
        while (m.find()) {
            String sub = s.substring(m.start(), m.end());
            System.out.println(sub);
        }
    }
}
```

这个代码就是找中间有前面一个字符后面一个。中间有个o的字符

return

```
row
fox
dog
```

## 替换

```
// regex
public class Main {
    public static void main(String[] args) {
        String s = "The     quick\t\t brown   fox  jumps   over the  lazy dog.";
        String r = s.replaceAll("\\s+", " ");
        System.out.println(r); // "The quick brown fox jumps over the lazy dog."
    }
}
```

return

```
The quick brown fox jumps over the lazy dog.
```

## 反向引用

```
// regex
public class Main {
    public static void main(String[] args) {
        String s = "the quick brown fox jumps over the lazy dog.";
        String r = s.replaceAll("\\s([a-z]{4})\\s", " <b>$1</b> ");
        System.out.println(r);
    }
}
```

return

```
the quick brown fox jumps <b>over</b> the <b>lazy</b> dog.
```

它实际上把任何4字符单词的前后用`<b>xxxx</b>`括起来。实现替换的关键就在于`" <b>$1</b> "`，它用匹配的分组子串`([a-z]{4})`替换了`$1`。

方法：

使用正则表达式可以：

- 分割字符串：`String.split()`
- 搜索子串：`Matcher.find()`
- 替换字符串：`String.replaceAll()`

# 练习

模板引擎是指，定义一个字符串作为模板：

```plain
Hello, ${name}! You are learning ${lang}!
```

其中，以`${key}`表示的是变量，也就是将要被替换的内容。

当传入一个`Map<String, String>`给模板后，需要把对应的key替换为Map的value。

例如，传入`Map`为：

```json
{
    "name": "Bob",
    "lang": "Java"
}
```

然后，`${name}`被替换为`Map`对应的值`Bob`，`${lang}`被替换为`Map`对应的值`Java`，最终输出的结果为：

```plain
Hello, Bob! You are learning Java!
```

请编写一个简单的模板引擎，利用正则表达式实现这个功能。

```
import java.util.Map;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class reg_exp {
    public static String render(String template, Map<String, String> values) {
        // 定义匹配 ${key} 的正则表达式
        Pattern pattern = Pattern.compile("\\$\\{(\\w+)}");
        Matcher matcher = pattern.matcher(template);

        // 使用 StringBuffer 来高效拼接替换后的字符串
        StringBuffer result = new StringBuffer();

        while (matcher.find()) {
            String key = matcher.group(1);  // 获取 ${key} 中的 key
            String replacement = values.getOrDefault(key, "");  // 从 Map 中获取 key 对应的值
            matcher.appendReplacement(result, replacement);  // 替换 ${key} 为对应的值
        }

        matcher.appendTail(result);  // 添加剩余的部分
        return result.toString();
    }

    public static void main(String[] args) {
        // 定义模板
        String template = "Hello, ${name}! You are learning ${lang}!";

        // 定义替换内容的 Map
        Map<String, String> values = Map.of(
                "name", "Bob",
                "lang", "Java"
        );
```

还有很多正则表达式的内容，可以在https://regex101.com/

在线查看