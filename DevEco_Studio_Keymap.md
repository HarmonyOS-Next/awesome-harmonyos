# 鸿蒙开发 HarmonyOS DevEco Studio 常用快捷键



## 前言

做 **HarmonyOS** 鸿蒙开发离不开 [DevEco Studio](https://developer.harmonyos.com/cn/develop/deveco-studio#download) 开发工具， [DevEco Studio 是基于 IntelliJ IDEA Community 开源版本打造](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/deveco_overview-0000001053582387-V3)，默认的快捷键其实继承于 IntelliJ IDEA 。

熟悉 DevEco Studio 的快捷键能提升开发效率和开发体验。

下面将详细列出 DevEco Studio 一些常用的快捷键，由[黑马程序员](https://space.bilibili.com/37974444)整理，希望对大家有帮助，也欢迎大家补充或修正。

## 一、编辑



| **快捷键(Win)**                 | **快捷键（Mac）**     | **英文说明**                                  | **中文说明**                                                 |
| ------------------------------- | --------------------- | --------------------------------------------- | ------------------------------------------------------------ |
| **Alt + J**                     | **^ + G**             | Find Next / Add Selection for Next Occurrence | 选择相同词，设置多个光标。（常用，批量选中）                 |
| **Alt + 1**                     | **⌘ + 1**             | Project                                       | 显示 或 隐藏 项目区。（常用）                                |
| **Alt + 7**                     | **⌘ + 7**             | Structure                                     | 显示 或 隐藏 代码结构树。（常用）                            |
| **Ctrl + E**                    | **⌘ + E**             | Recent Files                                  | 最近的文件（常用，切换文件、切换面板，强烈推荐）             |
| **Ctrl + P**                    | **⌘ + P**             | Parameter Info                                | 在某个方法小括号中，调用该按键后，会展示出这个方法的调用参数列表信息。（常用，类型提示神器） |
| **Ctrl + Q**                    | 无                    | Quick Documentation                           | 展示组件的 API 说明文档。（常用，查文档神器）                |
| **Ctrl + Alt + L**              | **⌥ + ⌘ + L**         | Reformat Code                                 | 格式化代码 。（常用）                                        |
| **Ctrl + Shift +  Enter**       | **Shift + ⌘ + Enter** | Complete Current Statement                    | 换行输入。（常用，换行添加新属性）                           |
| **Ctrl + B / Ctrl + 单击**      | **⌘ + B / ⌘ + 单击**  | Go to Declaration or Usages                   | 跳转源码、跳转文件。（常用，强烈推荐）                       |
| **Ctrl + Alt + T**              | **⌥ + ⌘ + T**         | Surround with…                                | 自动生成具有环绕性质的代码。（常用，生成 `if…else,try…catch` 等代码块） |
| **Ctrl + /**                    | **⌘ + /**             | Comment with Line Comment                     | 单行注释 `//`（常用）                                        |
| **Ctrl + Shift + /**            | **⌥ + ⌘ + /**         | Comment with Block Comment                    | 代码块注释 `/**/`，与 Ctrl +  /  的区别是只会在代码块的开头与结尾添加注释符号（常用） |
| **Tab / Shift + Tab**           | **Tab / Shift + Tab** | Indent/Unindent Selected Lines                | 缩进或者不缩进一次所选择的代码段。（常用）                   |
| **Ctrl + X**                    | **⌘ + X**             | Cut                                           | 剪切选中代码、剪切行、删除行。 （常用）                      |
| **Ctrl + C**                    | **⌘ + C**             | Copy                                          | 复制选中代码、复制行。 （常用）                              |
| **Ctrl + D**                    | **⌘ + D**             | Duplicate Line or Selection                   | 复印选中代码、复印行。（常用）                               |
| **Ctrl + V**                    | **⌘ + V**             | Paste                                         | 粘贴代码。（常用）                                           |
| **Ctrl + Shift + V**            | **Shift + ⌘ + V**     | Paste from History...                         | 剪贴板，复制过的内容都在这里。（常用）                       |
| **Ctrl + Z**                    | **⌘ + Z**             | Undo                                          | 撤消。（常用）                                               |
| **Ctrl + Shift + Z / Ctrl + Y** | **Shift + ⌘ + Z**     | Redo                                          | 重做。                                                       |
| **Ctrl + Shift + J**            | **Shift + ^ + J**     | Join Lines                                    | 把下一行的代码接续到当前的代码行。（常用，合并行）           |
| **Ctrl + Shift + U**            | **Shift + ⌘ + J**     | Toggle Case                                   | 所选择的内容进行大小写转换。（常用）                         |
| **Ctrl + Shift + ↑ / ↓**        | **Shift + ⌥ + ↑ / ↓** | Move Line Up / Down                           | 将选定语句向上或向下移动一行。                               |
| **Ctrl + (+/-)**                | **⌘ + (+/-)**         | Expand/Collapse                               | 折叠或展开代码。 （常用，代码量多时比较实用）                |
| **Shift + F6**                  | **Shift + F6**        | Refator Rename                                | 重构修改命名。（常用，能同步更新路径、变量名、函数名的重命名） |
| **Ctrl + F4**                   | **⌘ + W**             | Close Tab                                     | 关闭当前标签页。（建议：Win 系统操作不方便，修改快捷键为 Ctrl + W 操作起来更顺手） |
| **Ctrl + W**                    | 无                    | Extend Selection                              | 选中当前光标所在代码块，多次触发会逐级变大。（不常用，Win 系统建议 Ctrl +W 修改为关闭当前标签页） |



##  二、查找或替换



| **快捷键(Win)**      | **快捷键(Mac)**   | **英文说明**        | **中文说明**                           |
| -------------------- | ----------------- | ------------------- | -------------------------------------- |
| **Ctrl + F**         | **⌘ + F**         | Find...             | 文件内查找，还支持正则表达式。（常用） |
| **Ctrl + Shift + F** | **Shift + ⌘ + F** | Find  in Files...   | 项目中查找。（常用）                   |
| **Ctrl + R**         | **⌘ + R**         | Replace...          | 文件内替换。（常用）                   |
| **Ctrl + Shift + R** | **Shift + ⌘ + R** | Replace in Files... | 项目中替换。（常用）                   |
| **Shift + Shift**    | **Shift + Shift** | Fast  Find          | 快速查找（常用）                       |



## 三、编译与运行



| **快捷键(Win)**       | **快捷键(Mac)** | **英文说明**                   | **中文说明**                                                 |
| --------------------- | --------------- | ------------------------------ | ------------------------------------------------------------ |
| **Shift + F10**       | **^ + R**       | Run                            | 运行 entry。 （常用，特别好用）                              |
| **Shift + F9**        | **^ + D**       | Debug                          | 调试 entry。                                                 |
| **Alt + Shift + F10** | **^ + ⌥ + D**   | Choose and Run Configuration   | 会打开一个已经配置的运行列表，让你选择一个后，再运行。       |
| **Alt + Shift + F9**  | **^ + ⌥ + D**   | Choose and Debug configuration | 会打开一个已经配置的运行列表，让你选择一个后，再以调试模式运行。 |



## 四、调试



| **快捷键(Win)**       | **快捷键(Mac)**   | **英文说明**            | **中文说明**                                                 |
| --------------------- | ----------------- | ----------------------- | ------------------------------------------------------------ |
| **F8**                | **F8**            | Step  Over              | 跳到当前代码下一行。 （常用）                                |
| **F7**                | **F7**            | Step  Into              | 跳入到调用的方法内部代码。 （常用）                          |
| **Alt + F9**          | **⌥ + F9**        | Run  to Cursor          | 让代码运行到当前光标所在处，非常棒的功能。  （常用）         |
| **Alt + F8**          | **⌥ + F8**        | Evaluate  Expression... | 打开一个表达式面板，然后进行进一步的计算。                   |
| **F9**                | **F9**            | Resume  Program         | 结束当前断点的本轮调试（因为有可能代码会被调用多次，所以调用后只会结束当前的这一次）如果有下一个断点会跳到下一个断点中。（常用） |
| **Ctrl + Shift + F8** | **⌘ + Shift+ F8** | View  Breakpoints...    | 打开当前断点的面板，可进行条件过滤。                         |



## 五、其他




| **快捷键(Win)**    | **快捷键(Mac)** | **英文说明**           | **中文说明**                |
| ------------------ | --------------- | ---------------------- | --------------------------- |
| **Ctrl + Alt + S** | **⌘ + ,**       | Settings / Preferences | 快速打开设置，配置 IDE 等。 |


