---
title: Azure Stack 1807 güncelleştirme | Microsoft Docs
description: Bilinen sorunlar da dahil olmak üzere Azure Stack tümleşik sistemleri için 1807 güncelleştirmesindeki yenilikler ve güncelleştirme karşıdan yükleme konumu hakkında bilgi edinin.
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
ms.date: 07/24/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: fe37a62d364d53d03a232b6fdc41e38a9ae2e30f
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228111"
---
# <a name="azure-stack-1807-update"></a>Azure Stack 1807 güncelleştirme

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bu makalede 1807 güncelleştirme paketinin içeriğini açıklar. Bu güncelleştirme, Azure Stack nerede güncelleştirmeyi indirin ve bu sürüm için bilinen sorunlar geliştirmeleri ve düzeltmeleri içerir. Bilinen sorunlar için güncelleştirme işlemini doğrudan ilgili sorunları ve derleme (yükleme sonrası) ile ilgili sorunlar ayrılır.

> [!IMPORTANT]        
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="build-reference"></a>Yapı Başvurusu    
Azure Stack 1807 güncelleştirmenin yapı numarasıdır **1.1807.XX.X**.  


### <a name="new-features"></a>Yeni Özellikler
Bu güncelleştirme Azure Stack için aşağıdaki geliştirmeleri içerir.

- <!-- 1658937 | ASDK, IS --> **Önceden tanımlanmış bir zamanlamaya göre yedeklemeleri başlatmak** -gereçlerden biri Azure Stack artık otomatik olarak düzenli aralıklarla insan müdahalesi ortadan kaldırmak için altyapı yedekleme tetikleyebilirsiniz. Azure Stack tanımlanan saklama süresinden daha eski yedeklemeler için dış paylaşımı da otomatik olarak temizler. 

- <!-- 2496385 | ASDK, IS --> **Eklenen veri aktarımı için toplam yedek zamanına zaman.**

-   <!-- 1702130 | ASDK, IS --> **Yedekleme dış kapasite artık dış paylaşım doğru kapasitesini gösterir.** (Daha önce bu kodu sabit-10 GB.)

- <!-- 2508488 |  IS   -->   Tarafından kapasiteyi genişletmeniz [ek ölçek birimi düğüm ekleme](azure-stack-add-scale-node.md).



### <a name="fixed-issues"></a>Giderilen sorunlar

- <!-- 448955 | IS ASDK --> Etkinlik günlükleri bir UTC + N saat diliminde dağıtılan sistemler için artık başarılı bir şekilde sorgulayabilirsiniz.    

- <!-- 2319627 |  ASDK, IS --> Yedekleme yapılandırması parametreleri (yol/kullanıcı adı/parola/şifreleme anahtarı) için ön denetim, artık için yedekleme yapılandırması ayarları yanlış ayarlar. (Daha önce ayarları yanlış yedekleme ayarlandığı ve yedekleme ardından başarısız olurdu ne zaman tirggered.)

- <!-- 2215948 |  ASDK, IS --> Dış paylaşımından el ile yedekleme sildiğinizde yedekleme listesinde artık yeniler.

- <!-- 2332636 - IS -->  Bu sürüm için güncelleştirme dağıtılan AD FS ile yerleşik CloudAdmin kullanıcı varsayılan sağlayıcı aboneliği varsayılan sahibi artık sıfırlar.

