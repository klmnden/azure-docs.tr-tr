---
title: Uygulama hizmeti Azure yığın güncelleştirme 1 sürüm notları | Microsoft Docs
description: Azure yığın uygulama hizmeti için güncelleştirme nedir hakkında bilgi edinin bilinen sorunlar ve güncelleştirme karşıdan yükleme konumu.
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
ms.date: 03/20/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: 80bd865b7a08d9488c0fb6a1a5b60445b9c6eaaa
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "34358090"
---
# <a name="app-service-on-azure-stack-update-1-release-notes"></a>Uygulama hizmeti Azure yığın güncelleştirme 1 sürüm notları

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bu sürüm notları, Azure App Service düzeltmeler ve geliştirmeler Azure yığın güncelleştirme 1 ve bilinen sorunlar açıklanmaktadır. Bilinen sorunlar doğrudan dağıtım, güncelleştirme işlemi ve sorunları yapı (yükleme sonrası) ile ilgili sorunlar ayrılır.

> [!IMPORTANT]
> Azure tümleşik yığını sisteminizi 1802 güncelleştirmesini veya Azure uygulama hizmeti dağıtmadan önce en son Azure yığın Geliştirme Seti dağıtın.
>
>

## <a name="build-reference"></a>Derleme başvurusu

Uygulama hizmeti Azure yığın güncelleştirme 1 yapı numarası olduğu **69.0.13698.9**

### <a name="prerequisites"></a>Önkoşullar

