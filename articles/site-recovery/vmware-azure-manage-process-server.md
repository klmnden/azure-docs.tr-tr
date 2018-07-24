---
title: Azure Site recovery'de bir işlem sunucusunu | Microsoft Docs
description: Bu makalede VMware VM ve fiziksel sunucu çoğaltması Azure Site recovery'de için ayarlanmış bir işlem sunucusunu.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/21/2018
ms.author: ramamill
ms.openlocfilehash: f8368aab1bc979492143d77a191a31afef754c4c
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213019"
---
# <a name="manage-process-servers"></a>İşlem sunucularını yönetme

Varsayılan olarak VMware Vm'lerini veya fiziksel sunucuları Azure'a çoğaltırken kullanılan işlem sunucusu şirket içi yapılandırma sunucusu makineye yüklenir. Birkaç ayrı işlem sunucusu kurmak gereksinim örneklerinin vardır:

- Büyük dağıtımlar için kapasite ölçeklendirme şirket içinde ek işlem sunucularının gerekebilir.
- Yeniden çalışma için ihtiyacınız geçici bir işlem sunucusu Azure'da ayarlayın. Yeniden çalışma tamamlandığında bu VM'yi silebilirsiniz. 

Bu makalede, bu ek işlem sunucularının için tipik yönetim görevleri özetler.

## <a name="upgrade-a-process-server"></a>Bir işlem sunucusunu yükseltme

Şirket içinde veya azure'da (yeniden çalışma amaçları için), şu şekilde çalışan bir işlem sunucusu yükseltme:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

> [!NOTE]
  Genellikle, bir işlem sunucusu Azure'da yeniden çalışma amacıyla oluşturmak üzere Azure Galerisi görüntüsünü kullandığınızda, kullanılabilir en son sürümü çalışıyor. Site Recovery yayın düzeltmeler ve geliştirmeler düzenli olarak ekiplerinin ve işlem sunucularının güncel tutmanızı öneririz.



## <a name="reregister-a-process-server"></a>İşlem sunucusu yeniden kaydettirin

Şirket içinde çalışan bir işlem sunucusu yeniden kaydettirin veya Azure'da, yapılandırma sunucusu ile aşağıdakileri yapmanız gerekirse:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

Ayarları kaydettikten sonra aşağıdakileri yapın:

1. İşlem sunucusu, bir yönetici komut istemi açın.
2. Klasöre göz atın **%PROGRAMDATA%\ASR\Agent**, ve şu komutu çalıştırın:

    ```
    cdpcli.exe --registermt
    net stop obengine
    net start obengine
    ```

## <a name="modify-proxy-settings-for-an-on-premises-process-server"></a>Bir şirket içi işlem sunucusu için proxy ayarlarını değiştirme

İşlem sunucusu, Azure Site Recovery hizmetine bağlanmak için bir ara sunucu kullanıyorsa, var olan proxy ayarları değiştirmeniz gerekirse bu yordamı kullanın.

1. İşlem sunucusu makinede oturum açın. 
2. Bir yönetici PowerShell komut penceresi açın ve aşağıdaki komutu çalıştırın:
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
2. Klasöre göz atın **%PROGRAMDATA%\ASR\Agent**, ve aşağıdaki komutu çalıştırın:
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```


## <a name="remove-a-process-server"></a>Bir işlem sunucusunu Kaldır

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]


