---
title: VMware Vm'leri ve fiziksel sunucuları Azure Site Recovery ile olağanüstü durum kurtarma sırasında geri başarısız için azure'da bir işlem sunucusu ayarlama | Microsoft Docs
description: Bu makalede, azure'da bir işlem sunucusu Azure'dan şirket içi VMware Vm'leri ve fiziksel sunucuları olağanüstü durum kurtarma sırasında yeniden çalışma için nasıl ayarlanacağı açıklanır.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 4/9/2019
ms.author: ramamill
ms.openlocfilehash: 6849ffb6fa46365aa775b9410067cb0874c70ef8
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59362164"
---
# <a name="scale-for-failback-with-additional-process-servers"></a>Ek işlem sunucusu ile yeniden çalışma için ölçek

Varsayılan olarak, VMware Vm'lerini veya fiziksel sunucuları azure'a çoğaltırken [Site Recovery](site-recovery-overview.md), bir işlem sunucusu yapılandırma sunucusu makineye yüklenir ve Site Recovery arasında veri aktarımını koordine etmek için kullanılır ve Şirket içi altyapınızı. Kapasite ve çoğaltma dağıtımınızın ölçeğini artırmak için ek bağımsız işlem sunucuları ekleyebilirsiniz. Bu makalede bunun nasıl yapılacağı açıklanır.

## <a name="before-you-start"></a>Başlamadan önce

### <a name="capacity-planning"></a>Kapasite planlaması

Gerçekleştirdiğiniz emin [kapasite planlaması](site-recovery-plan-capacity-vmware.md) VMware çoğaltması için. Bu, tanımlamanıza yardımcı olur nasıl ve ne zaman ek işlem sunucusu dağıtmanız gerekir.

> [!NOTE]
> Kopyalanan bir işlem sunucusu bileşeni kullanımı desteklenmiyor. Her PS genişleme için bu makaledeki adımları izleyin.

### <a name="sizing-requirements"></a>Boyutlandırma gereksinimleri 

Tabloda özetlenen boyutlandırma gereksinimlerini doğrulayın. Genel olarak, dağıtımınızı 200'den fazla kaynak makineye ölçeklendirmek sahip olduğunuz veya günlük 2 TB'den fazla oranını karmaşıklığı toplam varsa, trafik hacmini işlemeye yetecek ek işlem sunucularının gerekir.

| **Ek işlem sunucusu** | **Önbellek diski boyutu** | **Veri değişiklik oranı** | **Korumalı makineler** |
| --- | --- | --- | --- |
|4 Vcpu (2 yuva * 2 Çekirdek \@ 2.5 GHz), 8 GB bellek |300 GB |250 GB veya daha az |85 ya da daha az makineleri çoğaltabilir. |
|8 Vcpu (2 yuva * 4 çekirdek \@ 2.5 GHz), 12 GB bellek |600 GB |250 GB ila 1 TB |85 150 makineler arasında çoğaltılır. |
|12 Vcpu (2 yuva * 6 çekirdek \@ 2.5 GHz) 24 GB bellek |1 TB |1 TB ile 2 TB |150-225 makineler arasında çoğaltılır. |

Her korumalı olduğunda, kaynak makinenin 100 GB'lık 3 diskleri ile yapılandırıldı.

### <a name="prerequisites"></a>Önkoşullar

Ek işlem sunucusu için Önkoşulları aşağıdaki tabloda özetlenmiştir.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]



## <a name="download-installation-file"></a>Yükleme dosyasını indirin

İşlem sunucusu için yükleme dosyasını aşağıdaki gibi indirin:

1. Azure portalında oturum açın ve kurtarma Hizmetleri kasanıza göz atın.
2. Açık **Site Recovery altyapısı** > **VMWare ve fiziksel makineler** > **yapılandırma sunucusu** (altında VMware ve fiziksel Makineler için).
3. Sunucu ayrıntıları detaya gitmek için yapılandırma sunucusunu seçin. Ardından **+ işlem sunucusu**.
4. İçinde **ekleme işlem sunucusu** >  **seçin, işlem sunucunuzu dağıtmak istediğiniz**seçin **Dağıt bir genişleme işlem sunucusu şirket içi**.

   ![Sunucuları Sayfası Ekle](./media/vmware-azure-set-up-process-server-scale/add-process-server.png)
1. Tıklayın **indirme Microsoft Azure Site Recovery birleşik Kurulumu**. Bu yükleme dosyasının en son sürümünü indirir.

   > [!WARNING]
   > Configuration server sürümü çalışıyor olması, işlem sunucusu yükleme sürümü aynı farklı veya daha önceki bir sürümü olmalıdır. Sürüm uyumluluğu sağlamak için basit bir yol, en son yükleme veya yapılandırma sunucunuzu güncelleştirmek için kullanılan aynı yükleyici kullanmaktır.

## <a name="install-from-the-ui"></a>Kullanıcı Arabiriminden yükleyin

Aşağıda gösterildiği gibi yükleyin. Sunucuyu ayarladıktan sonra bunu kullanmak için kaynak makineler geçirin.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="install-from-the-command-line"></a>Komut satırından yükleme

Aşağıdaki komutu çalıştırarak yükleyin:

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

Burada komut satırı parametreleri aşağıdaki gibidir:

[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]

Örneğin:

```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```
### <a name="create-a-proxy-settings-file"></a>Proxy ayarları dosyası oluşturma

Bir proxy ayarlamak gerekiyorsa ProxySettingsFilePath parametresi bir dosya girdi olarak alır. Dosya şu şekilde oluşturun ve bunu giriş ProxySettingsFilePath parametre olarak geçiriyoruz.

```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [işlem sunucusu ayarlarını yönetme](vmware-azure-manage-process-server.md)