> [!IMPORTANT]
> Azure uygulama hizmeti Azure yığında yeni dağıtımı şimdi gerektiren bir [üç konulu bir joker sertifika](azure-stack-app-service-before-you-get-started.md#get-certificates) içinde SSO Kudu için şimdi işlenir Azure App Service'te şekilde geliştirmeleri nedeniyle. Yeni konu  **\*. sso.appservice.\< Bölge\>.\< DomainName\>.\< uzantısı\>**
>
>

Başvurmak [önce Get Started belgelerine](azure-stack-app-service-before-you-get-started.md) dağıtımına başlamadan önce.

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Azure uygulama hizmeti Azure yığın güncelleştirme 1 aşağıdaki geliştirmeleri ve düzeltmeler içerir:

- **Yüksek kullanılabilirlik, Azure App Service** -arasında dağıtılacak şekilde Azure yığın 1802 etkin güncelleştirme iş yükleri hata etki alanları. Bu nedenle uygulama hizmeti hata etki alanlarında dağıtılacak hataya dayanıklı olması mümkün altyapısıdır. Varsayılan olarak Azure App Service, tüm yeni dağıtımlar olan bu yetenek Azure yığın 1802 önce tamamlandı dağıtımlar için uygulanan güncelleştirme başvurmak ancak [App Service hata etki alanı belgeleri](azure-stack-app-service-fault-domain-update.md)

- **Mevcut sanal ağda dağıtmak** -müşteriler var olan bir sanal ağ içindeki Azure yığın uygulama hizmeti şimdi dağıtabilir. Varolan bir sanal ağı dağıtma özel bağlantı noktaları üzerinden Azure uygulama hizmeti için gerekli bir dosya sunucusu ve SQL Server için bağlanmasına olanak sağlar. Dağıtım sırasında var olan bir sanal ağ içinde ancak dağıtmak için müşteriler seçebilirsiniz [uygulama hizmeti tarafından kullanım için alt ağlar oluşturmanız gerekir](azure-stack-app-service-before-you-get-started.md#virtual-network) dağıtımından önce.

- Güncelleştirmeleri **uygulama hizmet Kiracı, yönetim işlevleri portalları ve Kudu Araçları**. Azure yığın portalı SDK sürümü ile tutarlı.

- **Aşağıdaki uygulama çerçeveleri ve Araçları güncelleştirmelerini**:
    - Eklenen **.Net 2.0 çekirdek** desteği
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
        - 7.0.26 (x86 hem x64)
        - 7.1.12 (x86 hem x64)
    - Güncelleştirilmiş **Windows için Git** v 2.14.1
    - Güncelleştirilmiş **Mercurial** v4.5.0 için

  - Desteği eklendi **yalnızca HTTPS** uygulama servis Kiracı Portalı'nda özel etki alanı özelliği içindeki özelliği. 

  - Azure işlevleri için özel depolama seçicisinde depolama bağlantısı eklenen doğrulama 

#### <a name="fixes"></a>Düzeltmeler

- Bir çevrimdışı dağıtım paketi oluştururken, müşteriler artık erişim reddedildi klasörü uygulama hizmeti Yükleyicisi'nden açılırken hata iletisi almaz

- Uygulama hizmeti Kiracı portalı özel etki alanları özelliğinde çalışırken sorunları çözümlendi.

- Kurulum sırasında ayrılmış yönetici adları kullanan müşteriler engelle

- Uygulama hizmeti dağıtımı ile etkin **etki alanına katılmış** dosya sunucusu

- Azure yığın kök geliştirilmiş alınmasını komut dosyasında sertifika ve şimdi uygulama hizmeti yükleyici kök cert doğrulayın.

- Azure Resource Manager için bir abonelik olduğunda döndürülen sabit durumu yanlış Microsoft.Web ad alanında kapsanan bu kaynakları sildi.

### <a name="known-issues-with-the-deployment-process"></a>Dağıtım işlemi ile ilgili bilinen sorunlar

- Sertifika doğrulama hataları

Bazı müşteriler, sertifikaları, yükleyici aşırı kısıtlayıcı doğrulama nedeniyle tümleşik bir sistemde dağıtırken uygulama hizmeti yükleyici sağlanırken sorunları karşılaşmıştır. Uygulama Hizmeti Yükleyici yeniden yayımlandı, müşterilerin gereken [güncelleştirilmiş yükleyici indirmek](https://aka.ms/appsvconmasinstaller). Güncelleştirilmiş yükleyici sertifikalarla doğrulama sorunları yaşamaya devam ederseniz, desteğe başvurun.

- Tümleşik sistemden Azure yığın kök sertifikası alınırken bir sorun oluştu.

Get-AzureStackRootCert.ps1 hata müşteriler kök sertifikasının yüklü olmayan bir makineye komut dosyası yürütme zaman Azure yığın kök sertifikası almak başarısız olmasına neden oldu. Komut ayrıca artık, bu sorun ve istek müşteriler çözme yeniden yayımlandı [güncelleştirilmiş yardımcı komut dosyalarını indirme](https://aka.ms/appsvconmashelpers). Güncelleştirilmiş bir komut dosyası ile kök sertifika alma sorunları yaşamaya devam ederseniz, desteğe başvurun.

### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar

- Azure uygulama hizmeti Azure yığın güncelleştirme 1 güncelleştirmesi için bilinen herhangi bir sorun vardır.

### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

- Yuva değiştirmenin çalışmaz

Site yuvası takas bu sürümde ayrılır. İşlevselliği geri yüklemek için aşağıdaki adımları tamamlayın:

1. ControllersNSG ağ güvenlik grubu değiştirme **izin** uygulama hizmet denetleyicisi örnekleri için Uzak Masaüstü bağlantıları. AppService.local App Service'te dağıttığınız kaynak grubu adını değiştirin.

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

2. Göz atın **CN0 VM** sanal makineleri Azure yığın Yönetici portalı'nda altında ve **Bağlan'ı tıklatın** denetleyici örneği ile Uzak Masaüstü oturumu açın. Uygulama hizmeti dağıtım sırasında belirtilen kimlik bilgilerini kullanın.
3. Başlat **bir yönetici olarak PowerShell'i** ve aşağıdaki komut dosyası yürütme

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

4. Uzak Masaüstü oturumu kapatın.
5. ControllersNSG ağ güvenlik grubuna geri **reddetme** uygulama hizmet denetleyicisi örnekleri için Uzak Masaüstü bağlantıları. AppService.local App Service'te dağıttığınız kaynak grubu adını değiştirin.

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
- Uygulama hizmeti var olan bir sanal ağda dağıtılmış ve dosya sunucusu yalnızca özel ağda kullanılabilir dosya sunucusuna erişemedi çalışanlardır.
 
Varolan bir sanal ağı ve dosya sunucusuna bağlanmak için bir iç IP adresi dağıtmak seçerseniz, çalışan alt ağ ve dosya sunucusu arasında SMB trafiği etkinleştirme bir giden güvenlik kuralı eklemeniz gerekir. Bunu yapmak için yönetim portalında WorkersNsg gidin ve aşağıdaki özelliklere sahip bir giden güvenlik kuralı ekleyin:
 * Kaynak: tüm
 * Kaynak bağlantı noktası aralığı: *
 * Hedef: IP adresleri
 * Hedef IP adresi aralığı: Dosya sunucunuz için IP aralığı
 * Hedef bağlantı noktası aralığı: 445
 * Protokol: TCP
 * Eylem: izin ver
 * Öncelik: 700
 * Ad: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Bulut Azure uygulama hizmeti Azure yığında işletim yöneticileri için bilinen sorunlar

Belgelerde başvurmak [Azure yığın 1802 sürüm notları](azure-stack-update-1802.md)

## <a name="next-steps"></a>Sonraki adımlar

- Azure uygulama hizmeti genel bakış için bkz: [Azure uygulama hizmeti Azure yığın genel bakış](azure-stack-app-service-overview.md).
- Azure yığın uygulama hizmeti dağıtmak hazırlanması hakkında daha fazla bilgi için bkz: [Azure yığın uygulama hizmeti ile çalışmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md).
