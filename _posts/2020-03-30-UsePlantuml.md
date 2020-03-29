---
layout: post
title: "plant uml 用法"
date:   2020-3-30
tags: [后台开发]
comments: true
author: young
---
> 原文地址 https://www.cnblogs.com/ChanWunsam/p/9863154.html

[使用 Sublime + PlantUML 高效地画图](https://www.jianshu.com/p/e92a52770832)
[一分钟 Sublime Text 搭建 PlantUML 生成环境](https://www.jianshu.com/p/d5fd9133c78a)

## 最省力的方式

使用 chrome 插件`plantuml viewer`，然后直接用 chrome 打开该文件即可。注意要在插件管理中勾选：允许访问文件网址。

## 本地生成方式

对于一些特别大的文件，直接用 chrome 打开可能会很卡。这时候可以本地将 puml 文件转化为 png 或 svg 图，安装步骤如下：

### 安装 java 运行时环境

[官网地址](https://www.oracle.com/technetwork/java/index.html)

注意，应安装 javaSE 版本，然后同时在系统设置那里添加`“JAVA_HOME”：“XXX/java/jdk”`。

### 安装 plantuml.jar

[官网地址](http://plantuml.com/download)

如果你安装好了 java 环境，理论上在命令行中输入 java -jar plantuml.jar 'sample'.uml 就可以了。
新版本的 plantuml.jar 可以有不需要 graphviz 的语法，不过如果你使用的仍是旧版本的语法，那就需要进行下一步的安装了。

```

@startuml
(*)-->"HelloWorld"
"HelloWorld"-->(*)
@enduml

@startuml
start
:HelloWorld;
stop
@enduml

```

### 安装 graphviz

[官网地址](http://graphviz.org/)

graphviz 是一个开源图形库。安装后最好在系统设置那里添加`"GRAPHVIZ_DOT":"XXX/bin/dot.exe"`，XXX 表示你安装的本地路径。

### 配置 sublime 环境

在`Tools->Build System->New Build System`打开的文件中，加入以下代码:

```
{
    "cmd": "java.exe -jar XXX/plantuml.jar -charset UTF-8 $file",
    "path":"XXX/Java/jre/bin/",
    "env": {"GRAPHVIZ_DOT":"XXX/graphviz/bin/dot.exe"}
}

```

说明：

*   如果`JAVA_HOME`已经加到了环境变量`PATH`中，可以省略上面的`path`
*   如果`GRAPHVIZ_DOT`已经加到了环境变量中，可以省略上面的`env`
*   保存文件即可，文件名任取，建议为`Puml.sublime-build`

### 测试

在 sublime 中任选一个 puml 文件，`crtl+B`运行该文件，就会生成该文件的 png 图片，和直接用 chrome 插件打开的效果一样。

[官方文档（中英混杂）](http://plantuml.com/sitemap-language-specification)

## 流程图

[文档](http://plantuml.com/activity-diagram-beta)

Tips：

*   使用 `start` 来表示流程开始，使用 `stop` 来表示流程结束
*   顺序流程使用冒号和分号 `:xxx;`
*   条件语句使用 `if ("condition 1") then (true/yes/false/no) + else (false/no) + endif`
*   循环语句使用 `while('condition') is ('stop condition') + endwhile`
*   注释使用 `note left/right + end note`

例子 1：

```
@startuml
start
if (condition A) then (yes)
  :Text 1;
elseif (condition B) then (yes)
  :Text 2;
  stop
elseif (condition C) then (yes)
  :Text 3;
elseif (condition D) then (yes)
  :Text 4;
else (nothing)
  :Text else;
endif
stop
@enduml

```

![](https://img2018.cnblogs.com/blog/1381006/201810/1381006-20181027201039345-424199806.png)

例子 2：

```
@startuml
while (check filesize ?) is (not empty)
  :read file;
endwhile (empty)
:close file;
@enduml

```

![](https://img2018.cnblogs.com/blog/1381006/201810/1381006-20181027200915397-1821409646.png)

## 时序图

[文档](http://plantuml.com/sequence-diagram)

> 时序图（Sequence Diagram），又名序列图、循序图，是一种 UML 交互图。它通过描述对象之间发送消息的时间顺序显示多个对象之间的动态协作。

## 用例图

[文档](http://plantuml.com/use-case-diagram)

## 类图或对象图

[文档](http://plantuml.com/object-diagram)
