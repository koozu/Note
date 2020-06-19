# Fabric
## 安裝
1. 安裝開發人員工具 `yum groupinstall 'Development Tools'`
2. 擴充yum資料庫 `yum install epel-release`
3. 安裝Fabric `yum install Fabric`
4. 檢查是否安裝成功 `python -c "from fabric.api import * ; print env.version"`
5. 創建一個fabfile.py `vim fabfile.py`
    ```py
    def hello():
        print('hello')
    ```
6. 使用函式`fab hello`
## fabfile
- 引用API
    ```py
    from fabric.api import *
    ```
- 設定環境參數
    - `env.hosts` : 設定主機IP
    - `env.user` : 設定登入的使用者
    - `enc.passwords` : 設定登入的密碼
    - `env.roledefs` : 設定主機的角色，類似分組功能
    ```py
    env.hosts = ['192.168.56.106', '192.168.56.107']
    env.user = 'root'
    env.passwords = {
    'root@192.168.56.106:22':'centos',
    'root@192.168.56.107:22':'centos'}
    env.roledefs = {
    'all':['192.168.56.106', '192.168.56.107'],
    'test1':['192.168.56.106'],
    'test2':['192.168.56.107']
    }
    ```
- API可使用含式
    - `cd()` : 移動目錄
    - `run()` : 執行遠端命令
    - `local()` : 執行本地命令
    - `sudo()` : 使用`sudo`執行遠端命令
    - `get(remote, local)` : 從遠端下載文件至本地
    - `put(local, remote)` : 將本地文件上傳至遠端
    - `settings()` : 臨時覆蓋環境參數
    - `hide()` : 隱藏指定類型的訊息
    - `show()` : 顯示指定類型的訊息
        - `status` : 狀態訊息
        - `aborts` : 終止訊息
        - `warnings` : 警告訊息
        - `running` : 運行過程訊息
        - `stdout` : 標準輸出訊息
        - `stderr` : 標準錯誤訊息
        - `user` : 使用者輸出訊息
        - `output` : stdout, stderr
        - `everything` : stdout, stderr, warnings, running, user
        - `command` : stdout, running
## 範例
- 在user家目錄創建資料夾
    ```py
    def CreateDir():
        with cd('/home/user/'):
                run('mkdir fabrictest')
    ```
- 取得主機IP
    ```py
    def GetIP():
        with settings(hide('everything'), warn_only=True):
            ip = run('ip addr show | grep inet | grep -v inet6 | awk {\'print $2\'}')
        print(ip)
    ```
- 開啟Httpd
    ```py
    @roles('all')
    def OpenHttpd():
        with settings(hide('everything')):
                result = run('rpm -qa | grep httpd')
                if not result.failed:
                        print('Httpd has been installed')
                        result1 = run('systemctl status httpd')
                        if not result1.failed:
                                print('Httpd has been opened')
                        else:
                                print('Httpd is not open')
                                run('systemctl start httpd')
                                print('Httpd turned on')
                else:
                        print('Httpd has not been installed')
                        run('yum install httpd -y')
                        print('Httpd installed')
                        run('systemctl start httpd')
                        print('Httpd turned on')

    ```
---
參考資料:
- [Fabric 讓 Linux 系統部署變得簡單 – Pilot資料館 – Router.HK](https://www.linuxpilot.com/use-%EF%BB%BFfabric-to-easily-deploy-on-linux)
- [Mr. Big Nose: [Linux]在 CentOS 上安裝核心開發工具 Development Tools (Automake, Gcc (C/C++), Perl, Python & Debuggers)](http://mrbignose.blogspot.com/2010/04/linux-centos-development-tools-automake.html)
- [CentOS 7 安裝EPEL Repository 入門教學！ - BrilliantCode.net](https://www.brilliantcode.net/108/centos-7-install-epel-repository/)
- [The environment dictionary, env — Fabric  documentation](https://docs.fabfile.org/en/1.14/usage/env.html)
- [Python模块学习 - fabric - 一只小小的寄居蟹 - 博客园](https://www.cnblogs.com/xiao-apple36/p/9124292.html#_label6)