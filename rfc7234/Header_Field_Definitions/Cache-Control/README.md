“Cache-Control”头字段用于指定请求/响应链上的缓存指令。这个缓存指令是单向的，因为在请求中存在指令并不意味着在响应中给出相同的指令。

缓存**必须**遵循本节定义的Cache-Control指令的要求。查看5.2.3节的信息了解关于其他地方定义的Cache-Control指定如何处理。

*注意：一些HTTP/1.0的缓存可能没有实现Cache-Control。*

一个代理，如果它是否实现了一个缓存，**必须**在转发的消息中传递缓存指令，而不管它对那么应用程序的意义，因为指令可能适用于所有请求/响应链上的接收者。它不可能将一个指令只作用于一个特定的缓存。

缓存指令是由标识符识别的，不区分大小写的进行比较，并且有一个可选的参数，可以使用标记和双引号语法。对于下面定义的定义了参数的指令，接收者应该接收两种形式，即使其中一个是被记录为优先的。对于任何没有在本规范定义的指令，接收者**必须**接受两种形式。

> ```
>      Cache-Control   = 1#cache-directive
>
>      cache-directive = token [ "=" ( token / quoted-string ) ]
> ```

对于下面定义的缓存指令，没有参数被定义（也不允许），除非单独申明。