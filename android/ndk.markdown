#CrystaX NDK 10  

##˵��  
**CrystaX NDK**��Google's Android NDK��һ���������. ��ȹȸ��NDK, **CrystaX NDK**��֧����ͬ���ܵ�ͬʱ, �ṩ��һЩ�ܰ����¹��ܺʹ�����ȱ���޸��͸Ľ�.  
  
**CrystaX NDK**����ҪĿ������Android�����߸���Ч�����ñ�׼���������native����. ͨ��ʹ����Щ֧�ֶ�ƽ̨(IOS,OS X,Windows,Linux�ȵ�)�ı�׼�����,**CrystaX NDK**������ʡ�˿���ʱ��,����Ҫ��ΪAndroidƽ̨�������޸�(������ΪAndroid���ⶨ��ʵ����Щ������ƽ̨����ʵ�ֵĹ���).  

����Android libc(Bionic)��������,�汾֮�仹������ͬ,��������Ҫ���ܶ�����ʱ�汾���ͼ���������Ķ��⹤��. **CrystaX NDK**�ṩ��libcrystax������Android�汾�����Բ���,������д�˺ܶ�libc����,ʹ��Ӧ�ó���������Android�豸�ϱ���һ��.  
  
**CrystaX NDK**����һ��Ŀ����ΪAndroid Native�����ṩһЩ�ܰ����¹���.����: Objective-C������������Ե�֧��.  
  
�����Ŀ�����2009��Dmitry Moskalchuk�ĸ�����Ŀ,ֻ��Ϊ�����һЩGoogle Android NDKȱʧ��C++����(exceptions,RTTI,C++��׼��),����Խ��Խ��, �����Ż�, **CrystaX NDK**�𲽱����Android native��������ѽ���.�ܶ࿪Դ����ҵ����Ŀʹ��**CrystaX NDK**����Android��������ֲ.  
  
����**CrystaX NDK**�ṩ�˴����¹���,ʹ��Android native����������,������¹ؼ�����.  
  
###1. ����֧�ֿ��ַ�  
���ַ���C��C++��׼���һ����,����C��׼���**wcsnlen, wcsncasecmp, wcsncmp**��[����](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/wchar.h.html), C++��׼���**wchar_t**, **std::base_string, std::basic_ostream, std::basic_regex**�ȵ�.Android libc (Bionic)����Щ֧�ֲ���.  
  
**CrystaX NDK**�ṩ��׼���Ŀ��ַ�֧��,����multibyte-to-wide�ͷ�֮��Ȼ��ת��. �����������ֲ���д���, ����׫д�´���, ����ʹ�ÿ��ַ�, �ַ���, ���Լ�������ʽ, ����������Androidϵͳ.  
����  
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

    std::string hello = "���է�ѧӧ��ӧ��, �ާڧ�!";

    wchar_t buf[128];
    mbstowcs(buf, hello.c_str(), hello.size());

    std::wstring s(buf);
    std::wcout << s << std::endl;  //=> ���է�ѧӧ��ӧ��, �ާڧ�!
}

