# Linux Shell Script Examples

> shell two data types `string` and `one-dimensional string array`  
> Bash provides one-dimensional indexed and associative array variables. Any variable may be used as an indexed array

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
