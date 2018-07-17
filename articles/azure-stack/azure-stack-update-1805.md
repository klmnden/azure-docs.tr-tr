---
title: Azure Stack 1805 güncelleştirme | Microsoft Docs
description: Bilinen sorunlar da dahil olmak üzere Azure Stack tümleşik sistemleri için 1805 güncelleştirmesindeki yenilikler ve güncelleştirme karşıdan yükleme konumu hakkında bilgi edinin.
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
ms.openlocfilehash: ba162a04d41d9ce6f0bf00e377b7717f78967e7f
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091781"
---
# <a name="azure-stack-1805-update"></a>Azure Stack 1805 güncelleştirme

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bu makalede geliştirmeleri açıklar ve bilinen sorunlar bu sürümü ve güncelleştirmeyi yüklemek nereye 1805 güncelleştirme paketindeki giderir. Bilinen sorunlar için güncelleştirme işlemini doğrudan ilgili sorunları ve derleme (yükleme sonrası) ile ilgili sorunlar ayrılır.

> [!IMPORTANT]        
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="build-reference"></a>Yapı Başvurusu    
Azure Stack 1805 güncelleştirmenin yapı numarasıdır **1.1805.1.47**.  

> [!TIP]  
> Müşteri geri bildirimi doğrultusunda, Microsoft Azure Stack için kullanılan sürüm şema için bir güncelleştirme yok.  Bu güncelleştirmeyle, 1805, başlangıç yeni şemayı daha iyi geçerli bulut sürümü temsil eder.  
> 
> Sürüm şeması sunulmuştur *Version.YearYearMonthMonth.MinorVersion.BuildNumber* ikinci ve üçüncü kümeleri nerede göstermek sürüm ve sürüm. Örneğin, 1805.1 temsil *yayın için üretim* 1805 sürümü (RTM).  


### <a name="new-features"></a>Yeni Özellikler
Bu güncelleştirme Azure Stack için aşağıdaki geliştirmeleri içerir.
<!-- 2297790 - IS, ASDK --> 
- **Azure Stack artık içeren bir *Syslog* istemci** olarak bir *önizleme özelliği*. Bu istemci, Azure Stack için dış bir Syslog sunucusu veya güvenlik bilgileri ve Olay yönetimi (SIEM) yazılımı için Azure Stack altyapısının ilgili denetim ve güvenlik günlüklerini iletilmesini sağlar. Şu anda, Syslog istemci varsayılan bağlantı noktası 514'üzerinden yalnızca kimliği doğrulanmamış UDP bağlantıyı destekler. Her Syslog ileti yükünü Common Event Format (CEF) olarak biçimlendirilir. 

  Syslog istemcisini yapılandırmak için kullanın **kümesi SyslogServer** ayrıcalıklı uç noktayı kullanıma cmdlet. 

  Bu önizlemeyle, aşağıdaki üç uyarılar görebilirsiniz. Azure yığını tarafından sunulan, bu uyarılar dahil *açıklamaları* ve *düzeltme* Kılavuzu. 
  - Başlık: Kod bütünlüğü devre dışı  
  - Başlık: Denetim modunda kod bütünlüğü 
  - Başlık: Oluşturulan kullanıcı hesabı

  Bu özellik Önizleme aşamasında olduğu sürece, bu bağlı üretim ortamlarında kullanılmamalıdır.   




### <a name="fixed-issues"></a>Giderilen sorunlar

