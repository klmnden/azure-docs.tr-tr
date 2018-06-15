---
title: Azure yığın 1802 güncelleştirme | Microsoft Docs
description: Azure yığın 1802 güncelleştirmesi nedir hakkında bilgi edinin tümleşik sistemleri, bilinen sorunlar ve güncelleştirme karşıdan yükleme konumu.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: af65ffc088c2beadf415b72ec284ef77f3e4f6d4
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757266"
---
# <a name="azure-stack-1802-update"></a>Azure yığın 1802 güncelleştirme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bu makalede yenilikleri ve bilinen sorunlar bu sürüm ve güncelleştirmeyi yüklemek nereye 1802 güncelleştirme paketindeki giderir. Bilinen sorunlar için güncelleştirme işlemini doğrudan ilgili sorunlar ve yapı (yükleme sonrası) ile ilgili sorunları ayrılır.

> [!IMPORTANT]        
> Bu güncelleştirme paketi, yalnızca Azure tümleşik yığını sistemleri içindir. Bu güncelleştirme paketi Azure yığın Geliştirme Seti için geçerli değildir.

## <a name="build-reference"></a>Derleme başvurusu    
Azure yığın 1802 güncelleştirme yapı numarası **20180302.1**.  


## <a name="before-you-begin"></a>Başlamadan önce    
> [!IMPORTANT]    
> Bu güncelleştirmenin yüklenmesi sırasında sanal makineler oluşturmak çalışmayın. Güncelleştirmeleri yönetme hakkında daha fazla bilgi için bkz: [yönetmek Azure yığın genel bakış güncelleştirmelerinde](azure-stack-updates.md#plan-for-updates).


### <a name="prerequisites"></a>Önkoşullar
- Azure yığın yükleme [1712 güncelleştirme](azure-stack-update-1712.md) Azure yığın 1802 güncelleştirmeyi uygulamadan önce.    

- Yükleme **AzS düzeltmesi – 1.0.180312.1-yapı 20180222.2** Azure yığın 1802 güncelleştirmeyi uygulamadan önce. Bu düzeltme, Windows Defender güncelleştirir ve Azure yığını için güncelleştirmeler olduğunda kullanılabilir.

  Düzeltmeyi yüklemek için normal yordamları izleyin [Azure yığını için güncelleştirmeleri yükleme](azure-stack-apply-updates.md). Güncelleştirme adı olarak görünür **AzS düzeltmesi – 1.0.180312.1**ve aşağıdaki dosyaları içerir: 
    - PUPackageHotFix_20180222.2-1.exe
    - PUPackageHotFix_20180222.2-1.bin
    - Metadata.XML

  Bu dosyaları bir depolama hesabı ve kapsayıcı dosyalarını karşıya yükledikten sonra Yönetim Portalı'nda güncelleştirme döşeme yükleme çalıştırın. 
  
  Azure yığınına yönelik güncelleştirmeler, bu güncelleştirmeyi yüklemeden Azure yığın sürümünü değiştirmez.  Bu güncelleştirmenin yüklendiğini doğrulamak için listesini görüntülemek **yüklü güncelleştirmeleri**.
 


### <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar
1802 yüklendikten sonra geçerli düzeltmeleri yükleyin. Daha fazla bilgi için aşağıdaki Bilgi Bankası makaleleri görüntülemek yanı sıra bizim [hizmet İlkesi](azure-stack-servicing-policy.md). 
- Azure yığın düzeltme **1.0.180302.4**. [KB 4131152 - var olan sanal makine ölçek kümeleri kullanılamaz hale gelebilir]( https://support.microsoft.com/help/4131152) 

  Bu düzeltmeyi de ayrıntılı sorunlarını giderir [KB 4103348 - bir Azure yığın güncelleştirme yüklemeye çalıştığınızda, Ağ denetleyicisi API'si Hizmeti kilitleniyor](https://support.microsoft.com/help/4103348).


### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler
Bu güncelleştirme aşağıdaki geliştirmeleri ve düzeltmeler için Azure yığınına içerir.

- **Aşağıdaki Azure depolama hizmeti API sürümleri için ek destek**:
    - 2017-04-17 
    - 2016-05-31 
    - 2015-12-11 
    - 2015-07-08 
    
    Daha fazla bilgi için bkz: [Azure yığın depolama: farklar ve konuları](/azure/azure-stack/user/azure-stack-acs-differences).

- **Desteği büyük [blok Blobları](azure-stack-acs-differences.md)**:
    - İzin verilen en büyük blok boyutu 4 MB 100 MB olarak artar.
    - En fazla blob boyutu 195 GB ile 4.75 TB'ye kadar artar.  

- **Altyapı yedekleme** artık kaynak sağlayıcıları parçasında görünür ve yedekleme etkin için uyarılar. Altyapı Backup hizmeti hakkında daha fazla bilgi için bkz: [altyapı Backup hizmeti ile Azure yığını için yedekleme ve veri kurtarma](azure-stack-backup-infrastructure-backup.md).

- **Güncelleştirme *Test AzureStack* cmdlet** depolama için tanılama geliştirmek için. Bu cmdlet hakkında daha fazla bilgi için bkz: [Azure yığını için doğrulama](azure-stack-diagnostic-test.md).

- **Rol tabanlı erişim denetimi (RBAC) geliştirmeleri** -AD FS ile Azure yığın dağıtıldığında Evrensel kullanıcı gruplarına izinlere temsilci seçmek için RBAC artık kullanabilirsiniz. RBAC hakkında daha fazla bilgi için bkz: [RBAC yönetme](azure-stack-manage-permissions.md).

- **Birden çok hata etki alanları için ek destek**.  Daha fazla bilgi için bkz: [Azure yığını için yüksek kullanılabilirlik](azure-stack-key-features.md#high-availability-for-azure-stack).

- **Fiziksel bellek yükseltme desteği** -ilk dağıtımınızdan sonra artık Azure tümleşik yığını sistem bellek kapasitesini genişletebilirsiniz. Daha fazla bilgi için bkz: [fiziksel bellek kapasitesi Azure yığınının yönetmek](azure-stack-manage-storage-physical-memory-capacity.md).

- **Çeşitli düzeltmeleri** performans, sağlamlık, güvenlik ve Azure yığını tarafından kullanılan işletim sistemi için.

<!--
#### New features
-->


<!--
#### Fixes
-->


### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar    
*Güncelleştirme 1802 yüklemesi için bilinen herhangi bir sorun vardır.*


### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)
Derleme için yükleme sonrası bilinen sorunlar verilmiştir **20180302.1**

#### <a name="portal"></a>Portal
- <!-- 2332636 - IS -->  When you use AD FS for your Azure Stack identity system and update to this version of Azure Stack, the default owner of the default provider subscription is reset to the built-in **CloudAdmin** user.  
  Geçici çözüm: Bu güncelleştirmeyi yükledikten sonra bu sorunu çözmek için 3 adımından kullanmak [yapılandırmak için tetikleyici Otomasyon Talep sağlayıcı güveni Azure yığınında](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) varsayılan sağlayıcı aboneliğin sahibi sıfırlamak için yordamı.   

- Özelliği [aşağı açılır listeden yeni bir destek isteği açma](azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen içinde Yönetici portalı kullanılamıyor. Bunun yerine, aşağıdaki bağlantıyı kullanın:     
    - Azure yığını için tümleşik sistemleri kullanan https://aka.ms/newsupportrequest.

- <!-- 2050709 --> In the admin portal, it is not possible to edit storage metrics for Blob service, Table service, or Queue service. When you go to Storage, and then select the blob, table, or queue service tile, a new blade opens that displays a metrics chart for that service. If you then select Edit from the top of the metrics chart tile, the Edit Chart blade opens but does not display options to edit metrics.

- Yönetici portalı'nda işlem ve depolama kaynaklarını görüntülemek mümkün olmayabilir. Bu sorunun nedeni yanlış başarılı olarak bildirilmesini güncelleştirme neden güncelleştirmenin yüklenmesi sırasında bir hata var. Bu sorun devam ederse, Yardım için Microsoft Müşteri Destek Hizmetleri'ne başvurun.

- Boş bir portal panosunda görebilirsiniz. Pano kurtarmak için portalın sağ üst köşesindeki dişli simgesini seçin ve ardından **varsayılan ayarları geri**.

- Kullanıcı abonelikleri yalnız bırakılmış kaynakları sonuçlarında siliniyor. Geçici bir çözüm olarak ilk kullanıcı kaynakları veya tüm kaynak grubunu silme ve kullanıcı abonelikleri silin.

- Azure yığın Portal kullanımı aboneliğinizi izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.

- Yönetim Portalı'nı panosunda güncelleştirmeler hakkında bilgi görüntülemek güncelleştirme döşeme başarısız olur. Bu sorunu çözmek için yenilemeniz kutucuğa tıklayın.

-   Yönetim Portalı'nda Microsoft.Update.Admin bileşeni için kritik bir uyarı görebilirsiniz. Uyarı adı, açıklama ve düzeltmesi tümü olarak görüntülenir:  
    - *HATA - FaultType ResourceProviderTimeout için şablon eksik.*

    Bu uyarı güvenle yoksayılabilir. 

- <!-- 2253274 --> In the admin and user portals, the Settings blade for vNet Subnets fails to load. As a workaround, use PowerShell and the [Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig?view=azurermps-5.5.0) cmdlet to view and  manage this information.

- Hem Yönetim Portalı ve Kullanıcı Portalı, daha eski bir API sürümüyle oluşturulmuş depolama hesapları için genel bakış dikey seçtiğinizde yüklemeye genel bakış dikey penceresinde başarısız (örnek: 2015-06-15). Bu sistem depolama hesapları gibi içerir **updateadminaccount** düzeltme ve güncelleştirme sırasında kullanılır. 

  Geçici bir çözüm olarak çalıştırmak için PowerShell kullanın **başlangıç ResourceSynchronization.ps1** için depolama hesabı ayrıntıları erişimi geri yüklemek için komut dosyası. [Komut dosyası Github'dan edinilebilir]( https://github.com/Azure/AzureStack-Tools/tree/master/Support/scripts)ve Hizmet Yöneticisi kimlik bilgileriyle ayrıcalıklı noktadaki çalıştırmanız gerekir. 

- **Hizmet durumu** dikey başarısız yüklenemiyor. Yönetici veya Kullanıcı Portalı'nda Azure yığın hizmet durumu dikey penceresini açtığınızda bir hata görüntüler ve bilgi yüklemez. Bu beklenen bir davranıştır. Seçin ve hizmetin sistem durumunu açın, ancak bu özellik henüz kullanılabilir değil ancak Azure yığın gelecek bir sürümünde uygulanacaktır.


#### <a name="health-and-monitoring"></a>Sistem durumu ve izleme
- <!-- 1264761 - IS ASDK -->  You might see alerts for the *Health controller* component that have the following details:  

  Uyarı #1:
   - Ad: Altyapı rolü sağlıksız
   - Önem DERECESİ: uyarı
   - Bileşen: Sistem durumu denetleyicisi
   - Açıklama: Sistem durumu denetleyicisi sinyal tarayıcı kullanılamıyor. Bu sistem durumu raporları ve ölçümleri etkileyebilir.  

  Uyarı #2:
   - Ad: Altyapı rolü sağlıksız
   - Önem DERECESİ: uyarı
   - Bileşen: Sistem durumu denetleyicisi
   - Açıklama: Sistem durumu denetleyicisi hataya tarayıcı kullanılamıyor. Bu sistem durumu raporları ve ölçümleri etkileyebilir.

  Her iki uyarı güvenle yoksayılabilir. Bunlar zaman içinde otomatik olarak kapatılacak.  


#### <a name="marketplace"></a>Market
- Kullanıcılar bir abonelik olmadan tam Market göz atabilir ve planları ve teklifleri gibi yönetim öğelerini görebilirsiniz. Bu öğeler kullanıcılara işlevsiz.

#### <a name="compute"></a>İşlem
- Sanal makine ölçek kümeleri için ölçeklendirme ayarları portalda kullanılabilir değildir. Geçici bir çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürümü farklılıkları nedeniyle kullanmalısınız `-Name` yerine parametre `-VMScaleSetName`.

- <!-- 2290877  --> You cannot scale up a virtual machine scale set (VMSS) that was created when using Azure Stack prior to version 1802. This is due to the change in support for using availability sets with virtual machine scale sets. This support was added with version 1802.  When you attempt to add additional instances to scale a VMSS that was created prior to this support being added, the action fails with the message *Provisioning state failed*. 

  Bu sorunu sürüm 1803 çözümlenir. Sürüm 1802 için bu sorunu çözmek için Azure yığın düzeltmeyi yükleyin **1.0.180302.4**. Daha fazla bilgi için bkz: [KB 4131152: var olan sanal makine ölçek kümeleri kullanılamaz hale]( https://support.microsoft.com/help/4131152). 

- Azure yığını yalnızca sabit türü VHD'lerin destekler. Dinamik VHD Azure yığında Market üzerinden sunulan bazı görüntüleri kullanır ancak bu kaldırıldı. Ekli dinamik bir diski bir sanal makine (VM) yeniden boyutlandırma VM başarısız durumda bırakır.

  Bu sorunu azaltmak için VM'in disk, VHD blob depolama hesabındaki silmeden VM silin. Ardından VHD'yi dinamik bir disk, sabit bir diske Dönüştür ve sanal makine yeniden oluşturun.

- Kullanılabilirlik giderek portalda kümesi oluşturduğunuzda **yeni** > **işlem** > **kullanılabilirlik kümesi**, oluşturabilmeniz için bir kullanılabilirlik, bir hata etki alanı ve güncelleştirme etki alanı 1 ile ayarlayın. Yeni bir sanal makine oluştururken, bir geçici çözüm olarak, kullanılabilirlik PowerShell'i, CLI, kullanarak veya içinden kümesini oluşturmak portal.

- Sanal makineler Azure yığın kullanıcı portalında oluşturduğunuzda, portal D serisinin VM iliştirebilirsiniz veri diskleri yanlış sayıda görüntüler. Tüm desteklenen D serisinin VM'ler sayıda veri diski Azure yapılandırması sağlayabilir.

- Oluşturulacak bir VM görüntüsü başarısız olduğunda, VM görüntüleri işlem dikey penceresine eklenen silemezsiniz başarısız bir öğe.

  Geçici bir çözüm olarak, Hyper-V ile oluşturulan bir kukla VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yol C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silmeden engeller sorunu çözer. Ardından, kukla görüntüsünü oluşturduktan sonra 15 dakika başarılı bir şekilde silebilirsiniz.

  Ayrıca, daha önce başarısız VM görüntüsü yeniden indirin daha sonra deneyebilirsiniz.

-  VM dağıtımı üzerinde uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemi durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermemelisiniz.  

- <!-- 1662991 --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings.  




#### <a name="networking"></a>Ağ
- Bir VM oluşturulur ve bir ortak IP adresi ile ilişkili sonra bu VM IP adresinden ilişkisini olamaz. Çalışmak için disassociation görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu davranış oluşur (genellikle olarak adlandırılan bir *VIP takası*). Tüm gelecekte bu IP adresi sonucu başlangıçta ilişkili VM değil de yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.

- Arka uç VM'ler için hatalı işler MAC adresleri Linux örnekleri arka uç ağ üzerinde kullanırken bölüneceği ILB neden olan iç yük dengeleyici (ILB).  ILB ile Windows örnekleri arka uç ağ üzerinde düzgün çalışır.

-   IP iletimi özelliği portalda görünür, ancak IP iletimini etkinleştirme etkisi yoktur. Bu özellik henüz desteklenmiyor.

- Azure yığın destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu Kiracı abonelikler arasında geçerlidir. Aynı IP adresiyle bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı sonraki oluşturulmasını denemesinden sonra engellenir.

- Bir DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu Vnet vm'lerinin güncelleştirilmiş ayarları gönderilir değil.

- Azure yığın VM dağıtıldıktan sonra ek ağ arabirimleri VM örneğine eklemeyi desteklemez. VM birden fazla ağ arabirimi gerektiriyorsa, dağıtım sırasında tanımlanmalıdır.

-   <!-- 2096388 --> You cannot use the admin portal to update rules for a network security group. 

    Uygulama hizmeti için geçici çözüm: Denetleyici örnekleri için Uzak Masaüstü'nü gerekiyorsa, PowerShell ile ağ güvenlik grupları içindeki güvenlik kuralları Değiştir.  Nasıl yapılır örnekleri şunlardır *izin*ve yapılandırmasını geri yüklemek *Reddet*:  
    
    - *İzin ver:*
 
      ```powershell    
      Login-AzureRMAccount -EnvironmentName AzureStackAdmin
      
      $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"
      
      $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"
      
      ##This doesn’t work. Need to set properties again even in case of edit
      
      #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  
      
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

    - *Reddet:*

        ```powershell
        
        Login-AzureRMAccount -EnvironmentName AzureStackAdmin
        
        $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"
        
        $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"
        
        ##This doesn’t work. Need to set properties again even in case of edit
    
        #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  
    
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





#### <a name="sql-and-mysql"></a>SQL ve MySQL
 - Devam etmeden önce önemli notta gözden [başlamadan önce](#before-you-begin) bu başlangıç sürüm notları.
- Kullanıcıların veritabanlarını yeni bir SQL veya MySQL dağıtımı oluşturmadan önce en çok bir saat sürebilir.

- Yalnızca kaynak sağlayıcısı, o ana bilgisayar SQL veya MySQL sunucuları üzerinde öğeleri oluşturmak için desteklenir. Kaynak sağlayıcısı tarafından oluşturulmamış bir ana bilgisayar sunucusunda oluşturulan öğeleri eşleşmeyen bir duruma neden olabilir.  

- <!-- IS, ASDK --> Special characters, including spaces and periods, are not supported in the **Family** name when you create a SKU for the SQL and MySQL resource providers.

> [!NOTE]  
> Azure yığın 1802 güncelleştirdikten sonra daha önce dağıtmış SQL ve MySQL kaynak sağlayıcıları kullanmaya devam edebilirsiniz.  Yeni bir sürüm kullanılabilir olduğunda SQL ve MySQL güncelleştirme öneririz. Azure yığın gibi güncelleştirmeleri sırayla SQL ve MySQL kaynak sağlayıcıları için geçerlidir.  Örneğin, 1710 sürümünü kullanıyorsanız, önce sürüm 1711 sonra 1712 uygulayın ve ardından 1802 için güncelleştirin.      
>   
> Güncelleştirme 1802 yüklemesini SQL veya MySQL kaynak sağlayıcıları kullanıcılarınız tarafından geçerli kullanımını etkilemez.
> Kullandığınız kaynak sağlayıcıları sürümü, bağımsız olarak kendi veritabanlarını kullanıcılar verilerinizi touched değil ve erişilebilir kalır.    


#### <a name="app-service"></a>App Service
- Kullanıcılar, bunlar ilk Azure işlevlerine abonelikte oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

- (Çalışanları, yönetim, ön uç rolleri) altyapıyı ölçeklendirme için PowerShell işlem için Sürüm Notları'nda açıklandığı şekilde kullanmanız gerekir.

<!--
#### Identity
-->



#### <a name="downloading-azure-stack-tools-from-github"></a>Azure yığın araçları Github'dan indirme
- Kullanırken *çağırma webrequest* Azure yığın indirmek için PowerShell cmdlet araçları Github'dan, hata iletisi:     
    -  *çağırma webrequest: istek iptal edildi: SSL/TLS güvenli kanalı oluşturulamadı.*     

  Bir son GitHub destek kullanımdan Tlsv1 ve Tlsv1.1 şifreleme standartları (PowerShell için varsayılan) nedeniyle bu hata oluşur. Daha fazla bilgi için bkz: [zayıf şifreleme standartları kaldırma bildirimi](https://githubengineering.com/crypto-removal-notice/).

  Bu sorunu çözmek için ekleme `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` TLSv1.2 GitHub havuzların indirirken kullanmak için PowerShell konsolunu zorlamak için komut dosyasının en üstüne.


## <a name="download-the-update"></a>Güncelleştirme karşıdan yükle
Azure yığın 1802 güncelleştirme paketinden indirebilirsiniz [burada](https://aka.ms/azurestackupdatedownload).


## <a name="more-information"></a>Daha fazla bilgi
Microsoft, izlemek ve ayrıcalıklı uç noktası (güncelleştirme 1710 ile yüklü CESARETLENDİRİCİ) kullanarak güncelleştirmeleri sürdürmek için bir yol sağlamıştır.

- Bkz: [izlemek Azure ayrıcalıklı endpoint belgelerini kullanarak yığınında güncelleştirmeleri](https://docs.microsoft.com/azure/azure-stack/azure-stack-monitor-update).

## <a name="see-also"></a>Ayrıca bkz.

- Güncelleştirme yönetimi Azure yığınında genel bakış için bkz: [yönetmek güncelleştirmeleri Azure yığın genel bakış](azure-stack-updates.md).
- Azure yığın güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz: [Azure yığınında güncelleştirmelerini](azure-stack-apply-updates.md).
