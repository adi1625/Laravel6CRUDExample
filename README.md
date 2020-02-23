# Laravel6CRUDExample
Laravel 6 CRUD Example | Laravel 6 Tutorial For Beginners

1. 下载git、virtualbox、vagrant插件，并安装，顺便配置vagrant代理（不然下虚机超慢）。
1. 按照laravel官网教程，用vagrant下载 laravel 环境：homestead这个虚机，及其配置文件
（都是通过git bash输入命令完成，期间有说需要生成ssh密钥的，用来登入到虚机中）。
1. 修改虚机配置文件，设置共享目录，站点域名（每个域名都需要添加到hosts里，ip一样的）。
1. 启动虚机（vagrant up），在配置好的路径（此处为宿主机laravel6这个文件夹）下检出项目代码。
1. 通过ssh连到虚机中，在项目路径下执行 composer install、npm install。
1. 项目环境：复制项目内 .env.example 为 .env 文件，然后把里面的数据库用户名密码改了。
1. 在mysql数据库中创建库（空的），此处为laravel6。
1. 初始化数据库表，虚机内项目路径下：`php artisan migrate` 。
1. 初始化应用密钥，虚机内项目路径下：`php artisan key:generate` 。
1. 浏览器访问项目地址：`http://laravel6.test/shows/create` 。
1. 就能看见页面了，也可以操作，点击添加后，可以看到数据库里出现了一条记录。

注：

- vagrant 虚机用户和密码都是 vagrant。
- 虚机开启root用户: 先使用vagrant ssh 进入虚拟机（默认的是用户名密码是vagrant）然后执行以下代码就可以提取root权限，sudo passwd root 根据提示输入两次新密码，su root 切换到 root 用户，然后whoami  查看当前登录用户；
- 数据库用户名：homestead，密码：secret；# mysql -uhomestead -p"secret"；开启权限：mysql>GRANT ALL PRIVILEGES ON *.* TO 'homestead'@'%' IDENTIFIED BY 'secret' WITH GRANT OPTION; 重启服务；
- 遇到浏览器页面报错“No input file specified.”，检查nginx配置文件 `cd /etc/nginx/sites-available`，用vim命令查看当前目录下的网站域名文件中站点指定的路径是否正确；

## 虚机配置流程

以下命令都是在Git bash中执行。

- 配置vagrant代理`export http_proxy=127.0.0.1:1080`, `export https_proxy=127.0.0.1:1080`

- 下载虚机`vagrant box add laravel/homestead`
- 虚机内代理安装`vagrant plugin install vagrant-proxyconf`
- 下载配置文件 `git clone https://github.com/laravel/homestead.git Homestead`, `cd ~/Homestead`, `git checkout v9.2.0`
- 初始化虚机 `bash init.sh`
- 生成用来连接虚机的密钥文件（此处暂时生成一个空文件，上级的.ssh文件夹可能需要手动创建） `touch ~/.ssh/id_rsa`
- 启动虚机 `vagrant up`
- 进入虚机 `vagrant ssh`
- 关闭虚机 `vagrant halt`


### 虚机内代理配置

如果要代理全局生效的话，修改 $HOME/.vagrant.d/Vagrantfile 文件 （如果只应用到一个项目，则修改项目的 Vagrantfile 文件），加入以下内容:
```
Vagrant.configure("2") do |config|
if Vagrant.has_plugin?("vagrant-proxyconf")
config.proxy.http     = "https://127.0.0.1:1080/"
config.proxy.https    = "https://127.0.0.1:1080/"
config.proxy.no_proxy = "localhost,127.0.0.1,.example.com"
end
```

### 虚机共享文件夹

C:\Users\yourf\Homestead\Homestead.yaml 文件
```
folders:
    - map: C:\Code\project_1
      to: /home/vagrant/project_1
    - map: C:\Code\laravel6
      to: /home/vagrant/laravel6
```
