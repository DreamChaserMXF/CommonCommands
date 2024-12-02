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
syntax on
# jump to the last position when reopening a file
if has("autocmd")
  au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif
endif
```
4. Vim 把 tab 转为空格：retab
5. Vim as HexEditor:
   ```
   :%!xxd
   :%!xxd -r
   ```
6. case insensitive search
    ```
    /word\c   # case insensitive
    /word\C   # case sensitive
    ```
7. Count number of matches of a pattern
    ```
    :%s/pattern//gn
    ```
    Omit g to display the number of lines where the pattern matches
    ```
    :%s/pattern//n
    ```
    refer to https://vim.fandom.com/wiki/Count_number_of_matches_of_a_pattern

## Linux

1. 批量文本字符串替换：sed -i 's/原字符串/新字符串/g' *.txt
2. 压缩包解压到指定文件夹（若无则创建(macOS下需要手动创建)）：tar -xzvf src.tar\[.gz\] -C dst_folder
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
7. bash 将字符串作为命令执行([example](https://stackoverflow.com/a/2005201))
    ```
    s='ls'
    eval $s
    ```
8. history 显示执行时间：bashrc 中加入 export HISTTIMEFORMAT="%F %T " 即可
9. 在某个目录下的所有文本文件中搜索指定字符串：
    ```
    grep "str" path/to/directory -rinI
    ```
    r 表示 recursive，i 表示 case insensitive，n 表示搜索结果显示被搜到的结果在文件中的行号，I 表示仅在文本文件中搜索。
    ```
    grep -v "str" pat
    ```
    -v 表示 invert-match，反向匹配，只过滤未出现 “str” 的行。
10. 命令失败后立即退出 bash 脚本：
    ```
    set -e
    ```
11. 打印脚本执行的每行命令
    ```
    set -x
    ```
12. 文本行逆序
    ```
    cat file1.txt | tail -r > file2.txt
    ```
    或
    ```
    tac file1.txt > file2.txt
    ```
14. 字符串数组逆序
    ```
    echo "aaaa eeee bbbb ffff cccc"|tr ' ' '\n'|tac|tr '\n' ' '
    echo $str|tr ' ' '\n'|tac|tr '\n' ' '
    ```
15. 忽略大小写的文件查找
    ```
    find . -iname "xxx.*"
    ```

## macOS
1. 安装任意来源的软件
    ```
    sudo spctl --master-disable  # 解除安装限制
    sudo spctl --master-enable   # 恢复安装限制
    ```
    或者用 wget, curl 下载安装包，这样就不会被 macOS 检测到“downloaded from the internet”了
2. 批量文本字符串替换（sed 在 MacOS 下不太好用）：
    ```
    sed -i -e 's/原字符串/新字符串/g' *.txt  # sed 在 MacOS 下要加 -e，或 ''，如 sed -i '' 's/原字符串/新字符串/g' *.txt
    perl -i -pe's/原字符串/新字符串/g' *.txt
    grep -rli 'old-word' * | xargs -I@ sed -i '' 's/old-word/new-word/g' @
    ```
3. Xcode 版本切换
    ```
    xcode-select -v  查看 xcode-select 版本
    xcode-select -p  查看当前默认 Xcode 路径
    xcode-select --switch /Applications/XcodeXX.X.app  切换默认 Xcode 路径
    ```
4. 安装 brew
    ```
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```
5. brew 安装指定版本的软件
    ```
    brew install cmake@3.18
    refer to https://stackoverflow.com/a/4158763/8795791
    ```

## Visual Studio

1. 代码静态检查：[PVS-Studio](https://www.viva64.com/en/pvs-studio/)
2. 内存泄漏动态检查：[Visual Leak Detector](https://kinddragon.github.io/vld/)
3. 调试时输出指针 p 指向的前10个数组元素：监视窗口添加 p, 10
4. 调试时查看 Eigen 的 Matrix/Vector 元素：[Debug Eigen in Visual Studio](http://eigen.tuxfamily.org/index.php?title=Developer%27s_Corner#Debugging_under_Visual_Studio)
5. 调试时查看 OpenCV 的图像的插件：[Image Watch](https://docs.opencv.org/master/d4/d14/tutorial_windows_visual_studio_image_watch.html)
5. 多处理器编译：项目->属性->C/C++->常规->多处理器编译：是

## Xcode
1. debug, view pointer as array
    ```
    (float(*)[16])data
    or
    memory read -t int -c8 `array_name`  // https://stackoverflow.com/a/23401104/8795791
    ```

## tmux

1. 常用 tmux 快捷键配置（放在~/.tmux.conf下）
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

    # enable pane selection by mouse
    set -g mouse on 
    
    # reload tmux conf
    bind r source-file ~/.tmux.conf \; display "tmux.conf reload!"
    ```

