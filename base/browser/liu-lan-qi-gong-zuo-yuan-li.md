# 浏览器工作原理

## 浏览器工作流程：

1. **输入网址。**
2. **浏览器查找域名的IP地址（DNS 解析）**
3. **浏览器给WEB服务器发送一个HTTP请求**
4. **网站服务的永久重定向响应**
5. **浏览器跟踪重定向地址（现在浏览器知道了要访问的正确地址，所以它会发送另一个获取请求）**
6. **服务器“处理”请求，服务器接收到获取请求，然后处理并返回一个响应。**
7. **服务器发回一个HTML响应**
8. **浏览器开始显示HTML**
9. **浏览器发送请求，以获取嵌入在HTML中的对象**

## **浏览器的渲染过程：**

1. **处理html生成Dom（Document Object Model） Tree**
2. **处理css生成CSSOM（CSS Object Model） Tree**
3. **在文档对象模型和CSS对象模型之上，创建一棵由一组待生成渲染的对象组成的渲染树，即RenderTree**
4. **对Render树进行布局计算，即对渲染树上的每个元素，计算它的坐标，称之为布局。**
5. **.绘制即Painting,渲染树上的元素最终展示在浏览器里**

## 引用

[How browsers work](http://taligarsiel.com/Projects/howbrowserswork1.htm)