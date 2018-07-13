---
title: Microsoft Azure Stack Geliştirme Seti sürüm notları | Microsoft Docs
description: Geliştirmeleri, düzeltmeleri ve bilinen sorunlar için Azure Stack Geliştirme Seti.
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
ms.date: 07/11/2018
ms.author: brenduns
ms.reviewer: misainat
ms.openlocfilehash: 86ac1f1b5433104faa89e1f107fa36fc1da5f70e
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989903"
---
# <a name="azure-stack-development-kit-release-notes"></a>Azure Stack Geliştirme Seti sürüm notları  
Bu sürüm notları geliştirmeleri ve düzeltmeleri Azure Stack geliştirme Seti'ni'de bilinen sorunlar hakkında bilgi sağlar. Hangi sürümü çalıştırdığınızdan emin değilseniz yapabilecekleriniz [denetlemek için portal'ı kullanmanızı](.\.\azure-stack-updates.md#determine-the-current-version).

> Abone olarak ASDK yenilikler ile güncel kalın [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [akış](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-11805147"></a>Derleme 1.1805.1.47

> [!TIP]  
> Müşteri geri bildirimi doğrultusunda, Microsoft Azure Stack için kullanılan sürüm şema için bir güncelleştirme yok. Bu güncelleştirmeyle, 1805, başlangıç yeni şemayı daha iyi geçerli bulut sürümü temsil eder.  
> 
> Sürüm şeması sunulmuştur *Version.YearYearMonthMonth.MinorVersion.BuildNumber* ikinci ve üçüncü kümeleri nerede göstermek sürüm ve sürüm. Örneğin, 1805.1 temsil *yayın için üretim* 1805 sürümü (RTM).  


### <a name="new-features"></a>Yeni Özellikler 
Bu derleme, Azure Stack için aşağıdaki geliştirmeleri ve düzeltmeleri içerir.  

- <!-- 2297790 - IS, ASDK --> **Azure Stack artık içeren bir *Syslog* istemci** olarak bir *önizleme özelliği*. Bu istemci, Azure Stack için dış bir Syslog sunucusu veya güvenlik bilgileri ve Olay yönetimi (SIEM) yazılımı için Azure Stack altyapısının ilgili denetim ve güvenlik günlüklerini iletilmesini sağlar. Şu anda, Syslog istemci varsayılan bağlantı noktası 514'üzerinden yalnızca kimliği doğrulanmamış UDP bağlantıyı destekler. Her Syslog ileti yükünü Common Event Format (CEF) olarak biçimlendirilir. 

  Syslog istemcisini yapılandırmak için kullanın **kümesi SyslogServer** ayrıcalıklı uç noktayı kullanıma cmdlet. 

  Bu önizlemeyle, aşağıdaki üç uyarılar görebilirsiniz. Azure yığını tarafından sunulan, bu uyarılar dahil *açıklamaları* ve *düzeltme* Kılavuzu. 
  - Başlık: Kod bütünlüğü devre dışı  
  - Başlık: Denetim modunda kod bütünlüğü 
  - Başlık: Oluşturulan kullanıcı hesabı

  Bu özellik Önizleme aşamasında olduğu sürece, bu bağlı üretim ortamlarında kullanılmamalıdır.   


### <a name="fixed-issues"></a>Giderilen sorunlar
- Sorunu engellenen düzelttik [açılan listeden yeni bir destek isteği açma](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen Yönetici portalındaki. Artık bu seçenek, beklendiği gibi çalışır. 

- **Çeşitli düzeltmeleri** performans, kararlılık, güvenlik ve Azure Stack tarafından kullanılan işletim sistemi


<!-- ### Changes  --> 


<!--   ### Additional releases timed with this update  -->


### <a name="known-issues"></a>Bilinen sorunlar
 
#### <a name="portal"></a>Portal
- <!-- 2551834 - IS, ASDK --> Seçtiğinizde, **genel bakış** için yönetici veya Kullanıcı Portalı, bilgileri bir depolama hesabında *Essentials* bölmesinde görüntülemez.  Temel bileşenler bölmesine gibi hesabıyla ilgili bilgileri görüntüler, *kaynak grubu*, *konumu*, ve *abonelik kimliği*.  Diğer seçenekleri genel bakış için erişilebilir gibi *Hizmetleri* ve *izleme*, farklı seçenekleri için *Gezgini'nde Aç* veya *depolama hesabını Sil* .  

  Kullanılamayan bilgileri görüntülemek için kullanın [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) PowerShell cmdlet'i. 

- <!-- 2551834 - IS, ASDK --> Seçtiğinizde, **etiketleri** Yöneticisi ya da Kullanıcı Portalı'nda depolama hesabı için bilgiler yüklenemiyor ve görüntülenmez.  

  Kullanılamayan bilgileri görüntülemek için kullanın [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) PowerShell cmdlet'i.

- <!-- TBD - IS ASDK --> Yeni Yönetim abonelik türlerini kullanmayın *abonelik ölçümü*, ve *tüketim abonelik*. Bu yeni abonelik türleriyle 1804 sürümüyle kullanıma sunulan ancak henüz kullanıma sunulmamıştır. Kullanmaya devam etmelidir *varsayılan sağlayıcı* abonelik türü.  

- <!-- 2403291 - IS ASDK --> Yatay kaydırma çubuğunun alt kısmında bulunan yönetici ve Kullanıcı Portalı'nın kullanımını olmayabilir. Yatay kaydırma çubuğunun erişemiyorsanız, önceki bir dikey pencere portaldaki dikey penceresinin adı seçerek, gitmek için içerik haritaları en üstünde bulunan içerik haritası listeden görüntülemek istediğiniz kullanım portalın sol.
  ![İçerik haritası](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

- <!-- TBD -  IS ASDK --> Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.


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

  Her iki uyarı #1 ve 2 # güvenle yoksayılabilir ve zaman içinde otomatik olarak kapatılması. 

  Aşağıdaki uyarı da görebilirsiniz *kapasite*. Bu uyarı için bir açıklama tanımlanan kullanılabilir bellek yüzdesini değişebilir:  

  Uyarı #3:
   - ADI: Düşük bellek kapasitesi
   - Önem DERECESİ: kritik
   - Bileşen: kapasite
   - Açıklama: Birden fazla %80.00 kullanılabilir bellek bölgesini tüketti. Büyük miktarlarda bellek ile sanal makine oluşturma başarısız olabilir.  

  Azure Stack bu sürümünde, yanlış bu uyarıyı tetikleyebilir. Kiracı sanal makineleri başarılı bir şekilde dağıtmak devam ederseniz, bu uyarıyı güvenle yok sayabilirsiniz. 
  
  Uyarı #3 otomatik olarak kapatmaz. Bu uyarıyı kapatırsanız, Azure Stack 15 dakika içinde aynı uyarı oluşturur.  

- <!-- 2368581 - IS ASDK --> Düşük bellek uyarısı alırsanız ve Kiracı sanal makinelerini başarısız ile dağıtmak bir Azure Stack operatörü bir *Fabric VM oluşturma hatası*, Azure Stack damga kullanılabilir bellek yetersiz olabilir. Kullanım [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) iş yükleriniz için kapasite en iyi anlamak için. 


#### <a name="compute"></a>İşlem
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


- <!-- TBD -  IS ASDK --> Ölçeklendirme ayarları sanal makine ölçek kümeleri için portalda kullanılabilir değildir. Geçici çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürüm farklar nedeniyle, kullanmanız gereken `-Name` yerine parametre `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Portal, Azure Stack kullanıcı portalında sanal makine oluşturduğunuzda, D serisi VM ekleyebilirsiniz veri diskleri yanlış sayıda görüntüler. Tüm desteklenen D serisi VM'ler gibi çok sayıda veri diski Azure yapılandırması sağlayabilir.

- <!-- TBD -  IS ASDK --> Bir VM görüntüsü oluşturulması başarısız olduğunda, nelze odstranit başarısız bir öğe için VM görüntüleri işlem dikey eklenebilir.

  Geçici çözüm olarak, Hyper-V ile oluşturduğunuz işlevsiz bir VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yolu C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silme engelleyen sorunu çözer. Ardından, işlevsiz bir görüntü oluşturduktan sonra 15 dakika başarıyla silebilirsiniz.

  Ardından, daha önce başarısız bir VM görüntüsü yeniden indirilemiyor deneyebilirsiniz.

- <!-- TBD -  IS ASDK --> VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

- <!-- 1662991 - IS ASDK --> Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur. 

#### <a name="networking"></a>Ağ
- <!-- TBD - IS ASDK --> Yönetici veya Kullanıcı Portalı ya da kullanıcı tanımlı yollar oluşturamazsınız. Geçici çözüm olarak, [Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell).

- <!-- 1766332 - IS, ASDK --> Altında **ağ**, tıklarsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **ilkesine** bir VPN türü listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure Stack'te desteklenir.

- <!-- 2388980 -  IS ASDK --> Bir VM oluşturulur ve bir genel IP adresiyle ilişkili sonra o VM'den bir IP adresi ilişkisini olamaz. İlişkisi kaldırma çalışıyor gibi görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu durumda (genellikle olarak adlandırılan bir *VIP takas*). Tüm gelecek bu IP adresi sonucu özgün VM ve yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.


- <!-- 2292271 - IS ASDK --> Bir teklif ve Kiracı abonelikle ilişkilendirilmiş planı parçası olan bir ağ kaynağı için bir kota sınırı yükseltirseniz, bu abonelik için yeni sınır uygulanmaz. Ancak, yeni sınır kota artırılır sonra oluşturulan yeni abonelikler için geçerlidir. 

  Bu sorunu gidermek için plan aboneliği ile ilişkili olduğunda, bir ağ Kotayı artırmak için bir eklenti planı kullanın. Daha fazla bilgi için bkz. nasıl [eklenti planı kullanılabilmesini](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> DNS bölgesi veya yol tablosu kaynaklar ile ilişkili olan bir abonelik silinemiyor. Abonelik başarıyla silmek için DNS bölgesi ve yol tablosu kaynakları Kiracı abonelikten silmeniz gerekir. 


- <!-- 1902460 -  IS ASDK --> Azure Stack destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu tüm Kiracı abonelikler arasında geçerlidir. Aynı IP adresine sahip bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı, sonraki oluşturulmasını denemeden sonra engellenir.

- <!-- 16309153 -  IS ASDK --> DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu vnet'teki VM'ler için güncelleştirilmiş ayarları gönderiliyor değil.
 
- <!-- TBD -  IS ASDK --> Azure Stack, VM dağıtıldıktan sonra ek ağ arabirimi bir sanal makine örneğine eklemeyi desteklemez. VM birden fazla ağ arabirimi gerektiriyorsa, dağıtım sırasında tanımlanmalıdır.


#### <a name="sql-and-mysql"></a>SQL ve MySQL 
- <!-- TBD - ASDK --> Veritabanını barındıran sunucu kaynak sağlayıcısı ve kullanıcı iş yükleri tarafından kullanım için ayrılmış olmalıdır. Uygulama Hizmetleri dahil olmak üzere diğer herhangi bir tüketici tarafından kullanılan bir örnek kullanılamaz.

- <!-- IS, ASDK --> Özel karakterler, boşluk ve nokta dahil olmak üzere desteklenmez **ailesi** adı, SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda. 

#### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Kullanıcılar, bunlar, ilk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

- <!-- TBD - IS ASDK --> (Çalışan, yönetim, ön uç rollerini) altyapıyı ölçeklendirme için sürüm notları için işlem açıklandığı PowerShell kullanmanız gerekir.  

- <!-- TBD - IS ASDK --> App Service yalnızca dağıtılabilir içine *varsayılan sağlayıcı aboneliği* şu anda. App Service gelecek bir güncelleştirmede yeni dağıtacağınız *abonelik ölçümü* Azure Stack 1804 ' tanıtılmıştır. Ölçüm kullanım için desteklenir, bu yeni bir abonelik türü için tüm mevcut dağıtımları geçirilecektir.

#### <a name="usage"></a>Kullanım  
- <!-- TBD -  IS ASDK --> Kullanım genel IP adresi kullanım ölçümü verilerini gösterir aynı *olay tarihi-saati* yerine her kaydın değerini *TimeDate* gösteren kaydın oluşturulduğu zaman damgası. Şu anda, genel IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.

<!-- #### Identity -->



## <a name="build-201805131"></a>Derleme 20180513.1

### <a name="new-features"></a>Yeni Özellikler 
Bu derleme, Azure Stack için aşağıdaki geliştirmeleri ve düzeltmeleri içerir.  

- <!-- 1759172 - IS, ASDK --> **Yeni Yönetim abonelikler**. 1804 ile portalda kullanılabilir iki yeni abonelik türü vardır. Bu yeni abonelik türleriyle 1804 sürümünden başlayarak yeni Azure Stack yükleme varsayılan sağlayıcı aboneliği yanı sıra ve görünür olan. *Azure Stack bu sürümü bu yeni abonelik türleriyle kullanmayın*. Bu abonelik türleri gelecek güncelleştirmelerden oturum kullanmak için kullanılabilirlik duyuracağız. 

  Bu yeni abonelik türleriyle görünür, ancak varsayılan sağlayıcı aboneliği güvenliğini sağlamak için ve SQL barındırma sunucuları gibi paylaşılan kaynakları dağıtmayı kolaylaştırmak için daha büyük bir değişikliğin bir parçası değildir. 

  Artık kullanılabilir üç abonelik türleri şunlardır:  
  - Sağlayıcı aboneliği varsayılan: Bu abonelik türü kullanmaya devam edin. 
  - Abonelik ölçümü: *Bu abonelik türü kullanmayın.*
  - Tüketim abonelik: *Bu abonelik türü kullanmayın*

### <a name="fixed-issues"></a>Giderilen sorunlar
- <!-- IS, ASDK -->  Yönetim Portalı'nda artık bir bilgi görüntülemeden önce güncelleştirme kutucuk yenilemeniz gerekir. 

- <!-- 2050709 - IS, ASDK -->  Ayrıca, Blob hizmeti, tablo hizmeti ve kuyruk hizmeti depolama ölçümleri düzenlemek için artık Yönetici portalını kullanabilir.

- <!-- IS, ASDK --> Altında **ağ**'ye tıkladığınızda **bağlantı** bir VPN bağlantısı kurmak için **siteden siteye (IPSec)** kullanılabilir tek seçenek sunulmuştur. 

- **Çeşitli düzeltmeleri** performans, kararlılık, güvenlik ve Azure Stack tarafından kullanılan işletim sistemi

<!-- ### Changes  --> 
### <a name="additional-releases-timed-with-this-update"></a>Bu güncelleştirme ile zaman aşımına ek sürümleri  
Aşağıdaki kullanıma sunulmuştur, ancak Azure Stack güncelleştirme 1804 gerektirmez.
- **Güncelleştirmek için Microsoft Azure Stack System Center Operations Manager izleme paketi**. Yeni bir sürümünü (1.0.3.0) Microsoft System Center Operations Manager izleme paketi Azure Stack için kullanılabilir [indirme](https://www.microsoft.com/download/details.aspx?id=55184). Bağlı bir Azure Stack dağıtımı eklediğinizde, bu sürümü ile hizmet sorumluları kullanabilirsiniz. Bu sürüm ayrıca düzeltme doğrudan Operations Manager içinde harekete geçmenizi sağlayan bir güncelleştirme yönetim deneyimi sunar. Kaynak sağlayıcılarını görüntüleme birimlerini ölçeklendirebilir ve birim düğümlerini ölçeklendirme yeni panolar da vardır.

- **Yeni Azure Stack yönetici PowerShell sürümü 1.3.0**.  Azure Stack PowerShell 1.3.0 artık yükleme için kullanılabilir. Bu sürüm, Azure Stack yönetmek tüm yönetici kaynak sağlayıcıları için komutlar sağlar.  Bu sürümle birlikte, bazı içeriklerin Azure Stack araçları Github'dan kaldırılacak [depo](https://github.com/Azure/AzureStack-Tools). 

   Yükleme ayrıntılarını izleyin [yönergeleri](.\.\azure-stack-powershell-install.md) veya [yardımcı](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) Azure Stack modülü 1.3.0 içeriği. 

- **İlk Azure Stack Rest API sürümü**. [Tüm Azure Stack yönetici kaynak sağlayıcıları için API Başvurusu](https://docs.microsoft.com/rest/api/azure-stack/) artık yayımlanır. 

### <a name="known-issues"></a>Bilinen sorunlar
 
#### <a name="portal"></a>Portal
- <!-- TBD - IS ASDK --> Özelliği [açılan listeden yeni bir destek isteği açmak için](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen içinde Yönetici portalı kullanılamıyor. Bunun yerine, aşağıdaki bağlantıyı kullanın:     
    - Azure Stack Geliştirme Seti için kullanmak https://aka.ms/azurestackforum.    

- <!-- 2403291 - IS ASDK --> Yatay kaydırma çubuğunun alt kısmında bulunan yönetici ve Kullanıcı Portalı'nın kullanımını olmayabilir. Yatay kaydırma çubuğunun erişemiyorsanız, önceki bir dikey pencere portaldaki dikey penceresinin adı seçerek, gitmek için içerik haritaları en üstünde bulunan içerik haritası listeden görüntülemek istediğiniz kullanım portalın sol.
  ![İçerik haritası](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

- <!-- TBD -  IS ASDK --> Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.

-   <!-- TBD -  IS ASDK --> Yönetim Portalı'nda Microsoft.Update.Admin bileşen için önemli bir uyarı görebilirsiniz. Uyarı adı, açıklaması ve düzeltme tüm olarak görüntüle:  
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
- <!-- TBD -  IS ASDK --> Ölçeklendirme ayarları sanal makine ölçek kümeleri için portalda kullanılabilir değildir. Geçici çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürüm farklar nedeniyle, kullanmanız gereken `-Name` yerine parametre `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Portal, Azure Stack kullanıcı portalında sanal makine oluşturduğunuzda, DS serisi VM ekleyebilirsiniz veri diskleri yanlış sayıda görüntüler. DS serisi VM'ler gibi çok sayıda veri diski Azure yapılandırması sağlayabilir.

- <!-- TBD -  IS ASDK --> Bir VM görüntüsü oluşturulması başarısız olduğunda, nelze odstranit başarısız bir öğe için VM görüntüleri işlem dikey eklenebilir.

  Geçici çözüm olarak, Hyper-V ile oluşturduğunuz işlevsiz bir VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yolu C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silme engelleyen sorunu çözer. Ardından, işlevsiz bir görüntü oluşturduktan sonra 15 dakika başarıyla silebilirsiniz.

  Ardından, daha önce başarısız bir VM görüntüsü yeniden indirilemiyor deneyebilirsiniz.

- <!-- TBD -  IS ASDK --> VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

- <!-- 1662991 - IS ASDK --> Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur. 

#### <a name="networking"></a>Ağ
- <!-- 1766332 - IS, ASDK --> Altında **ağ**, tıklarsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **ilkesine** bir VPN türü listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure Stack'te desteklenir.

- <!-- 2388980 -  IS ASDK --> Bir VM oluşturulur ve bir genel IP adresiyle ilişkili sonra o VM'den bir IP adresi ilişkisini olamaz. İlişkisi kaldırma çalışıyor gibi görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu durumda (genellikle olarak adlandırılan bir *VIP takas*). Tüm gelecek bu IP adresi sonucu özgün VM ve yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.

- <!-- 2292271 - IS ASDK --> Bir teklif ve Kiracı abonelikle ilişkilendirilmiş planı parçası olan bir ağ kaynağı için bir kota sınırı yükseltirseniz, bu abonelik için yeni sınır uygulanmaz. Ancak, yeni sınır kota artırılır sonra oluşturulan yeni abonelikler için geçerlidir. 

  Bu sorunu gidermek için plan aboneliği ile ilişkili olduğunda, bir ağ Kotayı artırmak için bir eklenti planı kullanın. Daha fazla bilgi için bkz. nasıl [eklenti planı kullanılabilmesini](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> DNS bölgesi veya yol tablosu kaynaklar ile ilişkili olan bir abonelik silinemiyor. Abonelik başarıyla silmek için DNS bölgesi ve yol tablosu kaynakları Kiracı abonelikten silmeniz gerekir. 


- <!-- 1902460 -  IS ASDK --> Azure Stack destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu tüm Kiracı abonelikler arasında geçerlidir. Aynı IP adresine sahip bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı, sonraki oluşturulmasını denemeden sonra engellenir.

- <!-- 16309153 -  IS ASDK --> DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu vnet'teki VM'ler için güncelleştirilmiş ayarları gönderiliyor değil.
 
- <!-- TBD -  IS ASDK --> Azure Stack, VM dağıtıldıktan sonra ek ağ arabirimi bir sanal makine örneğine eklemeyi desteklemez. VM birden fazla ağ arabirimi gerektiriyorsa, dağıtım sırasında tanımlanmalıdır.


#### <a name="sql-and-mysql"></a>SQL ve MySQL 
- <!-- TBD - ASDK --> Veritabanını barındıran sunucu kaynak sağlayıcısı ve kullanıcı iş yükleri tarafından kullanım için ayrılmış olmalıdır. Uygulama Hizmetleri dahil olmak üzere diğer herhangi bir tüketici tarafından kullanılan bir örnek kullanılamaz.

- <!-- IS, ASDK --> Özel karakterler, boşluk ve nokta dahil olmak üzere desteklenmez **ailesi** adı, SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda. 

#### <a name="app-service"></a>App Service
- <!-- TBD -  IS ASDK --> Kullanıcılar, bunlar, ilk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

- <!-- TBD -  IS ASDK --> (Çalışan, yönetim, ön uç rollerini) altyapıyı ölçeklendirme için sürüm notları için işlem açıklandığı PowerShell kullanmanız gerekir.
 
#### <a name="usage"></a>Kullanım  
- <!-- TBD -  IS ASDK --> Kullanım genel IP adresi kullanım ölçümü verilerini gösterir aynı *olay tarihi-saati* yerine her kaydın değerini *TimeDate* gösteren kaydın oluşturulduğu zaman damgası. Şu anda, genel IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.

<!--
#### Identity
-->



#### <a name="downloading-azure-stack-tools-from-github"></a>Github'dan Azure Stack araçları indirme
- Kullanırken *Invoke-webrequest* Github'dan Azure Stack indirmek için PowerShell cmdlet araçları, bir hata alırsınız:     
    -  *Invoke-webrequest: istek iptal edildi: SSL/TLS güvenli kanal oluşturulamadı.*     

  Bu hata, Tlsv1 ve Tlsv1.1 şifreleme standartları (PowerShell için varsayılan), yeni bir GitHub destek kullanımdan kaldırma nedeniyle oluşur. Daha fazla bilgi için [zayıf şifreleme standartları Temizleme Bildirimi](https://githubengineering.com/crypto-removal-notice/).

<!-- #### Identity -->



## <a name="build-201803021"></a>Derleme 20180302.1

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler
Azure Stack tümleşik sistemleri sürüm 1803 uygulamak için Azure Stack Geliştirme Seti için yayımlanan düzeltmeler ve yeni özellikleri. Bkz: [yeni özellikler](.\.\azure-stack-update-1803.md#new-features) ve [sorunlar düzeltildi](.\.\azure-stack-update-1803.md#fixed-issues) Azure Stack 1803 bölümlerini Güncelleştirme ayrıntıları için sürüm notları.  
> [!IMPORTANT]    
> Listelenen öğelerin bazıları **yeni özellikler** ve **sorunlar düzeltildi** bölümler yalnızca Azure Stack tümleşik sistemleri ilgilidir.

### <a name="changes"></a>Değişiklikler
- Yeni oluşturulan teklifinden durumunu değiştirmek için yol *özel* için *genel* veya *yetkisi alınmış* değişti. Daha fazla bilgi için [teklif oluşturma](.\.\azure-stack-create-offer.md). 


### <a name="known-issues"></a>Bilinen sorunlar
 
#### <a name="portal"></a>Portal
- Özelliği [açılan listeden yeni bir destek isteği açmak için](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen içinde Yönetici portalı kullanılamıyor. Bunun yerine, aşağıdaki bağlantıyı kullanın:     
    - Azure Stack Geliştirme Seti için kullanmak https://aka.ms/azurestackforum.    

- <!-- 2050709 --> Yönetim Portalı'nda Blob hizmeti, tablo hizmeti ve kuyruk hizmeti için depolama ölçümleri düzenlemek mümkün değildir. Depolama alanına gidin ve blob, tablo veya kuyruk hizmeti kutucuğu seçtiğinizde, bu hizmet için bir ölçüm grafiği görüntüleyen yeni bir dikey pencere açılır. Ölçüm grafiği kutucuğundan üst kısmından ardından Düzen seçerseniz, grafiği Düzenle dikey penceresi açılır ancak ölçümleri düzenleme seçeneklerini görüntülemez.  

- Gördüğünüz bir **etkinleştirme gerekli** , Azure Stack geliştirme Seti'ni kaydedilecek öneren uyarı bildirimi. Bu davranış beklenmektedir.

- Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

- Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.

- Yönetim Portalı'nın Panoda güncelleştirmeler hakkında bilgi görüntülemek kutucuğu güncelleştir başarısız olur. Bu sorunu çözmek için yenilemek için kutucuğa tıklayın.

-   Yönetim Portalı'nda Microsoft.Update.Admin bileşen için önemli bir uyarı görebilirsiniz. Uyarı adı, açıklaması ve düzeltme tüm olarak görüntüle:  
    - *HATA - şablon FaultType ResourceProviderTimeout için eksik.*

    Bu uyarıyı güvenle yoksayılabilir. 

- Hem Yönetim Portalı ve Kullanıcı Portalı, daha eski bir API sürümüyle oluşturulan depolama hesapları için genel bakış dikey seçtiğinizde yüklemeye genel bakış dikey başarısız (örnek: 2015-06-15 arasında). 

  Geçici bir çözüm olarak çalıştırmak için PowerShell kullanın. **başlangıç ResourceSynchronization.ps1** erişimi için depolama hesabı ayrıntıları geri yüklemek için betik. [Komut dosyasını Github'dan edinilebilir]( https://github.com/Azure/AzureStack-Tools/tree/master/Support/scripts)ve ASDK kullanırsanız, Hizmet Yöneticisi kimlik bilgileri ile Geliştirme Seti ana bilgisayarda çalıştırmanız gerekir.  

- **Hizmet durumu** dikey başarısız yüklenemedi. Hizmet durumu dikey penceresi yönetici veya Kullanıcı Portalı'nda, Azure Stack açtığınızda, bir hata görüntüler ve bilgi yüklemez. Bu beklenen bir davranıştır. Seçin ve hizmetin sistem durumunu açık olsa da, bu özellik henüz kullanılamıyor, ancak Azure Stack gelecek bir sürümünde uygulanacaktır.


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

- Azure Stack Yönetim Portalı'nda ada sahip bir kritik uyarı görebileceğiniz **dış sertifika sona erme bekleyen**.  Bu uyarıyı güvenle yok sayılabilir ve Azure Stack geliştirme Seti'ni işlemlerini etkiler. 


#### <a name="marketplace"></a>Market
- Kullanıcılar, abone olmadan tüm markete göz atabilir ve planlar ve teklifler gibi yönetim öğelerini görebilirsiniz. Bu kullanıcılara işlevsel olmayan öğelerdir.
 
#### <a name="compute"></a>İşlem
- Ölçeklendirme ayarları sanal makine ölçek kümeleri için portalda kullanılabilir değildir. Geçici çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürüm farklar nedeniyle, kullanmanız gereken `-Name` yerine parametre `-VMScaleSetName`.

- Portal, Azure Stack kullanıcı portalında sanal makine oluşturduğunuzda, DS serisi VM ekleyebilirsiniz veri diskleri yanlış sayıda görüntüler. DS serisi VM'ler gibi çok sayıda veri diski Azure yapılandırması sağlayabilir.

- Bir VM görüntüsü oluşturulması başarısız olduğunda, nelze odstranit başarısız bir öğe için VM görüntüleri işlem dikey eklenebilir.

  Geçici çözüm olarak, Hyper-V ile oluşturduğunuz işlevsiz bir VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yolu C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silme engelleyen sorunu çözer. Ardından, işlevsiz bir görüntü oluşturduktan sonra 15 dakika başarıyla silebilirsiniz.

  Ardından, daha önce başarısız bir VM görüntüsü yeniden indirilemiyor deneyebilirsiniz.

-  VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

- <!-- 1662991 --> Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur. 


#### <a name="networking"></a>Ağ
- Altında **ağ**, tıklarsanız **bağlantı** bir VPN bağlantısı kurmak için **VNet-VNet** olası bağlantı türü olarak listelenir. Bu seçeneği belirlemeyin. Şu anda yalnızca **siteden siteye (IPSec)** seçeneği desteklenmiyor.

- Bir VM oluşturulur ve bir genel IP adresiyle ilişkili sonra o VM'den bir IP adresi ilişkisini olamaz. İlişkisi kaldırma çalışıyor gibi görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu durumda (genellikle olarak adlandırılan bir *VIP takas*). Tüm gelecek bu IP adresi sonucu özgün VM ve yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.



- Azure Stack destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu tüm Kiracı abonelikler arasında geçerlidir. Aynı IP adresine sahip bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı, sonraki oluşturulmasını denemeden sonra engellenir.

- DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu vnet'teki VM'ler için güncelleştirilmiş ayarları gönderiliyor değil.
 
- Azure Stack, VM dağıtıldıktan sonra ek ağ arabirimi bir sanal makine örneğine eklemeyi desteklemez. VM birden fazla ağ arabirimi gerektiriyorsa, dağıtım sırasında tanımlanmalıdır.



#### <a name="sql-and-mysql"></a>SQL ve MySQL 
- Uygulamanın, kullanıcıların yeni bir SQL veya MySQL SKU veritabanlarını oluşturabilmek kadar bir saat sürebilir.

- Veritabanını barındıran sunucu kaynak sağlayıcısı ve kullanıcı iş yükleri tarafından kullanım için ayrılmış olmalıdır. Uygulama Hizmetleri dahil olmak üzere diğer herhangi bir tüketici tarafından kullanılan bir örnek kullanılamaz.

- <!-- IS, ASDK --> Özel karakterler, boşluk ve nokta dahil olmak üzere desteklenmez **ailesi** veya **katmanı** adları, SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda.

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