2. 常用 tmux 指令
    ```
    * 强制关闭当前窗口: prefix+&
    * Session 改名：prefix+$
    * Window 改名：prefix+,
    * Session detach: prefix+d
    * Session attach: tmux a -t <session_name>
    * Session 切换: prefix+s
    * tmux 内部的复制粘贴：prefix+[ 进入复制模式，鼠标选择，或者用空格开始选择，vim 中的光标移动键来移动选择框，Enter键结束选择。prefix+] 进行粘贴。
    ```

3. 参考链接
    ```
    [tmux常用命令及快捷方式](https://zhuanlan.zhihu.com/p/90464490)
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
    ffmpeg -i video.mp4 -vsync 0 -q:v 2 frame/%04d.jpg
    ffmpeg -i video.mp4 -vsync 0 frame/%04d.png
    ffmpeg -i video.mp4 -vsync 0 -start_number 0 frame/%04d.png
    ```
    q:v 2~5, 2 means best quality, 5 means smallest file size, only valid with jpeg images. png is lossless compression.  
    \-vsync 0 means ffmpeg should extract every frame exactly. Either dropping or duplicating is forbidden.  
    \-start_number 0 means output image index will start from 0000 rather than 0001

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

6. make comparison video / stitching videos
    ```
    ffmpeg -i clip2/%04d_rlt.png -i clip3/%04d.png -i clip4/%04d.png -filter_complex hstack=inputs=3 -vcodec libx264 -crf 10 clip_2_3_4.mp4
    ffmpeg -i top_left.mp4 -i top_right.mp4 -i bottom_left.mp4 -i bottom_right.mp4 \ 
       -lavfi "[0:v][1:v]hstack[top];[2:v][3:v]hstack[bottom];[top][bottom]vstack" \
       -shortest 2by2grid.mp4  # https://github.com/stoyanovgeorge/ffmpeg/wiki/How-to-Stitch-Videos-Together
    ```
7. specify start image number and how many images do we use. In the following example, we use image/0250.png ~ image/0749.png to synthesize the yuv video
    ```
    ffmpeg -i image/%04d.png -c:v rawvideo -pix_fmt yuv420p -start_number 250 -frames:v 500 out_360x720.yuv
    ```
8. check resolution, frame number, bitrate in all mp4s under the current directory
    ```
    for f in *.mp4; do ffprobe -v error -select_streams v:0 -show_entries stream=width,height,nb_frames,duration,bit_rate,r_frame_rate -of csv=s=x:p=0 "$f"; done
    ```
    -of determines the output format.

9. remove audio of a video
   > ffmpeg -i in.mp4 -c:v copy -an out.mp4

10. cut up video by start time and duration (-t) or end time (-to)
    ```
    ffmpeg -i input.wmv -ss 00:00:30.0 -c copy -t 00:00:10.0 output.wmv
    ffmpeg -i input.wmv -ss 30 -c copy -t 10 output.wmv
    ffmpeg -i input.wmv -ss 30 -c copy -to 40 output.wmv
    ```
11. crop a video with x,y,w,h
   ```
   ffmpeg -i in.mp4 -c:v libx264 "crop=out_w:out_h:x:y" out.mp4
   ```
12. synthesis any images under a folder without monotonically increasing number as filenames
    ```
    ffmpeg -pattern_type glob -i 'certrain_folder/*.jpg' -c:v libx264 out.mp4
    ```
