## linux

### zip

1. 打包文件

```bash
zip -r outputfile folder1 folder2 folder3
```

2. 解压乱码问题

```bash
unzip -O cp936
```

### rename

修改后缀：
```bash
rename 's/\.js/\.jsx/' *
```

修改前缀：
```bash
for i in `ls`; do mv -f $i `echo "text_"$i`; done
```


