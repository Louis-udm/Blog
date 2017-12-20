# Python 学习
***

## 快捷键
* 格式化 ctrl+shift+r
* 新增一行 cmd+entre, cmd+shift+entre
* 复制并剪切当前行 cmd+shift+d,cmd+x
* 选中括号中内容 ctrl+shift+m
* 选中光标所在行 cmd+l
* 多选关键字 cmd+d,跳过cmd+k+d,取消前一个cmd+u
* 多行产生光标，cmd+l选中多行后，cmd+shift+l
* 替换 option+cmd+f
* 撤销cmd+z, 重做cmd+y


## 设置
* 缩进用4个空格 preferences->setting, add:
    "tab_size": 4,
* 支持python3, 支持中文, tool->build system->new bulid system
{
  "env": {"PYTHONIOENCODING": "utf8"},
  "cmd": ["/usr/local/bin/python3", "-u", "$file"],
  "file_regex": "^[ ]*File \"(…*?)\", line ([0–9]*)",
  "selector": "source.python"
}