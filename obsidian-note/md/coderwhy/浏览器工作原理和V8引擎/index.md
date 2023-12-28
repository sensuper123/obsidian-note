# 浏览器工作原理
1. 浏览器下载资源
	1. 浏览器输入域名，经域名解析成ip地址，到对应的静态服务器，下载index.html文件，浏览器解析html，遇到link标签下载对应的css文件，遇到script标签下载对应的js文件
	2. ![image.png](https://s2.loli.net/2023/12/29/Wuj4Qc7XxqL8hiS.png)
	3. 下载下来的资源交由浏览器内核解析，就能在网页上渲染页面
2. 浏览器内核是什么
	1. 不同浏览器有不同的内核
	2. ![image.png](https://s2.loli.net/2023/12/29/4RsAEKauqNiDz62.png)
	3. 浏览器内核也指浏览器的排版引擎
3. 浏览器渲染页面流程
	1. 下载下来的html代码，会经过HTML Parser解析生成DOM Tree，在此过程中，如果遇到link标签，浏览器会启动异步任务下载对应的CSS文件，直到拿到css文件后会交给CSS parser 解析，html解析完生成的DOM Tree 会与 CSS解析完生成的样式规则结合，生成render tree，之后浏览器再根据渲染树进行painting渲染到屏幕上
	2. AI描述
		1. **HTML 解析和 DOM Tree 构建：** 浏览器在解析 HTML 时会构建 DOM Tree，这个过程是逐行进行的。HTML 解析器遇到 HTML 标签时会生成相应的 DOM 节点，直到整个文档被解析完毕。这时，生成的 DOM Tree 包含了 HTML 中的元素和其它相关信息。
		2. **异步下载 CSS 文件：** 如果在 HTML 中遇到 `<link>` 标签引用外部 CSS 文件，浏览器会启动异步任务去下载该 CSS 文件。这个过程不会阻塞 HTML 解析，浏览器可以继续构建 DOM Tree。
		3. **CSS 文件下载完成后的处理：** 当浏览器异步下载完 CSS 文件后，它会交给 CSS 解析器进行解析。解析器将 CSS 代码转换成浏览器能够理解的样式规则。这一步完成后，浏览器不会等待所有 CSS 文件都下载完，而是会尽早开始构建渲染树（Render Tree）。
		4.  **构建渲染树：** 渲染树是由 DOM Tree 和样式规则组成的，用于表示页面的可视化结构。渲染树中的每个节点对应于 DOM Tree 中的一个节点，但只包括那些需要在屏幕上渲染的节点，例如可见的元素和应用了样式的元素。
		5. **Layout 和 Painting：** 渲染树构建完成后，浏览器执行 layout（布局）和 painting（绘制）阶段。在 layout 阶段，浏览器确定每个节点在屏幕上的确切位置和大小。在 painting 阶段，浏览器使用渲染树和 layout 信息将页面绘制到屏幕上。
	3. ![image.png](https://s2.loli.net/2023/12/29/C5itxBVaYRkUI3f.png)
# js引擎
1. js引擎将高级语言转换成机器语言
2. 机器语言交给cpu执行
# 浏览器内核与js引擎的关系
浏览器内核实际包括两个部分，以WebKit为例
1. WebCore：负责Html、css解析布局、渲染等工作
2. JavaScriptCore：解析、执行JavaScript代码

# V8引擎执行js分析
1. V8首先会把源代码交给Parse模块进行词法分析、语法分析，之后生成AST抽象语法树
2. 抽象语法树再由Ignition解释器转成字节码
3. 字节码如果不经过TurboFan转成机器码，那么会由Ignition直接执行，Ignition 解释器会逐条解释字节码，并将其转换为 CPU 能够执行的底层指令。
4. 如果代码执行很多次，那么TurboFan会拿到AST直接转成机器码，之后直接执行机器码，如果V8发现机器码执行无效，例如一个函数返回两个数值相加，突然传入了两个字符串，这样子那个函数转成的机器码就无效了，就会反优化，回到字节码那步，重新编译转成机器码
5. 字节码是跨平台的，在ignition中，不直接转成机器码是因为不知道JavaScript代码运行在哪个平台，不同平台的CPU接受指令有些是不同的，所以转成字节码，等到运行时再转成对应平台的机器码
6. ![image.png|625](https://s2.loli.net/2023/12/29/WmOeicLXCs6GMq2.png)
7. parse过程
	1. ![image.png](https://s2.loli.net/2023/12/29/W2tIKQZTJyohfcm.png)