- <!-- 2360715 |  ASDK, IS -->  Bundan sonra veri merkezi tümleştirmeyi ayarladığınızda, AD FS meta veri dosyası bir paylaşımdan erişemezsiniz. Daha fazla bilgi için [Federasyon meta veri dosyası sağlayarak AD FS tümleştirmenin ayarlanması](azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

- <!-- 2388980 | ASDK, IS --> Bir sorunu düzelttik döndürülmesiyle kullanıcılardan var olan bir genel IP adresi atanmış, daha önce bir ağ arabirimiyle ya da yeni bir ağ arabirimiyle ya da yük dengeleyici için yük dengeleyici için atanmış.  

-   <!--2292271 | ASDK, IS --> Burada değiştirilmiş bir kota sınırı mevcut aboneliklerin uygulamadı sorununu düzelttik.  Artık, bir teklif parçası olan bir ağ kaynağı için bir kota sınırı yükseltmek ve planı bir kiracı aboneliği ile ilişkili olduğunda, önceden mevcut abonelikler, aynı zamanda için yeni abonelikler yeni sınır geçerlidir.

- **Çeşitli düzeltmeleri** performans, kararlılık, güvenlik ve Azure Stack tarafından kullanılan işletim sistemi için.


<!-- ### Additional releases timed with this update    -->



## <a name="before-you-begin"></a>Başlamadan önce    

### <a name="prerequisites"></a>Önkoşullar
- Azure yığını'nı yükleme [1805 güncelleştirme](azure-stack-update-1805.md) Azure Stack 1807 güncelleştirmeyi uygulamadan önce.  Hiçbir güncelleştirme 1806 vardı.  

- Güncelleştirme 1807 yüklemesi başlamadan önce çalıştırmak [Test AzureStack](azure-stack-diagnostic-test.md) Azure Stack durumunu doğrulamak ve bulunan tüm çalışma sorunlarını çözün. Ayrıca etkin Uyarıları gözden geçirin ve eylemi gerektiren tüm çözümleyin.

### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar   
- <!-- 2468613 - IS --> Bu güncelleştirme yüklemesi sırasında başlık uyarılarla görebileceğiniz *hatası – şablon FaultType UserAccounts.New için eksik.*  Bu uyarılar güvenle yok sayabilirsiniz. Bu güncelleştirme yüklemesi tamamlandıktan sonra bu uyarıları otomatik olarak kapatılacak.   

- <!-- 2489559 - IS --> Bu güncelleştirme yüklemesi sırasında sanal makineleri oluşturmaya çalışmayın. Güncelleştirmeleri yönetme hakkında daha fazla bilgi için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar
*1807 güncelleştirmesi güncelleştirme sonrası adımları yoktur.*

<!-- After the installation of this update, install any applicable Hotfixes. For more information view the following knowledge base articles, as well as our [Servicing Policy](azure-stack-servicing-policy.md).  
 - Link to KB  
 -->

## <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)
Bu derleme sürümü için yükleme sonrası bilinen sorunlar verilmiştir.

### <a name="portal"></a>Portal  
- <!-- 2551834 - IS, ASDK --> Seçtiğinizde, **genel bakış** için yönetici veya Kullanıcı Portalı, bilgileri bir depolama hesabında *Essentials* bölmesinde görüntülemez.  Temel bileşenler bölmesine gibi hesabıyla ilgili bilgileri görüntüler, *kaynak grubu*, *konumu*, ve *abonelik kimliği*.  Diğer seçenekleri genel bakış için erişilebilir gibi *Hizmetleri* ve *izleme*, farklı seçenekleri için *Gezgini'nde Aç* veya *depolama hesabını Sil* .

  Kullanılamayan bilgileri görüntülemek için kullanın [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) PowerShell cmdlet'i.

- <!-- 2551834 - IS, ASDK --> Seçtiğinizde, **etiketleri** Yöneticisi ya da Kullanıcı Portalı'nda depolama hesabı için bilgiler yüklenemiyor ve görüntülenmez.  

  Kullanılamayan bilgileri görüntülemek için kullanın [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) PowerShell cmdlet'i. 

- <!-- TBD - IS ASDK --> Bazı yönetim abonelik türlerinde kullanılamaz.  Azure Stack bu sürüme yükseltme yaptığınızda iki abonelik türlerini başlatılmalı [1804 sürümü ile sunulan](azure-stack-update-1804.md#new-features) konsolunda görünmez. Bu beklenen bir durumdur. Kullanılabilir abonelik türleridir *abonelik ölçümü*, ve *tüketim abonelik*. Bu abonelik türlerini 1804 sürümünden başlayarak yeni Azure Stack ortamlarında görülebilir ancak henüz kullanıma sunulmamıştır. Kullanmaya devam etmelidir *varsayılan sağlayıcı* abonelik türü.  

- <!-- 2403291 - IS ASDK --> Yatay kaydırma çubuğunun alt kısmında bulunan yönetici ve Kullanıcı Portalı'nın kullanımını olmayabilir. Yatay kaydırma çubuğunun erişemiyorsanız, önceki bir dikey pencere portaldaki dikey penceresinin adı seçerek, gitmek için içerik haritaları en üstünde bulunan içerik haritası listeden görüntülemek istediğiniz kullanım portalın sol.
  ![İçerik haritası](media/azure-stack-update-1804/breadcrumb.png)

- <!-- TBD - IS --> Yönetici portalı'nda işlem ve depolama kaynaklarını görüntülemek mümkün olmayabilir. Bu sorunun nedeni, yanlış başarılı olarak raporlanacak güncelleştirilecek neden olan güncelleştirme yüklemesi sırasında bir hata var. Bu sorun devam ederse Yardım için Microsoft Müşteri Destek Hizmetleri'ne başvurun.

- <!-- TBD - IS --> Bir boş Pano portalında görebilirsiniz. Pano kurtarmak için portalın sağ üst köşesindeki dişli simgesini seçin ve ardından **varsayılan ayarları geri**.

- <!-- TBD - IS ASDK --> Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

- <!-- TBD - IS ASDK --> Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.


### <a name="health-and-monitoring"></a>Sistem durumu ve izleme
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

  Her iki uyarılar güvenle yoksayılabilir ve zaman içinde otomatik olarak kapatılması.  

- <!-- 2368581 - IS. ASDK --> Düşük bellek uyarısı alırsanız ve Kiracı sanal makinelerini başarısız ile dağıtmak bir Azure Stack operatörü bir *Fabric VM oluşturma hatası*, Azure Stack damga kullanılabilir bellek yetersiz olabilir. Kullanım [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) iş yükleriniz için kapasite en iyi anlamak için.


### <a name="compute"></a>İşlem
- <!-- 2724873 - IS --> PowerShell cmdlet'lerini kullanırken **başlangıç AzsScaleUnitNode** veya **Stop-AzsScaleunitNode** ölçek birimleri yönetmek için başlatma veya durdurma ölçek birimi için yapılan ilk girişim başarısız olabilir. İlk çalıştırılmasında cmdlet'i başarısız olursa, cmdlet'in ikinci kez çalıştırın. İkinci çalıştırma işlemi tamamlamak için başarılı olmalıdır. 

- <!-- 2494144 - IS, ASDK --> Bir sanal makine dağıtımı için bir sanal makine boyutu seçerken, bazı F serisi VM boyutlarının görünür olmayan bir VM oluşturduğunuzda boyut seçici bir parçası olarak. Aşağıdaki VM boyutları seçicide görünmez: *F8s_v2*, *F16s_v2*, *F32s_v2*, ve *F64s_v2*.  
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

  Ardından, daha önce başarısız olan VM görüntüsünün indirilmesi yeniden deneyebilir.

- <!-- TBD - IS ASDK --> VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

- <!-- 1662991 IS ASDK --> Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur.  


### <a name="networking"></a>Ağ  

- <!-- 1766332 - IS ASDK --> Altında **ağ**, tıklarsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **ilkesine** bir VPN türü listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure Stack'te desteklenir.

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
> <!-- TBD - IS --> Bu Azure Stack sürümüne güncelleştirdikten sonra daha önce dağıttığınız SQL ve MySQL kaynak sağlayıcıları kullanmaya devam edebilirsiniz.  Yeni bir sürüm kullanıma sunulduğunda SQL ve MySQL güncelleştirme öneririz. Azure Stack gibi güncelleştirmeleri sırayla SQL ve MySQL kaynak sağlayıcıları için geçerlidir. Örneğin, 1804. sürümünü kullanıyorsanız, önce sürüm 1805 uygulayın ve ardından 1807 için güncelleştirin.      
>   
> Bu güncelleştirme yüklemesi, SQL veya MySQL kaynak sağlayıcıları kullanıcılarınız tarafından geçerli kullanımını etkilemez.
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
Azure Stack 1807 güncelleştirme paketinden indirebileceğiniz [burada](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Ayrıca bkz.
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).
- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).
