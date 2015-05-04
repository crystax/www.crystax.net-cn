#CrystaX NDK 10  

##说明  
CrystaX NDK是Google's Android NDK的一个替代方案,提供了一些很棒的新功能和大量的bugfix和改进.  
它的主要目的是让Android开发者更高效地运用标准化代码进行native开发. 通过使用那些支持多平台(IOS,OS X,Windows,Linux等等)的标准代码库,它显著减少了开发时间,不需要再为Android平台做特殊修改(甚至于为Android特殊定制实现那些在其他平台早已实现的功能).  
Android libc(Bionic)功能有限,版本之间还不尽相同,开发者需要做很多运行时版本检测和兼容性适配的额外工作.  
CrystaX NDK提供的libcrystax屏蔽了Android版本兼容性差异,甚至重写了很多libc函数,使得应用程序在所有Android设备上表现一致.  
CrystaX NDK的另一个目的是为Android Native开发提供一些很棒的新功能.例如: Objective-C和其他编程语言的支持.  
这个项目最初是2009年Dmitry Moskalchuk的个人项目,只是为了添加一些Google Android NDK缺失的C++特性(exceptions,RTTI,C++标准库),后来越加越多,不断优化一步步步变成了Android native开发的最佳进化.很多开源和商业化项目使用CrystaX NDK进行Android开发和移植.  
现在CrystaX NDK提供了大量新功能,使得Android native开发更容易,详见以下关键特性.  
  
###1. Full support of wide characters  
宽字符是C&C++标准库的一部分,例如C标准库的wcsnlen, wcsncasecmp, wcsncmp和[其他](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/wchar.h.html),C++标准库的wchar_t, std::base_string, std::basic_ostream, std::basic_regex等等.  
Android libc (Bionic)对这些支持不足.  
CrystaX NDK提供标准化宽字符支持,包括multibyte-to-wide和wide-to-multibyte转换. 轻松使用wide characters, strings, streams, and regular expressions.  
例子  
```
#include <iostream>
#include <iomanip>
#include <stdio.h>
#include <locale.h>

void hello()
{
    // C-style
    printf("%ls", L"Hello, world!\n");           //=> Hello, world!
    // C++-style
    std::wcout << L"Hello, world!" << std::endl; //=> Hello, world!
}

void strconv()
{
    setlocale(LC_CTYPE, "ru_RU.UTF-8");

    std::string hello = "Здравствуй, мир!";

    wchar_t buf[128];
    mbstowcs(buf, hello.c_str(), hello.size());

    std::wstring s(buf);
    std::wcout << s << std::endl;  //=> Здравствуй, мир!
}

int main()
{
    hello();
    strconv();

    return 0;
}
```

