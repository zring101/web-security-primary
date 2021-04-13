# Web安全测试手册（初级）

本手册最终目的在于指导如何通过简单的操作发现常见应用安全漏洞，后续深入利用请参考各大安全论坛（或者等本手册中级更新）

如果把WEB/APP看成一个抽象意义上的程序，它们（程序）最基本也是最核心的功能，就是处理输入输出流。一个典型的程序输入输出流如下：

```bash
user_input=input()
program_output=code_exec(user_input)
screen_print(program_output)
```

一个经过初步安全防护的程序流程如下：

```bash
user_input=input()
user_input_after_filter=safe_func(user_input)
program_output=code_exec(user_input_after_filter)
screen_print(program_output)
```

衡量一个程序安全性的标准分以下两点：

1、有无safe_func()

2、功能接口常规输入可以变形成哪些有攻击性的输入？safe_func()过滤这些恶意字符串的能力如何？

**对于普通站点而言，所有的输入流均来自不可信的用户，你永远无法预测用户的输入（即便是网站本身的参数也可以被用户恶意修改）但是你可以通过代码控制站点的输出（输出是指广义上的输出，调用其他模块功能也属于输出），所以掌握基本的Web安全测试能力对于提升网站整体安全性十分关键。**

**Web安全测试，原则上就是利用【精心构造】的user_input去测试能否得到【该功能存在漏洞】的program_output。**

本手册除**基础知识**外，包含两个大的模块，服务端（backend）和用户端（frontend），模块中的每章结构均为：简介-原理-技巧

简介：符合直觉理解的图文【WHAT】

原理：概括类型漏洞的产生原因【WHY】

技巧：攻防两端的对抗升级【HOW】

#如果你对于HTTP协议及相关工具十分熟悉，可以跳过基础知识章节。

#否则，建议阅读过后再进入具体的漏洞类型

**章节列表：**

**#基础知识**

[HTTP数据包](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/HTTP%E6%95%B0%E6%8D%AE%E5%8C%85%202b20560a623644cc9e61d09cd0328758.md)

[COOKIE、JWT](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/COOKIE%E3%80%81JWT%207bef3a6028734a46bfb2935d522a8318.md)

[Burpsuite基本功能介绍](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/Burpsuite%E5%9F%BA%E6%9C%AC%E5%8A%9F%E8%83%BD%E4%BB%8B%E7%BB%8D%20ad08a490531544e2877b0929466c2781.md)

[盲打与无回显](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/%E7%9B%B2%E6%89%93%E4%B8%8E%E6%97%A0%E5%9B%9E%E6%98%BE%20d579a79936d54ba8a2e36b71edd15f1e.md)

[网站信息泄漏](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/%E7%BD%91%E7%AB%99%E4%BF%A1%E6%81%AF%E6%B3%84%E6%BC%8F%20d72199d7d5334255916ff3c412c00d3e.md)

[XML](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/XML%208b548a89843c4963bcd4ba6f280697d3.md)

[AJAX、同源策略、JSONP、CORS](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/AJAX%E3%80%81%E5%90%8C%E6%BA%90%E7%AD%96%E7%95%A5%E3%80%81JSONP%E3%80%81CORS%20bbfac3abde5f499db2dbebb9b2ac89e5.md)

**#服务端**

[目录遍历](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/%E7%9B%AE%E5%BD%95%E9%81%8D%E5%8E%86%201c83a0e8d48f41108cfae55172639eab.md)

[命令注入](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/%E5%91%BD%E4%BB%A4%E6%B3%A8%E5%85%A5%20a0887fdcad3d48fd9fb54a1097816ef7.md)

[越权](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/%E8%B6%8A%E6%9D%83%2043b0e0ac79644272866b5d66b6f21c93.md)

[文件上传](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%20fd253a201d6a4dcc8a20f4b2dfb9420f.md)

[XXE](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/XXE%203b1954df08834d3485fe3b8ca3e1caa5.md)

[逻辑漏洞（登陆、重置、业务等）](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/%E9%80%BB%E8%BE%91%E6%BC%8F%E6%B4%9E%EF%BC%88%E7%99%BB%E9%99%86%E3%80%81%E9%87%8D%E7%BD%AE%E3%80%81%E4%B8%9A%E5%8A%A1%E7%AD%89%EF%BC%89%20ee502be15db44943b52ef21e3a88ce2e.md)

[SQL注入](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/SQL%E6%B3%A8%E5%85%A5%203549da5b5d9e43f49d997b3497f76487.md)

[SSRF](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/SSRF%205eabcf9885914010a5619bd5d875fa32.md)

**#用户端**

[XSS](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/XSS%20cbfd67a6ffd948b980b7eef26267b582.md)

[CSRF](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/CSRF%2042513c0335514295ab5e67e7894afa5d.md)

[不合理的CORS配置](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/%E4%B8%8D%E5%90%88%E7%90%86%E7%9A%84CORS%E9%85%8D%E7%BD%AE%20846bd21e40a146c8a1270bfd9d06c8b4.md)

[JSONP](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/JSONP%20cff118074c50464b9eee55a31e2ed116.md)

[URL跳转](Web%E5%AE%89%E5%85%A8%E6%B5%8B%E8%AF%95%E6%89%8B%E5%86%8C%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89%205ba12b990dcc4b36916c39f0bfa556b9/URL%E8%B7%B3%E8%BD%AC%20b5fd8782d8e14b6daa7e50cb0b531898.md)

**#致谢，本手册在编辑期间参考了如下网站及书籍，排名不分先后**

xz.aliyun.com

paper.seebug.org

portswigger.net

freebuf.com

白帽子讲web安全

从0到1：ctfer成长之路