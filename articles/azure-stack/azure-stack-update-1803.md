---
title: Azure yığın 1803 güncelleştirme | Microsoft Docs
description: Azure yığın 1803 güncelleştirmesi nedir hakkında bilgi edinin tümleşik sistemleri, bilinen sorunlar ve güncelleştirme karşıdan yükleme konumu.
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
ms.date: 06/22/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: a74e77f84aa70519015a589cbc6e7478c0c41592
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318818"
---
# <a name="azure-stack-1803-update"></a>Azure yığın 1803 güncelleştirme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bu makalede yenilikleri ve bilinen sorunlar bu sürüm ve güncelleştirmeyi yüklemek nereye 1803 güncelleştirme paketindeki giderir. Bilinen sorunlar için güncelleştirme işlemini doğrudan ilgili sorunlar ve yapı (yükleme sonrası) ile ilgili sorunları ayrılır.

> [!IMPORTANT]        
> Bu güncelleştirme paketi, yalnızca Azure tümleşik yığını sistemleri içindir. Bu güncelleştirme paketi Azure yığın Geliştirme Seti için geçerli değildir.

## <a name="build-reference"></a>Derleme başvurusu    
Azure yığın 1803 güncelleştirme yapı numarası **20180329.1**.


## <a name="before-you-begin"></a>Başlamadan önce    
> [!IMPORTANT]    
> Bu güncelleştirmenin yüklenmesi sırasında sanal makineler oluşturmak çalışmayın. Güncelleştirmeleri yönetme hakkında daha fazla bilgi için bkz: [yönetmek Azure yığın genel bakış güncelleştirmelerinde](azure-stack-updates.md#plan-for-updates).


### <a name="prerequisites"></a>Önkoşullar
- Azure yığın yükleme [1802 güncelleştirme](azure-stack-update-1802.md) Azure yığın 1803 güncelleştirmeyi uygulamadan önce.   

- Yükleme **AzS düzeltmesi – 1.0.180312.1-yapı 20180222.2** Azure yığın 1803 güncelleştirmeyi uygulamadan önce. Bu düzeltme, Windows Defender güncelleştirir ve Azure yığını için güncelleştirmeler olduğunda kullanılabilir.

  Düzeltmeyi yüklemek için normal yordamları izleyin [Azure yığını için güncelleştirmeleri yükleme](azure-stack-apply-updates.md). Güncelleştirme adı olarak görünür **AzS düzeltmesi – 1.0.180312.1**ve aşağıdaki dosyaları içerir: 
    - PUPackageHotFix_20180222.2-1.exe
    - PUPackageHotFix_20180222.2-1.bin
    - Metadata.XML

  Bu dosyaları bir depolama hesabı ve kapsayıcı dosyalarını karşıya yükledikten sonra Yönetim Portalı'nda güncelleştirme döşeme yükleme çalıştırın. 
  
  Azure yığınına yönelik güncelleştirmeler, bu güncelleştirmeyi yüklemeden Azure yığın sürümünü değiştirmez. Bu güncelleştirmenin yüklendiğini doğrulamak için listesini görüntülemek **yüklü güncelleştirmeleri**.



### <a name="new-features"></a>Yeni Özellikler 
Bu güncelleştirme aşağıdaki geliştirmeleri ve düzeltmeler için Azure yığınına içerir.

- **Azure yığın gizli güncelleştirme** - (hesapları ve sertifikaları). Parolaları yönetme hakkında daha fazla bilgi için bkz: [döndürme gizli anahtarları Azure yığınında](azure-stack-rotate-secrets.md). 

- <!-- 1914853 --> **HTTPS için otomatik yeniden yönlendirme** yönetici ve kullanıcı portalı erişmek için HTTP kullandığınızda. Bu geliştirme, temel alarak yapıldığı [UserVoice](https://feedback.azure.com/forums/344565-azure-stack/suggestions/32205385-it-would-be-great-if-there-was-a-automatic-redirec) Azure yığını için geri bildirim. 

- <!-- 2202621  --> **Market erişim** – kullanarak Azure yığın Market şimdi açabilirsiniz [+ yeni](https://ms.portal.azure.com/#create/hub) seçenek gelen içinde yönetici ve Kullanıcı Portalı, Azure portallarında aynı şekilde.
 
- <!-- 2202621 --> **Azure İzleyici** -Azure yığını ekler [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) yönetici ve kullanıcı portalı için. Bu ölçümleri ve etkinlik günlükleri için yeni gezginler içerir. Bu Azure İzleyici dış ağlardan erişmek için bağlantı noktası **13012** güvenlik duvarı yapılandırmalarını açık olması gerekir. Azure yığını tarafından gerekli bağlantı noktaları hakkında daha fazla bilgi için bkz: [Azure yığın datacenter tümleştirmesi - uç noktalarını yayımlama](azure-stack-integrate-endpoints.md).

   Ayrıca bu parçası olarak altında değişiklik **daha fazla hizmet**, *denetim günlüklerini* olarak artık görünür *etkinlik günlükleri*. Artık Azure portalı ile tutarlı bir işlevdir. 

- <!-- 1664791 --> **Seyrek dosyalar** - Azure yığını için yeni bir görüntü ekleyin veya bir görüntü Market dağıtım aracılığıyla eklediğinizde görüntü seyrek dosyasına dönüştürülür. Azure yığın sürüm 1803 kullanılmadan önce eklenen görüntüleri dönüştürülemiyor. Bunun yerine, bu özelliğin avantajlarından yararlanmak için bu görüntüleri yeniden gönderin Market dağıtım kullanmanız gerekir. 
 
   Depolama alanı kullanımını azaltır ve g/ç geliştirmek için kullanılan bir verimli dosya biçimi seyrek dosyalarıdır.  Daha fazla bilgi için bkz: [Fsutil seyrek](https://docs.microsoft.com/windows-server/administration/windows-commands/fsutil-sparse) Windows Server için. 

### <a name="fixed-issues"></a>Giderilen sorunlar

- <!-- 1739988 --> İç yük dengeleyici (ILB) artık düzgün MAC adresleri arka uç VM'ler için ILB arka uç ağ paketlerini Linux örnekleri arka uç ağ üzerinde kullanırken bırakmasına neden olan işler. ILB ile Windows örnekleri arka uç ağ üzerinde düzgün çalışır. 

- <!-- 1805496 --> Bir sorun olduğu Azure yığın arasında VPN bağlantıları Azure IKE İlkesi için farklı ayarları kullanarak Azure yığın nedeniyle bağlantıları kesildiğinde. SALifetime (saat) ve SALiftetime (bayt) değerlerini Azure ile uyumlu değil ve Azure ayarlarla eşleşecek şekilde 1803 değiştirilmiştir. SALifetime (saniye) değeri 1803 önce 14,400 ve şimdi 27.000 değişiklikler 1803 içinde oluştu. SALifetime (bayt) değeri 1803 önce 819,200 idi ve 1803 33,553,408 yapılan değişiklikler.

- <!-- 2209262 --> VPN bağlantıları portalda daha önce görünür olduğu IP sorunu; ancak etkinleştirme veya IP iletimi geçiş etkisi yoktur. Bu özellik, varsayılan ve bu henüz desteklenmiyor değiştirme olanağı tarafından açıktır.  Denetim Portalı'ndan kaldırıldı. 

- <!-- 1766332 --> Seçenek portalda görünse bile azure yığın ilke tabanlı VPN ağ geçitleri, desteklemez.  Seçenek Portalı'ndan kaldırıldı. 

- <!-- 1868283 --> Dinamik diskler ile oluşturulan bir sanal makinenin yeniden boyutlandırma Azure yığını şimdi engeller. 

- <!-- 1756324 --> Sanal makineler için kullanım verilerini şimdi saatlik aralıklarla ayrılır. Bu, Azure ile tutarlıdır. 

- <!--  2253274 --> Sorunu yönetici ve Kullanıcı Portalı Ayarları dikey penceresinde vNet alt ağlar için burada başarısız yüklenemiyor. Geçici bir çözüm olarak, PowerShell kullanın ve [Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig?view=azurermps-5.5.0) görüntülemek ve bu bilgileri yönetmek için cmdlet.

- İleti bir sanal makine oluşturduğunuzda *fiyatlandırma görüntülenemiyor* VM boyutu için bir boyut seçerken, artık görünür.

- Performans, sağlamlık, güvenlik ve Azure yığını tarafından kullanılan işletim sistemi için çeşitli düzeltmeler.


### <a name="changes"></a>Değişiklikler
- Yeni oluşturulan bir tekliften durumunu değiştirmek için yol *özel* için *ortak* veya *yetkisi alınmış* değişti. Daha fazla bilgi için bkz: [teklifi oluşturmak](azure-stack-create-offer.md).


### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar    
<!-- 2328416 --> 1803 güncelleştirme yüklemesi sırasında blob hizmeti ve blob hizmeti kullanan iç Hizmetleri kapalı kalma süresi olabilir. Bu, bazı sanal makine işlemlerini içerir. Bu kesinti Kiracı hatalarının işlemleri veya uyarıları verilere erişemezsiniz Hizmetleri'nden neden olabilir. Güncelleştirme yüklemesi tamamlandığında kendisini bu sorunu çözer. 



### <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar
- 1803 yüklendikten sonra geçerli düzeltmeleri yükleyin. Daha fazla bilgi için aşağıdaki Bilgi Bankası makaleleri görüntülemek yanı sıra bizim [hizmet İlkesi](azure-stack-servicing-policy.md).

  - [KB 4341390 - Azure yığın düzeltme 1.0.180424.12](https://support.microsoft.com/en-us/help/4341390).

- Bu güncelleştirmeyi yükledikten sonra emin olmak için güvenlik duvarı yapılandırmasını gözden [gerekli bağlantı noktalarını](azure-stack-integrate-endpoints.md) açıktır. Örneğin, bu güncelleştirme tanıtır *Azure İzleyici* denetim günlüklerini etkinlik günlükleri için bir değişiklik içerir. Bu değişikliği bağlantı noktası 13012 kullanılan ve açık olmalıdır.  


### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)
Derleme için yükleme sonrası bilinen sorunlar verilmiştir **20180323.2**.

#### <a name="portal"></a>Portal
- <!-- 2332636 - IS -->  Azure yığınının bu sürüme güncelleştirin ve Azure yığın kimlik sistemi için AD FS kullandığınızda, varsayılan sağlayıcı aboneliğin varsayılan sahibi yerleşik olarak sıfırlanır **CloudAdmin** kullanıcı.  
  Geçici çözüm: Bu güncelleştirmeyi yükledikten sonra bu sorunu çözmek için 3 adımından kullanmak [yapılandırmak için tetikleyici Otomasyon Talep sağlayıcı güveni Azure yığınında](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) varsayılan sağlayıcı aboneliğin sahibi sıfırlamak için yordamı.   

- Özelliği [aşağı açılır listeden yeni bir destek isteği açma](azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen içinde Yönetici portalı kullanılamıyor. Bunun yerine, aşağıdaki bağlantıyı kullanın:     
    - Azure yığını için tümleşik sistemleri kullanan https://aka.ms/newsupportrequest.

- <!-- 2050709 --> Yönetim Portalı'nda Blob hizmeti, tablo hizmeti ya da sıra hizmeti için depolama ölçümlerini düzenlemek mümkün değil. Depolama birimine gidin ve blob, tablo veya kuyruğu hizmeti bölmesi seçtiğinizde, bu hizmet için bir ölçüm grafik görüntüleyen yeni bir dikey pencere açılır. Ölçümleri grafik döşeme üstten ardından Düzenle seçerseniz grafiği Düzenle dikey penceresini açar ancak ölçümleri düzenlemek için seçenekleri görüntülemez.

- Yönetici portalı'nda işlem ve depolama kaynaklarını görüntülemek mümkün olmayabilir. Bu sorunun nedeni yanlış başarılı olarak bildirilmesini güncelleştirme neden güncelleştirmenin yüklenmesi sırasında bir hata var. Bu sorun devam ederse, Yardım için Microsoft Müşteri Destek Hizmetleri'ne başvurun.

- Boş bir portal panosunda görebilirsiniz. Pano kurtarmak için portalın sağ üst köşesindeki dişli simgesini seçin ve ardından **varsayılan ayarları geri**.

- Kullanıcı abonelikleri yalnız bırakılmış kaynakları sonuçlarında siliniyor. Geçici bir çözüm olarak ilk kullanıcı kaynakları veya tüm kaynak grubunu silme ve kullanıcı abonelikleri silin.

- Azure yığın Portal kullanımı aboneliğinizi izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.

- Yönetim Portalı'nı panosunda güncelleştirmeler hakkında bilgi görüntülemek güncelleştirme döşeme başarısız olur. Bu sorunu çözmek için yenilemeniz kutucuğa tıklayın.

- Yönetim Portalı'nda bir kritik uyarı görebilirsiniz *Microsoft.Update.Admin* bileşeni. Uyarı adı, açıklama ve düzeltmesi tümü olarak görüntülenir:  
    - *HATA - FaultType ResourceProviderTimeout için şablon eksik.*

  Bu uyarı güvenle yoksayılabilir. 


#### <a name="health-and-monitoring"></a>Sistem durumu ve izleme
- <!-- 1264761 - IS ASDK -->  Uyarılar için görebilirsiniz *sistem durumu denetleyicisi* aşağıdaki ayrıntıları olan bileşen:  

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

- Kullanılabilirlik giderek portalda kümesi oluşturduğunuzda **yeni** > **işlem** > **kullanılabilirlik kümesi**, oluşturabilmeniz için bir kullanılabilirlik, bir hata etki alanı ve güncelleştirme etki alanı 1 ile ayarlayın. Yeni bir sanal makine oluştururken, bir geçici çözüm olarak, kullanılabilirlik PowerShell'i, CLI, kullanarak veya içinden kümesini oluşturmak portal.

- Sanal makineler Azure yığın kullanıcı portalında oluşturduğunuzda, portal D serisinin VM iliştirebilirsiniz veri diskleri yanlış sayıda görüntüler. Tüm desteklenen D serisinin VM'ler sayıda veri diski Azure yapılandırması sağlayabilir.

- Oluşturulacak bir VM görüntüsü başarısız olduğunda, VM görüntüleri işlem dikey penceresine eklenen silemezsiniz başarısız bir öğe.

  Geçici bir çözüm olarak, Hyper-V ile oluşturulan bir kukla VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yol C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silmeden engeller sorunu çözer. Ardından, kukla görüntüsünü oluşturduktan sonra 15 dakika başarılı bir şekilde silebilirsiniz.

  Ayrıca, daha önce başarısız VM görüntüsü yeniden indirin daha sonra deneyebilirsiniz.

-  VM dağıtımı üzerinde uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemi durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermemelisiniz.  

- <!-- 1662991 --> Linux VM tanılama Azure yığınında desteklenmiyor. VM tanılaması etkin bir Linux VM dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz dağıtım de başarısız olur.  


#### <a name="networking"></a>Ağ
- Bir VM oluşturulur ve bir ortak IP adresi ile ilişkili sonra bu VM IP adresinden ilişkisini olamaz. Çalışmak için disassociation görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu davranış oluşur (genellikle olarak adlandırılan bir *VIP takası*). Tüm gelecekte bu IP adresi sonucu başlangıçta ilişkili VM değil de yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.



- Azure yığın destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu Kiracı abonelikler arasında geçerlidir. Aynı IP adresiyle bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı sonraki oluşturulmasını denemesinden sonra engellenir.

- Bir DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu Vnet vm'lerinin güncelleştirilmiş ayarları gönderilir değil.

- Azure yığın VM dağıtıldıktan sonra ek ağ arabirimleri VM örneğine eklemeyi desteklemez. VM birden fazla ağ arabirimi gerektiriyorsa, dağıtım sırasında tanımlanmalıdır.

- <!-- 2096388 --> Bir ağ güvenlik grubu kuralları için Yönetim Portalı'nı kullanamazsınız. 

    Uygulama hizmeti için geçici çözüm: Denetleyici örnekleri için Uzak Masaüstü'nü gerekiyorsa, PowerShell ile ağ güvenlik grupları içindeki güvenlik kuralları Değiştir.  Nasıl yapılır örnekleri şunlardır *izin*ve yapılandırmasını geri yüklemek *Reddet*:  
    
    - *İzin ver:*
 
      ```powershell    
      Add-AzureRmAccount -EnvironmentName AzureStackAdmin
      
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
        
        Add-AzureRmAccount -EnvironmentName AzureStackAdmin
        
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

- <!-- IS, ASDK --> Alanları ve dönemleri gibi özel karakterler desteklenmiyor **ailesi** SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda olarak adlandırın.

> [!NOTE]  
> Azure yığın 1803 güncelleştirdikten sonra daha önce dağıtmış SQL ve MySQL kaynak sağlayıcıları kullanmaya devam edebilirsiniz.  Yeni bir sürüm kullanılabilir olduğunda SQL ve MySQL güncelleştirme öneririz. Azure yığın gibi güncelleştirmeleri sırayla SQL ve MySQL kaynak sağlayıcıları için geçerlidir.  Örneğin, 1711 sürümünü kullanıyorsanız, önce sürüm 1712 sonra 1802 uygulayın ve ardından 1803 için güncelleştirin.      
>   
> Güncelleştirme 1803 yüklemesini SQL veya MySQL kaynak sağlayıcıları kullanıcılarınız tarafından geçerli kullanımını etkilemez.
> Kullandığınız kaynak sağlayıcıları sürümü, bağımsız olarak kendi veritabanlarını kullanıcılar verilerinizi touched değil ve erişilebilir kalır.    



#### <a name="app-service"></a>App Service
- Kullanıcılar, bunlar ilk Azure işlevlerine abonelikte oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

- (Çalışanları, yönetim, ön uç rolleri) altyapıyı ölçeklendirme için PowerShell işlem için Sürüm Notları'nda açıklandığı şekilde kullanmanız gerekir.


#### <a name="usage"></a>Kullanım  
- Kullanım genel IP adresi kullanım ölçüm verileri gösterilir aynı *olay tarihi-saati* yerine her kayıt için değer *TimeDate* kaydının oluşturulduğu gösterilir Damga. Şu anda, ortak IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.

<!--
#### Identity
-->



#### <a name="downloading-azure-stack-tools-from-github"></a>Azure yığın araçları Github'dan indirme
- Kullanırken *çağırma webrequest* Azure yığın indirmek için PowerShell cmdlet araçları Github'dan, hata iletisi:     
    -  *çağırma webrequest: istek iptal edildi: SSL/TLS güvenli kanalı oluşturulamadı.*     

  Bir son GitHub destek kullanımdan Tlsv1 ve Tlsv1.1 şifreleme standartları (PowerShell için varsayılan) nedeniyle bu hata oluşur. Daha fazla bilgi için bkz: [zayıf şifreleme standartları kaldırma bildirimi](https://githubengineering.com/crypto-removal-notice/).

  Bu sorunu çözmek için ekleme `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` TLSv1.2 GitHub havuzların indirirken kullanmak için PowerShell konsolunu zorlamak için komut dosyasının en üstüne.


## <a name="download-the-update"></a>Güncelleştirme karşıdan yükle
Azure yığın 1803 güncelleştirme paketinden indirebilirsiniz [burada](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Ayrıca bkz.
- İzleme ve güncelleştirmeleri sürdürmek için ayrıcalıklı uç noktası (CESARETLENDİRİCİ) kullanmak için bkz: [izlemek Azure kullanan ayrıcalıklı uç noktasını yığınında güncelleştirmeleri](azure-stack-monitor-update.md).
- Güncelleştirme yönetimi Azure yığınında genel bakış için bkz: [yönetmek güncelleştirmeleri Azure yığın genel bakış](azure-stack-updates.md).
- Azure yığın güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz: [Azure yığınında güncelleştirmelerini](azure-stack-apply-updates.md).
