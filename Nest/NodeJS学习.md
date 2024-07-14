!> 安装 @types/node 使VScode有node代码的智能提示：npm i -D @types/node

## Node概述

- Node官网：https://nodejs.org/en/
- Node民间中文网：http://nodejs.cn/

- Node是一个js运行环境，基于V8引擎，运行的是高性能V8 JS脚本语言，比浏览器拥有更多的能力

- 浏览器中的JS是EcmaScript和Web Api的组合，Web Api提供了BOM、DOM和AJAX来操作浏览器窗口和页面，但能力十分有限，会产生跨域问题和文件读写问题，只能使用浏览器提供的功能做有限的操作

- NodeJS是EcmaScript与Node Api的组合，Node Api几乎提供了所有能做的事，提供了完整的控制计算机的能力，几乎可以通过Node提供的接口，实现对整个操作系统的控制
单线程、异步、事件驱动

- Node可以开发桌面应用程序，像聊天软件和VScode等，但是目前用Node开发桌面应用程序非常少

- Node最常用于开发服务器应用程序，通常有两个结构 
  - 做浏览器与数据库交流的中间服务器应用，常用于微型站点上。Node服务器要完成请求的处理、响应和数据库的交互以及各种业务逻辑
  - 做浏览器与后端服务器的中间服务器应用，最常见的一种结构，应用在各种规模的站点上。Node服务器不做任何与业务逻辑有关的事情，绝大部分的时候只是简单的转发请求，但可能有时候会有一些额外的功能，比如简单的信息记录(请求日志、用户偏好、广告信息等)、静态资源托管、缓存等

## Node的全局对象

1. setTimeout       计时执行 
2. setlnterval      每多少时执行 
3. setlmmediate     类似于setTimeout0，立即执行 
4. console          控制台 
5. __dirname        用于获取当前目录 (并非global属性) 
6. __filename       用于获取当前模块文件的文件路径 (并非global属性) 
7. Buffer           一个类型化数组，继承自UInt8Array，计算机中存储的基本单位是字节，使用和输出时可能需要用十六进制表示 
8. process 
9. cwd()      返回当前nodejs进程的工作目录，绝对路径
10. exit()     强制退出当前node进程，可传入退出码，0表示成功退出，默认为0，1表示失败
11. argv       String[]，获取命令中所有的参数
12. platform   获取当前的操作系统
13. kill(pid)  根据进程ID杀死进程
14. env        获取环境变量对象

### 模块的查找

- 模块都是根据绝对路径进行直接加载 
- 相对于当前模块的相对路径./或../会在底层转换为绝对路径进行加载 
- 相对路径查找顺序： 
  - 检查是否内置模块，如fs、path等
  - 检查当前目录中的node_modules
  - 检查上级目录中的node_modules
  - 转换为绝对路径
  - 加载模块
- 可以不写后缀名，会自动补全，顺序是.js、.json、.node、.mjs 
- 如果只提供目录不提供文件名，则会自动寻找该目录中的index.js，也可以配置package.json中的main字段(包的默认入口)，导入或执行包时若仅提供目录，则使用main补全入口，默认为index.js 
- module对象，记录当前模块信息 
- require函数，导出值 
- 当执行一个模块或使用require时，会将模块放置在一个函数环境中 

### Node中的ES模块化

目前仍然处于试验阶段，Node模块默认是commonJS，ES模块需要将文件后缀名改为.mjs，或最近的package.json中type的值为module，当使用ES模块化运行时，必须添加--experimental-modules标记 

## 基本内置模块(查文档)

