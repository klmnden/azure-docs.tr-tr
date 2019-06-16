---
title: VMware Vm'lerini ve fiziksel sunucuları Azure Site Recovery ile azure'a olağanüstü durum kurtarma için kullanılan bir işlem sunucusunu | Microsoft Docs
description: Bu makalede VMware Vm'lerinin ve fiziksel sunucuları Azure Site Recovery kullanarak azure'a olağanüstü durum kurtarma için ayarlanmış bir işlem sunucusunu.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/28/2019
ms.author: ramamill
ms.openlocfilehash: 2c27779719c73adf4d7fc1a61a0c77d03df71815
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64925598"
---
# <a name="manage-process-servers"></a>İşlem sunucularını yönetme

Bu makalede Site Recovery işlem sunucusunu yönetmek için ortak görevler açıklanmaktadır.

İşlem sunucusu, alma, en iyi duruma getirmek ve çoğaltma verileri Azure'a göndermek için kullanılır. Bu ayrıca Mobility hizmetinin göndererek yükleme VMware Vm'lerini ve fiziksel sunucuları çoğaltmak istediğiniz gerçekleştirir ve şirket içi makineleri otomatik olarak bulunmasını gerçekleştirir. Şirket içi VMware Vm'lerini veya fiziksel sunucuları Azure'a çoğaltmak için işlem sunucusu yapılandırma server makinesinde varsayılan olarak yüklenir. 

- Büyük dağıtımlar için kapasite ölçeklendirme şirket içinde ek işlem sunucularının gerekebilir.
- Yeniden çalışma için azure'dan şirket içine, azure'da geçici işlem sunucusu ayarlamanız gerekir. Yeniden çalışma tamamlandığında bu VM'yi silebilirsiniz. 

İşlem Sunucusu hakkında daha fazla bilgi edinin.


## <a name="upgrade-a-process-server"></a>Bir işlem sunucusunu yükseltme

İşlem sunucusu dağıtımı yaparken şirket içinde veya bir Azure VM yeniden çalışma için işlem sunucusu en son sürümü yüklü. Site Recovery yayın düzeltmeler ve geliştirmeler düzenli olarak ekiplerinin ve işlem sunucularının güncel tutmanızı öneririz. İşlem sunucusu aşağıdaki gibi yükseltebilirsiniz:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]


## <a name="move-vms-to-balance-the-process-server-load"></a>İşlem sunucusunun Yük Dengelemesi için sanal makineleri taşıma

VM'ler gibi iki işlem sunucusu arasında taşıyarak yükü dengelemek:

1. Bir kasadaki altında **Yönet** tıklayın **Site Recovery altyapısı**. Altında **VMware ve fiziksel makineler**, tıklayın **Configuration Servers**.
2. İşlem sunucusu ile kayıtlı yapılandırma sunucusunda tıklayın.
3. Trafiğin yükünü dengelemenizi istediğiniz işlem sunucusu tıklayın.

    ![Yük Dengeleme](media/vmware-azure-manage-process-server/LoadBalance.png)

4. Tıklayın **Yük Dengeleme**, makineyi taşımak istediğiniz hedef işlem sunucusunu seçin. Ardından **Tamam**

    ![LoadPS](media/vmware-azure-manage-process-server/LoadPS.PNG)

2. Tıklayın **makineleri**ve geçerli hedef işlem sunucusuna taşımak istediğiniz makineleri seçin. Ortalama veri değişikliği ayrıntılarını, her sanal makineye karşı görüntülenir. Daha sonra, **Tamam**'a tıklayın. 
3. Kasada altında işinin ilerleme durumunu izlemek **izleme** > **Site Recovery işleri**.

Değişikliklerin portalda yansıtılması için yaklaşık 15 dakika sürer. Daha hızlı bir efekt için [yapılandırma sunucusunu Yenile](vmware-azure-manage-configuration-server.md#refresh-configuration-server).

## <a name="switch-an-entire-workload-to-another-process-server"></a>Tüm bir iş yükünü başka bir işlem sunucusuna geçiş

Aşağıdaki gibi farklı bir işlem sunucusuna, bir işlem sunucusu tarafından işlenen tüm iş yükünü taşıyın:

1. Bir kasadaki altında **Yönet** tıklayın **Site Recovery altyapısı**. Altında **VMware ve fiziksel makineler**, tıklayın **Configuration Servers**.
2. İşlem sunucusu ile kayıtlı yapılandırma sunucusunda tıklayın.
3. İş yükü geçiş yapmak istediğiniz işlem sunucusu tıklayın.
4. Tıklayarak **anahtar**, iş yükü taşımak istediğiniz hedef işlem sunucusunu seçin. Ardından **Tamam**

    ![Anahtar](media/vmware-azure-manage-process-server/Switch.PNG)

5. Kasada altında işinin ilerleme durumunu izlemek **izleme** > **Site Recovery işleri**.

Değişikliklerin portalda yansıtılması için yaklaşık 15 dakika sürer. Daha hızlı bir efekt için [yapılandırma sunucusunu Yenile](vmware-azure-manage-configuration-server.md#refresh-configuration-server).



## <a name="reregister-a-process-server"></a>İşlem sunucusu yeniden kaydettirin

Şirket içinde çalışan bir işlem sunucusu yeniden kaydettirin veya aşağıdaki gibi yapılandırma sunucusu ile Azure sanal makinesinde:

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

Bir şirket içi işlem sunucusunu Azure'a bağlanmak için bir ara sunucu kullanıyorsa, proxy ayarları aşağıdaki gibi değiştirebilirsiniz:

1. İşlem sunucusunun makineye oturum açın. 
2. Bir yönetici PowerShell komut penceresi açın ve aşağıdaki komutu çalıştırın:
   ```powershell
   $pwd = ConvertTo-SecureString -String MyProxyUserPassword
   Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
   net stop obengine
   net start obengine
   ```
2. Klasöre göz atın **%PROGRAMDATA%\ASR\Agent**, ve şu komutu çalıştırın:
   ```
   cmd
   cdpcli.exe --registermt

   net stop obengine

   net start obengine

   exit
   ```

## <a name="remove-a-process-server"></a>Bir işlem sunucusunu Kaldır

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="exclude-folders-from-anti-virus-software"></a>Virüsten koruma yazılımından klasörleri Dışla

Virüsten koruma yazılımının bir genişleme işlem sunucusu (veya ana hedef sunucusu) üzerinde çalışıyorsa, aşağıdaki klasörleri virüsten koruma işlemlerinden dışlayacak:


- C:\Program Files\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site kurtarma
- İşlem sunucusu yükleme dizininde. Örneğin: C:\Program dosyaları (x86) \Microsoft Azure Site kurtarma