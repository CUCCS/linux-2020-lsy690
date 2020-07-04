# 第二章：（实验）
## From GUI to CLI

## 软件环境
 Ubuntu 18.04 Server 64bit

## 在asciinema注册一个账号，并在本地安装配置好asciinema
 确保本地已经完成asciinema auth，并在asciinema成功关联了本地账号和在线账号
### 安装
 sudo apt-add-repository ppa:zanchey/asciinema
 sudo apt-get update
 sude apt-get install asciinema
 配置
 asciinema auth

## 上传本人亲自动手完成的vimtutor操作全程录像
 在自己的github仓库上新建markdown格式纯文本文件附上asciinema的分享URL
 （提醒 避免在终端操作录像过程中暴漏密码、个人隐私等任何机密数据）

 屏幕录制
  asciinema rec

  录制结束
  Ctrl+d

  视频网址

  [vimtutor操作录像](https://asciinema.org/connect/e036dcb9-f8d0-4466-b30a-a1db688669ff)
 
## vimtutor完成后的自查清单
你了解vim有哪几种工作模式？
 一般模式
 编辑模式
 命令模式

Normal模式下，从当前行开始，一次向下移动光标10行的操作方法？如何快速移动到文件开始行和结束行？如何快速跳转到文件中的第N行？
 10j ；gg到开始行，G到结束行 ；NG

Normal模式下，如何删除单个字符、单个单词、从当前光标位置一直删除到行尾、单行、当前行开始向下数N行？
 单个字符：x ；单个单词: dw ; 删到行尾：d$ ; 单行：dd ；当前行向下行:  ndd ;

如何在vim中快速插入N个空行？如何在vim中快速输入80个-？
 Ni ENTER ; 80i-

如何撤销最近一次编辑操作？如何重做最近一次被撤销的操作？
 u ； ctrl+r

vim中如何实现剪切粘贴单个字符？单个单词？单行？如何实现相似的复制粘贴操作呢？
 单个字符：剪切x，p粘贴 ；单个单词：剪切dw，p粘贴 ；单行：剪切dd，p粘贴 ；复制：v 选中文本 y(yank)，粘贴：p

为了编辑一段文本你能想到哪几种操作方式（按键序列）？
 i ，I , a ，A , o ,O , r ,R , c ,ce

查看当前正在编辑的文件名的方法？查看当前光标所在行的行号的方法？
 ctrl+g 

在文件中进行关键词搜索你会哪些方法？如何设置忽略大小写的情况下进行匹配搜索？如何将匹配的搜索结果进行高亮显示？如何对匹配到的关键词进行批量替换？
 关键词搜索： /word  ；忽略大小写: set ic ; 高亮显示: set hls  ;  批量替换： %s/old/new/g

在文件中最近编辑过的位置来回快速跳转的方法？
 ctrl o ; ctrl i

如何把光标定位到各种括号的匹配项？例如：找到(, [, or {对应匹配的),], or }
 %

在不退出vim的情况下执行一个外部程序的方法？
 command

如何使用vim的内置帮助系统来查询一个内置默认快捷键的使用方法？如何在两个不同的分屏窗口中移动光标？
 help  快捷键  ； ctrl+w