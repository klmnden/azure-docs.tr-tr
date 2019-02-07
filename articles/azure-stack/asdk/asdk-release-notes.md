---
title: Microsoft Azure Stack Geliştirme Seti sürüm notları | Microsoft Docs
description: Geliştirmeleri, düzeltmeleri ve bilinen sorunlar için Azure Stack Geliştirme Seti.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2018
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 12/21/2018
ms.openlocfilehash: c60ba4f4106ddd0c3fc643288894fb55d3d27f8c
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55816467"
---
# <a name="asdk-release-notes"></a>ASDK sürüm notları 
 
Bu makalede, geliştirmeleri, düzeltmeleri ve bilinen sorunlar Azure Stack geliştirme Seti'ni (ASDK) hakkında bilgi sağlar. Hangi sürümü çalıştırdığınızdan emin değilseniz yapabilecekleriniz [denetlemek için portal'ı kullanmanızı](../azure-stack-updates.md#determine-the-current-version).

> Abone olarak ASDK yenilikler ile güncel kalın [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [akış](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-118110101"></a>Derleme 1.1811.0.101

### <a name="new-features"></a>Yeni Özellikler

Bu derleme, Azure Stack için aşağıdaki geliştirmeleri ve düzeltmeleri içerir:  

- Bir dizi yeni en düşük ve önerilen donanım ve yazılım gereksinimleri ASDK yoktur. Özellikleri önerilen yeni bu bölümünde belgelendirilen [Azure Stack dağıtım planlama konuları](asdk-deploy-considerations.md). Azure Stack platform geliştirildiğini gibi diğer hizmetler kullanıma sunulmuştur ve daha fazla kaynak gerekli olabilir. Bu yeniden düzenlenen öneriler artan özellikleri yansıtır.

### <a name="fixed-issues"></a>Düzeltilen sorunlar

<!-- TBD - IS ASDK --> 
- İçinde genel IP adresi kullanım ölçüm verileri gösterilen aynı bir sorun düzeltildi **olay tarihi-saati** yerine her kaydın değerini **TimeDate** gösteren kaydın oluşturulduğu zaman damgası. Bu veriler artık ortak IP adresi kullanımının doğru hesap gerçekleştirmek için de kullanabilirsiniz.

<!-- 3099544 – IS, ASDK --> 
- Azure Stack portalını kullanarak bir yeni sanal makine (VM) oluştururken gerçekleşen bir sorun düzeltildi. VM boyutunu seçme görüntülenecek ABD Doları/ay sütunu nedeniyle bir **kullanılamıyor** ileti. Bu sütunu artık görünür; VM görüntüleme fiyatlandırma sütunu Azure Stack'te desteklenmiyor.

<!-- 2930718 - IS ASDK --> 
- İçinde bir sorun düzeltildi Yönetici portalını herhangi bir kullanıcı aboneliği ayrıntıları tıklandığında ve dikey pencereyi kapatmadan sonra erişirken **son**, kullanıcı abonelik adı görünmez. Kullanıcı abonelik adı görünür.

<!-- 3060156 - IS ASDK --> 
- İçinde yönetici ve kullanıcı portalı bir sorun düzeltildi: portalı ayarlarını tıklayıp seçerek **tüm ayarları ve özel panoları Sil** beklendiği gibi çalışmaması ve bir hata bildirimi zobrazilo. 

<!-- 2930799 - IS ASDK --> 
- İçinde yönetici ve kullanıcı portalı bir sorun düzeltildi: altında **tüm hizmetleri**, varlık **DDoS koruma planları** yanlış, kullanılamaz durumdayken Azure Stack'te listelenmişti.
 
<!--2760466 – IS  ASDK --> 
- Yeni bir Azure Stack ortamı yüklü olduğunda gerçekleşen bir sorun düzeltildi gösteren uyarı **etkinleştirme gerekli** görüntüleme. Artık düzgün bir şekilde görüntüler.

<!--1236441 – IS  ASDK --> 
- AD FS kullanılırken bir kullanıcı grubuna RBAC ilkelerini uygulama önleyen bir sorun düzeltildi.

<!--3463840 - IS, ASDK --> 
- Altyapı yedeklemeleri erişilemez dosya sunucusundan genel VIP ağları nedeniyle başarısız olan ilgili sorun düzeltildi. Bu düzeltmenin hizmet altyapı yedekleme genel altyapı ağına geri taşınır. Bu sorunu ele en son Azure Stack için düzeltme 1809 uyguladıysanız 1811 güncelleştirme herhangi bir değişiklik yapmaz. 

<!-- 2967387 – IS, ASDK --> 
- Azure Stack yönetici veya kullanıcı portalı oturum açmak için kullandığınız hesabı görüntülendiği olarak bir sorun düzeltildi **tanımlanmayan kullanıcı**. Hesap ya da sahip olduğunda bu ileti görüntülendi bir *ilk* veya *son* adı belirtilmedi.   

<!--  2873083 - IS ASDK --> 
- Hangi portalı bir sanal makine oluşturmak için kullanarak ölçek kümesi (VMSS), bir sorun düzeltildi *örnek boyutu* açılan doğru şekilde yüklenmedi Internet Explorer kullanırken. Bu tarayıcıyı artık düzgün şekilde çalışır.  

<!-- 3190553 - IS ASDK -->
- Bir altyapı rol örneği kullanılamıyor veya ölçek birimi düğümü çevrimdışı gösteren gürültülü uyarıları oluşturan bir sorun düzeltildi.

### <a name="known-issues"></a>Bilinen sorunlar

#### <a name="portal"></a>Portal  

<!-- 2930820 - IS ASDK --> 
- "Docker için" araması yaparsanız yönetici ve kullanıcı portalı yanlış öğesi döndürülür. Azure Stack'te kullanılamıyor. Bunu oluşturmayı denerseniz, bir hata göstergesi içeren bir dikey pencere görüntülenir. 

<!-- 2931230 – IS  ASDK --> 
- Kullanıcı aboneliği plan kaldırdığınızda bile, bir kullanıcı abonelikte eklenti planı eklendiği planları silinemiyor. Eklenti planı başvuru abonelikleri de silinene kadar plan kalır. 

<!-- TBD - IS ASDK --> 
- 1804 sürümü ile sunulan iki Yönetim abonelik türlerini kullanılmamalıdır. Abonelik türleridir **abonelik ölçümü**, ve **tüketim abonelik**. Bu abonelik türlerini 1804 sürümünden başlayarak yeni Azure Stack ortamlarında görülebilir ancak henüz kullanıma sunulmamıştır. Kullanmaya devam etmelidir **varsayılan sağlayıcı** abonelik türü.

<!-- TBD - IS ASDK --> 
- Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

<!-- TBD - IS ASDK --> 
- Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.

#### <a name="compute"></a>İşlem 

<!-- 3235634 – IS, ASDK -->
- Vm'leri içeren boyutları ile dağıtmak için bir **v2** soneki; Örneğin, **işler için standart_a2_v2**, sonek olarak lütfen **işler için standart_a2_v2** (küçük harf v). Kullanmayın **işler için standart_a2_v2** (Büyük Harf V). Bu genel Azure'da çalışır ve Azure Stack'te bir tutarsızlık olduğunu.

<!-- 2869209 – IS, ASDK --> 
- Kullanırken [ **Ekle AzsPlatformImage** cmdlet'i](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), kullanmalısınız **- OsUri** parametre olarak depolama hesabı URI'si disk nereye yüklenir. Yerel yol diskin kullanırsanız, cmdlet şu hatayla başarısız olur: *İşlemi uzun süre çalışan 'Başarısız' durumuyla başarısız oldu*. 

<!--  2795678 – IS, ASDK --> 
- VM, portalı sanal makineler (VM) oluşturmak için bir premium VM boyutu (DS, Ds_v2, FS, FSv2) kullandığınızda, bir standart depolama hesabı oluşturulur. Bir standart depolama hesabı oluşturma IOPS, işlevsel olarak, etkilemez ya da fatura. Bildiren bir uyarıyı güvenle yok sayabilirsiniz: **Premium diskleri destekleyen bir boyutta standart disk kullanmayı seçtiniz. Bu işletim sisteminin performansını etkileyebilir ve önerilmez. Premium depolamayı (SSD) kullanmayı düşünün.**

<!-- 2967447 - IS, ASDK --> 
- Dağıtım için bir seçenek olarak, 7.2 CentOS tabanlı sanal makine ölçek kümesi (VMSS) oluşturma deneyimi sağlar. Bu görüntüyü Azure Stack üzerinde kullanılabilir olmadığından, dağıtımınız için başka bir işletim sistemi seçin veya Market'ten dağıtımdan işleciyle indirildi başka bir CentOS görüntüsü belirten bir Azure Resource Manager şablonu kullanın.  

<!-- TBD - IS ASDK --> 
- VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

<!-- 1662991 IS ASDK --> 
- Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur.  

<!-- 2724961- IS ASDK --> 
- Kaydettiğinizde **Microsoft.Insight** abonelik ayarlarını, kaynak sağlayıcısındaki bir Windows VM oluşturma ve konuk işletim sistemi etkin Tanılama ile CPU yüzdesi grafiğin VM genel bakış sayfasında ölçüm verilerini da göstermez.

   Sanal makine için CPU yüzdesi grafik gibi ölçüm verileri bulmak için ölçümleri penceresine gidin ve tüm desteklenen Windows VM Konuk ölçümleri göster.

<!-- 3507629 - IS, ASDK --> 
- Yönetilen diskler oluşturur iki yeni [kota türleri işlem](../azure-stack-quota-types.md#compute-quota-types) sağlanabilir yönetilen diskler hizmetin maksimum kapasitesi sınırlamak için. Varsayılan olarak, 2048 GiB her yönetilen diskler kota türü için ayrılır. Ancak, aşağıdaki sorunlarla karşılaşabilirsiniz:

   - 2048 GiB ayrılır ancak 1808 güncelleştirmeden önce oluşturulan kotalarını 0 değerleri yönetilen diskler kotası yönetici Portalı'nda gösterir. Gerçek gereksinimlerinize ve yeni belirlenen göre değerini azaltın veya artırabilirsiniz kota değeri, 2048 GiB varsayılan'ı geçersiz kılar.
   - Kota değeri 0'a güncelleştirin, 2048 GiB varsayılan değerini eşdeğer olacaktır. Geçici çözüm olarak, kota değeri 1 olarak ayarlayın.

<!-- TBD - IS ASDK --> 
- Güncelleştirme 1811 uyguladıktan sonra yönetilen disklere sahip VM'ler dağıtırken aşağıdaki sorunlarla karşılaşabilirsiniz:

   1. Yönetilen disklerle bir VM dağıtma 1808 güncelleştirmeden önce Abonelik oluşturulurken bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için her abonelik için şu adımları izleyin:
      1. Kiracı Portalı'nda Git **abonelikleri** ve aboneliği bulunamıyor. Tıklayın **kaynak sağlayıcıları**, ardından **Microsoft.Compute**ve ardından **yeniden kaydettirin**.
      2. Aynı abonelik altında Git **erişim denetimi (IAM)**, doğrulayın **Azure Stack – yönetilen Disk** listelenir.
   2. Bir konuk dizin ile ilişkili bir abonelik içindeki Vm'leri dağıtma, çok kiracılı bir ortam yapılandırdıysanız, bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için aşağıdaki adımları izleyin. [bu makalede](../azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) her Konuk dizinlerinizi yeniden yapılandırmak için.

#### <a name="networking"></a>Ağ

<!-- 1766332 - IS ASDK --> 
- Altında **ağ**, tıklarsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **ilkesine** bir VPN türü listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure Stack'te desteklenir.

<!-- 1902460 - IS ASDK --> 
- Azure Stack destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu tüm Kiracı abonelikler arasında geçerlidir. Aynı IP adresine sahip bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı, sonraki oluşturulmasını denemeden sonra engellenir.

<!-- 16309153 - IS ASDK --> 
- DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu vnet'teki VM'ler için güncelleştirilmiş ayarları gönderiliyor değil.

<!-- 2529607 - IS ASDK --> 
- Azure Stack sırasında *gizli dönüş*, bir süre içinde genel IP adresleri olan erişilemeyen iki ila beş dakikalığına yoktur.

<!-- 2664148 - IS ASDK --> 
-   Bir S2S VPN tüneli aracılığıyla Kiracı sanal makinelerini burada erişiyor senaryolarda, yerel ağ geçidi için şirket içi alt ağ geçidi zaten oluşturulduktan sonra eklenmişse nerede bağlantı girişimleri başarısız bir senaryo hatalarla karşılaşabilirsiniz. 

#### <a name="app-service"></a>App Service

<!-- 2352906 - IS ASDK --> 
- Kullanıcılar, bunlar, ilk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

<!-- #### Usage -->  

<!-- #### Identity -->

## <a name="build-11809090"></a>Derleme 1.1809.0.90

### <a name="new-features"></a>Yeni Özellikler
Bu derleme, Azure Stack için aşağıdaki geliştirmeleri ve düzeltmeleri içerir.  

<!--  2712869   | IS  ASDK -->  
- **Azure Stack syslog istemcisi (Genel kullanım)** bu istemci denetimler, uyarılar ve Azure Stack altyapısının bir syslog sunucusu veya güvenlik bilgileri ve Olay yönetimi (SIEM) yazılım ilgili güvenlik günlükleri iletilmesini sağlar. Azure Stack için dış. Syslog istemci artık syslog sunucunun dinlediği bağlantı noktasını destekler.

Bu sürümle birlikte, syslog istemci genel olarak kullanılabilir ve üretim ortamlarında kullanılabilir.

Daha fazla bilgi için [Azure Stack syslog iletmeyi](../azure-stack-integrate-security.md).

### <a name="fixed-issues"></a>Düzeltilen sorunlar

<!-- TBD - IS ASDK -->
- Portalda, boş ve kullanılan kapasiteyi raporlama bellek grafik artık doğru olur. Artık daha güvenilir bir şekilde oluşturmak için kaç tane Vm'niz tahmin edebilirsiniz.

<!-- TBD - IS ASDK --> 
- Sanal makineler Azure Stack kullanıcı portalında oluşturulan ve DS serisi VM ekleyebilirsiniz veri diskleri yanlış sayıda portalı görüntülenen bir sorun düzeltildi. DS serisi VM'ler gibi çok sayıda veri diski Azure yapılandırması sağlayabilir.

- <!-- 2702741 -  IS, ASDK --> Dinamik Ayırma kullanılarak dağıtılan hangi genel IP'ler, yöntemi değildi neden olan sorun düzeltildi durdurun-serbest verildiği sonra korunması garanti. Bunlar artık korunur.

- <!-- 3078022 - IS, ASDK --> Bir VM 1808 önce durdu-serbest olduysa, 1808 güncelleştirmeden sonra yeniden tahsis bulunamadı.  Bu sorun 1809 içinde düzeltilmiştir. Bu düzeltmeyle 1809 içinde başlatılamadı ve bu durumda örnek başlatılabilir. Düzeltme aynı zamanda bu sorunu yeniden oluşmasını engeller.

<!-- TBD -  IS ASDK --> 
- Bir VM görüntüsü oluşturulması başarısız olduğunda, nelze odstranit başarısız bir öğe için VM görüntüleri işlem dikey eklenebilir.

  Geçici çözüm olarak, Hyper-V ile oluşturduğunuz işlevsiz bir VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yolu C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silme engelleyen sorunu çözer. Ardından, işlevsiz bir görüntü oluşturduktan sonra 15 dakika başarıyla silebilirsiniz.

  Ardından, daha önce başarısız olan VM görüntüsünün indirilmesi yeniden deneyebilir.

- **Çeşitli düzeltmeleri** performans, kararlılık, güvenlik ve Azure Stack tarafından kullanılan işletim sistemi

### <a name="changes"></a>Değişiklikler

### <a name="known-issues"></a>Bilinen sorunlar

#### <a name="portal"></a>Portal  

<!-- 2515955   | IS ,ASDK--> 
- *Tüm hizmetler* değiştirir *diğer hizmetler* Azure Stack yönetici ve Kullanıcı Portalı'nda. Artık *tüm hizmetleri* Azure portallarında yaptığınız gibi Azure Stack portallarında gezinmek için alternatif olarak.

<!--  TBD – IS, ASDK --> 
- *Temel A* sanal makine boyutları için devre dışı [sanal makine ölçek kümeleri oluşturma](../azure-stack-compute-add-scalesets.md) (VMSS) portal üzerinden. Bu boyut ile bir VMSS oluşturmak için PowerShell ya da bir şablon kullanın.

#### <a name="health-and-monitoring"></a>Sistem durumu ve izleme

<!-- 1264761 - IS ASDK -->  
- Uyarıları görebilirsiniz *sistem durumu denetleyicisi* aşağıdaki ayrıntıları olan bir bileşeni:  

   Uyarı #1:
   - ADI:  Sağlıksız altyapı rolü
   - ÖNEM DERECESİ: Uyarı
   - BİLEŞEN: Denetleyici sistem durumu
   - AÇIKLAMA: Sistem durumu denetleyici sinyal tarayıcı kullanılamıyor. Bu sistem durumu raporlarının ve ölçümler etkileyebilir.  

  Uyarı #2:
   - ADI:  Sağlıksız altyapı rolü
   - ÖNEM DERECESİ: Uyarı
   - BİLEŞEN: Denetleyici sistem durumu
   - AÇIKLAMA: Sistem durumu denetleyicisi hata tarayıcı kullanılamıyor. Bu sistem durumu raporlarının ve ölçümler etkileyebilir.

  Her iki uyarılar güvenle yoksayılabilir ve zaman içinde otomatik olarak kapatılacak.  

<!-- 2368581 - IS. ASDK --> 
- Düşük bellek uyarısı alırsanız ve Kiracı sanal makinelerini başarısız ile dağıtmak bir Azure Stack operatörü bir *Fabric VM oluşturma hatası*, Azure Stack damga kullanılabilir bellek yetersiz olabilir. Kullanım [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) iş yükleriniz için kapasite en iyi anlamak için.


#### <a name="compute"></a>İşlem 

<!-- 3164607 – IS, ASDK -->
- Aynı ada ve LUN ile aynı sanal makine (VM) için ayrılmış bir diski yeniden bağlanması başarısız bir hata ile gibi **veri diski 'datadisk', 'vm1' VM'sine iliştirilemiyor**. Disk şu anda ayrılmakta veya son ayırma işlemi başarısız oldu hata oluşur. Lütfen disk tamamen ayrılmış kadar bekleyin ve sonra yeniden deneyin veya silme/diski açıkça tekrar ayırın. Geçici çözüm, farklı bir adla veya farklı bir LUN üzerinde yeniden sağlamaktır. 

<!-- 3235634 – IS, ASDK -->
- Vm'leri içeren boyutları ile dağıtmak için bir **v2** soneki; Örneğin, **işler için standart_a2_v2**, sonek olarak lütfen **işler için standart_a2_v2** (küçük harf v). Kullanmayın **işler için standart_a2_v2** (Büyük Harf V). Bu genel Azure'da çalışır ve Azure Stack'te bir tutarsızlık olduğunu.

<!-- 3099544 – IS, ASDK --> 
- Azure Stack portalını kullanarak bir yeni sanal makine (VM) oluşturun ve VM boyutu seçin, ABD Doları/ay sütun içeren görüntülenir bir **kullanılamıyor** ileti. Bu sütun görünmemelidir; VM görüntüleme fiyatlandırma sütunu Azure Stack'te desteklenmiyor.

<!-- 2869209 – IS, ASDK --> 
- Kullanırken [ **Ekle AzsPlatformImage** cmdlet'i](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), kullanmalısınız **- OsUri** parametre olarak depolama hesabı URI'si disk nereye yüklenir. Yerel yol diskin kullanırsanız, cmdlet şu hatayla başarısız olur: *İşlemi uzun süre çalışan 'Başarısız' durumuyla başarısız oldu*. 

<!--  2966665 – IS, ASDK --> 
- SSD ekleme yönetilen disk sanal makineler (DS, DSv2, Fs, Fs_V2) bir hatayla başarısız oluyor premium boyutuna veri diskleri:  *'Vmname' hata sanal makinenin diskleri güncelleştirilemedi: İstenen işlem gerçekleştirilemiyor, depolama hesabı türü 'Premium_LRS' VM boyutu için desteklenmediğinden ' Standard_DS/Ds_V2/FS/Fs_v2)*

   Bu sorunu geçici olarak çözmek için kullanın *Standard_LRS* veri diskleri yerine *Premium_LRS diskleri*. Kullanım *Standard_LRS* veri diskleri IOPS veya fatura ücreti değiştirmez.  

<!--  2795678 – IS, ASDK --> 
- VM, portalı sanal makineler (VM) oluşturmak için bir premium VM boyutu (DS, Ds_v2, FS, FSv2) kullandığınızda, bir standart depolama hesabı oluşturulur. Bir standart depolama hesabı oluşturma IOPS, işlevsel olarak, etkilemez ya da fatura. 

   Bildiren bir uyarıyı güvenle yok sayabilirsiniz: *Premium diskleri destekleyen bir boyutta standart disk kullanmayı seçtiniz. Bu işletim sisteminin performansını etkileyebilir ve önerilmez. Premium depolamayı (SSD) kullanmayı düşünün.*

<!-- 2967447 - IS, ASDK --> 
- Sanal makine ölçek kümesi (VMSS) deneyimi oluşturmasına 7.2 CentOS tabanlı dağıtım için bir seçenek olarak sağlar. Bu görüntüyü Azure Stack üzerinde kullanılabilir olmadığından, dağıtımınız için başka bir işletim sistemi seçin veya Market'ten dağıtımdan işleciyle indirildi başka bir CentOS görüntüsü belirten bir Azure Resource Manager şablonu kullanın.

<!-- TBD -  IS ASDK --> 
- Ölçeklendirme ayarları sanal makine ölçek kümeleri için portalda kullanılabilir değildir. Geçici çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürüm farklar nedeniyle, kullanmanız gereken `-Name` yerine parametre `-VMScaleSetName`.



<!-- TBD -  IS ASDK --> 
- VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

<!-- 1662991 - IS ASDK --> 
- Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur.

<!-- 2724961- IS ASDK --> 
- Kaydettiğinizde **Microsoft.Insight** abonelik ayarlarında, kaynak sağlayıcısı ve bir Windows VM konuk işletim sistemi etkin Tanılama ile oluşturma, VM genel bakış sayfasında CPU yüzdesi grafik, ölçüm verilerini göstermek mümkün olmayacaktır.
 
  Sanal makine için CPU yüzdesi grafik bulmak için Git **ölçümleri** dikey penceresinde ve desteklenen tüm Windows VM show Konuk ölçümleri.

 

#### <a name="networking"></a>Ağ
<!-- 1766332 - IS, ASDK --> 
- Altında **ağ**, tıklarsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **ilkesine** bir VPN türü listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure Stack'te desteklenir.

<!-- 1902460 -  IS ASDK --> 
- Azure Stack destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu tüm Kiracı abonelikler arasında geçerlidir. Aynı IP adresine sahip bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı, sonraki oluşturulmasını denemeden sonra engellenir.

<!-- 16309153 -  IS ASDK --> 
- DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu vnet'teki VM'ler için güncelleştirilmiş ayarları gönderiliyor değil.

<!-- 2702741 -  IS ASDK --> 
- Dinamik ayırma yöntemi kullanılarak dağıtılan genel IP'ler garanti edilmez durdurun-serbest verildiği sonra korunması.

<!-- 2529607 - IS ASDK --> 
- Azure Stack sırasında *gizli dönüş*, bir süre içinde genel IP adresleri olan erişilemeyen iki ila beş dakikalığına yoktur.

<!-- 2664148 - IS ASDK --> 
- Bir S2S VPN tüneli aracılığıyla Kiracı sanal makinelerini burada erişiyor senaryolarda, yerel ağ geçidi için şirket içi alt ağ geçidi zaten oluşturulduktan sonra eklenmişse nerede bağlantı girişimleri başarısız bir senaryo hatalarla karşılaşabilirsiniz. 


<!--  #### SQL and MySQL  -->


#### <a name="app-service"></a>App Service
<!-- 2352906 - IS ASDK --> 
- Kullanıcılar, bunlar, ilk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

<!-- TBD - IS ASDK --> 
- (Çalışan, yönetim, ön uç rollerini) altyapıyı ölçeklendirme için sürüm notları için işlem açıklandığı PowerShell kullanmanız gerekir.  


#### <a name="usage"></a>Kullanım  
<!-- TBD -  IS ASDK --> 
- Kullanım genel IP adresi kullanım ölçümü verilerini gösterir aynı *olay tarihi-saati* yerine her kaydın değerini *TimeDate* gösteren kaydın oluşturulduğu zaman damgası. Şu anda, genel IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.

<!-- #### Identity -->

## <a name="build-11808097"></a>Derleme 1.1808.0.97

### <a name="new-features"></a>Yeni Özellikler
Bu derleme, Azure Stack için aşağıdaki geliştirmeleri ve düzeltmeleri içerir.  

<!-- 1658937 | ASDK, IS --> 
- **Önceden tanımlanmış bir zamanlamaya göre yedeklemeleri başlatmak** -gereçlerden biri Azure Stack artık otomatik olarak düzenli aralıklarla insan müdahalesi ortadan kaldırmak için altyapı yedekleme tetikleyebilirsiniz. Azure Stack tanımlanan saklama süresinden daha eski yedeklemeler için dış paylaşımı da otomatik olarak temizler. Daha fazla bilgi için [PowerShell ile Azure Stack için yedeklemeyi etkinleştir](../azure-stack-backup-enable-backup-powershell.md).

<!-- 2496385 | ASDK, IS -->  
- **Eklenen veri aktarımı için toplam yedek zamanına zaman.** Daha fazla bilgi için [PowerShell ile Azure Stack için yedeklemeyi etkinleştir](../azure-stack-backup-enable-backup-powershell.md).

<!-- 1702130 | ASDK, IS --> 
- **Yedekleme dış kapasite artık dış paylaşım doğru kapasitesini gösterir.** (Daha önce bu kodu sabit-10 GB.) Daha fazla bilgi için [PowerShell ile Azure Stack için yedeklemeyi etkinleştir](../azure-stack-backup-enable-backup-powershell.md).
 
<!-- 2753130 |  IS, ASDK   -->  
- **Azure Resource Manager şablonları artık destek koşulu öğesi** -artık bir kaynak bir koşulunu kullanarak bir Azure Kaynak Yöneticisi şablonu olarak dağıtabilirsiniz. Bir parametre değeri varsa, değerlendirme gibi bir koşula bağlı olarak kaynak dağıtma için şablonunuzu tasarlayabilirsiniz. Bir koşul olarak bir şablon kullanma hakkında daha fazla bilgi için bkz: [koşullu olarak kaynak dağıtma](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/conditional-deploy) ve [değişkenler bölümü Azure Resource Manager şablonlarının](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-templates-variables) Azure belgelerinde. 

   Şablonlar için de kullanabilirsiniz [birden fazla abonelik veya kaynak grubu için kaynakları dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-cross-resource-group-deployment).  

<!--2753073 | IS, ASDK -->  
- **Microsoft.Network API kaynak sürüm desteği güncelleştirildi** API Sürüm 2017-10-01 2015-06-15 arasında Azure Stack ağ kaynakları için desteği eklenecek. Bu sürümde 2017-10-01 2015-06-15 arasındaki kaynak sürümleri desteği dahil edilmez. Lütfen [Azure Stack ağ iletişimi için Değerlendirmeler](../user/azure-stack-network-differences.md) işlev farklılıkları için.

<!-- 2272116 | IS, ASDK   -->  
- **Azure Stack, harici olarak Azure Stack altyapı uç noktalarına yönelik için ters DNS araması için destek ekledi** (değil portal, adminportal, yönetim ve adminmanagement için). Bu, bir IP adresinden çözülmesi Azure Stack dış uç nokta adları sağlar.

<!-- 2780899 |  IS, ASDK   --> 
- **Azure Stack artık mevcut bir VM'ye ek ağ arabirimi eklenmesine izin verir.**  Bu işlev, portal, PowerShell ve CLI kullanarak kullanılabilir. Daha fazla bilgi için [ekleme veya kaldırma ağ arabirimleri](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-vm) Azure belgeleri. 

<!-- 2222444 | IS, ASDK   -->  
- **Kullanım ölçümleri ağ doğruluk ve dayanıklılık geliştirmeleri yapıldı**. Ağ kullanım ölçümleri, artık daha doğru ve hesabı askıya alınmış Abonelikleri, kesinti süreleri ve yarış durumları alın.

<!-- 2297790 | IS, ASDK -->  
- **Azure Stack syslog istemciye (Önizleme özelliği) geliştirmeleri**. Bu istemci, Denetim ve Azure Stack altyapısının bir syslog sunucusu veya güvenlik bilgileri ve Olay yönetimi (SIEM) yazılım Azure Stack'e dış ilgili günlükleri iletilmesini sağlar. Syslog istemci, artık düz metin veya varsayılan yapılandırmayı ikinci olan, TLS 1.2 şifrelemeyi TCP protokolünü destekler. TLS bağlantısını yalnızca sunucu ya da karşılıklı kimlik doğrulaması ile yapılandırabilirsiniz.

  Syslog istemci (protokol, şifreleme ve kimlik doğrulaması gibi) nasıl iletişim kurduğu syslog sunucusu ile yapılandırmak için Set-SyslogServer cmdlet'ini kullanın. Bu cmdlet, ayrıcalıklı uç noktasından (CESARETLENDİRİCİ) kullanılabilir. 

- <!-- ASDK --> **Sanal makine ölçek kümeleri için galeri öğeleri yerleşik artık**.  Sanal makine ölçek kümesi galeri öğeleri artık bunları yüklemeye gerek kalmadan kullanıcı ve Yönetici portalı içinde kullanılabilir hale getirilir. 

- <!-- IS, ASDK --> **Sanal makine ölçek kümesi ölçeklendirme**.  Portala kullanabileceğiniz [bir sanal makine ölçek kümesi ölçeği](../azure-stack-compute-add-scalesets.md#scale-a-virtual-machine-scale-set) (VMSS).   

- <!-- 2489570 | IS ASDK--> **Özel IPSec/IKE İlkesi yapılandırmalarını desteği** için [Azure Stack hizmetinde VPN ağ geçitlerinin](/azure/azure-stack/azure-stack-vpn-gateway-about-vpn-gateways).

- <!-- | IS ASDK--> **Kubernetes Market öğesi**. Şimdi kullanarak Kubernetes kümelerini dağıtmayı [Kubernetes Market öğesi](/azure/azure-stack/azure-stack-solution-template-kubernetes-cluster-add). Kullanıcılar, Kubernetes öğeyi seçin ve Azure Stack'e bir Kubernetes kümesi dağıtmak için bazı parametreleri doldurun. Şablonları amacı, kullanıcıların Kurulum geliştirme/test Kubernetes dağıtımları birkaç adımda basit olmasını sağlamaktır.

<!-- ####### | IS, ASDK -->  
- **Azure Resource Manager, bölge adını içerir.** Bu sürümle birlikte, Azure Resource Manager'dan alınan nesneler artık bölge adı özniteliği içerir. Varolan bir PowerShell komut dosyasını doğrudan nesne başka bir cmdlet'e geçerse, betik hataya neden ve başarısız. Bu, Azure Resource Manager uyumlu davranıştır ve bölge öznitelik çıkarılacak çağıran istemci gerektirir. Azure Resource Manager bakın hakkında daha fazla bilgi için [Azure Resource Manager belgeleri](https://docs.microsoft.com/azure/azure-resource-manager/).

<!-- TBD | IS, ASDK -->  
- **Abonelikler, temsilci sağlayıcılar arasında taşıyın.** Aynı dizin kiracısına ait yeni veya var olan bir temsilci sağlayıcısı abonelikler arasında abonelikler artık taşıyabilirsiniz. Varsayılan sağlayıcı aboneliğine ait abonelikleri de aynı dizin kiracısında temsilci sağlayıcı aboneliği için taşınabilir. Daha fazla bilgi için [temsilci sunan Azure Stack'te](../azure-stack-delegated-provider.md).
 
<!-- 2536808 IS ASDK --> 
- **VM oluşturma zamanı geliştirilmiş** görüntüleri Azure Market'te indir ile oluşturulan sanal makineleri için.

### <a name="fixed-issues"></a>Düzeltilen sorunlar
- <!-- IS ASDK--> Bir kullanılabilirlik kümesi Portal'da bulunan ve bir hata etki alanı ve 1 güncelleştirme etki alanına sahip kümesinde sonuçlandı oluşturmaya yönelik bir sorunu düzelttik.

<!-- TBD | ASDK, IS --> 
- Çeşitli iyileştirmeler, daha güvenilir hale getirmek için güncelleştirme işlemi için yapılmıştır. Ayrıca, düğüm boşaltma, böylece güncelleştirme sırasında iş yükleri için olası kapalı kalma süresini en aza artıran altyapının düzeltmeler yapılmıştır.

<!--2292271 | ASDK, IS --> 
- Burada değiştirilmiş bir kota sınırı mevcut aboneliklerin uygulamadı sorununu düzelttik.  Artık, bir teklif parçası olan bir ağ kaynağı için bir kota sınırı artırmak ve kullanıcı abonelikle planı, önceden mevcut abonelikler, aynı zamanda için yeni abonelikler yeni sınır geçerlidir.

<!-- 2448955 | IS ASDK --> 
- Etkinlik günlükleri bir UTC + N saat diliminde dağıtılan sistemler için artık başarılı bir şekilde sorgulayabilirsiniz.    

<!-- 2319627 |  ASDK, IS --> 
- Yedekleme yapılandırması parametreleri (yol/kullanıcı adı/parola/şifreleme anahtarı) için ön denetim, artık için yedekleme yapılandırması ayarları yanlış ayarlar. (Daha önce yedeklemeyi ayarları yanlış ayarlanmış ve yedekleme ardından tetiklendiğinde başarısız olur.)

<!-- 2215948 |  ASDK, IS --> 
- Dış paylaşımından el ile yedekleme sildiğinizde yedekleme listesinde artık yeniler.

<!-- 2360715 |  ASDK, IS -->  
- Bundan sonra veri merkezi tümleştirmeyi ayarladığınızda, AD FS meta veri dosyası bir paylaşımdan erişemezsiniz. Daha fazla bilgi için [Federasyon meta veri dosyası sağlayarak AD FS tümleştirmenin ayarlanması](../azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

<!-- 2388980 | ASDK, IS --> 
- Bir sorunu düzelttik döndürülmesiyle kullanıcılardan var olan bir genel IP adresi atanmış, daha önce bir ağ arabirimiyle ya da yeni bir ağ arabirimiyle ya da yük dengeleyici için yük dengeleyici için atanmış.  

<!-- 2551834 - IS, ASDK --> 
- Seçtiğinizde, *genel bakış* Yöneticisi ya da Kullanıcı Portalı'nda depolama hesabı için *Essentials* bölmesi artık görüntüler beklenen tüm bilgilerin doğru. 

<!-- 2551834 - IS, ASDK --> 
- Seçtiğinizde, *etiketleri* için bir depolama hesabı yönetici veya Kullanıcı Portalı'nda, bilgileri artık doğru görüntüler.

<!-- TBD - IS ASDK --> 
- Azure Stack bu sürümü, sürücü güncelleştirmelerini uygulamadan OEM uzantı paketleri önleyen sorunu giderir.

<!-- 2055809- IS ASDK --> 
- VM'nin oluşturulması başarısız oldu. işlem dikey penceresinden Vm'leri silmesi önleyen bir sorunu düzelttik.  

<!--  2643962 IS ASDK -->  
- Uyarı *düşük bellek kapasitesi* artık yanlış görüntülenir.

<!--  TBD ASDK --> 
- Ayrıcalıklı uç noktasını (CESARETLENDİRİCİ) barındıran sanal makine için 4 GB arttı. ASDK bu sanal makine AzS-ERCS01 olarak adlandırılır.

- <!--  TBD – IS, ASDK --> *Temel A* sanal makine boyutları için devre dışı [sanal makine ölçek kümeleri oluşturma](../azure-stack-compute-add-scalesets.md) (VMSS) portal üzerinden. Bu boyut ile bir VMSS oluşturmak için PowerShell ya da bir şablon kullanın. 

### <a name="known-issues"></a>Bilinen sorunlar

#### <a name="portal"></a>Portal  
- <!-- 2967387 – IS, ASDK --> Azure Stack yönetici veya kullanıcı portalı oturum açmak için kullandığınız hesap görüntüler olarak **tanımlanmayan kullanıcı**. Hesap ya da sahip olmadığında bu oluşur bir *ilk* veya *son* adı belirtilmedi. Bu sorunu geçici olarak çözmek için ilk veya son sağlamak için kullanıcı hesabı düzenleyin. Oturumu kapatın ve sonra portalda yeniden oturum gerekir. 

-  <!--  2873083 - IS ASDK --> Bir sanal makine ölçek oluşturmak için portalı kullandığınızda ayarlayın (VMSS) *örnek boyutu* açılan olmayan yük doğru bir şekilde Internet Explorer kullandığınızda. Bu sorunu çözmek için bir VMSS oluşturmak için portalı kullanırken başka bir tarayıcı kullanın.  

#### <a name="portal"></a>Portal  
<!-- 2931230 – IS  ASDK --> 
- Kullanıcı aboneliği plan kaldırdığınızda bile, bir kullanıcı abonelikte eklenti planı eklendiği planları silinemiyor. Eklenti planı başvuru abonelikleri de silinene kadar plan kalır. 

<!--2760466 – IS  ASDK --> 
- Bu sürümünü çalıştıran yeni bir Azure Stack ortamına yüklediğinizde, uyarıyı gösterir *etkinleştirme gerekli* görüntülenmeyebilir. [Etkinleştirme](../azure-stack-registration.md) Market dağıtım kullanabilmeniz için gereklidir. 

<!-- TBD - IS ASDK --> 
- 1804 sürümü ile sunulan iki Yönetim abonelik türlerini kullanılmamalıdır. Abonelik türleridir **abonelik ölçümü**, ve **tüketim abonelik**. Bu abonelik türleri **abonelik ölçümü**, ve **tüketim abonelik**. Bu abonelik türlerini 1804 sürümünden başlayarak yeni Azure Stack ortamlarında görülebilir ancak henüz kullanıma sunulmamıştır. Kullanmaya devam etmelidir **varsayılan sağlayıcı aboneliği** türü.

<!-- 2403291 - IS ASDK --> 
- Yatay kaydırma çubuğunun alt kısmında bulunan yönetici ve Kullanıcı Portalı'nın kullanımını olmayabilir. Yatay kaydırma çubuğunun erişemiyorsanız, önceki bir dikey pencere portaldaki dikey penceresinin adı seçerek, gitmek için içerik haritaları en üstünde bulunan içerik haritası listeden görüntülemek istediğiniz kullanım portalın sol.
  ![İçerik haritası](media/asdk-release-notes/breadcrumb.png)

<!-- TBD -  IS ASDK --> 
- Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

<!-- TBD -  IS ASDK --> 
- Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.

<!--  TBD | ASDK -->  
- Azure Stack dağıtımınıza için varsayılan saat dilimini artık UTC'ye ayarlar. Ancak, bunu otomatik olarak UTC'ye varsayılan olarak yükleme sırasında dönecek Azure Stack, yükleme sırasında bir saat dilimi seçebilirsiniz.

#### <a name="health-and-monitoring"></a>Sistem durumu ve izleme
<!-- 1264761 - IS ASDK -->  
- Uyarıları görebilirsiniz *sistem durumu denetleyicisi* aşağıdaki ayrıntıları olan bir bileşeni:  

   Uyarı #1:
   - ADI:  Sağlıksız altyapı rolü
   - ÖNEM DERECESİ: Uyarı
   - BİLEŞEN: Denetleyici sistem durumu
   - AÇIKLAMA: Sistem durumu denetleyici sinyal tarayıcı kullanılamıyor. Bu sistem durumu raporlarının ve ölçümler etkileyebilir.  

  Uyarı #2:
   - ADI:  Sağlıksız altyapı rolü
   - ÖNEM DERECESİ: Uyarı
   - BİLEŞEN: Denetleyici sistem durumu
   - AÇIKLAMA: Sistem durumu denetleyicisi hata tarayıcı kullanılamıyor. Bu sistem durumu raporlarının ve ölçümler etkileyebilir.

  Her iki uyarılar güvenle yoksayılabilir ve zaman içinde otomatik olarak kapatılacak.  

<!-- 2368581 - IS. ASDK --> 
- Düşük bellek uyarısı alırsanız ve Kiracı sanal makinelerini başarısız ile dağıtmak bir Azure Stack operatörü bir *Fabric VM oluşturma hatası*, Azure Stack damga kullanılabilir bellek yetersiz olabilir. Kullanım [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) iş yükleriniz için kapasite en iyi anlamak için.

<!-- TBD - IS. ASDK --> 
- Çalıştırırken **Test AzureStack** ayrıcalıklı uç noktası (CESARETLENDİRİCİ) cmdlet'ini **Azure Stack altyapısını rol örneği performans** test ERCS VM için bir uyarı iletisi üretir. Güvenli bir şekilde UYAR iletiyi yoksayıp ASDK kullanmaya devam edin.

#### <a name="compute"></a>İşlem
<!-- 2494144 - IS, ASDK --> 
- Bir sanal makine dağıtımı için bir sanal makine boyutu seçerken, bazı F serisi VM boyutlarının görünür olmayan bir VM oluşturduğunuzda boyut seçici bir parçası olarak. Aşağıdaki VM boyutları, seçicide görünmez: *F8s_v2*, *F16s_v2*, *F32s_v2*, ve *F64s_v2*.  
  Geçici bir çözüm olarak, bir VM'yi dağıtmak için aşağıdaki yöntemlerden birini kullanın. Her bir yöntemin, kullanmak istediğiniz VM boyutu belirtmeniz gerekir.

- <!-- 3099544 – IS, ASDK --> Azure Stack portalını kullanarak bir yeni sanal makine (VM) oluşturun ve VM boyutu seçin, ABD Doları/ay sütun içeren görüntülenir bir **kullanılamıyor** ileti. Bu sütun görünmemelidir; VM görüntüleme fiyatlandırma sütunu Azure Stack'te desteklenmiyor.

- <!-- 2869209 – IS, ASDK --> Kullanırken [ **Ekle AzsPlatformImage** cmdlet'i](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), kullanmalısınız **- OsUri** parametre olarak depolama hesabı URI'si disk nereye yüklenir. Yerel yol diskin kullanırsanız, cmdlet şu hatayla başarısız olur: *İşlemi uzun süre çalışan 'Başarısız' durumuyla başarısız oldu*. 

- <!--  2966665 – IS, ASDK --> SSD ekleme yönetilen disk sanal makineler (DS, DSv2, Fs, Fs_V2) bir hatayla başarısız oluyor premium boyutuna veri diskleri:  *'Vmname' hata sanal makinenin diskleri güncelleştirilemedi: İstenen işlem gerçekleştirilemiyor, depolama hesabı türü 'Premium_LRS' VM boyutu için desteklenmediğinden ' Standard_DS/Ds_V2/FS/Fs_v2)*

   Bu sorunu geçici olarak çözmek için kullanın *Standard_LRS* veri diskleri yerine *Premium_LRS diskleri*. Kullanım *Standard_LRS* veri diskleri IOPS veya fatura ücreti değiştirmez.  

- <!--  2795678 – IS, ASDK --> VM, portalı sanal makineler (VM) oluşturmak için bir premium VM boyutu (DS, Ds_v2, FS, FSv2) kullandığınızda, bir standart depolama hesabı oluşturulur. Bir standart depolama hesabı oluşturma IOPS, işlevsel olarak, etkilemez ya da fatura. 

   Bildiren bir uyarıyı güvenle yok sayabilirsiniz: *Premium diskleri destekleyen bir boyutta standart disk kullanmayı seçtiniz. Bu işletim sisteminin performansını etkileyebilir ve önerilmez. Premium depolamayı (SSD) kullanmayı düşünün.*

- <!-- 2967447 - IS, ASDK --> Sanal makine ölçek kümesi (VMSS) deneyimi oluşturmasına 7.2 CentOS tabanlı dağıtım için bir seçenek olarak sağlar. Bu görüntüyü Azure Stack üzerinde kullanılabilir olmadığından, dağıtımınız için başka bir işletim sistemi seçin veya Market'ten dağıtımdan işleciyle indirildi başka bir CentOS görüntüsü belirten bir ARM şablonu kullanın.

<!-- TBD -  IS ASDK --> 
- Ölçeklendirme ayarları sanal makine ölçek kümeleri için portalda kullanılabilir değildir. Geçici çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürüm farklar nedeniyle, kullanmanız gereken `-Name` yerine parametre `-VMScaleSetName`.

<!-- TBD -  IS ASDK --> 
- Portal, Azure Stack kullanıcı portalında sanal makine oluşturduğunuzda, D serisi VM ekleyebilirsiniz veri diskleri yanlış sayıda görüntüler. Tüm desteklenen D serisi VM'ler gibi çok sayıda veri diski Azure yapılandırması sağlayabilir.

<!-- TBD -  IS ASDK --> 
- Bir VM görüntüsü oluşturulması başarısız olduğunda, nelze odstranit başarısız bir öğe için VM görüntüleri işlem dikey eklenebilir.

  Geçici çözüm olarak, Hyper-V ile oluşturduğunuz işlevsiz bir VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yolu C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silme engelleyen sorunu çözer. Ardından, işlevsiz bir görüntü oluşturduktan sonra 15 dakika başarıyla silebilirsiniz.

  Ardından, daha önce başarısız olan VM görüntüsünün indirilmesi yeniden deneyebilir.

<!-- TBD -  IS ASDK --> 
- VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

<!-- 1662991 - IS ASDK --> 
- Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur.

<!-- 2724961- IS ASDK --> 
- Kaydettiğinizde **Microsoft.Insight** abonelik ayarlarını, kaynak sağlayıcısındaki bir Windows VM oluşturma ve konuk işletim sistemi etkin Tanılama ile VM genel bakış sayfasında ölçüm verilerini da göstermez. 

 

#### <a name="networking"></a>Ağ
<!-- 1766332 - IS, ASDK --> 
- Altında **ağ**, tıklarsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **ilkesine** bir VPN türü listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure Stack'te desteklenir.

<!-- 1902460 -  IS ASDK --> 
- Azure Stack destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu tüm Kiracı abonelikler arasında geçerlidir. Aynı IP adresine sahip bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı, sonraki oluşturulmasını denemeden sonra engellenir.

<!-- 16309153 -  IS ASDK --> 
- DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu vnet'teki VM'ler için güncelleştirilmiş ayarları gönderiliyor değil.

<!-- 2702741 -  IS ASDK --> 
- Dinamik ayırma yöntemi kullanılarak dağıtılan genel IP'ler garanti edilmez durdurun-serbest verildiği sonra korunması.

<!-- 2529607 - IS ASDK --> 
- Azure Stack sırasında *gizli dönüş*, bir süre içinde genel IP adresleri olan erişilemeyen iki ila beş dakikalığına yoktur.

<!-- 2664148 - IS ASDK --> 
- Bir S2S VPN tüneli aracılığıyla Kiracı sanal makinelerini burada erişiyor senaryolarda, yerel ağ geçidi için şirket içi alt ağ geçidi zaten oluşturulduktan sonra eklenmişse nerede bağlantı girişimleri başarısız bir senaryo hatalarla karşılaşabilirsiniz. 


#### <a name="sql-and-mysql"></a>SQL ve MySQL
<!-- TBD - ASDK --> 
- Veritabanını barındıran sunucu kaynak sağlayıcısı ve kullanıcı iş yükleri tarafından kullanım için ayrılmış olmalıdır. Uygulama Hizmetleri dahil olmak üzere diğer herhangi bir tüketici tarafından kullanılan bir örnek kullanılamaz.

<!-- IS, ASDK --> 
- Özel karakterler, boşluk ve nokta dahil olmak üzere desteklenmez **ailesi** adı, SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda.

#### <a name="app-service"></a>App Service
<!-- 2352906 - IS ASDK --> 
- Kullanıcılar, bunlar, ilk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

<!-- TBD - IS ASDK --> 
- (Çalışan, yönetim, ön uç rollerini) altyapıyı ölçeklendirme için sürüm notları için işlem açıklandığı PowerShell kullanmanız gerekir.  

<!-- TBD - IS ASDK --> 
- App Service yalnızca dağıtılabilir içine *varsayılan sağlayıcı aboneliği* şu anda.

#### <a name="usage"></a>Kullanım  
<!-- TBD -  IS ASDK --> 
- Kullanım genel IP adresi kullanım ölçümü verilerini gösterir aynı *olay tarihi-saati* yerine her kaydın değerini *TimeDate* gösteren kaydın oluşturulduğu zaman damgası. Şu anda, genel IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.

<!-- #### Identity -->

