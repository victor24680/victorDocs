#### tput 命令
    用户可以使用shell脚本创建交互性的，专业性强的屏幕输出。

####### 用法:先初始化 `$ tput init`

###### 常用字符
|名字|解释|
|---|---|
|bel|警铃|
|blink|闪烁模式||
|blod|粗体|
|civis|隐藏光标|
|clear|清屏|
|cnorm|不隐藏光标|
|cup|移动光标到屏幕位置（x,y）|
|el|清除到行尾|
|ell|清除到行首|
|smoso|启动突出模式|
|rmso|停止突出模式|
|smull|开始下划线模式|
|rmull|结束下划线模式|
|sc|保存当前光标位置|
|rc|恢复光标到最后保存位置|
|sgr0|正常屏幕|
|rev|逆转|
|cols|列数目（数字输出）|
|it|tab设置宽度（数字输出）|
|lines|屏幕行数（屏幕行数）|
|chts|光标不可见（布尔输出）|
|hs|具有状态行（布尔输出）|

###### shell嵌入命令完整列表
|命令名|解释|
|---|---|
|：|空，永远返回为 true|
|. 从当前shell中执行操作
|break |退出for、 while、 until或case语句|
|cd |改变到当前目录|
|continue |执行循环的下一步|
|echo |反馈信息到标准输出|
|eval| 读取参数，执行结果命令|
|exec |执行命令，但不在当前 shell|
|exit |退出当前shell|
|export |导出变量，使当前shell可利用它|
|pwd |显示当前目录|
|read |从标准输入读取一行文本|
|readonly |使变量只读|
|return |退出函数并带有返回值|
|set |控制各种参数到标准输出的显示|
|shift |命令行参数向左偏移一个|
|test |评估条件表达式|
|times |显示shell运行过程的用户和系统时间|
|trap| 当捕获信号时运行指定命令|
|ulimit |显示或设置shell资源|
|umask |显示或设置缺省文件创建模式|
|unset |从shell内存中删除变量或函数|
|wait|等待直到子进程运行完毕，报告终止|
 