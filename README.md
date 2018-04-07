# emacs_config
This project for my emacs configuration . Most of them are copyed.
说明：

            1.为什么使用emacs和vim而不使用IDE？

                 大牛只用这两种工具，我等弱渣纯属装比。

            2.为什么弃vim而使用emacs ?

                 听说用vim的人JJ短，用emacs的人JJ长，所以我用emacs.

             （emacs和vim没有谁好谁差，完全是个人喜好，只有吊丝才会争论好坏，就像争论win与linux的人一样，你说win好，惜大牛及专业人士不用，他说linux好，有本事他卖出去啊。程序员的人生时间匆匆，陌生的人，路遇emacs与vim相争者，语言优劣相争者，技术与销售相争者，公知与五毛相争者，皆不可与其谋事，纠与小节而不休，思于嘴辩而不实，皆市井小民而非谋大事之人也）

            3.本文对emacs的说明有

                    -emacs如何加载配置文件。

                    -emacs配置文件如何书写与组织

                    -emacs的基本常规设置

                    -emacs的插件扩展配置说明

                    -emacs开发环境配置

            4.本文不会讲解emacs使用操作和lisp编程

        



闲话少说，入正题

一.emacs如何加载配置文件

      Emacs的核心部分是一个emacs lisp解释器，emacs lisp是lisp的一种方言版本。对emacs进行配置，其实就是lisp解释器对emacs lisp配置文件进行解释。而我们对emacs进行配置其实就是书写emacs lisp配置文件。所有的emacs配置文件都以.el为后缀(emacs lisp简写).emacs在“/home/你的用户名/”下有一个统一的配置文件.emacs，一般用于放置一些基本的配置内容和对其他配置文件的路径引用。

     

Emacs初始化文件 (Init file)
　　Emacs启动的时候通常会尝试从初始化文件中加载解释一系列Lisp程序。Emacs会查找~/.emacs，~/.emacs.el或者~/.emacs.d/init.el之一 (el实际上就是Emacs Lisp的缩写)。Emacs还可以有一个默认的初始化文件default.el，可以位于Emacs的任何标准的库搜索目录下，其中Emacs的库搜索目录由load-path变量定义。除此之外，Emacs还可以有一个对所有用户都有效的通用配置文件 (site-wide startup file)，称为site-start.el，也可以位于Emacs的任何标准的库搜索目录下。如果想要知道load-path的内容，可以在Emacs的*scratch*缓冲区中输入(print load-path)，然后在将光标移至右括号后，使用快捷键C-j (M-x eval-print-last-sexp) 来执行这条语句，就可以在当前缓冲区中打印出load-path的内容。

　　Emacs加载这些配置文件的顺序为:site-start.el，初始化文件，最后才是默认的初始化文件default.el。Emacs启动时，可以使用-q或–no-init-file选项来阻止Emacs加载初始化文件;如果初始化文件中将inhibit-default-init设置为t，那么Emacs不会加载default.el;最后，可以使用–no-site-file来阻止Emacs加载通用配置文件。



二.emacs配置文件的书写与组织

       编写emacs配置文件就是编写emacs lisp文件，语法遵循lisp语言，     而通常大多数配置选项为（emacs变量    emacs样式）

        如这种形式 （emacs variable      emacs face）

        常见的emacs variable有set-background-color,set-foreground-color,column-number-mode。。。等等，他们都代表特定的emacs属性样式，后面的是设定的值。

        例如：