<!-- # - applicability -->
- Sorunu engellenen düzelttik [açılan listeden yeni bir destek isteği açma](azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen Yönetici portalındaki. Artık bu seçenek, beklendiği gibi çalışır. 

- **Çeşitli düzeltmeleri** performans, kararlılık, güvenlik ve Azure Stack tarafından kullanılan işletim sistemi için.


<!-- # Additional releases timed with this update    -->



## <a name="before-you-begin"></a>Başlamadan önce    

### <a name="prerequisites"></a>Önkoşullar
- Azure yığını'nı yükleme [1804 güncelleştirme](azure-stack-update-1804.md) Azure Stack 1805 güncelleştirmeyi uygulamadan önce.    
- 1805 güncelleştirme yüklemesi başlamadan önce çalıştırması [Test AzureStack](azure-stack-diagnostic-test.md) Azure Stack durumunu doğrulamak ve bulunan tüm çalışma sorunlarını çözün. Ayrıca etkin Uyarıları gözden geçirin ve eylemi gerektiren tüm çözümleyin. 

### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar   
- 1805 güncelleştirme yüklemesi sırasında başlık uyarılarla görebileceğiniz *hatası – şablon FaultType UserAccounts.New için eksik.*  Bu uyarılar güvenle yok sayabilirsiniz. Bu uyarılar, 1805 Güncelleştirme tamamlandıktan sonra otomatik olarak kapatılacak.   

- <!-- 2489559 - IS --> Bu güncelleştirme yüklemesi sırasında sanal makineleri oluşturmaya çalışmayın. Güncelleştirmeleri yönetme hakkında daha fazla bilgi için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar
1805 yüklendikten sonra geçerli düzeltmeleri yükleyin. Daha fazla bilgi için aşağıdaki Bilgi Bankası makaleleri görüntülemek hem de bizim [hizmet İlkesi](azure-stack-servicing-policy.md).  
 - [KB 4344102 - Azure Stack düzeltme 1.1805.7.57](https://support.microsoft.com/help/4344102).


## <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)
Bu derleme sürümü için yükleme sonrası bilinen sorunlar verilmiştir.

### <a name="portal"></a>Portal  
- <!-- 2551834 - IS, ASDK --> Seçtiğinizde, **genel bakış** için yönetici veya Kullanıcı Portalı, bilgileri bir depolama hesabında *Essentials* bölmesinde görüntülemez.  Temel bileşenler bölmesine gibi hesabıyla ilgili bilgileri görüntüler, *kaynak grubu*, *konumu*, ve *abonelik kimliği*.  Diğer seçenekleri genel bakış için erişilebilir gibi *Hizmetleri* ve *izleme*, farklı seçenekleri için *Gezgini'nde Aç* veya *depolama hesabını Sil* . 

  Kullanılamayan bilgileri görüntülemek için kullanın [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) PowerShell cmdlet'i. 

- <!-- 2551834 - IS, ASDK --> Seçtiğinizde, **etiketleri** Yöneticisi ya da Kullanıcı Portalı'nda depolama hesabı için bilgiler yüklenemiyor ve görüntülenmez.  

  Kullanılamayan bilgileri görüntülemek için kullanın [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) PowerShell cmdlet'i.


- <!-- 2332636 - IS -->  Bu Azure Stack sürümüne güncelleştirin ve Azure Stack kimlik sistemi için AD FS kullanıyorsanız, varsayılan sağlayıcı aboneliği varsayılan sahibi yerleşik olarak sıfırlanır **CloudAdmin** kullanıcı.  
  Geçici çözüm: Bu güncelleştirmeyi yükledikten sonra bu sorunu çözmek için 3 adımda kullanın [yapılandırmak için tetikleyici Otomasyon Talep sağlayıcı güveni Azure Stack'te](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) varsayılan sağlayıcı aboneliğin sahibi sıfırlamak için yordamı.   

- <!-- TBD - IS ASDK --> Bazı yönetim abonelik türlerinde kullanılamaz.  Azure Stack bu sürüme yükseltme yaptığınızda iki abonelik türlerini başlatılmalı [1804 sürümü ile sunulan](azure-stack-update-1804.md#new-features) konsolunda görünmez. Bu beklenen bir durumdur. Kullanılabilir abonelik türleridir *abonelik ölçümü*, ve *tüketim abonelik*. Bu abonelik türlerini 1804 sürümünden başlayarak yeni Azure Stack ortamlarında görülebilir ancak henüz kullanıma sunulmamıştır. Kullanmaya devam etmelidir *varsayılan sağlayıcı* abonelik türü.  

- <!-- 2403291 - IS ASDK --> Yatay kaydırma çubuğunun alt kısmında bulunan yönetici ve Kullanıcı Portalı'nın kullanımını olmayabilir. Yatay kaydırma çubuğunun erişemiyorsanız, önceki bir dikey pencere portaldaki dikey penceresinin adı seçerek, gitmek için içerik haritaları en üstünde bulunan içerik haritası listeden görüntülemek istediğiniz kullanım portalın sol.
  ![İçerik haritası](media/azure-stack-update-1804/breadcrumb.png)

- <!-- TBD - IS --> Yönetici portalı'nda işlem ve depolama kaynaklarını görüntülemek mümkün olmayabilir. Bu sorunun nedeni, yanlış başarılı olarak raporlanacak güncelleştirilecek neden olan güncelleştirme yüklemesi sırasında bir hata var. Bu sorun devam ederse Yardım için Microsoft Müşteri Destek Hizmetleri'ne başvurun.

- <!-- TBD - IS --> Bir boş Pano portalında görebilirsiniz. Pano kurtarmak için portalın sağ üst köşesindeki dişli simgesini seçin ve ardından **varsayılan ayarları geri**.

- <!-- TBD - IS ASDK --> Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

- <!-- TBD - IS ASDK --> Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.


### <a name="health-and-monitoring"></a>Sistem durumu ve izleme
- <!-- 1264761 - IS ASDK -->  Uyarıları görebilirsiniz *sistem durumu denetleyicisi* aşağıdaki ayrıntıları olan bir bileşeni:  
- 
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

  Her iki uyarı #1 ve 2 # güvenle yoksayılabilir ve zaman içinde otomatik olarak kapatılması. 

  Aşağıdaki uyarı da görebilirsiniz *kapasite*. Bu uyarı için bir açıklama tanımlanan kullanılabilir bellek yüzdesini değişebilir:  

  Uyarı #3:
   - ADI: Düşük bellek kapasitesi
   - Önem DERECESİ: kritik
   - Bileşen: kapasite
   - Açıklama: Birden fazla %80.00 kullanılabilir bellek bölgesini tüketti. Büyük miktarlarda bellek ile sanal makine oluşturma başarısız olabilir.  

  Azure Stack bu sürümünde, yanlış bu uyarıyı tetikleyebilir. Kiracı sanal makineleri başarılı bir şekilde dağıtmak devam ederseniz, bu uyarıyı güvenle yok sayabilirsiniz. 
  
  Uyarı #3 otomatik olarak kapatmaz. Bu uyarıyı kapatırsanız, Azure Stack 15 dakika içinde aynı uyarı oluşturur.  

- <!-- 2368581 - IS. ASDK --> Düşük bellek uyarısı alırsanız ve Kiracı sanal makinelerini başarısız ile dağıtmak Azure Stack operatör olarak, bir *Fabric VM oluşturma hatası*, Azure Stack damga kullanılabilir bellek yetersiz olabilir. Kullanım [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) iş yükleriniz için kapasite en iyi anlamak için. 


### <a name="compute"></a>İşlem
- <!-- TBD - IS, ASDK --> Bir sanal makine dağıtımı için bir sanal makine boyutu seçerken, bazı F serisi VM boyutlarının görünür olmayan bir VM oluşturduğunuzda boyut seçici bir parçası olarak. Aşağıdaki VM boyutları seçicide görünmez: *F8s_v2*, *F16s_v2*, *F32s_v2*, ve *F64s_v2*.  
  Geçici bir çözüm olarak, bir VM'yi dağıtmak için aşağıdaki yöntemlerden birini kullanın. Her bir yöntemin, kullanmak istediğiniz VM boyutu belirtmeniz gerekir.

  - **Azure Resource Manager şablonu:** bir şablon kullandığınızda *vmSize* şablonunda kullanmak istediğiniz VM boyutuna eşit olur. Örneğin, şu girişi kullanan bir VM dağıtmak için kullanılan *F32s_v2* boyutu:  

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

- <!-- TBD - IS ASDK --> Portal, Azure Stack kullanıcı portalında sanal makine oluşturduğunuzda, DS serisi VM ekleyebilirsiniz veri diskleri yanlış sayıda görüntüler. DS serisi VM'ler gibi çok sayıda veri diski Azure yapılandırması sağlayabilir.

- <!-- TBD - IS ASDK --> Bir VM görüntüsü oluşturulması başarısız olduğunda, nelze odstranit başarısız bir öğe için VM görüntüleri işlem dikey eklenebilir.

  Geçici çözüm olarak, Hyper-V ile oluşturduğunuz işlevsiz bir VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yolu C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silme engelleyen sorunu çözer. Ardından, işlevsiz bir görüntü oluşturduktan sonra 15 dakika başarıyla silebilirsiniz.

  Ardından, daha önce başarısız bir VM görüntüsü yeniden indirilemiyor deneyebilirsiniz.

- <!-- TBD - IS ASDK --> VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

- <!-- 1662991 IS ASDK --> Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur.  


### <a name="networking"></a>Ağ
- <!-- TBD - IS ASDK --> Yönetici veya Kullanıcı Portalı ya da kullanıcı tanımlı yollar oluşturamazsınız. Geçici çözüm olarak, [Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell).

- <!-- 1766332 - IS ASDK --> Altında **ağ**, tıklarsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **ilkesine** bir VPN türü listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure Stack'te desteklenir.

- <!-- 2388980 - IS ASDK --> Bir VM oluşturulur ve bir genel IP adresiyle ilişkili sonra o VM'den bir IP adresi ilişkisini olamaz. İlişkisi kaldırma çalışıyor gibi görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu durumda (genellikle olarak adlandırılan bir *VIP takas*). Tüm gelecek bu IP adresi sonucu özgün VM ve yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.

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


### <a name="sql-and-mysql"></a>SQL ve MySQL

- <!-- TBD - IS --> Yalnızca kaynak sağlayıcısı, ana bilgisayar SQL veya MySQL sunucuları üzerinde öğeleri oluşturmak için desteklenir. Kaynak sağlayıcısı tarafından oluşturulmamış bir ana bilgisayar sunucusunda oluşturulan öğeler, eşleşmeyen bir duruma neden olabilir.  

- <!-- IS, ASDK --> Özel karakterler, boşluk ve nokta dahil olmak üzere desteklenmez **ailesi** veya **katmanı** adları, SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda.


> [!NOTE]  
> <!-- TBD - IS --> Azure Stack 1805 için güncelleştirdikten sonra daha önce dağıttığınız SQL ve MySQL kaynak sağlayıcıları kullanmaya devam edebilirsiniz.  Yeni bir sürüm kullanıma sunulduğunda SQL ve MySQL güncelleştirme öneririz. Azure Stack gibi güncelleştirmeleri sırayla SQL ve MySQL kaynak sağlayıcıları için geçerlidir. Örneğin, 1803. sürümünü kullanıyorsanız, önce sürüm 1804 uygulayın ve ardından 1805 için güncelleştirin.      
>   
> Güncelleştirme 1805 yüklenmesi, kullanıcılarınız tarafından SQL veya MySQL kaynak sağlayıcıları geçerli kullanımını etkilemez.
> Kullandığınız kaynak sağlayıcıları sürümünden bağımsız olarak kullanıcılar verilerinizi veritabanlarındaki bilgiler değildir ve erişilebilir kalır.    



### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Kullanıcılar, bunlar, ilk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

- <!-- 2489178 - IS ASDK --> (Çalışan, yönetim, ön uç rollerini) altyapıyı ölçeklendirme için sürüm notları için işlem açıklandığı PowerShell kullanmanız gerekir.

- <!-- TBD - IS ASDK --> App Service yalnızca dağıtılabilir içine *varsayılan sağlayıcı aboneliği* şu anda. App Service gelecek bir güncelleştirmede yeni dağıtacağınız *abonelik ölçümü* Azure Stack 1804 ' tanıtılmıştır. Ölçüm kullanım için desteklenir, bu yeni bir abonelik türü için tüm mevcut dağıtımları geçirilecektir.


### <a name="usage"></a>Kullanım  
- <!-- TBD - IS ASDK --> Kullanım genel IP adresi kullanım ölçümü verilerini gösterir aynı *olay tarihi-saati* yerine her kaydın değerini *TimeDate* gösteren kaydın oluşturulduğu zaman damgası. Şu anda, genel IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>Güncelleştirmeyi indirin
Azure Stack 1805 güncelleştirme paketinden indirebileceğiniz [burada](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Ayrıca bkz.
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).
- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