###2. Full support of C locales  
Android libc (Bionic) 不支持locales, 唯一使用方法是通过JNI调用Java实现.很明显效率低下.  
CrystaX NDK内置支持locales,只需简单调用标准的setlocale，所有后续C标准函数调用都使用设置的locale做输入输出.  
CrystaX NDK还支持"extended locales" (part of [IEEE Std 1003.1, 2013 Edition](http://pubs.opengroup.org/onlinepubs/9699919799/)), 提供很多标准C接口的locale-enabled函数,例如printf_l, strftime_l, strfmon_l等等.  
例子  

```
#include <locale.h>
#include <stdio.h>
#include <time.h>
#include <monetary.h>

void printtime(const char *locale, time_t t)
{
    char buf[256];

    setlocale(LC_TIME, locale);

    strftime(buf, sizeof(buf), "%c", localtime(&t));
    printf("TIME (locale "%s"): %s\n", locale, buf);
}

void printmonetary(const char *locale, double v)
{
    char buf[256];

    setlocale(LC_MONETARY, locale);

    strfmon(buf, sizeof(buf), "%.2n", v);
    printf("MONETARY (locale "%s"): %s\n", locale, buf);
}

int main()
{
    time_t t = (time_t)1411859044;
    double v = 13653.6783;

    printtime("C", t);                /*=> Sun Sep 28 02:04:04 2014 */
    printtime("en_US.UTF-8", t);      /*=> Sun Sep 28 02:04:04 2014 */
    printtime("fi_FI.UTF-8", t);      /*=> Su 28 Syy 02:04:04 2014  */
    printtime("sv_SE.UTF-8", t);      /*=> S?n 28 Sep 02:04:04 2014 */
    printtime("no_NO.UTF-8", t);      /*=> s?n 28 sep 02:04:04 2014 */
    printtime("ru_RU.UTF-8", t);      /*=> воскресенье, 28 сентября 2014 г. 02:04:04 */
    printtime("tr_TR.ISO8859-9", t);  /*=> Paz 28 Eyl 02:04:04 2014 */

    printmonetary("C", v);                /*=> 13653.68       */
    printmonetary("en_US.UTF-8", v);      /*=> $13,653.68     */
    printmonetary("fi_FI.UTF-8", v);      /*=> 13.653,68�     */
    printmonetary("sv_SE.ISO8859-1", v);  /*=> 13 653,68 kr   */
    printmonetary("no_NO.ISO8859-15", v); /*=> kr13.653,68    */
    printmonetary("ru_RU.UTF-8", v);      /*=> 13 653,68 руб. */
    printmonetary("tr_TR.UTF-8", v);      /*=> L 13.653,68    */

    return 0;
}
```
###3. Full math support, including complex and type-generic math functions  
ISO C标准定义头文件<tgmath.h>和<complex.h>. 在Google's Android NDK中, <complex.h>始于r10, 仅支持Android 5.0 (android-21), type-generic math functions没有完整实现.  
而CrystaX NDK, 没什么好说的,都完整实现了. just enjoy!  

###4. The most recent toolchains  
CrystaX NDK包含最新版GCC和Clang编译器. 支持新特性,例如C++11/C++14.  

###5. C++11/C++14 support  
由于CrystaX NDK包含最新版GCC和Clang编译器, 所以它支持所有C++11/C++14特性. [C++11 Support in GCC](https://gcc.gnu.org/projects/cxx0x.html), [C++14 Support in GCC](https://gcc.gnu.org/projects/cxx1y.html)和[C++ Support in Clang](http://clang.llvm.org/cxx_status.html).  

###6. Full C++ standard library  
CrystaX NDK提供完整可用的C++11标准库, 包括std::thread 和 std::mutex, functions/classes from std::chrono, and functions std::stol, std::stoul etc. 这些在Google's Android NDK中都缺失.或者严格地说,存在但仅限于实验版的LLMV libc++实现.  
CrystaX NDK完整支持C++标准库, 无论你使用GNU libstdc++或LLVM libc++.  
另外, LLVM libc++ 在CrystaX NDK中也是第一优先级, 地位等同于GNU libstdc++, 所以你可以自由选择其中之一,默认使用GNU libstdc++.  
例子  
```
#include <iostream>
#include <thread>
#include <mutex>
#include <chrono>
#include <ctime>

long fibonacci(unsigned n)
{
    if (n < 2) return n;
    return fibonacci(n-1) + fibonacci(n-2);
}

int main()
{
    std::mutex outmtx;

    auto f = [&](int n) {
        std::chrono::time_point<std::chrono::system_clock> start, end;
        start = std::chrono::system_clock::now();
        long fib = fibonacci(42);
        end = std::chrono::system_clock::now();

        std::chrono::duration<double> elapsed_seconds = end - start;
        std::time_t end_time = std::chrono::system_clock::to_time_t(end);

        std::lock_guard<std::mutex> lock(outmtx);
        std::cout << "THREAD #" << n << ": f(42) = " << fib << '\n';
        std::cout << "THREAD #" << n << ": finished computation at " << std::ctime(&end_time)
            << "elapsed time: " << elapsed_seconds.count() << "s\n";
    };

    std::thread t1(f, 1);
    std::thread t2(f, 2);
    std::thread t3(f, 3);
    t1.join();
    t2.join();
    t3.join();

    return 0;
}
```
###7. Boost C++ libraries out of the box  
提供了预编译好的Boost C++库文件. 轻松使用Boost,不用再自己折腾了.  
更重要的是, Boost C++库跟CrystaX NDK更配!  
例子 [如何在Android工程中使用Boost C++库](https://www.crystax.net/en/blog/2)  

###8. Objective-C and Objective-C++ support  
Google's NDK只支持C C++. CrystaX NDK还支持Objective-C和Objective-C++. 目前只支持核心语言部分; 正在实现Cocoa-like库和Objective-C v2 runtime. 使用Objective-C, 只需添加源码文件到Android.mk, LOCAL_SRC_FILES添加 .m (Objective-C) 和 .mm (Objective-C++).  

###9. Tons of fixes and improvements of standard C library implementation  
众所周知Android libc (Bionic)的标准C函数实现有大量bug, 例如strtod.有些改了,有些还没改. 即使改了,也只对后续新版本有效.开发人员只能自己搞定已发布版本的适配.  
CrystaX NDK在libcrystax中重写了这些有bug的函数,而且平台版本无关,对开发人员透明.  

###10. To be continued...  
如果你还需要其他特性,请[联系我们](https://www.crystax.net/en/contact). 欢迎反馈[问题和缺陷](https://tracker.crystax.net/projects/ndk), 当然,也非常欢迎贡献!  

##众筹

展望CrystaX NDK的未来. 我们想把它做成一个综合性的Android native开发工具箱, 提供一个坚实可靠的基础,使得任何POSIX兼容的软件无需修改就可运行在Android上. 为了实现这一目标,我们需要社区的支持帮助.  
我们在[BountySource](https://www.bountysource.com/teams/crystaxndk/fundraiser)发起了一项众筹活动, 为了项目的未来发展, 为了对所有Android开发人员始终保持免费, 请求每一个关注Android native developm未来发展的人员的帮助. 欢迎任何形式的支持, 资金, 给我们发送patch, 帮助传播.  
请阅读[CrystaX NDK的历史和未来](https://www.crystax.net/en/blog/4), 帮助我们实现它.  

##授权

CrystaX NDK包含若干open-source licenses. 详见The copyright disclaimers in each respective file.  
CrystaX NDK发布包中也包括编译器,链接器,文档等的预编译二进制文件. 工具链的源码在GitHub (你可以使用编译脚本自动下载它).  
预编译的GCC和其他二进制文件(GDB, binutils 等等)基于the GNU General Public License (GPL) 或 the GNU Lesser General Public License (LGPL). 详见COPYING and COPYING.LIB 位于 $NDK/toolchains/<toolchain>/prebuilt/<system>.  
预编译的LLVM/Clang工具链基于the LLVM "BSD" license.  
基本上, 授权规则等同于Google's Android NDK - 例如, 允许商业&非商业用途. 唯一区别是CrystaX部分, 基于BSD 2-clause license.  
##下载  

| File | Size | SHA1 |
| :--- | :--- | :--- |
| [crystax-ndk-10.1.0-windows-x86.exe](https://www.crystax.net/download/crystax-ndk-10.1.0-windows-x86.exe) | 800.793 MB | 1474fc065311f5cfd40531cba71fcc878ca591d7 |
| [crystax-ndk-10.1.0-windows-x86_64.exe](https://www.crystax.net/download/crystax-ndk-10.1.0-windows-x86_64.exe) |858.172 MB | f73a16892dc8f007872c5457a650e9b50969ebe1|
| [crystax-ndk-10.1.0-darwin-x86.7z](https://www.crystax.net/download/crystax-ndk-10.1.0-darwin-x86.7z) | 560.190 MB | d502f57ea314e5a572387b4492c297f77e68a982|
| [crystax-ndk-10.1.0-darwin-x86.tar.bz2](https://www.crystax.net/download/crystax-ndk-10.1.0-darwin-x86.tar.bz2) | 1.422 GB | c30724ff738bbcc1906bc2d89114fbe706a05fb5|
| [crystax-ndk-10.1.0-darwin-x86_64.7z](https://www.crystax.net/download/crystax-ndk-10.1.0-darwin-x86_64.7z) | 530.204 MB | dc125ae803ede7d4b68ac9d57b30909bfcb1a8d6|
| [crystax-ndk-10.1.0-darwin-x86_64.tar.bz2](https://www.crystax.net/download/crystax-ndk-10.1.0-darwin-x86_64.tar.bz2) | 1.386 GB | 505677a7c971fbfeb56e1ba19ea2262aadd439ed|
| [crystax-ndk-10.1.0-linux-x86.7z](https://www.crystax.net/download/crystax-ndk-10.1.0-linux-x86.7z) | 790.919 MB | 55aa8750e8ed8704e748114f7788dbfe81668737|
| [crystax-ndk-10.1.0-linux-x86.tar.bz2](https://www.crystax.net/download/crystax-ndk-10.1.0-linux-x86.tar.bz2) | 1.798 GB | 07698fe850311c43d14d29f93123a6f6ec69bf9b|
| [crystax-ndk-10.1.0-linux-x86_64.7z](https://www.crystax.net/download/crystax-ndk-10.1.0-linux-x86_64.7z) | 814.350 MB | a5a6964e5062239cfa72af37d9147f4577a8f21a|
| [crystax-ndk-10.1.0-linux-x86_64.tar.bz2](https://www.crystax.net/download/crystax-ndk-10.1.0-linux-x86_64.tar.bz2) | 1.824 GB | 498ebae28243de399bb9a59fbde1d04b24653e64|

##如何编译

如果你不想使用预编译版本, 也可以自己编译.详见以下流程.  

**配置编译环境**  
遵循AOSP的标准 (除了Java部分):  
For [Linux](http://source.android.com/source/initializing.html#setting-up-a-linux-build-environment)主机.  
For [OS X](http://source.android.com/source/initializing.html#setting-up-a-mac-os-x-build-environment)主机.  
警告!!! 编译Windows版本的CrystaX NDK,只支持Linux主机(使用交叉编译). 在Windows主机上编译未经测试和支持.  

**编译NDK**  

下载运行[build script](https://www.crystax.net/download/ndk-crystax-r10-build.sh).  
例子:  
```
\curl -sSL https://www.crystax.net/download/ndk-crystax-r10-build.sh | /bin/bash
```
会运行很久 - 几个小时. 脚本执行完成后, 会输出生成的路径.  
完成了! 使用它代替Google's NDK吧!