[plain] view plain copy
                   (global-linum-mode 'linum-mode)  
                   ;;auto show row-num  
  
                   (partial-completion-mode 1)  
                   ;;use partial-completion  
  
                   (icomplete-mode 1)  
                    ;;use complete-completion                   
                   (display-time-mode 1);;  

         其中;;为注释符号，也可以用'来注释。

          所有的emacs配置文件中，只有.emacs不以.el为后缀，但它也是被以emacs lisp文件来解释的。可以把各种功能的配置写成独立的el文件，然后在其他文件中相互包含，最后在.emacs配置文件中包含这些el文件。即若在a.el中包含b.el，只需在.emacs中包含a.el即可，与c语言中include一样

          按照一般的习惯，.emacs文件中一般不会放太多的设置信息，一般放一些emacs的搜索路径的信息。

       

          以我的配置为例，.emacs放置在/home/fenice/下，而其他el文件放在/home/fenice/.emacs.d/lisps/_emacs中。

          因此首先要添加一个emacs的搜索路径

           ;;add plugin to emacs
           ;;设置插件加载路径
           ;;plugin directory ~/.emacs.d/lisp/_emacs/
            (add-to-list 'load-path "~/.emacs.d/lisp/_emacs/")

           将别人写好的插件也可以添加到路径，例如

           

[plain] view plain copy
            ;;;; 添加Emacs搜索路径  
            (add-to-list 'load-path "~/.emacs.d/lisps/_emacs")  
            (add-to-list 'load-path "~/.emacs.d/lisps/_emacs/install/ecb-2.40")  
            (add-to-list 'load-path "~/.emacs.d/lisps/_emacs/codepilot")  
            (add-to-list 'load-path "~/.emacs.d/lisps/_emacs/emacs-eclim")  
            (add-to-list 'load-path "~/.emacs.d/lisps/_emacs/icicles")  
            (add-to-list 'load-path "~/.emacs.d/lisps/_emacs/gnuserv")  
            (add-to-list 'load-path "~/.emacs.d/lisps/_emacs/install/cedet-1.0/common")              


            添加搜索路径后需要包含文件才能在emacs启动时加载

            例如包含xxx.el文件，可以(require 'xxx)或者(load "xxx.el")



三.emacs基本常规设置

            大家可以下载我的配置文件，基本设置放在/home/fenice/.emacs.d/lisps/_emacs/base.el中,即文件夹的_emacs下的base.el

[plain] view plain copy
;;=======================================================================  
;;Author:kevin_samuel(孙俊彦)  
;;Date:2013-7-30  
;;Update Date:2013-7-30  
;;=======================================================================  
  
;;设置字体，默认是Monospace  
;;set-defalut-font  
;;(set-default-font "Monospace")  
(set-default-font "-unknown-DejaVu Sans Mono-normal-normal-normal-*-13-*-*-*-m-0-iso10646-1")  ;;个人感觉Mono系字体适合程序（对普通青年）  


[plain] view plain copy
;;custom-set-variables  
;;set-color  
(set-background-color "black")  ;;use black-ground  
(set-foreground-color "white")  ;;use white-fore  
(set-face-foreground 'region "green") ;;  
(set-face-background 'region "blue") ;;  

[plain] view plain copy
;;===============================================================  
;;外观设置  
;;===============================================================  
;; 去掉滚动条  
(set-scroll-bar-mode nil)  
;;关闭开启画面  
(setq inhibit-startup-message t)  
;;禁用工具栏  
(tool-bar-mode nil)  
;;禁用菜单栏  
;;(menu-bar-mode nil)  
  
;;remove alert-bell  
(mouse-wheel-mode t)  
  
  
(global-linum-mode 'linum-mode)  
;;auto show row-num  
;;自动加载行号  
  
(partial-completion-mode 1)  
;;use partial-completion  
  
(icomplete-mode 1)  
;;use complete-completion  
  
  
;; 高亮当前行  
;;(require 'hl-line-settings)  
  
;; 可以把光标由方块变成一个小长条  
;;(require 'bar-cursor)  
;;======================================================================  
  
  
  
;;======================================================================  
;;状态栏  
;;======================================================================  
;;显示时间  
;;(display-time)  
(display-time-mode 1);;启用时间显示设置，在minibuffer上面的那个杠上  
(setq display-time-24hr-format t);;时间使用24小时制  
(setq display-time-day-and-date t);;时间显示包括日期和具体时间  
;;(setq display-time-use-mail-icon t);;时间栏旁边启用邮件设置  
;;(setq display-time-interval 10);;时间的变化频率，单位多少来着？  
  
  
;;显示列号  
(setq column-number-mode t)  
;;没列左边显示行号,按f3显示/隐藏行号  
(require 'setnu)  
(setnu-mode t)  
;;(global-set-key[f3] (quote setnu-mode))  
  
;;显示标题栏 %f 缓冲区完整路径 %p 页面百分数 %l 行号  
(setq frame-title-format "%f")  
  
;;=======================================================================  
;;缓冲区  
;;=====================================================================  
;;设定行距  
(setq default-line-spaceing 4)  
;;页宽  
(setq default-fill-column 60)  
;;缺省模式 text-mode  
;;(setq default-major-mode 'text-mode)  
;;设置删除记录  
(setq kill-ring-max 200)  
;;以空行结束  
;;(setq require-final-newline t)  
;;开启语法高亮。  
(global-font-lock-mode 1)  
;;高亮显示区域选择  
(transient-mark-mode t)  
;;页面平滑滚动,scroll-margin 3 靠近屏幕边沿3行开始滚动，正好可以看到上下文  
;;(setq scroll-margin 3 scroll-consrvatively 10000)  
;;高亮显示成对括号  
(show-paren-mode t)  
(setq show-paren-style 'parentheses)  
;;鼠标指针避光标  
(mouse-avoidance-mode 'animate)  
;;粘贴于光标处,而不是鼠标指针处  
(setq mouse-yank-at-point t)  
  
;;=======================================================================  
;;回显区  
;;=======================================================================  
;;闪屏报警  
(setq visible-bell t)  
;;使用y or n提问  
(fset 'yes-or-no-p 'y-or-n-p)  
;;锁定行高  
(setq resize-mini-windows nil)  
;;递归minibuffer  
(setq enable-recursive-minibuffers t)  
;;当使用M-x COMMAND后，过1秒显示该COMMAND绑定的键  
(setq suggest-key-bindings-1)   ;;默认？  
  
;;======================================================================  
;;编辑器的设定  
;;======================================================================  
;;不产生备份文件  
(setq make-backup-files nil)  
;;不生成临时文件  
(setq-default make-backup-files nil)  
;;只渲染当前屏幕语法高亮，加快显示速度  
(setq lazy-lock-defer-on-scrolling t)  
;;(setq font-lock-support-mode 'lazy-lock-mode)  
(setq font-lock-maximum-decoration t)  
;;将错误信息显示在回显区  
(condition-case err  
    (progn  
      (require 'xxx))  
  (error  
   (message "Can't load xxx-mode %s" (cdr err))))  
;;使用X剪贴板  
(setq x-select-enable-clipboard t)  
;;设定剪贴板的内容格式 适应Firefox  
(set-clipboard-coding-system 'ctext)  
  
;;设置TAB宽度为4  
(setq default-tab-width 4)   
;;以下设置缩进  
;;用tab缩进  
(setq indent-tabs-mode t)  
(setq c-indent-level 4)  
(setq c-continued-statement-offset 4)  
(setq c-brace-offset -4)  
(setq c-argdecl-indent 4)  
(setq c-label-offset -4)  
(setq c-basic-offset 4)  
(global-set-key "\C-m" 'reindent-then-newline-and-indent)  

这是我的base.el文件中的部分内容，就不全部贴上了。但是请记住，emacs所有的配置文件都是在启动时一下子全部加载进lisp解释器的，如果配置文件或插件太多，编辑器会变得很慢，曾经看过一篇把emacs配置成IDE开发环境的文章，里面加入了无数插件，我按照那篇文章配置出来的emacs要启动5秒钟（i5-3230   8g），正常情况下emacs都是瞬开。如果非要那么冗余，都不如去用IDE了，emacs看着就土鳖，难道不是么。


四.emacs的插件扩展配置说明

（1）

     我的emacs配置文件

        /home/fenice/.emacs

      我的emacs配置文件包

       /home/fenice/.emacs.d/lisps/_emacs

       主要功能调用如下（.emacs中）加载配置文件包

      

[plain] view plain copy
;;;;读取脚本  
;;=========================================  
;;bind the key  
(load "cykbd.el")  
  
;;basic setting  
(load "base.el")  
;;expand setting  
(load "cyexpand.el")  
;;IDE frame setting  
;;(load "addon.el")  
  
;;code setting为了编程的配置  
(load "cycode.el")  
;;========================================  


     我的配置文件中有一个cycode.el,为什么叫cy code.el呢，因为此文件是陈杨所写。一下有cy前缀的都是。

      调试功能，在cycode.el中添加：

[plain] view plain copy
;;==============================================================  
;;gdb-UI配置  
;;==============================================================  
(setq gdb-many-windows t)  
(load-library "multi-gud.el")  
(load-library "multi-gdb-ui.el")  

上面包含了两个从网上下载的调试插件，可以很方便的在emacs中进行gdb调试。
（2）安装cedet插件。需要从网上下载。cedet可以进行多种语言语法分析，自动补全，面向对象设计，UML图绘制等功能，还有工程管理功能，不过没用过。

          参考文章： http://www.cnblogs.com/logicbaby/archive/2011/10/19/2217253.html

         如果用我的配置文件，那么最好把/home/fenice/.emacs.d/lisps/_emacs/install中的内容清空，然后將cedet（我的共享文件里有压缩包）解压到install目录中，然后根据cedet内的INSTALL文件里的指示来安装。最后在.emacs或cycode.el中添加路径及配置信息。我的配置文件中目录为~/.emacs.d/lisps/_emacs/install/cedet-1.0/

如果使用，需要添加

[plain] view plain copy
;;==================================================  
;;cedet插件设置  
;;==================================================  
(add-to-list 'load-path "~/.emacs.d/lisps/_emacs/install/cedet-1.0/speedbar")  
(add-to-list 'load-path "~/.emacs.d/lisps/_emacs/install/cedet-1.0/eieio")  
(add-to-list 'load-path "~/.emacs.d/lisps/_emacs/install/cedet-1.0/semantic")  
;; Load CEDET.,从cedet的INSTALL中复制过来的  
;; See cedet/common/cedet.info for configuration details.  

最后在配置文件中添加（在.emacs中或者其他自定义文件，我的是cycode.el）
[plain] view plain copy
(load-file "~/.emacs.d/lisps/_emacs/install/cedet-1.0/common/cedet.el")  
  
  
;; Enable EDE (Project Management) features  
(global-ede-mode 1)  
  
;; Enable EDE for a pre-existing C++ project  
;; (ede-cpp-root-project "NAME" :file "~/myproject/Makefile")  
  
  
;; Enabling Semantic (code-parsing, smart completion) features  
;; Select one of the following:  
  
;; * This enables the database and idle reparse engines  
;;(semantic-load-enable-minimum-features)  
  
;; * This enables some tools useful for coding, such as summary mode  
;;   imenu support, and the semantic navigator  
;;(semantic-load-enable-code-helpers)  
  
;; * This enables even more coding tools such as intellisense mode  
;;   decoration mode, and stickyfunc mode (plus regular code helpers)  
;; (semantic-load-enable-gaudy-code-helpers)  
  
;; * This enables the use of Exuberent ctags if you have it installed.  
;;   If you use C++ templates or boost, you should NOT enable it.  
;; (semantic-load-enable-all-exuberent-ctags-support)  
;;   Or, use one of these two types of support.  
;;   Add support for new languges only via ctags.  
;; (semantic-load-enable-primary-exuberent-ctags-support)  
;;   Add support for using ctags as a backup parser.  
;; (semantic-load-enable-secondary-exuberent-ctags-support)  
  
;; Enable SRecode (Template management) minor-mode.  
 ;;(global-srecode-minor-mode 1)  
   
 ;;----------------------------------------------------------------------  
(semantic-load-enable-minimum-features)  
(semantic-load-enable-code-helpers)  
;;(semantic-load-enable-guady-code-helpers)  
;;(semantic-load-enable-excessive-code-helpers)  
(semantic-load-enable-semantic-debugging-helpers)  
  
(global-ede-mode t)  
;;代码折叠  
;;(require 'semantic-tag-folding nil 'noerror)  
(global-semantic-tag-folding-mode 1)  
;;折叠和打开整个buffer的所有代码  
(define-key semantic-tag-folding-mode-map (kbd "C--") 'semantic-tag-folding-fold-all)  
(define-key semantic-tag-folding-mode-map (kbd "C-=") 'semantic-tag-folding-show-all)  
;;折叠和打开单个buffer的所有代码  
(define-key semantic-tag-folding-mode-map (kbd "C-_") 'semantic-tag-folding-fold-block)  
(define-key semantic-tag-folding-mode-map (kbd "C-+") 'semantic-tag-folding-show-block)  
（3）为了使用方便可以使用ecb插件配合cedet的使用。不足之处就是这两个插件使emacs的启动速度慢了2秒。

  直接下载ecb到~/.emacs.d/lisps/emacs/install目录下解压即可，这是我的目录，也可以自定义目录，不过要注意修改路径

  然后加上

[plain] view plain copy
;;==============================================================  
;;ecb配置  
;;==============================================================  
;;(require 'ecb)  
;;开启ecb用,M-x:ecb-activate  
(require 'ecb-autoloads)  
;;自动启动ecb并且不显示每日提示  
;;(require 'ecb)  
;;(setq ecb-auto-activate t)  
(setq ecb-tip-of-the-day nil)  
  
(require 'cc-mode)  
(c-set-offset 'inline-open 0)  
(c-set-offset 'friend '-)  
(c-set-offset 'substatement-open 0)  

（4）真正的自动补全功能，就像windos下visual studio装上visual assist后的补全。
 这里需要两个插件—auto-complete和yasnippet。先安装 auto-complete，下载后放到~/.emacs.d/lisps/emacs/install目录中解压（也可以用自己的目录，注意修改路径），然后进入解压后的目录，然后输入make命令即可；下面安装yasnippet，下载 后放到~/.emacs.d/lisps/emacs/install目录，然后解压即可；下面是几句关于这两个插件的配置：

[plain] view plain copy
;;==========================================================  
;;加载cscope  
;;==========================================================  
(require 'xcscope)  
  
;;==========================================================  
;;YASnippet的配置  
;;==========================================================  
;;太强大了，强大的模板功能  
(require 'yasnippet)    ;;not yasnippet-bundle  
(yas/initialize)  
(yas/load-directory "~/.emacs.d/lisps/_emacs/install/yasnippet-0.6.1c/snippets")  
  
;;自动补全  
;;(require 'auto-complete-config)  
;;(ac-config-default)  
 （5）键绑定功能

         即设置emacs的快捷键

        我的键绑定全部设置在cykbd.el文件中，以下是部分内容包括编译和调试

[html] view plain copy
(global-set-key [f1] 'manual-entry)  
(global-set-key [C-f1] 'info )  
  
;;f3为查找字符串,alt+f3关闭当前缓冲区  
(global-set-key [f3] 'grep-find)  
(global-set-key [M-f3] 'kill-this-buffer)  
  
;;.emacs中设一个speedbar的快捷键  
(global-set-key [(f4)] 'speedbar-get-focus)  
;;ctrl-f4,激活,ecb  
(global-set-key [C-f4] 'ecb-activate)  
  
;;F5显示/隐藏工具栏 方便调试  
(global-set-key [f5] 'tool-bar-mode)  
;;ctrl-F5显示/隐藏菜单栏 ;; M-x menu-bar-open  
(global-set-key [C-f5] 'menu-bar-mode)  
  
(global-set-key [f6] 'gdb)  
  
;;  C-f7, 设置编译命令; f7, 保存所有文件然后编译当前窗口文件  
(defun du-onekey-compile ()  
  "Save buffers and start compile"  
  (interactive)  
  (save-some-buffers t)  
  (switch-to-buffer-other-window "*compilation*")  
  (compile compile-command))  


（6）其他很多搜集的点el扩展文件均用cyexpand.el包含。不过其中有很多用不到的被我注掉了。如果不需要可以在.emacs中把(load "cyexpand.el")注掉。

  （7）我的配置文件中有一个addon.el，这个配置插件可以将emacs的外观变得更像IDE，但因实在太丑陋，所以我在.emacs把它注掉了。



如果使用我的配置文件 http://pan.baidu.com/share/link?shareid=4196458904&uk=3708780105

下载Emacs_setting.tar.gz，按照readme.txt指示即可，略有麻烦，可以细心摸索。将samuel_emacs重命名为.emacs放到/home/yourusername/下

将_emacs.d.tar.gz解压后重命名为.emacs.d后放入/home/yourusername/下，其他插件配置按照readme.txt



注意用不到的配置文件包含全部要注掉，不然emacs真的会像某些IDE一样臃肿。相对来说vim配置成IDE 要简单一点，吴垠大神曾经写过一篇把vim配置乘IDE的文章，不过那个更复杂。
