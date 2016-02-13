##Objective-C 编程规范_学习总结
***
###**代码格式**

- **使用空格缩进而不是制表符Tab**<br/>
  在 Xcode 中将Tab和自动缩进都设置为4个空格
- **每一行最大长度为 80**
- **函数的书写**<br/>
  一个典型的Objective-C函数<br/>
     
		- (void)writeVideoFrameWithData:(NSData *)frameData timeStamp:(int)timeStamp {
		...
		}
在 `-` 和 `（void）` 之间应该又一个空格，第一个大括号 `{` 的位置在函数所在行的末尾，同样应该有一个空格。
如果一个行数有特别多的参数或者名称很长，应该将其按照 `:` 来对齐，分行显示。<br/>
在分行时，如果第一段名称果断，后续名称可以以 Tab 的长度（4个空格）为单位进行缩进。<br/>
- **函数调用**<br/>
  函数调用的格式和书写差不多，可以按照函数的长短来选择写在一行或者分成多行<br/>
- **@public和@private标记符**？？？？？<br/>
  @public和@private标记符应该以 一个空格 来进行缩进<br/>
- **协议（Protocols）**<br/>
  在书写协议的时候注意用 `<>` 括起来的协议和类型名之间是没有空格的，比如 `IPCConnectHandler()<IPCPreconnectorDelegate>`<br/>
  这个规则适用所有书写协议的地方，包括函数声明、类声明、实例变量等等<br/>
- **闭包（Blocks）**<br/>
  根据block的长度，有不同的书写规则：
  - 较短的block可以写在一行内
  - 如果分行显示的话，block的右括号`}`应该和调用block那行代码的第一个非空字符对齐
  - block内的代码采用 4个空格的缩进
  - 如果block过于庞大，应该单独声明成一个变量来使用
  - `^`和`(`之间，`^`和`{`之间都没有空格，参数列表的右括号`)`和`{`之间有一个空格
- **数据结构的语法糖**<br/>
  - 应该使用可读性更好的语法糖来构造 `NSArray`，`NSDictionary`等数据结构，避免使用冗长的`alloc`，`init`方法<br/>
  - 如果构造代码写在一行，需要在括号两端留有一个空格，使得被构造的元素与构造语法区分开来<br/>
  - 如果构造代码不写在一行内，构造元素需要使用 两个空格 来进行缩进，右括号`]`或者`}`写在新的一行，并且与调用语法糖那行代码的第一个非空字符对齐<br/>
  - 构造字典时，字典的Key和Value与中间的冒号`:`都要留有一个空格，多行书写时，也可以将Value对齐

***  
###**命名规范**

- **基本原则**<br/>
	- 清晰<br/>
		- 命名应该尽可能的清晰和简洁，但在Objective-C中，清晰比简洁更重要。由于Xcode有强大的自动补全功能，不用担心名称过长问题。<br/>
		- 不要使用单词的简写，拼写出完整的单词
		- 然而，有部分单词简写在Objective-C编码过程中是非常常用的，以致成为一种规范，这些可以直接使用，如 
		
				alloc = Allocate      alt = Alternate      msg = Message
				calc  = Calculate     nib = Interface Builder archive
				func  = Function      rect= Rectangle      vert= Vertical   
		- 命名方法或者函数是要避免歧义
   - 一致性<br/>
   	   整个工程的命名风格要保持一致性，最好和苹果SDK的代码保持统一<br/>
   	   不用类中完成相似功能的方法应该叫一样的名字，比如我们总是使用count来返回集合元素的个数，不能再A类中使用count而在B类中使用getNumber
- **使用前缀**<br/>
	如果代码需要打包成Framework给别的工程使用，或者工程项目非常庞大，需要拆分成不同的模块，使用命名前缀是非常有用的
	- 前缀由大写字母缩写而成，比如`Cocoa`中`NS`前缀代表`Foundation`框架中的类，`IB`则代表`Interface Builder`框架
	- 可以在为类、协议、函数、常量以及`typedef`宏命名的时候使用前缀，但注意不要为成员变量或者方法使用前缀，因为他们本身就包含在类的命名空间内
	- 命名前缀的时候不要和苹果SDK框架冲突

