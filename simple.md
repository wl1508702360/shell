
### 添加用户user1，并给其设定密码，如果添加成功，就直接输出添加成功

```shell
#!/bin/bash
! id -u testUser &> /dev/null && useradd testUser && echo 'testUser' | passwd --stdin testUser | echo 'add testUser success' || echo 'testUser is exist'
```

### 给定一个用户，如果其UID为0，显示其为管理员，否则，就显示其为普通用户

```shell
#!/bin/bash
USER=user1
USERID=`id -u $USER`
[ $USERID -eq 0] && echo "$USER is a admin." || echo "$USER is a common user."
```

### 判断系统上是否用户的默认shell为bash，如果有，就显示有多少这类用户，否则就显示没有这类用户

```shell
#!/bin/bash
# \< 锚定词首
grep "\<bash$" /etc/passwd &> /dev/null

# $?捕获grep的执行的结果
RETVAL=$?
if [ $RETVAL -eq 0 ]; then
    USERS=`grep "\<bash$" /etc/passwd | cut -d: -f1`
    echo "$USERS"
else
    echo 'No such users'
fi
```

### 给定一个用户，判断其UID和GID是否一样，如果一样，就输出ok，否则输出no
```shell
#!/bin/bash
USER=user1
# cat /etc/passwd | grep user1 | cut -d: -f3 
USERID=`id -u $USER`

# # cat /etc/passwd | grep user1 | cut -d: -f4
GROUPID=`id -g $USER`
if [ $USERID -eq $GROUPID ]; then
    echo 'ok'
else
    echo 'no'
fi
```

### 给定一个文件，判断这个文件中是否存在空白行，如果有，显示空白行的数目，否则就显示无空白行
```shell
#!/bin/bash
FILE=/home/ling/space
grep "^$" $FILE &> /dev/null
RETVAL=$?
if [ $RETVAL -eq 0 ]; then
    COUNT=`grep "^$" $FILE | wc -l`
    echo "the empty sapce count is $COUNT"
else
    echo "no empty space"
fi
```

### 判断文件是否存在
```shell
#!/bin/bash
FILE=/etc/passwd
if [ ! -e $FILE ]; then
    echo "$FILE is not exists"
fi
```

### 取出history命令中的总条目
```shell
#!/bin/bash
history | sed 's/^[[:space:]]*//g' | cut -d ' ' -f1 | tail -f -n 1
```

### 给定一个用户，获取其密码警告期限（6），而后判断用户最近一次修改密码时间(3)
### 距离其最长使用期限(5)是否已经小于警告期限(6)，小于，显示warning，否则显示ok
### 注意已经使用掉的天数
```shell
#!/bin/bash
USER=user1
# 使用时间 = 当前时间 - 修改时间
USEDTIME=$[$[`date +%s`/86400] - `egrep "^$USER\>" /etc/shadow | cut -d: -f3`]

# 获取最长使用期限
LONGTIME=`egrep "^$USER\>" /etc/shadow | cut -d: -f5`

# 获取警告期限
WARNINGTIME=`egrep "^$USER\>" /etc/shadow | cut -d: -f6`

# 剩余使用时间
REMAINTIME=$[$LONGTIME - $USEDTIME]
if [ $REMAINTIME -lt $WARNINGTIME ]; then
    echo "warning"
else
    echo "ok"
fi
```

### 接受一个参数（文件路径），判断此参数是一个存在的文件，就显示ok，否则显示no such file
```shell
#!/bin/bash
if [ $# -lt 1 ]; then
    echo 'usage: ./test.sh /etc/issue'
    exit 7
fi

if [ -e $1 ]; then
    echo 'ok'
else
    echo 'no such file'
fi
```

### 给脚本传递两个参数（整数），显示两数之和、之积
```shell
#!/bin/bash
if [ $# -lt 2 ]; then
    echo "Usage: ./test.sh 1 2"
    exit 7
fi

echo $[$1 + $2]
echo $[$1 * $2]
```

### 传递一个用户参数给脚本，判断传入用户的用户名和其基本组的组名是否一样，并显示相应的结果
```shell
#!/bin/bash
if [ $# -lt 1 ]; then
    echo "Usage: ./test.sh user1"
    exit 7
fi

if ! id $1 &> /dev/null; then
    echo "no such user"
    exit 7
fi

if  [ `id -nu $1` == `id -ng $1` ]; then
    echo "same"
else
    eco "not same"
fi
```

### 传递一个参数给脚本，如果参数为q、Q、quit或者Quit，就退出脚本，否则就显示用户退出的参数
```shell
if [ $# -lt 1 ]; then
    echo 'Usage: ./test.sh q'
    exit 7
fi

if [ $1 == 'q' ]; then
    echo 'quiting1'
    exit
elif [ $1 == 'Q' ]; then 
    echo 'quiting2'
    exit
elif [ $1 == 'quit' ]; then
    echo 'quiting3'
    exit
elif [ $1 == 'Quit' ]; then
    echo 'quiting4'
    exit
else
    echo $1;
fi
```
