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
在分行时，如果第一段名称过短，后续名称可以以 Tab 的长度（4个空格）为单位进行缩进。<br/>
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
	当特定的事件发生时，对象会触发它注册的委托方法。
	委托是Objective-C中常用的传递消息的方式。
	委托有它固定的命名范式。<br/>
	一个委托方法的第一个参数是触发它的对象，第一个关键词是触发对象的类名，除非委托方法只有一个名为`sender`的参数。<br/>
	根据委托方法触发的时机和目的，使用`should`，`will`，`did`等关键词。
	
- **集合操作类方法（Collection Methods）**<br/>
	有些对象管理着一系列其他对象或者元素的集合，需要使用类似“增删查改”的方法来对集合进行操作，这些方法的命名范式一般为：
		
		//集合操作范式
		- (void)addElement:(elementType)anObj;
		- (void)removeElement:(elementType)anObj;
		- (NSArray *)elements;
		
		//栗子
		- (void)addLayoutManager:(NSLayoutManager *)obj;
		- (void)removeLayoutManager:(NSLayoutManager *)obj;
		- (NSArray *)layoutManagers;
	注意，如果返回的集合是无序的，使用`NSSet`来代替`NSArray`。<br/>
	如果需要将元素插入到特定的位置，使用类似于这样的命名：
		
		- (void)insertLayoutManager:(NSLayoutManager *)obj atIndex:(int) index;
		- (void)removeLayoutManagerAtIndex:(int)index;
	如果管理的集合元素中有指向管理对象的指针，要设置成`weak`类型以防止循环引用。
	
- **命名函数（Functions）**<br/>
	函数的命名和方法有一些不同，主要是：
	- 函数名称一般带有缩写前缀，表示方法所在的框架
	- 前缀后的单词以“驼峰”表示法显示，第一个单词首字母大写
	
	函数名的第一个单词通常是一个动词，表示方法执行的操作，如`NSDealocateObject`<br/>
	如果函数返回其参数的某个属性，省略动词，如`float NSHeight(NSRect aRect)`<br/>
	如果函数通过指针参数来返回值，需要在函数名中使用`Get`，如`const char *NSGetSizeAndAlignment(...)`<br/>
	函数的返回类型是BOOL时的命名，`BOOL NSDecimalIsNotANumber(const NSDecimal *decimal)`

- **命名属性和实例变量（properties&Instance variables）**<br/>
	属性和对象的存取方法相关联，属性的第一个字母小写，后续单词首字母大写，不必添加前缀。<br/>
	属性按功能命名成名词或者动词
		
		//名词属性
		@property (strong) NSString *title;
		//动词属性
		@property (assign) BOOL showsAlpha;
	
	属性也可以命名成形容词，这时候通常会指定一个带有is前缀的get方法来提高可读性
		
		@property (assign, getter=isEditable) BOOL editable;
		
	命名实例变量，在变量前加上 `—` 前缀，其他和命名属性一样
	
		@implement MyClass {
			BOOL _showsTitle;
		}
	
	一般来说，类需要对使用者隐藏数据存储的细节，所以不要将实例方法定义成公共可访问的接口，可以使用 `@private`，`@protected`前缀<br/>
	*按照苹果的说法，不建议在除了init和dealloc方法以外的地方直接访问实例变量，但很多人认为直接访问会让代码更加清晰可读，只在需要计算或者执行操作的时候才使用存取方法访问*<br/>
	
