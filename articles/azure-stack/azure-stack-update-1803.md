---
title: Azure Stack 1803 güncelleştirme | Microsoft Docs
description: Azure Stack 1803 güncelleştirmesi nedir hakkında bilgi edinin tümleşik sistemleri, bilinen sorunlar ve nerede güncelleştirmeyi indirin.
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
ms.date: 07/11/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: 11430a0d194a722c0c0520c936db3c08b1a6b863
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989580"
---
# <a name="azure-stack-1803-update"></a>Azure Stack 1803 güncelleştirme

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bu makalede geliştirmeleri açıklar ve bilinen sorunlar için bu sürüm ve güncelleştirme karşıdan yükleme konumu 1803 güncelleştirme paketindeki giderir. Bilinen sorunlar için güncelleştirme işlemini doğrudan ilgili sorunları ve derleme (yükleme sonrası) ile ilgili sorunlar ayrılır.

> [!IMPORTANT]        
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="build-reference"></a>Yapı Başvurusu    
Azure Stack 1803 güncelleştirmenin yapı numarasıdır **20180329.1**.


## <a name="before-you-begin"></a>Başlamadan önce    
> [!IMPORTANT]    
> Bu güncelleştirme yüklemesi sırasında sanal makineleri oluşturmaya çalışmayın. Güncelleştirmeleri yönetme hakkında daha fazla bilgi için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md#plan-for-updates).


### <a name="prerequisites"></a>Önkoşullar
- Azure yığını'nı yükleme [güncelleştirme 1802](azure-stack-update-1802.md) Azure Stack 1803 güncelleştirmeyi uygulamadan önce.   

- Yükleme **AzS düzeltme – 1.0.180312.1-derleme 20180222.2** Azure Stack 1803 güncelleştirmeyi uygulamadan önce. Bu düzeltme, Windows Defender'ı güncelleştirir ve Azure Stack için güncelleştirmeleri yüklediğinizde kullanılabilir.

  Düzeltmeyi yüklemek için normal yordamları izleyin [Azure Stack için güncelleştirmeleri yükleme](azure-stack-apply-updates.md). Güncelleştirme adı olarak görünür **AzS düzeltme – 1.0.180312.1**ve aşağıdaki dosyaları içerir: 
    - PUPackageHotFix_20180222.2-1.exe
    - PUPackageHotFix_20180222.2-1.bin
    - Metadata.XML

  Bir depolama hesabı ve kapsayıcı bu dosyalar karşıya yüklendikten sonra Yönetim Portalı'nda güncelleştirme kutucuğundan yüklemeyi çalıştırın. 
  
  Azure Stack için güncelleştirmeleri, bu güncelleştirmeyi yüklemeden Azure Stack sürümünü değiştirmez. Bu güncelleştirme yüklendiğini doğrulamak için listesini görüntülemek **yüklü güncelleştirmeleri**.



### <a name="new-features"></a>Yeni Özellikler 
Bu güncelleştirme, Azure Stack için aşağıdaki geliştirmeleri ve düzeltmeleri içerir.

