#Android原生开发套件  
  
我们推出的CrystaX NDK - 是一个多年来被众多商业和开源项目广泛使用的开源产品. 为什么不使用谷歌的Android NDK? 因为CrystaX NDK在保持对谷歌官方版本兼容的同时表现更佳．基本上, CrystaX NDK是谷歌NDK的完整替代版本, 允许开发者以相同方式使用,同时提供了很多新增功能和优化改进.  
  
以下是CrystaX NDK的主要目的:  
+ 更好的标准化兼容性  
+ 轻松移植已有代码库到Android平台  
+ 为Android Native开发添加新特性  
  
有趣吧? CrystaX NDK给Android开发者们带来了哪些新特性, 请[查看更多信息](https://www.crystax.net/cn/android/ndk).  
#新闻  
  
##[GCC 6的Android进展](https://www.crystax.net/cn/blog/8)  2016.02.27 06:45  
  
如您所知，GCC 6来了。它拥有许多新特性， 包括针对C++17的实验性支持（新规范，**std::invoke**，**std::shared_mutex**,等等） 和许多新警告，帮助开发者编写更加安全可靠的代码。事实上，GCC 6， 如果启用所有警告功能，可以完全替代某些静态代码扫描分析工具。  
  
没有必要在此列出所有GCC 6的特性。对此感兴趣者可以到其他网站查阅即将发布的GCC 6的相关信息 （例如，[这里](http://developerblog.redhat.com/2016/02/23/upcoming-features-in-gcc-6/) 和 [这里](https://gnu.wildebeest.org/blog/mjw/2016/02/15/looking-forward-to-gcc6-many-new-warnings/) ）。但是我想分享一些关于使用GCC 6进行Android开发的信息，特别是，**CrystaX NDK**中GCC的未来发展。  
  
[阅读全文](https://www.crystax.net/cn/blog/8)  
  
##[Android Studio 实验性 Gradle 插件与 CrystaX NDK 的配合使用](https://www.crystax.net/cn/blog/7)  2015.12.14 14:20  
  
之前我们介绍过如何在 [Android Studio 中使用 CrystaX NDK](https://www.crystax.net/cn/blog/3) 。前些时候 Google 发布了一个 Android Studio 相关支持 NDK 开发的新(实验性)Gradle插件。 让我们来看看 如何基于这个新插件使用 **CrystaX NDK**。  
  
为了保持连贯性， 我们配合新插件的使用来重新梳理之前的文章， 依然是一个简单的UI应用程序，使用 Boost.Serialization。 使用 Boost 并不是为了配合 Gradle 实验性插件的新功能；我们使用 Boost 举例只 是为了覆盖更多的使用场景。  
  
[阅读全文](https://www.crystax.net/cn/blog/7)  
  
##[CrystaX NDK 10.3.0 已发布!](https://www.crystax.net/cn/blog/6)  2015.12.14 14:18  
我们很高兴的宣布 **CrystaX NDK 10.3.0** 已发布!  
  
这次发布的重大更新有：  
  
+ 更新GCC和LLVM/clang工具链  
+ Objective-C v2支持的重大优化  
+ 初步支持 Android 平台 Python (2.7 和 3.5)  
  
另外，我们修复大量缺陷并增加若干优化改进。详见下文。  

[阅读全文](https://www.crystax.net/cn/blog/6)  
  
##[CrystaX NDK 10.2.0 已发布!](https://www.crystax.net/cn/blog/5)  06/24/2015 03:01 PM  
  
我们很高兴的宣布**CrystaX NDK 10.2.0**已发布!  
  
我们[如约前行](https://www.crystax.net/cn/blog/4), 在这个版本中, 我们修复大量缺陷并实现大量优化改进. 请阅读下面的全文, 查看**CrystaX NDK 10.2.0**带来了哪些重要新特性.  
  
[阅读全文](https://www.crystax.net/cn/blog/5)  
  
##[筹款宣传活动开启](https://www.crystax.net/cn/blog/4)	03/24/2015 01:47 PM  
  
我们在[BountySource](https://www.bountysource.com/teams/crystaxndk/fundraiser)发起了一项筹款宣传活动, 旨在维持和继续CrystaX NDK的开发, 使得它能够对所有Android开发人员始终保持免费.  
我们请求每一个关注Android原生开发的未来发展的朋友帮助. 我们衷心的感谢任何形式的支持, 无论是捐款, 给项目提交补丁, 或者帮助我们宣传, 让更多人了解它.  
  
[阅读全文](https://www.crystax.net/cn/blog/4)  
##[使用Android Studio构建基于NDK和Boost C++库的应用程序](https://www.crystax.net/cn/blog/3)	01/29/2015 01:40 PM
  
在[上一篇](https://www.crystax.net/cn/blog/2), 我们介绍了如何构建一个简单的使用Boost C++库的Android可执行程序. 这个例子很好的说明了工作流程和内部原理. 但是从实用角度出发, 我们需要了解如何构建一个可以提交Google Play商店的完整Android应用程序. 本篇将举例说明.  
  
[阅读全文](https://www.crystax.net/cn/blog/3)  
##Boost + Android? CrystaX NDK!	01/20/2015 04:40 PM
  
你是否代码中使用了Boost C++库? 你是否想轻松移植这些代码到Android? 或者你只是想使用Boost开始创建一个Android平台的新项目? 通过CrystaX NDK 10.1.0, 你就可以同时拥有Android原生开发套件和Boost C++库!  
  
[阅读全文](https://www.crystax.net/cn/blog/2)  
##[Android原生本地化? 是的, 通过CrystaX NDK!](https://www.crystax.net/cn/blog/1)	01/20/2015 04:40 PM
  
好的应用必定要有好的用户体验. 首先这意味着应用程序应该支持用户所熟知的语言. 但是仅仅翻译文字和段落是远远不够的 - 不同语言文化之间还有很多其他不同元素, 包括日期格式, 货币, 格式化规则等等. 这些具体的信息都以[locales](http://zh.wikipedia.org/wiki/%E5%8C%BA%E5%9F%9F%E8%AE%BE%E7%BD%AE)来界定.  
  
ISO C和ISO C++标准定义了locale相关的输入输出操作规范, 所以通常只要按照标准正确本地化你的应用就够了. 不幸的是, 这种方法不适用于Android C/C++. Android libc (Bionic)不支持locales, 所以在native代码中使用本地化输入输出的唯一方法是通过JNI调用Java本地化实现. 显然这种方法增加了显著的运行时开销, 但这是你使用谷歌Android NDK的唯一选择.  
  
[阅读全文](https://www.crystax.net/cn/blog/1)  
##[CrystaX NDK 10.1.0 已发布!](https://www.crystax.net/cn/android/ndk)	01/20/2015 04:35 PM
  
我们很高兴的宣布CrystaX NDK 10.1.0已发布! 在这个版本中, 你可以找到一些很棒的功能和改进。例如更好的标准化兼容性,等等.  
  
[阅读全文](https://www.crystax.net/cn/android/ndk)  
