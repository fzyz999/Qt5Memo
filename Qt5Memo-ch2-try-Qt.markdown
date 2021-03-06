﻿# Qt学习备忘录2：初试Qt#
　　在对Qt做完了基本的介绍之后（其实，你认为这是布道我也没太大意见），我们开始尝试一下Qt。先配置一下Qt的环境，然后用Qt写个简单的程序（对，你猜对了，就是hello，world）。感受一下用Qt写程序的感觉。

## 如何安装Qt开发环境 ##
　　本节将会说明如何安装Qt的开发环境，包括编译安装和二进制包安装。

### 二进制包安装 ###
#### Linux（Ubuntu） ####
　　在Linux环境下安装Qt，这里以Ubuntu Linux为例进行说明（下面简称Ubuntu），Ubuntu环境下有两种不同的选择。

+  选择发行版打包好的Qt

	Ubuntu发行版的软件仓库中有一个打包好的Qt。你可以直接打开Ubuntu软件中心，然后搜索Qt Creator，点击安装。也可以使用命令行：
        
        sudo apt-get install qtcreator qt5-default
	
	安装Qt Creator及开发Qt5必要的环境
	
+  从官方网站下载最新的二进制包

	发行版中包含的Qt版本很可能不是最新的Qt版本，或者版本无法使你满意。这种情况下，你可以选择从官网下载Qt。官网下载地址为：http://qt-project.org/downloads
	
	根据你的具体情况下载Qt (版本号) for Linux 32-bit(或64-bit)。然后打开命令行，到你下载下的安装包所在的目录下，执行：
	
        chmod +x ./qt-linux-opensource-(你所下载的Qt的版本号)-x86_64-offline.run
        
	这条命令使我们下载的.run文件具有可执行权限。然后执行：
	
        sudo ./qt-linux-opensource-(你所下载的Qt的版本号)-x86_64-offline.run
        
	使用root权限安装Qt。之后会打开一个和Microsoft Windows下的安装程序相同的界面，一直点下一步即可，相信各位都没问题。
	
	（注意：笔者不使用sudo安装时会发生错误，但使用sudo后Qt Creator的配置文件的读写均需root权限，需手工修改权限为普通用户可读写，否则调Qt Creator设置的时候会报错。大牛们如果有更好的方法，还请赐教）
	
#### Windows ####

　　在Windows环境下搭建Qt环境的方法较为唯一，首先请到Qt官网获得离线安装包文件。
	
　　对于Windows环境来说，5.0.1版本共有4个安装包，其中32位的安装包有三种，所采用的编译器（库）不一样：MinGW 4.7（650 MB）、VS 2010（485 MB）和VS 2010, OpenGL（476 MB）。而64位的安装包只有VS 2012（500 MB）一种。这里，笔者以在Windows 7 SP1的硬件环境下安装32位的VS 2010 OpenGL为例。
	
　　下载完毕后运行安装包，在选择安装位置时注意路径不可以包含空格还有很多Windows中允许却不可使用的字符。依次进行组件选择、协议阅读和开始菜单文件夹的建立后，单击Install即可开始安装。安装时间较长，在17%左右会停滞相当长的时间。带安装完毕后，Qt也就配置好了。

　　这里需要指出的是：如果机器上没有安装VS 2010的话，使用VS2010版本的Qt会提示找不到编译器的错误。再次笔者建议怕麻烦的用户使用MinGW版。（最后笔者其实也换成了MinGW版）

### 编译安装Qt5 ####
#### Ubuntu ####
首先，下载源代码，编译，安装。
FIXME：没实际经验，先不写了。

#### windows ####
FIXME：没实际经验，暂不写。

至此，Qt的开发环境就配置完毕了。接下来，我们开始完成一个简单的Hello World程序。

## Hello World ！ ##
### 敲出第一段Qt代码 ###
步骤：

1. 「文件」->「新建文件或项目」
2. 选择「其他项目」中的「空Qt项目」
3. 跟着Qt Creator的引导创建项目，我们的项目名就叫Hello World吧。

创建好项目后，首先在HelloWorld.pro中输入：

        QT += core gui widgets
        
		SOURCES += \
		    main.cpp
			
这段是用来描述我们的项目都需要包含什么内容的。qmake用它生成makefile（具体内容以后讨论）。我们需要使用Qt的core、gui和widgets三个模块，所以我们把它加入到项目中(`QT += core gui widgets`)。source指我们项目所包含的文件。这行不用自己敲，直接在Qt Creator->「项目」->「Hello World」上右键，然后添加main.cpp即可自动产生这行代码。

在main.cpp中输入以下代码：

```cpp
#include <QApplication>
#include <QLabel>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    
    QLabel label("Hello World!");
    label.show();
    
    return a.exec();
}
```
		
