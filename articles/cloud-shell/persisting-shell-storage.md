---
title: "Azure bulut Kabuk Bash'te için dosyaları kalıcı | Microsoft Docs"
description: "Azure bulut Kabuk Bash'te dosyaları nasıl devam ederse gözden geçirme."
services: azure
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: juluk
ms.openlocfilehash: d8188634846a7ce75b5294cb3012069d9eafafc1
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
[!INCLUDE [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-bash-in-cloud-shell-storage-works"></a>Bulut Kabuk depolama Bash'te nasıl çalışır? 
Bulut Kabuk bash'te dosyalar aşağıdaki yöntemlerin her ikisi de ile devam eder: 
* Bir disk görüntüsünü oluşturma, `$Home` dizini içindeki tüm içeriği kalıcı hale getirmek için dizin. Disk görüntüsü, belirtilen dosya paylaşımı olarak kaydedilir `acc_<User>.img` adresindeki `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, ve değişiklikleri otomatik olarak eşitlenir. 
* Belirtilen dosya paylaşımı olarak takma `clouddrive` içinde `$Home` doğrudan dosya paylaşımı etkileşim için dizin. `/Home/<User>/clouddrive`eşlenmiş `fileshare.storage.windows.net/fileshare`.
 
> [!NOTE]
> Tüm dosyaları, `$Home` SSH anahtarları gibi dizini, bağlı dosya paylaşımında depolanan, kullanıcı disk görüntüsü kaldı. Bilgileri kalıcı sırasında en iyi yöntemleri uygulamak, `$Home` dizin ve bağlı dosya paylaşımı.

## <a name="use-the-clouddrive-command"></a>Kullanım `clouddrive` komutu
Bulut Kabuğu'nda Bash ile denilen bir komut çalıştırabilirsiniz `clouddrive`, bağlı dosya paylaşımı için bulut Kabuk el ile güncelleştirmeniz sağlar.
!["Clouddrive" komutunu çalıştırarak](media/persisting-shell-storage/clouddrive-h.png)

## <a name="mount-a-new-clouddrive"></a>Yeni bir clouddrive bağlama

### <a name="prerequisites-for-manual-mounting"></a>El ile bağlama için Önkoşullar
Kullanarak bulut kabuğu ile ilişkili dosya paylaşımı güncelleştirebilirsiniz `clouddrive mount` komutu.

Varolan bir dosya paylaşımını bağlama depolama hesapları olması gerekir:
* Yerel olarak yedekli depolama veya coğrafi olarak yedekli depolama dosya paylaşımları desteği.
* Atanan bölgenizde bulunur. Onboarding olduğunda, kaynak grubu adı atandığınız bölge listelenen `cloud-shell-storage-<region>`.

### <a name="the-clouddrive-mount-command"></a>Clouddrive bağlama komutu

> [!NOTE]
> Yeni bir dosya paylaşımı takma varsa, yeni bir kullanıcı görüntüsü için oluşturulur, `$Home` dizin. Önceki `$Home` görüntü önceki dosya paylaşımınıza tutulur.

Çalıştırma `clouddrive mount` komutunu aşağıdaki parametrelerle:

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

Daha fazla ayrıntı görüntülemek için çalıştırın `clouddrive mount -h`, aşağıda gösterildiği gibi:

![Çalışan ' clouddrive mount'command](media/persisting-shell-storage/mount-h.png)

## <a name="unmount-clouddrive"></a>Clouddrive çıkarın
Bulut Kabuk herhangi bir anda takılı bir dosya paylaşımı çıkarın. Bulut Kabuk kullanılacak bağlı dosya paylaşımı gerektirdiğinden, oluşturma ve bir sonraki oturumda başka bir dosya paylaşımını bağlama istenir.

1. `clouddrive unmount` öğesini çalıştırın.
2. Onayla ve istekleri onaylayın.

Dosya paylaşımınızı el ile silmediğiniz sürece var olmaya devam eder. Bulut Kabuk, artık bu dosya paylaşımı için sonraki oturumlarda arar. Daha fazla ayrıntı görüntülemek için çalıştırın `clouddrive unmount -h`, aşağıda gösterildiği gibi:

![Çalışan ' clouddrive unmount'command](media/persisting-shell-storage/unmount-h.png)

> [!WARNING]
> Bu komutu çalıştıran herhangi bir kaynağa silmez ancak el ile bir kaynak grubu, depolama hesabı ya da bulut Kabuk eşlenmiş dosya paylaşımını silme siler, `$Home` dizin disk görüntüsü ve dosya paylaşımınızı klasördeki tüm dosyaları. Bu eylem geri alınamaz.

## <a name="list-clouddrive"></a>Liste`clouddrive`
Hangi dosya paylaşımı olarak takılı bulmak için `clouddrive`, çalışma `df` komutu. 

URL'de, depolama hesabı adı ve dosya paylaşımı clouddrive dosya yolu gösterir. Örneğin, `//storageaccountname.file.core.windows.net/filesharename`

```
justin@Azure:~$ df
Filesystem                                          1K-blocks   Used  Available Use% Mounted on
overlay                                             29711408 5577940   24117084  19% /
tmpfs                                                 986716       0     986716   0% /dev
tmpfs                                                 986716       0     986716   0% /sys/fs/cgroup
/dev/sda1                                           29711408 5577940   24117084  19% /etc/hosts
shm                                                    65536       0      65536   0% /dev/shm
//mystoragename.file.core.windows.net/fileshareName 5368709120    64 5368709056   1% /home/justin/clouddrive
justin@Azure:~$
```

[!INCLUDE [features-introblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Sonraki adımlar
[Bulut Kabuk hızlı başlangıcı bash](quickstart.md) <br>
[Microsoft Azure dosyaları depolama hakkında bilgi edinin](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Depolama etiketler hakkında bilgi edinin](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>
