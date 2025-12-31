360 t7 使用 istoreos 风格主页时 quickstart 修复 CPU 温度显示，CMCC RAX3000 EMMC 或 增强版也可使用这个文件

参考https://www.right.com.cn/forum/thread-8329673-7-1.html里的解决办法"
可以试试用这个 lua 文件覆盖 /usr/lib/lua/luci/controller/istore_backend.lua 修复温度显示：
https://gist.github.com/puteulanus/1c180fae6bccd25e57eb6d30b7aa28aa

看了一下，是 /cgi-bin/luci/istore/system/status/ 这个请求没有 cpuTemperature 这个返回值，
这个请求是 /usr/lib/lua/luci/controller/istore_backend.lua 在处理的，但 lua 里只是转给了 quickstart 监听的端口，
核心问题是 istoreos 的 quickstart 不支持这个架构的 CPU 温度获取所以没有输出。
因为重新编译 quickstart 难度太高，从 lua 这边下手，让 cursor 帮忙修改了 lua 脚本，
对 /istore/system/status/ 请求，读取 /sys/class/thermal/thermal_zone0/temp 的 CPU 温度添加到返回值中，测试温度显示正常了"