- **命名常量（Constants）**<br/>
	如果要定义一组相关的常量，尽量使用枚举类型（enumerations），枚举类型的命名规则和函数命名规则相同。建议使用NS_ENUM和NS_OPTIONS宏来定义枚举类型。
		
		//定义一个枚举
		typedef NS_ENUM(NSInteger, NSMatrixMode) {
			NSRadioModeMatrix,
			NSHighlightModeMatrix,
			NSListModeMatrix,
			NSTrackModeMatrix
		};
	定义bit map
		
		typedef NS_OPTIONS(NSUInteger, NSWindowMask) {
			NSBorderlessWindowMask      = 0,
			NSTitleWindowMask           = 1 << 0,
			NSClosableWindowMask        = 1 << 1,
			NSMiniaturizableWindowMask  = 1 << 2,
			NSResizableWindowMask       = 1 << 3
		};
	使用 const 定义浮点型或者单个的整数型常量，如果要定义一组相关的整数常量，应该优先使用美剧。常量的命名规范和函数相同
		
		const float NSLightGray;
	不要使用#define宏来定义常量，如果是整型变量，尽量使用美剧，浮点型常量，使用const定义。#define通常用来给编译器巨鼎是否编译某块代码，如常用的
		
		#ifdef DEBUG
		
	注意到一般由编译器定义的宏会在前后都有一个 `__` , 比如 `__MACH__`
	
- **命名通知（Notifications）**<br/>
	通知常用语在模块间传递消息，所以通知要尽可能地表示出放生的事件，通知的命名范式是
		
		[触发通知的类名] + [Did | Will] + [动作] + Notification
		//栗子
		NSApplicaitonDidBecomActiveNotification
		NSWindowDidMiniaturizaNotificaiton
		NSTextViewDidChangeSelectionNotification

***
##**注释**

好的注释不仅能让人轻松读懂你的程序，还能提升代码逼格<br/>

- **文件注释**<br/>
	每一个文件都必须写文件注释，文件注释通常包含
	- 文件所在模块
	- 作者信息
	- 历史版本信息
	- 版权信息
	- 文件包含的内容，作用
	文件注释的格式通常不作要求，能清晰易读就可以了，但在整个工程中峰峰要统一。

- **代码注释**<br/>
	好的代码应该是“自解释”（self-documenting）的，但仍然需要详细的注释来说明参数的意义，返回值，功能以及可能的副作用。<br/>
	方法，函数，类，协议，类别的定义都需要注释，推荐采用Apple的标准注释风格，好处是可以在引用的地方`alt+点击`自动弹出注释，很方便<br/>
	有很多可以自动生成注释格式的插件，推荐使用VVDocument
	如果在注释中要引用参数名或者方法函数名，使用`||`将参数或者方法括起来以避免歧义
		
		// Remember to call |StringWithoutSpaces("foo bar bax")|
	
	定义在头文件里的接口方法，属性必须有注释

***
##**编码风格**
每个人都有自己的编码风格，这里总结一些比较好的Cocoa编程风格和注意点<br/>
- **不要使用new方法**<br/>
	尽管很多时候能用`new`代替`alloc init`方法，但这可能会导致调试内存时出现不可预料的问题

- **Public API要尽量简洁**<br/>
	共有接口要设计的简洁，满足核心的功能需求就可以了。不要设计很少会被用到，但是参数及其复杂的API。如果要定义复杂的方法，使用类别或者类扩展。
	
- **#import和#include**<br/>	
	`#mport`是Cocoa中常用的引用头文件的方式，它能自耦东防止重复引用文件
	- 当引用的是一个Objective-C或者Objective-C++的头文件时，使用#import
	- 当引用的是一个C或者C++的头文件是，使用#include，这是必须要保证被引用的文件提供了保护域（#define guard）
			
			//栗子
			#import <Cocoa/Cocoa.h>
			#include <CoreFoundation/CoreFoundation.h>
			#import "GTMFoo.h"
			#include "base/basictypes.h"
	为什么不全使用#import？主要是是为了保证代码在不同平台间共享时不出现问题
	
- **引用框架的根头文件**<br/>
	每一个框架都会有一个和框架同名的头文件，它包含了框架内接口的所有引用，在使用框架的时候，应该直接引用这个根头文件，而不是其他子模块的头文件，即使是只用到了其中一小部分，编译器会自动完成优化
		
		//正确，引用根头文件
		#import <Foundation/Foundation.h>
		
		//错误，不要单独引用框架内其他的头文件
		#import <Foundation/NSArray.h>
		
