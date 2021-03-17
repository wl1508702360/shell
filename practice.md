### [给定一个包含电话号码列表（一行一个电话号码）的文本文件 file.txt，写一个 bash 脚本输出所有有效的电话号码。](https://leetcode-cn.com/problems/valid-phone-numbers)

```shell
sed -nr "/^([0-9]{3}-|\([0-9]{3}\) )[0-9]{3}-[0-9]{4}$/p"
或者：
cat file.txt | grep -P "^(\d{3}-|\(\d{3}\) )\d{3}-\d{4}$"
```

### [写一个 bash 脚本以统计一个文本文件 words.txt 中每个单词出现的频率。](https://leetcode-cn.com/problems/word-frequency)

```shell
cat words.txt | tr " " "\n" | sort | uniq -c | sort -r | awk '{print $2,$1}'
```

### [给定一个文本文件 file.txt，请只打印这个文件中的第十行。](https://leetcode-cn.com/problems/tenth-line)

```shell
#!/bin/bash
# 方法1
COUNT=`cat file.txt | wc -l`
if [ $COUNT -lt 10 ]; then
    echo ""
else
    cat file.txt | head -10 | tail -f -n 1
fi

# 方法2
sed -n 10p file.txt
或者cat file.txt | sed -n 10p 

# 方法3
awk "NR == 10" file.txt
或者cat file.txt | awk "NR == 10"
```

### [给定一个文件 file.txt，转置它的内容。](https://leetcode-cn.com/problems/transpose-file)

```shell
#!/bin/bash
num=`head -n1 file.txt | wc -w`
for i in `seq 1 $num`; do
    echo `cut -d' ' -f$i file.txt`
done
```

