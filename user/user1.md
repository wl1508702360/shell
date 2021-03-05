
## 添加用户user1，并给其设定密码，如果添加成功，就直接输出添加成功

```shell
#!/bin/bash
! id -u testUser &> /dev/null && useradd testUser && echo 'testUser' | passwd --stdin testUser | echo 'add testUser success' || echo 'testUser is exist'
```
