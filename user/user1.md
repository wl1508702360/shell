
### 添加用户user1，并给其设定密码，如果添加成功，就直接输出添加成功

```shell
#!/bin/bash
! id -u testUser &> /dev/null && useradd testUser && echo 'testUser' | passwd --stdin testUser | echo 'add testUser success' || echo 'testUser is exist'
```

### 判断系统上是否用户的默认shell为bash，如果有，就显示有多少这类用户，否则就显示没有这类用户

```shell
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