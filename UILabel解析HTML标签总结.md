将字符串中的某些文字进行着色（将消息中心的每一条消息内容中的金额变色）  
========================

>## 初次尝试实现需求

### 刚看到需求时，我首先想到的实现方式就是使用正则，找出字符串当中所有的数字，然后定位该数字在字符串中的位置，使用NSMutableAttributedString的addAttribute方法对该数字进行着色。后来仔细的想了想，觉得这种方法比较繁琐并且不够灵活。后来在网上查阅资料才知道UILbal是可以解析文本中的HTML标签的，需要使用到以下几个关键词：`NSAttributedString`、`NSDocumentTypeDocumentAttribute`、`NSHTMLTextDocumentType`，所以直接让拍黄片的同学直接在接口返回的数据里面加上`<font>`标签。

### 废话不多说，直接上代码：
```
NSString *exampleStr = @"订单总金额<font color='#333333'">100.00</font>元，返现<font color='#333333'">50.00</font>元"; // 该参数不能为nil，如果为nil在下面的解析会crash

NSAttributedString *attrStr = [[NSAttributedString alloc] initWithData:[exampleStr dataUsingEncoding:NSUnicodeStringEncoding] options:@{NSDocumentTypeDocumentAttribute: NSHTMLTextDocumentType} documentAttributes:nil error:nil];
```

### 很简单，两行代码就实现了，但是当我Run起来的时候，发现根本不是我想要的效果，相较于我storyboard中的默认设置，整个文本的字体变小了，没有被便签`<font color='#333333'></font>`包裹的文本，颜色也由我原先默认设置的灰色变成了黑色。

>## 解决需求实现过程中遇到的问题

### 1、解决字体缩小的问题

#### 首先想到的是在`<font>`标签中加上字体大小的属性，`<font color='#333333' size=22px></font>`，再次Run，发现字体变无比巨大，无论怎么修改`size`属性的值都没用，故而放弃了这种方法。

#### 最后是在`UILabel`的`attributedText`设置完后，重新设置一下`UILabel`的字体大小，最终代码如下：

```
NSString *exampleStr = @"订单总金额<font color='#333333'">100.00</font>元，返现<font color='#333333'">50.00</font>元"; // 该参数不能为nil，如果为nil在下面的解析会crash

NSAttributedString *attrStr = [[NSAttributedString alloc] initWithData:[exampleStr dataUsingEncoding:NSUnicodeStringEncoding] options:@{NSDocumentTypeDocumentAttribute: NSHTMLTextDocumentType} documentAttributes:nil error:nil];

self.contentLabel.attributedText = attrStr;
self.contentLabel.font = [UIFont systemFontOfSize:11.0f];
```

### 2、默认字体颜色改变的问题

#### 这个问题最后是通过服务端修改返回的数据解决的，代码如下：

```
NSString *exampleStr = @"<font color='#666666'">订单总金额<font color='#333333'">100.00<font color='#666666'">元，返现<font color='#333333'">50.00<font color='#666666'">元";
```

#### 经过验证发现`</font>`这个尾巴不要并不会出现问题。

#### 最后还有一个优化点我没有写，就是将得到的`attrStr`缓存起来，因为消息中心是一个列表，这样在滚动的时候就可以直接使用缓存，不必频繁的解析HTML标签。