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
    - `env.hosts`設定主機IP
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
---
參考資料:
- [Fabric 讓 Linux 系統部署變得簡單 – Pilot資料館 – Router.HK](https://www.linuxpilot.com/use-%EF%BB%BFfabric-to-easily-deploy-on-linux)
- [Mr. Big Nose: [Linux]在 CentOS 上安裝核心開發工具 Development Tools (Automake, Gcc (C/C++), Perl, Python & Debuggers)](http://mrbignose.blogspot.com/2010/04/linux-centos-development-tools-automake.html)
- [CentOS 7 安裝EPEL Repository 入門教學！ - BrilliantCode.net](https://www.brilliantcode.net/108/centos-7-install-epel-repository/)