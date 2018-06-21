---
title: Azure yığın 1805 güncelleştirme | Microsoft Docs
description: Bilinen sorunlar da dahil olmak üzere Azure tümleşik yığını sistemler için 1805 güncelleştirmesindeki yenilikler ve güncelleştirme karşıdan yükleme konumu hakkında bilgi edinin.
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
ms.date: 06/20/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: 80ed0d2353fc6ea3a515c0d05475c713920abe46
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295750"
---
# <a name="azure-stack-1805-update"></a>Azure yığın 1805 güncelleştirme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bu makalede yenilikleri ve bilinen sorunlar bu sürüm ve güncelleştirmeyi yüklemek nereye 1805 güncelleştirme paketindeki giderir. Bilinen sorunlar için güncelleştirme işlemini doğrudan ilgili sorunlar ve yapı (yükleme sonrası) ile ilgili sorunları ayrılır.

> [!IMPORTANT]        
> Bu güncelleştirme paketi, yalnızca Azure tümleşik yığını sistemleri içindir. Bu güncelleştirme paketi Azure yığın Geliştirme Seti için geçerli değildir.

## <a name="build-reference"></a>Derleme başvurusu    
Azure yığın 1805 güncelleştirme yapı numarası **1.1805.1.47**.  

> [!TIP]  
> Müşteri geri bildirimi doğrultusunda, Microsoft Azure yığın için kullanımda sürüm şeması için bir güncelleştirme yok.  Bu güncelleştirmeyle, 1805, başlangıç yeni şema daha iyi geçerli bulut sürümünü temsil eder.  
> 
> Sürüm şema sunulmuştur *Version.YearYearMonthMonth.MinorVersion.BuildNumber* ikinci ve üçüncü kümeleri burada belirtmek sürüm ve sürüm. Örneğin, 1805.1 temsil *yayın üretim için* 1805 (RTM) sürümü.  


### <a name="new-features"></a>Yeni Özellikler
Bu güncelleştirme Azure yığını için aşağıdaki geliştirmeleri içerir.
<!-- 2297790 - IS, ASDK --> 
- **Artık Azure yığını içeren bir *Syslog* istemci** olarak bir *önizleme özelliği*. Bu istemci Azure yığınına dış bir Syslog sunucusu veya güvenlik bilgileri ve Olay yönetimi (SIEM) yazılımını Azure yığın altyapısına ilgili denetim ve güvenlik günlüklerini iletilmesini sağlar. Şu anda, Syslog istemcisi yalnızca varsayılan bağlantı noktası 514'üzerinden kimlik doğrulamasız UDP bağlantılar destekler. Her Syslog ileti yükünü ortak olay biçimi (CEF) olarak biçimlendirilir. 

  Syslog istemcisini yapılandırmak için kullanın **kümesi SyslogServer** ayrıcalıklı uç sunulan cmdlet'i. 

  Bu önizlemede, aşağıdaki üç uyarıları görebilirsiniz. Bu uyarılar dahil Azure yığını tarafından sunulan, *açıklamaları* ve *düzeltme* Kılavuzu. 
  - Başlık: Kod bütünlüğü devre dışı  
  - Başlık: Kod bütünlüğü denetim modunda 
  - Başlık: Kullanıcı hesabı oluşturuldu

  Bu özellik Önizleme'de olsa da, bunun üzerine üretim ortamlarında kullanılmamalıdır.   




### <a name="fixed-issues"></a>Giderilen sorunlar

