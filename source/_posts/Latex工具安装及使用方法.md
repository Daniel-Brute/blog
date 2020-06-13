---
title: Latex工具安装及简单的使用方法
date: 2020-04-25 13:54:42
categories: 
- 工具
tags:
- latex
---

# 1 软件下载及安装

## 1.1 下载地址

latex: https://www.latex-project.org/get/#tex-distributions

texstudio: http://texstudio.sourceforge.net/

## 1.2 安装说明

latex作为文档地格式编辑器，在下载地址中选择一个下载即可。但是安装latex时，需要在安装提示中显示地选择“默认A4纸张”。

texstudio是图像化地编辑界面，提供了简便地编辑文本编辑方式。

![1](\./Latex工具安装及使用方法/texstudio.png)

Tools：编译工具所在选项

LaTeX：texstudio自带的文本命令行

Options：配置默认命令配置按钮

ref.bib：参考文献配置文件

# 2 Latex导入宏包

导入宏包的方式

```
\usepackage{宏包名}
```

# 3 常用的使用方法

## 3.1 设置主页

```
\begin{titlepage}
	\centering
	\vspace*{\stretch{1}}
	{\sffamily\fontsize{50}{60} 标题}\\
	\vspace{\stretch{1}}
	{\Large 作者}\\
	\vspace{\stretch{3}}
	{\today}
\end{titlepage}
```

## 3.2 设置目录

使用一下命令可以自动生成目录

```
\tableofcontents
```

## 3.3 设置标题

- 设置章节主标题

```
\chapter{主题}
```

- 设置一级标题

```
\section{标题}
```

- 设置段二级标题

```
\subsection{标题}
```

- 设置段三级标题

```
\subsubsection{标题}
```

- 设置段落标题

```
\paragraph{段落标题}
```

## 3.4 设置内容

- 设置表格

```
\begin{center}
	\begin{tabular}{|l|c|r|} %l(left)居左显示 r(right)居右显示 c居中显示
		\hline 
		Name & Steve & Bill \\
		\hline  
		Matlab & Mathmatica & Maple \\
		\hline 
	\end{tabular}
\end{center}
```

- 设置图片

添加宏包

```
\usepackage{graphicx}
\usepackage{subfigure}
\usepackage{epstopdf} 
```

添加图片

```
\begin{figure}[!t]
	\centering
	\includegraphics[width=3.5in]{aa.png}
	\caption{Time-interleaved ADC system}
	\label{fig1}
\end{figure}
```

- 设置算法

导入宏包

```
\usepackage{algorithm2e,setspace}
```

添加算法

```
\begin{algorithm}
	\setstretch{1.35}
	\SetAlgoLined
	\KwData{this text}
	\KwResult{how to write algorithm with \LaTeX2e }
	initialization\;
	\While{not at end of this document}{
		read current\;
		\eIf{understand}{
			go to next section\;
			current section becomes this one\;
		}{
			go back to the beginning of current section\;
		}
	}
	\caption{How to write algorithms}
\end{algorithm}
```

- 设置引用（BibTex）

导入宏包

```
\usepackage{cite}
```

bib文件
```
@article{name01, 
	author = {作者, 多个作者用 and 连接}, 
	title = {标题}, 
	journal = {期刊名}, 
	volume = {卷号}, 
	number = {页码}, 
	year = {年份}, 
	abstract = {摘要, 这个主要是引用的时候自己参考的, 这一行不是必须的} 
} 

@book{name02, 
	author ="作者", 
	year="年份", 
	title="书名", 
	publisher ="出版社名称" 
}
```

引用方式
```
\bibliographystyle{unsrt} % unsrt按照引用地先后顺序排序
你好:\cite{name01}，世界：\cite{name02}
\bibliography{bib文件名} 
```

## 3.5 设置文本格式

- 设置缩进

```
\usepackage{indentfirst}
```

使用宏包做统一缩进，凡是新段落则自动缩进

- 设置换行

```
\newline
```

- 设置分页

```
\newpage
```

- 设置页眉和页脚

导入宏包
```
\usepackage{lastpage}
\usepackage{fancyhdr}
\pagestyle{fancy}
\lhead{}
\chead{}
\rhead{}
\lfoot{}
\cfoot{}
\rfoot{\thepage \ / \pageref{LastPage}} %当前页 of 总页数
\renewcommand{\headrulewidth}{0pt} %改为0pt即可去掉页眉下面的横线
\renewcommand{\footrulewidth}{0pt} %改为0pt即可去掉页脚上面的横线
```

使用方式
```
\thispagestyle{empty} %目录页不显示页码
\setcounter{page}{1} %从下面开始编页码
```