　　编译，运行。然后一个Hello world程序就完成了，很简单，不是吗？

　　接下来，我们来具体分析一下这个Hello World程序。头两行引用头文件，这个自然不用多说。然后先实例化一个QApplication并将argc和argv参数传入。这里，argc和argv代表的含义是命令行参数的个数和每个参数指向的指针，这点与大多数C++中的main函数声明是一致的。显然，我们大致猜到，QApplication类是用于处理Qt应用程序中最基本的功能的。比如处理外部传入的参数等等，是Qt应用程序中最基本的类。QLabel顾名思义，是一个标签控件，用于显示文字。那么，向构造函数传入的字符串，自然就是该标签的内容了。下一行`label.show()`的意思应该是显示该控件。最后，返回程序运行结果。

　　通过刚刚的目测，我们已经大致猜测了整段程序的含义，接下来，我们来细细地查阅Qt5的文档，来验证一下我们的想法。

### 查阅梳理Qt文档 ###

#### QApplication类 ####
　　QApplication继承了QGuiApplication类，而QGuiApplication继承了QCoreApplication类，这是Qt程序的核心。

　　QApplication类管理着GUI程序的控制流和主要设置。QApplication继承了QGuiApplication类，并实现了一些基于QWidget的应用程序需要的功能。它负责处理关于widget的一些特定的初始化和终止化操作。注意！无论你的程序有几个窗口，你只能有 **一个** QAppliaction对象。对于那些不基于QWidget的Qt程序，你应当使用QGuiApplication代替QApplication，因为它不必依赖于QtWidgets库。

　　一些GUI程序会提供批处理模式（从命令行输入一些参数，然后自动执行任务而不需要人工操作）。在这类非GUI的模式下，初始化一些和图形用户界面相关的资源就显得有些浪费了。为了避免这个问题，Qt提供了QCoreApplication。下面这个例子（源自Qt5Doc）展示了如何动态选择创建QCoreApplication还是QApplication。

```cpp
        QCoreApplication* createApplication(int &argc, char *argv[])
        {
            for (int i = 1; i < argc; ++i)
                if (!qstrcmp(argv[i], "-no-gui"))
                    return new QCoreApplication(argc, argv);
            return new QApplication(argc, argv);
        }
       
        int main(int argc, char* argv[])
        {
            QScopedPointer<QCoreApplication> app(createApplication(argc, argv));

            if (qobject_cast<QApplication *>(app.data())) {
               // 启动GUI版本...
            } else {
               // 启动非GUI版本...
            }
            
            return app->exec();
        }
```
		
　　这段代码运行时，首先检查是否传入-no-gui。如果传入了，则实例化QCoreApplication类，否则就创建QApplication类。QScopedPointer是Qt提供的智能指针类之一，可以自动进行回收内存等等工作，目前我们先不理它，把它当作普通指针就可以。`qobject_cast`可以将父类指针转化为子类指针，如果强制转型成功则返回子类指针。失败则返回NULL。通过判断其返回值，我们就可以知道我们创建的是否是QApplication类，从而启动对应的处理。这样就实现了动态选择创建QApplication或QCoreApplication的功能。

　　QApplication对象可以通过`instance()`函数访问。这是一个静态程序函数，返回QApplication对象的指针，这个指针和全局的qApp指针是相同的。函数原型及功能如下：

```cpp
        QCoreApplication * QCoreApplication::instance() [static]
	　　//返回指向程序的QCoreApplication（或QApplication）对象的指针
		//如果没有QCoreApplication（或QApplication）被创建，则会返回NULL
```

　　QApplication主要负责这样一些工作：

+  它使用用户的桌面设置初始化应用程序，例如`palette()`, `font()` 和`doubleClickInterval()`。它会对这些属性进行跟踪，以应对用户修改桌面的全局设置（changes the desktop globally）。例如用户通过某种控制面板一类的东西修改某些设置。

+  它负责处理事件，也就是说，他从窗口系统接受事件消息并把他们分发到相应的部件。你可以通过调用`sendEvent()`和`postEvent()`发送你自己的事件给一些窗口部件(widgets)。

+  它解析命令行参数并且根据它们设置内部状态。所有的Qt应用程序都自动支持如下命令行选项：

    `-style= style`，设置程序的图形用户界面风格。可行的值取决于系统设定。如果你使用其它风格编译Qt或者有其他的风格插件，那么它们都可以被用作-style命令行选项。你可以通过设置QT_STYLE_OVERRIDE环境变量来为所有的Qt程序设置风格。
    
    `-style style` 和上面的一项功能相同。
    
    `-stylesheet= stylesheet`，设置应用程序的样式表。这个值必须是一个包含样式表的文件的路径。
    
    注意：样式表文件中的相对路径是相对于当前项目表文件的路径的。
    
    `-stylesheet stylesheet`，和上面的一项相同。
    
    `-widgetcount`，程序最后会输出调试信息，包括未销毁的部件数和同时存在的部件数的最大值。
    
    `-reverse`，将应用程序的层（layout）的方向设为Qt::RightToLeft
    
    `-qmljsdebugger=`，有具体端口的活动的QML/JS调试器。这个值必须是这样的格式`端口:1234[,block]`， 其中block是可选的。这个参数会使程序等待直到一个调试器连接它。
		
