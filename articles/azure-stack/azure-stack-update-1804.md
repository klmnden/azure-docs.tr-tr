---
title: Azure Stack 1804 güncelleştirme | Microsoft Docs
description: Azure Stack 1804 güncelleştirmesi nedir hakkında bilgi edinin tümleşik sistemleri, bilinen sorunlar ve nerede güncelleştirmeyi indirin.
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
ms.openlocfilehash: 496aea1195885c582d3529d7ddb43210aad5fea1
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990141"
---
# <a name="azure-stack-1804-update"></a>Azure Stack 1804 güncelleştirme

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bu makalede geliştirmeleri açıklar ve bilinen sorunlar için bu sürüm ve güncelleştirme karşıdan yükleme konumu 1804 güncelleştirme paketindeki giderir. Bilinen sorunlar için güncelleştirme işlemini doğrudan ilgili sorunları ve derleme (yükleme sonrası) ile ilgili sorunlar ayrılır.

> [!IMPORTANT]        
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="build-reference"></a>Yapı Başvurusu    
Azure Stack 1804 güncelleştirmenin yapı numarasıdır **20180513.1**.   

### <a name="new-features"></a>Yeni Özellikler
Bu güncelleştirme Azure Stack için aşağıdaki geliştirmeleri içerir.

- <!-- 15028744 - IS -->  **AD FS kullanarak, bağlantısı kesilmiş Azure Stack dağıtımları için Visual Studio desteği**. Şimdi abonelikler ekleyebilir ve kimlik doğrulaması Visual Studio içinde AD FS kullanarak kullanıcı kimlik bilgilerini sürümüyle federasyona eklenenler. 
 
