# Linux Shell Script Examples

> shell two data types `string` and `one-dimensional string array`  
> Bash provides one-dimensional indexed and associative array variables. Any variable may be used as an indexed array

## 字符串转数组、数组转字符串

···shell
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