+  它定义由QStyle对象封装的程序的外观。这个可以在运行时使用`setStyle()`修改。

+  它确定程序如何分配颜色。可以使用`setColorSpec()`函数设置。

    `void QApplication::setColorSpec(int spec) [static]`
	
	设置颜色规格（color specification）为spec。
	
	颜色规格控制当运行在有限的颜色数之下（例如：8位/256色）时，程序如何分配颜色。这个参数必须在你创建QApplication对象前设定好。
	
	更具体的内容可以参考Qt5的文档。

+  它提供通过`translate()`函数可见的本地化字符串。

+  它提供一些魔法般的对象，如`desktop()`和`clipboard()`。

+  它了解应用程序的所有窗口。你可以使用`widgetAt()`知道哪个窗口部件在当前位置上，通过`topLevelWidgets()`获得顶级窗口部件（或窗口）列表，以及调用`closeAllWindows()`关闭所有窗口等等。

+  它管理应用程序的鼠标光标处理，具体请参考`setOverrideCursor()`


*关于qApp宏*：qApp是一个全局指针，指向唯一的应用程序中的对象。它等于`QCoreApplication::instance()`函数返回的指针。在GUI程序里，它指向一个QApplication实例。只有一个应用程序对象可以被创建。

#### QCoreApplication ####
　　按照官方文档的说法，QCoreApplication为非GUI的Qt应用程序提供事件循环。GUI程序需要使用QApplication。但由于QCoreApplication是QApplication的基类，且它们都提供事件循环功能，所以，我们可以猜测大部分的事件循环都是在QCoreApplication中实现的。

　　QCoreApplication包含了主事件循环。源自操作系统的事件(例如：定时器和网络事件等等)和其他的一些资源的处理和分派都是在这里实现的。它同时处理应用程序的初始化和终止化，以及系统层面和程序层面上的设置。

+  关于事件循环

	如果你没有写过GUI程序，你可能会困惑事件循环是个什么东西，为什么我们要如此关注它。甚至不惜笔墨和精力，来大段地翻译QCoreApplication的类以搞清Qt中和事件循环相关的一些内容。
	
	事件循环是程序，特别是GUI程序在现代操作系统上运行所不可或缺的一部分。如你所见，现代操作系统都是多任务的。多个程序在同时运行。以windows为例，你在操作电脑的时候很可能会同时打开多个GUI程序。它们都有图形界面，且有些部分甚至相互重叠、相互覆盖。那么它们怎么知道用户是在向它输入消息，还是在向它附近的某个窗口输入呢？这个工作就由操作系统来完成。操作系统进行一系列的判断，确定用户到底向哪个程序输入，然后通知对应的程序。
	
	那么windows怎样通知具体的应用程序呢？首先，windows为每一个应用程序维护一个队列，每发生一个事件（例如用户按了哪个键，点了程序窗口的哪个位置，窗口尺寸改变了等等），windows就会把这个消息插入到对应程序的队列中。例如你点击了Firefox窗口中的某个按钮，windows就会向Firefox的队列中插入一个消息，通知它用户点击了它的某个位置。由于只向Firefox队列中插入消息，所以旁边的记事本就不必为Firefox所关心的事件头疼，更不会莫名其妙地响应用户对Firefox的操作了。这个队列就叫消息队列。
	
	那么，作为应用程序，如果想要知道用户输入了什么，只需要不断读取自己的消息队列就可以了。读取，处理，读取，处理，遵循这样的步骤，应用程序就可以和用户不断交互，直到接到要求它关闭的消息。这个不断读取消息，处理事件，然后再读取，再处理的循环就叫事件循环。
	
	程序的事件循环的伪代码如下：

