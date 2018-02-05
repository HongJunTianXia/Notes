# HTTP协议
## 1.持久连接节省通信量
```
HTTP 协议的初始版本中，每进行一次 HTTP 通信就要断开一次 TCP
连接。比如，使用浏览器浏览一个包含多张图片的 HTML 页面时，在发送
请求访问 HTML 页面资源的同时，也会请求该 HTML 页面里包含的
其他资源。因此，每次的请求都会造成无谓的 TCP 连接建立和断
开，增加通信量的开销。
为解决上述 TCP 连接的问题，HTTP/1.1 和一部分的 HTTP/1.0 想出了持久连接（HTTP Persistent Connections，也称为 HTTP keep-alive 或HTTP connection reuse）的方法。持久连接的特点是，只要任意一端没有明确提出断开连接，则保持 TCP 连接状态。
持久连接的好处在于减少了 TCP 连接的重复建立和断开所造成的额
外开销，减轻了服务器端的负载。另外，减少开销的那部分时间，使
HTTP 请求和响应能够更早地结束，这样 Web 页面的显示速度也就相
应提高了。
在 HTTP/1.1 中，所有的连接默认都是持久连接，但在 HTTP/1.0 内并
未标准化。虽然有一部分服务器通过非标准的手段实现了持久连接，
但服务器端不一定能够支持持久连接。毫无疑问，除了服务器端，客
户端也需要支持持久连接。
```
## 2.管线化
```
持久连接使得多数请求以管线化（pipelining）方式发送成为可能。从
前发送请求后需等待并收到响应，才能发送下一个请求。管线化技术
出现后，不用等待响应亦可直接发送下一个请求。
这样就能够做到同时并行发送多个请求，而不需要一个接一个地等待
响应了。
```
## 3.告知服务器意图的 HTTP 方法
```
GET ：获取资源
POST：传输实体主体
PUT：传输文件
HEAD：获得报文首部(HEAD 方法和 GET 方法一样，只是不返回报文主体部分。用于确认URI 的有效性及资源更新的日期时间等。)
DELETE：删除文件
OPTIONS：询问支持的方法
TRACE：追踪路径
CONNECT：要求用隧道协议连接代理(CONNECT 方法要求在与代理服务器通信时建立隧道，实现用隧道协
议进行 TCP 通信。主要使用 SSL（Secure Sockets Layer，安全套接
层）和 TLS（Transport Layer Security，传输层安全）协议把通信内容
加 密后经网络隧道传输。)
```
## 4.HTTP报文格式
<img src="https://github.com/withstars/Notes/blob/master/Static/1.PNG">
<img src="https://github.com/withstars/Notes/blob/master/Static/2.PNG">
<img src="https://github.com/withstars/Notes/blob/master/Static/3.PNG">

