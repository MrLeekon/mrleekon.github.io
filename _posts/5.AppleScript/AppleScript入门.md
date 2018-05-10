# <AppleScript>AppleScript入门
```applescript
display dialog the ["Hello AppleScript."] buttons {"1", "2"} default button 2 giving up after 2
```
display dialog the ["Hello AppleScript."]
对话框显示信息

buttons {"1", "2"} default button 2 
两个buttons 默认为botton2

giving up after 2
不选择的在两秒后放弃

```applescript
tell application "Finder" to open home
--让Finder打开Home

tell application "Finder" to set current view of Finder window 1 to list view
--让Finder最前面的Finder window的查看模式设为列表

tell application "Finder" to set the target of the Finder window 1 to startup disk
--让Finder的最前面的Finder window导航到系统根目录下
tell application "Finder" to close Finder window 1
--让Finder关闭最前面的Finder window
```

```applescript
tell application "Finder"	set greeting_text to text returned of (display dialog "请输入欢迎语：" default answer "你好，Applescript。")	display dialog the [greeting_text] end tell
--greeting_text为变量
```

```applescript
--猜数字循环练习
set rand_num to (random number from 1 to 10) as textrepeat	set result to text returned of (display dialog "请输入 1-10 直接的数字" default answer "")	if result = rand_num then		display dialog "回答正确，答案为：" & rand_num		exit repeat	end ifend repeat
```

```applescript
--从1到5循环
repeat with i from 1 to 5
    display dialog i
end repeat

--从5到1循环
repeat with i from 5 to 1 by -1
    display dialog i
end repeat

--while循环的实践 从1到5
set x to 0
repeat while x is not 6
    set x to x+1
    display dialog x
end repeat

--until循环的实践 从1到5
set x to 0
repeat until x is 5
    set x to x+1
    display dialog x
end repeat
```

