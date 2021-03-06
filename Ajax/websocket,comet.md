# Comet

两种实现Comet的方式：长轮询和流

## 长轮询

长轮询是页面发起一个请求，然后服务器一直保持打开状态，直到有数据可以发送，发送完数据之后，浏览器关闭连接，随机又发起一个到服务器的新请求，这一过程在页面打开期间一致持续不断。

无论是长轮询还是短轮询，浏览器都要在接受数据之前，发起对浏览器的连接，两者最大的区别在于浏览器核实发送数据，短轮询要求浏览器立即发送数据，无论数据是否有效，长轮询是等待发送响应，通过`XHR`和`setTimeout`就可以实现，我们需要处理的就是决定什么时候发送。

## 实现Http流

浏览器向服务器发送一个请求，而浏览器保持连接打开，然后周期性的向浏览器发送数据，在浏览器中，我们通过`onreadystatechange`监听,检测`readystate`状态，当readystate的值变为3时`responseText`属性就会保存接收到的所有数据，这样我们来比较接受到的数据和之前的数据，决定从什么位置开始接收数据

## 服务端发送时间

### SE

我们要预定新的事件流，首先要创建一个新的`EventSource`对象，并传入一个入口点。

```js
var source = new EventSource("url");
```

我们传入的url必须与创建的页面同源，，在eventSource中有一个readyState属性，0表示正连接到服务器，值为1表示浏览器打开了连接，值为2表示关闭了连接。

我们之后有三个事件

- open:在建立连接时触发
- message：在从服务器上收到新事件时触发
- error:再无法创建连接时触发

```js
source.onmessgae = function(evnet){
    source.date = event.data;
}
```

我们关闭连接时，可以调用close()来关闭连接。

`source.close();`

`SSE`响应的格式是纯文本，相应的`MIME`类型为`text/eventstream`，最简单的情况是每个数据项都带有`data:`

> **对于多个连续的以data:开头的数据行，将作为多段数据解析，每个值之间以一个换行符进行分隔，只有包含data:数据之后有空行时才会触发message事件,通过id:可以给特定事件关联一个ID，这个ID位于data:行前或者后面都行**

设置了id之后，`eventSource`会跟踪上一次触发的事件，如果浏览器断开,会向浏览器发送一个包含名为`Last-Event-ID`的特殊的HTTP请求头部，以便服务器知道下一次触发哪个事件，在多次连接对的事件流中，这种机制可以确保浏览器以确定的舒徐收到连接的数据段。

## Web  socket

我们在javascript中创建了Web socket之后会有一个HTTP请求发送到浏览器以发起连接，在取得响应之后，建立的连接会使用HTTP升级从HTTP协议交换为Web socket协议，也就是说，使用标准的HTTP浏览器午发使用web socket.

web scoket协议使用了自己的协议，对于URL模式也有不同，一般为`ws://`，加密之后的为`wss://`
