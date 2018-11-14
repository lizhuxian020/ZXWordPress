## Pod install 报错

```
[!] /usr/local/bin/bash -c
set -e
#!/bin/bash
set -e

PLATFORM_NAME="${PLATFORM_NAME:-iphoneos}"
echo "lzx",${PLATFORM_NAME}
CURRENT_ARCH="${CURRENT_ARCH:-armv7}"
echo "lzx",${CURRENT_ARCH}
export CC="$(xcrun -find -sdk $PLATFORM_NAME cc) -arch $CURRENT_ARCH -isysroot $(xcrun -sdk $PLATFORM_NAME --show-sdk-path)"
export CXX="$CC"

# Remove automake symlink if it exists
if [ -h "test-driver" ]; then
    rm test-driver
fi

./configure --host arm-apple-darwin

# Fix build for tvOS
cat << EOF >> src/config.h

/* Add in so we have Apple Target Conditionals */
#ifdef __APPLE__
#include <TargetConditionals.h>
#include <Availability.h>
#endif

/* Special configuration for AppleTVOS */
#if TARGET_OS_TV
#undef HAVE_SYSCALL_H
#undef HAVE_SYS_SYSCALL_H
#undef OS_MACOSX
#endif

/* Special configuration for ucontext */
#undef HAVE_UCONTEXT_H
#undef PC_FROM_UCONTEXT
#if defined(__x86_64__)
#define PC_FROM_UCONTEXT uc_mcontext->__ss.__rip
#elif defined(__i386__)
#define PC_FROM_UCONTEXT uc_mcontext->__ss.__eip
#endif
EOF

lzx,iphoneos
lzx,armv7
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for arm-apple-darwin-strip... no
checking for strip... strip
checking for a thread-safe mkdir -p... ./install-sh -c -d
checking for gawk... no
checking for mawk... no
checking for nawk... no
checking for awk... awk
checking whether make sets $(MAKE)... yes
checking whether make supports nested variables... yes
checking for arm-apple-darwin-gcc... /Library/Developer/CommandLineTools/usr/bin/cc -arch armv7 -isysroot
checking whether the C compiler works... no
xcrun: error: SDK "iphoneos" cannot be located
xcrun: error: SDK "iphoneos" cannot be located
xcrun: error: SDK "iphoneos" cannot be located
xcrun: error: unable to lookup item 'Path' in SDK 'iphoneos'
/Users/zhuxianli/Library/Caches/CocoaPods/Pods/External/glog/f09d6cdb8398b4922e87d51f5245de7e-0226e/missing: Unknown `--is-lightweight' option
Try `/Users/zhuxianli/Library/Caches/CocoaPods/Pods/External/glog/f09d6cdb8398b4922e87d51f5245de7e-0226e/missing --help' for more information
configure: WARNING: 'missing' script is too old or missing
configure: error: in `/Users/zhuxianli/Library/Caches/CocoaPods/Pods/External/glog/f09d6cdb8398b4922e87d51f5245de7e-0226e':
configure: error: C compiler cannot create executables
See `config.log' for more details

```



原因: 在执行glog.podSepc的时候, 有个prepareScripts, 会执行…/Scripts里的xxxx-glog.sh, 里面执行到xcrun这个命令的时候, 报错了, 随后谷歌了一下"xcrun: error: SDK "iphoneos" cannot be located", 发现是XcodeCommand的路径不对, 想着是因为我新电脑先装了HomeBrew, 然后brew装了XcodeCommand, 之后再装了Xcode, 可能这里造成路径出错, 验证此错误使用以下命令:

```
xcodebuild -showsdks
xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory '/Library/Developer/CommandLineTools' is a command line tools instance
```

使用一下命令还原回来即可: 

```
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer/
```

----



## Yarn命令

修改 | 添加 指定报名, 指定版本: `Yarn add PackageName@Version`



## NPM 命令

`npm install PackgeName --save` 会将模块依赖写入Package.json的dependencies节点

`npm install PackageName --save-dev` 会写入Package.json的devDependencies节点

## 安装node指定版本10.7.0

先安装nvm, 使用以下命令:

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
```

使用nvm -v测试一下是否成功, 如果没成功,就重启terminal

使用以下命令安装node(PS: 一定要加v)

`nvm install v10.7.0`



## Git仓库

`git clone http://lwx629116@rnd-ott.huawei.com/hw-ws/hiapp/cross_platform/smarthome -b hw/sz/mbb_home/platform/Myna/master`



