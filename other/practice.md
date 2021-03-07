### [给定一个包含电话号码列表（一行一个电话号码）的文本文件 file.txt，写一个 bash 脚本输出所有有效的电话号码。](https://leetcode-cn.com/problems/valid-phone-numbers)

```shell
sed -nr "/^([0-9]{3}-|\([0-9]{3}\) )[0-9]{3}-[0-9]{4}$/p"
或者：
cat file.txt | grep -P "^(\d{3}-|\(\d{3}\) )\d{3}-\d{4}$"
```