## 5.HTTP首部
#### 通用首部字段
```
通用首部字段是指，请求报文和响应报文双方都会使用的首部。
Cache-Control:通过指定首部字段 Cache-Control 的指令，就能操作缓存的工作机
制。
Connection:控制不再转发给代理的首部字段,管理持久连接

Date:首部字段 Date 表明创建 HTTP 报文的日期和时间。

Trailer:首部字段 Trailer 会事先说明在报文主体后记录了哪些首部字段。该
首部字段可应用在 HTTP/1.1 版本分块传输编码时。

Transfer-Encoding:首部字段 Transfer-Encoding 规定了传输报文主体时采用的编码方式。

Upgrade:首部字段 Upgrade 用于检测 HTTP 协议及其他协议是否可使用更高的
版本进行通信，其参数值可以用来指定一个完全不同的通信协议。如WebSocket协议

Via:使用首部字段 Via 是为了追踪客户端与服务器之间的请求和响应报文
的传输路径。

Warning：HTTP/1.1 的 Warning 首部是从 HTTP/1.0 的响应首部（Retry-After）演变过来的。该首部通常会告知用户一些与缓存相关的问题的警告。
```
#### 请求首部字段
```
请求首部字段是从客户端往服务器端发送请求报文中所使用的字段，
用于补充请求的附加信息、客户端信息、对响应内容相关的优先级等
内容。
Accept:Accept 首部字段可通知服务器，用户代理能够处理的媒体类型及媒体
类型的相对优先级。可使用 type/subtype 这种形式，一次指定多种媒
体类型。当服务器提供多种内容时，将会首先返回权重值最高的媒体类型。

Accept-Charset:Accept-Charset 首部字段可用来通知服务器用户代理支持的字符集及字符集的相对优先顺序。另外，可一次性指定多种字符集。与首部字
段 Accept 相同的是可用权重 q 值来表示相对优先级。

Accept-Encoding:Accept-Encoding 首部字段用来告知服务器用户代理支持的内容编码及内容编码的优先级顺序。可一次性指定多种内容编码。如gzip, deflate.

Accept-Language:首部字段 Accept-Language 用来告知服务器用户代理能够处理的自然语言集（指中文或英文等），以及自然语言集的相对优先级。可一次
指定多种自然语言集。和 Accept 首部字段一样，按权重值 q 来表示相对优先级。

Authorization:首部字段 Authorization 是用来告知服务器，用户代理的认证信息（证书值）。通常，想要通过服务器认证的用户代理会在接收到返回的
401 状态码响应后，把首部字段 Authorization 加入请求中。共用缓存
在接收到含有 Authorization 首部字段的请求时的操作处理会略有差
异。

Expect:客户端使用首部字段 Expect 来告知服务器，期望出现的某种特定行
为。因服务器无法理解客户端的期望作出回应而发生错误时，会返回
状态码 417 Expectation Failed。

From:首部字段 From 用来告知服务器使用用户代理的用户的电子邮件地
址。通常，其使用目的就是为了显示搜索引擎等用户代理的负责人的
电子邮件联系方式。使用代理时，应尽可能包含 From 首部字段（但
可能会因代理不同，将电子邮件地址记录在 User-Agent 首部字段
内）。

Host:首部字段 Host 会告知服务器，请求的资源所处的互联网主机名和端
口号。Host 首部字段在 HTTP/1.1 规范内是唯一一个必须被包含在请
求内的首部字段。

If-Match:形如 If-xxx 这种样式的请求首部字段，都可称为条件请求。服务器接收到附带条件的请求后，只有判断指定条件为真时，才会执行请求。

If-Modified-Since:首部字段 If-Modified-Since，属附带条件之一，它会告知服务器若 If-Modified-Since 字段值早于资源的更新时间，则希望能处理该请求。而在指定 If-Modified-Since 字段值的日期时间之后，如果请求的资源都没有过更新，则返回状态码 304 Not Modified 的响应。

If-None-Match:首部字段 If-None-Match 属于附带条件之一。它和首部字段 If-Match作用相反。用于指定 If-None-Match 字段值的实体标记（ETag）值与请求资源的 ETag 不一致时，它就告知服务器处理该请求。

If-Range:首部字段 If-Range 属于附带条件之一。它告知服务器若指定的 If-
Range 字段值（ETag 值或者时间）和请求资源的 ETag 值或时间相一
致时，则作为范围请求处理。反之，则返回全体资源。

If-Unmodified-Since:首部字段 If-Unmodified-Since 和首部字段 If-Modified-Since 的作用相反。它的作用的是告知服务器，指定的请求资源只有在字段值内指定的日期时间之后，未发生更新的情况下，才能处理请求。如果在指定日期时间后发生了更新，则以状态码 412 Precondition Failed 作为响应返回。

Max-Forwards:通过 TRACE 方法或 OPTIONS 方法，发送包含首部字段 Max-
Forwards 的请求时，该字段以十进制整数形式指定可经过的服务器最
大数目。服务器在往下一个服务器转发请求之前，Max-Forwards 的
值减 1 后重新赋值。当服务器接收到 Max-Forwards 值为 0 的请求
时，则不再进行转发，而是直接返回响应。

Proxy-Authorization:接收到从代理服务器发来的认证质询时，客户端会发送包含首部字段Proxy-Authorization 的请求，以告知服务器认证所需要的信息。

Range:对于只需获取部分资源的范围请求，包含首部字段 Range 即可告知服
务器资源的指定范围。上面的示例表示请求获取从第 5001 字节至第
10000 字节的资源。

Referer:首部字段 Referer 会告知服务器请求的原始资源的 URI。

TE:首部字段 TE 会告知服务器客户端能够处理响应的传输编码方式及相
对优先级。它和首部字段 Accept-Encoding 的功能很相像，但是用于
传输编码。

User-Agent:首部字段 User-Agent 会将创建请求的浏览器和用户代理名称等信息传达给服务器。
```
#### 响应首部字段
```
响应首部字段是由服务器端向客户端返回响应报文中所使用的字段，
用于补充响应的附加信息、服务器信息，以及对客户端的附加要求等
信息。

Accept-Ranges:首部字段 Accept-Ranges 是用来告知客户端服务器是否能处理范围请求，以指定获取服务器端某个部分的资源。

Age:首部字段 Age 能告知客户端，源服务器在多久前创建了响应。字段值
的单位为秒。

ETag:首部字段 ETag 能告知客户端实体标识。它是一种可将资源以字符串
形式做唯一性标识的方式。服务器会为每份资源分配对应的 ETag
值。

Location:使用首部字段 Location 可以将响应接收方引导至某个与请求 URI 位置不同的资源。

Proxy-Authenticate:首部字段 Proxy-Authenticate 会把由代理服务器所要求的认证信息发送给客户端。

Retry-After:首部字段 Retry-After 告知客户端应该在多久之后再次发送请求。主要配合状态码 503 Service Unavailable 响应，或 3xx Redirect 响应一起使用。

Server:首部字段 Server 告知客户端当前服务器上安装的 HTTP 服务器应用程
序的信息。不单单会标出服务器上的软件应用名称，还有可能包括版
本号和安装时启用的可选项。

Vary:当代理服务器接收到带有 Vary 首部字段指定获取资源的请求
时，如果使用的 Accept-Language 字段的值相同，那么就直接从缓
存返回响应。反之，则需要先从源服务器端获取资源后才能作为
121响应返回。

WWW-Authenticate:首部字段 WWW-Authenticate 用于 HTTP 访问认证。它会告知客户端适用于访问请求 URI 所指定资源的认证方案（Basic 或是 Digest）和带参数提示的质询（challenge）。状态码 401 Unauthorized 响应中，肯定带有首部字段 WWW-Authenticate。

```
#### 实体首部字段
```
实体首部字段是包含在请求报文和响应报文中的实体部分所使用的首
部，用于补充内容的更新时间等与实体相关的信息。在请求和响应两方的 HTTP 报文中都含有与实体相关的首部。

Allow:首部字段 Allow 用于通知客户端能够支持 Request-URI 指定资源的所
有 HTTP 方法。当服务器接收到不支持的 HTTP 方法时，会以状态码
405 Method Not Allowed 作为响应返回。与此同时，还会把所有能支
持的 HTTP 方法写入首部字段 Allow 后返回

Content-Encoding:首部字段 Content-Encoding 会告知客户端服务器对实体的主体部分选用的内容编码方式。内容编码是指在不丢失实体信息的前提下所进行的压缩。

Content-Language:首部字段 Content-Language 会告知客户端，实体主体使用的自然语言（指中文或英文等语言）。

Content-Length:首部字段 Content-Length 表明了实体主体部分的大小（单位是字节）。对实体主体进行内容编码传输时，不能再使用 Content-Length
首部字段。

Content-Location:首部字段 Content-Location 给出与报文主体部分相对应的 URI。和首部字段 Location 不同，Content-Location 表示的是报文主体返回资源对应的 URI。

Content-MD5:首部字段 Content-MD5 是一串由 MD5 算法生成的值，其目的在于检查报文主体在传输过程中是否保持完整，以及确认传输到达。

Content-Range：针对范围请求，返回响应时使用的首部字段 Content-Range，能告知客户端作为响应返回的实体的哪个部分符合范围请求。字段值以字节为单位，表示当前发送部分及整个实体大小。

Content-Type:首部字段 Content-Type 说明了实体主体内对象的媒体类型。和首部字段 Accept 一样，字段值用 type/subtype 形式赋值。

Expires:首部字段 Expires 会将资源失效的日期告知客户端。缓存服务器在接
收到含有首部字段 Expires 的响应后，会以缓存来应答请求，在
Expires 字段值指定的时间之前，响应的副本会一直被保存。当超过
指定的时间后，缓存服务器在请求发送过来时，会转向源服务器请求
资源。

Last-Modified:首部字段 Last-Modified 指明资源最终修改的时间。一般来说，这个值就是 Request-URI 指定资源被修改的时间。但类似使用 CGI 脚本进行动态数据处理时，该值有可能会变成数据最终修改时的时间。
```
#### 为 Cookie 服务的首部字段
```
Set-Cookie
Cookie:首部字段 Cookie 会告知服务器，当客户端想获得 HTTP 状态管理支
持时，就会在请求中包含从服务器接收到的 Cookie。接收到多个
Cookie 时，同样可以以多个 Cookie 形式发送。
```

## 6.确保 Web 安全的HTTPS
* HTTP+ 加密 + 认证 + 完整性保护=HTTPS
```
HTTPS 并非是应用层的一种新协议。只是 HTTP 通信接口部分用
SSL（Secure Socket Layer）和 TLS（Transport Layer Security）协议代
替而已。通常，HTTP 直接和 TCP 通信。当使用 SSL 时，则演变成先和 SSL 通信，再由 SSL 和 TCP 通信了。简言之，所谓 HTTPS，其实就是身披
SSL 协议这层外壳的 HTTP.在采用 SSL 后，HTTP 就拥有了 HTTPS 的加密、证书和完整性保护这些功能。SSL 是独立于 HTTP 的协议，所以不光是 HTTP 协议，其他运行在应用层的 SMTP 和 Telnet 等协议均可配合 SSL 协议使用。可以说 SSL 是当今世界上应用最为广泛的网络安全技术。
```