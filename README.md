脚本只在干净的 Ubuntu 22.04 上经过测试。
在所有步骤之前，推荐提前更新软件包，并设置 git username 和 email。
如有需要，可以提前自行更换 ubuntu 软件源镜像，设置代理等。

-----
在 Ubuntu 22.04 上安装所有需要的包和工具：
```
setup.sh env
```
具体而言，脚本进行如下工作：
1. 更新软件包列表，并更新所有软件包
2. 检查 git email 和 username，若未设置则报错并退出
3. 安装所有需要的软件包，并修复 riscv32 编译错误
4. 检查 verilator，若未安装则从 gitee 克隆源码并编译安装

> 理论上来说，只要有所有的工具依赖，不需要本脚本配置环境，也不需要特定发行版，后续步骤也可以顺利执行。但这种情况未测试，不保证能 work。

-----
在当前目录下，配置学员 ysyx_23060203 的运行环境：
```
setup.sh repo ysyx_23060203
```
具体而言，脚本进行如下工作：
- 将该同学通过 ci 的代码，克隆到工作目录下 `ysyx-workbench` 目录中，关闭 tracer
- 在工作目录下生成 `activate.sh` 文件，用于应用环境变量
- 下载所有框架代码，对 rtt 和 soc 应用 patch，执行 rtt init
- 安装 mill 到工作目录下 `bin` 文件夹中，并生成 soc verilog

-----
应用运行该同学代码所需的环境变量：
```
source activate.sh
```
这样就可以在 `ysyx-workbench` 中调试运行学员的代码了。
`activate.sh` 使用绝对路径定位 `bin/mill` 和 `ysyx-workbench`。

-----
清理当前工作目录下的 `ysyx-workbench`，删除所有编译产物和临时文件（除了 soc verilog），但保留对代码的修改：
```
setup.sh clean
```
完成后，可将目录打包发送。注意：务必保留 `bin/mill` 和 `activate.sh`。
由于使用绝对路径，接收者需修改 `activate.sh` 中 `B_EXAM_HOME` 为解压后的目录。

-----
`setup repo` 不可避免地需要从神秘网络下载很多东西，所以可能需要在此之前设置代理：
```
https_proxy=http://127.0.0.1:9080
http_proxy=http://127.0.0.1:9080
all_proxy=socks5://127.0.0.1:9080
JAVA_OPTS="-Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=9080 -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=9080"
```
此处假设你的代理为 `127.0.0.1:9080`