int main()
{
    hello();
    strconv();

    return 0;
}
```

###2. ����֧��C��������locales  
Android libc (Bionic) ��֧��locales, ������native������ʹ�ñ��ػ����������Ψһ������ͨ��JNI����Java���ػ�ʵ��. ��Ȼ���ַ�������������������ʱ����, ��������ʹ�ùȸ�Android NDK��Ψһѡ��.  
  
**CrystaX NDK**����֧��locales, ֻ��򵥵��ñ�׼��**setlocale**, ���к���C��׼�������ö�ʹ�����õ�locale���������. **CrystaX NDK**��֧��"locales��չ" (����[IEEE Std 1003.1, 2013 Edition](http://pubs.opengroup.org/onlinepubs/9699919799/)), �ṩ�ܶ��׼C�ӿڵ�locale��Ч�ĺ���,����**printf_l, strftime_l, strfmon_l**�ȵ�.  
����  

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
    printtime("ru_RU.UTF-8", t);      /*=> �ӧ��ܧ�֧�֧ߧ��, 28 ��֧ߧ��ҧ�� 2014 ��. 02:04:04 */
    printtime("tr_TR.ISO8859-9", t);  /*=> Paz 28 Eyl 02:04:04 2014 */

    printmonetary("C", v);                /*=> 13653.68       */
    printmonetary("en_US.UTF-8", v);      /*=> $13,653.68     */
    printmonetary("fi_FI.UTF-8", v);      /*=> 13.653,68�     */
    printmonetary("sv_SE.ISO8859-1", v);  /*=> 13 653,68 kr   */
    printmonetary("no_NO.ISO8859-15", v); /*=> kr13.653,68    */
    printmonetary("ru_RU.UTF-8", v);      /*=> 13 653,68 ����. */
    printmonetary("tr_TR.UTF-8", v);      /*=> L 13.653,68    */

    return 0;
}
```
###3. ������ѧ֧��, �����������͵ķ��ͺ���  
ISO C��׼����ͷ�ļ�<tgmath.h>��<complex.h>. ��Google's Android NDK��, <complex.h>ʼ��r10, ��֧��Android 5.0 (android-21), type-generic math functionsû������ʵ��.  
��**CrystaX NDK**, ûʲô��˵��,������ʵ����. ����ʹ��!  

###4. ���¹�����  
**CrystaX NDK**�������°�GCC��Clang������. ֧��������,����C++11/C++14.  