```cpp	
        int main()
		{
			while(getEvent() && event!=QUIT) //不断获取事件列表中的事件，直到收到用户要求退出的消息。
			{
				switch(event)
				{
					case MOUSE_EVENT:
						//处理鼠标输入
					    break;
					
					case KEYBOARD_EVENT:
						//处理键盘输入
						break;
					
					case PAINT_EVENT:
						//需要重绘（比如之前被别的窗口覆盖了一部分，这部分的内容需要重新绘制）
						//绘制Hello World什么的，是吧:)
						break;
					
					default:
						//不关心的事件，直接无视掉。
				}
			}
		}
```
　　可以说，事件循环是运行在多任务系统上的GUI程序的一个主线。所有的交互和响应基本都是在事件循环中完成的。但直接用面向过程的方式使用事件循环十分不方便，不便于思考程序的逻辑。所以，为了便于开发，多数的图形用户界面框架都对事件循环进行了封装。用各种不同的方法在框架内部分发处理消息。但无论怎样封装，只要把握主事件循环这条主线，很多图形用户界面框架就都很好上手了。例如：MFC的消息映射表、VB中的什么Click啊mouse move什么的等等。毕竟，只要知道怎样处理事件，我们就知道怎样与用户交互了。知道怎样与用户交互，其他的问题其实都好解决。
	
　　在Qt中，事件循环通过调用`exec()`来启动。如果你要执行一个事件较长的操作，你可以调用`processEvent()`来保持程序对新事件及时地响应（执行耗时较长的任务时，如果新事件一直被操作系统存放在队列中，而没有程序被及时地读取、处理。程序就会看上去像在埋头干自己的活而不理用户一样。显然这会严重影响用户体验）。

　　通常，Qt推荐你在main()函数中创建QCoreApplication或QApplication对象，而且越早越好（毕竟，这是Qt程序的核心嘛）。然后通过调用`exec()`函数进入事件循环。这个函数直到事件循环结束时才会返回（比如`quit()`函数被调用的时候）。

　　QCoreApplication中还提供了一些方便的静态成员函数。事件可以使用`sendEvent()`,`postEvent()`和`sendPostedEvents()`来发送。它还提供了`quit()`信号和`aboutToQuit()`槽（关于信号和槽的内容我们会在以后探究）。

　　QCoreApplication还提供这样几个功能：

+  应用程序和库的路径

	可以通过`applicationDirPath()`获取程序所在目录的路径，通过`applicationFilePath()`获取程序本身的路径。库的路径可以通过`libraryPaths()`获取，并通过`setLibraryPaths()`，`addLibraryPath()`和`removeLibraryPath()`函数进行操作。
	
+  国际化和翻译

	可以通过`installTranslator()`和`removeTranslator()`来添加或删除翻译文件。更多关于国际化的内容我们或者以后进行更深入的学习。

+  访问命令行参数

	可以通过`arguments()`函数访问被QCoreApplication处理过的命令行参数。
	
+  地域设置(Locale Settings)

	在Unix/Linux上Qt默认使用系统默认的本地化设置。当然，这可能会导致一些冲突.例如：当将浮点数和字符串相互转换时，可能遇到各地的记法不同的问题。你可以通过在初始化QApplication或QCoreApplication后调用POSIX函数setlocale(LC_NUMERIC,"C")来重置地域的方法来解决这个问题。
	
### 深入解读Hello World ###
　　之前我们只对我们的Hello World做了目测式的简单理解，在详细查过Qt的文档后，我们重新解读一下这段代码。

```cpp
        #include <QApplication>
        #include <QLabel>
        
        int main(int argc, char *argv[])
        {
			//这里可以添加一些初始化代码
			//用于进行所有应该在QApplication创建之前进行的初始化
			//比如：使用`setColorSpec()`分配颜色之类的
			
            QApplication a(argc, argv);
			
			//在QApplication初始化后，所有基于QWidget的类才能正常使用
			//例如下面的QLabel什么的。
 
            QLabel label("Hello World!");
            label.show();
    
            return a.exec();
        }
```

　　程序头两行引用了QApplication类和QLabel类的头文件。

　　接着，在main函数中，我们首先创建了一个QApplication实例，并将参数传入。按照Qt文档的建议，QApplication越早创建越好，所以习惯上，在main函数中进行完必要的初始化之后，就应该立刻创建QApplication(或QCoreApplication，其区别我们在上面已经解读过了)。

　　接下来，我们就该创建Hello World程序的核心——一个标有Hello World字样的标签了。通过将Hello World字符串传入QLabel的构造函数，我们很轻松地实现了这个功能。当然，光这样是不行的，因为一般情况下基于QWidget的类（QLabel什么的）不会自动显示，需要我们手工调用`show()`函数让它显示出来（其实也有不少情况会自动显示，比如插入到QTabWidget中的QWidget，这个我们后面再慢慢学）。

　　最后，也是最重要的一步，进入事件循环。这样，我们的使用Qt的GUI程序Hello World就算正式跑起来了。麻雀虽小，五脏俱全。可以庆祝一下胜利了。