<!-- # - applicability -->
- Sorunu engellenen biz sabit [aşağı açılır listeden yeni bir destek isteği açma](azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen yönetim portalında bulabilirsiniz. Bu seçenek artık tasarlandığı gibi çalışır. 

- **Çeşitli düzeltmeleri** performans, sağlamlık, güvenlik ve Azure yığını tarafından kullanılan işletim sistemi için.


<!-- # Additional releases timed with this update    -->



## <a name="before-you-begin"></a>Başlamadan önce    

### <a name="prerequisites"></a>Önkoşullar
- Azure yığın yükleme [1804 güncelleştirme](azure-stack-update-1804.md) Azure yığın 1805 güncelleştirmeyi uygulamadan önce.    
- Güncelleştirme 1805 yüklemesi başlamadan önce çalıştırın [Test AzureStack](azure-stack-diagnostic-test.md) Azure yığın durumunu doğrulamak ve bulunan tüm çalışma sorunlarını çözün. Ayrıca etkin Uyarıları gözden geçirin ve herhangi bir eylem gerektiren çözün. 

### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar   
- 1805 güncelleştirme yüklemesi sırasında başlık uyarılarla görebilirsiniz *hatası – FaultType UserAccounts.New için şablon eksik.*  Bu uyarılar güvenle yok sayabilirsiniz. 1805 Güncelleştirme tamamlandıktan sonra bu uyarılar otomatik olarak kapatılacak.   

- <!-- 2489559 - IS --> Bu güncelleştirmenin yüklenmesi sırasında sanal makineler oluşturmak çalışmayın. Güncelleştirmeleri, seSe yönetme hakkında daha fazla bilgi için [yönetmek Azure yığın genel bakış güncelleştirmelerinde](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar
1805 yüklendikten sonra geçerli düzeltmeleri yükleyin. Daha fazla bilgi için aşağıdaki Bilgi Bankası makaleleri görüntülemek yanı sıra bizim [hizmet İlkesi](azure-stack-servicing-policy.md).  
 - [KB 4340474 - Azure yığın düzeltme 1.1805.4.53](https://support.microsoft.com/en-us/help/4340474).


## <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)
Bu yapı sürümü için yükleme sonrası bilinen sorunlar verilmiştir.

### <a name="portal"></a>Portal  
- <!-- 2551834 - IS, ASDK --> Seçtiğinizde, **genel bakış** yönetici veya Kullanıcı Portalı, bilgileri bir depolama hesabı için *Essentials* bölmesinde görüntülemez.  Essentials bölmesi gibi hesabı hakkındaki bilgileri görüntüler kendi *kaynak grubu*, *konumu*, ve *abonelik kimliği*.  Genel bakış için diğer seçenekleri gibi erişilebilir *Hizmetleri* ve *izleme*, de olarak seçenekleri için *Gezgini'nde Aç* veya *depolama hesabı Sil* . 

  Kullanılabilir bilgi görüntülemek için kullanın [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) PowerShell cmdlet'i. 

- <!-- 2551834 - IS, ASDK --> Seçtiğinizde, **etiketleri** bir depolama hesabı için yönetici veya Kullanıcı Portalı'nda, bilgileri yüklenemiyordur ve görüntülenmez.  

  Kullanılabilir bilgi görüntülemek için kullanın [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) PowerShell cmdlet'i.


- <!-- 2332636 - IS -->  Azure yığınının bu sürüme güncelleştirin ve Azure yığın kimlik sistemi için AD FS kullandığınızda, varsayılan sağlayıcı aboneliğin varsayılan sahibi yerleşik olarak sıfırlanır **CloudAdmin** kullanıcı.  
  Geçici çözüm: Bu güncelleştirmeyi yükledikten sonra bu sorunu çözmek için 3 adımından kullanmak [yapılandırmak için tetikleyici Otomasyon Talep sağlayıcı güveni Azure yığınında](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) varsayılan sağlayıcı aboneliğin sahibi sıfırlamak için yordamı.   

- <!-- TBD - IS ASDK --> Bazı yönetim abonelik türleri kullanılabilir değildir.  Azure yığın bu sürüme yükselttiğinizde, olduğunu iki abonelik türlerini [1804 sürümü ile sunulan](azure-stack-update-1804.md#new-features) konsolunda görünmez. Bu beklenen bir durumdur. Kullanılabilir abonelik türleri *abonelik ölçümü*, ve *tüketim abonelik*. Bu abonelik türleri 1804 sürümünden başlayarak yeni Azure yığın ortamlarda görünür ancak henüz kullanıma hazır değil. Kullanmaya devam etmelidir *varsayılan sağlayıcı* abonelik türü.  

- <!-- 2403291 - IS ASDK --> Yatay kaydırma çubuğunun alt kısmındaki yönetici ve kullanıcı portalı kullanımını sahip olmayabilir. Yatay kaydırma çubuğu erişemiyorsanız, önceki bir dikey pencerede portal dikey adını seçerek, gitmek için içerik haritalarında en üstünde bulunan içerik haritası listeden görüntülemek istediğiniz kullanım portalın sol.
  ![İçerik haritası](media/azure-stack-update-1804/breadcrumb.png)

- <!-- TBD - IS --> Yönetici portalı'nda işlem ve depolama kaynaklarını görüntülemek mümkün olmayabilir. Bu sorunun nedeni yanlış başarılı olarak bildirilmesini güncelleştirme neden güncelleştirmenin yüklenmesi sırasında bir hata var. Bu sorun devam ederse, Yardım için Microsoft Müşteri Destek Hizmetleri'ne başvurun.

- <!-- TBD - IS --> Boş bir portal panosunda görebilirsiniz. Pano kurtarmak için portalın sağ üst köşesindeki dişli simgesini seçin ve ardından **varsayılan ayarları geri**.

- <!-- TBD - IS ASDK --> Kullanıcı abonelikleri yalnız bırakılmış kaynakları sonuçlarında siliniyor. Geçici bir çözüm olarak ilk kullanıcı kaynakları veya tüm kaynak grubunu silme ve kullanıcı abonelikleri silin.

- <!-- TBD - IS ASDK --> Azure yığın Portal kullanımı aboneliğinizi izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.


### <a name="health-and-monitoring"></a>Sistem durumu ve izleme
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

  Her iki uyarı güvenle yoksayılabilir ve zaman içinde otomatik olarak kapatılması.  

- <!-- 2368581 - IS. ASDK --> Düşük bellek uyarısı alırsanız ve Kiracı sanal makineler yük ile dağıtmak bir Azure yığın işleci bir *doku VM oluşturma hatası*, Azure yığın damga kullanılabilir bellek yetersiz olduğunu mümkündür. Kullanım [Azure yığın kapasite Planlayıcısı](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) , iş yükleri için kapasite en iyi anlamak için. 


### <a name="compute"></a>İşlem
- <!-- TBD - IS, ASDK --> Bir sanal makine dağıtımı için bir sanal makine boyutu seçerken, bazı F-serisi VM boyutları görünür olmayan bir VM oluştururken boyutu seçici bir parçası olarak. Aşağıdaki VM boyutları seçicide görünmez: *F8s_v2*, *F16s_v2*, *F32s_v2*, ve *F64s_v2*.  
  Geçici bir çözüm olarak, bir VM'yi dağıtmak için aşağıdaki yöntemlerden birini kullanın. Her bir yöntemin, kullanmak istediğiniz VM boyutu belirtmeniz gerekir.

  - **Azure Resource Manager şablonu:** bir şablonu kullandığınızda ayarlamak *vmSize* şablonda kullanmak istediğiniz VM boyutu eşit. Örneğin, şu girdiyi kullanan bir VM'yi dağıtmak için kullanılan *F32s_v2* boyutu:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **Azure CLI:** kullanabileceğiniz [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) komut ve benzer bir parametre olarak VM boyutu belirtin `--size "Standard_F32s_v2"`.

  - **PowerShell:** kullanabileceğiniz PowerShell ile [yeni AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) benzer VM boyutunu belirleyen parametresiyle `-VMSize "Standard_F32s_v2"`.


- <!-- TBD - IS ASDK --> Sanal makine ölçek kümeleri için ölçeklendirme ayarları portalda kullanılabilir değildir. Geçici bir çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürümü farklılıkları nedeniyle kullanmalısınız `-Name` yerine parametre `-VMScaleSetName`.

- <!-- TBD - IS --> Kullanılabilirlik giderek portalda kümesi oluşturduğunuzda **yeni** > **işlem** > **kullanılabilirlik kümesi**, oluşturabilmeniz için bir kullanılabilirlik, bir hata etki alanı ve güncelleştirme etki alanı 1 ile ayarlayın. Yeni bir sanal makine oluştururken, bir geçici çözüm olarak, kullanılabilirlik PowerShell'i, CLI, kullanarak veya içinden kümesini oluşturmak portal.

- <!-- TBD - IS ASDK --> Sanal makineler Azure yığın kullanıcı portalında oluşturduğunuzda, portal DS serisi VM iliştirebilirsiniz veri diskleri yanlış sayıda görüntüler. DS serisi VM'ler sayıda veri diski Azure yapılandırması sağlayabilir.

- <!-- TBD - IS ASDK --> Oluşturulacak bir VM görüntüsü başarısız olduğunda, VM görüntüleri işlem dikey penceresine eklenen silemezsiniz başarısız bir öğe.

  Geçici bir çözüm olarak, Hyper-V ile oluşturulan bir kukla VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yol C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silmeden engeller sorunu çözer. Ardından, kukla görüntüsünü oluşturduktan sonra 15 dakika başarılı bir şekilde silebilirsiniz.

  Ayrıca, daha önce başarısız VM görüntüsü yeniden indirin daha sonra deneyebilirsiniz.

- <!-- TBD - IS ASDK --> VM dağıtımı üzerinde uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemi durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermemelisiniz.  

- <!-- 1662991 IS ASDK --> Linux VM tanılama Azure yığınında desteklenmiyor. VM tanılaması etkin bir Linux VM dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz dağıtım de başarısız olur.  


### <a name="networking"></a>Ağ
- <!-- TBD - IS ASDK --> Yönetici veya Kullanıcı Portalı ya da kullanıcı tanımlı yollar oluşturamazsınız. Geçici bir çözüm olarak kullanmak [Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell).

- <!-- 1766332 - IS ASDK --> Altında **ağ**, tıklatırsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **İlkesi tabanlı** bir VPN türü olarak listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure yığınında desteklenir.

- <!-- 2388980 - IS ASDK --> Bir VM oluşturulur ve bir ortak IP adresi ile ilişkili sonra bu VM IP adresinden ilişkisini olamaz. Çalışmak için disassociation görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu davranış oluşur (genellikle olarak adlandırılan bir *VIP takası*). Tüm gelecekte bu IP adresi sonucu orijinal VM ve yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.

- <!-- 2292271 - IS ASDK --> Bir teklif ve Kiracı abonelikle ilişkili planı parçası olan bir ağ kaynağı için kota sınırı yükseltirseniz, bu abonelik için yeni sınır uygulanmaz. Ancak, yeni sınır kota artışı sonra oluşturulan yeni abonelikler için geçerlidir.

  Bu sorunu geçici olarak çözmek için plan zaten bir aboneliği ile ilişkili olduğunda ağ Kotayı artırmak için bir eklenti planı kullanın. Daha fazla bilgi için bkz: nasıl yapılır [bir eklenti planı kullanılabilir hale](azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> DNS bölgesi kaynakları veya yol tablosu kaynakları ilişkili olan bir abonelik silinemiyor. Abonelik başarıyla silmek için DNS bölgesi ve yol tablosu kaynakları Kiracı abonelikten silmeniz gerekir.


- <!-- 1902460 - IS ASDK --> Azure yığın destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu Kiracı abonelikler arasında geçerlidir. Aynı IP adresiyle bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı sonraki oluşturulmasını denemesinden sonra engellenir.

- <!-- 16309153 - IS ASDK --> Bir DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu Vnet vm'lerinin güncelleştirilmiş ayarları gönderilir değil.

- <!-- TBD - IS ASDK --> Azure yığın VM dağıtıldıktan sonra ek ağ arabirimleri VM örneğine eklemeyi desteklemez. VM birden fazla ağ arabirimi gerektiriyorsa, dağıtım sırasında tanımlanmalıdır.

- <!-- 2096388 IS --> Bir ağ güvenlik grubu kuralları için Yönetim Portalı'nı kullanamazsınız.

    Uygulama hizmeti için geçici çözüm: Denetleyici örnekleri için Uzak Masaüstü'nü gerekiyorsa, PowerShell ile ağ güvenlik grupları içindeki güvenlik kuralları Değiştir.  Nasıl yapılır örnekleri şunlardır *izin*ve yapılandırmasını geri yüklemek *Reddet*:  

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

- <!-- TBD - IS --> Yalnızca kaynak sağlayıcısı, o ana bilgisayar SQL veya MySQL sunucuları üzerinde öğeleri oluşturmak için desteklenir. Kaynak sağlayıcısı tarafından oluşturulmamış bir ana bilgisayar sunucusunda oluşturulan öğeleri eşleşmeyen bir duruma neden olabilir.  

- <!-- IS, ASDK --> Özel karakterler, boşluklar ve dönemleri dahil olmak üzere desteklenmiyor **ailesi** veya **katmanı** SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda adları.


> [!NOTE]  
> <!-- TBD - IS --> Azure yığın 1805 güncelleştirdikten sonra daha önce dağıtmış SQL ve MySQL kaynak sağlayıcıları kullanmaya devam edebilirsiniz.  Yeni bir sürüm kullanılabilir olduğunda SQL ve MySQL güncelleştirme öneririz. Azure yığın gibi güncelleştirmeleri sırayla SQL ve MySQL kaynak sağlayıcıları için geçerlidir. Örneğin, 1803 sürümünü kullanıyorsanız, önce sürüm 1804 uygulayın ve ardından 1805 için güncelleştirin.      
>   
> Güncelleştirme 1805 yüklemesini SQL veya MySQL kaynak sağlayıcıları kullanıcılarınız tarafından geçerli kullanımını etkilemez.
> Kullandığınız kaynak sağlayıcıları sürümü, bağımsız olarak kendi veritabanlarını kullanıcılar verilerinizi touched değil ve erişilebilir kalır.    



### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Kullanıcılar, bunlar ilk Azure işlevlerine abonelikte oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

- <!-- 2489178 - IS ASDK --> (Çalışanları, yönetim, ön uç rolleri) altyapıyı ölçeklendirme için PowerShell işlem için Sürüm Notları'nda açıklandığı şekilde kullanmanız gerekir.

- <!-- TBD - IS ASDK --> Uygulama hizmeti yalnızca dağıtılabilir içine *varsayılan sağlayıcı abonelik* şu anda. Uygulama hizmeti gelecek bir güncelleştirmede yeni içine dağıtacağınız *abonelik ölçümü* Azure yığın 1804 sunulmuştur. Ölçüm kullanılmak üzere desteklendiğinde, bu yeni abonelik türü için tüm mevcut dağıtımları geçirilecektir.


### <a name="usage"></a>Kullanım  
- <!-- TBD - IS ASDK --> Kullanım genel IP adresi kullanım ölçüm verileri gösterilir aynı *olay tarihi-saati* yerine her kayıt için değer *TimeDate* kaydının oluşturulduğu gösterilir Damga. Şu anda, ortak IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>Güncelleştirme karşıdan yükle
Azure yığın 1805 güncelleştirme paketinden indirebilirsiniz [burada](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Ayrıca bkz.
- İzleme ve güncelleştirmeleri sürdürmek için ayrıcalıklı uç noktası (CESARETLENDİRİCİ) kullanmak için bkz: [izlemek Azure kullanan ayrıcalıklı uç noktasını yığınında güncelleştirmeleri](azure-stack-monitor-update.md).
- Güncelleştirme yönetimi Azure yığınında genel bakış için bkz: [yönetmek güncelleştirmeleri Azure yığın genel bakış](azure-stack-updates.md).
- Azure yığın güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz: [Azure yığınında güncelleştirmelerini](azure-stack-apply-updates.md).
