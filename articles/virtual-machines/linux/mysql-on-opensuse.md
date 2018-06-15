---
title: OpenSUSE VM Azure üzerinde MySQL yükleme | Microsoft Docs
description: Azure'da OpenSUSE Linux VMirtual makine MySQL yüklemek öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 1594e10e-c314-455a-9efb-a89441de364b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: cynthn
ms.openlocfilehash: 88bd895cb3a384f1ada0394fe2da206aca86b981
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
ms.locfileid: "28001181"
---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Azure'da OpenSUSE Linux çalıştıran bir sanal makineye MySQL yükleme

[MySQL](http://www.mysql.com) popüler, açık kaynaklı bir SQL veritabanı. Bu öğretici OpenSUSE Linux çalıştıran bir sanal makine oluşturun ve MySQL yükleme gösterilmektedir.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, 2.0 veya üstü Azure CLI sürüm gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-virtual-machine-running-opensuse-linux"></a>OpenSUSE Linux çalıştıran bir sanal makine oluşturma

İlk olarak, bir kaynak grubu oluşturun. Bu örnekte, biz kaynak grubu adlandırma *mySQSUSEResourceGroup* ve içinde oluşturma *Doğu ABD* bölge.

```azurecli-interactive
az group create --name mySQLSUSEResourceGroup --location eastus
```

VM oluşturun. Bu örnekte, biz VM adlandırma *myVM*. Biz de bir VM boyutu kullanacaksanız *Standard_D2s_v3*, ancak seçmesi gereken [VM boyutu](sizes.md) işleminizi iş yükü için en uygun olduğunu düşündüğünüz.

```azurecli-interactive
az vm create --resource-group mySQLSUSEResourceGroup \
   --name myVM \
   --image openSUSE-Leap \
   --size Standard_D2s_v3 \
   --generate-ssh-keys
```

Bir kural için MySQL 3306 bağlantı noktası üzerinden trafiğe izin verme ağ güvenlik grubuna eklemeniz gerekir.

```azurecli-interactive
az vm open-port --port 3306 --resource-group mySQLSUSEResourceGroup --name myVM
```

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

SSH VM'e bağlanmak için kullanacağız. Bu örnekte, VM'nin ortak IP adresidir *10.111.112.113*. VM oluşturduğunuz sırada çıktıda IP adresi görebilirsiniz.

```azurecli-interactive  
ssh 10.111.112.113
```

 
## <a name="update-the-vm"></a>VM güncelleştir
 
VM'ye bağlandıktan sonra sistem güncelleştirmelerini ve düzeltme eklerini yükleyin. 
   
```bash
sudo zypper update
```

VM güncelleştirmek için istemleri izleyin.

## <a name="install-mysql"></a>MySQL'i yükleme 


MySQL VM'yi SSH üzerinden yükleyin. Yanıtla uygun şekilde ister.

```bash
sudo zypper install mysql
```
 
Sistem önyüklendiğinde başlatmak için MySQL ayarlayın. 

```bash
sudo systemctl enable mysql
```
MySQL etkin olduğunu doğrulayın.

```bash
systemctl is-enabled mysql
```

Bu döndürmelidir: etkin.


## <a name="mysql-password"></a>MySQL parola

Yükleme sonrasında, MySQL kök parola varsayılan olarak boştur. Çalıştırma **mysql\_güvenli\_yükleme** MySQL güvenli hale getirmek için komut dosyası. Komut dosyası MySQL kök parolasını değiştirmek, anonim kullanıcı hesaplarını kaldırın, uzak kök oturum açmalar devre dışı bırakmak, test veritabanlarını kaldırmanız ve ayrıcalıkları tablo yeniden ister. 


```bash
mysql_secure_installation
```

## <a name="log-in-to-mysql"></a>MySQL için oturum açın

Şimdi oturum açın ve MySQL istemi girin.

```bash  
mysql -u root -p
```
Bu veritabanı ile etkileşim kurmak için SQL deyimleri nerede sorun MySQL istemi geçer.

Artık, yeni bir MySQL kullanıcı oluşturun.

```   
CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
```
   
Noktalı virgülle (;) satırın sonuna komutu bitiş için önemlidir.


## <a name="create-a-database"></a>Veritabanı oluşturma


Bir veritabanı oluşturmak ve vermek `mysqluser` kullanıcı izinleri.

```   
CREATE DATABASE testdatabase;
GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';
```
   
Yalnızca veritabanı kullanıcı adları ve parolalar veritabanına bağlanırken komut dosyaları tarafından kullanılır.  Veritabanı kullanıcı hesabı adları mutlaka gerçek kullanıcı hesapları sistem üzerindeki temsil etmiyor.

Başka bir bilgisayardan oturum açma etkinleştirin. Bu örnekte, oturum açmak için istiyoruz bilgisayarın IP adresidir *10.112.113.114*.

```   
GRANT ALL ON testdatabase.* TO 'mysqluser'@'10.112.113.114' IDENTIFIED BY 'password';
```
   
MySQL veritabanı yönetim yardımcı programı'ndan çıkmak için şunu yazın:

```    
quit
```


## <a name="next-steps"></a>Sonraki adımlar
MySQL hakkında daha fazla ayrıntı için bkz: [MySQL belgeleri](http://dev.mysql.com/doc/index-topic.html).




