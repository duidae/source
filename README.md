# Notes for Linux
[5分鐘了解 Linux 和 BSD 異同](https://www.linuxpilot.com/linux-bsd)
* 用戶授權是兩者的主要差異，雖然它不會對大多數人產生影響。Linux 使用 GNU 通用公共用戶授權即 GPL。如果你修改了 Linux 核心，並將其分發，就必須放出修改的原程式碼。BSD 使用 BSD 用戶授權，如果修改了 BSD 核心或並且發佈它，也不需要必須公開程式碼。你可以自由地對你的 BSD 程式碼做任何你想做的事，你沒有義務發佈修改的程式碼，當然你想發佈也行。兩者都是開放源碼的，但是以不同的方式。人們有時會陷入關於哪種用戶授權是「更自由」的辯論。GPL 可以幫助用戶以確保他們可以擁有 GPL 軟體的程式碼，並限制開發人員迫使他們開放程式碼。BSD 用戶授權不能確保用戶可以擁有程式碼，只是給開發人員選擇是否公開程式碼的權利，即使他們想要把它變成一個閉源項目。

## Install Linux
* [RHEL suggestion](https://access.redhat.com/documentation/zh-tw/red_hat_enterprise_linux/7/html/installation_guide/sect-disk-partitioning-setup-x86#sect-recommended-partitioning-scheme-x86)
* / 50G
* /boot 500MB
* swap 兩倍記憶體大小
* /home whatever you need
* 新手or簡單安裝, 切 / & swap 就好

## Installing Ubuntu, CentOS, RHEL, Freebsd with USB
* find usb device
  * $ lsblk
* format usb to FAT32
  * $ sudo mkdosfs -F 32 -I /dev/sdb
* write ISO to usb
  * $ sudo dd if=/home/duidae/source/rhel-server-7.0x86_64-boot.iso of=/dev/sdb status=progress
  * $ ~watch -n 5 killall -USR1 dd //check dd progress~
* usb不能用太爛的...雜牌的dd copy不進去/copy進去裝不起來...創見的就都ok裝的起來...what the hell??
* Freebsd usb install要抓memstick的img, ex: FreeBSD-11.2-RELEASE-amd64-memstick.img, 抓iso用dd寫進usb不能用

## CentOS repo & yum
* epel repo
  * $ sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
  * https://www.brilliantcode.net/108/centos-7-install-epel-repository/
* yum 
  * $ sudo yum -y update
  * $ sudo yum -y install [app]
  * $ sudo yum remove [app name] // 移除軟體
  * $ sudo yum info gcc // get info of app
* 裝rpm
  * $ sudo yum install code-1.25.0-1530796575.el7.x86_64.rpm
  
## Freebsd
* GUI 桌面環境
  * 有gnome, KDE, xfce4可選, gnome/KDE都頗肥的
  * 裝xfce4
    * https://www.linuxhelp.com/how-to-install-xfce-desktop-in-freebsd/
  * gnome/KDE
    * $ pkg install -y xorg // 要先裝 xorg(X server), 再照下面的 5.7 裝跟設定gnome or KDE
    * https://www.freebsd.org/doc/zh_TW/books/handbook/x11-wm.html

## 軟體
* necessary: git, office, 小畫家, grub customizer
  * $ sudo yum install -y vim, git, libreoffice, kolourpaint, grub-customizer
 * grub-customizer @ Ubuntu
    * $ sudo add-apt-repository ppa:danielrichter2007/grub-customizer
    * $ sudo apt-get update
    * $ sudo apt-get install grub-customizer
* build-essentials
* Development Tools @ CentOS
  * $ sudo yum groupinstall -y 'Development Tools'
* VS code setting
  * put settings.json in ~/.config/Code/User/
* screen
  * https://unix.stackexchange.com/questions/7453/how-to-split-the-terminal-into-more-than-one-view
* checkinstall
  * 最後make install的指令替換成checkinstall就可以了。而且不需sudo，產生deb安裝包後，安裝時用sudo就可以了。例如：
    * ./configure 
    * make 
    * checkinstall -D --install=no
  
## Google Chrome
* CentOS 7/RHEL 7
  * 1. 連官網下載rpm
    * sudo yum install -y [rpm]
  * 2. $ sudo vi /etc/yum.repos.d/google-chrome.repo // 這個repo裡有寫好的, 直接clone下來即可
    * $ sudo yum install -y google-chrome-stable
* Ubuntu
  * 直接連官網裝
  
## 軟體安裝位置
* /usr: package manager裝的地方
* /usr/local: 手動安裝, 不影響package manager, 不要時直接刪掉整個folder+移除PATH即可
  * 裝時記得 --prefix=/usr/local/[appname+version]
* /opt: optional, 不需要時可刪掉

## Shell
* "> /dev/null 2>&1"
  * http://ibookmen.blogspot.com/2010/11/unix-2.html
  * 0: 標準輸入 (stdin)
  * 1: 標準輸出 (stdout)
  * 2: 標準錯誤輸出 (stderr)
  * 2(stderr) > &1 (stdout), 如果寫成 2>1 就會變成輸出到"1"這個檔案
  * 重新導向的順序是由左至右。也就是說「>/dev/null 2>&1」會先處理「>/dev/null」再處理「2>&1」。用「2>&1 >/dev/null」並不會將 stderr 導到 /dev/null。
* $ uname -s  // print kernel name
  * Linux
* 看版本
  * $ cat /etc/centos-release
* 休眠 @ CentOs
  * $ systemctl hibernate
* $ grep -irn [string]
* $ find . -name "\*.png"
* 權限
  * $ sudo ls -l // 看owner
  * $ sudo chown -R duidae [dir/file] // 改目錄或檔案的owner
* 用terminal打開目前資料夾
  * $ nautilus .
* Terminal終端機分頁工具
  * 新增tab: Ctrl + Shift + t, 切換: Ctrl + Page Up/Down
  * screen, 可以同一視窗分割, 好用
    * $ screen
    * https://unix.stackexchange.com/questions/7453/how-to-split-the-terminal-into-more-than-one-view
  * Terminator
    * $ sudo yum install -y terminator
    * CentOs 裝了python有問題, 還要研究怎麼解

## Environment
* $ export PATH=/usr/local/Cellar/qt/4.8.7_3/bin:$PATH 

## Tool for trace code
* VS code
* Eclipse
* C/C++ Global
  * $ sudo apt install global
  * http://blog.xuite.net/jesonchung/scienceview/93560500-Trace+code+tool
* doxygen

## 疑難雜症
* [cannot-find-lxxx, library沒裝](http://i-pogo.blogspot.com/2010/01/usrbinld-cannot-find-lxxx.html)
* 若缺甚麼library
  * $ yum whatprovides libstdc++.so.6
* 'GLIBCXX_3.4.20' not found
  * $ strings  /usr/lib64/libstdc++.so.6 | grep GLIBCXX
* $ locate libuv.so

## ADSL(pppoe) on Linux
* CentOS 7 / RHEL 7
  * http://blog.kevinlinul.idv.tw/?p=174
  * http://www.bos.tw/2016/09/centos-%E6%88%96%E6%98%AF-redhat-%E7%B3%BB%E7%9A%84linux-adsl%E6%88%96pppoe-%E7%9A%84%E6%92%A5%E8%99%9F%E8%A8%AD%E5%AE%9A/
  * $ ifconfig
  * Hinet DNS: 168.95.1.1
  * $ sudo /sbin/ifup ppp0
* Ubuntu 18.04 LTS
  * $ systemctl stop NetworkManager.service
  * $ systemctl disable NetworkManager.service
  * $ pppoeconf

## Print列印
* CentOS 7
  * 防火牆會擋印表機...要先去 [應用程式]->[雜項]->[列印設定值],按新增會有程序自動解除
  * 需要/usr/lib/cups/filter/hpps
    * 缺HPLIP PostScript Finishing Filter
    * $ sudo yum install hplip
    * $ system-config-printer