### 层层深入——跟着Hello World看看Qt内部 ###
　　在高呼完Hello World!之后，我们或许又会产生新的问题，Qt的内部又是什么样子呢？众所周知，开放源代码的一大好处就是可以给那些渴望学习技术的人学习技术的机会。既然有这机会一窥这样一个成熟的框架，我们何不去试试呢？

　　好，说干就干。可是Qt这样大的库，我们从哪里入手去看它的内部情况呢？其实，踏破铁鞋无觅处，得来全不费功夫。我们想知道Qt一步一步都做了什么，跟着我们刚刚的Hello World一步一步走不久可以了？调用了什么函数，执行了哪些操作，岂不一目了然？那我们现在就开始。

　　（ *我们使用Qt 5.0.2版本的源代码。为使逻辑更清晰，无关的代码我们都直接忽略* ）
　　首先，执行的是`QApplication a(argc, argv);`这是Hello World的起点，也是我们这趟深入Qt之旅的起点。

```cpp
        //qtbase/src/widgets/kernel/qapplication.h
		class Q_WIDGETS_EXPORT QApplication : public QGuiApplication
		{
			//...
			
		public:
			QApplication(int &argc, char **argv, int = ApplicationFlags);
			//从这里继续深入
			virtual ~QApplication();
			
			//...
		}
        
        //qtbase/src/widgets/kernel/qapplication.cpp
		QApplication::QApplication(int &argc, char **argv, int _internal)
			: QGuiApplication(*new QApplicationPrivate(argc, argv, _internal))
        {
			QApplicationPrivate * const d = d_func()
			d->construct();
		}
        //...
```

然后转入QApplication的父类QGuiApplication的构造函数
```cpp

		//qtbase/src/gui/kernel/qguiapplication.h
		//...
		class Q_GUI_EXPORT QGuiApplication : public QCoreApplication
        {	
        public:
			QGuiApplication(int &argc, char **argv, int = ApplicationFlags);
			//从这里继续
            virtual ~QGuiApplication();
			
			//...
        }

        //qtbase/src/gui/kernel/qguiapplication.cpp
		QGuiApplication::QGuiApplication(int &argc, char **argv, int flags)
            : QCoreApplication(*new QGuiApplicationPrivate(argc, argv, flags))
        {
			d_func()->init();
			
			QCoreApplicationPrivate::eventDispatcher->startingUp();
		}
```

和QApplication的定义相当相似，我们继续进入到QCoreApplication中一探究竟。

```cpp
        //qtbase/src/corelib/kernel/qcoreaapplication.h
		//...
		
		class Q_CORE_EXPORT QCoreApplication : public QObject
		{
			//...
			
			QCoreApplication(int &argc, char **argv, int = ApplicationFlags);
			~QCoreApplication();
			
			//...
			
		private:
			void init();
				
			//...
		}

        //qtbase/src/corelib/kernel/qcoreapplication.cpp
		//...
		
		QCoreApplication::QCoreApplication(int &argc, char **argv, int _internal)
            : QObject(*new QCoreApplicationPrivate(argc, argv, _internal))
		{
			init();
			QCoreApplicationPrivate::eventDispatcher->startingUp();
		}
```
	
终于找到有实际意义的函数`QCoreApplication::init()`了，几经辗转啊！

```cpp
        //qtbase/src/corelib/kernel/qcoreapplication.cpp
		void QCoreApplication::init()
		{
			QCoreApplicationPrivate * const d = d_func()
			
			//初始化Loacle
			QCoreApplicationPrivate::initLocale();
			
			//判断是否已经创建过一个应用程序对象
			Q_ASSERT_X(!self, "QCoreApplication", "there should be only one application object");
			QCoreApplication::self = this;
			
			//使用app程序员创建的事件分发器(如果有的话)
			if (!QCoreApplicationPrivate::eventDispatcher)
				QCoreApplicationPrivate::eventDispatcher = d->threadData->eventDispatcher;
			//如果没有，自动创建一个默认的
			if (!QCoreApplicationPrivate::eventDispatcher)
				d->createEventDispatcher();
			Q_ASSERT(QCoreApplicationPrivate::eventDispatcher != 0);
			
			if (!QCoreApplicationPrivate::eventDispatcher->parent()) {
				QCoreApplicationPrivate::eventDispatcher->moveToThread(d->threadData->thread);
				QCoreApplicationPrivate::eventDispatcher->setParent(this);
			}
			
			d->threadData->eventDispatcher = QCoreApplicationPrivate::eventDispatcher;
		
        #ifndef QT_NO_LIBRARY
		    if (coreappdata()->app_libpaths)
			    d->appendApplicationPathToLibraryPaths();
        #endif
		
        #if defined(Q_OS_UNIX) && !(defined(QT_NO_PROCESS))
			// Make sure the process manager thread object is created in the main
			// thread.
			QProcessPrivate::initializeProcessManager();
        #endif
		
        #ifdef QT_EVAL
			extern void qt_core_eval_init(uint);
			qt_core_eval_init(d->application_type);
        #endif
			
			//处理命令行参数
			d->processCommandLineArguments();
			
			//
			qt_startup_hook();
		}
		//
```


