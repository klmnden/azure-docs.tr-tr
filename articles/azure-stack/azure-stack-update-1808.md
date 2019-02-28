---
title: Azure Stack 1808 güncelleştirme | Microsoft Docs
description: Bilinen sorunlar da dahil olmak üzere Azure Stack tümleşik sistemleri için 1808 güncelleştirmesindeki yenilikler ve güncelleştirme karşıdan yükleme konumu hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2019
ms.author: sethm
ms.reviewer: justini
ms.lastreviewed: 02/28/2019
ms.openlocfilehash: b94d27e903fd01c3be029128a64c3e3369669bf7
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2019
ms.locfileid: "56986161"
---
# <a name="azure-stack-1808-update"></a>Azure Stack 1808 güncelleştirme

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Bu makalede 1808 güncelleştirme paketinin içeriğini açıklar. Güncelleştirme paketinin bu sürümü, Azure Stack için bilinen sorunlar geliştirmeleri ve düzeltmeleri içerir. Bu makalede, güncelleştirmeyi indirebilmesi bağlantıyı da içerir. Bilinen sorunlar için güncelleştirme işlemini doğrudan ilgili sorunları ve derleme (yükleme sonrası) ile ilgili sorunlar ayrılır.

> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack 1808 güncelleştirmenin yapı numarasıdır **1.1808.0.97**.  

### <a name="new-features"></a>Yeni Özellikler

Bu güncelleştirme Azure Stack için aşağıdaki geliştirmeleri içerir.

<!--  2682594   | IS  --> 
- **Tüm Azure Stack ortamlarında artık Eşgüdümlü Evrensel Saat (UTC) saat dilimi biçimini kullanın.**  Tüm günlük verileri ve ilgili bilgileri artık UTC biçiminde görüntüler. UTC saat kullanarak yüklenmedi önceki bir sürümden güncelleştirmezseniz ortamınız UTC kullanmak için güncelleştirilir. 

<!-- 2437250  | IS  ASDK --> 
- **Yönetilen diskleri desteklenir.** Azure Stack sanal makineler ve sanal makine ölçek kümeleri artık yönetilen diskler kullanabilirsiniz. Daha fazla bilgi için [Azure Stack yönetilen diskler: Farklılıklar ve dikkat edilmesi gerekenler](/azure/azure-stack/user/azure-stack-managed-disk-considerations).