1. os 
2. EOL           操作系统特定的行末标志
3. arch()        返回为其编译 Node.js 二进制文件的操作系统的 CPU 架构
4. cpus()        返回一个对象数组，其中包含有关每个逻辑 CPU 内核的信息
5. freemem()     以整数的形式返回空闲的系统内存量（以字节为单位）
6. homedir()     返回当前用户的主目录的字符串路径
7. hostname()    以字符串的形式返回操作系统的主机名
8. tmpdir()      以字符串的形式返回操作系统的默认临时文件目录
9. path 
10. basename      path.basename() 方法会返回 path 的最后一部分，尾部的目录分隔符会被忽略
11. sep           提供平台特定的路径片段分隔符，Windows 上是 \，POSIX 上是 /；在 Windows 上，正斜杠（/）和反斜杠（\）都被接受为路径片段分隔符。 但是， path 方法只添加反斜杠（\）
12. delimiter     提供平台特定的路径定界符, Windows 是 ; ，POSIX是 :
13. dirname       path.dirname() 方法会返回 path 的目录名，类似于 Unix 的 dirname 命令。 尾部的目录分隔符会被忽略
14. extname       path.extname() 方法会返回 path 的扩展名，即 path 的最后一部分中从最后一次出现 .（句点）字符直到字符串结束。 如果在 path 的最后一部分中没有 . ，或者如果 path 的基本名称除了第一个字符以外没有 . ，则返回空字符串
15. join          path.join() 方法会将所有给定的 path 片段连接到一起（使用平台特定的分隔符作为定界符），然后规范化生成的路径。长度为零的 path 片段会被忽略。 如果连接后的路径字符串为长度为零的字符串，则返回 '.'，表示当前工作目录
16. normalize     path.normalize() 方法规范化给定的 path，解析 '..' 和 '.' 片段，当找到多个连续的路径段分隔字符时（例如 POSIX 上的 /、Windows 上的 \ 或 /），则它们将被替换为单个平台特定的路径段分隔符（POSIX 上的 /、Windows 上的 \）。 尾部的分隔符会保留。如果 path 是零长度的字符串，则返回 '.'，表示当前工作目录
17. relative      path.relative(from, to) 方法根据当前工作目录返回 from 到 to 的相对路径。 如果 from 和 to 各自解析到相同的路径（分别调用 path.resolve() 之后），则返回零长度的字符串。如果将零长度的字符串传入 from 或 to，则使用当前工作目录代替该零长度的字符串
18. resolve       path.resolve() 方法会将路径或路径片段的序列解析为绝对路径。给定的路径序列会从右到左进行处理，后面的每个 path 会被追加到前面，直到构造出绝对路径。 例如，给定的路径片段序列：/目录1、 /目录2、 目录3，调用 path.resolve('/目录1', '/目录2', '目录3') 会返回 /目录2/目录3，因为 '目录3' 不是绝对路径，但 '/目录2' + '/' + '目录3' 是。如果在处理完所有给定的 path 片段之后还未生成绝对路径，则会使用当前工作目录。生成的路径会被规范化，并且尾部的斜杠会被删除（除非路径被解析为根目录）。零长度的 path 片段会被忽略。如果没有传入 path 片段，则 path.resolve() 会返回当前工作目录的绝对路径
19. url   查文档 据浏览器的约定， URL 对象的所有属性都是在类的原型上实现为getter和setter，而不是作为对象本身的数据属性。因此，与传统的urlObjects不同，在 URL 对象的任何属性(例如 delete myURL.protocol， delete myURL.pathname等)上使用 delete 关键字没有任何效果，但仍返回 true
20. util 
21. callbackify            将 async 异步函数（或者一个返回值为 Promise 的函数）转换成遵循异常优先的回调风格的函数，例如将 (err, value) => ... 回调作为最后一个参数。 在回调函数中，第一个参数为拒绝的原因（如果 Promise 解决，则为 null），第二个参数则是解决的值。回调函数是异步执行的，并且有异常堆栈错误追踪。 如果回调函数抛出一个异常，进程会触发一个 'uncaughtException' 异常，如果没有被捕获，进程将会退出。null 在回调函数中作为一个参数有其特殊的意义，如果回调函数的首个参数为 Promise 拒绝的原因且带有返回值，且值可以转换成布尔值 false，这个值会被封装在 Error 对象里，可以通过属性 reason 获取。
22. inherits               不建议使用 util.inherits()。 请使用 ES6 的 class 和 extends 关键词获得语言层面的继承支持。 这两种方式是语义上不兼容的。
从一个构造函数中继承原型方法到另一个。 constructor 的原型会被设置到一个从 superConstructor 创建的新对象上。这主要在 Object.setPrototypeOf(constructor.prototype, superConstructor.prototype) 之上添加了一些输入验证。 作为额外的便利，可以通过 constructor.super_属性访问 superConstructor。
23. isDeepStrictEqual      util.isDeepStrictEqual(val1, val2), 如果val1和val2之间严格相等，则返回true。否则,返回false。
24. promisify              传入一个遵循常见的错误优先的回调风格的函数（即以 (err, value) => ... 回调作为最后一个参数），并返回一个返回 promise 的版本。

## 文件的I/O  input output

即对外部设备的输入输出，外部设备包含磁盘、网卡、显卡、打印机等设备。IO的速度往往低于内存和CPU的交互速度

1. fs模块 (查文档) 
2. fs.readFile      异步读取一个文件，回调会传入两个参数 (err, data)，其中 data 是文件的内容。  
3. fs.writeFile     向文件写入内容 
4. fs.stat          获取文件或目录信息 
5. size     占用字节
6. atime    上次访问时间
7. mtime    上次内容被修改时间
8. ctime    上次文件状态被修改时间
9. birthtime   文件创建时间
10. isDirectory()  判断是否是目录
11. isFile()       判断是否是文件
12. fs.readdir       获取目录中的文件和子目录 
13. fs.mkdir         创建目录 
14. fs.exists        判断文件或目录是否存在，已经被废弃 

