title: iOS ViewController入门
date: 2016-03-15 09:22:41
tags: iOS
comments: true
categories: iOS
---

ViewController是App最基础的结构, 一个App由1到多个ViewController组成, 
ViewController扮演`MVC`模式中的`C(Controller)`, 展示并管理子视图树结构.
通过子类化`UIViewController`来实现实现自定义数据模型和视图之间的逻辑和通信

## ViewController 分类

* Content ViewController 直接管理内容子View显示
* Container ViewController 以不同的方式管理和显示子ViewController

## ViewController 展示和过渡

`presenting view controller` vs `presented view controller`

### ViewController 显示

* segue 
* showViewController:sender:
* presentViewControllerAnimated:completion:

### ViewController 关闭

在`presenting view controller`中调用`dismissViewControllerAnimated:completion:`关闭一个`presented view controller`.

`presented view controller`返回数据用`delegation`模式, 参考`UIImagePickerController`和`UIImagePickerControllerDelegate`