13. use ffmpeg standard input/output pipe (do not use it on Windows comand line or PowerShell because their pipe mechanism are not the same with Linux)
    ```
    ffmpeg -i input.mp4 -f rawvideo -pix_fmt bgr24 - | ffmpeg -f rawvideo -pix_fmt bgr24 -s 480x640 -i - -c:v libx264 results.mp4
    ```
14. use ffmpeg to rotate video (1=clockwise, 2=counter-clockwise)
    ```
    ffmpeg -i input.mp4 -c:v libx264 -crf 10 -vf "transpose=1" output.mp4
    ```
15. hide ffmpeg/ffprobe/ffplay info
    ```
    ffmpeg/ffprobe/ffplay -hide_banner
    ```
17. play video
    ```
    ffplay -hide_banner xxx.mp4
    ```
19. show video infomation
    ```
    ffprobe -hide_banner xxx.mp4
    ```
20. concat multiple videos
    ```
    # https://stackoverflow.com/questions/7333232/how-to-concatenate-two-mp4-files-using-ffmpeg
    # https://ffmpeg.org/ffmpeg-formats.html#concat
    $ cat mylist.txt
    file '/path/to/file1'
    file '/path/to/file2'
    file '/path/to/file3'
    
    $ ffmpeg -f concat -safe 0 -i mylist.txt -c copy output.mp4
    ```
21. concat multiple images with specific duration
    ```
    # https://stackoverflow.com/a/76990338/8795791
    # duration <seconds>
    $ cat filelist.txt
    file 1.jpg
    duration 2
    file 2.jpg
    duration 2
    file 3.jpg
    duration 2
    file 4.jpg
    duration 2
    file 5.jpg
    duration 2
    file 6.jpg
    duration 2
    file 1.jpg
    duration 2
    file 6.jpg
    duration 2
    file 1.jpg
    duration 2
    file 6.jpg
    duration 2
    file 1.jpg
    duration 2
    file 6.jpg
    duration 2
    file 1.jpg
    duration 2
    file 6.jpg
    duration 2

    # ffmpeg -f concat -i filelist.txt output_concat_filelist.mp4
    ```
22. concat images and videos simultaneously
    ```
    ffmpeg \
    -loop 1 -framerate 24 -t 10 -i image1.jpg \
    -i video.mp4 \
    -loop 1 -framerate 24 -t 10 -i image2.jpg \
    -loop 1 -framerate 24 -t 10 -i image3.jpg \
    -filter_complex "[0][1][2][3]concat=n=4:v=1:a=0" out.mp4
    # https://stackoverflow.com/a/43958846
    ```
23. extract keyframes only
    ```
    ffmpeg -skip_frame nokey -i test.mp4 -vsync vfr -frame_pts true out-%02d.jpeg
    # https://jdhao.github.io/2021/12/25/ffmpeg-extract-key-frame-video/#:~:text=So%20we%20can%20extract%20I-frames%20only%20to%20get,true%20out-%2502d.jpeg%20-vsync%20vfr%3A%20discard%20the%20unused%20frames
    ```
24. speed up or slow down the video
    ```
    # speed up by dropping some frames
    ffmpeg -i input.mp4 -vf "setpts=0.5*PTS" output.mp4
    # speed up by increasing framerate
    ffmpeg -i input.mp4 -r 60 -vf "setpts=0.5*PTS" output.mp4
    # slow down video with same framerate
    ffmpeg -i input.mp4 -vf "setpts=2*PTS" output.mp4
    # slow down video with frame interpolation
    ffmpeg -i input.mp4 -vf "setpts=2*PTS,minterpolate='mi_mode=mci:mc_mode=aobmc:vsbmc=1:fps=60'" output.mp4
    # refer to https://creatomate.com/blog/how-to-speed-up-or-slow-down-video-playback-using-ffmpeg
    ```

## python
1. command shell
    ```
    import subprocess
    command = 'ls'
    subprocess.call(command, shell=True)
    ```