- **Azure Stack gizli anahtarları güncelleştirme** - (hesaplar ve sertifikaları). Gizli anahtarları yönetme hakkında daha fazla bilgi için bkz. [döndürme gizli dizileri Azure Stack'te](azure-stack-rotate-secrets.md). 

- <!-- 1914853 --> **HTTPS için otomatik yeniden yönlendirme** yönetici ve kullanıcı portalı erişmek için HTTP kullandığınızda. Bu geliştirme, temel alarak yapıldığı [UserVoice](https://feedback.azure.com/forums/344565-azure-stack/suggestions/32205385-it-would-be-great-if-there-was-a-automatic-redirec) Azure Stack için geri bildirim. 

- <!-- 2202621  --> **Market erişim** – Azure Stack Marketini kullanarak artık açabilirsiniz [+ yeni](https://ms.portal.azure.com/#create/hub) seçenek gelen içinde yönetici ve Kullanıcı Portalı, Azure portallarında aynı şekilde.
 
- <!-- 2202621 --> **Azure İzleyici** -Azure yığını ekler [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) yönetici ve kullanıcı portalı için. Bu ölçümlere ve etkinlik günlükleri için yeni gezginler içerir. Dış ağlardan bu Azure İzleyici erişmek için bağlantı noktası **13012** güvenlik duvarı yapılandırmalarını açık olması gerekir. Azure yığını tarafından gerekli bağlantı noktaları hakkında daha fazla bilgi için bkz. [Azure Stack'i veri merkezi tümleştirmesi - uç noktalarını yayımlama](azure-stack-integrate-endpoints.md).

   Ayrıca bunun bir parçası olarak altında değişiklik **diğer hizmetler**, *denetim günlükleri* olarak görünür *etkinlik günlüklerini*. Artık Azure portalı ile tutarlı bir işlevdir. 

- <!-- 1664791 --> **Seyrek dosyalar** - yeni görüntüyü Azure Stack'e ekleme ya da Market'te dağıtım aracılığıyla resim ekleme resmi bir seyrek dosyasına dönüştürülür. Azure Stack sürüm 1803 kullanılmadan önce eklenmiş olan görüntüleri dönüştürülemez. Bunun yerine, bu özelliğin avantajlarından yararlanmak için bu görüntülerin yeniden göndermek için Market dağıtım kullanmanız gerekir. 
 
   Seyrek dosyalar depolama alanı kullanımını azaltır ve g/ç geliştirmek için kullanılan bir verimli dosya biçimine sahiptir.  Daha fazla bilgi için [Fsutil seyrek](https://docs.microsoft.com/windows-server/administration/windows-commands/fsutil-sparse) Windows Server için. 

### <a name="fixed-issues"></a>Giderilen sorunlar

- <!-- 1739988 --> İç Yük Dengeleme (ILB) artık düzgün şekilde MAC adresleri arka uç sanal makineler için ILB arka uç ağ paketlerini Linux örnek arka uç ağ üzerinde kullanırken bırakmasına neden olan işler. ILB, Windows örnekleri ile arka uç ağ üzerinde düzgün çalışır. 

- <!-- 1805496 --> Bir sorun, burada Azure Stack arasında VPN bağlantıları Azure Stack, Azure daha IKE ilke için farklı ayarlarla nedeniyle bağlantısı. Değerleri SALifetime (saat) ve SALiftetime (bayt), Azure ile uyumlu değil ve Azure ayarlarınızla eşleşecek şekilde 1803 ' değiştirildi. 1803 önce SALifetime (saniye) değeri 1803 14,400 ve artık 27.000 değişiklikleri oluştu. SALifetime (bayt) için önce 1803 819,200 değerindeydi ve 33,553,408 1803'teki değişiklikler.

- <!-- 2209262 --> VPN bağlantıları portalda daha önce görünen olduğu IP sorun; ancak etkinleştirme veya IP iletimi geçiş hiçbir etkisi olmaz. Bu özellik, varsayılan ve bu henüz desteklenmiyor değiştirme özelliği tarafından açıktır.  Denetim Portalı'ndan kaldırıldı. 

- <!-- 1766332 --> Seçeneği portalda görünse bile ilke tabanlı VPN ağ geçitleri, azure Stack desteklemez.  Seçeneği, Portalı'ndan kaldırıldı. 

- <!-- 1868283 --> Dinamik diskler ile oluşturulan bir sanal makinenin yeniden boyutlandırma, Azure Stack artık engeller. 

- <!-- 1756324 --> Sanal makineler için kullanım verilerini, saatlik aralıklarla artık ayrılır. Azure ile tutarlı budur. 

- <!--  2253274 --> Sorunu yönetici ve kullanıcı portalı sanal ağ alt ağlar dikey burada başarısız yüklenemedi. Geçici çözüm olarak, PowerShell kullanın ve [Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig?view=azurermps-5.5.0) görüntülemek ve bu bilgileri yönetmek için cmdlet'i.

- İleti bir sanal makine oluştururken *fiyatlandırma görüntülenemiyor* VM boyutu için bir boyut seçerken, artık görünür.

- Performans, kararlılık, güvenlik ve Azure Stack tarafından kullanılan işletim sistemi için çeşitli düzeltmeleri.


### <a name="changes"></a>Değişiklikler
- Yeni oluşturulan teklifinden durumunu değiştirmek için yol *özel* için *genel* veya *yetkisi alınmış* değişti. Daha fazla bilgi için [teklif oluşturma](azure-stack-create-offer.md).


### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar    
<!-- 2328416 --> 1803 güncelleştirme yüklemesi sırasında blob hizmeti ve blob hizmeti kullanan iç Hizmetleri kapalı kalma süresi olabilir. Bu, bazı sanal makine işlemlerini içerir. Bu kesinti Kiracı hatalarının işlemleri veya uyarıları verilere erişemediğinizi hizmetlerinden neden olabilir. Güncelleştirme yüklemesi tamamlandığında kendisi bu sorunu çözer. 



### <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar
- 1803 yüklendikten sonra geçerli düzeltmeleri yükleyin. Daha fazla bilgi için aşağıdaki Bilgi Bankası makaleleri görüntülemek hem de bizim [hizmet İlkesi](azure-stack-servicing-policy.md).

  - [KB 4344115 - Azure Stack düzeltme 1.0.180427.15](https://support.microsoft.com/help/4344115).

- Bu güncelleştirmeyi yükledikten sonra emin olmak için güvenlik duvarı yapılandırmanızı gözden [gerekli bağlantı noktalarını](azure-stack-integrate-endpoints.md) açıktır. Örneğin, bu güncelleştirme sunuyor *Azure İzleyici* denetim günlükleri etkinlik günlükleri için bir değişiklik içerir. Bu değişiklik ile bağlantı noktası 13012 artık kullanılan ve açık olmalıdır.  


### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)
Derleme için yükleme sonrası bilinen sorunlar verilmiştir **20180323.2**.

#### <a name="portal"></a>Portal
- <!-- 2332636 - IS -->  Bu Azure Stack sürümüne güncelleştirin ve Azure Stack kimlik sistemi için AD FS kullanıyorsanız, varsayılan sağlayıcı aboneliği varsayılan sahibi yerleşik olarak sıfırlanır **CloudAdmin** kullanıcı.  
  Geçici çözüm: Bu güncelleştirmeyi yükledikten sonra bu sorunu çözmek için 3 adımda kullanın [yapılandırmak için tetikleyici Otomasyon Talep sağlayıcı güveni Azure Stack'te](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) varsayılan sağlayıcı aboneliğin sahibi sıfırlamak için yordamı.   

- Özelliği [açılan listeden yeni bir destek isteği açmak için](azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen içinde Yönetici portalı kullanılamıyor. Bunun yerine, aşağıdaki bağlantıyı kullanın:     
    - İçin Azure Stack tümleşik sistemleri kullanan https://aka.ms/newsupportrequest.

- <!-- 2050709 --> Yönetim Portalı'nda Blob hizmeti, tablo hizmeti ve kuyruk hizmeti için depolama ölçümleri düzenlemek mümkün değildir. Depolama alanına gidin ve blob, tablo veya kuyruk hizmeti kutucuğu seçtiğinizde, bu hizmet için bir ölçüm grafiği görüntüleyen yeni bir dikey pencere açılır. Ölçüm grafiği kutucuğundan üst kısmından ardından Düzen seçerseniz, grafiği Düzenle dikey penceresi açılır ancak ölçümleri düzenleme seçeneklerini görüntülemez.

- Yönetici portalı'nda işlem ve depolama kaynaklarını görüntülemek mümkün olmayabilir. Bu sorunun nedeni, yanlış başarılı olarak raporlanacak güncelleştirilecek neden olan güncelleştirme yüklemesi sırasında bir hata var. Bu sorun devam ederse Yardım için Microsoft Müşteri Destek Hizmetleri'ne başvurun.

- Bir boş Pano portalında görebilirsiniz. Pano kurtarmak için portalın sağ üst köşesindeki dişli simgesini seçin ve ardından **varsayılan ayarları geri**.

- Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

- Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.

- Yönetim Portalı'nın Panoda güncelleştirmeler hakkında bilgi görüntülemek kutucuğu güncelleştir başarısız olur. Bu sorunu çözmek için yenilemek için kutucuğa tıklayın.

- Yönetim Portalı'nda bir kritik uyarı görebileceğiniz *Microsoft.Update.Admin* bileşeni. Uyarı adı, açıklaması ve düzeltme tüm olarak görüntüle:  
    - *HATA - şablon FaultType ResourceProviderTimeout için eksik.*

  Bu uyarıyı güvenle yoksayılabilir. 


#### <a name="health-and-monitoring"></a>Sistem durumu ve izleme
- <!-- 1264761 - IS ASDK -->  Uyarıları görebilirsiniz *sistem durumu denetleyicisi* aşağıdaki ayrıntıları olan bir bileşeni:  

   Uyarı #1:
   - ADI: Sağlıksız altyapı rolü
   - Önem DERECESİ: uyarı
   - BİLEŞENİ: Sistem durumu denetleyicisi
   - Açıklama: Sistem durumu denetleyici sinyal tarayıcı kullanılamıyor. Bu sistem durumu raporlarının ve ölçümler etkileyebilir.  

  Uyarı #2:
   - ADI: Sağlıksız altyapı rolü
   - Önem DERECESİ: uyarı
   - BİLEŞENİ: Sistem durumu denetleyicisi
   - Açıklama: Hata tarayıcı durumu denetleyicisi kullanılamıyor. Bu sistem durumu raporlarının ve ölçümler etkileyebilir.

  Her iki uyarılar güvenle yoksayılabilir. Bunlar, zaman içinde otomatik olarak kapatılacak.  


#### <a name="marketplace"></a>Market
- Kullanıcılar, abone olmadan tüm markete göz atabilir ve planlar ve teklifler gibi yönetim öğelerini görebilirsiniz. Bu kullanıcılara işlevsel olmayan öğelerdir.



#### <a name="compute"></a>İşlem
- Ölçeklendirme ayarları sanal makine ölçek kümeleri için portalda kullanılabilir değildir. Geçici çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürüm farklar nedeniyle, kullanmanız gereken `-Name` yerine parametre `-VMScaleSetName`.

- Portaldaki giderek bir kullanılabilirlik kümesi oluşturduğunuzda, **yeni** > **işlem** > **kullanılabilirlik kümesi**, oluşturabilmeniz için bir bir hata etki alanı ve 1 güncelleştirme etki alanı ile kullanılabilirlik kümesi. Yeni bir sanal makine oluştururken bir geçici çözüm olarak, PowerShell, CLI kullanarak veya içinden kullanılabilirlik oluşturma portalı.

- Portal, Azure Stack kullanıcı portalında sanal makine oluşturduğunuzda, D serisi VM ekleyebilirsiniz veri diskleri yanlış sayıda görüntüler. Tüm desteklenen D serisi VM'ler gibi çok sayıda veri diski Azure yapılandırması sağlayabilir.

- Bir VM görüntüsü oluşturulması başarısız olduğunda, nelze odstranit başarısız bir öğe için VM görüntüleri işlem dikey eklenebilir.

  Geçici çözüm olarak, Hyper-V ile oluşturduğunuz işlevsiz bir VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yolu C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silme engelleyen sorunu çözer. Ardından, işlevsiz bir görüntü oluşturduktan sonra 15 dakika başarıyla silebilirsiniz.

  Ardından, daha önce başarısız bir VM görüntüsü yeniden indirilemiyor deneyebilirsiniz.

-  VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

- <!-- 1662991 --> Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur.  


#### <a name="networking"></a>Ağ
- Bir VM oluşturulur ve bir genel IP adresiyle ilişkili sonra o VM'den bir IP adresi ilişkisini olamaz. İlişkisi kaldırma çalışıyor gibi görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu durumda (genellikle olarak adlandırılan bir *VIP takas*). Tüm gelecek bu IP adresi sonuç başlangıçta ilişkili VM ve yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.



- Azure Stack destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu tüm Kiracı abonelikler arasında geçerlidir. Aynı IP adresine sahip bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı, sonraki oluşturulmasını denemeden sonra engellenir.

- DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu vnet'teki VM'ler için güncelleştirilmiş ayarları gönderiliyor değil.

- Azure Stack, VM dağıtıldıktan sonra ek ağ arabirimi bir sanal makine örneğine eklemeyi desteklemez. VM birden fazla ağ arabirimi gerektiriyorsa, dağıtım sırasında tanımlanmalıdır.

- <!-- 2096388 --> Bir ağ güvenlik grubu kurallarını güncelleştirmek için Yönetim Portalı'nı kullanamazsınız. 

    App Service için geçici çözüm: denetleyici örneği Uzak Masaüstü bağlantısı için gerekiyorsa, PowerShell ile ağ güvenlik grupları içindeki güvenlik kurallarını değiştirin.  Nasıl yapılır örnekleri aşağıda verilmiştir *izin*ve ardından yapılandırmasına geri *Reddet*:  
    
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
- Devam etmeden önce önemli nota gözden [başlamadan önce](#before-you-begin) bu başlangıcı sürüm notları.

- Uygulamanın, kullanıcıların yeni bir SQL veya MySQL dağıtımında veritabanlarını oluşturabilmek kadar bir saat sürebilir.

- Yalnızca kaynak sağlayıcısı, ana bilgisayar SQL veya MySQL sunucuları üzerinde öğeleri oluşturmak için desteklenir. Kaynak sağlayıcısı tarafından oluşturulmamış bir ana bilgisayar sunucusunda oluşturulan öğeler, eşleşmeyen bir duruma neden olabilir.  

- <!-- IS, ASDK --> Özel karakterler, boşluk ve nokta dahil olmak üzere desteklenmez **ailesi** adı, SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda.

> [!NOTE]  
> Azure Stack 1803 için güncelleştirdikten sonra daha önce dağıttığınız SQL ve MySQL kaynak sağlayıcıları kullanmaya devam edebilirsiniz.  Yeni bir sürüm kullanıma sunulduğunda SQL ve MySQL güncelleştirme öneririz. Azure Stack gibi güncelleştirmeleri sırayla SQL ve MySQL kaynak sağlayıcıları için geçerlidir.  Örneğin, 1711. sürümünü kullanıyorsanız, önce sürüm 1712 sonra 1802 uygulayın ve ardından 1803 için güncelleştirin.      
>   
> Güncelleştirme 1803 yüklenmesi, kullanıcılarınız tarafından SQL veya MySQL kaynak sağlayıcıları geçerli kullanımını etkilemez.
> Kullandığınız kaynak sağlayıcıları sürümünden bağımsız olarak kullanıcılar verilerinizi veritabanlarındaki bilgiler değildir ve erişilebilir kalır.    



#### <a name="app-service"></a>App Service
- Kullanıcılar, bunlar, ilk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

- (Çalışan, yönetim, ön uç rollerini) altyapıyı ölçeklendirme için sürüm notları için işlem açıklandığı PowerShell kullanmanız gerekir.


#### <a name="usage"></a>Kullanım  
- Kullanım genel IP adresi kullanım ölçümü verilerini gösterir aynı *olay tarihi-saati* yerine her kaydın değerini *TimeDate* gösteren kaydın oluşturulduğu zaman damgası. Şu anda, genel IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.

<!--
#### Identity
-->



#### <a name="downloading-azure-stack-tools-from-github"></a>Github'dan Azure Stack araçları indirme
- Kullanırken *Invoke-webrequest* Github'dan Azure Stack indirmek için PowerShell cmdlet araçları, bir hata alırsınız:     
    -  *Invoke-webrequest: istek iptal edildi: SSL/TLS güvenli kanal oluşturulamadı.*     

  Bu hata, Tlsv1 ve Tlsv1.1 şifreleme standartları (PowerShell için varsayılan), yeni bir GitHub destek kullanımdan kaldırma nedeniyle oluşur. Daha fazla bilgi için [zayıf şifreleme standartları Temizleme Bildirimi](https://githubengineering.com/crypto-removal-notice/).

  Bu sorunu çözmek için ekleme `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` GitHub depolarından indirirken TLSv1.2 kullanmak için PowerShell konsolunu zorlamak için komut dosyasının en üstüne.


## <a name="download-the-update"></a>Güncelleştirmeyi indirin
Azure Stack 1803 güncelleştirme paketinden indirebileceğiniz [burada](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Ayrıca bkz.
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).
- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
