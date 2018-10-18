# CommonCommands

## Vim

1. 十六进制查看：命令行模式（冒号进入）下输入命令： %!xxd
2. Visual Studio 下写的代码放在 Linux 下可能会报 null pointer ignored 的 warning，原因是 VS 用了 UTF-16（Unicode） 编码，可以用 Linux 的命令 iconv 去转。格式是：iconv [options] [-f from-encoding] [-t to-encoding] [inputfile]。 如： iconv -f UTF-16 -t UTF-8 test.h -o test.h

## Linux

1. 批量文本字符串替换：sed -i 's/原字符串/新字符串/g' \*.txt
2. 压缩包解压到指定文件夹（若无则创建）：tar -xzvf src.tar -C dst_folder

## Visual Studio

1. 调试时输出指针 p 指向的前10个数组元素：监视窗口添加 b, 10
2. Eigen 方便查看 Matrix Vector 元素：[Debug Eigen in Visual Studio](http://eigen.tuxfamily.org/index.php?title=Developer%27s_Corner#Debugging_under_Visual_Studio)
3. 多处理器编译：项目->属性->C/C++->常规->多处理器编译：是
