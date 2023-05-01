---
title: tmux常用命令
categories:
  - Linux
abbrlink: c3f7601e
date: 2023-04-17 23:59:27
tags:
---
## 功能

1. 分屏
2. 允许断开terminal连接后，继续运行进程

## 结构

一个tmux可以包含多个session，一个session可以包含多个window，一个window可以包含多个pane

示例：

tmux:

&emsp;&emsp;session0:

&emsp;&emsp;&emsp;&emsp;window0:

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;pane0

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;pane1

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;pane2

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;...

&emsp;&emsp;&emsp;&emsp;window1:

&emsp;&emsp;&emsp;&emsp;window2:

&emsp;&emsp;&emsp;&emsp;...

&emsp;&emsp;session1

&emsp;&emsp;session2

&emsp;&emsp;...

## 常用命令

1. tmux：新建一个session，其中包含一个window，window包含一个pane，pane里打开了一个shell对话框
2. 按下`Ctrl + a`后手指松开，然后按`%`:将当前pane左右平分成两个pane
3. 按下`Ctrl + a`后手指松开，然后按`”`:将当前pane上下平分成两个pane
4. `Ctrl + d`: 关闭当前pane；如果当前window的所有pane均已关闭，则自动关闭window；如果当前session的所有window均已关闭，则自动关闭session
5. 鼠标点集可以选pane
6. 按下`Ctrl + a`后手指松开，然后按下方向键：选择相邻的pane
7. 鼠标拖动pane之间的分割线，可以调整分割线的位置
8. 按下`Ctrl + a`的同时按方向键，可以调整pane之间分割线的位置（鼠标按着分界线也可以位置）
9. 按下`Ctrl + a`后手指松开，然后按`d`：挂起当前session
10. 按下`Ctrl + a`后手指松开，然后按`z`：将当前pane全屏/取消全屏
11. `tmux a`: 打开之前挂起的session
12. 按下`Ctrl + a`后手指松开，然后按`s`：选择其他session
    
    方向键 ——— 上：选择上一项 session/window/pane
    
    方向键 ——— 下：选择下一项 session/window/pane
    
    方向键 ——— 右：展开当前项 session/window
    
    方向键 ——— 左：闭合当前项 session/window
    
13. 按下`Ctrl + a`后手指松开，然后按`c`：在当前session种创建一个新的window
14. 按下`Ctrl + a`后手指松开，然后按`w`：选择其他window，操作方法与（12）完全相同
15. 按下`Ctrl + a`后手指松开，然后按`PageUp`：翻阅当前pane的内容
16. 鼠标滚轮：翻阅当前pane内的内容
17. 在tmux中选中文本时，需要按住`shift`键
18. tmux中复制/粘贴文本的通用方式
    1. 按下`Ctrl + a`后松开手指，然后按`[`
    2. 用鼠标选中文本，被选中的文本会被自动复制到tmux的剪贴板
    3. 按下`Ctrl + a`后松开手指，然后按`]`，会将剪贴板中的内容粘贴到光标处