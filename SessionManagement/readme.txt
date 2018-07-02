会话管理与验证机制有着紧密相连的关系

会话介绍：
会话可简单理解为：用户开一个浏览器，点击多个超链接，访问服务器多个web资源，然后关闭浏览器，整个过程称之为一个会话。
保存会话数据，3种web会话管理的方式：Cookie、Session、Token；另外，有些公司也存在AK/SK认证
参见链接介绍：
http://www.cnblogs.com/lyzg/p/6067766.html


Session技术：
Session的实现主要两种方式：cookie与url重写，而cookie是首选(默认)的方式。
因为各种现代浏览器都默认开通cookie功能，但是每种浏览器也都有允许cookie失效的设置。如果浏览器禁用了Cookie，浏览器就没有办法JESSIONID cookie，这样就用不了Session了。我们可以使用URL重写的机制，在所有的超链接后都以参数的形式拼接JESSIONID信息，从而在点击超链接时可以使用URL参数的方式带回JESSIONID，从而使用Session。将URL进行重写拼接上JESSIONID的过程就叫做URL重写。

Session与Cookie的比较
    1、从存取方式上比较
      Cookie中只能保存ASCII字符串，如果需要存取Unicode字符或者二进制数据，需要进行UTF-8，GBK或者BASE64等方式的编码。Cookie中也不能直接存取Java对象。若要存储复杂的信息，使用Cookie是比较困难的。
      Session中可以存取任何类型的数据，包括而不限于String、Integer、List、Map等。Session中也可以直接保存Java Bean乃至Java类，对象等，使用起来非常方便，可以把Session看做是一个Java容器类。
    2、从隐私安全上比较
      Cookie存储在客户端浏览器中，对客户端是可见的，客户端的一些程序可能会窥探、复制甚至修改Cookie中的内容。而Session存储在服务器上，对客户端是透明的，不存在敏感信息泄露的危险。如果选用Cookie，比较好的办法是，敏感的信息如账号密码等尽量不要写到Cookie中。
    3、从有效期上比较
      通过设置Cookie的maxAge属性为一个很大很大的数字或者Integer.MAX_VALUE可以使得浏览器长久地记录用户的登录信息。使用Session理论上也能实现这种效果。只要调用方法setMaxInactiveInterval(Integer.MAX_VALUE)就可以了。但是由于Session依赖于名为JESSIONID的Cookie,而这个Cookie的maxAge默认为-1，只要关闭了浏览器Session就会失效，因此Session不能实现信息永久有效的效果。使用URL地址重写也不能实现。而且如果设置Session的超时时间过长，服务器累计的Session就会越多，越容易导致内存溢出。
    4、对服务器的负担上比较
      Session是保存在服务器端的，每个用户都会产生一个Session。如果并发访问的用户非常多，会产生非常多的Session，消耗大量的内存。因此像Google、Baidu这样并发访问量极高的网站，是不太可能使用Session来追踪客户会话的。而Cookie保存在客户端，不占用服务器资源。如果并发浏览的用户非常多，Cookie是很好的选择。
    5、从浏览器支持上比较
      Cookie是需要客户端浏览器支持的。如果客户端禁用了Cookie，或者不支持Cookie，则会话跟踪会失效。对于WAP上的应用，常规的Cookie就派不上用场了。如果客户端浏览器不支持Cookie，需要使用Session以及URL地址重写。需要注意的是所有的用到Session程序的URL都要使用response.encodeURL(String URL)或者response.encodeRedirectURL(String URL)进行URL地址重写，否则导致Session会话跟踪失败。对于WAP应用来说，Session+URL地址重写也许是它唯一的选择。如果客户端支持Cookie，则Cookie既可以设为本浏览器窗口以及子窗口内有效（把maxAge设为-1），也可以设为所有浏览器窗口内有效（把maxAge设为某个大于0的整数）。但Session只能在本浏览器窗口以及子窗口内有效。如果两个浏览器窗口互不相干，它们将使用两个不同的Session。
    6、从跨域名上比较
      Cookie支持跨域名访问，而Session不会支持跨域名访问。Session仅在他所在的域名内有效

会话管理的风险
  1.会话令牌生成过程中的薄弱环节
    令牌有一定含义
    令牌可预测

  2.会话令牌处理中的薄弱环节
    在网络上泄露令牌
    在日志中泄露令牌
    令牌-会话映射易受攻击
    会话终止易受攻击
    客户暴露在令牌劫持风险之中
    宽泛的cookie范围
    
安全保障方案--保障会话管理的安全
  生成强大的令牌
  在整个生命周期保障令牌的安全
  日志、监控与警报
