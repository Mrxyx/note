几种快速清空文件内容的方法：
```
$ : > filename #其中的 : 是一个占位符, 不产生任何输出.
```
```
$ > filename
```
```
$ echo “” > filename
```
```
$ echo /dev/null > filename
```
```
$ echo > filename
```
```
$ cat /dev/null > filename
```
```
$ cp /dev/null filename
```