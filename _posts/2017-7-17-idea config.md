发现编译java,使用32位会速度快很多
一、系统参数设置：
1.64位IDEA：增加IDEA_JDK_64系统变量

2.32位IDEA：增加IDEA_JKD系统变量
寻找顺序:
二、参考说明

https://intellij-support.jetbrains.com/hc/en-us/articles/206544879-Selecting-the-JDK-version-the-IDE-will-run-under

详情如下：

<product>.exe uses this JDK search sequence:

IDEA_JDK / PHPSTORM_JDK / WEBIDE_JDK / PYCHARM_JDK / RUBYMINE_JDK /CLION_JDK /DATAGRIP_JDK environment variable (depends on the product,WEBIDE_JDK applies to both WebStorm and PhpStorm before version 2016.1)

..\jre directory
system Registry
JDK_HOME environment variable
JAVA_HOME environment variable
idea64.exe uses this JDK search sequence:

IDEA_JDK_64 environment variable
..\jre64 directory
system Registry
JDK_HOME environment variable
JAVA_HOME environment variable
It’s also possible to start the IDE with <product>.bat file located in the bin directory, it uses the following JDK search sequence:

IDEA_JDK / PHPSTORM_JDK / WEBIDE_JDK / PYCHARM_JDK / RUBYMINE_JDK/ CLION_JDK /DATAGRIP_JDK environment variable (depends on the product)
..\jre directory
JDK_HOME environment variable
JAVA_HOME environment variable
Environment variable must point to the JDK installation home directory, for example:

c:\Program Files (x86)\Java\jdk1.8.0_66
The actual JDK version used by the IDE can be verified in Help | About dialog (open any project to access the menu).
Define IDEA_JDK / PHPSTORM_JDK / WEBIDE_JDK / PYCHARM_JDK / RUBYMINE_JDK / CLION_JDK /DATAGRIP_JDK variable depending on the product to override the default version from IDE_HOME\jre.
Use Rapid Environment Editor to add/edit the variables, it will detect incorrect paths.

目前设置:
IDEA_JKD_64 使用 jdk.18.0_91_x64版本,
编译使用的是32位版本的jdk


