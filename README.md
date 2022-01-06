2021年工作总结与2022年的计划
工位流程概述
问题反馈


工作 总结


工作总结：DBC、PT、CW、MC
DBC：
DBC是Download and Board Check的简称，DBC工位是SMT之后的第一个测试工位，主要完成单板的软件加载，单板条码写入，以及加载完成后对单板进行基本测试。包括：单板信息、加载前漏电流、系统加载、电池电压校准等。这些测试会涵盖大部分基本功能，可以拦截较多不良单板，便于提早发现问题（器件损坏或漏焊），以免整机测试发现问题后返工成本较高。
DBC也是第一个与UFS强相关的工位，主要的相关行为就是烧片版本的加载。
PT：
PT是整机测试的重要工位之一，PT工位是Power Test简称，即功耗测试。关键测试项为：开机前漏电流、开机电流、充电电流、待机电流、特殊场景待机电流、关机漏电流等。待机电流为重中之重的故障拦截途径。
DBC、PT是烧片软件版本测试项。
CW、MC是定制版本软件测试项。


DIAG指令、AT指令：
通过串口通信软件向机器发送指令，分析返回值判断机器是否正常。
可以通过cmd_list_table.c找AT指令相关代码和diag_fac.h找DIAG相关代码。
4BC977EE0000000042D27E                    查看开机启动状态
4BC980E4BC945EEE00000000300E7E  查询ddr、emmc、cpu信息 
4BC96AEE000000002DA57E             	    读取器件型号
AT^DEVBOOTSTATE?  		     查看机器是否正常启动 
AT^BSN?			     查询BSN 
AT^SN?			     查询SN 

如果遇到指令返回值异常，
需要先查询机器log（hilogs），找到问题报错内容，分析下一步问题方向。
如果与软件无关，需要把问题转到相关岗位人员；否则需要排查指令出错的原因。


学习关于python语言的知识，写出自动化脚本，提高工作效率。
能够帮助产线解决问题。
工作能力提高一点，了解其他岗位。


问题反馈：产线
发送给产线的问题解决方向邮件，没有问题解决情况的邮件。
自身优劣势：
不限制在自己的的岗位，学习其他岗位情况。



WIFIonly熔丝校验问题案例
1、经过熔丝烧入步骤，向机器发指令：4B C9 48 EE 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 fe cb 7e，结果：手机返回全0，
正常情况是4b c9 48 ee 00 00 00 30 30 30 00 15 00 6F 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 2E 49 7E

2、分析hiapplogcat
hw_diag_server: Lib:oem_qmi_cmd:Build socket connection failed,error info:Connection refused 
[Diag_sec]: [HwdiagQfuse,00919]HwdiagQfuse: qmi read result: 0, qfuseData[0]:0x0, qfuseData[1]:0x0

0x0 表示执行的是LTE的函数，是缺少WiFiOnly判断导致连接失败。 
3、添加判断函数
if (IsWiFiOnlyProduct()) {
            opResult = GetFuseStatusByQsee(rspPkt->rdata, QFUSE_REGISTER_COUNT);
        } else {
            opResult = GetFuseStatusByModem(rspPkt->rdata, QFUSE_REGISTER_COUNT);
        }

        DIAG_SEC_LOGE("%s: qfuse read result: %d, qfuseData[0]:0x%x, qfuseData[1]:0x%x \n",
            __func__, ret, rspPkt->rdata[0], rspPkt->rdata[1]);

GetFuseStatusByQsee函数为了判断出wifi产品，GetFuseStatusByModem函数为了判断出lte产品。
4、向代码中加入IsWiFiOnlyProduct判断条件，熔丝返回值正常，成功。最后华为人员上库，问题解决。

出现问题
分析装备log
确定解决方向
解决问题
下一个问题












# CoCoin 是一个详尽的个人财务和记账解决方案，运行在一个干净漂亮的用户界面之上。
如果你想了解如何正确管理大量的用户数据，并从这些数据中绘制漂亮的图表，制作一些很酷的自定义视图，那么这个开源仓库就是为你而设的。
https://github.com/Nightonke/CoCoin
# Timber 是一款设计精美、功能齐全的 Android 音乐播放器。如果你想要建立自己的音乐播放器或任何音乐相关的应用程序，那么这是你需要查看的项目。
该项目规模相当大，正处于密集开发状态。对于初学者来说，掌握所有代码可能会有点困难，但对于任何中级或高级 Android 开发人员来说，这应该是非常有趣的。
https://github.com/naman14/Timber
# 这个应用的功能相当简单，就是列出所有流行电影的预告片和评论，但是这个项目真正有趣的地方是它的实现方式。
这个应用展示了一些非常棒的开发项目，像 MVP、Bob 的 Clean Architecture、Dagger 2 实现的依赖注入，和 RxJava 。
这个应用程序非常简单，但实现方式非常棒，绝对值得一看。
https://github.com/esoxjem/MovieGuide
# 执行Git命令时出现各种 SSL certificate problem 的解决办法
https://blog.csdn.net/officercat/article/details/39989837
# Android Studio怎么从GitHub克隆下载代码
https://blog.csdn.net/binjianliu/article/details/78052613
# Android studio build.gradle 配置
前言
最近众多同事需要用到android studio进行二进制apk的编译工作，苦于不能在线更新项目的依赖库。解决方案来了！！！！！

配置内容
配置位置
根目录下的build.gradle

