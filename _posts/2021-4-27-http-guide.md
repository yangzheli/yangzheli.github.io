---
title: HTTP 指南
categories: [frontend]
comments: true
---

## HTTP 报文

HTTP 报文是在 HTTP 应用程序之间发送的数据块，报文由三部分组成：

- 起始行：对报文进行描述；

- 首部：包含报文的属性；

- 主体：包含报文的数据，可选。

下图是 HTTP 响应报文的一个示例：

![HTTP 报文组成](../assets/img/http-guide/http-response-message.png)

HTTP 方法：

- GET：用于请求服务器发送某个资源；

- HEAD：和 GET 方法类似，但服务器在响应时只返回首部，不会返回主体部分；

- POST：向服务器发送需要处理的数据；

- PUT：将请求的主体部分放在服务器；

- TRACE：对可能经过代理服务器传送到服务器上的报文进行跟踪；

- OPTIONS：决定可以在服务器上执行哪些方法；

- DELETE：从服务器删除一份文档。

HTTP 状态码：

- 100-199：信息类状态码；

| 状态码 | 原因短语            | 描述                                                                                         |
| :----- | :------------------ | :------------------------------------------------------------------------------------------- |
| 100    | Continue            | 说明收到了请求的初始部分，请客户端继续。发送了这个状态码之后，服务器在收到请求后必须进行响应 |
| 101    | Switching Protocols | 说明服务器正在根据客户端的指定，将协议切换成 Update 首部所列的协议                           |

- 200-299：成功状态码；

| 状态码 | 原因短语                      | 描述                                                                     |
| :----- | :---------------------------- | :----------------------------------------------------------------------- |
| 200    | Ok                            | 请求没问题，实体的主体部分包含了所请求的资源                             |
| 201    | Created                       | 用于创建服务器对象的请求 (如 PUT)，响应的主体部分应包含已创建资源的 URL  |
| 202    | Accepted                      | 请求已被接受，但服务器还未对其执行任何动作，不能保证服务器会完成这个请求 |
| 203    | Non-Authoritative Information | 实体首部包含的信息不是来自于源服务器，而是来自资源的一份副本             |
| 204    | No Content                    | 响应报文包含起始行和首部，但没有实体的主体部分                           |
| 205    | Reset Content                 | 告知浏览器清除当前页面中的所有 HTML 表单元素                             |
| 206    | Partial Content               | 成功执行了部分或 Range (范围) 请求                                       |

- 300-399：重定向状态码；

| 状态码 | 原因短语           | 描述                                                                                                                                                                                                                                                                       |
| :----- | :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 300    | Multiple Choices   | 当客户端请求一个实际指向多个资源的 URL 时会返回这个状态码                                                                                                                                                                                                                  |
| 301    | Moved Permanently  | 在请求的 URL 已被移除时使用，响应报文 Location 首部应该包含资源现在所在的 URL                                                                                                                                                                                              |
| 302    | Found              | 与 301 状态码类似，但客户端只是临时使用 Location 首部的 URL 定位资源，将来的请求仍应该使用老的 URL                                                                                                                                                                         |
| 303    | See Other          | 告知客户端应该用另一个 URL 来获取资源，新的 URL 位于响应报文的 Location 首部                                                                                                                                                                                               |
| 304    | Not Modified       | 客户端可以通过所包含的请求首部，使其请求变成有条件的 (如包含 If-Modified-Since: 时间)。如果客户端发送了一个这样的有条件的 GET 请求，并且请求资源最近未修改的话，就可以返回这个状态来码说明资源未被修改，带有这个状态码的响应报文不应该包含主体部分，客户端显示的是本地副本 |
| 305    | Use Proxy          | 来说明必须通过一个代理来访问资源，代理的位置由 Location 首部给出。注意：客户端只是对请求的特定资源使用该代理，而不是对所有请求资源都使用该代理，如果错误地让代理介入某个请求，可能会引发破坏性的行为                                                                       |
| 306    | Unused             | 当前未使用                                                                                                                                                                                                                                                                 |
| 307    | Temporary Redirect | 与 301 状态码类似，但客户端只是临时使用 Location 首部的 URL 定位资源，将来的请求仍应该使用老的 URL (这不是和 302 状态码一样吗？)                                                                                                                                           |

