HTTP使用一个“<主>.<次>”的编号方案来表示协议版本。这份规范定义了“1.1”版本。协议版本作为一个整体指示发送者与该版本的相应HTTP规范中规定的一组要求一致。

一个HTTP消息的版本由消息第一行中的HTTP-version域指出。HTTP-version是区分大小写的。

> ```
>  HTTP-version  = HTTP-name "/" DIGIT "." DIGIT
>  HTTP-name     = %x48.54.54.50 ; "HTTP", case-sensitive
> ```

HTTP版本号由两个十进制数组成，由一个“.”（周期或小数点）分割。第一个数字（“主版本”）指示HTTP消息句法，而第二个数字（“次版本”）指示发送者符合的并在后续通讯中能够理解的主版本下的最高次版本。次版本通告发送者的通讯能力（虽然发送者可能仅使用了一个向下兼容的子版本），从而让接收者知道在响应（通过服务器）或者在未来的请求（通过客户端）中可以使用更高级的特性。

当一个一个HTTP/1.1的消息被发送给一个HTTP/1.1的接收者或一个未知版本的接收者，考虑到这种情况，HTTP/1.1消息的构建为忽略所有的新特性也能被视作一个合法的HTTP/1.0消息。本规范在一些新特性上做出了接受者版本的要求，这样一个符合的发送者将只使用兼容的特性，直到通过配置或消息回执确定接收者支持HTTP/1.1。

虽然一个接收者在缺少一个头字段时候的默认行为可能有所不同，但这个头字段的解释在同一个主版本的不同次版本之间没有改变。除非另外指定，否则在HTTP/1.1中定义的头字段是也为所有HTTP/1.x版本定义的。特别是Host和Connection头字段应该被所有的HTTP/1.1实现来实现，而不管他们是否与HTTP/1.1一致。

新的头字段可以在不改变协议版本的情况下被引入，如果它们的定义的语义允许它们被不能识别的接收者安全地忽略它们。 标题字段的扩展性在3.2.1节中讨论。

处理HTTP消息的中介（即所有不是扮演隧道的中介）必须在转发消息中发送他们自己的HTTP版本。另一方面，如果不能确保消息中的协议版本与消息的接收和发送所遵循的版本相匹配，他们就不被允许盲目的转发HTTP消息的第一行。转发一个HTTP消息而不重写HTTP-version可能在下游接收者使用消息发送者的版本来决定在后续通讯中什么特性对那个发送者是安全的的时候导致通讯错误。

客户端**应该**发送一个请求版本，该请求版本等于客户端所符合的最高版本，并且其主要版本不高于服务器所支持的最高版本（如果知道的话）。客户端**不得**发送不符合的版本。

客户端如果知道服务器错误的实现了HTTP规范，它**可能**发送一个低版本的请求，但只有在客户端已经尝试至少一个正常的请求，并且从响应码或头字段（例如，Server）确定服务器不当的处理了高版本的请求之后才会发生。

服务器**应该**发送一个响应版本，该响应版本等于服务器符合的最高版本，该版本的主版本小于或等于请求中收到的版本。服务器**不得**发送不符合的版本。服务器如果想要以任何原因拒绝服务客户端的主版本，可以发送一个505响应（HTTP Version Not Supported [注：HTTP版本不支持]）。

如果知道或者怀疑客户端错误的实现了HTTP规范并且无法正确处理后续版本的响应，例如当一个客户端无法正确的解析HTTP版本数字或者当一个实现被知道在不匹配协议的给定次版本情况下盲目的转发了HTTP-version，服务器**可能**给请求发送一个HTTP/1.0的响应。这种协议降级不应该出现，除非被指定的客户端属性触发，例如当一个或多个请求头字段（例如User-Agent）唯一的匹配到了由已知的错误客户端发送的值。

HTTP版本设计的意图是，主版本号只有在引入不兼容的消息句法时才会增加，次版本号只有在协议的变更影响消息句法或暗示发送者的额外功能时才会增加。然而，对于[RFC2068]和[RFC2616]之间引入的更改，次要版本没有增加，并且这个调整特别避免了对协议的任何这样的改变。

当接收到的HTTP消息的主要版本号是接收者实现的，但是次要版本号比接收者实现的版本号更高时，接收者**应该**处理该消息，就好像它是该主要版本中的接收者符合的最高次要版本。收件人可以认为，当发送给尚未指出对较高版本的支持的收件人时，具有较高次要版本的消息足够向后兼容，以便由相同主要版本的任何实现进行安全处理。