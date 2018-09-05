# WKWebView打开本地H5页面，链接CSS和JS文件无效的问题

之前一直是把样式、JS代码和HTML都写在以`.html`结尾的文件里（界面比较简单，代码量少，所以都写在同一个文件里），然后直接调用如下代码加载：
```
NSString *content = [NSString stringWithContentsOfFile:[[NSBundle mainBundle] pathForResource:@"JSSDK" ofType:@"html"] encoding:NSUTF8StringEncoding error:nil];
[self.webView loadHTMLString:content baseURL:nil];
```
不过这次遇到的界面比较复杂，代码量比较多，所以为了方便阅读，就将CSS和JS代码单独抽了出来，然后在HTML文件里导入。Run起来之后，发现样式全部失效，按钮点击之后的JS函数也不响应了。代码反复看了好几遍也没发现问题，看了一下官方文档也没有找到线索，直觉告诉我应该是`baseURL`设置为`nil`导致的，但是该怎么改呢？无奈只能Google一下了。最终解决问题的代码修改如下：
```
NSString *content = [NSString stringWithContentsOfFile:[[NSBundle mainBundle] pathForResource:@"JSSDK" ofType:@"html"] encoding:NSUTF8StringEncoding error:nil];
[self.webView loadHTMLString:self.loadHtml baseURL:[NSURL fileURLWithPath:[[NSBundle mainBundle] bundlePath]]];
```
