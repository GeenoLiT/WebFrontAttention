# escape,encodeURI与encodeURIComponent详解编码方式以及规则

## 三个方法的编码范围
```
escape不编码字符有69个：*，+，-，.，/，@，_，0-9，a-z，A-Z

encodeURI不编码字符有82个：!，#，$，&，'，(，)，*，+，,，-，.，/，:，;，=，?，@，_，~，0-9，a-z，A-Z

encodeURIComponent不编码字符有71个：!， '，(，)，*，-，.，_，~，0-9，a-z，A-Z
```

## encodeURI 和 encodeURIComponent
```
encodeURI 和 encodeURIComponent都是ECMA-262标准中定义的函数,所有兼容这个标准的语言（如JavaScript, ActionScript）都会实现这两个函数。它们都是用来对URI （RFC-2396）字符串进行编码的全局函数，但是它们的处理方式和使用场景有所不同。为了解释它们的不同，我们首先需要理解RFC-2396中对于 URI中的字符分类：

1>保留字符（reserved characters）：这类字符是URI中的保留关键字符，它们用于分割URI中的各个部分。这些字符是：";" | "/" | "?" | ":" | "@" | "&" | "=" | "+" | "$" | "," 

2>Mark字符（mark characters）：这类字符在RFC-2396中特别定义，但是没有特别说明用途，可能是和别的RFC标准相关。 这些字符是："-" | "_" | "." | "!" | "~" | "*" | "'" | "(" | ")" 
3>基本字符（alphanum characters）：这类字符是URI中的主体部分，它包括所有的大写字母、小写字母和数字。

在介绍完上面三类字符串后，我们就非常容易来解释encodeURI encodeURIComponent函数的不同之处了：
encodeURI: 该函数对传入字符串中的所有非（基本字符、Mark字符和保留字符）进行转义编码（escaping）。所有的需要转义的字符都按照UTF-8编码转化成 为一个、两个或者三个字节的十六进制转义字符（％xx）。例如，字符空格" "转换成为"%20"。在这种编码模式下面，需要编码的ASCII字符用一个字节转义字符代替，在\u0080和\u007ff之间的字符用两个字节转义字符代替，其他16为Unicode字符用三个字节转义字符代替。
encodeURIComponent: 该函数处理方式和encodeURI只有一个不同点，那就是对于保留字符同样做转义编码。这样url中的参数和值才不会被#等特殊字符截断。 比如：http://localhost:8080/xss/XssServlet?username=A&T Plastic，
```

encodeURI: 该函数对传入字符串中的所有非（基本字符、Mark字符和保留字符）进行转义编码（escaping）。所有的需要转义的字符都按照UTF-8编码转化成 为一个、两个或者三个字节的十六进制转义字符（％xx）。例如，字符空格" "转换成为"%20"。在这种编码模式下面，需要编码的ASCII字符用一个字节转义字符代替，在\u0080和\u007ff之间的字符用两个字节转义字符代替，其他16为Unicode字符用三个字节转义字符代替。

encodeURIComponent: 该函数处理方式和encodeURI只有一个不同点，那就是对于保留字符同样做转义编码。这样url中的参数和值才不会被#等特殊字符截断。

## escape
```remark
该方法不会对 ASCII 字母和数字进行编码,escape不编码字符有69个：*，+，-，.，/，@，_，0-9，a-z，A-Z
也不会对下面这些 ASCII 标点符号进行编码： * @ - _ + . / 。
其他所有的字符都会被转义序列替换。其它情况下escape，encodeURI，encodeURIComponent编码结果相同。
escape对0-255以外的unicode值进行编码时输出%u****格式,解码的话应使用 decodeURI() 和 decodeURIComponent() 替代它，
一般情况下，使用decodeURIComponent()进行解码
```
