# CommonCommands

## Vim

1. 十六进制查看：命令行模式（冒号进入）下输入命令： %!xxd
2. Visual Studio 下写的代码放在 Linux 下可能会报 null pointer ignored 的 warning，原因是 VS 用了 UTF-16（Unicode） 编码，可以用 Linux 的命令 iconv 去转。格式是：iconv [options] [-f from-encoding] [-t to-encoding] [inputfile]。 如： iconv -f UTF-16 -t UTF-8 test.h -o test.h

## Linux

1. 批量文本字符串替换：sed -i 's/原字符串/新字符串/g' \*.txt
2. 压缩包解压到指定文件夹（若无则创建）：tar -xzvf src.tar -C dst_folder