配置方式
buildscript {
   repositories {
       google()
       mavenCentral() // maven 远程库
       maven { url 'http://mirrors.tools.huawei.com/maven/' } //华为自己的镜像库
       jcenter{
           url "http://jcenter.bintray.com" //因为https代理老是失败，这里改为http的地址
      }
  }
}
allprojects {
   repositories {
       google()
       mavenCentral() // maven 远程库
       maven { url 'http://mirrors.tools.huawei.com/maven/' }
       jcenter{
           url "http://jcenter.bintray.com"
      }
  }
}

# BaronZ88/MinimalistWeather
https://github.com/BaronZ88/MinimalistWeather
# 魅族天气API
https://blog.csdn.net/qq_24810411/article/details/89175825


# Welcome to GitHub

Welcome to GitHub—where millions of developers work together on software. Ready to get started? Let’s learn how this all works by building and publishing your first GitHub Pages website!

## Repositories

Right now, we’re in your first GitHub **repository**. A repository is like a folder or storage space for your project. Your project's repository contains all its files such as code, documentation, images, and more. It also tracks every change that you—or your collaborators—make to each file, so you can always go back to previous versions of your project if you make any mistakes.

This repository contains three important files: The HTML code for your first website on GitHub, the CSS stylesheet that decorates your website with colors and fonts, and the **README** file. It also contains an image folder, with one image file.

## Describe your project

You are currently viewing your project's **README** file. **_README_** files are like cover pages or elevator pitches for your project. They are written in plain text or [Markdown language](https://guides.github.com/features/mastering-markdown/), and usually include a paragraph describing the project, directions on how to use it, who authored it, and more.

[Learn more about READMEs](https://help.github.com/en/articles/about-readmes)

## Your first website

**GitHub Pages** is a free and easy way to create a website using the code that lives in your GitHub repositories. You can use GitHub Pages to build a portfolio of your work, create a personal website, or share a fun project that you coded with the world. GitHub Pages is automatically enabled in this repository, but when you create new repositories in the future, the steps to launch a GitHub Pages website will be slightly different.

[Learn more about GitHub Pages](https://pages.github.com/)

## Rename this repository to publish your site

We've already set-up a GitHub Pages website for you, based on your personal username. This repository is called `hello-world`, but you'll rename it to: `username.github.io`, to match your website's URL address. If the first part of the repository doesn’t exactly match your username, it won’t work, so make sure to get it right.

Let's get started! To update this repository’s name, click the `Settings` tab on this page. This will take you to your repository’s settings page. 

![repo-settings-image](https://user-images.githubusercontent.com/18093541/63130482-99e6ad80-bf88-11e9-99a1-d3cf1660b47e.png)

Under the **Repository Name** heading, type: `username.github.io`, where username is your username on GitHub. Then click **Rename**—and that’s it. When you’re done, click your repository name or browser’s back button to return to this page.

<img width="1039" alt="rename_screenshot" src="https://user-images.githubusercontent.com/18093541/63129466-956cc580-bf85-11e9-92d8-b028dd483fa5.png">

Once you click **Rename**, your website will automatically be published at: https://your-username.github.io/. The HTML file—called `index.html`—is rendered as the home page and you'll be making changes to this file in the next step.

Congratulations! You just launched your first GitHub Pages website. It's now live to share with the entire world

## Making your first edit

When you make any change to any file in your project, you’re making a **commit**. If you fix a typo, update a filename, or edit your code, you can add it to GitHub as a commit. Your commits represent your project’s entire history—and they’re all saved in your project’s repository.

With each commit, you have the opportunity to write a **commit message**, a short, meaningful comment describing the change you’re making to a file. So you always know exactly what changed, no matter when you return to a commit.

## Practice: Customize your first GitHub website by writing HTML code

Want to edit the site you just published? Let’s practice commits by introducing yourself in your `index.html` file. Don’t worry about getting it right the first time—you can always build on your introduction later.

Let’s start with this template:

```
<p>Hello World! I’m [username]. This is my website!</p>
```

To add your introduction, copy our template and click the edit pencil icon at the top right hand corner of the `index.html` file.

<img width="997" alt="edit-this-file" src="https://user-images.githubusercontent.com/18093541/63131820-0794d880-bf8d-11e9-8b3d-c096355e9389.png">


Delete this placeholder line:

```
<p>Welcome to your first GitHub Pages website!</p>
```

Then, paste the template to line 15 and fill in the blanks.

<img width="1032" alt="edit-githuboctocat-index" src="https://user-images.githubusercontent.com/18093541/63132339-c3a2d300-bf8e-11e9-8222-59c2702f6c42.png">


When you’re done, scroll down to the `Commit changes` section near the bottom of the edit page. Add a short message explaining your change, like "Add my introduction", then click `Commit changes`.


<img width="1030" alt="add-my-username" src="https://user-images.githubusercontent.com/18093541/63131801-efbd5480-bf8c-11e9-9806-89273f027d16.png">

Once you click `Commit changes`, your changes will automatically be published on your GitHub Pages website. Refresh the page to see your new changes live in action.

:tada: You just made your first commit! :tada:

## Extra Credit: Keep on building!

Change the placeholder Octocat gif on your GitHub Pages website by [creating your own personal Octocat emoji](https://myoctocat.com/build-your-octocat/) or [choose a different Octocat gif from our logo library here](https://octodex.github.com/). Add that image to line 12 of your `index.html` file, in place of the `<img src=` link.

Want to add even more code and fun styles to your GitHub Pages website? [Follow these instructions](https://github.com/github/personal-website) to build a fully-fledged static website.

![octocat](./images/create-octocat.png)

## Everything you need to know about GitHub

Getting started is the hardest part. If there’s anything you’d like to know as you get started with GitHub, try searching [GitHub Help](https://help.github.com). Our documentation has tutorials on everything from changing your repository settings to configuring GitHub from your command line.
