---
layout: default
title: IE6/7中getAttribute及jquery.attr与prop
---
##{{page.title}}

###起因
同事发现 IE6 下,
```html
<a href="/a/b" id="link_a">link</a>
```
```javascript
    var href=$('#link_a').attr('href');
    //值与期望不同,chrome下,attr() 操作只取出原始html文本
    //但是 ie6 下会将完整的Url返回
    //jquery 中 suport.hrefNomalized 检测浏览器是否自动完成这一动作,此时版本的jquery.attr()
```

####?如果非a元素使用了href是否也会补全`

###溯源

SD9006: IE 混淆了 DOM 对象属性（property）及 HTML 标签属性（attribute），造成了对 setAttribute、getAttribute 的不正确实现
>根据 DOM (Core) Level 1 规范中的描述，getAttribute 与 setAttribute 为 Element 接口下的方法，其功能分别是通过 "name" 获取和设置一个元素的属性（attribute）值。getAttribute 方法会以字符串的形式返回属性值，若该属性没有设定值或缺省值则返回空字符串。setAttribute 方法则无返回值。
在 DOM Level 2 规范中，则更加明确了 getAttribute 与 setAttribute 方法参数中的 "name" 的类型为 DOMString，setAttribute 方法参数中的 "value" 的类型为 DOMString，getAttribute 的返回值类型也为 DOMString。

>HTML 文档中的 DOM 对象的属性（property）被定义在 DOM (HTML) 规范中。这个规范中明确定义了 document 对象以及所有标准的 HTML 元素所对应的 DOM 对象拥有的属性及方法。

>因为在早期的 DOM Level 0 阶段，某些 HTML 标签的属性会将其值暴露给对应的 DOM 对象的属性，如 HTML 元素的 id 属性与其对应的 DOM 对象的 id 属性会保持一种同步关系，然而这种方式目前已经废弃，这是由于它不能被扩展到所有可能存在的 XML 的属性名称。W3C 建议使用 DOM (Core) 中 Element 接口上定义的通用方法去获取（getting）、设置（setting）及删除（removing）元素的属性。

####验证 自定义属性转成 DOM 对象属性 现代浏览器不支持,ele.propname <=!=> ele.get/setAttribute('propname');
```html
    <a id="link_b" mid='mid'>link b</a>
```
```javascript
   var mid=$('#link_b')[0].mid;
   var mid=$('#link_b').attr('mid');
```
###prop(),attr()的区别
prop(),jquery.1.6引入,解决之前attr一锅端访问静态/动态属性造成的混乱
严格区分了静态属性和动态属性:静态属性/html/自定义属性,DOM 元素的属性
通常可以认为:
$('xxx').prop(name,value)=>$('xxx')[0][name]=value;
$('xxx').attr(name,value)=>$('xxx')[0].get/setAttribute(name,value);
在自定义属性时是有区别的.(不属于html默认属性时),只做简单的键值存储;如果是w3chtml规定的属性,浏览器会解析并应用新值改变dom元素在浏览器中的样式/行为等待而产生作用.
$('xxx').prop('propname','aaa')//不能被$('xxx').attr('propname')检索到,反之同理.

$('a').prop('href','aa.html');//$('a').prop('href')==>http://xxx/aa.html

如果被设置的属性要影响到dom的表现/行为而不只是简单存储数据,则应使用prop


##延伸
是否引用外部资源的时候都有这样的问题
```html
<img src="a.png" />
<link href="a.css" rel="stylesheet"/>
<script src="a.js"></script>
<iframe src="a.html"></iframe>
<embed src="a.swf" />
```