- **BOOL的使用**<br/>
	BOOL在Objective-C中被定义成`signed char`类型，意味着一个BOOL类型的变量不仅仅可以表示YES（1）和NO（0）两个值，所以永远不要讲BOOL类型变量直接和YES比较
		
		//错误，无法确定|great|的值是否是YES（1），不要将BOOL值直接与YES比较
		BOOL great = [foo isGreat];
		if (great == YES)
			//...
		
		//正确
		BOOL great = [foo isGreat];
		if (great)
			//...
		
	同样，也不要将其他类型的值作为BOOL来返回，这种情况下，BOOL变量只会取值的最后一个字节来赋值，这样很可能会取到0（NO）。但是，一些逻辑操作符如&&，||，！的返回值是可以直接赋给BOOL的<br/>
	另外，BOOL类型可以和_Bool,bool相互转化，但是不能喝Boolean转化。
	
- **使用ARC**<br/>
	除非想要兼容一些古董级的机器和操作系统，没有理由放弃使用ARC
	
- **在init和dealloc中不要使用存取方法访问实例变量**<br/>
	当init和dealloc方法被执行时，类的运行时环境不是处于正常状态，使用存取方法访问变量可能会导致不可预测的结果，因此应当在这两个方法内直接访问实例变量 `_xxxx`

- **按照定义的顺序释放资源**<br/>
	在类或者Controller的生命周期结束时，往往需要做一些扫尾工作，比如释放资源，停止线程等，这些扫尾工作的释放顺序应当与它们的初始化或者定义的顺序保持一致。这样方便调试时寻找错误，也能防止遗漏。

- **保证NSString在赋值时被复制**<br/>
	NSString非常常用，在它被传递或者赋值时应当保证是以复制（copy）的方式进行的，这样可以防止在不知情的情况下String的值被其他对象修改。
		
		- (void)setFoo:(NSString *)aFoo {
			_foo = [aFoo copy];
		}
		
- **使用NSNumber的语法糖**<br/>
	使用带有@符号的语法糖来生成NSNumber对象能使代码更简洁
			
			NSNumber *fortyTwo = @42;
			NSNumber *piOverTow = @(M_PI / 2);
			enum {
				kMyEnum = 2;
			};
			NSNumber *myEnum = @(kMyEnum);
- **nil检查**<br/>		
	因为在Objective-C中向nil对象发送命令是不会抛出异常或者导致崩溃的，只是完全的“什么都不干”，所以，只在程序中使用nil来做逻辑上的检查。<br/>
	另外，不要使用诸如 `nil == Object`或者`Object == nil`的形式来判断
		
		//正确，直接判断
		if (!objc) {
			...
		} 
		
		//错误，不要使用 nil == Object 的形式
		if (nil == objc) {
			...
		}
	
- **属性的线程安全**<br/>
	定义一个属性时，编译器会自动生成线程安全的存取方法（Atomic），但这样会大大降低性能，特别是对于那些需要频繁存取的属性来说，是极大的浪费。所以，如果定义的属性不需要线程保护，记得手动添加属性关键字nonatomic来取消编译器的优化。

- **点分语法的使用**<br/>
	不要用点分语法来调用方法，只用来访问属性。这样是为了防止代码可读性的问题。
	
- **Delegate要使用弱引用**<br/>
	一个类的Delegate对象通常还引用这类本身，这样容易造成循环引用的问题，所以类的Delegate属性要设置为弱引用。
		
		/** delegate **/
		@property (nonatomic, weak) id <IPConnectHandlerDelegate> delegate;
		

***
Reference:<br/>
https://github.com/QianKaiLu/Objective-C-Coding-Guidelines-In-Chinese