# 前端学HTTP之日志记录

　　几乎所有的服务器和代理都会记录下它们所处理的HTTP事务摘要。这么做出于一系列的原因：跟踪使用情况、安全性、计费、错误检测等等。本文将介绍日志记录

&nbsp;

### 记录内容

　　大多数情况下，日志的记录出于两种原因：査找服务器或代理中存在的问题(比如，哪些请求失败了)，或者是生成Web站点访问方式的统计信息。统计数据对市场营销、计费和容量规划(比如，决定是否需要增加服务器或带宽)都非常有用

　　可以把一个HTTP事务中所有的首部都记录下来，但对每天要处理数百万个事务的服务器和代理来说，这些数据的体积超大，很快就会失控。不应该记录实际上并不感兴趣，甚至从来都不会去看一眼的数据

　　通常，只记录事务的基本信息就行了。通常会记录下来的几个字段示例为：HTTP方法；客户端和服务器的HTTP版本；所请求资源的URL；响应的HTTP状态码；请求和响应报文的尺寸(包含所有的实体主体部分)；事务开始时的时间戳；Referer首部和User-Agent首部的值

　　HTTP方法和URL说明了请求试图做些什么&mdash;&mdash;比如，GET某个资源或POST某个订单。可以用URL来记录Web站点上页面的受欢迎程度

　　版本字符串给出了与客户端和服务器有关的一些提示，在客户端和服务器之间出现一些比较奇怪或非预期的交互动作时，它会非常有用。比如，如果请求的失败率高于预期，那版本信息指向的可能是一个无法与服务器进行交互的新版浏览器

　　HTTP状态码说明了请求的执行状况：是否成功执行，认证请求是否失败，资源是否找到等

　　请求/响应的大小和时间戳主要用于记账&mdash;&mdash;记录流入、流出或流经应用程序的字节有多少。还可用时间戳将观察到的问题与当时发起的一些请求关联起来

&nbsp;

### 日志格式

　　大部分商用和开源的HTTP应用程序都支持以一种或多种常用格式进行日志记录。很多这样的应用程序都支持管理者配置日志格式，创建自定义的格式

　　应用程序支持管理者使用这些更标准的格式的主要好处之一在于，可以充分利用那些已构建好的工具处理这些日志，并产生基本的统计信息。有很多开源包和商用包都可用来压缩日志，以进行汇报。使用标准格式，应用程序及其管理员就都可以利用这些包了

【常见日志格式】

　　现在，最常见的日志格式之一就是常用日志格式。这种日志格式最初由NCSA定义，很多服务器在默认情况下都会使用这种日志格式。可以将大部分商用及开源服务器配置为使用这种格式，有很多商用及免费工具都可辅助解析常用日志格式的文件。下表按序列出了常用日志格式中的字段