- **命名类和协议（Classes&Protocol）**<br/>
	- 类名以大写字母开头，应该包含一个*名词*来表示它代表的对象类型，同时可以加上必要的前缀，比如`NSString`，`NSData`，`NSScanner`，`NSApplication`等等
	- 而协议名称应该清晰地表示它所执行的行为，而且要和类名区分开，所以通常使用`ing`词尾来命名一个协议，比如`NSCoping`，`NSLocking`
	- 有些协议本身包含了很多不相关的功能，主要用来为某一特定类服务，这时候可以直接用类名来命名这个协议，比如`NSObject`协议，它包含了`id`对象在生存周期内的一系列方法

- **命名头文件（Headers）**<br/>
	源码的头文件名应该清晰地暗示它的功能和包含的内容：
	- 如果头文件内只定义了单个类或者协议，直接用类名或者协议名来命名头文件，比如`NSLocal.h`定义了`NSLocale`类
	- 如果头文件内定义了一系列的类、协议、类别，使用其中最主要的类名来命名头文件，比如`NSString.h`来定义`NSString`和`NSMutableString`
	- 每一个`Framework`都应该有一个和框架同名的头文件，包含了框架中所有公共类头文件的引用，如`Foundation.h`
	- `Framework`中有时候会实现在别的框架中类别扩展，这样的文件通常使用`被扩展的框架名` + `Additions`的方式来命名，比如`NSBundleAdditions.h`

- **命名方法（Methods）**<br/>
	Objective-C的方法名通常都比较长，是为了让程序有更好的可读性。
	- 方法一般以小写字母大头，每一个后续的单词首字母大写，方法名中不应该有标点符号（包括下划线），有两个例外<br/>
    	- 可以用一些通用的大写字母缩写打头方法，如 PDF，TIFF等<br/>
    	- 可以用带下划线的前缀来明明私有方法或者类别中的方法
	- 如果方法表示让对象执行一个动作，使用动词打头来命名，注意不要使用 do，does 这种多余的关键字，动词本身的暗示就足够了
	- 如果方法是为了获取对象的一个属性值，直接用属性名称来命名这个方法，注意不要添加 get 或者其他的动词前缀
	- 对于有多个参数的方法，务必在每一个参数前都添加关键词，关键词应当清晰说明参数的作用
	- 不要用 and 来连接两个参数，通常 and 用来表示方法执行了两个相对独立的操作 *（从设计上来说，这时候应该拆分成两个独立的方法）*
	- 方法的参数命名也有一些注意的地方：
   		- 和方法名类似，参数的第一个字母小写，后面的每一个单词首字母大写
    	- 不要在方法名中使用类似 pointer，ptr 这样的字眼表示指针，参数本身的类型足以说明
    	- 不要使用只有一两个字母的参数名
    	- 不要使用简写，拼出完整的单词

- **存取方法（Accessor Methods）**<br/>
	存取方法是指用来获取和设置类属性的方法，属性的不同类型，对应着不同的存取方法规范<br/>
	
		//属性是一个名词时
		- (type)noun;
		- (void)setNoun:(type)aNoun;
		//栗子
		- (NSString *)title;
		- (void)setTitle:(NSString *)aTitle;
		
		//属性是一个形容词时
		- (BOOL)isAdjective;
		- (void)setAdjective:(BOOL)flag;
		//栗子
		- (BOOL)isEditable;
		- (void)setEditable:(BOOL)flag;
		
		//属性是一个动词时
		- (BOOL)verbObject;
		- (void)setVerbObject:(BOOL)flag;
		//栗子
		- (BOOL)showsAlpha;
		- (void)setShowsAlpha:(BOOL)flag;
	
	命名存取方法时不要将动词转化为被动形式来使用
	<br/>
	可以使用can, should, will 等词来协助表达存取方法的意思，但不要使用do和does<br/>
	为什么Objective-C中不适用get前缀来表示属性获取方法？因为get在Objective-C中通常指用来表示从函数指针返回值的函数

- **命名委托(Delegate)**<br/>



***
Reference:<br/>
https://github.com/QianKaiLu/Objective-C-Coding-Guidelines-In-Chinese