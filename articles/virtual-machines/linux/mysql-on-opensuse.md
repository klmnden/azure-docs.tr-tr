---
title: Azure'da OpenSUSE VM'ye MySQL yükleme | Microsoft Docs
description: Azure'da OpenSUSE Linux sanal bir makineye MySQL yükleme öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 1594e10e-c314-455a-9efb-a89441de364b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: za-rhoads
ms.openlocfilehash: 9f5ead43fb2d516697f7bbca5367cb02620152f9
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67667511"
---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Azure'da OpenSUSE Linux çalıştıran bir sanal makineye MySQL yükleme

[MySQL](https://www.mysql.com) popüler, açık kaynaklı bir SQL veritabanı. Bu öğreticide, OpenSUSE Linux çalıştıran bir sanal makine oluşturun, sonra MySQL yükleme işlemini göstermektedir.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-virtual-machine-running-opensuse-linux"></a>OpenSUSE Linux çalıştıran bir sanal makine oluşturma

İlk olarak bir kaynak grubu oluşturun. Bu örnekte, kaynak grubunun adı *mySQSUSEResourceGroup* içinde oluşturulan ve *Doğu ABD* bölge.

```azurecli-interactive
az group create --name mySQLSUSEResourceGroup --location eastus
```

Bir VM oluşturun. Bu örnekte, VM adlı *myVM* ve VM boyutu *Standard_D2s_v3*, ancak seçmeniz gereken [VM boyutu](sizes.md) yükünüz için en uygun olduğunu düşündüğünüz.

```azurecli-interactive
az vm create --resource-group mySQLSUSEResourceGroup \
   --name myVM \
   --image openSUSE-Leap \
   --size Standard_D2s_v3 \
   --generate-ssh-keys
```

Trafik için MySQL 3306 bağlantı noktası üzerinden izin vermek için ağ güvenlik grubu kuralı eklemeniz gerekir.

```azurecli-interactive
az vm open-port --port 3306 --resource-group mySQLSUSEResourceGroup --name myVM
```

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

SSH, bir VM'ye bağlanmak için kullanacaksınız. Bu örnekte, sanal makinenin genel IP adresidir *10.111.112.113*. Sanal Makineyi oluştururken, çıkış IP adresini görebilirsiniz.

```azurecli-interactive  
ssh 10.111.112.113
```

 
## <a name="update-the-vm"></a>VM'yi güncelleştirin
 
Sanal Makineye bağlandıktan sonra sistem güncelleştirmeleri ve yamaları yükleyin. 
   
```bash
sudo zypper update
```

Sanal makinenizin güncelleştirmek için yönergeleri izleyin.

## <a name="install-mysql"></a>MySQL'i yükleme 


MySQL, SSH üzerinden sanal Makineye yükleyin. Uygun şekilde istemleri yanıtlayın.

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

Sunucuyu yeniden başlatın.

```bash
sudo reboot
```


## <a name="mysql-password"></a>MySQL parolası

Yüklemeden sonra MySQL kök parolası varsayılan olarak boştur. Çalıştırma **mysql\_güvenli\_yükleme** MySQL güvenli hale getirmek için komut dosyası. Betiği, MySQL kök parolası değiştirmek, anonim kullanıcı hesaplarını kaldırın, uzak kök oturum açma devre dışı bırakmak, test veritabanlarını kaldırmanız ve ayrıcalıkları tablo yeniden isteyip istemediğinizi sorar. 

Sunucu, ssh için VM yeniden yeniden başlatıldıktan sonra.

```azurecli-interactive  
ssh 10.111.112.113
```



```bash
mysql_secure_installation
```

## <a name="sign-in-to-mysql"></a>MySQL için oturum açın

Şimdi oturum açın ve MySQL isteminden girin.

```bash  
mysql -u root -p
```
Bu veritabanıyla etkileşime girmek için SQL deyimleri burada verebilir için MySQL isteminde geçer.

Şimdi yeni bir MySQL kullanıcısı oluşturun.

```sql
CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
```
   
Noktalı virgülle (;) satırın sonunda sona erdirme komutu için çok önemlidir.


## <a name="create-a-database"></a>Veritabanı oluşturma


Bir veritabanı oluşturma ve verme `mysqluser` kullanıcı izinleri.

```sql
CREATE DATABASE testdatabase;
GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';
```
   
Yalnızca veritabanı kullanıcı adları ve parolalar veritabanına bağlanırken betikler tarafından kullanılır.  Veritabanı kullanıcı hesabı adları gerçek kullanıcı hesapları sistem üzerindeki göstermez.

Oturum açma başka bir bilgisayardan etkinleştirin. Bu örnekte, gelen oturum açma izin vermek için bilgisayar IP adresidir *10.112.113.114*.

```sql
GRANT ALL ON testdatabase.* TO 'mysqluser'@'10.112.113.114' IDENTIFIED BY 'password';
```
   
MySQL veritabanı yönetim yardımcı programından çıkın için şunu yazın:

```    
quit
```


## <a name="next-steps"></a>Sonraki adımlar
MySQL hakkında daha fazla ayrıntı için bkz: [MySQL belgeleri](https://dev.mysql.com/doc).