这么复杂的一堆代码我们自然不可能都看完。着重看一下我们一直关注的和事件处理相关的部分吧。我们来看看Qt是怎样创建默认的事件分发器的。透过这里的代码，我们应该可以简单地推测一下Qt是如何跨平台的。

```cpp
        //qtbase/src/corelib/kernel/qcoreapplication.cpp
		void QCoreApplicationPrivate::createEventDispatcher()
		{
			Q_Q(QCoreApplication);
			
		//为Unix类平台创建事件分发器
        #if defined(Q_OS_UNIX)
        #  if defined(Q_OS_BLACKBERRY)
			//黑莓平台
			eventDispatcher = new QEventDispatcherBlackberry(q);
        #  else
        #  if !defined(QT_NO_GLIB)
			if (qEnvironmentVariableIsEmpty("QT_NO_GLIB") && QEventDispatcherGlib::versionSupported())
				//Glib事件分发器
				eventDispatcher = new QEventDispatcherGlib(q);
			else
        #  endif
			//UNIX事件分发器
            eventDispatcher = new QEventDispatcherUNIX(q);
        #  endif
        #elif defined(Q_OS_WIN)
			//windows事件分发器
			eventDispatcher = new QEventDispatcherWin32(q);
        #else
			//没有发现该使用何种事件分发器
        #  error "QEventDispatcher not yet ported to this platform"
        #endif
		}
```
		
在这里为不同的平台选择了不同的事件分发器。看来Qt使用了QAbstractEventDispatcher这个抽象类来抽象不同平台的差异，并为不同平台实现对应的子类。我们来看看QEventDispatcherWin32类。

