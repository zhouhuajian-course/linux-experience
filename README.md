# Linux Shell Manual

> shell four data types `integer`, `double `, `string` and `one-dimensional string array`  (may be not correct)  
> Bash provides one-dimensional indexed and associative array variables. Any variable may be used as an indexed array

## 服务启动关闭

```shell
#!/bin/bash

############################
# 服务 启动/停止 脚本
#
# @author  zhouhuajian
# @version 1.0
############################

projectPath='/root/test'

case $1 in
  'start') 
    tmp=$(ps -ef | grep app.py | wc -l)
    if [[ $tmp -eq 1 ]]; then
      nohup python3 ${projectPath}/app.py > ${projectPath}/flask.log 2>&1 &
    fi
    tmp=$(ps -ef | grep stat-server | wc -l)
    if [[ $tmp -eq 1 ]]; then
      nohup ${projectPath}/stat-server > ${projectPath}/stat-server.log 2>&1 &
    fi
    ;;
  'stop') 
    tmp=$(ps -ef | grep app.py | wc -l)
    if [[ $tmp -gt 1 ]]; then
      ps -ef | grep -m 1 app.py | awk '{print $2}' | xargs kill -9  
    fi
    tmp=$(ps -ef | grep stat-server | wc -l)
    if [[ $tmp -gt 1 ]]; then
      ps -ef | grep -m 1 stat-server | awk '{print $2}' | xargs kill -9  
    fi
    ;;
  *) 
    echo "Usage: sh server-ctl.sh {start | stop}"
    ;;
esac
```

## 命令写多行

```shell
cmake \
-DCMAKE_INSTALL_PREFIX=${mysqlSourcePath} \
-DDOWNLOAD_BOOST=0 \
-DWITH_BOOST=${mysqlSourcePath}/boost/boost_1_77_0 \
-DWITH_DEBUG=1 \
${mysqlSourcePath}
```

## 查看 二进制文件 可打印字符

strings - print the strings of printable characters in files.

例如开发libc.so.6的开发人员，为了让开发者知道，这个libc兼容那些版本的标准库，就在库中定义了一些字符串常量，使用如下命令可以查看向下兼容的版本

```shell
$ strings /lib64/libc.so.6 | grep GLIBC
GLIBC_2.2.5
GLIBC_2.2.6
GLIBC_2.3
GLIBC_2.3.2
GLIBC_2.3.3
GLIBC_2.3.4
...
```

更多例子

```shell
$ strings /lib64/libstdc++.so.6 | grep CXXABI
CXXABI_1.3
CXXABI_1.3.1
CXXABI_1.3.2
CXXABI_1.3.3
CXXABI_1.3.4
...
```

## CURL 发送 POST 请求

shell
```
Usage: curl [options...] <url>

curl -X POST -H 'Content-Type: application/json' -H 'User-Agent: Mozilla/5.0' -d "{\"name\":\"xiaoming\",\"age\":18}" 192.168.1.206:8080/user/add
或者
curl -X POST -d '{"name":"xiaoming","age":18}' 192.168.1.206:8080/user/add
或者
curl -X POST -d {"name":"xiaoming","age":18} 192.168.1.206:8080/user/add
```

## 同时作多个文件或文件夹，创建、删除、移动等

```shell
$ touch /tmp/{a,b,c}
$ mkdir /tmp/{test1,test2}
$ mv /tmp/{a,b} /tmp/test1
$ ls /tmp/test1
a  b
```

## shell 显示当前目录

```shell
echo "export PS1='[\u@\h $PWD]\$ '" >> /etc/profile
source /etc/profile
```

## zip 压缩 解压缩

zip -r 文件名.zip 文件 目录 文件 目录 ...

unzip 文件名.zip

## 根据端口/命令名找进程和工作目录

```shell
$ netstat -anp | grep 22
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      972/sshd            
tcp6       0      0 :::22                   :::*                    LISTEN      972/sshd 
$ netstat -anp | grep sshd
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      972/sshd            
tcp6       0      0 :::22                   :::*                    LISTEN      972/sshd     
$ pwdx 972
972: /
```

## 创建用户组、创建用户

```shell
$ groupadd groupname
$ useradd -g groupname -m -d /home/username -s /bin/bash username
$ passwd username
$ id username
uid=1000(username) gid=1000(groupname) groups=1000(groupname)
```

## 解析任务配置 运行任务

```shell

# 代码层级较多，使用2个空格进行代码缩进

function changeDatabaseType() {
}
function changeMysqlUserPasswordAndJdbcurl() {
}
function start() {
}
function stop() {
}

# 初始化
# 安装yq命令
yq > /dev/null 2>&1
if [[ $? == 127 ]]; then
  echo "[ERROR] yq命令不存在，5秒后自动进行yq命令自动安装，安装过程可能较慢，请耐心等待"
  sleep 5s
  wget https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64 -O /usr/bin/yq && chmod +x /usr/bin/yq
fi
# 项目目录、所有模块
declare projectDirPath=`pwd`
declare allModules="`ls . | xargs`"
declare version=
# 解析配置，执行任务
declare isFirstLine='true'
declare -i index=0
declare -a currentTask
awk '!/^$/' tasks | awk '!/^#/' | while read line
do
  if [[ $isFirstLine == 'true' ]]; then
    version=$line
    isFirstLine='false'
  else
    currentTask[$index]=$line
    # 处理一下模块
    if [[ $index == 1 ]] && [[ ${currentTask[1]} == 'all' ]]; then
      currentTask[1]=$allModules
    fi
    # 执行当前任务，并把索引初始化为0
    if [[ $index == 2 ]]; then
      echo "========== ${currentTask[0]} =========="
      # declare -p currentTask
      ${currentTask[0]}
      index=0
    # 索引+1
    else
      ((index++))
    fi
  fi    
done
```