可以注意到，302、303、307 状态码存在一些交叉，这些状态码的用法只有细微的区别，源于 HTTP/1.0 和 HTTP/1.1 应用程序对这些状态码处理方式不同。如对于 HTTP/1.1 客户端，用 307 状态码取代 302 进行临时重定位，这样服务器可以将 302 状态码保存起来，供 HTTP/1.0 客户端使用。因此，服务器选择合适的重定向状态码时，就需要查看客户端的 HTTP 版本

- 400-499：客户端错误状态码；

| 状态码 | 原因短语                        | 描述                                                                                                    |
| :----- | :------------------------------ | :------------------------------------------------------------------------------------------------------ |
| 400    | Bad Request                     | 告知客户端发送了一个错误的请求                                                                          |
| 401    | Unauthorized                    | 在获取对资源的访问权之前，要进行认证                                                                    |
| 402    | Payment Required                | 还未使用，已被保留                                                                                      |
| 403    | Forbidden                       | 说明请求被服务器拒绝了                                                                                  |
| 404    | Not Found                       | 说明服务器无法找到所请求的 URL                                                                          |
| 405    | Method Not Allowed              | 发起的请求中带有所请求的 URL 不支持的方法                                                               |
| 406    | Not Acceptable                  | 客户端可以指定参数说明它们愿意接收的实体类型，服务器没有与客户端可接受的 URL 相匹配的资源，返回此状态码 |
| 407    | Proxy Authentication            | 与 401 状态码类似，用于要求对资源进行认证的代理服务器                                                   |
| 408    | Request Timeout                 | 客户端完成请求所花的时间太长，服务器返回此状态码，并关闭连接                                            |
| 409    | Conflict                        | 说明请求可能在资源上引发一些冲突                                                                        |
| 410    | Gone                            | 与 404 类似，只是服务器曾经拥有过此资源，但现在被移除了                                                 |
| 411    | Length Required                 | 服务器要求在请求报文中包含 Content-Length 首部时使用                                                    |
| 412    | Precondition Failed             | 客户端发起了条件请求，且其中一个条件失败的时候使用                                                      |
| 413    | Request Entity Too Large        | 客户端发送的实体主体部分比服务器能够处理的要长时，使用此状态码                                          |
| 414    | Request URL Too Long            | 客户端发送请求的 URL 比服务器能够处理的要长时，使用此状态码                                             |
| 415    | Unsupported Media Type          | 服务器无法支持客户端发送实体的内容类型时，使用此状态码                                                  |
| 416    | Requested Range Not Satisfiable | 无法满足请求报文所指定的资源的范围时，使用此状态码                                                      |
| 417    | Expectation Failed              | 请求的 Expect 请求首部包含一个期望，服务器无法满足这个期望时，使用此状态码                              |

- 500-599：服务器错误状态码。

| 状态码 | 原因短语                   | 描述                                                                                              |
| :----- | :------------------------- | :------------------------------------------------------------------------------------------------ |
| 500    | Internal Server Error      | 服务器遇到一个妨碍它为请求提供服务的错误                                                          |
| 501    | Not Implemented            | 客户端发送的请求超出服务器的能力范围                                                              |
| 502    | Bad Gateway                | 作为代理或者网关使用的服务器从请求响应链的下一条链路上收到了一条伪响应 (比如它无法连接到其父网关) |
| 503    | Service Unavailable        | 服务器现在无法为请求提供服务，但将来可以                                                          |
| 504    | Gateway Timeout            | 与状态码 408 类似，只是响应来自一个网关或代理，它们在等待另一台服务器对其请求进行响应时超时了     |
| 505    | HTTP Version Not Supported | 服务器收到的请求使用了它不支持的 HTTP 版本                                                        |

## 缓存

## 参考