###5. C++11/C++14֧��  
����**CrystaX NDK**�������°�GCC��Clang������, ������֧������C++11/C++14����. [C++11 Support in GCC](https://gcc.gnu.org/projects/cxx0x.html), [C++14 Support in GCC](https://gcc.gnu.org/projects/cxx1y.html)��[C++ Support in Clang](http://clang.llvm.org/cxx_status.html).  

###6. ������C++��׼��  
**CrystaX NDK**�ṩ�������õ�C++11��׼��, ����**std::thread** �� **std::mutex**, **std::chrono**����ͷ���, �Լ�����**std::stol, std::stoul** etc. ��Щ��Google's Android NDK�ж�ȱʧ.�����ϸ��˵,���ڵ�������ʵ����LLMV libc++ʵ��.  
  
**CrystaX NDK**����֧��C++��׼��, ������ʹ��GNU libstdc++��LLVM libc++.  
  
����, LLVM libc++ ��**CrystaX NDK**��Ҳ�ǵ�һ���ȼ�, ��λ��ͬ��GNU libstdc++, �������������ѡ������֮һ,Ĭ��ʹ��GNU libstdc++.  
  
����  
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
###7. ����Boost C++��  
�ṩ��Ԥ����õ�Boost C++���ļ�. ����ʹ��Boost,�������Լ�������.����Ҫ����, Boost C++���**CrystaX NDK**����! ��Ϊ���ڸ��ӱ�׼��֧�ֵ�**CrystaX NDK**(�ǵ�, ����libcrystax, **Crystax NDK**�ĺ���).  
  
���� [�����Android������ʹ��Boost C++��](https://www.crystax.net/en/blog/2)  

###8. ֧��Objective-C and Objective-C++  
Google's NDKֻ֧��C C++. **CrystaX NDK**��֧��Objective-C��Objective-C++. Ŀǰֻ֧�ֺ������Բ���; ����ʵ��Cocoa���Objective-C v2 runtime. ʹ��Objective-C, ֻ�����Դ���ļ���Android.mk, LOCAL_SRC_FILES��� .m (Objective-C) �� .mm (Objective-C++).  

###9. ��׼C��Ĵ���ȱ���޸��͹��ܸĽ�  
������֪Android libc (Bionic)�ı�׼C����ʵ���д���bug, ����**strtod**, ֻ����򵥸�ʽ���ַ���������Ч, ��֧��������׼C��Ҫ��. ��Щbug�޸���,��Щ��û. ��ʹ�޸���,Ҳֻ�Ժ����°汾��Ч.������Աֻ���Լ�ʵ������ѷ����汾������.  
**CrystaX NDK**��libcrystax����д����Щ��bug�ĺ���,����ƽ̨�汾�޹�,�Կ�����Ա͸��.  

###10. δ�����...  
����㻹��Ҫ��������,��[��ϵ����](https://www.crystax.net/en/contact). ��ӭ����[�����ȱ��](https://tracker.crystax.net/projects/ndk), ��Ȼ,Ҳ�ǳ���ӭ����!  

##���

չ��CrystaX NDK��δ��. �������������һ���ۺ��Ե�Android native����������, �ṩһ����ʵ�ɿ��Ļ���,ʹ���κ�POSIX���ݵ���������޸ľͿ�������Android��. Ϊ��ʵ����һĿ��,������Ҫ������֧�ְ���.  
������[BountySource](https://www.bountysource.com/teams/crystaxndk/fundraiser)������һ���ڳ�, Ϊ����Ŀ��δ����չ, Ϊ�˶�����Android������Աʼ�ձ������, ����ÿһ����עAndroid native developmδ����չ����Ա�İ���. ��ӭ�κ���ʽ��֧��, �ʽ�, �����Ƿ���patch, ��������.  
���Ķ�[CrystaX NDK����ʷ��δ��](https://www.crystax.net/en/blog/4), ��������ʵ����.  

##��Ȩ

**CrystaX NDK**��������open-source licenses. ���ÿ��ģ��İ�Ȩ����.  
��ע��**CrystaX NDK**��������Ҳ����������,������,�ĵ��ȵ�Ԥ����������ļ�. ��������Դ����[GitHub](https://github.com/crystax/) (�����ʹ��[����ű�](https://www.crystax.net/download/ndk-crystax-r10-build.sh)�Զ�������).  
Ԥ�����GCC�������������ļ�(GDB, binutils �ȵ�)����the GNU General Public License (GPL) �� the GNU Lesser General Public License (LGPL). ���COPYING and COPYING.LIB λ�� $NDK/toolchains/<toolchain>/prebuilt/<system>.  
Ԥ�����LLVM/Clang����������[LLVM "BSD" license](http://llvm.org/docs/DeveloperPolicy.html#license).  
������, ��Ȩ�����ͬ��Google's Android NDK - ����, ������ҵ&����ҵ��;. Ψһ������CrystaX����, ����BSD 2-clause license.  
```
CrystaX NDK contains code from libc library of FreeBSD project which by-turn contains
code from other projects. Also, several another open-source projects used, such as GNU gcc,
GNU binutils, LLVM clang, LLVM libc++ etc. To see specific authors and/or licenses, look
into appropriate source file. Here is license for those parts which are not derived from
any other projects but written by CrystaX .NET.

Copyright (c) 2011-2015 CrystaX .NET.
All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are
permitted provided that the following conditions are met:

   1. Redistributions of source code must retain the above copyright notice, this list of
      conditions and the following disclaimer.

   2. Redistributions in binary form must reproduce the above copyright notice, this list
      of conditions and the following disclaimer in the documentation and/or other materials
      provided with the distribution.

THIS SOFTWARE IS PROVIDED BY CrystaX .NET ''AS IS'' AND ANY EXPRESS OR IMPLIED
WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND
FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL CrystaX .NET OR
CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF
ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

The views and conclusions contained in the software and documentation are those of the
authors and should not be interpreted as representing official policies, either expressed
or implied, of CrystaX .NET.

```
##����  

| �ļ� | ��С | SHA1У��ֵ |
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

##��α���

����㲻��ʹ��Ԥ����汾, Ҳ�����Լ�����.�����������.  

**���ñ��뻷��**  
��ѭAOSP�ı�׼ (����Java����):  
��� [Linux](http://source.android.com/source/initializing.html#setting-up-a-linux-build-environment)����.  
��� [OS X](http://source.android.com/source/initializing.html#setting-up-a-mac-os-x-build-environment)����.  
**����!!! ����Windows�汾��CrystaX NDK,ֻ֧��Linux����(ʹ�ý������). ��Windows�����ϱ���δ�����Ժ�֧��**.  

**����NDK**  

��������[build script](https://www.crystax.net/download/ndk-crystax-r10-build.sh).  
����:  
```
\curl -sSL https://www.crystax.net/download/ndk-crystax-r10-build.sh | /bin/bash
```
�����кܾ� - ������Сʱ. �ű�ִ����ɺ�, ��������ɵ�NDK��������·��.  
�����! ʹ��������Google's NDK��!