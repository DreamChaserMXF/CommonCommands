# CommonCommands

## Vim

1. 十六进制编辑：命令行模式（冒号进入）下输入命令： %!xxd 可将当前缓存转为 16 进制查看/编辑，输入命令： %!xxd -r 可进行逆变换。其中，"%"表示对当前文档的缓存直接进行操作，"!"表示用shell命令进行操作。
2. Visual Studio 下写的代码放在 Linux 下可能会报 null pointer ignored 的 warning，原因是 VS 用了 UTF-16（Unicode） 编码，可以用 Linux 的命令 iconv 去转。格式是：iconv [options] [-f from-encoding] [-t to-encoding] [inputfile]。 如： iconv -f UTF-16 -t UTF-8 test.h -o test.h
3. Vim 的配置：
```
set autoindent
set tabstop=4
set expandtab
set nu
```
4. Vim 把 tab 转为空格：retab

## Linux

1. 批量文本字符串替换：sed -i 's/原字符串/新字符串/g' \*.txt
2. 压缩包解压到指定文件夹（若无则创建）：tar -xzvf src.tar\[.gz\] -C dst_folder
3. bash 遍历文件：for file in $(ls \*.txt); do echo$file; done

## Visual Studio

1. 代码静态检查：[PVS-Studio](https://www.viva64.com/en/pvs-studio/)
2. 内存泄漏动态检查：[Visual Leak Detector](https://kinddragon.github.io/vld/)
3. 调试时输出指针 p 指向的前10个数组元素：监视窗口添加 b, 10
4. Eigen 方便查看 Matrix Vector 元素：[Debug Eigen in Visual Studio](http://eigen.tuxfamily.org/index.php?title=Developer%27s_Corner#Debugging_under_Visual_Studio)
5. 多处理器编译：项目->属性->C/C++->常规->多处理器编译：是

## tmux

1. 常用 tmux 快捷键配置
```
# replace prefix from b to a
unbind C-b
set -g prefix C-a
 
# press ctrl+a twice to achieve the original functionality
bind C-a send-prefix

# new window
bind c new-window -c "#{pane_current_path}"

# reload tmux conf
bind r source-file ~/.tmux.conf \; display "tmux.conf reload!"
 
# split window
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"

# select pane
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R
 
# set start index of window and pane
set -g base-index 1
set -g pane-base-index 1
```

2. 常用 tmux 指令：
```
* 强制关闭当前窗口: prefix+&
```

## ffmpeg
1. mp4->yuv
```
ffmpeg -i video.mp4 video.yuv
```
2. mp4->jpg
```
ffmpeg -i video.mp4 frame/%04d.jpg
```

3. jpg->mp4
```
ffmpeg -i frame/%04d.jpg -vcodec libx264 video.mp4
```

## python
1. command shell
```
import subprocess
command = 'ls'
subprocess.call(command, shell=True)
```