```cpp
	    bool QEventDispatcherWin32::processEvents(QEventLoop::ProcessEventsFlags flags)
		{
			Q_D(QEventDispatcherWin32);
			
			if (!d->internalHwnd)
				createInternalHwnd();
			
			d->interrupt = false;
			emit awake();
			
			bool canWait;
			bool retVal = false;
			bool seenWM_QT_SENDPOSTEDEVENTS = false;
			bool needWM_QT_SENDPOSTEDEVENTS = false;
			do {
				DWORD waitRet = 0;
				HANDLE pHandles[MAXIMUM_WAIT_OBJECTS - 1];
				QVarLengthArray<MSG> processedTimers;
				while (!d->interrupt) {
					DWORD nCount = d->winEventNotifierList.count();
					Q_ASSERT(nCount < MAXIMUM_WAIT_OBJECTS - 1);
				
					MSG msg;
					bool haveMessage;
					
					if (!(flags & QEventLoop::ExcludeUserInputEvents) && !d->queuedUserInputEvents.isEmpty()) {
						// 处理队列中的用户输入事件
						haveMessage = true;
						msg = d->queuedUserInputEvents.takeFirst();
					} else if(!(flags & QEventLoop::ExcludeSocketNotifiers) && !d->queuedSocketEvents.isEmpty()) {
						// 处理队列中的socket事件
						haveMessage = true;
						msg = d->queuedSocketEvents.takeFirst();
					} else {
						//调用win32api读取windows的消息队列
						haveMessage = PeekMessage(&msg, 0, 0, 0, PM_REMOVE);
						if (haveMessage && (flags & QEventLoop::ExcludeUserInputEvents)
							&& (//笔者吐槽：这对括号中间包含的东西也太多了吧，差点没区分出来
							(msg.message >= WM_KEYFIRST
							&& msg.message <= WM_KEYLAST)
							//WM_KEYFIRST和WM_KEYLAST可作为过滤值取得所有键盘消息——源自百度百科
							
							|| (msg.message >= WM_MOUSEFIRST
								&& msg.message <= WM_MOUSELAST)
								//常数WM_MOUSEFIRST和WM_MOUSELAST可用来接收所有的鼠标消息——还是来自百度百科
								
							|| msg.message == WM_MOUSEWHEEL
							|| msg.message == WM_MOUSEHWHEEL
							//鼠标滚轮消息
							
							|| msg.message == WM_TOUCH
                        #ifndef QT_NO_GESTURES
						    || msg.message == WM_GESTURE
							|| msg.message == WM_GESTURENOTIFY
                        #endif
							
							|| msg.message == WM_CLOSE)
							) {
							// 将输入的消息插入队列中
							haveMessage = false;
							d->queuedUserInputEvents.append(msg);
						}
						
						if (haveMessage && (flags & QEventLoop::ExcludeSocketNotifiers)
							&& (msg.message == WM_QT_SOCKETNOTIFIER && msg.hwnd == d->internalHwnd)) {
							//将socket事件插入队列中
							haveMessage = false;
							d->queuedSocketEvents.append(msg);
						}
					}
					if (!haveMessage) {
						// 没有消息，检查发出信号的对象
						for (int i=0; i<(int)nCount; i++)
							pHandles[i] = d->winEventNotifierList.at(i)->handle();
						waitRet = MsgWaitForMultipleObjectsEx(nCount, pHandles, 0, QS_ALLINPUT, MWMO_ALERTABLE);
						if ((haveMessage = (waitRet == WAIT_OBJECT_0 + nCount))) {
							// 一条新消息到达，处理它
						continue;
					}
				}
				if (haveMessage) {
					
					if (d->internalHwnd == msg.hwnd && msg.message == WM_QT_SENDPOSTEDEVENTS) {
						if (seenWM_QT_SENDPOSTEDEVENTS) {
							// when calling processEvents() "manually", we only want to send posted
							// events once
							needWM_QT_SENDPOSTEDEVENTS = true;
							continue;
						}
						seenWM_QT_SENDPOSTEDEVENTS = true;
					} else if (msg.message == WM_TIMER) {
						// avoid live-lock by keeping track of the timers we've already sent
						bool found = false;
						for (int i = 0; !found && i < processedTimers.count(); ++i) {
							const MSG processed = processedTimers.constData()[i];
							found = (processed.wParam == msg.wParam && processed.hwnd == msg.hwnd && processed.lParam == msg.lParam);
						}
						if (found)
							continue;
						processedTimers.append(msg);
					} else if (msg.message == WM_QUIT) {
						if (QCoreApplication::instance())
							QCoreApplication::instance()->quit();
						return false;
					}
					
					if (!filterNativeEvent(QByteArrayLiteral("windows_generic_MSG"), &msg, 0)) {
						//终于出现了，win32程序消息处理函数中的常客
						TranslateMessage(&msg);
						DispatchMessage(&msg);
					}
				} else if (waitRet - WAIT_OBJECT_0 < nCount) {
					d->activateEventNotifier(d->winEventNotifierList.at(waitRet - WAIT_OBJECT_0));
				} else {
					// 无事可做，退出
					break;
				}
				retVal = true;
			}
			
			// 仍然无事可做，等待消息或对象发出信号
			canWait = (!retVal
			&& !d->interrupt
			&& (flags & QEventLoop::WaitForMoreEvents));
			if (canWait) {
				DWORD nCount = d->winEventNotifierList.count();
				Q_ASSERT(nCount < MAXIMUM_WAIT_OBJECTS - 1);
				for (int i=0; i<(int)nCount; i++)
					pHandles[i] = d->winEventNotifierList.at(i)->handle();
					
				emit aboutToBlock();
				waitRet = MsgWaitForMultipleObjectsEx(nCount, pHandles, INFINITE, QS_ALLINPUT, MWMO_ALERTABLE | MWMO_INPUTAVAILABLE);
				emit awake();
				if (waitRet - WAIT_OBJECT_0 < nCount) {
					d->activateEventNotifier(d->winEventNotifierList.at(waitRet - WAIT_OBJECT_0));
					retVal = true;
				}
			}
			} while (canWait);
			
			if (!seenWM_QT_SENDPOSTEDEVENTS && (flags & QEventLoop::EventLoopExec) == 0) {
				// when called "manually", always send posted events
				sendPostedEvents();
			}
			
			if (needWM_QT_SENDPOSTEDEVENTS)
				PostMessage(d->internalHwnd, WM_QT_SENDPOSTEDEVENTS, 0, 0);
			
			return retVal;
		}
		//
```

长长一大片代码，虽然有些杂乱无章，不过还是看到了我们想要看的东西：`PeekMessage()`这是windows平台上读取消息用的一个函数，说明我们已经成功地找到了Qt获取并分发消息的地方。现在，让我们来简单整理一下思路。Qt程序的跨平台方案是这样的：Qt程序在编译时通过`Q_OS_UNIX`、`Q_OS_WIN32`等宏判断目标平台，然后选择创建对应的事件分发器。所有的事件分发器均基于QAbstractEventDispatcher抽象类。这样，不同平台的差异性就被抽象了出来，具体实现由QEventDispatcherWin32等等子类实现，这使得Qt的平台相关的部分与那些与平台无关的部分分离。
  
那么，最后，我们进入`exec()`函数，也即进入事件循环。具体查找Qt源代码的过程和上面相仿，我们直接给出结果，就不再赘述无关紧要的细节了。