<!-- 2563799  | IS  ASDK --> 
- **Azure İzleyici**. Azure'da Azure İzleyici gibi Azure Stack'te Azure İzleyici, temel düzeyde altyapı ölçümlerini ve günlüklerini çoğu hizmetleri sağlar. Daha fazla bilgi için [Azure Stack'te Azure İzleyici](/azure/azure-stack/user/azure-stack-metrics-azure-data).

<!-- 2487932| IS --> 
- **Uzantı ana bilgisayar için hazırlama**. Uzantısı konağı gerekli TCP/IP bağlantı noktası sayısını azaltarak güvenli Azure Stack yardımcı olmak için kullanabilirsiniz. 1808 güncelleştirmesiyle, hazırlama, Azure Stack uzantısı konağı için hazırlanın. Daha fazla bilgi için [hazırlamak için Azure Stack için uzantısı konağı](/azure/azure-stack/azure-stack-extension-host-prepare).

<!-- IS --> 
- **Sanal makine ölçek kümeleri için galeri öğeleri artık yerleşiktir**. Sanal makine ölçek kümesi galeri öğesini şimdi indirmek zorunda kalmadan kullanıcı ve Yönetici portalındaki yapılır.  1808 için yükseltirseniz, Yükseltme tamamlandığında kullanılabilir.  

<!-- IS, ASDK --> 
- **Sanal makine ölçek kümesi ölçeklendirme**. Portala kullanabileceğiniz [bir sanal makine ölçek kümesi ölçeği](azure-stack-compute-add-scalesets.md#scale-a-virtual-machine-scale-set) (VMSS).    

<!-- 2489570 | IS ASDK--> 
- **Özel IPSec/IKE İlkesi yapılandırmalarını desteği** için [Azure Stack hizmetinde VPN ağ geçitlerinin](/azure/azure-stack/azure-stack-vpn-gateway-about-vpn-gateways).

<!-- | IS ASDK--> 
- **Kubernetes Market öğesi**. Şimdi kullanarak Kubernetes kümelerini dağıtmayı [Kubernetes Market öğesi](azure-stack-solution-template-kubernetes-cluster-add.md). Kullanıcılar, Kubernetes öğeyi seçin ve Azure Stack'e bir Kubernetes kümesi dağıtmak için bazı parametreleri doldurun. Şablonları amacı, geliştirme/test Kubernetes dağıtımları birkaç adımda ayarlamak için kullanıcılara basit olmasını sağlamaktır.

<!-- | IS ASDK--> 
- **Blok zinciri şablonları**. Artık yürütebilir [Ethereum consortium dağıtımları](user/azure-stack-ethereum.md) Azure Stack'te. Üç yeni şablonlarında bulabilirsiniz [Azure Stack hızlı başlangıç şablonları](https://github.com/Azure/AzureStack-QuickStart-Templates). Bunlar dağıtma ve Azure ve Ethereum minimum bilgi ile çok üye consortium Ethereum ağ yapılandırma izin verin. Şablonları amacı, geliştirme/test Blockchain dağıtımları birkaç adımda ayarlamak için kullanıcılara basit olmasını sağlamaktır.

<!-- | IS ASDK--> 
- **API Sürüm profili 2017-03-09-profile 2018-03-01-karma güncelleştirildi**. API profillerini Azure kaynak sağlayıcısı ve Azure REST uç noktaları için API sürümü belirtin. Profilleri hakkında daha fazla bilgi için bkz. [yönetme API sürümü profillerini Azure Stack'te](/azure/azure-stack/user/azure-stack-version-profiles).

### <a name="fixed-issues"></a>Düzeltilen sorunlar

<!-- IS ASDK--> 
- Bir kullanılabilirlik kümesi Portal'da bulunan ve bir hata etki alanı ve 1 güncelleştirme etki alanına sahip kümesinde sonuçlandı oluşturmaya yönelik bir sorunu düzelttik. 

<!-- IS ASDK --> 
- Sanal makine ölçek kümeleri ölçeklendirme ayarları Portalı'nda kullanıma sunulmuştur.  

<!-- 2494144- IS, ASDK --> 
- Bazı F serisi sanal makine boyutları, dağıtım için bir VM boyutu seçerken görünmesini engelleyen sorunu artık çözülmüştür. 

<!-- IS, ASDK --> 
- Sanal makineler ve en iyi duruma getirilmiş daha fazlasını oluştururken performans geliştirmeleri temel alınan depolama kullanın.

- **Çeşitli düzeltmeleri** performans, kararlılık, güvenlik ve Azure Stack tarafından kullanılan işletim sistemi için.


### <a name="changes"></a>Değişiklikler
<!-- 1697698  | IS, ASDK --> 
- *Hızlı Başlangıç öğreticileri* Kullanıcı Portalı Panosu şimdi bağlantıda ilgili makaleleri çevrimiçi Azure Stack belgeleri.

<!-- 2515955   | IS ,ASDK--> 
- *Tüm hizmetler* değiştirir *diğer hizmetler* Azure Stack yönetici ve Kullanıcı Portalı'nda. Artık *tüm hizmetleri* Azure portallarında yaptığınız gibi Azure Stack portallarında gezinmek için alternatif olarak.

<!-- TBD | IS, ASDK --> 
- *+ Kaynak Oluştur* değiştirir *+ yeni* Azure Stack yönetici ve Kullanıcı Portalı'nda.  Artık *+ kaynak Oluştur* Azure portallarında yaptığınız gibi Azure Stack portallarında gezinmek için alternatif olarak.  

<!--  TBD – IS, ASDK --> 
- *Temel A* sanal makine boyutları için devre dışı [sanal makine ölçek kümeleri oluşturma](azure-stack-compute-add-scalesets.md) (VMSS) portal üzerinden. Bu boyut ile bir VMSS oluşturmak için PowerShell ya da bir şablon kullanın.  

### <a name="common-vulnerabilities-and-exposures"></a>Yaygın güvenlik açıklarına ve exposures'ı

Bu güncelleştirme aşağıdaki güncelleştirmeleri yükler:  

- [CVE-2018-0952](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-0952)
- [CVE-2018-8200](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8200)
- [CVE-2018-8204](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8204)
- [CVE-2018-8253](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8253)
- [CVE-2018-8339](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8339)
- [CVE-2018-8340](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8340)
- [CVE-2018-8341](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8341)
- [CVE-2018-8343](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8343)
- [CVE-2018-8344](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8344)
- [CVE-2018-8345](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8345)
- [CVE-2018-8347](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8347)
- [CVE-2018-8348](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8348)
- [CVE-2018-8349](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8349)
- [CVE-2018-8394](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8394)
- [CVE-2018-8398](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8398)
- [CVE-2018-8401](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8401)
- [CVE-2018-8404](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8404)
- [CVE-2018-8405](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8405)
- [CVE-2018-8406](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8406)  

Bu güvenlik açıkları hakkında daha fazla bilgi için yukarıdaki bağlantılara tıklayın veya Microsoft Bilgi Bankası makalesi bkz [4343887](https://support.microsoft.com/help/4343887).

Bu güncelleştirme ayrıca L1 Terminal içinde açıklanan hata (L1TF) olarak bilinen kurgusal yürütme yan kanal güvenlik açığına yönelik risk azaltma içeren [Microsoft Güvenlik Danışma ADV180018](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180018).  

### <a name="prerequisites"></a>Önkoşullar

- Azure Stack 1808 güncelleştirmeyi uygulamadan önce Azure Stack 1807 güncelleştirmesini yükleyin. 

- En son kullanılabilir güncelleştirme veya düzeltme sürümü 1807 yükleyin.  
  > [!TIP]  
  > Aşağıdaki abone olmak *RRS* veya *Atom* Azure Stack düzeltmelerle birlikte kalmasını sağlamak için akışları:
  > - RRS: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss ... 
  > - Atom: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom ...

- Bu güncelleştirme yüklemesi başlamadan önce çalıştırması [Test AzureStack](azure-stack-diagnostic-test.md) bulunan tüm çalışma sorunlarını çözün ve Azure Stack durumunu doğrulamak için aşağıdaki parametreleri, tüm uyarılar ve hatalar dahil olmak üzere. Ayrıca etkin Uyarıları gözden geçirin ve eylemi gerektiren tüm çözümleyin.  

  ```PowerShell
  Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
  ```

- Azure Stack System Center Operations Manager (SCOM) tarafından yönetildiğinde 1808 uygulamadan önce Yönetim Paketi için Microsoft Azure Stack 10.0.3.11 sürümüne güncelleştirdiğinizden emin olun.

### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar

- Çalıştırdığınızda [Test AzureStack](azure-stack-diagnostic-test.md) 1808 Güncelleştirme tamamlandıktan sonra temel kart yönetim denetleyicisi (BMC) bir uyarı iletisi görüntülenir. Bu uyarıyı güvenle yok sayabilirsiniz.

<!-- 2468613 - IS --> 
- Bu güncelleştirme yüklemesi sırasında başlık uyarılarla görebileceğiniz *hatası – şablon FaultType UserAccounts.New için eksik.*  Bu uyarılar güvenle yok sayabilirsiniz. Bu güncelleştirme yüklemesi tamamlandıktan sonra bu uyarıları otomatik olarak kapatılacak.

<!-- 2489559 - IS --> 
- Bu güncelleştirme yüklemesi sırasında sanal makineleri oluşturmaya çalışmayın. Güncelleştirmeleri yönetme hakkında daha fazla bilgi için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md#plan-for-updates).

<!-- 2830461 - IS --> 
- Bazı durumlarda, bir güncelleştirme dikkat gerektirdiğinde karşılık gelen bir uyarı oluşturulamayabilir. Doğru durumu Portalı'nda yine de yansıtılır ve etkilenmez.

### <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar

> [!Important]  
> Azure Stack dağıtımınıza uzantısı konağı için hazır olun. Aşağıdaki yönergeleri kullanarak sisteminizi hazırlama [hazırlamak için Azure Stack için uzantısı konağı](azure-stack-extension-host-prepare.md).

Bu güncelleştirme yüklendikten sonra geçerli düzeltmeleri yükleyin. Daha fazla bilgi için aşağıdaki Bilgi Bankası makaleleri görüntülemek hem de bizim [hizmet İlkesi](azure-stack-servicing-policy.md). 
- [KB 4481066 – Azure Stack düzeltme Azure Stack düzeltme 1.1808.9.117](https://support.microsoft.com/help/4481066/)


## <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

Bu derleme sürümü için yükleme sonrası bilinen sorunlar verilmiştir.

### <a name="portal"></a>Portal

- Azure Stack teknik belgeleri, Azure Stack en son sürüm üzerinde odaklanır. Sürümler arasında Portal değişiklikleri nedeniyle Azure Stack portalı kullanırken gördüğünüz belgelerinde gördüğünüz verilerden değişebilir. 

<!-- TBD - IS ASDK --> 
- Bir boş Pano portalında görebilirsiniz. Pano kurtarmak için **Pano düzenleme**, sonra sağ tıklayıp **varsayılan durumuna sıfırlansın**.

<!-- 2930718 - IS ASDK --> 
- Yönetici portalında herhangi bir kullanıcı aboneliği ayrıntıları tıklandığında ve dikey pencereyi kapatmadan sonra erişirken **son**, kullanıcı abonelik adı görünmez.

<!-- 3060156 - IS ASDK --> 
- Yönetici ve Kullanıcı Portalı, portal ayarları ve seçme içinde **tüm ayarları ve özel panoları Sil** beklendiği gibi çalışmaz. Bir hata bildirimi görüntülenir. 

<!-- 2930799 - IS ASDK --> 
- Yönetici ve Kullanıcı Portalı'nda altında **tüm hizmetleri**, varlık **DDoS koruma planları** yanlış listelenir. Azure Stack'te gerçekten kullanılabilir değil. Bunu oluşturmayı denerseniz, portalda Market öğesi oluşturulamadı belirten bir hata görüntülenir. 

<!-- 2930820 - IS ASDK --> 
- "Docker için" araması yaparsanız yönetici ve kullanıcı portalı yanlış öğesi döndürülür. Azure Stack'te gerçekten kullanılabilir değil. Bunu oluşturmayı denerseniz, bir hata göstergesi içeren bir dikey pencere görüntülenir. 

<!-- 2967387 – IS, ASDK --> 
- Azure Stack yönetici veya kullanıcı portalı oturum açmak için kullandığınız hesap görüntüler olarak **tanımlanmayan kullanıcı**. Hesap ya da sahip olmadığında bu oluşur bir *ilk* veya *son* adı belirtilmedi. Bu sorunu geçici olarak çözmek için ilk veya son sağlamak için kullanıcı hesabı düzenleyin. Oturumu kapatın ve sonra portalda yeniden oturum gerekir. 

<!--  2873083 - IS ASDK --> 
-  Bir sanal makine ölçek oluşturmak için portalı kullandığınızda ayarlayın (VMSS) *örnek boyutu* açılan olmayan yük doğru bir şekilde Internet Explorer kullandığınızda. Bu sorunu çözmek için bir VMSS oluşturmak için portalı kullanırken başka bir tarayıcı kullanın.  

<!-- 2931230 – IS  ASDK --> 
- Kullanıcı aboneliği plan kaldırdığınızda bile, bir kullanıcı abonelikte eklenti planı eklendiği planları silinemiyor. Eklenti planı başvuru abonelikleri de silinene kadar plan kalır. 

<!--2760466 – IS  ASDK --> 
- Bu sürümünü çalıştıran yeni bir Azure Stack ortamına yüklediğinizde, uyarıyı gösterir *etkinleştirme gerekli* görüntülenmeyebilir. [Etkinleştirme](azure-stack-registration.md) Market dağıtım kullanabilmeniz için gereklidir.  

<!-- TBD - IS ASDK --> 
- 1804 sürümü ile sunulan iki Yönetim abonelik türlerini kullanılmamalıdır. Abonelik türleridir **abonelik ölçümü**, ve **tüketim abonelik**. Bu abonelik türlerini 1804 sürümünden başlayarak yeni Azure Stack ortamlarında görülebilir ancak henüz kullanıma sunulmamıştır. Kullanmaya devam etmelidir **varsayılan sağlayıcı** abonelik türü.

<!-- TBD - IS ASDK --> 
- Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

<!-- TBD - IS ASDK --> 
- Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.


### <a name="health-and-monitoring"></a>Sistem durumu ve izleme

<!-- TBD - IS -->
- Art arda görünür ve daha sonra Azure Stack sisteminizde kaybolur aşağıdaki uyarılar görebilirsiniz:
   - *Altyapı rol örneği kullanılamıyor*
   - *Ölçek birimi düğümü çevrimdışı.*
   
  Lütfen çalıştırın [Test AzureStack](azure-stack-diagnostic-test.md) altyapı rol örneklerinin durumunu doğrulamak ve birim düğümlerini ölçeklendirme cmdlet'i. Hiçbir sorun tarafından algılanan [Test AzureStack](azure-stack-diagnostic-test.md), bu uyarılar yoksayabilirsiniz. Bir sorun algılandığında, altyapı rol örneği veya Yönetim Portalı veya PowerShell kullanarak bir düğümü başlatma girişiminde bulunabilir.

  En son bu sorun düzeltilene [1808 düzeltme yayın](https://support.microsoft.com/help/4481066/), bu nedenle sorun yaşıyorsanız bu düzeltmenin yüklenebilmesi emin olun.

<!-- 1264761 - IS ASDK --> 
- Uyarıları görebilirsiniz **sistem durumu denetleyicisi** aşağıdaki ayrıntıları olan bir bileşeni:  

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

  Her iki uyarılar güvenle yoksayılabilir ve zaman içinde otomatik olarak kapatılması.  


<!-- 2812138 | IS --> 
- İçin bir uyarı görebileceğiniz **depolama** aşağıdaki ayrıntıları içeren bileşeni:

   - ADI: Depolama hizmeti iç iletişim hatası  
   - ÖNEM DERECESİ: Kritik  
   - BİLEŞEN: Depolama  
   - AÇIKLAMA: Aşağıdaki düğümlere istekleri gönderirken, depolama hizmeti iç iletişim hatası oluştu.  

    Uyarıyı güvenle yoksayılabilir, ancak uyarıyı el ile kapatmanız gerekir.

<!-- 2368581 - IS. ASDK --> 
- Düşük bellek uyarısı alırsanız ve Kiracı sanal makinelerini başarısız ile dağıtmak bir Azure Stack operatörü bir **Fabric VM oluşturma hatası**, Azure Stack damga kullanılabilir bellek yetersiz olabilir. Kullanım [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) iş yükleriniz için kapasite en iyi anlamak için.

### <a name="compute"></a>İşlem

- Oluştururken bir [Dv2 serisi VM](./user/azure-stack-vm-considerations.md#virtual-machine-sizes), D11-14v2 Vm'leri sırasıyla 4, 8, 16 ve 32 veri diskleri oluşturmanıza izin. Ancak, 8, 16, 32 ve 64 veri diski oluştur VM bölmesi gösterir.

<!-- 3164607 – IS, ASDK -->
- Aynı ada ve LUN ile aynı sanal makine (VM) için ayrılmış bir diski yeniden bağlanması başarısız bir hata ile gibi **veri diski 'datadisk', 'vm1' VM'sine iliştirilemiyor**. Disk şu anda ayrılmakta veya son ayırma işlemi başarısız oldu hata oluşur. Lütfen disk tamamen ayrılmış kadar bekleyin ve sonra yeniden deneyin veya silme/diski açıkça tekrar ayırın. Geçici çözüm, farklı bir adla veya farklı bir LUN üzerinde yeniden sağlamaktır. 

<!-- 3099544 – IS, ASDK --> 
- Azure Stack portalını kullanarak yeni bir VM oluşturun ve VM boyutu seçin, ABD Doları/ay sütun içeren görüntülenir bir **kullanılamıyor** ileti. Bu sütun görünmemelidir; VM görüntüleme fiyatlandırma sütunu Azure Stack'te desteklenmiyor.

<!-- 3090289 – IS, ASDK --> 
- Güncelleştirme 1808 uyguladıktan sonra yönetilen disklere sahip VM'ler dağıtırken aşağıdaki sorunlarla karşılaşabilirsiniz:

   1. Aboneliği, yönetilen diskler ile VM dağıtma 1808 güncelleştirmeden önce oluşturulduysa, bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için her abonelik için şu adımları izleyin:
      1. Kiracı Portalı'nda Git **abonelikleri** ve aboneliği bulunamıyor. Tıklayın **kaynak sağlayıcıları**, ardından **Microsoft.Compute**ve ardından **yeniden kaydettirin**.
      2. Aynı abonelik altında Git **erişim denetimi (IAM)**, doğrulayın **AzureStack DiskRP istemci** rol listelenmektedir.
   2. Bir konuk dizin ile ilişkili bir abonelik içindeki Vm'leri dağıtma, çok kiracılı bir ortam yapılandırdıysanız, bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için aşağıdaki adımları izleyin:
      1. Uygulama [1808 Azure Stack düzeltme](https://support.microsoft.com/help/4481066/).
      2. Bağlantısındaki [bu makalede](azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) her Konuk dizinlerinizi yeniden yapılandırmak için.
      
<!-- 3179561 - IS --> 
- Yönetilen diskler kullanımı açıklandığı saat içinde bildirilen [Azure Stack kullanım SSS](azure-stack-usage-related-faq.md#managed-disks). Ancak, yanlış veya öncesinde Eylül 27 yönetilen diskler kullanımı için ücretlendirilirsiniz, böylece bunun yerine Azure Stack faturalandırma aylık fiyat kullanır. Size geçici olarak ücretleri yönetilen diskler için Eylül faturalama sorunu çözümlenene kadar 27 sonra askıya. Yanlış yönetilen diskler kullanımı için herhangi bir ücret, Microsoft faturalama desteği'ne başvurun.
Azure Stack kullanım API'leri üretilen kullanım raporları, doğru miktarları gösterir ve kullanılabilir.

<!-- 3507629 - IS, ASDK --> 
- Yönetilen diskler oluşturur iki yeni [kota türleri işlem](azure-stack-quota-types.md#compute-quota-types) sağlanabilir yönetilen diskler hizmetin maksimum kapasitesi sınırlamak için. Varsayılan olarak, 2048 GiB her yönetilen diskler kota türü için ayrılır. Ancak, aşağıdaki sorunlarla karşılaşabilirsiniz:

   - 2048 GiB ayrılır ancak 1808 güncelleştirmeden önce oluşturulan kotalarını 0 değerleri yönetilen diskler kotası yönetici Portalı'nda gösterir. Gerçek gereksinimlerinize ve yeni belirlenen göre değerini azaltın veya artırabilirsiniz kota değeri, 2048 GiB varsayılan'ı geçersiz kılar.
   - Kota değeri 0'a güncelleştirin, 2048 GiB varsayılan değerini eşdeğer olacaktır. Geçici çözüm olarak, kota değeri 1 olarak ayarlayın.

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

<!-- 2724873 - IS --> 
- PowerShell cmdlet'lerini kullanırken **başlangıç AzsScaleUnitNode** veya **Stop-AzsScaleunitNode** ölçek birimleri yönetmek için başlatma veya durdurma ölçek birimi için yapılan ilk girişim başarısız olabilir. İlk çalıştırılmasında cmdlet'i başarısız olursa, cmdlet'in ikinci kez çalıştırın. İkinci çalıştırma işlemi tamamlamak için başarılı olmalıdır. 

<!-- TBD - IS ASDK --> 
- Portal, Azure Stack kullanıcı portalında sanal makine oluşturduğunuzda, DS serisi VM ekleyebilirsiniz veri diskleri yanlış sayıda görüntüler. DS serisi VM'ler gibi çok sayıda veri diski Azure yapılandırması sağlayabilir.

<!-- TBD - IS ASDK --> 
- VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

<!-- 1662991 IS ASDK --> 
- Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur.  

<!-- 2724961- IS ASDK --> 
- Kaydettiğinizde **Microsoft.Insight** abonelik ayarlarında, kaynak sağlayıcısı ve bir Windows VM konuk işletim sistemi etkin Tanılama ile oluşturma, VM genel bakış sayfasında CPU yüzdesi grafik, ölçüm verilerini göstermek mümkün olmayacaktır.

   Sanal makine için CPU yüzdesi grafik bulmak için Git **ölçümleri** dikey penceresinde ve desteklenen tüm Windows VM show Konuk ölçümleri.

- Bir Ubuntu 18.04 etkinleştirilmiş SSH yetkilendirme ile oluşturulan VM, oturum açmak için SSH anahtarları kullanmak izin vermez. Geçici bir çözüm olarak Lütfen VM erişimi Linux uzantısı için SSH anahtarları sağladıktan sonra uygulamak için kullanmak veya parola tabanlı kimlik doğrulaması kullanın.

### <a name="networking"></a>Ağ  

<!-- 1766332 - IS ASDK --> 
- Altında **ağ**, tıklarsanız **VPN ağ geçidi Oluştur** bir VPN bağlantısı kurmak için **ilkesine** bir VPN türü listelenir. Bu seçeneği belirlemeyin. Yalnızca **rota tabanlı** seçeneği Azure Stack'te desteklenir.

<!-- 1902460 - IS ASDK --> 
- Azure Stack destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu tüm Kiracı abonelikler arasında geçerlidir. Aynı IP adresine sahip bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı, sonraki oluşturulmasını denemeden sonra engellenir.

<!-- 16309153 - IS ASDK --> 
- DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu vnet'teki VM'ler için güncelleştirilmiş ayarları gönderiliyor değil.

<!-- 2702741 -  IS ASDK --> 
- Dinamik ayırma yöntemi kullanılarak dağıtılan genel IP'ler garanti edilmez durdurun-serbest verildiği sonra korunması.

<!-- 2529607 - IS ASDK --> 
- Azure Stack sırasında *gizli dönüş*, bir süre içinde genel IP adresleri olan erişilemeyen iki ila beş dakikalığına yoktur.

<!-- 2664148 - IS ASDK --> 
- Bir S2S VPN tüneli aracılığıyla Kiracı sanal makinelerini burada erişiyor senaryolarda, yerel ağ geçidi için şirket içi alt ağ geçidi zaten oluşturulduktan sonra eklenmişse nerede bağlantı girişimleri başarısız bir senaryo hatalarla karşılaşabilirsiniz. 


<!-- ### SQL and MySQL-->


### <a name="app-service"></a>App Service

<!-- 2352906 - IS ASDK --> 
- Kullanıcılar, bunlar, ilk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

<!-- 2489178 - IS ASDK --> 
- (Çalışan, yönetim, ön uç rollerini) altyapıyı ölçeklendirme için sürüm notları için işlem açıklandığı PowerShell kullanmanız gerekir.



### <a name="usage"></a>Kullanım  
<!-- TBD - IS ASDK --> 
- Kullanım genel IP adresi kullanım ölçümü verilerini gösterir aynı *olay tarihi-saati* yerine her kaydın değerini *TimeDate* gösteren kaydın oluşturulduğu zaman damgası. Şu anda, genel IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1808 güncelleştirme paketinden indirebileceğiniz [burada](https://aka.ms/azurestackupdatedownload). 

Yalnızca bağlı senaryolarda Azure Stack dağıtımları güvenli bir uç nokta düzenli aralıklarla denetleyin ve bulut için bir güncelleştirme varsa, otomatik olarak bilgilendirme. Daha fazla bilgi için [Azure Stack için güncelleştirmeleri yönetme](azure-stack-updates.md).

## <a name="next-steps"></a>Sonraki adımlar
- Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere bakım ilkeyi gözden geçirmek için bkz: [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).  
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).  
- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).  
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).  
