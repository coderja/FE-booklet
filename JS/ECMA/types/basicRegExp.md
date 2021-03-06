# javascript中正则表达式的基础语法

&emsp;&emsp;正则表达式在人们的印象中可能是一堆无法理解的字符，但就是这些符号却实现了字符串的高效操作。通常的情况是，问题本身并不复杂，但没有正则表达式就成了大问题。javascript中的正则表达式作为相当重要的知识，本文将介绍正则表达式的基础语法

&nbsp;

### 定义

&emsp;&emsp;正则表达式(Regular Expression)是一门简单语言的语法规范，是强大、便捷、高效的文本处理工具，它应用在一些方法中，对字符串中的信息实现查找、替换和提取操作

&emsp;&emsp;javascript中的正则表达式用RegExp对象表示，有两种写法：一种是字面量写法；另一种是构造函数写法

**Perl写法**

&emsp;&emsp;正则表达式字面量写法，又叫Perl写法，因为javascript的正则表达式特性借鉴自Perl

&emsp;&emsp;正则表达式字面量定义为包含在一对斜杠`/`之间的字符，并且可以设置3个标志

<div>
<pre>var expression = /pattern/flags;</pre>
</div>

&emsp;&emsp;正则表达式的匹配模式支持下列3个标志：

&emsp;&emsp;g:表示全局(global)模式，即模式将被应用于所有字符串，而并非在发现第一个匹配项时立即停止

&emsp;&emsp;i:表示不区分大小写(case-insensitive)模式，即在确定匹配项时忽略模式与字符串的大小写

&emsp;&emsp;m:表示多行(multiline)模式，即在到达一行文本末尾时还会继续查找下一行中是否存在与模式匹配的项

<div>
<pre>//匹配字符串所有'at'的实例
var p = /at/g;
//test()方法返回一个布尔值表示是否可以找到匹配项
console.log(p.test('ata'));//true
console.log(p.test('aba'));//false</pre>
</div>

**RegExp构造函数**

&emsp;&emsp;和普通的内置对象一样，RegExp正则表达式对象也支持new RegExp()构造函数的形式

&emsp;&emsp;RegExp构造函数接收两个参数：要匹配的字符串模式(pattern)和可选的标志字符串(flags)，标志字符串和字面量的三个标志含义相同：'g'、'i'、'm'

&emsp;&emsp;RegExp构造函数的两个参数都是字符串。且使用字面量形式定义的任何表达式都可使用构造函数

<div>
<pre>//匹配字符串所有'at'的实例
var p1 = /at/g;
//同上
var p2 = new RegExp('at','g');</pre>
</div>

&emsp;&emsp;注意：ES3规范规定，一个正则表达式直接量会在执行到它时转换为一个RegExp对象，同一段代码所表示正则表达式直接量的每次运算都返回同一个对象。ES5规范则做了相反的规定，同一段代码所表示的正则表达式直接量的每次运算都返回新对象。IE6-8一直是按照ECMAScript5规范的方式实现的，所以并没有兼容性问题

&nbsp;

### 特点

&emsp;&emsp;javascript中的正则表达式最大的特点是不支持空白，必须写在一行中

<div>
<pre>//匹配ab
console.log(/ab/.test('ab')); //true
console.log(/ ab/.test('ab')); //false
console.log(/a b/.test('ab')); //false
console.log(/ab /.test('ab')); //false</pre>
</div>

&nbsp;

### 元字符

&emsp;&emsp;大部分字符在正则表达式中，就是字面的含义，比如/a/匹配a，/b/匹配b

<div>
<pre>/dog/.test("old dog") // true</pre>
</div>

&emsp;&emsp;但还有一些字符，它们除了字面意思外，还有着特殊的含义，这些字符就是元字符

&emsp;&emsp;在javascript中，共有14个元字符(meta-character)

<div>
<pre>() [] {} \ ^ $ | ? * + . </pre>
</div>
<div>
<pre>元字符         名称              匹配对象
.             点号               单个任意字符(除回车\r、换行\n、行分隔符\u2028和段分隔符\u2029外)
[]            字符组             列出的单个任意字符
[^]           排除型字符组        未列出的单个任意字符
?             问号               匹配0次或1次
*             星号               匹配0次或多次
+             加号               匹配1次或多次
{min,max}     区间量词            匹配至少min次，最多max次
^             脱字符             行的起始位置
$             美元符             行的结束位置
|             竖线               分隔两边的任意一个表达式
()            括号               限制多选结构的范围，标注量词作用的元素，为反向引用捕获文本
\1,\2...      反向引用            匹配之前的第一、第二...组括号内的表达式匹配的文本</pre>
</div>

