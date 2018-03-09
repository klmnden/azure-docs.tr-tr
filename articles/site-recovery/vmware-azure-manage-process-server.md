---
title: "Azure Site kurtarma işlemi sunucuyu yönetme | Microsoft Docs"
description: "Bu makalede VMware sanal ve fiziksel sunucu çoğaltma Azure Site kurtarma için ayarlanmış bir işlem sunucusu yönetin."
services: site-recovery
author: AnoopVasudavan
manager: gauravd
editor: 
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: 096b2890d41402448809ae87759fcd6b67bee2fe
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="manage-process-servers"></a>İşlem sunucularını yönetme

Varsayılan olarak, VMware Vm'lerini veya fiziksel sunucuları Azure'a çoğaltırken kullanılan işlem sunucusu şirket içi yapılandırma sunucusu makineye yüklenir. Birkaç ayrı işlem sunucusu kurmak gereken örnekleri şunlardır:

- Büyük dağıtımlar için kapasite ölçeklendirmek için ek şirket içi işlem sunucuları gerekebilir.
- Yeniden çalışma için ihtiyacınız geçici bir işlem sunucusu Azure'da ayarlayın. Yeniden çalışma yapıldığında bu VM'yi silebilirsiniz. 

Bu makalede, bu ek işlem sunucular için tipik yönetim görevleri özetlenmektedir.

## <a name="upgrade-a-process-server"></a>Bir işlem sunucusunu yükseltme

Şirket içinde veya Azure (yeniden çalışma nedeniyle), aşağıdaki gibi çalışan bir işlem sunucusu yükseltme:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

> [!NOTE]
  Genellikle, bir işlem sunucusu yeniden çalışma amaçları doğrultusunda oluşturmak için Azure galeri görüntüsü kullandığınızda kullanılabilir en son sürümü çalışıyor. Site Recovery yayın düzeltmeler ve geliştirmeler düzenli olarak ekipleri ve işlem sunucuları güncel tutmak öneririz.



## <a name="reregister-a-process-server"></a>Bir işlem sunucusu yeniden kaydetme

Şirket içi çalışan bir işlem sunucusu kaydetmeden veya Azure'da, yapılandırma sunucusuyla aşağıdakileri yapmanız gerekirse:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

Ayarları kaydettikten sonra aşağıdakileri yapın:

1. İşlem sunucusunda bir yönetici komut istemi açın.
2. Klasöre Gözat **%PROGRAMDATA%\ASR\Agent**, ve şu komutu çalıştırın:

    ```
    cdpcli.exe --registermt
    net stop obengine
    net start obengine
    ```

## <a name="modify-proxy-settings-for-an-on-premises-process-server"></a>Bir şirket içi işlem sunucusu için proxy ayarlarını değiştirme

İşlem sunucusu, Azure Site Recovery bağlanmak için bir proxy kullanıyorsa, var olan proxy ayarlarını değiştirmeniz gerekiyorsa bu yordamı kullanın.

1. İşlem sunucusu makine oturum açın. 
2. Bir yönetici PowerShell komut penceresi açın ve aşağıdaki komutu çalıştırın:
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
2. Klasöre Gözat **%PROGRAMDATA%\ASR\Agent**, ve aşağıdaki komutu çalıştırın:
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```


## <a name="remove-a-process-server"></a>Bir işlem sunucusunu kaldırma

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]


