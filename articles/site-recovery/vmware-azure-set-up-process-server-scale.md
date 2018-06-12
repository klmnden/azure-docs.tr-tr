---
title: VMware VM ve Azure Site Recovery ile fiziksel sunucuya yeniden çalışma için bir işlem sunucusu Azure ayarlama | Microsoft Docs
description: Bu makalede, azure'da yeniden çalışma VMware Azure Vm'lerine bir işlem sunucusu kurma açıklar.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 06/10/2018
ms.author: raynew
ms.openlocfilehash: 3e53954341136a293052f9af755515a5552432fe
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300856"
---
# <a name="set-up-additional-process-servers-for-scalability"></a>Ölçeklenebilirlik sağlamak için ek işlem sunucuları ayarlama

Varsayılan olarak, VMware Vm'lerini veya fiziksel sunucuları Azure kullanmaya çoğaltırken, [Site Recovery](site-recovery-overview.md), bir işlem sunucusu yapılandırma sunucusu makineye yüklenir ve Site Recovery arasında veri aktarımını koordine etmek için kullanılır ve Şirket içi altyapınızı. Kapasite ve çoğaltma dağıtımınızın genişletme artırmak için ek tek başına işlem sunucuları ekleyebilirsiniz. Bu makalede bunun nasıl yapılacağı açıklanır.

## <a name="before-you-start"></a>Başlamadan önce

### <a name="capacity-planning"></a>Kapasite planlaması

Gerçekleştirdiğiniz emin olun [kapasite planlaması](site-recovery-plan-capacity-vmware.md) VMware çoğaltma için. Bu, belirlemenize yardımcı olabilir nasıl ve ne zaman ek işlem sunucusu dağıtmanız gerekir.

### <a name="sizing-requirements"></a>Boyutlandırma gereksinimleri 

Tabloda özetlenen boyutlandırma gereksinimlerini doğrulayın. Genel olarak, 200'den fazla kaynak makinelere dağıtımınız ölçeklendirmek için olması veya 2 TB'den fazla oranını günlük karmaşıklığı toplam varsa, trafik hacmi işlemek için ek işlem sunucuları gerekir.

| **Ek işlem sunucusu** | **Önbellek disk boyutu** | **Veri değişikliği oranı** | **Korumalı makineler** |
| --- | --- | --- | --- |
|4 Vcpu (2 yuva * 2,5 GHz @ 2 Çekirdek), 8 GB bellek |300 GB |250 GB veya daha az |85 veya daha az makine çoğaltılır. |
|8 Vcpu'lar (2 yuva * @ 2,5 GHz 4 çekirdek), 12 GB bellek |600 GB |1 TB ' 250 GB |85 150 makineler arasında çoğaltılır. |
|12 Vcpu'lar (2 yuva * 2,5 GHz @ 6 çekirdek) 24 GB bellek |1 TB |1 TB ile 2 TB |150-225 makineler arasında çoğaltılır. |

### <a name="prerequisites"></a>Önkoşullar

Ek işlem sunucusu için Önkoşulları aşağıdaki tabloda özetlenmiştir.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]


## <a name="download-installation-file"></a>Yükleme dosyasını indirin

İşlem sunucusu için yükleme dosyasını aşağıdaki gibi indirin:

1. Azure portalında oturum açın ve kurtarma Hizmetleri Kasası'na göz atın.
2. Açık **Site Recovery altyapısı** > **VMWare ve fiziksel makineleri** > **yapılandırma sunucularına** (altında VMware ve fiziksel için Makineler).
3. Sunucu ayrıntıları detaya gitmek için yapılandırma sunucusu seçin. Ardından **+ işlem sunucusu**.
4. İçinde **eklemek işlem sunucusu** >  **işlem sunucunuzu dağıtmak istediğiniz Seç**seçin **dağıtma bir genişleme işlem sunucusu şirket içi**.

  ![Sunucuları Sayfası Ekle](./media/vmware-azure-set-up-process-server-scale/add-process-server.png)
1. Tıklatın **karşıdan Microsoft Azure Site kurtarma birleşik Kurulum**. Bu yükleme dosyasının en son sürümünü yükler.

  > [!WARNING]
  İşlem sunucusu yükleme sürümü aynı, veya öncesi olması gerekir, yapılandırma server sürümü çalışıyor olması. Sürüm uyumluluğu sağlamak için basit bir yol, en son yüklemek veya yapılandırma sunucunuz güncelleştirmek için kullanılan aynı yükleyici kullanmaktır.

## <a name="install-from-the-ui"></a>Kullanıcı Arabiriminden yükleyin

Aşağıdaki gibi yükleyin. Sunucuyu ayarladıktan sonra bunu kullanmak için kaynak makineleri geçirin.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="install-from-the-command-line"></a>Komut satırından yükleyin

Aşağıdaki komutu çalıştırarak yükleyin:

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

Burada komut satırı parametreleri aşağıdaki gibidir:

[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]

Örneğin:

```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```
### <a name="create-a-proxy-settings-file"></a>Proxy ayarları dosyası oluşturma

Bir proxy ayarlamak gerekiyorsa, ProxySettingsFilePath parametresi bir dosya girdi olarak alır. Dosyanın aşağıdaki gibi oluşturun ve giriş ProxySettingsFilePath parametresi olarak geçirin.

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
