# \## 400-499 客户端错误状态码

有时候客户端会发送一些服务器无法处理的东西， 比如格式错误地请求报文，或者最常见的是， 请求一个不存在的URL。

400  用于告知客户端它发送了一个错误请求

401  与适当的首部一同返回， 对这些首部中请求客户端再获取对资源的访问权之前， 对自己进行认证。

402  现在这个状态码还未使用，但已经被保留，以作未来之用。

403  用于说明请求被服务器拒绝了。 如果服务器想说明为什么拒绝请求，可以包含实体的主题部分对原因进行描述。

404 用于说明服务器无法找到所请求的URL。通常会包含一个实体，以便客户端应用程序显示给用户看

405 发起的请求中带有所请求的URL不支持的方法时， 使用此状态码。 应该在响应中包含allow首部，已告知客户端对所请求的资源可以使用哪些方法。

406 客户端可以指定参数来说明它们愿意接收什么类型的实体。

407 与401状态码类似，但用于要求对资源进行认证的代理服务器。

408 如果客户端完成请求所花的时间太长， 服务器可以回送此状态码，并关闭连接。

409 用于说明请求可能在资源上引发的一些冲突。 服务器担心请求会引发冲突时，可以发送此状态码。

400-499是客户端错误的状态码。  400告知客户端发送的是一个错误的请求，405是发起的请求中URL不支持该方法。 403的请求是被服务端拒绝了，具体原因可以在相应报文中显示。

**出处**：[Gourley David,Totty Brian《HTTP权威指南》(2012) P<页码>](zotero://select/library/items/WK5NQJZ4)
