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
ms.openlocfilehash: 21ad3f9baf4b8e117f881d9a36fc606af04e17a5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66158434"
---
# <a name="how-to-install-mysql-on-azure"></a>Azure’a MySQL yükleme
Bu makalede, yükleme ve Linux çalıştıran Azure sanal makinesinde MySQL yapılandırma öğreneceksiniz.


> [!NOTE]
> Bu öğreticiyi tamamlamak için Linux çalıştıran bir Microsoft Azure sanal makine zaten olmalıdır. Lütfen [Azure Linux VM öğretici](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oluşturmak ve bir Linux VM ile ayarlamak için `mysqlnode` VM adı olarak ve `azureuser` devam etmeden önce kullanıcı olarak.
> 
> 

Bu durumda, MySQL bağlantı noktası olarak 3306 numaralı bağlantı noktasını kullanın.  

Bu makaledeki örnek olarak MySQL5.6 yüklemek için depo paket kullanacağız. Aslında, MySQL5.6 daha fazla geliştirme MySQL5.5 performansa sahiptir.  Daha fazla bilgi [burada](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).

## <a name="install-mysql56-on-ubuntu"></a>Ubuntu'da MySQL5.6 yükleyin
Ubuntu çalıştıran bir Linux VM kullanacağız.


### <a name="install-mysql"></a>MySQL'i yükleme

MySQL Server 5.6 geçerek yükleme `root` kullanıcı:

```bash  
sudo su -
```

MySQL sunucu 5.6 yükleyin:

```bash  
apt-get update
apt-get -y install mysql-server-5.6
```

  
Yükleme sırasında MySQL kök parolası ayarlamanıza izin istemek için görünen bir iletişim kutusu penceresi görürsünüz ve buraya parola ayarlamanız gerekir.
  
![image](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

Onaylamak için parolayı yeniden girin.

![image](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

### <a name="sign-in"></a>Oturum aç
  
MySQL hizmeti, MySQL server yüklemesi tamamlandığında, otomatik olarak başlatılır. MySQL sunucusuna oturum `root` kullanıcı ve parolanızı girin.

```bash  
mysql -uroot -p
```


### <a name="manage-the-mysql-service"></a>MySQL hizmeti yönetme

MySQL hizmeti durumunu alın

```bash   
service mysql status
```
  
MySQL Hizmeti Başlat

```bash  
service mysql start
```
  
MySQL Hizmeti Durdur

```bash  
service mysql stop
```
  
MySQL hizmeti yeniden başlatın

```bash  
service mysql restart
```

## <a name="install-mysql-on-red-hat-os-centos-oracle-linux"></a>İşletim sistemi Red Hat, CentOS, Oracle Linux üzerinde MySQL yükleme
Linux VM CentOS veya Oracle Linux ile burayı kullanacağız.

### <a name="add-the-mysql-yum-repository"></a>MySQL yum depo Ekle
    
Geçiş `root` kullanıcı:

```bash  
sudo su -
```

MySQL yayın paketi yükleyip yeniden oluştur:

```bash  
wget https://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
yum localinstall -y mysql-community-release-el6-5.noarch.rpm
```

### <a name="enable-the-mysql-repository"></a>MySQL depo etkinleştir
Dosya MySQL5.6 paketini indirme için MySQL deposunu etkinleştirmenize aşağıda düzenleyin.

```bash  
vim /etc/yum.repos.d/mysql-community.repo
```

  
Bu dosya her değeri için aşağıdaki güncelleştirin:

```  
\# *Enable to use MySQL 5.6*
  
[mysql56-community]
name=MySQL 5.6 Community Server
  
baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
enabled=1
  
gpgcheck=1
  
gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```

### <a name="install-mysql"></a>MySQL'i yükleme 

MySQL depodan yükleyin.

```bash  
yum install mysql-community-server
```
  
MySQL RPM paketini ve tüm ilişkili paketleri yüklenir.


## <a name="manage-the-mysql-service"></a>MySQL hizmeti yönetme
  
MySQL sunucusu hizmet durumunu kontrol edin:

```bash  
service mysqld status\
```
  
Varsayılan bağlantı noktası, MySQL server'ın çalışıp çalışmadığını kontrol edin:

```bash  
netstat  –tunlp|grep 3306
```

MySQL sunucusu başlatın:

```bash
service mysqld start
```

MySQL sunucusu durdurun:

```bash
service mysqld stop
```

MySQL başlayacak şekilde ayarlayın sistem önyükleme artırma:

```bash
chkconfig mysqld on
```

## <a name="install-mysql-on-suse-linux"></a>SUSE Linux üzerinde MySQL yükleme

Linux VM ile OpenSUSE burayı kullanacağız.

### <a name="download-and-install-mysql-server"></a>MySQL sunucusu indirin ve yükleyin
  
Geçiş `root` aşağıdaki komutu kullanıcı arabiriminden:  

```bash  
sudo su -
```
  
Paketini indirin ve MySQL yükleyin:

```bash  
zypper update
zypper install mysql-server mysql-devel mysql
```

### <a name="manage-the-mysql-service"></a>MySQL hizmeti yönetme
  
MySQL sunucusu durumunu kontrol edin:

```bash  
rcmysql status
```
  
Denetleme olmadığını MySQL Server varsayılan bağlantı noktası:

```bash  
netstat  –tunlp|grep 3306
```

MySQL sunucusu başlatın:

```bash
rcmysql start
```

MySQL sunucusu durdurun:

```bash
rcmysql stop
```

MySQL başlayacak şekilde ayarlayın sistem önyükleme artırma:

```bash
insserv mysql
```

## <a name="next-step"></a>Sonraki adım
Daha fazla bilgi için [MySQL](https://www.mysql.com/) Web sitesi.

