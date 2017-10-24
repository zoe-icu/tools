# 用法

## 注意  
+ 除非有另外的说明，在OS X中super(command)代替ctrl键  
+ 提到(GO)PATH变量，在Linux和OS X中用冒号`(:)`分割，在windows中用分号`(;)`分割 
 
## 设置
你可以自定义gosublime的‘行为’通过创建或者自定义`Packages/User/GoSublime.sublime-settings`文件。默认的配置记录在`Packages/GoSublime/GoSublime.sublime-settings`文件中。__注意__不要编辑任何在`Packages/User/`包外的任何包文件，包括`Packages/GoSublime`。这些文件将会在sublime更新或者各自的包更新的时候被重写。您还可能无意中阻止各自的包能够通过Git等的更新。

## 快速开始
这部分假设你已经知道什么是`GOPATH`并且知道怎么配置。如果你不知道，请访问
[http://golang.org/doc/code.html](http://golang.org/doc/code.html)

在一些系统中，环境变量并不能如预期的那样传递。这就造成一些命令例如`go build`会报`command cannot be found` 或者 `GOPATH is not set`的错误。为了解决这个问题，最简单的事情是在设置文件中设置这些变量。请参阅`env`或`shell`设置的文档，两者都记录在默认设置文件`Packages/User/GoSublime.sublime-settings`中.

## 代码完成
可以通过在Golang文件中键入（默认）组合`CTRL + [SPACE]`来访问完成。

## 快捷键绑定
默认情况下，一些绑定的快捷键已经提供了。可以通过打开命令面板并键入`GoSublime`或者通过键绑定的键`ctrl + .` (或`command + .`在 OS X)来查看他们。在OS X中（除非另有说明）我提到的`ctrl+`默认被定义为`command+`。

## 有用的快捷键
通常在注释一行时，紧随其后的操作是将光标移动到下一行以继续工作或注释掉接下来的行。通过设置快捷键，你可以将改行注释掉并且光秒自动移到下一行。e.g：

`{ "keys": ["ctrl+/"], "command": "gs_comment_forward", "context": [{ "key": "selector", "operator": "equal", "operand": "source.go" }] }`

## 包导入
按`ctrl+.`, 'ctrl+p'可以打开包列表，您可以从中快速引入或者删除引入的包。这两者的用法相同。如果包已导入, 则它将出现在顶部附近, 并被标记为删除操作，所以实际上它是一个切换。
如果您想编辑包的别名，例如数据库驱动:  
1. 首先将包作为正常导入, 然后按 `ctrl+ .`, `ctrl + i` 以快速跳转最后一个导入的包。  
2. 编辑后, 您可以返回到您所在的位置按 `ctrl + .`, `ctrl + [`

## 编译(building)、测试和 Go 命令
GoSublime带有的部分command/shell集成`9o`。查看`9o`的更多信息，请查看Packages/GoSublime/9o.md。在sublime中可以通过`ctrl+9`(`command+9`)，然后键入`help`进行了解。  

要运行程序包的测试，你有3个选项。  

* 按`ctrl+.`,`ctrl+t`打开快速测试面板.这个提供了基本/常用选项，如运行所有基准功能或者运行一个单元测试。  
* 在 _test. go 文件中, 按下 `ctrl + shift`键并left-click测试的名称、基准或示例函数, 例如点击TestAbc就只执行该函数。
* 如果以上的选项太简约，或者你想用调用`go test`运行你的选项，可以通过按`ctrl+9`打开'go'命令面板来开启9o。

在编译包的情况下，9o提供了一个回`replay`命令(详见9o.md)。这个命令会执行pkg命令当pkg是一个命令pkg(pkg main)或者运行所有的测如果他是一个正常的pkg.这个重播命令被绑定到`ctrl+.`,`ctrl+r`。

GoSublime通过`ctrl + b`为Sublime Text build-system提供了一个重写。在菜单栏`Tools>Build System`中，它被命名为`GoSublime`。`ctrl + b` 由Sublime Text自动处理,
所以如果您选择了另一个Go build system，`ctrl + b`将会执行你选择的。要直接访问GoSublime build system，请按`ctrl + .`，`ctrl + b`。这个build system只需打开9o并展开最后一个命令。即执行9o命令`^1`。

## 项目设置和基于项目的GOPATH
如果您的项目设置中有一个名为GoSublime的设置对象，其值将覆盖GoSublime.sublime-settings文件内的值。因此，您可以为单个项目设置特定的GOPATH。e.g.  
`my-project.sublime-project`

`{
    "settings": {
        "GoSublime": {  
            "env": {
                "GOPATH": "$HOME/my-project"
            }
        }
    },
    "folders": []
}`

如果你总是通过改变GOPATH来进行项目编译，那么，你也可以通过将字符串$ GS_GOPATH添加到全局GOPATH设置中来进行编译。

`{
    "env": { "GOPATH": "$HOME/go:$GS_GOPATH" }
}`

GS_GOPATH是伪环境变量。它其实是去匹配到GOPATH：  

* 当前工作目录, e.g. `~/go/src/pkg then` `$GS_GOPATH` 是 `~/go/`