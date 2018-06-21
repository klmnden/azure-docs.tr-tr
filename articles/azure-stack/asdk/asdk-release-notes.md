---
title: Microsoft Azure yığın Geliştirme Seti sürüm notları | Microsoft Docs
description: Geliştirmeler, düzeltmeler ve Azure yığın Geliştirme Seti için bilinen sorunlar
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: brenduns
ms.reviewer: misainat
ms.openlocfilehash: bbd9bb0d56dd61fd0a32531ac425a1dbc1aa8923
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295794"
---
# <a name="azure-stack-development-kit-release-notes"></a>Azure yığın Geliştirme Seti sürüm notları  
Bu sürüm notları geliştirmeleri, düzeltmeler ve Azure yığın Geliştirme Seti bilinen sorunlar hakkında bilgi sağlar. Çalıştırdığınız hangi sürümünün emin değilseniz, yapabilecekleriniz [denetlemek için portal'ı kullanmanızı](.\.\azure-stack-updates.md#determine-the-current-version).

> Abone tarafından ASDK yenilikler ile güncel kalmasını [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [akış](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-11805147"></a>Yapı 1.1805.1.47

> [!TIP]  
> Müşteri geri bildirimi doğrultusunda, Microsoft Azure yığın için kullanımda sürüm şeması için bir güncelleştirme yok. Bu güncelleştirmeyle, 1805, başlangıç yeni şema daha iyi geçerli bulut sürümünü temsil eder.  
> 
> Sürüm şema sunulmuştur *Version.YearYearMonthMonth.MinorVersion.BuildNumber* ikinci ve üçüncü kümeleri burada belirtmek sürüm ve sürüm. Örneğin, 1805.1 temsil *yayın üretim için* 1805 (RTM) sürümü.  


### <a name="new-features"></a>Yeni Özellikler 
Bu yapı aşağıdaki geliştirmeleri ve düzeltmeler için Azure yığınına içerir.  

- <!-- 2297790 - IS, ASDK --> **Artık Azure yığını içeren bir *Syslog* istemci** olarak bir *önizleme özelliği*. Bu istemci Azure yığınına dış bir Syslog sunucusu veya güvenlik bilgileri ve Olay yönetimi (SIEM) yazılımını Azure yığın altyapısına ilgili denetim ve güvenlik günlüklerini iletilmesini sağlar. Şu anda, Syslog istemcisi yalnızca varsayılan bağlantı noktası 514'üzerinden kimlik doğrulamasız UDP bağlantılar destekler. Her Syslog ileti yükünü ortak olay biçimi (CEF) olarak biçimlendirilir. 

  Syslog istemcisini yapılandırmak için kullanın **kümesi SyslogServer** ayrıcalıklı uç sunulan cmdlet'i. 

  Bu önizlemede, aşağıdaki üç uyarıları görebilirsiniz. Bu uyarılar dahil Azure yığını tarafından sunulan, *açıklamaları* ve *düzeltme* Kılavuzu. 
  - Başlık: Kod bütünlüğü devre dışı  
  - Başlık: Kod bütünlüğü denetim modunda 
  - Başlık: Kullanıcı hesabı oluşturuldu

  Bu özellik Önizleme'de olsa da, bunun üzerine üretim ortamlarında kullanılmamalıdır.   


### <a name="fixed-issues"></a>Giderilen sorunlar
- Sorunu engellenen biz sabit [aşağı açılır listeden yeni bir destek isteği açma](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen yönetim portalında bulabilirsiniz. Bu seçenek artık tasarlandığı gibi çalışır. 

- **Çeşitli düzeltmeleri** performans, sağlamlık, güvenlik ve Azure yığını tarafından kullanılan işletim sistemi için


<!-- ### Changes  --> 


<!--   ### Additional releases timed with this update  -->


### <a name="known-issues"></a>Bilinen sorunlar
 
#### <a name="portal"></a>Portal
- <!-- 2551834 - IS, ASDK --> Seçtiğinizde, **genel bakış** yönetici veya Kullanıcı Portalı, bilgileri bir depolama hesabı için *Essentials* bölmesinde görüntülemez.  Essentials bölmesi gibi hesabı hakkındaki bilgileri görüntüler kendi *kaynak grubu*, *konumu*, ve *abonelik kimliği*.  Genel bakış için diğer seçenekleri gibi erişilebilir *Hizmetleri* ve *izleme*, de olarak seçenekleri için *Gezgini'nde Aç* veya *depolama hesabı Sil* .  

  Kullanılabilir bilgi görüntülemek için kullanın [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) PowerShell cmdlet'i. 

- <!-- 2551834 - IS, ASDK --> Seçtiğinizde, **etiketleri** bir depolama hesabı için yönetici veya Kullanıcı Portalı'nda, bilgileri yüklenemiyordur ve görüntülenmez.  

  Kullanılabilir bilgi görüntülemek için kullanın [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) PowerShell cmdlet'i.

- <!-- TBD - IS ASDK --> Yeni Yönetim abonelik türünü kullanmayın *abonelik ölçümü*, ve *tüketim abonelik*. Bu yeni abonelik türleri 1804 sürümü ile sunulan ancak henüz kullanıma hazır değil. Kullanmaya devam etmelidir *varsayılan sağlayıcı* abonelik türü.  

- <!-- 2403291 - IS ASDK --> Yatay kaydırma çubuğunun alt kısmındaki yönetici ve kullanıcı portalı kullanımını sahip olmayabilir. Yatay kaydırma çubuğu erişemiyorsanız, önceki bir dikey pencerede portal dikey adını seçerek, gitmek için içerik haritalarında en üstünde bulunan içerik haritası listeden görüntülemek istediğiniz kullanım portalın sol.
  ![İçerik haritası](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Kullanıcı abonelikleri yalnız bırakılmış kaynakları sonuçlarında siliniyor. Geçici bir çözüm olarak ilk kullanıcı kaynakları veya tüm kaynak grubunu silme ve kullanıcı abonelikleri silin.

- <!-- TBD -  IS ASDK --> Azure yığın Portal kullanımı aboneliğinizi izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.


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

  Her iki uyarı güvenle yoksayılabilir ve zaman içinde otomatik olarak kapatılacak.  

- <!-- 2392907 – ASDK -->   Görebilirsiniz bir *kritik* uyarısını **düşük bellek kapasitesi**. Bu uyarı aşağıdaki açıklama vardır: *bölge kullanılabilir bellek % 95,00'den fazla tüketti. Büyük miktarlarda bellek ile sanal makineler oluşturma başarısız olabilir.*

  Azure yığın bellek kullanımı için Azure yığın Geliştirme Seti yanlış hesaplarını. Bu uyarı oluşturulabilir.  

  Bu uyarıyı göz ardı edilebilir ve sorunu sanal makinelerin yerleşimi üzerinde hiçbir etkisi olmaz. 

- <!-- 2368581 - IS. ASDK --> Düşük bellek uyarısı alırsanız ve Kiracı sanal makineler yük ile dağıtmak bir Azure yığın işleci bir *doku VM oluşturma hatası*, Azure yığın damga kullanılabilir bellek yetersiz olduğunu mümkündür. Kullanım [Azure yığın kapasite Planne](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) , iş yükleri için kapasite en iyi anlamak için. 


#### <a name="compute"></a>İşlem
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


- <!-- TBD -  IS ASDK --> Sanal makine ölçek kümeleri için ölçeklendirme ayarları portalda kullanılabilir değildir. Geçici bir çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürümü farklılıkları nedeniyle kullanmalısınız `-Name` yerine parametre `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Sanal makineler Azure yığın kullanıcı portalında oluşturduğunuzda, portal D serisinin VM iliştirebilirsiniz veri diskleri yanlış sayıda görüntüler. Tüm desteklenen D serisinin VM'ler sayıda veri diski Azure yapılandırması sağlayabilir.

- <!-- TBD -  IS ASDK --> Oluşturulacak bir VM görüntüsü başarısız olduğunda, VM görüntüleri işlem dikey penceresine eklenen silemezsiniz başarısız bir öğe.

  Geçici bir çözüm olarak, Hyper-V ile oluşturulan bir kukla VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yol C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silmeden engeller sorunu çözer. Ardından, kukla görüntüsünü oluşturduktan sonra 15 dakika başarılı bir şekilde silebilirsiniz.

  Ayrıca, daha önce başarısız VM görüntüsü yeniden indirin daha sonra deneyebilirsiniz.

- <!-- TBD -  IS ASDK --> VM dağıtımı üzerinde uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemi durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermemelisiniz.  

- <!-- 1662991 - IS ASDK --> Linux VM tanılama Azure yığınında desteklenmiyor. VM tanılaması etkin bir Linux VM dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz dağıtım de başarısız olur. 

#### <a name="networking"></a>Ağ
- <!-- TBD - IS ASDK --> Yönetici veya Kullanıcı Portalı ya da kullanıcı tanımlı yollar oluşturamazsınız. Geçici bir çözüm olarak kullanmak [Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell).

- <!-- 1766332 - IS, ASDK --> Altında **ağ**, tıklatırsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **İlkesi tabanlı** bir VPN türü olarak listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure yığınında desteklenir.

- <!-- 2388980 -  IS ASDK --> Bir VM oluşturulur ve bir ortak IP adresi ile ilişkili sonra bu VM IP adresinden ilişkisini olamaz. Çalışmak için disassociation görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu davranış oluşur (genellikle olarak adlandırılan bir *VIP takası*). Tüm gelecekte bu IP adresi sonucu orijinal VM ve yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.


- <!-- 2292271 - IS ASDK --> Bir teklif ve Kiracı abonelikle ilişkili planı parçası olan bir ağ kaynağı için kota sınırı yükseltirseniz, bu abonelik için yeni sınır uygulanmaz. Ancak, yeni sınır kota artışı sonra oluşturulan yeni abonelikler için geçerlidir. 

  Bu sorunu geçici olarak çözmek için plan zaten bir aboneliği ile ilişkili olduğunda ağ Kotayı artırmak için bir eklenti planı kullanın. Daha fazla bilgi için bkz: nasıl yapılır [bir eklenti planı kullanılabilir hale](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> DNS bölgesi kaynakları veya yol tablosu kaynakları ilişkili olan bir abonelik silinemiyor. Abonelik başarıyla silmek için DNS bölgesi ve yol tablosu kaynakları Kiracı abonelikten silmeniz gerekir. 


- <!-- 1902460 -  IS ASDK --> Azure yığın destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu Kiracı abonelikler arasında geçerlidir. Aynı IP adresiyle bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı sonraki oluşturulmasını denemesinden sonra engellenir.

- <!-- 16309153 -  IS ASDK --> Bir DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu Vnet vm'lerinin güncelleştirilmiş ayarları gönderilir değil.
 
- <!-- TBD -  IS ASDK --> Azure yığın VM dağıtıldıktan sonra ek ağ arabirimleri VM örneğine eklemeyi desteklemez. VM birden fazla ağ arabirimi gerektiriyorsa, dağıtım sırasında tanımlanmalıdır.


#### <a name="sql-and-mysql"></a>SQL ve MySQL 
- <!-- TBD - ASDK --> Veritabanı sunucuları barındırma kaynak sağlayıcısı ve kullanıcı iş yükleri tarafından kullanım için ayrılmış olmalıdır. Uygulama Hizmetleri dahil olmak üzere diğer herhangi bir tüketici tarafından kullanılan örneğini kullanamazsınız.

- <!-- IS, ASDK --> Alanları ve dönemleri gibi özel karakterler desteklenmiyor **ailesi** SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda olarak adlandırın. 

#### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Kullanıcılar, bunlar ilk Azure işlevlerine abonelikte oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

- <!-- TBD - IS ASDK --> (Çalışanları, yönetim, ön uç rolleri) altyapıyı ölçeklendirme için PowerShell işlem için Sürüm Notları'nda açıklandığı şekilde kullanmanız gerekir.  

- <!-- TBD - IS ASDK --> Uygulama hizmeti yalnızca dağıtılabilir içine *varsayılan sağlayıcı abonelik* şu anda. Uygulama hizmeti gelecek bir güncelleştirmede yeni içine dağıtacağınız *abonelik ölçümü* Azure yığın 1804 sunulmuştur. Ölçüm kullanılmak üzere desteklendiğinde, bu yeni abonelik türü için tüm mevcut dağıtımları geçirilecektir.

#### <a name="usage"></a>Kullanım  
- <!-- TBD -  IS ASDK --> Kullanım genel IP adresi kullanım ölçüm verileri gösterilir aynı *olay tarihi-saati* yerine her kayıt için değer *TimeDate* kaydının oluşturulduğu gösterilir Damga. Şu anda, ortak IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.

<!-- #### Identity -->



## <a name="build-201805131"></a>Yapı 20180513.1

### <a name="new-features"></a>Yeni Özellikler 
Bu yapı aşağıdaki geliştirmeleri ve düzeltmeler için Azure yığınına içerir.  

- <!-- 1759172 - IS, ASDK --> **Yeni Yönetim abonelikler**. İle 1804 portalda kullanılabilecek iki yeni abonelik türü vardır. Bu yeni abonelik varsayılan sağlayıcı abonelik yanı sıra ve görünür 1804 sürümünden başlayarak yeni Azure yığın yüklemelerde türleridir. *Bu yeni abonelik türleri Azure yığını, bu sürümü ile kullanmayın*. Biz bu abonelik türleri gelecekteki bir güncelleştirme ile kullanmak için kullanılabilirlik Duyurusu. 

  Bu yeni abonelik türleri görünür, ancak varsayılan sağlayıcı abonelik güvenli ve SQL barındırma sunucuları gibi paylaşılan kaynakları dağıtmayı kolaylaştırmak için daha büyük bir değişikliğin parçası değildir. 

  Artık kullanılabilir üç abonelik türleri şunlardır:  
  - Sağlayıcı abonelik varsayılan: Bu abonelik türünde kullanmaya devam edin. 
  - Abonelik ölçümü: *Bu abonelik türünde kullanmayın.*
  - Tüketim abonelik: *Bu abonelik türünde kullanmayın*

### <a name="fixed-issues"></a>Giderilen sorunlar
- <!-- IS, ASDK -->  Yönetim Portalı'nda artık bilgi görüntülemeden önce güncelleştirme döşeme Yenile yok. 

- <!-- 2050709 - IS, ASDK -->  Blob hizmeti, tablo hizmeti ve kuyruk hizmeti için depolama ölçümlerini düzenlemek için Yönetim Portalı'nı şimdi kullanabilirsiniz.

- <!-- IS, ASDK --> Altında **ağ**'ye tıkladığınızda **bağlantı** bir VPN bağlantısı kurmak için **siteden siteye (IPSec)** kullanılabilir tek seçenek sunulmuştur. 

- **Çeşitli düzeltmeleri** performans, sağlamlık, güvenlik ve Azure yığını tarafından kullanılan işletim sistemi için

<!-- ### Changes  --> 
### <a name="additional-releases-timed-with-this-update"></a>Bu güncelleştirmeyle zaman aşımına ek sürümler  
Aşağıdaki artık kullanılabilir, ancak Azure yığın güncelleştirme 1804 gerektirmez.
- **Güncelleştirme izleme paketi Microsoft Azure yığın System Center Operations Manager**. Yeni bir sürümünü (1.0.3.0) Microsoft System Center Operations Manager izleme paketi Azure yığın kullanılabilir [karşıdan](https://www.microsoft.com/download/details.aspx?id=55184). Bağlı bir Azure yığın dağıtımına eklediğinizde bu sürümle birlikte, hizmet asıl adı kullanabilirsiniz. Bu sürüm ayrıca Operations Manager içinde düzeltme eyleme doğrudan izin veren bir güncelleştirme yönetim deneyimi sunar. Kaynak sağlayıcıları görüntülemek, birimleri ölçeklendirme ve birim düğümleri ölçeklendirme yeni panolar vardır.

- **Yeni Azure yığın yönetici PowerShell sürümü 1.3.0**.  Azure yığın PowerShell 1.3.0 yükleme için kullanıma sunulmuştur. Bu sürüm Azure yığın yönetmek tüm yönetim kaynak sağlayıcıları için komutları sağlar.  Bu sürümle birlikte, bazı içerikler Azure yığın araçları Github'dan kullanım dışı kalacaktır [depo](https://github.com/Azure/AzureStack-Tools). 

   Yükleme ayrıntılarını izleyin [yönergeleri](.\.\azure-stack-powershell-install.md) veya [yardımcı](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) Azure yığın modülü 1.3.0 için içerik. 

- **İlk Azure yığın API Rest başvurusu sürümü**. [Tüm Azure yığın yönetici kaynak sağlayıcıları için API Başvurusu](https://docs.microsoft.com/rest/api/azure-stack/) şimdi yayımlanır. 

### <a name="known-issues"></a>Bilinen sorunlar
 
#### <a name="portal"></a>Portal
- <!-- TBD - IS ASDK --> Özelliği [aşağı açılır listeden yeni bir destek isteği açma](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen içinde Yönetici portalı kullanılamıyor. Bunun yerine, aşağıdaki bağlantıyı kullanın:     
    - Azure yığın Geliştirme Seti için kullanmak https://aka.ms/azurestackforum.    

- <!-- 2403291 - IS ASDK --> Yatay kaydırma çubuğunun alt kısmındaki yönetici ve kullanıcı portalı kullanımını sahip olmayabilir. Yatay kaydırma çubuğu erişemiyorsanız, önceki bir dikey pencerede portal dikey adını seçerek, gitmek için içerik haritalarında en üstünde bulunan içerik haritası listeden görüntülemek istediğiniz kullanım portalın sol.
  ![İçerik haritası](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Kullanıcı abonelikleri yalnız bırakılmış kaynakları sonuçlarında siliniyor. Geçici bir çözüm olarak ilk kullanıcı kaynakları veya tüm kaynak grubunu silme ve kullanıcı abonelikleri silin.

- <!-- TBD -  IS ASDK --> Azure yığın Portal kullanımı aboneliğinizi izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.

-   <!-- TBD -  IS ASDK --> Yönetim Portalı'nda Microsoft.Update.Admin bileşeni için kritik bir uyarı görebilirsiniz. Uyarı adı, açıklama ve düzeltmesi tümü olarak görüntülenir:  
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
- <!-- TBD -  IS ASDK --> Sanal makine ölçek kümeleri için ölçeklendirme ayarları portalda kullanılabilir değildir. Geçici bir çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürümü farklılıkları nedeniyle kullanmalısınız `-Name` yerine parametre `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Sanal makineler Azure yığın kullanıcı portalında oluşturduğunuzda, portal DS serisi VM iliştirebilirsiniz veri diskleri yanlış sayıda görüntüler. DS serisi VM'ler sayıda veri diski Azure yapılandırması sağlayabilir.

- <!-- TBD -  IS ASDK --> Oluşturulacak bir VM görüntüsü başarısız olduğunda, VM görüntüleri işlem dikey penceresine eklenen silemezsiniz başarısız bir öğe.

  Geçici bir çözüm olarak, Hyper-V ile oluşturulan bir kukla VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yol C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silmeden engeller sorunu çözer. Ardından, kukla görüntüsünü oluşturduktan sonra 15 dakika başarılı bir şekilde silebilirsiniz.

  Ayrıca, daha önce başarısız VM görüntüsü yeniden indirin daha sonra deneyebilirsiniz.

- <!-- TBD -  IS ASDK --> VM dağıtımı üzerinde uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemi durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermemelisiniz.  

- <!-- 1662991 - IS ASDK --> Linux VM tanılama Azure yığınında desteklenmiyor. VM tanılaması etkin bir Linux VM dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz dağıtım de başarısız olur. 

#### <a name="networking"></a>Ağ
- <!-- 1766332 - IS, ASDK --> Altında **ağ**, tıklatırsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **İlkesi tabanlı** bir VPN türü olarak listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure yığınında desteklenir.

- <!-- 2388980 -  IS ASDK --> Bir VM oluşturulur ve bir ortak IP adresi ile ilişkili sonra bu VM IP adresinden ilişkisini olamaz. Çalışmak için disassociation görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu davranış oluşur (genellikle olarak adlandırılan bir *VIP takası*). Tüm gelecekte bu IP adresi sonucu orijinal VM ve yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.

- <!-- 2292271 - IS ASDK --> Bir teklif ve Kiracı abonelikle ilişkili planı parçası olan bir ağ kaynağı için kota sınırı yükseltirseniz, bu abonelik için yeni sınır uygulanmaz. Ancak, yeni sınır kota artışı sonra oluşturulan yeni abonelikler için geçerlidir. 

  Bu sorunu geçici olarak çözmek için plan zaten bir aboneliği ile ilişkili olduğunda ağ Kotayı artırmak için bir eklenti planı kullanın. Daha fazla bilgi için bkz: nasıl yapılır [bir eklenti planı kullanılabilir hale](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> DNS bölgesi kaynakları veya yol tablosu kaynakları ilişkili olan bir abonelik silinemiyor. Abonelik başarıyla silmek için DNS bölgesi ve yol tablosu kaynakları Kiracı abonelikten silmeniz gerekir. 


- <!-- 1902460 -  IS ASDK --> Azure yığın destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu Kiracı abonelikler arasında geçerlidir. Aynı IP adresiyle bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı sonraki oluşturulmasını denemesinden sonra engellenir.

- <!-- 16309153 -  IS ASDK --> Bir DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu Vnet vm'lerinin güncelleştirilmiş ayarları gönderilir değil.
 
- <!-- TBD -  IS ASDK --> Azure yığın VM dağıtıldıktan sonra ek ağ arabirimleri VM örneğine eklemeyi desteklemez. VM birden fazla ağ arabirimi gerektiriyorsa, dağıtım sırasında tanımlanmalıdır.


#### <a name="sql-and-mysql"></a>SQL ve MySQL 
- <!-- TBD - ASDK --> Veritabanı sunucuları barındırma kaynak sağlayıcısı ve kullanıcı iş yükleri tarafından kullanım için ayrılmış olmalıdır. Uygulama Hizmetleri dahil olmak üzere diğer herhangi bir tüketici tarafından kullanılan örneğini kullanamazsınız.

- <!-- IS, ASDK --> Alanları ve dönemleri gibi özel karakterler desteklenmiyor **ailesi** SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda olarak adlandırın. 

#### <a name="app-service"></a>App Service
- <!-- TBD -  IS ASDK --> Kullanıcılar, bunlar ilk Azure işlevlerine abonelikte oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

- <!-- TBD -  IS ASDK --> (Çalışanları, yönetim, ön uç rolleri) altyapıyı ölçeklendirme için PowerShell işlem için Sürüm Notları'nda açıklandığı şekilde kullanmanız gerekir.
 
#### <a name="usage"></a>Kullanım  
- <!-- TBD -  IS ASDK --> Kullanım genel IP adresi kullanım ölçüm verileri gösterilir aynı *olay tarihi-saati* yerine her kayıt için değer *TimeDate* kaydının oluşturulduğu gösterilir Damga. Şu anda, ortak IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.

<!--
#### Identity
-->



#### <a name="downloading-azure-stack-tools-from-github"></a>Azure yığın araçları Github'dan indirme
- Kullanırken *çağırma webrequest* Azure yığın indirmek için PowerShell cmdlet araçları Github'dan, hata iletisi:     
    -  *çağırma webrequest: istek iptal edildi: SSL/TLS güvenli kanalı oluşturulamadı.*     

  Bir son GitHub destek kullanımdan Tlsv1 ve Tlsv1.1 şifreleme standartları (PowerShell için varsayılan) nedeniyle bu hata oluşur. Daha fazla bilgi için bkz: [zayıf şifreleme standartları kaldırma bildirimi](https://githubengineering.com/crypto-removal-notice/).

<!-- #### Identity -->



## <a name="build-201803021"></a>Yapı 20180302.1

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler
Yeni özellikleri ve düzeltmeleri Azure tümleşik sistemleri sürüm 1803 uygulamak için Azure yığın Geliştirme Seti yığınının yayımladı. Bkz: [yeni özellikler](.\.\azure-stack-update-1803.md#new-features) ve [giderilen sorunlar](.\.\azure-stack-update-1803.md#fixed-issues) Azure yığın 1803 bölümlerini Güncelleştirme ayrıntıları için sürüm notları.  
> [!IMPORTANT]    
> Listelenen öğelerin bazıları **yeni özellikler** ve **giderilen sorunlar** bölümleri yalnızca Azure tümleşik yığını sistemler için ilgili.

### <a name="changes"></a>Değişiklikler
- Yeni oluşturulan bir tekliften durumunu değiştirmek için yol *özel* için *ortak* veya *yetkisi alınmış* değişti. Daha fazla bilgi için bkz: [teklifi oluşturmak](.\.\azure-stack-create-offer.md). 


### <a name="known-issues"></a>Bilinen sorunlar
 
#### <a name="portal"></a>Portal
- Özelliği [aşağı açılır listeden yeni bir destek isteği açma](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen içinde Yönetici portalı kullanılamıyor. Bunun yerine, aşağıdaki bağlantıyı kullanın:     
    - Azure yığın Geliştirme Seti için kullanmak https://aka.ms/azurestackforum.    

- <!-- 2050709 --> Yönetim Portalı'nda Blob hizmeti, tablo hizmeti ya da sıra hizmeti için depolama ölçümlerini düzenlemek mümkün değil. Depolama birimine gidin ve blob, tablo veya kuyruğu hizmeti bölmesi seçtiğinizde, bu hizmet için bir ölçüm grafik görüntüleyen yeni bir dikey pencere açılır. Ölçümleri grafik döşeme üstten ardından Düzenle seçerseniz grafiği Düzenle dikey penceresini açar ancak ölçümleri düzenlemek için seçenekleri görüntülemez.  

- Gördüğünüz bir **gerekli etkinleştirme** Azure yığın Geliştirme Seti kaydetmek için öneren bir uyarı bildirimi. Bu davranış beklenir.

- Kullanıcı abonelikleri yalnız bırakılmış kaynakları sonuçlarında siliniyor. Geçici bir çözüm olarak ilk kullanıcı kaynakları veya tüm kaynak grubunu silme ve kullanıcı abonelikleri silin.

- Azure yığın Portal kullanımı aboneliğinizi izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.

- Yönetim Portalı'nı panosunda güncelleştirmeler hakkında bilgi görüntülemek güncelleştirme döşeme başarısız olur. Bu sorunu çözmek için yenilemeniz kutucuğa tıklayın.

-   Yönetim Portalı'nda Microsoft.Update.Admin bileşeni için kritik bir uyarı görebilirsiniz. Uyarı adı, açıklama ve düzeltmesi tümü olarak görüntülenir:  
    - *HATA - FaultType ResourceProviderTimeout için şablon eksik.*

    Bu uyarı güvenle yoksayılabilir. 

- Hem Yönetim Portalı ve Kullanıcı Portalı, daha eski bir API sürümüyle oluşturulmuş depolama hesapları için genel bakış dikey seçtiğinizde yüklemeye genel bakış dikey penceresinde başarısız (örnek: 2015-06-15). 

  Geçici bir çözüm olarak çalıştırmak için PowerShell kullanın **başlangıç ResourceSynchronization.ps1** için depolama hesabı ayrıntıları erişimi geri yüklemek için komut dosyası. [Komut dosyası Github'dan edinilebilir]( https://github.com/Azure/AzureStack-Tools/tree/master/Support/scripts)ve ASDK kullanırsanız, Hizmet Yöneticisi kimlik bilgileriyle Geliştirme Seti ana bilgisayarda çalıştırmanız gerekir.  

- **Hizmet durumu** dikey başarısız yüklenemiyor. Yönetici veya Kullanıcı Portalı'nda Azure yığın hizmet durumu dikey penceresini açtığınızda bir hata görüntüler ve bilgi yüklemez. Bu beklenen bir davranıştır. Seçin ve hizmetin sistem durumunu açın, ancak bu özellik henüz kullanılabilir değil ancak Azure yığın gelecek bir sürümünde uygulanacaktır.


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

- Azure yığın Yönetim Portalı'nda ada sahip bir kritik uyarı görebilirsiniz **dış sertifika sona erme bekleyen**.  Bu uyarı güvenle yoksayılabilir ve Azure yığın Geliştirme Seti işlemleri etkilemez. 


#### <a name="marketplace"></a>Market
- Kullanıcılar bir abonelik olmadan tam Market göz atabilir ve planları ve teklifleri gibi yönetim öğelerini görebilirsiniz. Bu öğeler kullanıcılara işlevsiz.
 
#### <a name="compute"></a>İşlem
- Sanal makine ölçek kümeleri için ölçeklendirme ayarları portalda kullanılabilir değildir. Geçici bir çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürümü farklılıkları nedeniyle kullanmalısınız `-Name` yerine parametre `-VMScaleSetName`.

- Sanal makineler Azure yığın kullanıcı portalında oluşturduğunuzda, portal DS serisi VM iliştirebilirsiniz veri diskleri yanlış sayıda görüntüler. DS serisi VM'ler sayıda veri diski Azure yapılandırması sağlayabilir.

- Oluşturulacak bir VM görüntüsü başarısız olduğunda, VM görüntüleri işlem dikey penceresine eklenen silemezsiniz başarısız bir öğe.

  Geçici bir çözüm olarak, Hyper-V ile oluşturulan bir kukla VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yol C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silmeden engeller sorunu çözer. Ardından, kukla görüntüsünü oluşturduktan sonra 15 dakika başarılı bir şekilde silebilirsiniz.

  Ayrıca, daha önce başarısız VM görüntüsü yeniden indirin daha sonra deneyebilirsiniz.

-  VM dağıtımı üzerinde uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemi durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermemelisiniz.  

- <!-- 1662991 --> Linux VM tanılama Azure yığınında desteklenmiyor. VM tanılaması etkin bir Linux VM dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz dağıtım de başarısız olur. 


#### <a name="networking"></a>Ağ
- Altında **ağ**, tıklatırsanız **bağlantı** bir VPN bağlantısı kurmak için **VNet-VNet** olası bağlantı türü olarak listelenir. Bu seçeneği belirlemeyin. Şu anda yalnızca **siteden siteye (IPSec)** seçeneği desteklenir.

- Bir VM oluşturulur ve bir ortak IP adresi ile ilişkili sonra bu VM IP adresinden ilişkisini olamaz. Çalışmak için disassociation görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu davranış oluşur (genellikle olarak adlandırılan bir *VIP takası*). Tüm gelecekte bu IP adresi sonucu orijinal VM ve yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.



- Azure yığın destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu Kiracı abonelikler arasında geçerlidir. Aynı IP adresiyle bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı sonraki oluşturulmasını denemesinden sonra engellenir.

- Bir DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu Vnet vm'lerinin güncelleştirilmiş ayarları gönderilir değil.
 
- Azure yığın VM dağıtıldıktan sonra ek ağ arabirimleri VM örneğine eklemeyi desteklemez. VM birden fazla ağ arabirimi gerektiriyorsa, dağıtım sırasında tanımlanmalıdır.



#### <a name="sql-and-mysql"></a>SQL ve MySQL 
- Kullanıcıların veritabanlarını yeni bir SQL veya MySQL SKU oluşturabilmeniz için önce en çok bir saat sürebilir.

- Veritabanı sunucuları barındırma kaynak sağlayıcısı ve kullanıcı iş yükleri tarafından kullanım için ayrılmış olmalıdır. Uygulama Hizmetleri dahil olmak üzere diğer herhangi bir tüketici tarafından kullanılan örneğini kullanamazsınız.

- <!-- IS, ASDK --> Özel karakterler, boşluklar ve dönemleri dahil olmak üzere desteklenmiyor **ailesi** veya **katmanı** SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda adları.

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





