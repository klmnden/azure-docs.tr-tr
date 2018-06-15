---
title: Bir Linux VM Azure üzerinde MySQL ayarlama | Microsoft Docs
description: Azure'da bir Linux sanal makineye (Ubuntu veya RedHat ailesi işletim sistemi) MySQL yığını yüklemeyi öğrenin
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
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
ms.author: iainfou
ms.openlocfilehash: d91f8cf8455a60d3e0afb2f209ba07933bcdee1c
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30239183"
---
# <a name="how-to-install-mysql-on-azure"></a>Azure’a MySQL yükleme
Bu makalede, yüklemek ve bir Azure Linux çalıştıran sanal makine üzerinde MySQL yapılandırmak öğreneceksiniz.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-mysql-on-your-virtual-machine"></a>MySQL sanal makinenize yükleyin
> [!NOTE]
> Bu öğreticiyi tamamlamak için Linux çalıştıran bir Microsoft Azure sanal makine zaten olmalıdır. Lütfen bakın [Azure Linux VM'de öğretici](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oluşturmak ve bir Linux VM ile ayarlamak için `mysqlnode` VM adı ve `azureuser` devam etmeden önce kullanıcı olarak.
> 
> 

Bu durumda, 3306 bağlantı noktası MySQL bağlantı noktası olarak kullanın.  

Putty üzerinden oluşturulan VM Linux bağlayın. Azure Linux VM'de kullandığınız ilk kez kullanıyorsanız putty kullanılması hakkında bilgi için bir Linux VM bağlanmak [burada](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Bu makaledeki örnek olarak MySQL5.6 yüklemek için depo paket kullanacağız. Aslında, MySQL5.6 performans MySQL5.5'den daha fazla geliştirme sahiptir.  Daha fazla bilgi [burada](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).

### <a name="how-to-install-mysql56-on-ubuntu"></a>Ubuntu üzerinde MySQL5.6 yükleme
Linux VM Ubuntu Azure ile burada kullanacağız.

* 1. adım: Yükleme MySQL Server 5.6 anahtara `root` kullanıcı:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    MySQL server 5.6 yükleyin:
  
            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6
  
    Yükleme sırasında MySQL kök parola ayarlamanızı ve parola Burada ayarlanan isteyin iletişim penceresi poping kadar görürsünüz.
  
    ![görüntü](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

    Onaylamak için parolayı yeniden girin.

    ![görüntü](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

* 2. adım: Oturum açma MySQL sunucusu
  
    MySQL sunucusu yüklemesi tamamlandığında, MySQL hizmeti otomatik olarak başlatılır. MySQL Server ile oturum açabileceğiniz `root` kullanıcı.
    Kullanım oturum açma ve giriş parola komutuna aşağıda.
  
             #[root@mysqlnode ~]# mysql -uroot -p
* 3. adım: çalışan MySQL hizmetini yönetme
  
    (a) MySQL hizmetinin durumunu Al
  
             #[root@mysqlnode ~]# service mysql status
  
    (b) MySQL hizmetini başlatın
  
             #[root@mysqlnode ~]# service mysql start
  
    (c) MySQL hizmetini durdurun
  
             #[root@mysqlnode ~]# service mysql stop
  
    (d) MySQL hizmetini yeniden başlatın
  
             #[root@mysqlnode ~]# service mysql restart

### <a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>Red Hat işletim sistemi ailesi CentOS, Oracle Linux gibi MySQL yükleme
Linux VM CentOS veya Oracle Linux burada kullanacağız.

* 1. adım: MySQL Yum Depo anahtarı eklemek için `root` kullanıcı:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    MySQL yayın paketine yükleyip yeniden açın:
  
            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm
* 2. adım: dosya MySQL5.6 paketini indirme için MySQL deposunu etkinleştirmek için aşağıda düzenleyin.
  
            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo
  
    Bu dosyayı her değerini aşağıda güncelleştirin:
  
        \# *Enable to use MySQL 5.6*
  
        [mysql56-community]
        name=MySQL 5.6 Community Server
  
        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
        enabled=1
  
        gpgcheck=1
  
        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
* 3. adım: Yükleme MySQL yüklemek MySQL MySQL depodan:
  
           #[root@mysqlnode ~]#yum install mysql-community-server
  
    MySQL RPM paketi ve tüm ilişkili paketleri yüklenir.
* Adım 4: çalışan MySQL hizmetini yönetme
  
    (a) MySQL sunucunun hizmet durumunu kontrol edin:
  
           #[root@mysqlnode ~]#service mysqld status
  
    (b) varsayılan bağlantı noktası, MySQL server çalışıp çalışmadığını kontrol edin:
  
           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306

    (c) MySQL server başlatın:

           #[root@mysqlnode ~]#service mysqld start

    (d) MySQL server durdurun:

           #[root@mysqlnode ~]#service mysqld stop

    (e) ne zaman başlatmak için kümesi MySQL sistem önyükleme Yukarı:

           #[root@mysqlnode ~]#chkconfig mysqld on


### <a name="how-to-install-mysql-on-suse-linux"></a>SUSE Linux MySQL yükleme
Linux VM ile OpenSUSE burada kullanacağız.

* 1. adım: MySQL Server yükleyip
  
    Geçiş `root` komutu altındaki kullanıcı arabiriminden:  
  
           #sudo su -
  
    Paketini indirin ve MySQL yükleyin:
  
           #[root@mysqlnode ~]# zypper update
  
           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql
* 2. adım: çalışan MySQL hizmetini yönetme
  
    (a) MySQL sunucusu durumunu kontrol edin:
  
           #[root@mysqlnode ~]# rcmysql status
  
    (b) onay olup olmadığını MySQL server varsayılan bağlantı noktası:
  
           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306

    (c) MySQL server başlatın:

           #[root@mysqlnode ~]# rcmysql start

    (d) MySQL server durdurun:

           #[root@mysqlnode ~]# rcmysql stop

    (e) ne zaman başlatmak için kümesi MySQL sistem önyükleme Yukarı:

           #[root@mysqlnode ~]# insserv mysql

### <a name="next-step"></a>Sonraki adım
Daha fazla kullanım ve MySQL ile ilgili bilgileri bulmak [burada](https://www.mysql.com/).

