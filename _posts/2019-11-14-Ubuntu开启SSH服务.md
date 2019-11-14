2019-11-14-Ubuntu开启SSH服务,Windows通过ssh连接

- 安装SSH服务  

  ```Coma
  sudo apt-get install openssh-client
  ```

- 确认sshserver启动 

  ```comman
  ps -e | grep ssh
  ```

  ​        若出现sshd，则说明ssh已启动

- 启动ssh 

  ```Comman
  sudo service ssh start
  ```

  查询IP

  ```comman
  ifconfig
  ```

  Windows通过Mobaxterm软件根据IP地址以及账户名登录Ubuntu服务器

