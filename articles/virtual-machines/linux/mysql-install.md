---
title: Azure'da bir Linux sanal makinesi üzerinde Mysql'i ayarlama | Microsoft Docs
description: Azure'da bir Linux sanal makinesi (Ubuntu veya Red Hat ailesi işletim sistemi) üzerinde MySQL yığını'nı yükleme hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 153bae7c-897b-46b3-bd86-192a6efb94fa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/01/2016
ms.author: cynthn
ms.openlocfilehash: f9e0582a1338bcae7b330c7ece7c3d8cc8593cfa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60543948"
---
# <a name="how-to-install-mysql-on-azure"></a>Azure’a MySQL yükleme
Bu makalede, yükleme ve Linux çalıştıran Azure sanal makinesinde MySQL yapılandırma öğreneceksiniz.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-mysql-on-your-virtual-machine"></a>Sanal makineye MySQL yükleme
> [!NOTE]
> Bu öğreticiyi tamamlamak için Linux çalıştıran bir Microsoft Azure sanal makine zaten olmalıdır. Lütfen [Azure Linux VM öğretici](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oluşturmak ve bir Linux VM ile ayarlamak için `mysqlnode` VM adı olarak ve `azureuser` devam etmeden önce kullanıcı olarak.
> 
> 

Bu durumda, MySQL bağlantı noktası olarak 3306 numaralı bağlantı noktasını kullanın.  

Putty üzerinden oluşturulan VM Linux bağlanın. Azure Linux VM komutunu ilk kez buysa, putty kullanma konusuna bakın. bir Linux VM'ye bağlanma [burada](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Bu makaledeki örnek olarak MySQL5.6 yüklemek için depo paket kullanacağız. Aslında, MySQL5.6 daha fazla geliştirme MySQL5.5 performansa sahiptir.  Daha fazla bilgi [burada](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).

### <a name="how-to-install-mysql56-on-ubuntu"></a>Ubuntu'da MySQL5.6 yükleme
Linux VM ile azure'dan Ubuntu burayı kullanacağız.

* 1. Adım: MySQL Server 5.6 anahtara yükleme `root` kullanıcı:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    MySQL sunucu 5.6 yükleyin:
  
            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6
  
    Yükleme sırasında MySQL kök parolası ayarlamanıza izin istemek için görünen bir iletişim kutusu penceresi görürsünüz ve buraya parola ayarlamanız gerekir.
  
    ![image](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

    Onaylamak için parolayı yeniden girin.

    ![image](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

* 2. Adım: Oturum açma MySQL sunucusu
  
    MySQL hizmeti, MySQL server yüklemesi tamamlandığında, otomatik olarak başlatılır. MySQL sunucusu ile oturum açabilirler `root` kullanıcı.
    Kullanım aşağıdaki oturum açma ve giriş parola komutu.
  
             #[root@mysqlnode ~]# mysql -uroot -p
* 3. Adım: Çalışan bir MySQL hizmetini yönetme
  
    (a) MySQL hizmetinin durumunu Al
  
             #[root@mysqlnode ~]# service mysql status
  
    (b) MySQL hizmetini başlatın
  
             #[root@mysqlnode ~]# service mysql start
  
    (c) MySQL hizmetini durdurun
  
             #[root@mysqlnode ~]# service mysql stop
  
    (d) MySQL hizmetini yeniden başlatın.
  
             #[root@mysqlnode ~]# service mysql restart

### <a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>CentOS, Oracle Linux gibi Red Hat işletim sistemi ailesi MySQL yükleme
Linux VM CentOS veya Oracle Linux ile burayı kullanacağız.

* 1. Adım: MySQL Yum deposu anahtar ekleme için `root` kullanıcı:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    MySQL yayın paketi yükleyip yeniden oluştur:
  
            #[root@mysqlnode ~]# wget https://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm
* 2. Adım: Dosya MySQL5.6 paketini indirme için MySQL deposunu etkinleştirmenize aşağıda düzenleyin.
  
            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo
  
    Bu dosya her değeri için aşağıdaki güncelleştirin:
  
        \# *Enable to use MySQL 5.6*
  
        [mysql56-community]
        name=MySQL 5.6 Community Server
  
        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
        enabled=1
  
        gpgcheck=1
  
        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
* 3. Adım: MySQL yüklemek MySQL MySQL depodan yükleyin:
  
           #[root@mysqlnode ~]#yum install mysql-community-server
  
    MySQL RPM paketi ve tüm ilişkili paketleri yüklenir.
* 4. Adım: Çalışan bir MySQL hizmetini yönetme
  
    (a) MySQL server'ın hizmet durumunu kontrol edin:
  
           #[root@mysqlnode ~]#service mysqld status
  
    (b) varsayılan bağlantı noktası, MySQL server'ın çalışıp çalışmadığını kontrol edin:
  
           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306

    (c) MySQL server'ı başlatın:

           #[root@mysqlnode ~]#service mysqld start

    (d) MySQL server durdurun:

           #[root@mysqlnode ~]#service mysqld stop

    (e) başlayacak şekilde kümesi MySQL sistem önyükleme artırma:

           #[root@mysqlnode ~]#chkconfig mysqld on


### <a name="how-to-install-mysql-on-suse-linux"></a>SUSE Linux üzerinde MySQL yükleme
Linux VM ile OpenSUSE burayı kullanacağız.

* 1. Adım: MySQL sunucusu indirin ve yükleyin
  
    Geçiş `root` aşağıdaki komutu kullanıcı arabiriminden:  
  
           #sudo su -
  
    Paketini indirin ve MySQL yükleyin:
  
           #[root@mysqlnode ~]# zypper update
  
           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql
* 2. Adım: Çalışan bir MySQL hizmetini yönetme
  
    (a) MySQL sunucusu durumunu kontrol edin:
  
           #[root@mysqlnode ~]# rcmysql status
  
    (b) onay olmadığını MySQL Server varsayılan bağlantı noktası:
  
           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306

    (c) MySQL server'ı başlatın:

           #[root@mysqlnode ~]# rcmysql start

    (d) MySQL server durdurun:

           #[root@mysqlnode ~]# rcmysql stop

    (e) başlayacak şekilde kümesi MySQL sistem önyükleme artırma:

           #[root@mysqlnode ~]# insserv mysql

### <a name="next-step"></a>Sonraki adım
Daha fazla kullanım ve MySQL ile ilgili bilgiler bulabilirsiniz [burada](https://www.mysql.com/).

