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
5. Vim as HexEditor:
   ```
   :%!xxd
   :%!xxd -r
   ```

## Linux

1. 批量文本字符串替换：sed -i 's/原字符串/新字符串/g' \*.txt
2. 压缩包解压到指定文件夹（若无则创建）：tar -xzvf src.tar\[.gz\] -C dst_folder
3. bash 遍历文件：for file in $(ls \*.txt); do echo$file; done
4. 查看 alias 的实际命令：type \<command\>，如 type ls
5. 多线程下载：axel -an 8 \<download_link\>
6. 截取路径的文件名，及不带后缀的文件名：
    ```
    $ s=/the/path/foo.txt
    $ echo ${s##*/}
    foo.txt
    $ s=${s##*/}
    $ echo ${s%.txt}
    foo
    $ echo ${s%.*}
    foo
    ```

## Visual Studio

1. 代码静态检查：[PVS-Studio](https://www.viva64.com/en/pvs-studio/)
2. 内存泄漏动态检查：[Visual Leak Detector](https://kinddragon.github.io/vld/)
3. 调试时输出指针 p 指向的前10个数组元素：监视窗口添加 b, 10
4. 调试时查看 Eigen 的 Matrix/Vector 元素：[Debug Eigen in Visual Studio](http://eigen.tuxfamily.org/index.php?title=Developer%27s_Corner#Debugging_under_Visual_Studio)
5. 调试时查看 OpenCV 的图像的插件：[Image Watch](https://docs.opencv.org/master/d4/d14/tutorial_windows_visual_studio_image_watch.html)
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

    # split window
    bind | split-window -h -c "#{pane_current_path}"
    bind - split-window -v -c "#{pane_current_path}"

    # swap window
    bind-key -n C-S-Left swap-window -t -1
    bind-key -n C-S-Right swap-window -t +1

    # select pane
    bind h select-pane -L
    bind j select-pane -D
    bind k select-pane -U
    bind l select-pane -R

    # set start index of window and pane
    set -g base-index 1
    set -g pane-base-index 1

    # reload tmux conf
    bind r source-file ~/.tmux.conf \; display "tmux.conf reload!"
    ```

2. 常用 tmux 指令
    ```
    * 强制关闭当前窗口: prefix+&
    ```

## ffmpeg
1. mp4->yuv
    ```
    ffmpeg -i video.mp4 -c:v rawvideo -pix_fmt yuv420p video.yuv
    ```

2. yuv->mp4
    ```
    ffmpeg -f rawvideo -vcodec rawvideo -s 1920x1080 -r 25 -pix_fmt yuv420p -i inputfile.yuv -c:v libx264 -preset ultrafast -qp 0 output.mp4
    ```

2. mp4->jpg
    ```
    ffmpeg -i video.mp4 frame/%04d.jpg
    ```

3. jpg->mp4
    ```
    ffmpeg -i frame/%04d.jpg -vcodec libx264 video.mp4
    ```
    如果找不到 libx264 编码器，可以用 apt 安装：
    > sudo apt install libavcodec-extra

4. bmp in 2 folders -> mp4 side by side
    ```
    ffmpeg -i ./control_group/%04d.bmp -i ./experimental_group/%04d.bmp -filter_complex '[0:v]pad=iw*2:ih[int];[int][1:v]overlay=W/2:0[vid]' -map [vid] -vcodec libx264 -crf 23 -preset veryfast ./comparison.mp4
    ```

5. rescale mp4
    ```
    ffmpeg -i origin.mp4 -vf scale=640:480 output.mp4
    ffmpeg -i origin.mp4 -vf scale=iw*.5:ih*.5 output.mp4
    ffmpeg -i origin.mp4 -vf scale=iw*.5:ih*.5:flags=bicubic output.mp4  # bicubic scaling
    ffmpeg -s:v 360:640 origin.yuv -vf scale=180:320 out.yuv
    ```

6. make comparison video
    ```
    ffmpeg -i clip2/%04d_rlt.png -i clip3/%04d.png -i clip4/%04d.png -filter_complex hstack=inputs=3 -vcodec libx264 -crf 10 clip_2_3_4.mp4
    ```
7. specify start image number and how many images do we use. In the following example, we use image/0250.png ~ image/0749.png to synthesize the yuv video
    ```
    ffmpeg -i image/%04d.png -c:v rawvideo -pix_fmt yuv420p -start_number 250 -frames:v 500 out_360x720.yuv
    ```
8. check resolution and number of frames in all mp4s under the current directory
    ```
    for f in *.mp4; do ffprobe -v error -select_streams v:0 -show_entries stream=width,height,nb_frames -of csv=s=x:p=0 "$f"; done
    ```
9. cut up video by start time and duration (-t) or end time (-to)
    ```
    ffmpeg -i input.wmv -ss 00:00:30.0 -c copy -t 00:00:10.0 output.wmv
    ffmpeg -i input.wmv -ss 30 -c copy -t 10 output.wmv
    ffmpeg -i input.wmv -ss 30 -c copy -to 40 output.wmv
    ```
10. synthesis any images under a folder without monotonically increasing number as filenames
    ```
    ffmpeg -pattern_type glob -i 'certrain_folder/*.jpg' -c:v libx264 out.mp4
    ```

## python
1. command shell
    ```
    import subprocess
    command = 'ls'
    subprocess.call(command, shell=True)
    ```

## git
1. alias
    ```
    git config --global alias.co checkout
    git config --global alias.br branch
    git config --global alias.ci commit
    git config --global alias.st status
    ```

