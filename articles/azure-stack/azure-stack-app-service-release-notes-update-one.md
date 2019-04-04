---
title: Güncelleştirme 1 sürüm notları Azure Stack üzerinde App Service'te | Microsoft Docs
description: Azure Stack'te App Service için güncelleştirme nedir hakkında bilgi edinin bilinen sorunlar ve nerede güncelleştirmeyi indirin.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: anwestg
ms.reviewer: sethm
ms.lastreviewed: 03/20/2018
ms.openlocfilehash: 3d75467da01f0672bb735e01cbd6d7634cdf843e
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445256"
---
# <a name="app-service-on-azure-stack-update-1-release-notes"></a>Güncelleştirme 1 sürüm notları Azure Stack üzerinde App Service'e

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu sürüm notları, Azure App Service'te düzeltmeleri ve geliştirmeleri ve Azure Stack güncelleştirme 1 bilinen sorunlar açıklanmaktadır. Bilinen sorunlar doğrudan dağıtım, güncelleştirme işlemi ve sorunları (yükleme sonrası) yapı ile ilgili sorunlar ayrılır.

> [!IMPORTANT]
> 1802 güncelleştirme, Azure Stack tümleşik sistemi için geçerli veya Azure App Service'ı dağıtmadan önce en son Azure Stack geliştirme Seti'ni dağıtın.
>
>

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack güncelleştirme 1 derleme numarası üzerinde App Service, **69.0.13698.9**

### <a name="prerequisites"></a>Önkoşullar

