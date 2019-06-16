---
title: İşletim sistemi yedekleme ve geri yükleme (büyük örnekler) Azure üzerinde SAP hana yazın II SKU'lara | Microsoft Docs
description: Type II SKU'lara (büyük örnekler) Azure üzerinde SAP HANA için Operatign sistem yedekleme ve geri yükleme gerçekleştirme
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
ms.date: 06/27/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c82c5c74fe13bad99528486be69089df5f477457
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60708604"
---
# <a name="os-backup-and-restore-for-type-ii-skus"></a>İşletim sistemi yedekleme ve geri yükleme Type II SKU'lara yönelik

Bu belgede bir işletim sistemi dosya düzeyi yedekleme gerçekleştirir ve geri yüklemek için adımları açıklanmaktadır **Type II SKU'lara** HANA büyük örnekleri. 

>[!NOTE]
>İşletim sistemi yedekleme betiklerini sunucuda önceden yüklenmiş olan arka yazılım kullanır.  

Varsayılan olarak, Microsoft Hizmet Yönetimi ekibi tarafından tamamlandıktan sonra dosya sistemi yedeklemek için iki yedekleme zamanlama ile yapılandırılmış işletim sisteminin düzeyi yedekleyin. Yedekleme işinin zamanlamasını, aşağıdaki komutu kullanarak denetleyebilirsiniz:
```
#crontab –l
```
Yedekleme zamanlamasını aşağıdaki komutu kullanarak istediğiniz zaman değiştirebilirsiniz:
```
#crontab -e
```
## <a name="how-to-take-a-manual-backup"></a>El ile yedekleyin nasıl?

İşletim sistemi dosya sistemi yedeği kullanarak zamanlanmış bir **sıralanmış işin** zaten. Ancak, işletim sistemi dosya düzeyinde yedekleme el ile de gerçekleştirebilirsiniz. El ile Yedekleme gerçekleştirmek için aşağıdaki komutu çalıştırın:

```
#rear -v mkbackup
```
Aşağıdaki ekran göster örnek el ile yedekleme gösterir:

![Nasıl](media/HowToHLI/OSBackupTypeIISKUs/HowtoTakeManualBackup.PNG)


## <a name="how-to-restore-a-backup"></a>Bir yedekleme geri yükleme

Tam yedekleme veya tek bir dosyayı bir yedekten geri yükleyebilirsiniz. Geri yüklemek için aşağıdaki komutu kullanın:

```
#tar  -xvf  <backup file>  [Optional <file to restore>]
```
Geçerli çalışma dizininde dosyanın geri yüklemeden sonra kurtarılır.

Aşağıdaki komut bir dosyanın geri gösterir */etc/fstabfrom* yedekleme dosyasını *backup.tar.gz*
```
#tar  -xvf  /osbackups/hostname/backup.tar.gz  etc/fstab 
```
>[!NOTE] 
>Yedeklemeden geri yüklendikten sonra dosyayı istediğiniz konuma kopyalamanız gerekecektir.

Aşağıdaki ekran görüntüsünde, bir tam yedekleme geri yükleme gösterilmektedir:

![HowtoRestoreaBackup.PNG](media/HowToHLI/OSBackupTypeIISKUs/HowtoRestoreaBackup.PNG)

## <a name="how-to-install-the-rear-tool-and-change-the-configuration"></a>Arka Aracı'nı yükleme ve yapılandırmasını değiştirmek nasıl? 

Relax-ve-Kurtarma (arka) paketleri **önceden yüklenmiş** içinde **Type II SKU'lara** HANA büyük örnekleri ve yapmanız gereken herhangi bir eylemi. Doğrudan işletim sistemi yedekleme için arka kullanarak da başlatabilirsiniz.
Ancak, kendi içindeki paketleri yüklemek için gerek duyduğunuz durumlarda, yüklemek ve arka Aracı'nı yapılandırmak için listelenen adımları takip edebilirsiniz.

Yüklenecek **arka** yedekleme paketleri, aşağıdaki komutları kullanın:

İçin **SLES** işletim sistemi, aşağıdaki komutu kullanın:
```
#zypper install <rear rpm package>
```
İçin **RHEL** işletim sistemi, aşağıdaki komutu kullanın: 
```
#yum install rear -y
```
Arka Aracı'nı yapılandırmak için parametreleri güncelleştirmeye gerek duyduğunuz **OUTPUT_URL** ve **BACKUP_URL** içinde */etc/rear/local.conf dosya*.
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

Aşağıdaki ekran görüntüsünde, bir tam yedekleme geri yükleme gösterilmektedir: ![RearToolConfiguration.PNG](media/HowToHLI/OSBackupTypeIISKUs/RearToolConfiguration.PNG)
