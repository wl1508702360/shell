
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