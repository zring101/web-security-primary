# AJAX、同源策略、JSONP、CORS

## AJAX的定义及用途

AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。

## AJAX的工作原理

![AJAX%E3%80%81%E5%90%8C%E6%BA%90%E7%AD%96%E7%95%A5%E3%80%81JSONP%E3%80%81CORS%20bbfac3abde5f499db2dbebb9b2ac89e5/Untitled.png](AJAX%E3%80%81%E5%90%8C%E6%BA%90%E7%AD%96%E7%95%A5%E3%80%81JSONP%E3%80%81CORS%20bbfac3abde5f499db2dbebb9b2ac89e5/Untitled.png)

## 同源策略的定义及用途

同源是指三个相同：协议、域名、端口

同源是为了防止B网页的JS等动作请求A网页进行：

1、获取cookie、localstorage、IndexDB

2、获取DOM

3、发送AJAX

## 一些规避同源策略的方式

### cookie的两种规避方式

- Set-Cookie: key=value; [domain=.example.com](http://domain%3D.example.com/); path=/
- document.domain = '[example.com](http://example.com/)';（两个网页设置相同的document.domain）

### AJAX的三种规避方式

- JSONP：网页通过添加一个<script>元素，向服务器请求JSON数据，这种做法不受同源政策限制；服务器收到请求后，将数据放在一个指定名字的回调函数里传回来。如下图，可以通过foo()函数进行data获取并打印log

![AJAX%E3%80%81%E5%90%8C%E6%BA%90%E7%AD%96%E7%95%A5%E3%80%81JSONP%E3%80%81CORS%20bbfac3abde5f499db2dbebb9b2ac89e5/Untitled%201.png](AJAX%E3%80%81%E5%90%8C%E6%BA%90%E7%AD%96%E7%95%A5%E3%80%81JSONP%E3%80%81CORS%20bbfac3abde5f499db2dbebb9b2ac89e5/Untitled%201.png)

- WebSocket：WebSocket是一种通信协议，使用ws://（非加密）和wss://（加密）作为协议前缀。该协议不实行同源政策，只要服务器支持，就可以通过它进行跨源通信。请求中会包含：[upgrade: websocket]及[origin]特征
- CORS：见下

## CORS的定义及用途

由于AJAX只能在同源网站中使用，对于现代网站的建设存在诸多不便之处，所以W3C设置了Cross-origin resource sharing（CORS，跨域资源共享）的标准，用以进行非同源AJAX请求。

## CORS的便捷性

整个CORS通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。

因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。

## CORS的特征

### 请求包特征：

包含Origin字段，如Origin: [http://api.bob.com](http://api.bob.com/)，依然是三要素，协议，域名，端口

### 返回包特征：

包含下列三个字段中的一个或多个

（1）Access-Control-Allow-Origin 接受域名的范围

（2）Access-Control-Allow-Credentials 是否允许发送cookie

（3）Access-Control-Expose-Headers 

CORS请求时，XMLHttpRequest对象的getResponseHeader()方法只能拿到6个基本字段：Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma。如果想拿到其他字段，就必须在Access-Control-Expose-Headers里面指定。