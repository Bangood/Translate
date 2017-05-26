---
type: tutorial
layout: tutorial
title:  "Kotlin and Continuous Integration with TeamCity"
description:
authors: Hadi Hariri
date: 2015-02-02
showAuthorInfo: false
---
接下来我们将去了解如何配置TeamCity来构建我们的Kotlin项目。如果想了解更多关于TeamCity的信息，可以去查看详细 [文档](https://www.jetbrains.com/teamcity/documentation/)，里面包含诸如安装、基本配置等等内容。


Kotlin可以使用许多不同的 [构建工具](build-tools.html), 因此，如果你使用像`Ant`, `Maven`, `Gradle`这样的标准工具来构建Kotlin项目，配置过程和你配置其他语言或库一样。TeamCity同样支持IntelliJ IDEA的内置构建系统JBS，只不过这可能有一点点不同，有一些小需求。

### 使用标准构建工具
如果使用Ant, Maven或者Gradle，配置过程十分简单，所需的就是定义构建步骤。在我们的案例中，使用Gradle, 我们只需要简单定义一些必须的如步骤名这样的参数，和供`Runner`执行的Gradle任务的类型。

<br/>
![Gradle 构建步骤]({{ url_for('tutorial_img', filename='kotlin-and-ci/teamcity-gradle.png') }})
<br/>

Kotlin所需的所有依赖需求都已经被定义在了Gradle文件里，我们不需要为Kotlin做其他的任何特殊配置就可以正确的运行。

如果使用Ant或Maven，我们可以使用相同的配置。唯一的区别就是Runner的类型需要设置成Ant或者Maven。


### 使用 IntelliJ IDEA 构建系统
如果通过TeamCity使用IntelliJ IDEA构建系统，我们需要确保IntelliJ IDEA所使用的Kotlin版本和TeamCity 运行使用的Kotlin版本一致。这就需要我们在TeamCity中下载安装特定的Kotlin插件版本。

幸运的是，有一个`meta-runner`已经几乎可以帮助我们完成这些所有手动操作。如果你对于TeamCity的meta-runner还不是很了解，可以查看这个[文档](https://confluence.jetbrains.com/display/TCD9/Working+with+Meta-Runner)。 Meta-runner是创建自定义Runner的快捷且强大的方式，避免了我们写插件。

#### 下载和安装 meta-runner
供Kotlin使用的meta-runner可以在[GitHub](https://github.com/jonnyzzz/Kotlin.TeamCity)上下载。如果你使用的是TeamCity 9或以上版本，可以非常便捷的在其用户界面导入meta-runner。

<br/>
![Meta-runner]({{ url_for('tutorial_img', filename='kotlin-and-ci/teamcity-metarunner.png') }})
<br/>

如果你使用的是TeamCity 9以下的版本，可以参考这篇 [关于如何添加meta-runner的文档](https://confluence.jetbrains.com/display/TCD9/Working+with+Meta-Runner)。

#### 设置获取Kotlin编译器的步骤
基本上，这一步骤限于我们所需的步骤名和Kotlin版本。 可以使用标签。

<br/>
![设置Kotlin编译器]({{ url_for('tutorial_img', filename='kotlin-and-ci/teamcity-setupkotlin.png') }})
<br/>

Runner会根据IntelliJ IDEA项目设置的路径，为`system.path.macro.KOTLIN.BUNDLED`属性设置一个正确的值。但是，这个值必须在TeamCity中定义好（可以设为任意值）。因此，我们需要将其定义为一个系统变量。

#### 设置 Kotlin 编译步骤
最后一步是定义使用标准的IntelliJ IDEA Runner类型项目的实际编译。

<br/>
![IntelliJ IDEA Runner]({{ url_for('tutorial_img', filename='kotlin-and-ci/teamcity-idearunner.png') }})
<br/>

至此，我们的项目就可以构建并生成相应结果了。

### 其它持续集成服务器
如果使用与TeamCity不同的持续集成工具，只要它支持任何构建工具，或者调用命令行工具，就可以编译Kotlin并将其自动化作为持续集成过程的一部分。


