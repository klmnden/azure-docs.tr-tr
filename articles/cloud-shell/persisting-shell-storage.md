---
title: Azure Cloud Shell'deki Bash hizmetinde için dosyaları kalıcı | Microsoft Docs
description: Azure Cloud Shell'deki Bash hizmetinde dosyaları nasıl devam ederse gözden geçirme.
services: azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/04/2018
ms.author: damaerte
ms.openlocfilehash: 0aa00af543a3d21db9b8ad0ed808a8bff0b534e1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60200298"
---
[!INCLUDE [PersistingStorage-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-cloud-shell-storage-works"></a>Cloud Shell depolama nasıl çalışır? 
Cloud Shell'i devam ederse aşağıdaki yöntemlerin her ikisi de aracılığıyla dosyaları: 
* Bir disk görüntüsü oluşturma, `$Home` dizini içindeki tüm içerikleri kalıcı hale getirmek için dizin. Disk görüntüsü belirtilen dosya paylaşımınızdaki kaydedilir `acc_<User>.img` adresindeki `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, ve değişiklikleri otomatik olarak eşitler. 
* Belirtilen dosya paylaşımınızı bağlamak `clouddrive` içinde `$Home` doğrudan dosya paylaşımı etkileşimi için dizin. `/Home/<User>/clouddrive` eşlenmiş `fileshare.storage.windows.net/fileshare`.
 
> [!NOTE]
> Tüm dosyaları, `$Home` SSH anahtarları gibi dizini, bağlı dosya paylaşımında depolanabilir, kullanıcı disk görüntüsü kaldı. Bilgileri kalıcı hale en iyi yöntemleri uygulayın, `$Home` dizin ve bağlı dosya paylaşımı.

## <a name="bash-specific-commands"></a>Bash-özel komutlar

### <a name="use-the-clouddrive-command"></a>Kullanım `clouddrive` komutu
Cloud shell'de Bash ile adlı bir komut çalıştırabileceğiniz `clouddrive`, bağlı dosya paylaşımı için Cloud Shell el ile güncelleştirmenizi sağlar.
!["Clouddrive" komutu çalıştırılıyor](media/persisting-shell-storage/clouddrive-h.png)

### <a name="mount-a-new-clouddrive"></a>Yeni bir clouddrive bağlama

#### <a name="prerequisites-for-manual-mounting"></a>El ile bağlama için Önkoşullar
Cloud Shell ile kullanarak ilişkili dosya paylaşımı güncelleştirebilirsiniz `clouddrive mount` komutu.

Var olan bir dosya paylaşımı bağlarsanız, depolama hesaplarını seçme Cloud Shell bölgeniz bulunması gerekir. Konumu almak `env` Bash ve denetimi `ACC_LOCATION`.

#### <a name="the-clouddrive-mount-command"></a>`clouddrive mount` Komutu

> [!NOTE]
> Yeni bir dosya paylaşımı bağlama, yeni bir kullanıcı görüntüsü için oluşturulur, `$Home` dizin. Önceki `$Home` görüntü, önceki dosya paylaşımınızdaki tutulur.

Çalıştırma `clouddrive mount` komutunu aşağıdaki parametrelerle:

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

Daha fazla ayrıntı görüntülemek için çalıştırın `clouddrive mount -h`, burada gösterildiği gibi:

![Çalışan ' clouddrive mount'command](media/persisting-shell-storage/mount-h.png)

### <a name="unmount-clouddrive"></a>Clouddrive çıkarın
Cloud Shell, herhangi bir zamanda takılı bir dosya paylaşımı çıkarma. Cloud Shell kullanılacak bir bağlı dosya paylaşımı gerektirdiğinden, oluşturma ve bir sonraki oturum başka bir dosya paylaşımını bağlama istenir.

1. `clouddrive unmount` öğesini çalıştırın.
2. Onayla ve bir istemi onaylayın.

Dosya paylaşımınızın el ile silmediğiniz sürece var olmaya devam eder. Cloud Shell'i üzerinde sonraki her oturumda bu dosya paylaşımı için artık arama yapar. Daha fazla ayrıntı görüntülemek için çalıştırın `clouddrive unmount -h`, burada gösterildiği gibi:

![Çalışan ' clouddrive unmount'command](media/persisting-shell-storage/unmount-h.png)

> [!WARNING]
> Bu komutu çalıştırarak tüm kaynakları silmez ancak el ile bir kaynak grubu, depolama hesabı veya Cloud shell'e eşlenmiş dosya paylaşımını silme siler, `$Home` dizin disk görüntüsü ve herhangi bir dosyayı dosya paylaşımınızdaki. Bu eylem geri alınamaz.

### <a name="list-clouddrive"></a>Liste `clouddrive`
Hangi dosya paylaşımı olarak bağlanmıştır bulunacak `clouddrive`çalıştırın `df` komutu. 

URL'de depolama hesabı adı ve dosya paylaşım clouddrive dosya yolunu gösterir. Örneğin, `//storageaccountname.file.core.windows.net/filesharename`

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
## <a name="powershell-specific-commands"></a>Özel PowerShell komutları

### <a name="list-clouddrive-azure-file-shares"></a>Liste `clouddrive` Azure dosya paylaşımları
`Get-CloudDrive` Cmdlet'i tarafından şu anda bağlanan Azure dosya paylaşımı bilgileri alır `clouddrive` Cloud shell'de. <br>
![Get-CloudDrive çalıştırma](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

### <a name="unmount-clouddrive"></a>Çıkarın `clouddrive`
Cloud Shell'i herhangi bir zamanda bağlanan Azure dosya paylaşımını çıkarma. Azure dosya paylaşımının kaldırılması halinde oluşturabilir ve bir sonraki oturumda yeni bir Azure dosya paylaşımını bağlama istenir.

`Dismount-CloudDrive` Cmdlet'i, geçerli bir depolama hesabından Azure dosya paylaşımını çıkarır. Kaldırma `clouddrive` geçerli oturumu sona erer. Kullanıcı oluşturma ve bir sonraki oturumu sırasında yeni bir Azure dosya paylaşımını bağlama istenir.
![Çıkarma CloudDrive çalıştırma](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!INCLUDE [PersistingStorage-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

Not: Bir dosyada bir fonksiyon tanımlayın ve PowerShell cmdlet'lerinden çağırmak gerekiyorsa, nokta işleci dahil edilmelidir. Örneğin:. .\MyFunctions.ps1

## <a name="next-steps"></a>Sonraki adımlar
[Bash cloud Shell hızlı başlangıçta](quickstart.md) <br>
[PowerShell Cloud Shell hızlı başlangıç](quickstart-powershell.md) <br>
[Microsoft Azure dosya depolama hakkında bilgi edinin](https://docs.microsoft.com/azure/storage/storage-introduction) <br>
[Depolama etiketleri hakkında bilgi edinin](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>