> [!IMPORTANT]
> Azure Stack'te Azure App Service'in yeni dağıtımlar artık gerektiren bir [üç konulu bir joker sertifikası](azure-stack-app-service-before-you-get-started.md#get-certificates) iyileştirmeleri, SSO için Kudu artık Azure App Service'te işlenme nedeniyle. Yeni konu  **\*. sso.appservice.\< Bölge\>.\< DomainName\>.\< Uzantı\>**
>
>

Başvurmak [önce Get Started belgeleri](azure-stack-app-service-before-you-get-started.md) dağıtımına başlamadan önce.

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure Stack güncelleştirme 1 üzerinde Azure App Service, aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- **Yüksek kullanılabilirlik, Azure App Service** -Azure Stack 1802 güncelleştirmenin etkin iş yükleri arasında dağıtılması için hata etki alanları. Bu nedenle App Service altyapısını hata etki alanlarına dağıtılır hataya dayanıklı olacak şekilde kuramıyor. Varsayılan olarak Azure App Service'in tüm yeni dağıtımlar olan bu özellik Azure Stack 1802 önce tamamlanan dağıtımları için uygulanan güncelleştirme başvurmak ancak [App Service hata etki alanı belgeleri](azure-stack-app-service-fault-domain-update.md)

- **Mevcut bir sanal ağ içinde dağıtma** -müşteriler artık olarak var olan bir sanal ağ içinde Azure Stack üzerinde App Service'e dağıtabilirsiniz. Mevcut bir sanal ağ dağıtma müşterilerin SQL Server ve dosya sunucusu, Azure App Service için özel bağlantı noktaları üzerinden gerekli bağlanmasını sağlar. Dağıtım sırasında ancak mevcut bir sanal ağ içinde dağıtmak için müşteriler seçebilir [App Service tarafından kullanılacak alt ağ oluşturmanız gerekir](azure-stack-app-service-before-you-get-started.md#virtual-network) dağıtımından önce.

- Güncelleştirmeleri **App Service Kiracı, yönetici, İşlevler portalları ve Kudu Araçları**. Azure Stack portalı SDK sürümü ile tutarlı.

- Güncelleştirmeleri **Azure işlevleri çalışma zamanı** için **v1.0.11388**.

- **Aşağıdaki uygulama çerçeveleri ve araçları güncelleştirmeleri**:
    - Eklenen **.NET Core 2.0** desteği
    - Eklenen **Node.JS** sürümleri:
        - 6.11.2
        - 6.11.5
        - 7.10.1
        - 8.0.0
        - 8.1.4
        - 8.4.0
        - 8.5.0
        - 8.7.0
        - 8.8.1
        - 8.9.0
    - Eklenen **NPM** sürümleri:
        - 3.10.10
        - 4.2.0
        - 5.0.0
        - 5.0.3
        - 5.3.0
        - 5.4.2
        - 5.5.1
    - Eklenen **PHP** güncelleştirmeleri:
        - 5.6.32
        - 7.0.26 (x86 ve x64)
        - 7.1.12 (x86 ve x64)
    - Güncelleştirilmiş **Git için Windows** V'ye 2.14.1
    - Güncelleştirilmiş **Mercurial** v4.5.0 için

  - İçin destek eklendi **yalnızca HTTPS** App Service Kiracı Portalı'nda özel etki alanı özelliği içinde özelliği. 

  - Azure işlevleri için özel depolama seçicisinde depolama bağlantısı eklendi doğrulama 

#### <a name="fixes"></a>Düzeltmeleri

- Bir çevrimdışı dağıtım paketini oluştururken, müşteriler artık erişim reddedildi App Service Yükleyicisi'nden, klasör açılırken hata iletisi alırsınız

- App Service Kiracı portalının özel etki alanları özelliğinde çalışırken sorun çözüldü.

- Kurulum sırasında ayrılmış yönetici adları kullanan müşteriler engelle

- App Service dağıtımı ile etkin **etki alanına katılmış** dosya sunucusu

- Azure Stack kök geliştirilmiş alınmasını, komut dosyasında sertifika ve şimdi App Service içindeki kök sertifika doğrulayın.

- Azure Resource Manager için bir abonelik olduğunda döndürülen sabit durumu yanlış, içerdiği kaynaklarla Microsoft.Web ad alanındaki silindi.

### <a name="known-issues-with-the-deployment-process"></a>Dağıtım işlemi ile ilgili bilinen sorunlar

- Sertifika doğrulama hataları

Bazı müşteriler yükleyici aşırı kısıtlayıcı doğrulama nedeniyle tümleşik bir sistem üzerinde dağıtırken sertifikaları App Service yükleyicinin sağlanırken sorunlar yaşamış. App Service yükleyici yeniden yayımlandı, müşterilerin gereken [güncelleştirilmiş yükleyici indirin](https://aka.ms/appsvconmasinstaller). Güncelleştirilmiş Yükleyici ile sertifikalarını doğrularken sorun yaşamaya devam ederseniz desteğe başvurun.

- Azure Stack kök sertifikasını tümleşik sistemden alınırken bir sorun oluştu.

Get-AzureStackRootCert.ps1 hata müşteriler kök sertifika yüklü olmayan bir makinede komut dosyası yürütme zaman Azure Stack kök sertifikasını almak başarısız olmasına neden oldu. Betik ayrıca artık, bu sorun ve istek müşteriler çözümleme yeniden yayımlandı [güncelleştirilmiş yardımcı betikleri indirin](https://aka.ms/appsvconmashelpers). Güncelleştirilmiş betiği ile kök sertifika alınırken sorun yaşamaya devam ederseniz desteğe başvurun.

### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar

- Azure App Service'in Azure Stack güncelleştirme 1 güncelleştirmesi için bilinen bir sorun vardır.

### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

- Yuvası takas çalışmaz

Bu sürümde site yuvası takas ayrılır. İşlevselliğini geri yüklemek için aşağıdaki adımları tamamlayın:

1. ControllersNSG ağ güvenlik grubunu değiştirme **izin** Uzak Masaüstü bağlantıları için App Service denetleyici örneği. AppService.local, App Service'te dağıttığınız kaynak grubunun adıyla değiştirin.

    ```powershell
      Add-AzureRmAccount -EnvironmentName AzureStackAdmin

      $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"

      $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"

      Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
        -Name $RuleConfig_Inbound_Rdp_3389.Name `
        -Description "Inbound_Rdp_3389" `
        -Access Allow `
        -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
        -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
        -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
        -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
        -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
        -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
        -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange

      # Commit the changes back to NSG
      Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

2. Göz atın **CN0 VM** altında Azure Stack Yönetici portalı'nda sanal makineler ve **Bağlan** denetleyici örneği ile bir Uzak Masaüstü oturumu açın. App Service dağıtımı sırasında belirtilen kimlik bilgilerini kullanın.
3. Başlangıç **yönetici olarak PowerShell** ve aşağıdaki betiği çalıştırın

    ```powershell
        Import-Module appservice

        $sm = new-object Microsoft.Web.Hosting.SiteManager

        if($sm.HostingConfiguration.SlotsPollWorkerForChangeNotificationStatus=$true)
        {
          $sm.HostingConfiguration.SlotsPollWorkerForChangeNotificationStatus=$false
        #  'Slot swap mode reverted'
        }
        
        # Confirm new setting is false
        $sm.HostingConfiguration.SlotsPollWorkerForChangeNotificationStatus
        
        # Commit Changes
        $sm.CommitChanges()

        Get-AppServiceServer -ServerType ManagementServer | ForEach-Object Repair-AppServiceServer
        
    ```

4. Uzak Masaüstü oturumunu kapatın.
5. ControllersNSG ağ güvenlik grubuna geri **Reddet** Uzak Masaüstü bağlantıları için App Service denetleyici örneği. AppService.local, App Service'te dağıttığınız kaynak grubunun adıyla değiştirin.

    ```powershell

        Add-AzureRmAccount -EnvironmentName AzureStackAdmin

        $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"

        $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
          -Name $RuleConfig_Inbound_Rdp_3389.Name `
          -Description "Inbound_Rdp_3389" `
          -Access Deny `
          -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
          -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
          -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
          -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
          -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
          -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
          -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange

        # Commit the changes back to NSG
        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

6. Çalışanları App Service, var olan bir sanal ağda dağıtılır ve dosya sunucusu yalnızca özel ağda kullanılabilir dosya sunucusuna erişemiyor.

Mevcut bir sanal ağ ve dosya sunucunuza bağlanmak için bir dahili IP adresine dağıtmayı seçerseniz, çalışan alt ağ ve dosya sunucusu arasında SMB trafiği etkinleştirme bir giden güvenlik kuralı eklemeniz gerekir. Bunu yapmak için Yönetim Portalı'nda WorkersNsg gidin ve aşağıdaki özelliklere sahip bir giden güvenlik kuralı ekleyin:

- Kaynak: Herhangi biri
- Kaynak bağlantı noktası aralığı: *
- Hedef: IP Adresleri
- Hedef IP adresi aralığı: Dosya sunucusu için IP aralığı
- Hedef bağlantı noktası aralığı: 445
- Protokol: TCP
- Eylem: İzin Ver
- Önceliği: 700
- Ad: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Azure Stack üzerinde Azure App Service'te çalışan bulut yöneticileri için bilinen sorunlar

Belgeye başvurun [Azure Stack 1802 sürüm notları](azure-stack-update-1802.md)

## <a name="next-steps"></a>Sonraki adımlar

- Azure App Service'e genel bakış için bkz. [Azure App Service, Azure Stack genel bakış](azure-stack-app-service-overview.md).
- Azure Stack üzerinde App Service'e dağıtmak hazırlanması hakkında daha fazla bilgi için bkz. [Azure Stack'te App Service ile çalışmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md).
