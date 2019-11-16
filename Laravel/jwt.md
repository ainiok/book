# JWT (json web token)

> 在JavaScript前端技术大行其道的今天，我们通常只需在后台构建API提供给前端调用，并且后端仅仅设计为给前端移动App调用。用户认证是Web应用的重要组成部分，基于API的用户认证有两个最佳解决方案 —— OAuth 2.0 和 JWT（JSON Web Token）。

## JWT定义及其组成

JWT（JSON Web Token）是一个非常轻巧的规范。这个规范允许我们使用JWT在用户和服务器之间传递安全可靠的信息。

一个JWT实际上就是一个字符串，它由三部分组成，头部、载荷与签名。

## 荷载(Payload)

我们先将用户认证的操作描述成一个JSON对象。其中添加了一些其他的信息，帮助今后收到这个JWT的服务器理解这个JWT。

```
{
    "sub": "1",
    "iss": "http://localhost:8000/auth/login",
    "iat": 1451888119,
    "exp": 1454516119,
    "nbf": 1451888119,
    "jti": "37c107e4609ddbcc9c096ea5ee76c667"
}
```

这里面的前6个字段都是由JWT的标准所定义的。

- sub: 该JWT所面向的用户
- iss: 该JWT的签发者
- iat(issued at): 在什么时候签发的token
- exp(expires): token什么时候过期
- nbf(not before)：token在此时间之前不能被接收处理
- jti：JWT ID为web token提供唯一标识

这些定义都可以[标准](https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32)中找到的

将上面的JSON对象进行base64编码可以得到下面的字符串：

```
eyJzdWIiOiIxIiwiaXNzIjoiaHR0cDpcL1wvbG9jYWx
ob3N0OjgwMDFcL2F1dGhcL2xvZ2luIiwiaWF0IjoxNDUxODg4MTE5LCJleHAiOjE0NTQ1MTYxMTksIm5iZiI6MTQ1MTg4OD
ExOSwianRpIjoiMzdjMTA3ZTQ2MDlkZGJjYzljMDk2ZWE1ZWU3NmM2NjcifQ
```

这个字符串我们将它称作JWT的Payload（载荷）。

> 注：Base64是一种编码，也就是说，它是可以被翻译回原来的样子来的。它并不是一种加密过程。


## 头部（Header）

JWT还需要一个头部，头部用于描述关于该JWT的最基本的信息，例如其类型以及签名所用的算法等。这也可以被表示成一个JSON对象：

```
{
  "typ": "JWT",
  "alg": "HS256"
}
```

在这里，我们说明了这是一个JWT，并且我们所用的签名算法（后面会提到）是HS256算法。

对它也要进行Base64编码，之后的字符串就成了JWT的Header（头部）：

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9
```

## 签名（Signature）

将上面的两个编码后的字符串都用句号.连接在一起（头部在前），就形成了：

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxIiwiaXNzIjoiaHR0cDpcL1wvbG9jYWx
ob3N0OjgwMDFcL2F1dGhcL2xvZ2luIiwiaWF0IjoxNDUxODg4MTE5LCJleHAiOjE0NTQ1MTYxMTksIm5iZiI6MTQ1MTg4OD
ExOSwianRpIjoiMzdjMTA3ZTQ2MDlkZGJjYzljMDk2ZWE1ZWU3NmM2NjcifQ
```

最后，我们将上面拼接完的字符串用[HS256算法](https://book.ainiok.com/Encryption/hmac_sha256.md)进行加密。在加密的时候，我们还需要提供一个密钥（secret）:

```
HMACSHA256(
    base64UrlEncode(header) + "." +
    base64UrlEncode(payload),
    secret
)
```

这样就可以得到我们加密后的内容：

```
wyoQ95RjAyQ2FF3aj8EvCSaUmeP0KUqcCJDENNfnaT4
```

这一部分又叫做签名。

最后将这一部分签名也拼接在被签名的字符串后面，我们就得到了完整的JWT：

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxIiwiaXNzIjoiaHR0cDpcL1wvbG9jYWx
ob3N0OjgwMDFcL2F1dGhcL2xvZ2luIiwiaWF0IjoxNDUxODg4MTE5LCJleHAiOjE0NTQ1MTYxMTksIm5iZiI6MTQ1MTg4OD
ExOSwianRpIjoiMzdjMTA3ZTQ2MDlkZGJjYzljMDk2ZWE1ZWU3NmM2NjcifQ.wyoQ95RjAyQ2FF3aj8EvCSaUmeP0KUqcCJDENNfnaT4
```

## 通过JWT 进行认证

JWT 是一个令牌（Token），客户端得到这个服务器返回的令牌后，可以将其存储到 Cookie 或 localStorage 中，此后，每次与服务器通信都要带上这个令牌，你可以把它放到 Cookie 中自动发送，但这样做不能跨域，所以更好的做法是将其放到 HTTP 请求头 `Authorization` 字段里面：

```
Authorization: Bearer <token>
```

服务端收到这个 JWT 令牌后，就可以根据令牌值认定用户身份。