&nbsp;

### 转义字符

&emsp;&emsp;转义字符(escape)表示为反斜线`\`+字符的形式，共有以下3种情况

&emsp;&emsp;【1】因为元字符有特殊的含义，所以无法直接匹配。如果要匹配它们本身，则需要在它们前面加上反斜杠`\`

<div>
<pre>console.log(/1+1/.test('1+1')); //false
console.log(/1\+1/.test('1+1')); //true
console.log(/\*/.test('*')); //true
console.log(/*/.test('*')); //报错
</pre>
</div>

&emsp;&emsp;但实际上，并非14个元字符都需要转义，右方括号]和右花括号}不需要转义

<div>
<pre>console.log(/]/.test(']')); //true
console.log(/\]/.test(']')); //true
console.log(/\}/.test('}')); //true
console.log(/}/.test('}')); //true</pre>
</div>

&emsp;&emsp;【2】`\`加非元字符，表示一些不能打印的特殊字符

<div>
<pre>\0        NUL字符\u0000
[\b]      匹配退格符\u0008，不要与\b混淆
\t        制表符\u0009
\n        换行符\u000A
\v        垂直制表符\u000B
\f        换页符\u000C
\r        回车符\u000D
\xnn      由十六进制数nn指定的拉丁字符
\uxxxx    由十六进制数xxxx指定的Unicode字符(\u4e00-\u9fa5代表中文)  
\cX       控制字符^X，表示ctrl-[X]，其中的X是A-Z之中任一个英文字母，用来匹配控制字符</pre>
</div>

&emsp;&emsp;【3】`\`加任意其他字符，默认情况就是匹配此字符，也就是说，反斜线`\`被忽略了

<div>
<pre>console.log(/\x/.test('x')); //true
console.log(/\y/.test('y')); //true
console.log(/\z/.test('z')); //true</pre>
</div>

**双重转义**

&emsp;&emsp;由于RegExp构造函数的参数是字符串，所以某些情况下，需要对字符进行双重转义

&emsp;&emsp;字符`\`在正则表达式字符串中通常被转义为`\\`

<div>
<pre>var p1 = /\.at/;
//等价于
var p2 = new RegExp('\\.at');
var p1 = /name\/age/;
//等价于
var p2 = new RegExp('name\\/age');
var p1 = /\w\\hello\\123/;
//等价于
var p2 = new RegExp('\\w\\\\hello\\\\123');</pre>
</div>

&nbsp;

### 字符组

&emsp;&emsp;字符组(Character Class)，有的编译成字符类或字符集。简单而言，就是指用方括号表示的一组字符，它匹配若干字符之一

<div>
<pre>//匹配0-9这10个数字之一
var p = /[0123456789]/;
p.test('1');//true
p.test('a');//false</pre>
</div>

&emsp;&emsp;注意：字符组中的字符排列顺序并不影响字符组的功能，出现重复字符也不会影响

<div>
<pre>/[0123456789]/
//等价于
/[9876543210]/
//等价于
/[1234567890123456789]/</pre>
</div>

**范围**

&emsp;&emsp;正则表达式通过连字符(-)提供了范围表示法，可以简化字符组

<div>
<pre>/[0123456789]/
//等价于
/[0-9]/</pre>
</div>
<div>
<pre>/[abcdefghijklmnopqrstuvwxyz]/
//等价于
/[a-z]/</pre>
</div>

&emsp;&emsp;连字符(-)表示的范围是根据ASCII编码的码值来确定的，码值小的在前，码值大的在后

![localeCompare](https://pic.xiaohuochai.site/blog/JS_ECMA_grammer_localeCompare.gif)

&emsp;&emsp;所以[0-9]是合法的，而[9-0]会报错

<div>
<pre>//匹配0-9这10个数字之一
var p1 = /[0-9]/;
p1.test('1');//true
var p2 = /[9-0]/;//报错
p2.test('1');</pre>
</div>

&emsp;&emsp;在字符组中可以同时并列多个'-'范围

<div>
<pre>/[0-9a-zA-Z]/;//匹配数字、大写字母和小写字母
/[0-9a-fA-F]/;//匹配数字，大、小写形式的a-f，用来验证十六进制字符

console.log(/[0-9a-fA-F]/.test('d'));//true
console.log(/[0-9a-fA-F]/.test('x'));//false</pre>
</div>

&emsp;&emsp;只有在字符组内部，连字符'-'才是元字符，表示一个范围，否则它就只能匹配普通的连字符号

&emsp;&emsp;注意：如果连字符出现在字符组的开头或末尾，它表示的也是普通的连字符号，而不是一个范围

<div>
<pre>//匹配中划线
console.log(/-/.test('-'));//true
console.log(/[-]/.test('-'));//true

//匹配0-9的数字或中划线
console.log(/[0-9]/.test('-'));//false
console.log(/[0-9-]/.test('-'));//true
console.log(/[0-9\-]/.test('-'));//true
console.log(/[-0-9]/.test('-'));//true
console.log(/[\-0-9]/.test('-'));//true</pre>
</div>

**排除**

&emsp;&emsp;字符组的另一个类型是排除型字符组，在左方括号后紧跟一个脱字符'^'表示，表示在当前位置匹配一个没有列出的字符&nbsp;

&emsp;&emsp;所以[^0-9]表示0-9以外的字符

<div>
<pre>//匹配第一个是数字字符，第二个不是数字字符的字符串
console.log(/[0-9][^0-9]/.test('1e'));//true
console.log(/[0-9][^0-9]/.test('q2'));//false</pre>
</div>

&emsp;&emsp;注意：在字符组内部，脱字符'^'表示排除，而在字符组外部，脱字符'^'表示一个行锚点

&emsp;&emsp;^符号是元字符，在字符组中只要^符号不挨着左方括号就可以表示其本身含义，不转义也可以

<div>
<pre>//匹配abc和^符号
console.log(/[a-c^]/.test('^'));//true
console.log(/[a-c\^]/.test('^'));//true
console.log(/[\^a-c]/.test('^'));//true</pre>
</div>

&emsp;&emsp;在字符组中，只有^、-、[、]这4个字符可能被当做元字符，其他有元字符功能的字符都只表示其本身

<div>
<pre>console.log(/[[1]]/.test('['));//false
console.log(/[[1]]/.test(']'));//false
console.log(/[\1]/.test('\\'));//false
console.log(/[^^]/.test('^'));//false
console.log(/[1-2]/.test('-'));//false
console.log(/[\[1\]]/.test('['));//true
console.log(/[\[1\]]/.test(']'));//true
console.log(/[\\]/.test('\\'));//true
console.log(/[^]/.test('^'));//true
console.log(/[1-2\-]/.test('-'));//true</pre>
</div>
<div>
<pre>console.log(/[(1)]/.test('('));//true
console.log(/[(1)]/.test(')'));//true
console.log(/[{1}]/.test('{'));//true
console.log(/[{1}]/.test('}'));//true
console.log(/[1$]/.test('$'));//true
console.log(/[1|2]/.test('|'));//true
console.log(/[1?]/.test('?'));//true
console.log(/[1*]/.test('*'));//true
console.log(/[1+]/.test('+'));//true
console.log(/[1.]/.test('.'));//true</pre>
</div>

**简记**

&emsp;&emsp;用[0-9]、[a-z]等字符组，可以很方便地表示数字字符和小写字母字符。对于这类常用字符组，正则表达式提供了更简单的记法，这就是字符组简记(shorthands)

&emsp;&emsp;常见的字符组简记有\d、\w、\s。其中d表示(digit)数字，w表示(word)单词，s表示(space)空白

&emsp;&emsp;正则表达式也提供了对应排除型字符组的简记法：\D、\W、\S。字母完全相同，只是改为大写，这些简记法匹配的字符互补

<div>
<pre>\d     数字，等同于[0-9]
\D     非数字，等同于[^0-9]
\s     空白字符，等同于[\f\n\r\t\u000B\u0020\u00A0\u2028\u2029]
\S     非空白字符，等同于[^\f\n\r\t\u000B\u0020\u00A0\u2028\u2029]
\w     字母、数字、下划线，等同于[0-9A-Za-z_](汉字不属于\w)
\W     非字母、数字、下划线，等同于[^0-9A-Za-z_]</pre>
</div>

&emsp;&emsp;注意：\w不仅包括字母、数字，还包括下划线。在进行数字验证时，只允许输入字母和数字时，不可以使用\w，而应该使用[0-9a-zA-Z]

**任意字符**

&emsp;&emsp;人们一般认为点号可以代表任意字符，其实并不是

&emsp;&emsp;.点号代表除回车(\r)、换行(\n) 、行分隔符(\u2028)和段分隔符(\u2029)以外的任意字符

&emsp;&emsp;妥善的利用互补属性，可以得到一些巧妙的效果。比如，[\s\S]、[\w\W]、[\d\D]都可以表示任意字符

<div>
<pre>//匹配任意字符
console.log(/./.test('\r'));//false
console.log(/[\s\S]/.test('\r'));//true</pre>
</div>

&nbsp;

### 量词

&emsp;&emsp;根据字符组的介绍，可以用字符组[0-9]或\d来匹配单个数字字符，如果用正则表达式表示更复杂的字符串，则不太方便

<div>
<pre>//表示邮政编码6位数字
/[0-9][0-9][0-9][0-9][0-9][0-9]/;
//等价于
/\d\d\d\d\d\d/;</pre>
</div>

&emsp;&emsp;正则表达式提供了量词，用来设定某个模式出现的次数

<div>
<pre>{n}       匹配n次
{n,m}     匹配至少n次，最多m次
{n,}      匹配至少n次
?         相当于{0,1}
*         相当于{0,}
+         相当于{1,}</pre>
</div>
<div>
<pre>//表示邮政编码6位数字
/\d{6}/;</pre>
</div>

&emsp;&emsp;美国英语和英国英语有些词的写法不一样，如果traveler和traveller，favor和favour，color和colour

<div>
<pre>//同时匹配美国英语和英国英语单词
/travell?er/;
/favou?r/;
/colou?r/;</pre>
</div>

&emsp;&emsp;协议名有http和https两种

<div>
<pre>/https?/;</pre>
</div>

&emsp;&emsp;量词广泛应用于解析HTML代码。HTML是一种标签语言，它包含各种各样的标签，比如&lt;head&gt;、&lt;img&gt;、&lt;table&gt;。它们都是从&lt;开始，到&gt;结束，而标签名的长度不同

<div>
<pre>console.log(/&lt;[^&lt;&gt;]+&gt;/.test('&lt;head&gt;'));//true
console.log(/&lt;[^&lt;&gt;]+&gt;/.test('&lt;img&gt;'));//true
console.log(/&lt;[^&lt;&gt;]+&gt;/.test('&lt;&gt;'));//false</pre>
</div>

&emsp;&emsp;HTML标签不能为空标签，而引号字符串的两个引号之间可以为0个字符

<div>
<pre>console.log(/'[^']*'/.test("''"));//true
console.log(/'[^']*'/.test("'abc'"));//true</pre>
</div>

**贪婪模式**

&emsp;&emsp;默认情况下，量词都是贪婪模式(greedy quantifier)，即匹配到下一个字符不满足匹配规则为止

<div>
<pre>//exec()方法以数组的形式返回匹配结果
/a+/.exec('aaa'); // ['aaa']</pre>
</div>

**懒惰模式**

&emsp;&emsp;懒惰模式(lazy quantifier)和贪婪模式相对应，在量词后加问号'?'表示，表示尽可能少的匹配，一旦条件满足就再不往下匹配

<div>
<pre>{n}?       匹配n次
{n,m}?     匹配至少n次，最多m次
{n,}?      匹配至少n次
??         相当于{0,1}
*?         相当于{0,}
+?         相当于{1,}</pre>
</div>
<div>
<pre>/a+?/.exec('aaa'); // ['a']</pre>
</div>

&emsp;&emsp;匹配&lt;script&gt;&lt;/script&gt;之间的代码看上去很容易

<div>
<pre>/&lt;script&gt;[\s\S]*&lt;\/script&gt;/</pre>
</div>
<div>
<pre>//["&lt;script&gt;alert("1");&lt;/script&gt;"]
/&lt;script&gt;[\s\S]*&lt;\/script&gt;/.exec('&lt;script&gt;alert("1");&lt;/script&gt;'); </pre>
</div>

&emsp;&emsp;但如果多次出现script标签，就会出问题

<div>
<pre>//["&lt;script&gt;alert("1");&lt;/script&gt;&lt;br&gt;&lt;script&gt;alert("2");&lt;/script&gt;"]
/&lt;script&gt;[\s\S]*&lt;\/script&gt;/.exec('&lt;script&gt;alert("1");&lt;/script&gt;&lt;br&gt;&lt;script&gt;alert("2");&lt;/script&gt;');     </pre>
</div>

&emsp;&emsp;它把无用的&lt;br&gt;标签也匹配出来了，此时就需要使用懒惰模式

<div>
<pre>//["&lt;script&gt;alert("1");&lt;/script&gt;"]
/&lt;script&gt;[\s\S]*?&lt;\/script&gt;/.exec('&lt;script&gt;alert("1");&lt;/script&gt;&lt;br&gt;&lt;script&gt;alert("2");&lt;/script&gt;');     </pre>
</div>

&emsp;&emsp;在javascript中，/* */是注释的一种形式，在文档中可能出现多次，这时就必须使用懒惰模式

<div>
<pre>/\/\*[\s\S]*?\*\//</pre>
</div>
<div>
<pre>//["/*abc*/"]
/\/\*[\s\S]*?\*\//.exec('/*abc*/&lt;br&gt;/*123*/');</pre>
</div>

&nbsp;

### 括号

&emsp;&emsp;括号有两个功能，分别是分组和引用。具体而言，用于限定量词或选择项的作用范围，也可以用于捕获文本并进行引用或反向引用

**分组**

&emsp;&emsp;量词控制之前元素的出现次数，而这个元素可能是一个字符，也可能是一个字符组，也可以是一个表达式

&emsp;&emsp;如果把一个表达式用括号包围起来，这个元素就是括号里的表达式，被称为子表达式

&emsp;&emsp;如果希望字符串'ab'重复出现2次，应该写为(ab){2}，而如果写为ab{2}，则{2}只限定b

<div>
<pre>console.log(/(ab){2}/.test('abab'));//true
console.log(/(ab){2}/.test('abb'));//false
console.log(/ab{2}/.test('abab'));//false
console.log(/ab{2}/.test('abb'));//true</pre>
</div>

&emsp;&emsp;身份证长度有15位和18位两种，如果只匹配长度，可能会想当然地写成\d{15,18}，实际上这是错误的，因为它包括15、16、17、18这四种长度。因此，正确的写法应该是\d{15}(\d{3})?

&emsp;&emsp;email地址以@分隔成两段，之前的部分是用户名，之后的部分是主机名

&emsp;&emsp;用户名允许出现数字、字母和下划线，长度一般在1-64个字符之间，则正则可表示为/\w{1,64}/

&emsp;&emsp;主机名一般表现为a.b.&middot;&middot;&middot;.c，其中c为主域名，其他为级数不定的子域名，则正则可表示为/([-a-zA-z0-9]{1,63}\.)+[-a-zA-Z0-9]{1,63}/

&emsp;&emsp;所以email地址的正则表达式如下：

<div>
<pre>    var p =/\w{1,64}@([-a-zA-z0-9]{1,63}\.)+[-a-zA-Z0-9]{1,63}/;
    console.log(p.test('q@qq.com'));//true
    console.log(p.test('q@qq'));//false
    console.log(p.test('q@a.qq.com'));//true</pre>
</div>

**捕获**

&emsp;&emsp;括号不仅可以对元素进行分组，还会保存每个分组匹配的文本，等到匹配完成后，引用捕获的内容。因为捕获了文本，这种功能叫捕获分组

&emsp;&emsp;比如，要匹配诸如2016-06-23这样的日期字符串

<div>
<pre>/(\d{4})-(\d{2})-(\d{2})/</pre>
</div>

&emsp;&emsp;与以往不同的是，年、月、日这三个数值被括号括起来了，从左到右为第1个括号、第2个括号和第3个括号，分别代表第1、2、3个捕获组

&emsp;&emsp;javascript有9个用于存储捕获组的构造函数属性

<div>
<pre>RegExp.$1\RegExp.$2\RegExp.$3&hellip;&hellip;到RegExp.$9分别用于存储第一、第二&hellip;&hellip;第九个匹配的捕获组。在调用exec()或test()方法时，这些属性会被自动填充</pre>
</div>
<div>
<pre>console.log(/(\d{4})-(\d{2})-(\d{2})/.test('2016-06-23'));//true
console.log(RegExp.$1);//'2016'
console.log(RegExp.$2);//'06'
console.log(RegExp.$3);//'23'
console.log(RegExp.$4);//''</pre>
</div>

&emsp;&emsp;而exec()方法是专门为捕获组而设计的，返回的数组中，第一项是与整个模式匹配的字符串，其他项是与模式中的捕获组匹配的字符串

<div>
<pre>console.log(/(\d{4})-(\d{2})-(\d{2})/.exec('2016-06-23'));//["2016-06-23", "2016", "06", "23"]</pre>
</div>

&emsp;&emsp;捕获分组捕获的文本，不仅可以用于数据提取，也可以用于替换

&emsp;&emsp;replace()方法就是用于进行数据替换的，该方法接收两个参数，第一个参数为待查找的内容，而第二个参数为替换的内容

<div>
<pre>console.log('2000-01-01'.replace(/-/g,'.'));//2000.01.01</pre>
</div>

&emsp;&emsp;在replace()方法中也可以引用分组，形式是$num，num是对应分组的编号

<div>
<pre>//把2000-01-01的形式变成01-01-2000的形式
console.log('2000-01-01'.replace(/(\d{4})-(\d{2})-(\d{2})/g,'$3-$2-$1'));//'01-01-2000'</pre>
</div>

**反向引用**

&emsp;&emsp;英文中不少单词都有重叠出现的字母，如shoot或beep。若想检查某个单词是否包含重叠出现的字母，则需要引入反向引用(back-reference)

&emsp;&emsp;反向引用允许在正则表达式内部引用之前捕获分组匹配的文本，形式是\num，num表示所引用分组的编号

<div>
<pre>//重复字母
/([a-z])\1/
console.log(/([a-z])\1/.test('aa'));//true
console.log(/([a-z])\1/.test('ab'));//false</pre>
</div>

&emsp;&emsp;反向引用可以用于建立前后联系。HTML标签的开始标签和结束标签是对应的

<div>
<pre>//开始标签
&lt;([^&gt;]+)&gt;
//标签内容
[\s\S]*?
//匹配成对的标签
/&lt;([^&gt;]+)&gt;[\s\S]*?&lt;\/\1&gt;/
console.log(/&lt;([^&gt;]+)&gt;[\s\S]*?&lt;\/\1&gt;/.test('&lt;a&gt;123&lt;/a&gt;'));//true
console.log(/&lt;([^&gt;]+)&gt;[\s\S]*?&lt;\/\1&gt;/.test('&lt;a&gt;123&lt;/b&gt;'));//false</pre>
</div>

**非捕获**

&emsp;&emsp;除了捕获分组，正则表达式还提供了非捕获分组(non-capturing group)，以(?:)的形式表示，它只用于限定作用范围，而不捕获任何文本

&emsp;&emsp;比如，要匹配abcabc这个字符，一般地，可以写为(abc){2}，但由于并不需要捕获文本，只是限定了量词的作用范围，所以应该写为(?:abc){2}

<div>
<pre>console.log(/(abc){2}/.test('abcabc'));//true
console.log(/(?:abc){2}/.test('abcabc'));//true</pre>
</div>

&emsp;&emsp;由于非捕获分组不捕获文本，对应地，也就没有捕获组编号

<div>
<pre>console.log(/(abc){2}/.test('abcabc'));//true
console.log(RegExp.$1);//'abc'
console.log(/(?:abc){2}/.test('abcabc'));//true
console.log(RegExp.$1);//''</pre>
</div>

&emsp;&emsp;非捕获分组也不可以使用反向引用

<div>
<pre>/(?:123)\1/.test('123123');//false
/(123)\1/.test('123123');//true</pre>
</div>

&emsp;&emsp;捕获分组和非捕获分组可以在一个正则表达式中同时出现

<div>
<pre>console.log(/(\d)(\d)(?:\d)(\d)(\d)/.exec('12345'));//["12345", "1", "2", "4", "5"]</pre>
</div>

&nbsp;

### 选择

 &emsp;&emsp;竖线'|'在正则表达式中表示或(OR)关系的选择，以竖线'|'分隔开的多个子表达式也叫选择分支或选择项。在一个选择结构中，选择分支的数目没有限制

&emsp;&emsp;在选择结构中，竖线|用来分隔选择项，而括号()用来规定整个选择结构的范围。如果没有出现括号，则将整个表达式视为一个选择结构

&emsp;&emsp;选择项的尝试匹配次序是从左到右，直到发现了匹配项，如果某个选择项匹配就忽略右侧其他选择项，如果所有子选择项都不匹配，则整个选择结构匹配失败

<div>
<pre>console.log(/12|23|34/.exec('1'));//null
console.log(/12|23|34/.exec('12'));//['12']
console.log(/12|23|34/.exec('23'));//['23']
console.log(/12|23|34/.exec('2334'));//['23']</pre>
</div>

&emsp;&emsp;ip地址一般由3个点号和4段数字组成，每段数字都在0-255之间

<div>
<pre>(00)?\d; //0-9
0?[1-9]\d;//10-99
1\d{2};//100-199
2[0-4]\d;//200-249
25[0-5];//250-255
//数字(0-255)
/(00)?\d|0?[1-9]\d|1\d{2}|2[0-4]\d|25[0-5]/;</pre>
</div>
<div>
<pre>//ip地址
var ip = /^(((00)?\d|0?[1-9]\d|1\d{2}|2[0-4]\d|25[0-5])\.){3}\2$/;
console.log(ip.test('1.1.1.1'));//true
console.log(ip.test('1.1.1'));//false
console.log(ip.test('256.1.1.1'));//false</pre>
</div>

&emsp;&emsp;类似地，时间匹配也需要分段处理

<div>
<pre>//月(1-12)
0?\d|1[0-2]
//日(1-31)
0?\d|[12]\d|3[01]
//小时(0-24)
0?\d|1\d|2[0-4]
//分钟(0-60)
0?\d|[1-5]\d|60</pre>
</div>

&emsp;&emsp;手机号一般是11位，前3位是号段，后8位一般没有限制。而且，在手机开头很可能有0或+86

<div>
<pre>//开头
(0|\+86)?
//前3位
13\d|14[579]|15[0-35-9]|17[0135-8]|18\d
//后8位
\d{8}
//手机号码
var phone = /(0|\+86)?(13\d|14[579]|15[0-35-9]|17[0135-8]|18\d)\d{8}/;
console.log(phone.test('13453250661'));//true
console.log(phone.test('1913250661'));//false
console.log(phone.test('1345325061'));//false</pre>
</div>

&emsp;&emsp;在选择结构中，应该尽量避免选择分支中存在重复匹配，因为这样会大大增加回溯的计算量

<div>
<pre>//不良的选择结构
a|[ab]
[0-9]|\w</pre>
</div>

&nbsp;

### 断言

&emsp;&emsp;在正则表达式中，有些结构并不真正匹配文本，而只负责判断在某个位置左/右侧是否符合要求，这种结构被称为断言(assertion)，也称为锚点(anchor)，常见的断言有3种：单词边界、行开头结尾、环视

**单词边界**

&emsp;&emsp;在文本处理中可能会经常进行单词替换，比如把row替换成line。但是，如果直接替换，不仅所有单词row都被替换成line，单词内部的row也会被替换成line。要想解决这个问题，必须有办法确定单词row，而不是字符串row

&emsp;&emsp;为了解决这类问题，正则表达式提供了专用的单词边界(word boundary)，记为\b，它匹配的是'单词边界'位置，而不是字符。\b匹配的是一边是单词字符\w，一边是非单词字符\W的位置

&emsp;&emsp;与\b对应的还有\B，表示非单词边界，但实际上\B很少使用

<div>
<pre>console.log(/\ban\b/.test('an apple'));//true
console.log(/\ban\b/.test('a an'));//true
console.log(/\ban\b/.test('an'));//true
console.log(/\ban\b/.test('and'));//false
console.log(/\ban\b/.test('ban'));//false</pre>
</div>

**起始结束**

&emsp;&emsp;常见的断言还有^和$，它们分别匹配字符串的开始位置和结束位置，所以可以用来判断整个字符串能否由表达式匹配

<div>
<pre>//匹配第一个单词
console.log(/^\w*/.exec('first word\nsecond word\nthird word'));//['first']
//匹配最后一个单词
console.log(/\w*$/.exec('first word\nsecond word\nthird word'));//['word']</pre>
</div>
<div>
<pre>console.log(/^a$/.test('a\n'));//false
console.log(/^a$/.test('a'));//true</pre>
</div>

&emsp;&emsp;^和$的常用功能是删除字符串首尾多余的空白，类似于字符串String对象的trim()方法

<div>
<pre>function fnTrim(str){
    return str.replace(/^\s+|\s+$/,'')
}  
console.log(fnTrim('      hello world   '));//'hello world'</pre>
</div>

**环视**

&emsp;&emsp;环视(look-around)，可形象地解释为停在原地，四处张望。环视类似于单词边界，在它旁边的文本需要满足某种条件，而且本身不匹配任何字符

&emsp;&emsp;环视分为正序环视和逆序环视，而javascript只支持正序环视，相当于只支持向前看，不支持往回看

&emsp;&emsp;而正序环视又分为肯定正序环视和否定正序环视

&emsp;&emsp;肯定正序环视的记法是(?=n)，表示前面必须是n才匹配；否定正序环视的记忆法是(?!n)，表示前面必须不是n才匹配

<div>
<pre>console.log(/a(?=b)/.exec('abc'));//['a']
console.log(/a(?=b)/.exec('ac'));//null
console.log(/a(?!b)/.exec('abc'));//null
console.log(/a(?!b)/.exec('ac'));//['a']
console.log(/a(?=b)b/.exec('abc'));//['ab']</pre>
</div>

&emsp;&emsp;注意：环视虽然也用到括号，却与捕获型分组编号无关；但如果环视结构出现捕获型括号，则会影响分组

<div>
<pre>console.log(/ab(?=cd)/.exec('abcd'));['ab']
console.log(/ab(?=(cd))/.exec('abcd'));['ab','cd']</pre>
</div>

&nbsp;

### 匹配模式

&emsp;&emsp;匹配模式(match mode)指匹配时使用的规则。设置特定的模式，可能会改变对正则表达式的识别。前面已经介绍过创建正则表达式对象时，可以设置'm'、'i'、'g'这三个标志，分别对应多行模式、不区分大小模式和全局模式三种

**i**

&emsp;&emsp;默认地，正则表达式是区分大小写的，通过设置标志'i'，可以忽略大小写(ignore case)

<div>
<pre>console.log(/ab/.test('aB'));//false
console.log(/ab/i.test('aB'));//true</pre>
</div>

**m**

&emsp;&emsp;默认地，正则表达式中的^和$匹配的是整个字符串的起始位置和结束位置，而通过设置标志'm'，开启多行模式，它们也能匹配字符串内部某一行文本的起始位置和结束位置

<div>
<pre>console.log(/world$/.test('hello world\n')); //false
console.log(/world$/m.test('hello world\n')); //true
console.log(/^b/.test('a\nb')); //false
console.log(/^b/m.test('a\nb')); //true</pre>
</div>

**g**

&emsp;&emsp;默认地，第一次匹配成功后，正则对象就停止向下匹配了。g修饰符表示全局匹配(global)，设置'g'标志后，正则对象将匹配全部符合条件的结果，主要用于搜索和替换

<div>
<pre>console.log('1a,2a,3a'.replace(/a/,'b'));//'1b,2a,3a'
console.log('1a,2a,3a'.replace(/a/g,'b'));//'1b,2b,3b'</pre>
</div>

&nbsp;

### 优先级

&emsp;&emsp;正则表达式千变万化，都是由之前介绍过的字符组、括号、量词等基本结构组合而成的

<div>
<pre>//从上到下，优先级逐渐降低
\                            转义符
() (?!) (?=) []              括号、字符组、环视
* + ? {n} {n,} {n,m}         量词
^ $                          起始结束位置
|                            选择</pre>
</div>

&emsp;&emsp;由于括号的用途之一就是为量词限定作用范围，所以优先级比量词高

<div>
<pre>console.log(/ab{2}/.test('abab'));//false
console.log(/(ab){2}/.test('abab'));//true</pre>
</div>

&emsp;&emsp;注意：选择符'|'的优先级最低，比起始和结束位置都要低

<div>
<pre>console.log(/^ab|cd$/.test('abc'));//true
console.log(/^(ab|cd)$/.test('abc'));//false</pre>
</div>

&nbsp;

### 局限性

&emsp;&emsp;尽管javascript中的正则表达式功能比较完备，但与其他语言相比，缺少某些特性

&emsp;&emsp;下面列出了javascript正则表达式不支持的特性

&emsp;&emsp;【1】POSIX字符组(只支持普通字符组和排除型字符组)

&emsp;&emsp;【2】Unicode支持(只支持单个Unicode字符)

&emsp;&emsp;【3】匹配字符串开始和结尾的\A和\Z锚(只支持^和$)

&emsp;&emsp;【4】逆序环视(只支持顺序环视)

&emsp;&emsp;【5】命名分组(只支持0-9编号的捕获组)

&emsp;&emsp;【6】单行模式和注释模式(只支持m、i、g)

&emsp;&emsp;【7】模式作用范围

&emsp;&emsp;【8】纯文本模式

## 参考资料

【1】 阮一峰Javascript标准参考教程&mdash;&mdash;标准库RegExp对象 [http://javascript.ruanyifeng.com/stdlib/regexp.html](http://javascript.ruanyifeng.com/stdlib/regexp.html#toc10)

【2】《正则指引》

【3】《精通正则表达式》

【4】《javascript权威指南(第6版)》第10章 正则表达式的模式匹配

【5】《javascript高级程序设计(第3版)》第5章 引用类型

【6】《javascript语言精粹(修订版)》第7章 正则表达式