```cpp
        //qtbase/src/corelib/kernel/qcoreapplication.cpp
        int QCoreApplication::exec()
		{
			if (!QCoreApplicationPrivate::checkInstance("exec"))
				return -1;
			
			QThreadData *threadData = self->d_func()->threadData;
			if (threadData != QThreadData::current()) {
				//检查是否是从主线程调用的，如果不是，则报错
				qWarning("%s::exec: Must be called from the main thread", self->metaObject()->className());
				return -1;
			}
			if (!threadData->eventLoops.isEmpty()) {
				//检查是否已经运行事件循环了
				qWarning("QCoreApplication::exec: The event loop is already running");
				return -1;
			}
			
			threadData->quitNow = false;
			QEventLoop eventLoop;
			self->d_func()->in_exec = true;
			self->d_func()->aboutToQuitEmitted = false;
			
			//笔者吐槽：封装也太厚了，完全超乎笔者的想象啊。
			//从这里移交给QEventLoop的exec()
			int returnCode = eventLoop.exec();
			
			threadData->quitNow = false;
			if (self) {
				self->d_func()->in_exec = false;
				if (!self->d_func()->aboutToQuitEmitted)
					emit self->aboutToQuit(QPrivateSignal());
				self->d_func()->aboutToQuitEmitted = true;
				sendPostedEvents(0, QEvent::DeferredDelete);
			}
			
			return returnCode;
		}
```
		
好吧，又调用了QEventLoop的`exec()`函数，真是层层转接啊。没办法，打开eventloop的源文件看：

```cpp
        //qtbase/src/corelib/kernel/qeventloop.cpp
		int QEventLoop::exec(ProcessEventsFlags flags)
        {
            Q_D(QEventLoop);
			//这里是为了线程安全做的保护
            //we need to protect from race condition with QThread::exit
            QMutexLocker locker(&static_cast<QThreadPrivate *>(QObjectPrivate::get(d->threadData->thread))->mutex);
            if (d->threadData->quitNow)
                return -1;

            if (d->inExec) {
                qWarning("QEventLoop::exec: instance %p has already called exec()", this);
                return -1;
            }

            struct LoopReference {
                QEventLoopPrivate *d;
                QMutexLocker &locker;

                bool exceptionCaught;
                LoopReference(QEventLoopPrivate *d, QMutexLocker &locker) : d(d), locker(locker), exceptionCaught(true)
                {
                    d->inExec = true;
                    d->exit = false;
                    ++d->threadData->loopLevel;
                    d->threadData->eventLoops.push(d->q_func());
                    locker.unlock();
                }

                ~LoopReference()
                {
                    if (exceptionCaught) {
                        qWarning("Qt has caught an exception thrown from an event handler. Throwing\n"
                                 "exceptions from an event handler is not supported in Qt. You must\n"
                                 "reimplement QApplication::notify() and catch all exceptions there.\n");
                    }
                    locker.relock();
                    QEventLoop *eventLoop = d->threadData->eventLoops.pop();
                    Q_ASSERT_X(eventLoop == d->q_func(), "QEventLoop::exec()", "internal error");
                    Q_UNUSED(eventLoop); // --release warning
                    d->inExec = false;
                    --d->threadData->loopLevel;
                }
            };
            LoopReference ref(d, locker);

            // 当进入一个新的事件循环时，移除所有已经发送的quit事件
            QCoreApplication *app = QCoreApplication::instance();
            if (app && app->thread() == thread())
                QCoreApplication::removePostedEvents(app, QEvent::Quit);
			
			//笔者吐槽：经历了千沟万壑，终于看到事件循环了。但，怎么这么短啊！太令人失望了
			//如果没有被要求退出，就一直循环并不断处理消息
            while (!d->exit)
                processEvents(flags | WaitForMoreEvents | EventLoopExec);

            ref.exceptionCaught = false;
            return d->returnCode;
        }
```

然后，我们看一看处理消息的函数：

```cpp
        bool QEventLoop::processEvents(ProcessEventsFlags flags)
		{
            Q_D(QEventLoop);
            if (!d->threadData->eventDispatcher)
                return false;
            return d->threadData->eventDispatcher->processEvents(flags);
        }
```

这个函数就很简单了，调用我们之前已经创建好的事件分发器的事件处理函数而已。至此，Qt程序运行的基本轮廓就已经被勾勒完全了。

### Qt程序运行基本轮廓 ###
在繁琐的跟踪过程之后，我们简单整理一下我们所看到的内容：

1.  对locale等等内容进行初始化
2.  创建对应平台的事件分发器
3.  调用`exec()`进入事件循环，直到接受到退出消息。

其中，事件循环是通过在QEventLoop中不断调用QAbstractEventDispatcher的子类的`processEvents()`实现的。

