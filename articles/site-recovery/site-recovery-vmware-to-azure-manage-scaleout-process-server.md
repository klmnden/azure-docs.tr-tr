---
title: " Bir genişleme işlem sunucusu Azure Site kurtarma yönetme | Microsoft Docs"
description: "Bu makalede, bir genişleme işlem sunucusu Azure Site kurtarma ayarlamanıza açıklar."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 02/22/2018
ms.author: anoopkv
ms.openlocfilehash: b2c2f8c6a10ca5098956de2402925bd9422212f8
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="manage-a-scale-out-process-server"></a>Bir genişleme işlem sunucusu yönetme

Genişleme işlem sunucusu Site kurtarma Hizmetleri ve şirket içi altyapınızı arasında veri aktarmak için bir düzenleyici gibi davranır. Bu makalede nasıl ayarlamak, yapılandırabilir ve bir genişleme işlem sunucusu yönetmek açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar
Önerilen donanım, yazılım ve bir genişleme işlem sunucusu kurmak için gereken ağ yapılandırması şunlardır:

> [!NOTE]
> [Kapasite planlama](site-recovery-capacity-planner.md) yapılandırmasıyla genişleme işlem sunucusu bu paketleri dağıtmak emin olmak için önemli bir adım yük gereksinimlerinizi. Daha fazla bilgi edinin [özellikleri için bir genişleme işlem sunucusu ölçeklendirme](#sizing-requirements-for-a-configuration-server).

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-the-scale-out-process-server-software"></a>Genişleme işlem sunucusu yazılım indirme
1. Azure portalında oturum açın ve kurtarma Hizmetleri Kasası'na göz atın.
2. Gözat **Site Recovery altyapısı** > **yapılandırma sunucularına** (altında VMware ve fiziksel makineler için).
3. Yapılandırma sunucusunun ayrıntıları sayfasına detaya gitmek için yapılandırma sunucunuzu seçin.
4. Tıklatın **+ işlem sunucusu** düğmesi.
5. İçinde **eklemek işlem sunucusu** sayfasında, **dağıtma bir genişleme işlem sunucusu şirket içi** gelen seçeneği **işlem sunucunuzu dağıtmak istediğiniz yeri seçin** açılır.

  ![Sunucuları Sayfası Ekle](./media/site-recovery-vmware-to-azure-manage-scaleout-process-server/add-process-server.png)
6. Tıklatın **Microsoft Azure Site Recovery birleşik Kurulumu karşıdan** genişleme işlem sunucusu yükleme en son sürümünü indirmek için bağlantı.

  > [!WARNING]
  Genişleme işlem sunucusu sürümü eşit veya ortamınızda çalışan yapılandırma sunucusu sürümden daha düşük olmalıdır. Sürüm uyumluluğu sağlamak için basit bir yol, yapılandırma sunucusu yüklemek/güncelleştirmek için kullanılan aynı yükleyici BITS kullanmaktır.

## <a name="installing-and-registering-a-scale-out-process-server-from-gui"></a>Yükleme ve bir genişleme işlem sunucusu GUI'den kaydetme
Varsa dağıtımınızı 200 kaynak makine ya da birden fazla 2 TB toplam günlük karmaşıklık oranı ötesine genişletmek için trafik hacmi işlemek için ek işlem sunucuları gerekir.

Denetleme [boyut işlem sunucuları için öneriler](#size-recommendations-for-the-process-server)ve işlem sunucusu kurmak için bu yönergeleri izleyin. Sunucuyu ayarladıktan sonra bunu kullanmak için kaynak makineleri geçirin.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="installing-and-registering-a-scale-out-process-server-using-command-line"></a>Yükleme ve komut satırı kullanarak bir genişleme işlem sunucusu kaydetme

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

### <a name="sample-usage"></a>Örnek Kullanım
```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```

### <a name="scale-out-process-server-installer-command-line-arguments"></a>Genişleme işlem sunucusu Yükleyici komut satırı bağımsız değişkenleri.
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-proxy-settings-configuration-file"></a>Proxy ayarlarını yapılandırma dosyası oluşturma
ProxySettingsFilePath parametresi bir dosya girdi olarak alır. Aşağıdaki biçimi kullanarak dosya oluşturma ve giriş ProxySettingsFilePath parametresi olarak geçirin.
```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-scale-out-process-server"></a>Genişleme işlem sunucusu için proxy ayarlarını değiştirme
1. Genişleme işlem sunucusu uygulamasına oturum açın.
2. Bir yönetici PowerShell komut penceresi açın.
3. Aşağıdaki komutu çalıştırın
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
4. Dizine'yanındaki Gözat **%PROGRAMDATA%\ASR\Agent** ve aşağıdaki komutu çalıştırın
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```

## <a name="re-registering-a-scale-out-process-server"></a>Bir genişleme işlem sunucusu yeniden kaydetme
[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

* Sonraki bir yönetici komut istemi açın.
* Dizine gözatın **%PROGRAMDATA%\ASR\Agent** ve şu komutu çalıştırın

```
cdpcli.exe --registermt

net stop obengine

net start obengine
```

## <a name="upgrading-a-scale-out-process-server"></a>Bir genişleme işlem sunucusu yükseltme
[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

## <a name="decommissioning-a-scale-out-process-server"></a>Bir genişleme işlem sunucusu yetkisini alma
1. Emin olun:
  - Yapılandırma sunucusunun bağlantı durumunu gösterir olarak **bağlı** Azure portalında
  - İşlem sunucusunun yapılandırma sunucusu ile iletişim kurmak hala yapabiliyor.
2. İşlem sunucusu için bir yönetici olarak oturum açın
3. Denetim Masası açın > Program > programları Kaldır
4. Aşağıdaki verilen dizisi programlarda kaldırın:
  * Microsoft Azure Site kurtarma yapılandırması sunucu/işlem sunucusu
  * Microsoft Azure Site kurtarma yapılandırması sunucu bağımlılıkları
  * Microsoft Azure Kurtarma Hizmetleri Aracısı

Azure portalında yansıtacak şekilde işlem sunucusu silme işlemi için 15 dakika kadar sürebilir.

  > [!NOTE]
  İşlem sunucusu yapılandırma sunucusuyla iletişim kuramıyor olup olmadığını (Portalı'nda bağlantı durumu bağlantı kesildi), sonra da yapılandırma sunucusundan temizlemek için aşağıdaki adımları izlemeniz gerekir.

## <a name="unregistering-a-disconnected-scale-out-process-server-from-a-configuration-server"></a>Bağlantısı kesilmiş bir genişleme işlem sunucusu bir yapılandırma sunucusundan kaydını silme

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="sizing-requirements-for-a-scale-out-process-server"></a>Bir genişleme işlem sunucusu için gereksinimleri boyutlandırma

| **Ek işlem sunucusu** | **Önbellek disk boyutu** | **Veri değişikliği oranı** | **Korumalı makineler** |
| --- | --- | --- | --- |
|4 Vcpu (2 yuva * 2,5 GHz @ 2 Çekirdek), 8 GB bellek |300 GB |250 GB veya daha az |85 veya daha az makine çoğaltılır. |
|8 Vcpu'lar (2 yuva * @ 2,5 GHz 4 çekirdek), 12 GB bellek |600 GB |1 TB ' 250 GB |85 150 makineler arasında çoğaltılır. |
|12 Vcpu'lar (2 yuva * 2,5 GHz @ 6 çekirdek) 24 GB bellek |1 TB |1 TB ile 2 TB |150-225 makineler arasında çoğaltılır. |
