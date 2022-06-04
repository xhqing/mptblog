创建于 2022-02-22<br>
关键词: Mac, 删除, ABC输入法.

Mac自带的ABC输入法是不能在设置中通过直接按减号删除的，网上有的文章说关闭SIP系统就可以了，实践证明关闭SIP系统还是会保证com.apple.HIToolbox.plist文件的完整性。考虑其本质，操作系统保持系统完整性其实就是在还原用户可能误操作的文件，既然如此，只要想办法不让操作系统修改用户操作的文件就好了。解决方法如下：

1、复制这个文件到当前目录

```bash
cp ~/Library/Preferences/com.apple.HIToolbox.plist .
```

2.1、用Xcode打开刚才复制的com.apple.HIToolbox.plist文件，编辑com.apple.HIToolbox.plist文件（依次点开 `Root` -` AppleEnabledInputSources` ，会看到一列 item ，找到其中 `KeyboardLayout Name` 为 ABC 的那一列，将整列 item 删掉，然后 command + S 保存）

2.2、如果不方便打开plist文件(早起版本的Mac可能安装不了Xcode)，可以使用Plist转换为Json的工具(比如：JSONPlistBuddy)，转为Json后找到`AppleEnabledInputSources`这个Key，然后删除这个Key下面的如下内容：

```bash
{
      "KeyboardLayout ID" : 252,
      "KeyboardLayout Name" : "ABC",
      "InputSourceKind" : "Keyboard Layout"
    }
```

删除以上内容后先保存成Json文件，然后把之前的com.apple.HIToolbox.plist文件删掉，再把这个Json文件改名为`com.apple.HIToolbox.plist`

**3、找到刚才修改(或由Json生成的)好的com.apple.HIToolbox.plist文件，鼠标右键，点击get info，当前用户权限更改为“read-only”并勾选上“Locked”**

**4、查看文件权限是否修改成功**

```bash
ls -l com.apple.HIToolbox.plist
```

显示-r--------.....都只是可读权限就是修改成功了。

5、执行如下命令进行替换

```bash
rm ~/Library/Preferences/com.apple.HIToolbox.plist && cp ./com.apple.HIToolbox.plist ~/Library/Preferences/ 
```

 注意这里不能直接cp，不会覆盖，要先rm删除原文件。

7、重启Mac