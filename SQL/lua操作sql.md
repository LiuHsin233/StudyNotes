# 安装对应的操作库
1. LuaSQL 可以使用LuaRocks


> 换成其他的库说不定更方便 但是需要学习额外的脚本语言
> 
> 
> 
> 
> ## lua脚本配置
> 1. VScode
> 2. 必要插件 CodeRunner(运行) LuaHelper(调试)
> 3. 新建文件夹,创建文件,点调试会提示自动生成一个json文件
> 4. 点左边的运行与调试,然后模式改为DebugFile(Attach是和其他的搭配用的)
> 5. 直接运行点右上角的运行(三角形)
> 
> C:\Users\user>luarocks install luasql-mysql
Installing http://luarocks.org/repositories/rocks/luasql-mysql-2.7.0-1.rockspec...

Error: Could not find expected file mysql.h for MYSQL -- you may have to install MYSQL in your system and/or set the MYSQL_DIR variable