- <!-- 1779474, 1779458 - IS --> **Av2 ve F serisi sanal makineler kullanın**. Azure Stack artık sanal makinelerin Av2 serisi ve F serisi sanal makine boyutlarına göre kullanabilirsiniz. Daha fazla bilgi için [Azure Stack'te desteklenen sanal makine boyutları](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes). 

- <!-- 1759172 - IS, ASDK --> **Yeni Yönetim abonelikler**. 1804 ile portalda kullanılabilir iki yeni abonelik türü vardır. Bu yeni abonelik türleriyle 1804 sürümünden başlayarak yeni Azure Stack yükleme varsayılan sağlayıcı aboneliği yanı sıra ve görünür olan. *Azure Stack bu sürümü bu yeni abonelik türleriyle kullanmayın*. Bu abonelik türleri gelecek güncelleştirmelerden oturum kullanmak için kullanılabilirlik duyuracağız. 

  Azure Stack 1804 sürüme güncelleştirirseniz, iki yeni abonelik türleriyle görünür değildir. Ancak, yeni dağıtımların Azure Stack tümleşik sistemleri ve yüklemeleri 1804 veya sonraki bir sürümü Azure Stack geliştirme Seti'ni sürümünün üç abonelik türlü erişimi.  

  Bu yeni abonelik türleriyle daha büyük bir değişiklik varsayılan sağlayıcı aboneliği güvenli ve SQL barındırma sunucuları gibi paylaşılan kaynakları dağıtma daha kolay hale getirmek için bir parçasıdır. Daha fazla parçasına ileride yapılacak güncelleştirmelerle daha büyük bu değişikliği Azure Stack'e ekledikçe bu yeni abonelik türleriyle altında dağıtılan kaynakların kaybolmuş olabilir. 

  Artık görünür üç abonelik türleri şunlardır:  
  - Sağlayıcı aboneliği varsayılan: Bu abonelik türü kullanmaya devam edin. 
  - Abonelik ölçümü: *Bu abonelik türü kullanmayın.*
  - Tüketim abonelik: *Bu abonelik türü kullanmayın*

  



## <a name="fixed-issues"></a>Giderilen sorunlar

- <!-- IS, ASDK -->  Yönetim Portalı'nda artık bir bilgi görüntülemeden önce güncelleştirme kutucuk yenilemeniz gerekir.
 
- <!-- 2050709  -->  Ayrıca, Blob hizmeti, tablo hizmeti ve kuyruk hizmeti depolama ölçümleri düzenlemek için artık Yönetici portalını kullanabilir.
 
- <!-- IS, ASDK --> Altında **ağ**'ye tıkladığınızda **bağlantı** bir VPN bağlantısı kurmak için **siteden siteye (IPSec)** kullanılabilir tek seçenek sunulmuştur.

- **Çeşitli düzeltmeleri** performans, kararlılık, güvenlik ve Azure Stack tarafından kullanılan işletim sistemi için.

## <a name="additional-releases-timed-with-this-update"></a>Bu güncelleştirme ile zaman aşımına ek sürümleri  
Aşağıdaki kullanıma sunulmuştur, ancak Azure Stack güncelleştirme 1804 gerektirmez.
- **Güncelleştirmek için Microsoft Azure Stack System Center Operations Manager izleme paketi**. Yeni bir sürümünü (1.0.3.0) Microsoft System Center Operations Manager izleme paketi Azure Stack için kullanılabilir [indirme](https://www.microsoft.com/download/details.aspx?id=55184). Bağlı bir Azure Stack dağıtımı eklediğinizde, bu sürümü ile hizmet sorumluları kullanabilirsiniz. Bu sürüm ayrıca düzeltme doğrudan Operations Manager içinde harekete geçmenizi sağlayan bir güncelleştirme yönetim deneyimi sunar. Kaynak sağlayıcılarını görüntüleme birimlerini ölçeklendirebilir ve birim düğümlerini ölçeklendirme yeni panolar da vardır.

- **Yeni Azure Stack yönetici PowerShell sürümü 1.3.0**.  Azure Stack PowerShell 1.3.0 artık yükleme için kullanılabilir. Bu sürüm, Azure Stack yönetmek tüm yönetici kaynak sağlayıcıları için komutlar sağlar.  Bu sürümle birlikte, bazı içeriklerin Azure Stack araçları Github'dan kaldırılacak [depo](https://github.com/Azure/AzureStack-Tools). 

   Yükleme ayrıntılarını izleyin [yönergeleri](azure-stack-powershell-install.md) veya [yardımcı](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) Azure Stack modülü 1.3.0 içeriği. 

- **İlk Azure Stack Rest API sürümü**. [Tüm Azure Stack yönetici kaynak sağlayıcıları için API Başvurusu](https://docs.microsoft.com/rest/api/azure-stack/) artık yayımlanır. 


## <a name="before-you-begin"></a>Başlamadan önce    

### <a name="prerequisites"></a>Önkoşullar
- Azure yığını'nı yükleme [1803 güncelleştirme](azure-stack-update-1803.md) Azure Stack 1804 güncelleştirmeyi uygulamadan önce.    

### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar   
- 1804 güncelleştirme yüklemesi sırasında başlık uyarılarla görebileceğiniz *hatası – şablon FaultType UserAccounts.New için eksik.*  Bu uyarılar güvenle yok sayabilirsiniz. 1804 Güncelleştirme tamamlandıktan sonra bu uyarıları otomatik olarak kapatılacak.   
 
- <!-- TBD - IS --> Bu güncelleştirme yüklemesi sırasında sanal makineleri oluşturmaya çalışmayın. Güncelleştirmeleri yönetme hakkında daha fazla bilgi için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar
1804 yüklendikten sonra geçerli düzeltmeleri yükleyin. Daha fazla bilgi için aşağıdaki Bilgi Bankası makaleleri görüntülemek hem de bizim [hizmet İlkesi](azure-stack-servicing-policy.md).  
 - [KB 4344114 - Azure Stack düzeltme 1.0.180527.15](https://support.microsoft.com/help/4344114).




### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)
Derleme için yükleme sonrası bilinen sorunlar verilmiştir **20180513.1**.

#### <a name="portal"></a>Portal
- <!-- 1272111 - IS --> Bu Azure Stack sürümüne güncelleştirin veya yükledikten sonra Yönetim Portalı'nda Azure Stack ölçek birimlerini görüntülemek mümkün olmayabilir.  
  Geçici çözüm: ölçek birimleri hakkında bilgi görüntülemek için PowerShell kullanma. Daha fazla bilgi için [yardımcı](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) Azure Stack modülü 1.3.0 içeriği. 

- <!-- 2332636 - IS -->  Bu Azure Stack sürümüne güncelleştirin ve Azure Stack kimlik sistemi için AD FS kullanıyorsanız, varsayılan sağlayıcı aboneliği varsayılan sahibi yerleşik olarak sıfırlanır **CloudAdmin** kullanıcı.  
  Geçici çözüm: Bu güncelleştirmeyi yükledikten sonra bu sorunu çözmek için 3 adımda kullanın [yapılandırmak için tetikleyici Otomasyon Talep sağlayıcı güveni Azure Stack'te](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) varsayılan sağlayıcı aboneliğin sahibi sıfırlamak için yordamı.   

- <!-- TBD - IS ASDK --> Bazı yönetim abonelik türlerinde kullanılamaz.  Azure Stack bu sürüme yükseltme yaptığınızda iki abonelik türlerini başlatılmalı [1804 sürümü ile sunulan](#new-features) konsolunda görünmez. Bu beklenen bir durumdur. Kullanılabilir abonelik türleridir *abonelik ölçümü*, ve *tüketim abonelik*. Bu abonelik türlerini 1804 sürümünden başlayarak yeni Azure Stack ortamlarında görülebilir ancak henüz kullanıma sunulmamıştır. Kullanmaya devam etmelidir *varsayılan sağlayıcı* abonelik türü.  


- <!-- TBD -  IS ASDK -->Özelliği [açılan listeden yeni bir destek isteği açmak için](azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen içinde Yönetici portalı kullanılamıyor. Bunun yerine, aşağıdaki bağlantıyı kullanın:     
    - İçin Azure Stack tümleşik sistemleri kullanan https://aka.ms/newsupportrequest.

- <!-- 2403291 - IS ASDK --> Yatay kaydırma çubuğunun alt kısmında bulunan yönetici ve Kullanıcı Portalı'nın kullanımını olmayabilir. Yatay kaydırma çubuğunun erişemiyorsanız, önceki bir dikey pencere portaldaki dikey penceresinin adı seçerek, gitmek için içerik haritaları en üstünde bulunan içerik haritası listeden görüntülemek istediğiniz kullanım portalın sol.
  ![İçerik haritası](media/azure-stack-update-1804/breadcrumb.png) 

- <!-- TBD - IS --> Yönetici portalı'nda işlem ve depolama kaynaklarını görüntülemek mümkün olmayabilir. Bu sorunun nedeni, yanlış başarılı olarak raporlanacak güncelleştirilecek neden olan güncelleştirme yüklemesi sırasında bir hata var. Bu sorun devam ederse Yardım için Microsoft Müşteri Destek Hizmetleri'ne başvurun.

- <!-- TBD - IS --> Bir boş Pano portalında görebilirsiniz. Pano kurtarmak için portalın sağ üst köşesindeki dişli simgesini seçin ve ardından **varsayılan ayarları geri**.

- <!-- TBD - IS ASDK --> Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

- <!-- TBD - IS ASDK --> Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.

- <!-- TBD - IS ASDK --> Yönetim Portalı'nda bir kritik uyarı görebileceğiniz *Microsoft.Update.Admin* bileşeni. Uyarı adı, açıklaması ve düzeltme tüm olarak görüntüle:  
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
 

#### <a name="compute"></a>İşlem
- <!-- TBD - IS --> Bir sanal makine dağıtımı için bir sanal makine boyutu seçerken, bazı F serisi VM boyutlarının görünür olmayan bir VM oluşturduğunuzda boyut seçici bir parçası olarak. Aşağıdaki VM boyutları seçicide görünmez: *F8s_v2*, *F16s_v2*, *F32s_v2*, ve *F64s_v2*.  
  Geçici bir çözüm olarak, bir VM'yi dağıtmak için aşağıdaki yöntemlerden birini kullanın. Her bir yöntemin, kullanmak istediğiniz VM boyutu belirtmeniz gerekir.

  - **Azure Resource Manager şablonu:** bir şablon kullandığınızda *vmSize* şablondaki istenen VM boyutu eşit. Örneğin, aşağıdaki kullanan bir VM dağıtmak için kullanılan *F32s_v2* boyutu:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **Azure CLI:** kullanabileceğiniz [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) benzer bir parametre olarak bir VM boyutu belirtin ve komutu `--size "Standard_F32s_v2"`.

  - **PowerShell:** kullanabileceğiniz PowerShell ile [New-AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) benzer VM boyutunu belirten parametresiyle `-VMSize "Standard_F32s_v2"`.


- <!-- TBD - IS ASDK --> Ölçeklendirme ayarları sanal makine ölçek kümeleri için portalda kullanılabilir değildir. Geçici çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürüm farklar nedeniyle, kullanmanız gereken `-Name` yerine parametre `-VMScaleSetName`.

- <!-- TBD - IS --> Portaldaki giderek bir kullanılabilirlik kümesi oluşturduğunuzda, **yeni** > **işlem** > **kullanılabilirlik kümesi**, oluşturabilmeniz için bir bir hata etki alanı ve 1 güncelleştirme etki alanı ile kullanılabilirlik kümesi. Yeni bir sanal makine oluştururken bir geçici çözüm olarak, PowerShell, CLI kullanarak veya içinden kullanılabilirlik oluşturma portalı.

- <!-- TBD - IS ASDK --> Portal, Azure Stack kullanıcı portalında sanal makine oluşturduğunuzda, D serisi VM ekleyebilirsiniz veri diskleri yanlış sayıda görüntüler. Tüm desteklenen D serisi VM'ler gibi çok sayıda veri diski Azure yapılandırması sağlayabilir.

- <!-- TBD - IS ASDK --> Bir VM görüntüsü oluşturulması başarısız olduğunda, nelze odstranit başarısız bir öğe için VM görüntüleri işlem dikey eklenebilir.

  Geçici çözüm olarak, Hyper-V ile oluşturduğunuz işlevsiz bir VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yolu C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silme engelleyen sorunu çözer. Ardından, işlevsiz bir görüntü oluşturduktan sonra 15 dakika başarıyla silebilirsiniz.

  Ardından, daha önce başarısız bir VM görüntüsü yeniden indirilemiyor deneyebilirsiniz.

- <!-- TBD - IS ASDK --> VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

- <!-- 1662991 IS ASDK --> Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur.  


#### <a name="networking"></a>Ağ
- <!-- 1766332 - IS ASDK --> Altında **ağ**, tıklarsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **ilkesine** bir VPN türü listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure Stack'te desteklenir.

- <!-- 2388980 - IS ASDK --> Bir VM oluşturulur ve bir genel IP adresiyle ilişkili sonra o VM'den bir IP adresi ilişkisini olamaz. İlişkisi kaldırma çalışıyor gibi görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu durumda (genellikle olarak adlandırılan bir *VIP takas*). Tüm gelecek bu IP adresi sonuç başlangıçta ilişkili VM ve yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.

- <!-- 2292271 - IS ASDK --> Bir teklif ve Kiracı abonelikle ilişkilendirilmiş planı parçası olan bir ağ kaynağı için bir kota sınırı yükseltirseniz, bu abonelik için yeni sınır uygulanmaz. Ancak, yeni sınır kota artırılır sonra oluşturulan yeni abonelikler için geçerlidir. 

  Bu sorunu gidermek için plan aboneliği ile ilişkili olduğunda, bir ağ Kotayı artırmak için bir eklenti planı kullanın. Daha fazla bilgi için bkz. nasıl [eklenti planı kullanılabilmesini](azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> DNS bölgesi veya yol tablosu kaynaklar ile ilişkili olan bir abonelik silinemiyor. Abonelik başarıyla silmek için DNS bölgesi ve yol tablosu kaynakları Kiracı abonelikten silmeniz gerekir. 
  

- <!-- 1902460 - IS ASDK --> Azure Stack destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu tüm Kiracı abonelikler arasında geçerlidir. Aynı IP adresine sahip bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı, sonraki oluşturulmasını denemeden sonra engellenir.

- <!-- 16309153 - IS ASDK --> DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu vnet'teki VM'ler için güncelleştirilmiş ayarları gönderiliyor değil.

- <!-- TBD - IS ASDK --> Azure Stack, VM dağıtıldıktan sonra ek ağ arabirimi bir sanal makine örneğine eklemeyi desteklemez. VM birden fazla ağ arabirimi gerektiriyorsa, dağıtım sırasında tanımlanmalıdır.

- <!-- 2096388 IS --> Bir ağ güvenlik grubu kurallarını güncelleştirmek için Yönetim Portalı'nı kullanamazsınız. 

    App Service için geçici çözüm: denetleyici örneği Uzak Masaüstü bağlantısı için gerekiyorsa, PowerShell ile ağ güvenlik grupları içindeki güvenlik kurallarını değiştirin.  Nasıl yapılır örnekleri aşağıda verilmiştir *izin*ve ardından yapılandırmasına geri *Reddet*:  
    
    - *İzin ver:*
 
      ```powershell    
      Connect-AzureRmAccount -EnvironmentName AzureStackAdmin
      
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
        
        Connect-AzureRmAccount -EnvironmentName AzureStackAdmin
        
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

- <!-- TBD - IS --> Yalnızca kaynak sağlayıcısı, ana bilgisayar SQL veya MySQL sunucuları üzerinde öğeleri oluşturmak için desteklenir. Kaynak sağlayıcısı tarafından oluşturulmamış bir ana bilgisayar sunucusunda oluşturulan öğeler, eşleşmeyen bir duruma neden olabilir.  

- <!-- IS, ASDK --> Özel karakterler, boşluk ve nokta dahil olmak üzere desteklenmez **ailesi** veya **katmanı** adları, SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda.


> [!NOTE]  
> <!-- TBD - IS --> Azure Stack 1804 için güncelleştirdikten sonra daha önce dağıttığınız SQL ve MySQL kaynak sağlayıcıları kullanmaya devam edebilirsiniz.  Yeni bir sürüm kullanıma sunulduğunda SQL ve MySQL güncelleştirme öneririz. Azure Stack gibi güncelleştirmeleri sırayla SQL ve MySQL kaynak sağlayıcıları için geçerlidir.  Örneğin, sürüm 1802 kullanıyorsanız, önce sürüm 1803 uygulayın ve ardından 1804 için güncelleştirin.      
>   
> 1804 güncelleştirme yüklemesi, SQL veya MySQL kaynak sağlayıcıları kullanıcılarınız tarafından geçerli kullanımını etkilemez.
> Kullandığınız kaynak sağlayıcıları sürümünden bağımsız olarak kullanıcılar verilerinizi veritabanlarındaki bilgiler değildir ve erişilebilir kalır.    



#### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Kullanıcılar, bunlar, ilk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

- <!-- TBD - IS ASDK --> (Çalışan, yönetim, ön uç rollerini) altyapıyı ölçeklendirme için sürüm notları için işlem açıklandığı PowerShell kullanmanız gerekir.

- <!-- TBD - IS ASDK --> App Service yalnızca dağıtılabilir içine **varsayılan sağlayıcı aboneliği** şu anda.  Gelecekteki bir güncelleştirmede App Service uygulamasına yeni ölçümü Azure Stack 1804 ' sunulan abonelik dağıtmak ve bu yeni abonelik için tüm mevcut dağıtımları da geçirilecektir.

#### <a name="usage"></a>Kullanım  
- <!-- TBD - IS ASDK --> Kullanım genel IP adresi kullanım ölçümü verilerini gösterir aynı *olay tarihi-saati* yerine her kaydın değerini *TimeDate* gösteren kaydın oluşturulduğu zaman damgası. Şu anda, genel IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.


<!-- #### Identity -->
<!-- #### Marketplace --> 


## <a name="download-the-update"></a>Güncelleştirmeyi indirin
Azure Stack 1804 güncelleştirme paketinden indirebileceğiniz [burada](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Ayrıca bkz.
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).
- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