2. print directory tree
    ```
    from __future__ import print_function
    import os.path
    import fnmatch

    for root, dir, files in os.walk("images"):
        depth = root.count('/')
        ret = ""
        if depth > 0:
            ret += "  " * (depth - 1) + "|-"
        print (ret + root)
        for items in fnmatch.filter(files, "*"):
            print (" " * len(ret) + "|-" + items)
    ```
3. control [numpy float print format](https://stackoverflow.com/a/22223261/8795791)
    ```
    np.set_printoptions(formatter={'float': lambda x: "{0:0.3f}".format(x)})
    ```

4. enter interactive mode
    ```
    import code
    
    ...
    
    code.interact(local=locals())
    ```
5. use conda environments in jupyter notebook
    ```
    https://medium.com/@nrk25693/how-to-add-your-conda-environment-to-your-jupyter-notebook-in-just-4-steps-abeab8b8d084
    conda install -c anaconda ipykernel
    python -m ipykernel install --user --name=<env_name>
    ```
6. http server
    ```
    python3 -m http.server [port_number]
    python2 -m SimpleHTTPServer [port_number]
    ```

## conda
1. create environment
    ```
    conda create -n <env_name> python=<python_version>
    ```
2. remove environment
    ```
    conda env remove -n <env_name>
    ```
3. rename environment
    ```
    https://stackoverflow.com/a/61355987/8795791
    conda create --name new_name --copy --clone old_name
    conda env remove --name old_name
    ```
5. search package
    ```
    conda serach <package_name>
    ```
4. list packages
    ```
    conda list
    ```

## git
1. alias
    ```
    git config --global alias.co checkout
    git config --global alias.br branch
    git config --global alias.ci commit
    git config --global alias.st status
    ```
    ```
    # .gitconfig
    [alias]
    co = checkout
    br = branch
    ci = commit
    ca = commit --amend
    can = commit --amend --no-edit
    cp = cherry-pick
    st = status
    stu = status -uno
    logline = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
    ```
2. stage modified files only:
    ```
    git add -u
    ```
3. check branch tracking info:
    ```
    git branch -vv
    ```
4. stage certain lines in a modified file
    ```
    git add -p file/you/want/to/stage
    ```
5. modify remote url (change from ssh to https)
    ```
    git remote set-url origin https://...
    ```
6. show modification history of a deleted file ([SO link](https://stackoverflow.com/a/7203551/8795791)):
    ```
    git log --all --full-history -- <path-to-file>
    git log --all --full-history -- "**/thefile.*"
    ```
7. remember password
    ```
    git config --global credential.helper store
    ```
8. check log for a file in all branches
    ```
    git log --all --full-history -- path/to/the/file
    ```
9. check log for any string in all branches
    ```
    git log -S "whatever" --source --all
    ```
    reference: https://stackoverflow.com/a/5816177/8795791
10. check log for certain lines of a file
    ```
    git log -L155,160:file/you/are/interested
    ```
12. Listing git branches containing the given commit
    ```
    git branch -a --contains <commit>/<tag>
    ```
    reference: https://www.w3docs.com/snippets/git/listing-branches-that-contain-a-given-commit.html
11. find the most recent common ancestor of two branches
    ```
    git merge-base branch2 branch3
    ```
    reference: https://stackoverflow.com/a/1549155/8795791
12. check file content in another branch
    ```
    git show branch:file
    ```
    reference: https://stackoverflow.com/questions/7856416/view-a-file-in-a-different-git-branch-without-changing-branches
13. list lfs tracked file
    ```
    git lfs ls-files
    git lfs ls-files --all  # https://stackoverflow.com/questions/46155906/git-lfs-get-the-complete-list-from-the-whole-repository
    ```
14. remove all untracked files and folders
    ```
    git clean -dfx
    git submodule foreach git clean -dxf  # for submodules
    ```
## adb

1. list installed package
    ```
    adb shell pm list packages -f
    ```
2. check SELinux's status
    ```
    adb shell getenforce  # Enforcing, Permissive，前者强制打开 SELinux 检查，后者仅记录违规行为不会禁止
    ```