## 文件流

流是指数据的流动，数据从一个地方缓缓的流动到另一个地方，流是有方向的

1. 可读流：Readable  数据从源头流向内存
2. 可写流：Writable  数据从内存流向源头
3. 双工流：Duplex    数据既可从源头流向内存，也可从内存流向源头

其他介质和内存的数据规模和数据处理能力都不一定一致，像磁盘的数据规模比内存大很多，但内存的数据处理速度又比磁盘快得多，所以需要流来做缓冲

文件流就是内存数据和磁盘文件数据之间的流动

### 文件流的创建

1. fs.createReadStream(path,[options])：创建一个文件可读流，用于读取文件内容
   1. path: 读取的文件路径
   2. options：可选配置
      1. encoding：编码方式
      2. start：起始字节
      3. end：结束字节
      4. highWaterMark：每次读取数量，如果encoding有值，该数量表示一个字符数，如果encoding为null，该量表示字节数
2. 返回：Readable的子类ReadStream：
   1. 事件：rs.on(事件名，处理函数)
   2.  open: 文件打开事件，文件被打开后触发
   3.  error: 发生错误后触发
   4.  close: 文件被关闭后触发，可通过rs.close手动关闭，或文件读取完成后自动关闭，auoClose配置选项，默认为true
   5.  data: 读取到一部分数据后触发，注册data事件后，才会真正开始读取，每次读取highWaterMark指定的读取数量；回调函数中会附带读取到的数据chunk，若指定编码，则读取到的数据会自动按照编码转换为字符串，若没有指定编码，读取到的数据是Buffer
   6.  end: 所有数据读取完毕后触发
   7.  rs.pause(): 暂停读取，会触发pause事件
   8.  rs.resume(): 恢复读取，会触发resume事件

### 文件流的写入

1. fs.createWriteStream(path,[options])：创建一个写入流
   1. path：写入文件的路径
2. options：可选配置
   1. flags: 操作文件的方式
   2. w: 覆盖
   3. a: 追加
   4. encoding：编码方式
   5. start：起始字节
   6. highWaterMark：每次最多写入的字节数
3. 返回：Writable的子类WriteStream
   1. ws.on(事件名, 处理函数)
      1. open: 文件打开事件，文件被打开后触发
      2. error: 发生错误后触发close: 文件被关闭后触发
      3. ws.write(data): 写入一组数据，data可以是字符串或Buffer
         1. 返回一个boolean值：
         2. true：写入通道没有被填满，接下来的数据可以直接写入，无须排队
         3. false：写入通道目前已被填满，接下来的数据将进入写入队列，要特别注意背压问题，因为写入队列是内存中的数据，是有限的
         4. 当写入队列清空时，会触发drain事件
      4. ws.end([data])：结束写入，将自动关闭文件(是否自动关闭取决于autoClose配置，默认为true)，data是可的，表示关闭前最后一次写入

     rs.pipe(ws): 将可读流连接到可写流，返回参数的值，该方法可解决被压问题

## net模块

- http请求有两种模式，一种是普通模式，先经过三次握手，然后客户端发送请求，服务器响应，接着四次挥手断开连接；二是长连接模式，客户端与服务器三次握手后进行持续的请求响应，不断开，一方请求断开的时候再进行四次挥手
- net模块是一个通信模块，它可以实现进程间的通信IPC，网络通信TCP/IP
- 创建客户端：net.createConnection(options[, connectListener])
  - 返回socket，socket是一个特殊的文件，在node中表现为一个双工流对象，通过向流写入内容发送数据，通过监听流的内容获取数据
- 创建服务器：net.createServer()
  - 返回server对象：
    - server.listen(port)：监听当前计算机中某个端口
    - server.on("listening", ()=>{})：开始监听端口后触发的事件
    - server.on("connection", socket=>{})：当某个连接到来时，触发该事件，事件的监听函数会获得一个socket对象

## http模块

- http模块建立在net模块之上，无须手动管理socket，无须手动组装消息格式
- http.request(url[, options][, callback]) 请求，查文档：https://nodejs.org/dist/latest-v12.x/docs/api/http.html#http_http_request_url_options_callback
- http.createServer([options][, requestListener])  创建服务器，查文档：https://nodejs.org/dist/latest-v12.x/docs/api/http.html#http_http_createserver_options_requestlistener
- 总的来说，就是如果是客户端，则请求：ClientRequest对象； 响应：IncomingMessage对象
- 如果是服务器，则请求：IncomingMessage对象； 响应：ServerResponse对象