503（服务不可用）状态码表明服务器目前由于临时过载或定期维护不能处理请求。服务器**可以**发送一个Retry-After头字段（7.1.3节）来建议客户端在重试请求之前等待一段合适的时间。

*注意：503状态码的存在并不意味着服务器在服务器过载的时候必须使用它。一些服务器可能简单的拒绝连接。*