# 关于使用NVWA服务器测试-NVWA02芯片工程整理
## 前期服务器配置
- NVWA服务器通过vscode配置ssh连接，目前命名为端口号码；计算所服务器命名为另一个端口号；ssh配置为wugaoxiang的远程并没有连接；最终的ecvsr为大团队立人楼GPU；通过vscode可以方便的进行vim内容的编辑，配合vscode插件使用
- 通过MOBA+VNC配置连接，必要操作
    - 使用打开端口 + tunning
    - 先使用命令行 '’vcserver: 38' 打开端口
    - 结束后使用 vncserver -kill: 38关闭端口
    - 上述命令用于打开图形化界面
- 通过Vscode的SSH协议连接不需要上述操作，可直接连接，工作文件夹区域保存在电脑project/NVWA02/Vscode配置/文件夹下
## 正确比对软件输出结果和RTL仿真结果
1. 验证在什么情况下的计算结果是正确的
    - 看之前的记录，
    RTL仿真输出：gpio_output.txt 
    软件仿真输出：data/x2/image1   
    比对方式: Makefile脚本内gvim函数(x2）

2. 出现比对后半部分RTL全是0的情况：VIM比对存在的折叠情况 

        - 1. 更改SR_mode需要更改两个地方：top_tb的 SR_mode; 

        - 2. 在FPGA工程配置：对于芯片配置里面的i2C_master_control.v文件，1317行 orvwb_datamaster2slave = 8'h03;结尾两位对应SR配置mode,对应关系如下：    
        -    `define     SRIDLE     2'b00
        -    `define     SRX2       2'b01
        -    `define     SRX3       2'b10
        -    `define     SRX4       2'b11

## 关于目前芯片文件内部记录
1. 目录下sr_test的内容是关于FPGA测试工程的vcs+verdi仿真，和存放在rtl里面的芯片代码不一样
2. 目前的$DE_DIR文件被设置为临时路径，在结束之后会被清除，需要设置路径，否则在vcs的complie步骤将会失效，导致内部的层次无法展开。
    - 已经将 export DE_DIR=/home/wugaoxiang/nvwa02/rtl 设置路径添加到了nvwa02目录下的-Makefile里面
    - forwjk文件夹同上操作
    - 对应此前的必须进行一次source操作，该文件内同样设置了DE_DIR的关系
3. 使用topvcs(vcs文件类似仿真方法)
    - make run指令指向run指令，run指令包含了testbench.f/vcs_files.f/等文件，vcs_file通过'+incdir+'的方式指定了读取file文件夹路径，可以通过$DE_DIR添加相对路径
    - testbench的路径需要注意，因为可能存在多个top_tb复制件，在Makefile对应文件路径下进行获取
    - 以上注意点在Verdi里面都可以观测到，实际运行文件路径会被标记。
## 关于使用VCS的记录
[CSDN](https://blog.csdn.net/JasonFuyz/article/details/107508893 “Linux上进行vcs与verdi仿真”)

[CSDN](https://blog.csdn.net/feihe0755/article/details/139260155 “基础verdi操作大全”)

    - Verdi- driven可直接拉取相关驱动信号

## Vscode配置
- 1 . 在服务器上安装跳转ctags：[https://blog.csdn.net/weixin_48122743/article/details/136295069]
针对服务器x86架构
- 2 . 本来编译报错的选择是vivado自带的xvlog,但是因为git仓库管理代码格式，会因为define文件经常报错，先暂时放弃不用，等后续vcs自行报错。
- 3. ctags解压路径 [/home/wugaoxiang/Downloads/uctags-2024.12.20-linux-x86_64.release/bin] 同样部署了ctags级别


## 后续安排
1. 对着波形看vcs内部成哥关键代码仿真，能够对应到文章的核心方法

2. 能够熟悉vcs操作布局，将自己的代码能够合理的部署到服务器上面使用vcs；为后续decoder解码器，毕设进行铺垫

3. 在服务器上面加装合适的vscode插件

4. 问豪哥为什么我的在define文件上要报错，但是他的不会报错。

5. 学习使用git管理代码版本，提高效率，
- 服务器的作用是跑硬件代码和进行波形仿真的地方，因为有vcs+verdi的存在；后续可以直接在vivado上面直接得到硬件码流
- 本地电脑的git直接拉取外部的代码，或者上传到git上：作用是将代码放到一个地方管理，大家都可以编辑；或者重要代码方便管理存储。
- 那应该怎么做？能够将本地电脑-服务器-Git托管仓库连到一起做到联合修改，并且方便代码的保存（感觉这个得问豪哥关于本地密钥的东西）

    -- 下载windows git/
    -- 在vscode上进行git配置（打通本地-git路线）
    -- 
 
 - Vscode本地的项目管理上：学习采取工作区的形式，方便管理

6. 安装基于vscode的GPT插件，但是不知道效果怎么样，后续想要补充关于正版GPT的插件
7. 现在做一些修改，主要是，，，，，，
