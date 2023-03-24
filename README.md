# Linux Shell Script Examples

> shell two data types `string` and `one-dimensional string array`  
> Bash provides one-dimensional indexed and associative array variables. Any variable may be used as an indexed array

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