![log1](https://pic.xiaohuochai.site/blog/HTTP_log1.png)

　　下面列出了几个常见日志格式条目

![log2](https://pic.xiaohuochai.site/blog/HTTP_log2.png)

　　在这些例子中，字段的分配如下所示

![log3](https://pic.xiaohuochai.site/blog/HTTP_log3.png)

　　[注意]remotehost字段可以是http-guide.com那样的主机名，也可以是209.1.32.44这样的IP地址

　　第二个(usemame)和第三个(auth-username)字段之间的破折号说明字段为空。这说明要么是没有进行ident査找(第二个字段为空)，要么是没有进行认证(第三 个字段为空)

【组合日志格式】

　　另一种常用日志格式为组合日志格式(Combined Log Format)，例如Apache服务器就支持这种格式。组合日志格式与常用日志格式很类似。实际上，它就是常用日志格式的精确镜像，只是添加了两个字段。User-Agent字段用于说明是哪个HTTP客户端应用程序在发起已被记录的请求，而Referer字段则提供了更多与请求端在何处找到这个URL的有关信息

　　下面列出了新加的组合日志格式字段

<div class="cnblogs_code">
<pre>字段                描述
Referer            Referer首部的内容
User-Agent         User-Agent首部的内容    </pre>
</div>

　　下例给出了一个组合日志格式的条目

![log4](https://pic.xiaohuochai.site/blog/HTTP_log4.png)

　　Referer字段和User-Agent字段的值如下所示&nbsp;

![log5](https://pic.xiaohuochai.site/blog/HTTP_log5.png)

　　上面的组合日志格式条目示例中的前七个字段和常用日志格式中的完全一样。两个新字段Referer和User-Agent附加在日志条目的末尾

【网景扩展日志格式】

　　网景进入商用HTTP应用程序领域时，为其服务器定义了很多其他HTTP应用程序开发者已接纳的日志格式。网景的格式是基于NCSA的常用日志格式的，但它们扩展了该格式，以支持与代理和Web缓存这样的HTTP应用程序相关的字段

　　网景扩展日志格式的前7个字段与常用日志格式中的那些字段完全相同。下表按序列出了网景扩展日志格式引入的新字段

![log6](https://pic.xiaohuochai.site/blog/HTTP_log6.png)

　　下面给出了一个网景扩展日志格式的条目

<div class="cnblogs_code">
<pre>209.1.32.44    - - [03/Oct/2016:14:16:00-0400] "GET / HTTP/1.0" 200 1024 200 1024 0 0 215 260
279 254 3</pre>
</div>

　　在这个例子中，扩展字段的值如下所示

![log7](https://pic.xiaohuochai.site/blog/HTTP_log7.png)

　　上面的网景扩展日志格式条目示例中的前7个字段是常用日志格式条目示例的镜像

　　另一种网景日志格式，网景扩展2日志格式采用了扩展日志格式，并添加了一些与HTTP代理和Web缓存应用程序有关的附加信息。这些附加字段有助于更好地描绘HTTP客户端和HTTP代理应用程序间的交互图景

　　网景扩展2日志格式是基于网景扩展日志格式的，初始字段与网景扩展日志的字段完全相同

　　下表按序列出了网景扩展2日志格式新加的字段

![log8](https://pic.xiaohuochai.site/blog/HTTP_log8.png)

　　下例给出了一个网景扩展2日志格式的条目

<div class="cnblogs_code">
<pre>209.1.32.44    - - [03/Oct/2016:14:16:00-0400] "GET / HTTP/1.0" 200 1024 200 1024 0 0 215 260 
279 254 3 DIRECT FIN WRITTEN</pre>
</div>

　　这个例子中拓展字段的值如下所示

![log9](https://pic.xiaohuochai.site/blog/HTTP_log9.png)

　　上面的网景扩展2日志格式条目中的前16个字段就是网景扩展日志格式示例条目的镜像

　　下表列出了有效的网景路由代码

![log10](https://pic.xiaohuochai.site/blog/HTTP_log10.png)

　　下表列出了有效的网景完成状态码

![log11](https://pic.xiaohuochai.site/blog/HTTP_log11.png)

　　下表列出了有效的网景缓存代码

![log12](https://pic.xiaohuochai.site/blog/HTTP_log12.png)

![log13](https://pic.xiaohuochai.site/blog/HTTP_log13.png)

　　与很多其他HTTP应用程序一样，网景应用程序也有其他的日志格式，包括一种灵活日志格式和一种管理者输出自定义日志字段的方式。这些格式给予管理者更大的控制权，并可以选择在日志中报告HTTP事务处理的哪些部分(首部、状态、尺寸等)，以自定义其日志

　　由于很难预测管理者希望从其日志中获取哪些信息，才添加了管理者配置自定义格式的能力。很多其他的代理和服务器都有发布自定义日志的能力

【Squid代理日志格式】

　　Squid代理缓存(`http://www.squid-cache.org`)是Web上一个很古老的部分。其起源可以回溯到一个早期的Web代理缓存项目(`ftp://ftp.cs.colorado.edu/pub/techreports/schwartz/Harvest.Conf.ps.Z`)。Squid是开源社团多年来扩展增强的一个开源项目。有很多工具可以用来辅助管理Squid应用程序，包括一些有助于处理、审核及开发其日志的工具。很多后继代理缓存都为自己的日志使用了Squid格式，这样才能更好地利用这些工具

　　Squid日志条目的格式相当简单。下表总结了该日志格式的字段

![log14](https://pic.xiaohuochai.site/blog/HTTP_log14.png)

![log15](https://pic.xiaohuochai.site/blog/HTTP_log15.png)

　　下面给出了一个Squid日志格式条目的例子

![log16](https://pic.xiaohuochai.site/blog/HTTP_log16.png)

　　这些字段的值如下所示

![log17](https://pic.xiaohuochai.site/blog/HTTP_log17.png)

　　下表列出了各种Squid结果代码

![log18](https://pic.xiaohuochai.site/blog/HTTP_log18.png)

### 命中率测量

　　原始服务器通常会出于计费的目的保留详细的日志记录。内容提供者需要知道URL的受访频率，广告商需要知道广告的出现频率，网站作者需要知道所编写的内容的受欢迎程度。客户端直接访问Web服务器时，日志记录可以很好地跟踪这些信息

　　但是，缓存服务器位于客户端和服务器之间，用于防止服务器同时处理大量访问请求(这正是缓存的目的)。缓存要处理很多HTTP请求，并在不访问原始服务器的情况下满足它们的请求，服务器中没有客户端访问其内容的记录，导致日志文件中出现遗漏

　　由于日志数据会遗失，所以，内容提供者会对其最重要的页面进行缓存清除(cache bust)。缓存清除是指内容提供者有意将某些内容设置为无法缓存，这样，所有对此内容的请求都会被导向原始服务器。于是，原始服务器就可以记录下访问情况了。不使用缓存可能会生成更好的日志，但会减缓原始服务器和网络的请求速度，并增加其负荷

　　由于代理缓存(及一些客户端)都会保留自己的日志，所以如果服务器能够访问这些日志(或者至少有一种粗略的方式可以判断代理缓存会以怎样的频率提供其内容)，就可以避免使用缓存清除。命中率测量协议是对HTTP的一种扩展，它为这个问题提供了一种解决方案。命中率测量协议要求缓存周期性地向原始服务器汇报缓存访问的统计数据

　　命中率测量协议定义了一种HTTP扩展，它提供了一些基本的功能，缓存和服务器可以实现这些功能来共享访问信息，规范已缓存资源的可使用次数

　　缓存给日志访问带来了问题，命中率测量并不是这个问题的完整解决方案，但它确实提供了一种基本方式，以获取服务器希望跟踪的度量值。命中率测量协议并没有(而且可能永远都不会)得到广泛的实现或应用。也就是说，在维护缓存性能增益的同时，像命中率测量这样的合作方案会给出一些提供精确访问统计信息的承诺。希望这会推动命中率测量协议的实现，而不是把内容标记为不可缓存的

【Meter 首部】

　　命中率测量扩展建议使用新增加的首部Meter，缓存和服务器可以通过它在相互间传输与用法和报告有关的指令，这与用来进行缓存指令交换的Cache-Control首部很类似

　　下表列出了定义的各种指令和谁可以在Meter首部传输这些指令

![log19](https://pic.xiaohuochai.site/blog/HTTP_log19.png)

　　下图给出了一个执行中的命中串测量实例。事务的第一部分就是客户端和代理缓存之间一个普通的HTTP事务，但在代理请求中，要注意有插入的Meter首部和来自服务器的响应。这里，代理正在通知服务器它可以进行命中率测量。作为回应，服务器则请求代理报告它的命中次数

![log20](https://pic.xiaohuochai.site/blog/HTTP_log20.png)

　　从客户端的角度来看，请求正常结束了，代理开始代表服务器跟踪该请求资源的命中次数。稍后，代理会尝试与服务器再次验证资源。代理会在发送给服务器的条件请求中嵌入它跟踪记录的计量信息

&nbsp;

### 隐私

　　日志记录实际上就是服务器和代理执行的一项管理功能，所以整个操作对用户来说都是透明的。通常，用户甚至都不清楚他们的HTTP事务已被记录

　　Web应用程序的开发者和管理者要清楚跟踪用户的HTTP事务可能带来的影响。他可以根据获取的信息收集很多有关用户的情况。很显然，这些信息可以用于不良目的&mdash;&mdash;歧视、骚扰、勒索等。进行日志记录的Web服务器和代理一定要注意保护其终端用户的隐私

　　有些情况下，比如在工作环境中，跟踪某用户的使用情况以确保他没有偷懒是可行的，但管理员也应该将监视大家事务处理的事情公之于众

　　简单来说，日志记录对管理者和开发者来说都是很有用的工具。只是要清楚在没有获得用户许可，或在其不知情的情况下，使用记录其行为的日志可能会存在侵犯隐私的问题
