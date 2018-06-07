---
title: İşletim sistemi yedekleme ve geri yükleme (büyük örnekler) azure'da SAP HANA yazın II SKU'ları | Microsoft Docs
description: SAP HANA Azure (büyük örnekler) türü II SKU'ları üzerinde için Operatign sistem yedekleme ve geri yükleme gerçekleştirin.
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/31/2017
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d5caf5836b96555e01b55d408e51f3df2407d35
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34657613"
---
# <a name="os-backup-and-restore-for-type-ii-skus"></a>İşletim sistemi yedekleme ve geri yükleme türü II SKU'ları için

Bu belgede bir işletim sistemi yedekleme ve geri yüklemek için gereken adımları açıklar **türü II SKU'ları** HANA büyük örneği. 

>[!NOTE]
>İşletim sistemi yedekleme komut dosyalarını kullanan Server'da önceden yüklenmiş olan arka yazılımı.  

Varsayılan olarak, Microsoft Hizmet Yönetimi ekibi tarafından tamamlandıktan sonra sunucunun tam işletim sistemini yedeklemek için iki yedekleme zamanlaması ile yapılandırılır. Aşağıdaki komutu kullanarak yedekleme işinin zamanlamasını denetleyebilirsiniz:
```
#crontab –l
```
Yedekleme zamanlamasını aşağıdaki komutu kullanarak dilediğiniz zaman değiştirebilirsiniz:
```
#crontab -e
```
## <a name="how-to-take-a-manual-backup"></a>El ile yedek almak nasıl?

İşletim sistemi Yedekleme kullanılarak zamanlanan bir **cron iş** zaten. Ancak, işletim sistemi yedekleme el ile de gerçekleştirebilirsiniz. El ile Yedekleme gerçekleştirmek için aşağıdaki komutu çalıştırın:

```
#rear -v mkbackup
```
Aşağıdaki ekran göster örnek el ile yedekleme gösterir:

![Nasıl](media/HowToHLI/OSBackupTypeIISKUs/HowtoTakeManualBackup.PNG)


## <a name="how-to-restore-a-backup"></a>Bir yedekleme geri yükleme mi?

Tek bir dosyayı veya tam yedekleme yedekten geri yükleyebilirsiniz. Geri yüklemek için aşağıdaki komutu kullanın:

```
#tar  -xvf  <backup file>  [Optional <file to restore>]
```
Geri yüklendikten sonra dosyanın geçerli çalışma dizininde kurtarılır.

Aşağıdaki komut dosyasının geri yükleme gösterir */etc/fstabfrom* yedekleme dosyası *backup.tar.gz*
```
#tar  -xvf  /osbackups/hostname/backup.tar.gz  etc/fstab 
```
>[!NOTE] 
>Yedeklemeden geri yüklendikten sonra dosyanın istenen konuma kopyalamanız gerekir.

Aşağıdaki ekran görüntüsünde bir tam yedekleme, geri yükleme gösterir:

![HowtoRestoreaBackup.PNG](media/HowToHLI/OSBackupTypeIISKUs/HowtoRestoreaBackup.PNG)

## <a name="how-to-install-the-rear-tool-and-change-the-configuration"></a>Arka aracı yükleme ve yapılandırmasını değiştirmek nasıl? 

Relax-ve-Kurtar (arka) paketleri **önceden yüklenmiş** içinde **türü II SKU'ları** HANA büyük örnekleri ve eylemde bulunmanız gerekmez. Doğrudan işletim sistemi yedekleme için arka kullanarak da başlatabilirsiniz.
Ancak, paketleri kendi içinde yüklemenize gerek burada durumlarda yükleyip arka Aracı'nı yapılandırmak için listelenen adımları izleyebilirsiniz.

Yüklemek için **arka** yedekleme paketleri, aşağıdaki komutları kullanın:

İçin **SLES** işletim sistemi, aşağıdaki komutu kullanın:
```
#zypper install <rear rpm package>
```
İçin **RHEL** işletim sistemi, aşağıdaki komutu kullanın: 
```
#yum install rear -y
```
Arka Aracı'nı yapılandırmak için parametreleri güncelleştirmeniz gerekir **OUTPUT_URL** ve **BACKUP_URL** içinde */etc/rear/local.conf dosya*.
```
OUTPUT=ISO
ISO_MKISOFS_BIN=/usr/bin/ebiso
BACKUP=NETFS
OUTPUT_URL="nfs://nfsip/nfspath/"
BACKUP_URL="nfs://nfsip/nfspath/"
BACKUP_OPTIONS="nfsvers=4,nolock"
NETFS_KEEP_OLD_BACKUP_COPY=
EXCLUDE_VG=( vgHANA-data-HC2 vgHANA-data-HC3 vgHANA-log-HC2 vgHANA-log-HC3 vgHANA-shared-HC2 vgHANA-shared-HC3 )
BACKUP_PROG_EXCLUDE=("${BACKUP_PROG_EXCLUDE[@]}" '/media' '/var/tmp/*' '/var/crash' '/hana' '/usr/sap'  ‘/proc’)
```

Aşağıdaki ekran görüntüsünde bir tam yedekleme, geri yükleme gösterir: ![RearToolConfiguration.PNG](media/HowToHLI/OSBackupTypeIISKUs/RearToolConfiguration.PNG)
