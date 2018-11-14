# 关于html等等的value以及赋值文本的存储属性的说明

>一、 aspx或者其他后台传入双引号的数据可能引发的问题：
```csharp
//返回的OrderCode: 960"61"0"001   960"6''1"0"0''01  9606''100''01已经包含单引号和双引号的情况
A. public string GetOrderCode() 
=> new DataClasses1DataContext().orders.Where(c=>c.Id==1).SingleOrDefault().orderCode;

B. aspx页面调用：
<p style="background-color: yellow;" id="<%=GetOrderCode().Replace("\"","&quot;")%>" onclick="Sayhello('<%=GetOrderCode().Replace("\"","&quot;")%>','<%=GetOrderCode().Replace("\"","&quot;")%>')"
        title="<%=GetOrderCode().Replace("\"","&quot;")%>" class="orderCode"><%=GetOrderCode().Replace("\"","&quot;")%></p>
    <script>
        function Sayhello(contents, c2) {
            alert(contents + "-----" + c2);
        }
    </script>

注意：都是英文状态下的标点，html里对应的属性赋值(title="value",onclick="XX('ss','ss')")
    1. 传入数据包括双引号
    2. 或者对于onclick事件而言：可能内部还会包含单引号
    单引号的实体字符：IE是不支持的所以可以采用,并且单引号的实体字符和编号都不能
    以他本来的样子在前台显示，所以数据还是会错乱，导致函数无法使用.
    --所以视情况而定，如果遇到这种情况的话就直接采用编码的方式去处理获取到的数据，在具体使用到的时候进行解码， 或者界面显示的话：只需要使用&quot;替换双引号就行了.
    3.对于jqury的属性和id选择器的话：要正确的获取到元素，就需要进行JS转义字符的操作：单引号：\', 双引号：\\\"，。

    执行实体字符替换解决的话，只适用于：不包括单引号的情况，如果有单引号请采用，编码的方式解决.
```

> 二、针对动态添加Form标签实现数据的提交时处理Value值说明
```js
        //测试用例参考
        var CreateForm = document.createElement("form");
        CreateForm.action = "WebForm2.aspx";
        CreateForm.enctype = "application/x-www-form-urlencoded";//"multipart/form-data";
        CreateForm.id = "StepOneForm";
        CreateForm.setAttribute("Name", "StepOneForm");
        CreateForm.setAttribute("method", "post");
        CreateForm.innerHTML = InnerHTML;
        document.getElementById("StepOne").appendChild(CreateForm);
        document.getElementById("StepOneForm").submit();
        /*通过这种方式进行窗体post提交之后，由于引号的影响导致获取到的数据错误，导致传送的数据被自动截断*/
```


## value

    就是input这样的标签一个value值,会受 单引号以及双引号的影响导致数据的加载出错问题

### Demo01

```js
   var demo = "ab\"d\"d23"; //数值直接就是：非纯文本，数据包含了双引号，这个
   var InnerHTML = "<input  name=\"mPackage\" value=\"" + demo+ "\"/>";
   获取InnerHTML的数据的话： 
   所以不管m的值是什么最终获取的value数据就是正常的.
```

### Demo02

```js
   var demo = "ab$werwd23"; //数值直接就是：纯文本，数据不包含了双引号
   var InnerHTML = "<input  name=\"mPackage\" value=\"" + demo+ "\"/>";
   获取InnerHTML的数据的话： 就是标准的数据：InnerHTML: ab$werwd23.
```

### Demo03

```html
    var demo = "ab\"d\"d23"; //包含了双引号
    //但是下面的InnerHtml的外包括为单引号，内部数据为双引号--并且内部数据包含了单引号
   InnerHTML = "<input type='hidden' name='mPackage' value='" + encodeURIComponent(JSON.stringify(mPackageList)) + "'/>";
    获取InnerHTML的数据的话：自动被截断返回数据为:ab,后面的数据都被自动截断了就是获取数据信息的时候出现了错误
```

`注意`：demo03这样的情况解决方案
> 使用javascript的编码函数进行编码即可解决函数为：`encodeURIComponent`[encodeURIComponent](http://www.cnblogs.com/tylerdonet/p/3483836.html)
，但是encodeURIComponent编码的`原则为`：

    定义和用法
    encodeURIComponent() 函数可把字符串作为 URI 组件进行编码。

    语法
    encodeURIComponent(URIstring)

    参数  描述
    URIstring  必需。一个字符串，含有 URI 组件或其他要编码的文本。 

    返回值
    URIstring 的副本，其中的某些字符将被十六进制的转义序列进行替换。

    说明
    该方法不会对 ASCII 字母和数字进行编码，也不会对这些 ASCII 标点符号进行编码： - _ . ! ~ * ' ( ) 。

    其他字符（比如 ：;/?:@&=+$,# 这些用于分隔 URI 组件的标点符号），都是由一个或多个十六进制的转义序列替换的。

    提示和注释
    提示：请注意 encodeURIComponent() 函数 与 encodeURI() 函数的区别之处，前者假定它的参数是 
    URI 的一部分（比如协议、主机名、路径或查询字符串）。     
    因此 encodeURIComponent() 函数将转义用于分隔 URI 各个部分的标点符号。
    也就是说： 对于单引号的话是不会进行编码操作的所以，`必须使用，双引号进行文本的bracket的操作，包括起来就可以了`


## 总结

> 如果对应的html拼接数据，为后台提供的话，注意对单双引号的处理，遇到特殊的情况，
需要进行编码处理，以保证数据的准确性.
>如：InnerHTML = "<input  name=\"mPackage\" value=\"" + demo+ "\"/>";其实就是：对于post数据的或者其他后台拼接的话都建议，全部先使用双引号
>然后根据实际情况，是否进行编码-主要针对单引号的情况，然后进行处理。
执行实体字符替换解决的话，只适用于：不包括单引号的情况，如果有单引号请采用，编码的方式解决.
