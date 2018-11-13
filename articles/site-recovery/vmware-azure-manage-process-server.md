---
title: VMware vm'lerinin olağanüstü durum kurtarma için bir işlem sunucusu ve fiziksel sunucuları azure'a Azure Site Recovery kullanarak yönetme | Microsoft Docs
description: Bu makalede VMware Vm'lerinin ve fiziksel sunucudan azure'a Azure Site Recovery ile olağanüstü durum kurtarma için ayarlanmış bir işlem sunucusunu.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: d99b5d1fdca39466d5e09ca077329b7ffa8622bc
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568861"
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
  ```powershell
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

## <a name="manage-anti-virus-software-on-process-servers"></a>Virüsten koruma yazılımı işlem sunucularını yönetme

Virüsten koruma yazılımının bir tek başına işlem sunucusu veya ana hedef sunucu üzerinde etkin değilse, aşağıdaki klasörleri virüsten koruma işlemlerinden dışlayacak:


- C:\Program Files\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site kurtarma
- İşlem sunucusu yükleme dizininde, örnek: C:\Program Files (x86) \Microsoft Azure Site Recovery