## 关联数组

```shell
$ declare -A user # 声明变量user是关联数组
$ user=([name]=zhouhuajian [age]=18) 
$ echo ${user[name]}
zhouhuajian
$ echo ${user[age]}
18
$ user[money]=10.03
$ echo ${user[money]}
10.03
$ echo ${user[*]}   # 所有值转字符串
10.03 zhouhuajian 18
$ echo ${user[@]}   # 所有值转字符串
10.03 zhouhuajian 18
$ echo ${!user[@]}  # 所有键转字符串
money name age
$ echo ${!user[*]}  # 所有键转字符串
money name age
# The -p option will display the attributes and values of each name.
$ declare -p user
declare -A user='([money]="10.03" [name]="zhouhuajian" [age]="18" )'
```

## if 命令 和 条件表达式

```shell
if test-commands; then
  consequent-commands;
[elif more-test-commands; then
  more-consequents;]
[else alternate-consequents;]
fi

The test-commands list is executed, and if its return status is zero, the consequent-commands list is executed. If test-commands returns a non-zero status, each elif list is executed in turn, and if its exit status is zero, the corresponding more-consequents is executed and the command completes. If ‘else alternate-consequents’ is present, and the final command in the final if or elif clause has a non-zero exit status, then alternate-consequents is executed. The return status is the exit status of the last command executed, or zero if no condition tested true.

[[…]]
[[ expression ]]
Return a status of 0 or 1 depending on the evaluation of the conditional expression expression.
```

```shell
# =~ 正则表达式
$ name="name1"
$ if [[ $name =~ ^name ]]; then echo "名字格式正确"; else echo "名字格式错误"; fi
名字格式正确
$ name="testname"
$ if [[ $name =~ ^name ]]; then echo "名字格式正确"; else echo "名字格式错误"; fi
名字格式错误
```

```shell
# -e file 文件是否存在
# True if file exists.
$ [[ -e /etc/profile ]]
$ echo $?
0
$ [[ -e /etc/profile111 ]]
$ echo $?
1

# file1 -nt file2 比较一个文件是否修改时间比另一个文件新
# True if file1 is newer (according to modification date) than file2, or if file1 exists and file2 does not.
$ touch a.txt
$ rsync -a a.txt b.txt # 拷贝 保留了文件时间
$ touch c.txt
$ [[ c.txt -nt a.txt ]]
$ echo $?
0
$ [[ b.txt -nt a.txt ]]
$ echo $?
1

# 字符串相等、字符串不相等
# string1 == string2
# string1 = string2
# True if the strings are equal. When used with the [[ command, this performs pattern matching as described above (see Conditional Constructs).
# ‘=’ should be used with the test command for POSIX conformance.
# string1 != string2
# True if the strings are not equal.
$ [[ "abc" == "abc" ]]
$ echo $?
0
$ [[ "abc" == "123" ]]
$ echo $?
1
$ test "abc" == "abc"
$ echo $?
0
$ test "abc" == "123"
$ echo $?
1
$ test "abc" = "abc"
$ echo $?
0
$ test "abc" = "123"
$ echo $?
1
```

## 字符串转数组、数组转字符串

```shell
export IFS=","
names="name1,name2,name3"
nameArr=(${names})
echo ${nameArr[*]}   # name1 name2 name3
echo ${nameArr[@]}   # name1 name2 name3
echo "${nameArr[*]}" # name1,name2,name3
echo "${nameArr[@]}" # name1 name2 name3
echo ${nameArr[0]}
echo ${nameArr[1]}
echo ${nameArr[2]}

export IFS="|"
names="name1|name2|name3"
nameArr=(${names})
echo ${nameArr[*]}   # name1 name2 name3
echo ${nameArr[@]}   # name1 name2 name3
echo "${nameArr[*]}" # name1|name2|name3
echo "${nameArr[@]}" # name1 name2 name3
echo ${nameArr[0]}
echo ${nameArr[1]}
echo ${nameArr[2]}
```
> IFS  
> A list of characters that separate fields; used when the shell splits words as part of expansion.
> Arrays are assigned to using compound assignments of the form  
> `name=(value1 value2 … )`  
> where each value may be of the form [subscript]=string. Indexed array assignments do not require anything but string. When assigning to indexed arrays, if the optional subscript is supplied, that index is assigned to; otherwise the index of the element assigned is the last index assigned to by the statement plus one. Indexing starts at zero.  

> 数组转字符串  
> ${arr[@]} ${arr[*]} 在双引号内不一样  
> If the subscript is ‘@’ or ‘*’, the word expands to all members of the array name. These subscripts differ only when the word appears within double quotes. If the word is double-quoted, ${name[*]} expands to a single word with the value of each array member separated by the first character of the IFS variable, and ${name[@]} expands each element of name to a separate word. When there are no array members, ${name[@]} expands to nothing. 
