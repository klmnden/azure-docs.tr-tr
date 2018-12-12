---
title: Azure Stack 1809 güncelleştirmesi | Microsoft Docs
description: Bilinen sorunlar da dahil olmak üzere Azure Stack tümleşik sistemleri için 1809 güncelleştirmesindeki yenilikler ve güncelleştirme karşıdan yükleme konumu hakkında bilgi edinin.
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
ms.date: 12/08/2018
ms.author: sethm
ms.reviewer: justini
ms.openlocfilehash: 5a0d7a0e96a788c3136adba70fb27a2c98674e7a
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53088060"
---
# <a name="azure-stack-1809-update"></a>Azure Stack 1809 güncelleştirme

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bu makalede 1809 güncelleştirme paketinin içeriğini açıklar. Güncelleştirme paketinin bu sürümü, Azure Stack için bilinen sorunlar geliştirmeleri ve düzeltmeleri içerir. Bu makalede, güncelleştirmeyi indirebilmesi bağlantıyı da içerir. Bilinen sorunlar için güncelleştirme işlemini doğrudan ilgili sorunları ve derleme (yükleme sonrası) ile ilgili sorunlar ayrılır.

> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack 1809 güncelleştirmenin yapı numarasıdır **1.1809.0.90**.  

### <a name="new-features"></a>Yeni Özellikler

Bu güncelleştirme Azure Stack için aşağıdaki geliştirmeleri içerir:

- Bu sürümle birlikte, Azure Stack sistemleri desteklediği yapılandırmalar 4-16 düğümlerinin tümleşik. Kullanabileceğiniz [Azure Stack Capacity Planner](https://aka.ms/azstackcapacityplanner) , Azure Stack kapasite ve yapılandırma için planlamasına yardımcı olmak için.

- <!--  2712869   | IS  ASDK -->  **Azure Stack syslog istemcisi (Genel kullanım)** bu istemci denetimler, uyarılar ve Azure Stack altyapısının bir syslog sunucusu veya güvenlik bilgileri ve Olay yönetimi (SIEM) yazılım ilgili güvenlik günlükleri iletilmesini sağlar. Azure Stack için dış. Syslog istemci artık syslog sunucunun dinlediği bağlantı noktasını destekler.

   Bu sürümle birlikte, syslog istemci genel olarak kullanılabilir ve üretim ortamlarında kullanılabilir.

   Daha fazla bilgi için [Azure Stack syslog iletmeyi](azure-stack-integrate-security.md).

- Artık [kayıt kaynak taşıma](azure-stack-registration.md#move-a-registration-resource) yeniden kaydetmek zorunda kalmadan, kaynak grupları arasında Azure üzerinde. Eski ve yeni abonelikler için aynı CSP iş ortağı kimliğini eşlenen sürece bulut çözüm sağlayıcıları (CSP'ler) ayrıca kayıt kaynağı abonelikler arasında taşıyabilirsiniz Bu, var olan müşteri Kiracı eşlemeleri etkilemez. 

### <a name="fixed-issues"></a>Düzeltilen sorunlar

<!-- TBD - IS ASDK -->
- Portalda, boş ve kullanılan kapasiteyi raporlama bellek grafik artık doğru olur. Artık daha güvenilir bir şekilde oluşturmak için kaç tane Vm'niz tahmin edebilirsiniz.

<!-- TBD - IS ASDK --> 
- Sanal makineler Azure Stack kullanıcı portalında oluşturulan ve DS serisi VM ekleyebilirsiniz veri diskleri yanlış sayıda portalı görüntülenen bir sorun düzeltildi. DS serisi VM'ler gibi çok sayıda veri diski Azure yapılandırması sağlayabilir.

- Aşağıdaki yönetilen disk sorunları içinde 1809 sabittir ve ayrıca 1808 içinde sabit [Azure Stack düzeltme 1.1808.9.117](https://support.microsoft.com/help/4481066/): 

   <!--  2966665 – IS, ASDK --> 
   - Hangi düğmelere SSD veri diskler yönetilen disk sanal makineler (DS, DSv2, Fs, Fs_V2) bir hata ile başarısız oldu premium boyuta sorun düzeltildi: *'vmname' hata sanal makinenin diskleri güncelleştirilemedi: İstenen işlem gerçekleştirilemiyor VM boyutu için desteklenmeyen 'Premium_LRS' depolama hesabı türü ' Standard_DS/Ds_V2/FS/Fs_v2)*. 
   
   - Kullanarak yönetilen disk VM'si oluşturma **createOption**: **iliştirme** şu hatayla başarısız oluyor: *işlemi uzun süre çalışan 'Başarısız' durumuyla başarısız oldu. Ek bilgi: 'bir iç yürütme hatası oluştu.'*
   Hata kodu: InternalExecutionError ErrorMessage: bir iç yürütme hatası oluştu.
   
   Bu sorunu artık düzeltildi.

- <!-- 2702741 -  IS, ASDK --> Dinamik Ayırma kullanılarak dağıtılan hangi genel IP'ler, yöntemi değildi neden olan sorun düzeltildi durdurun-serbest verildiği sonra korunması garanti. Bunlar artık korunur.

- <!-- 3078022 - IS, ASDK --> Bir VM 1808 önce durdu-serbest olduysa, 1808 güncelleştirmeden sonra yeniden tahsis bulunamadı.  Bu sorun 1809 içinde düzeltilmiştir. Bu düzeltmeyle 1809 içinde başlatılamadı ve bu durumda örnek başlatılabilir. Düzeltme aynı zamanda bu sorunu yeniden oluşmasını engeller.

### <a name="changes"></a>Değişiklikler

<!-- 2635202 - IS, ASDK -->
- Yedekleme hizmet altyapı taşır gelen [genel altyapı ağı](https://docs.microsoft.com/azure/azure-stack/azure-stack-network#public-infrastructure-network) için [genel VIP ağları](https://docs.microsoft.com/azure/azure-stack/azure-stack-network#public-vip-network). Müşterilerin hizmet yedekleme depolama konumu genel VIP ağdan erişim sahip olmak gerekir.  

> [!IMPORTANT]  
> Dosya sunucusuna genel VIP ağları bağlantılara izin vermeyen bir güvenlik duvarınız varsa, bu değişiklik altyapı yedeklemeleri "ağ yolu bulunamadı hatası ile 53" başarısız olmasına neden olur Geçici çözüm yoktur makul olan bir değişiklik budur. Microsoft, müşteri geri bildirimi doğrultusunda, bu değişikliği, bir düzeltme döner. Lütfen inceleyin [güncelleştirme adımlar bölümüne gönderin](#post-update-steps) 1809 kullanılabilir düzeltmeler hakkında daha fazla bilgi için. Düzeltme kullanılabilir olduğunda, yalnızca ağ ilkelerinizi altyapı aboneliklerinin kaynaklarına erişmek genel VIP ağları izin vermiyorsa için 1809 güncelleştirdikten sonra uygulanacak emin olun. 1811 içinde tüm sistemler için bu değişiklik geçerli olacaktır. İçinde 1809 düzeltme uygulandığında, gereken başka bir işlem yoktur.  

### <a name="common-vulnerabilities-and-exposures"></a>Yaygın güvenlik açıklarına ve exposures'ı

Bu güncelleştirme, aşağıdaki güvenlik güncelleştirmeleri yükler:  

- [ADV180022](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180022)
- [CVE-2018-0965](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-0965)
- [CVE-2018-8271](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8271)
- [CVE-2018-8320](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8320)
- [CVE-2018-8330](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8330)
- [CVE-2018-8332](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8332)
- [CVE-2018-8333](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8333)
- [CVE-2018-8335](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8335)
- [CVE-2018-8392](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8392)
- [CVE-2018-8393](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8393)
- [CVE-2018-8410](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8410)
- [CVE-2018-8411](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8411)
- [CVE-2018-8413](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8413)
- [CVE-2018-8419](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8419)
- [CVE-2018-8420](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8420)
- [CVE-2018-8423](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8423)
- [CVE-2018-8424](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8424)
- [CVE-2018-8433](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8433)
- [CVE-2018-8434](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8434)
- [CVE-2018-8435](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8435)
- [CVE-2018-8438](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8438)
- [CVE-2018-8439](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8439)
- [CVE-2018-8440](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8440)
- [CVE-2018-8442](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8442)
- [CVE-2018-8443](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8443)
- [CVE-2018-8446](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8446)
- [CVE-2018-8449](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8449)
- [CVE-2018-8453](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8453)
- [CVE-2018-8455](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8455)
- [CVE-2018-8462](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8462)
- [CVE-2018-8468](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8468)
- [CVE-2018-8472](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8472)
- [CVE-2018-8475](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8475)
- [CVE-2018-8481](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8481)
- [CVE-2018-8482](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8482)
- [CVE-2018-8484](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8484)
- [CVE-2018-8486](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8486)
- [CVE-2018-8489](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8489)
- [CVE-2018-8490](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8490)
- [CVE-2018-8492](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8492)
- [CVE-2018-8493](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8493)
- [CVE-2018-8494](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8494)
- [CVE-2018-8495](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8495)
- [CVE-2018-8497](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8497)

Bu güvenlik açıkları hakkında daha fazla bilgi için yukarıdaki bağlantılara tıklayın veya Microsoft Bilgi Bankası makalelerine bakın [4457131](https://support.microsoft.com/help/4457131) ve [4462917](https://support.microsoft.com/help/4462917).

### <a name="prerequisites"></a>Önkoşullar

- En son Azure Stack düzeltmeyi 1809 uygulamadan önce 1808 için yükleyin. Daha fazla bilgi için [KB 4481066 – Azure Stack düzeltme Azure Stack düzeltme 1.1808.9.117](https://support.microsoft.com/help/4481066/).

  > [!TIP]  
  > Aşağıdaki abone olmak *RRS* veya *Atom* Azure Stack düzeltmelerle birlikte kalmasını sağlamak için akışları:
  > - RRS: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss ... 
  > - Atom: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom ...


- Bu güncelleştirme yüklemesi başlamadan önce çalıştırması [Test AzureStack](azure-stack-diagnostic-test.md) bulunan tüm çalışma sorunlarını çözün ve Azure Stack durumunu doğrulamak için aşağıdaki parametreleri, tüm uyarılar ve hatalar dahil olmak üzere. Ayrıca etkin Uyarıları gözden geçirin ve eylemi gerektiren tüm çözümleyin.  

  ```PowerShell
  Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
  ``` 

### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar

- Çalıştırdığınızda [Test AzureStack](azure-stack-diagnostic-test.md) 1809 Güncelleştirme tamamlandıktan sonra temel kart yönetim denetleyicisi (BMC) bir uyarı iletisi görüntülenir. Bu uyarıyı güvenle yok sayabilirsiniz.

- <!-- 2468613 - IS --> Bu güncelleştirme yüklemesi sırasında başlık uyarılarla görebileceğiniz *hatası – şablon FaultType UserAccounts.New için eksik.*  Bu uyarılar güvenle yok sayabilirsiniz. Bu güncelleştirme yüklemesi tamamlandıktan sonra bu uyarıları otomatik olarak kapatılacak.

- <!-- 2489559 - IS --> Bu güncelleştirme yüklemesi sırasında sanal makineleri oluşturmaya çalışmayın. Güncelleştirmeleri yönetme hakkında daha fazla bilgi için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md#plan-for-updates).

- <!-- 3139614 | IS --> Azure Stack için bir güncelleştirme uyguladıysanız, OEM'den **güncelleştirme kullanılabilir** bildirim Azure Stack Yönetici portalında görünmeyebilir. Microsoft update yüklemek için indirin ve burada bulunan yönergeleri kullanarak el ile içeri aktarılması [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).

### <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar

> [!Important]  
> Azure Stack dağıtımınıza, sonraki güncelleştirme paketi tarafından etkinleştirilen uzantısı konağı için hazırlanın. Aşağıdaki yönergeleri kullanarak sisteminizi hazırlama [hazırlamak için Azure Stack için uzantısı konağı](azure-stack-extension-host-prepare.md).

Bu güncelleştirme yüklendikten sonra geçerli düzeltmeleri yükleyin. Daha fazla bilgi için aşağıdaki Bilgi Bankası makaleleri görüntülemek hem de bizim [hizmet İlkesi](azure-stack-servicing-policy.md).  
- [KB 4481548 – Azure Stack düzeltme Azure Stack düzeltme 1.1809.12.114](https://support.microsoft.com/help/4481548/)  

## <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

Bu derleme sürümü için yükleme sonrası bilinen sorunlar verilmiştir.

### <a name="portal"></a>Portal

- Azure Stack teknik belgeleri, en son sürüm üzerinde odaklanır. Sürümler arasında Portal değişiklikleri nedeniyle Azure Stack portalı kullanırken gördüğünüz belgelerinde gördüğünüz verilerden değişebilir.

<!-- 2930718 - IS ASDK --> 
- Yönetici portalında herhangi bir kullanıcı aboneliği ayrıntıları tıklandığında ve dikey pencereyi kapatmadan sonra erişirken **son**, kullanıcı abonelik adı görünmez.

<!-- 3060156 - IS ASDK --> 
- Yönetici ve Kullanıcı Portalı, portal ayarları ve seçme içinde **tüm ayarları ve özel panoları Sil** beklendiği gibi çalışmaz. Bir hata bildirimi görüntülenir. 

<!-- 2930799 - IS ASDK --> 
- Yönetici ve Kullanıcı Portalı'nda altında **tüm hizmetleri**, varlık **DDoS koruma planları** yanlış listelenir. Azure Stack'te kullanılamıyor. Bunu oluşturmayı denerseniz, portalda Market öğesi oluşturulamadı belirten bir hata görüntülenir. 

<!-- 2930820 - IS ASDK --> 
- "Docker için" araması yaparsanız yönetici ve kullanıcı portalı yanlış öğesi döndürülür. Azure Stack'te kullanılamıyor. Bunu oluşturmayı denerseniz, bir hata göstergesi içeren bir dikey pencere görüntülenir. 

<!-- 2967387 – IS, ASDK --> 
- Azure Stack yönetici veya kullanıcı portalı oturum açmak için kullandığınız hesap görüntüler olarak **tanımlanmayan kullanıcı**. Bu ileti, hesap ya da sahip olmadığı durumlarda görüntülenir bir *ilk* veya *son* adı belirtilmedi. Bu sorunu geçici olarak çözmek için ilk veya son sağlamak için kullanıcı hesabı düzenleyin. Oturumu kapatın ve sonra portalda yeniden oturum gerekir.  

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
   
  Çalıştırma [Test AzureStack](azure-stack-diagnostic-test.md) altyapı rol örneklerinin durumunu doğrulamak ve birim düğümlerini ölçeklendirme cmdlet'i. Hiçbir sorun tarafından algılanan [Test AzureStack](azure-stack-diagnostic-test.md), bu uyarılar yoksayabilirsiniz. Bir sorun algılandığında, altyapı rol örneği veya Yönetim Portalı veya PowerShell kullanarak bir düğümü başlatma girişiminde bulunabilir.

  En son bu sorun düzeltilene [1809 düzeltme yayın](https://support.microsoft.com/help/4481548/), bu nedenle sorun yaşıyorsanız bu düzeltmenin yüklenebilmesi emin olun. 

<!-- 1264761 - IS ASDK -->  
- Uyarıları görebilirsiniz **sistem durumu denetleyicisi** aşağıdaki ayrıntıları olan bir bileşeni:  

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


<!-- 2812138 | IS --> 
- İçin bir uyarı görebileceğiniz **depolama** aşağıdaki ayrıntıları olan bir bileşeni:

   - Ad: Depolama hizmeti iç iletişim hatası  
   - Önem DERECESİ: kritik  
   - Bileşen: depolama  
   - Açıklama: Aşağıdaki düğümlere istekleri gönderirken, depolama hizmeti iç iletişim hatası oluştu.  

    Uyarıyı güvenle yoksayılabilir, ancak uyarıyı el ile kapatmanız gerekir.

<!-- 2368581 - IS, ASDK --> 
- Düşük bellek uyarısı alırsanız ve Kiracı sanal makinelerini başarısız ile dağıtmak bir Azure Stack operatörü bir **Fabric VM oluşturma hatası**, Azure Stack damga kullanılabilir bellek yetersiz olabilir. Kullanım [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) iş yükleriniz için kapasite en iyi anlamak için.

### <a name="compute"></a>İşlem

<!-- 3235634 – IS, ASDK -->
- Vm'leri içeren boyutları ile dağıtmak için bir **v2** soneki; Örneğin, **işler için standart_a2_v2**, sonek olarak lütfen **işler için standart_a2_v2** (küçük harf v). Kullanmayın **işler için standart_a2_v2** (Büyük Harf V). Bu genel Azure'da çalışır ve Azure Stack'te bir tutarsızlık olduğunu.

<!-- 3099544 – IS, ASDK --> 
- Azure Stack portalını kullanarak bir yeni sanal makine (VM) oluşturun ve VM boyutu seçin, ABD Doları/ay sütun içeren görüntülenir bir **kullanılamıyor** ileti. Bu sütun görünmemelidir; VM görüntüleme fiyatlandırma sütunu Azure Stack'te desteklenmiyor.

<!-- 2869209 – IS, ASDK --> 
- Kullanırken [ **Ekle AzsPlatformImage** cmdlet'i](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), kullanmalısınız **- OsUri** parametre olarak depolama hesabı URI'si disk nereye yüklenir. Yerel yol diskin kullanırsanız, cmdlet şu hatayla başarısız olur: *işlemi uzun süre çalışan 'Başarısız' durumuyla başarısız oldu*. 

<!--  2795678 – IS, ASDK --> 
- VM, portalı sanal makineler (VM) oluşturmak için bir premium VM boyutu (DS, Ds_v2, FS, FSv2) kullandığınızda, bir standart depolama hesabı oluşturulur. Bir standart depolama hesabı oluşturma IOPS, işlevsel olarak, etkilemez ya da fatura. 

   Bildiren bir uyarıyı güvenle yok sayabilirsiniz: *premium diskleri destekleyen bir boyutta standart disk kullanmayı seçtiniz. Bu işletim sisteminin performansını etkileyebilir ve önerilmez. Premium depolamayı (SSD) kullanmayı düşünün.*

<!-- 2967447 - IS, ASDK --> 
- Dağıtım için bir seçenek olarak, 7.2 CentOS tabanlı sanal makine ölçek kümesi (VMSS) oluşturma deneyimi sağlar. Bu görüntüyü Azure Stack üzerinde kullanılabilir olmadığından, dağıtımınız için başka bir işletim sistemi seçin veya Market'ten dağıtımdan işleciyle indirildi başka bir CentOS görüntüsü belirten bir Azure Resource Manager şablonu kullanın.  

<!-- 2724873 - IS --> 
- PowerShell cmdlet'lerini kullanırken **başlangıç AzsScaleUnitNode** veya **Stop-AzsScaleunitNode** ölçek birimleri yönetmek için başlatma veya durdurma ölçek birimi için yapılan ilk girişim başarısız olabilir. İlk çalıştırılmasında cmdlet'i başarısız olursa, cmdlet'in ikinci kez çalıştırın. İkinci çalıştırma işlemi tamamlamak için başarılı olmalıdır. 

<!-- TBD - IS ASDK --> 
- VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

<!-- 1662991 IS ASDK --> 
- Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur.  

<!-- 2724961- IS ASDK --> 
- Kaydettiğinizde **Microsoft.Insight** abonelik ayarlarını, kaynak sağlayıcısındaki bir Windows VM oluşturma ve konuk işletim sistemi etkin Tanılama ile CPU yüzdesi grafiğin VM genel bakış sayfasında ölçüm verilerini da göstermez.

   Sanal makine için CPU yüzdesi grafik gibi ölçüm verileri bulmak için ölçümleri penceresine gidin ve tüm desteklenen Windows VM Konuk ölçümleri göster.

<!-- 3507629 - IS, ASDK --> 
- Yönetilen diskler oluşturur iki yeni [kota türleri işlem](azure-stack-quota-types.md#compute-quota-types) sağlanabilir yönetilen diskler hizmetin maksimum kapasitesi sınırlamak için. Varsayılan olarak, 2048 GiB her yönetilen diskler kota türü için ayrılır. Ancak, aşağıdaki sorunlarla karşılaşabilirsiniz:

   - 2048 GiB ayrılır ancak 1808 güncelleştirmeden önce oluşturulan kotalarını 0 değerleri yönetilen diskler kotası yönetici Portalı'nda gösterir. Gerçek gereksinimlerinize ve yeni belirlenen göre değerini azaltın veya artırabilirsiniz kota değeri, 2048 GiB varsayılan'ı geçersiz kılar.
   - Kota değeri 0'a güncelleştirin, 2048 GiB varsayılan değerini eşdeğer olacaktır. Geçici çözüm olarak, kota değeri 1 olarak ayarlayın.

<!-- TBD - IS ASDK --> Güncelleştirme 1809 uyguladıktan sonra yönetilen disklere sahip VM'ler dağıtırken aşağıdaki sorunlarla karşılaşabilirsiniz:

   - Yönetilen disklerle bir VM dağıtma 1808 güncelleştirmeden önce Abonelik oluşturulurken bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için her abonelik için şu adımları izleyin:
      1. Kiracı Portalı'nda Git **abonelikleri** ve aboneliği bulunamıyor. Tıklayın **kaynak sağlayıcıları**, ardından **Microsoft.Compute**ve ardından **yeniden kaydettirin**.
      2. Aynı abonelik altında Git **erişim denetimi (IAM)**, doğrulayın **Azure Stack – yönetilen Disk** listelenir.
   2. Bir konuk dizin ile ilişkili bir abonelik içindeki Vm'leri dağıtma, çok kiracılı bir ortam yapılandırdıysanız, bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için aşağıdaki adımları izleyin. [bu makalede](azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) her Konuk dizinlerinizi yeniden yapılandırmak için.

### <a name="networking"></a>Ağ  

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


<!-- ### SQL and MySQL-->


### <a name="app-service"></a>App Service

<!-- 2352906 - IS ASDK --> 
- Kullanıcılar, bunlar, ilk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.


### <a name="usage"></a>Kullanım  

<!-- TBD - IS ASDK --> 
- Aynı genel IP adresi kullanım ölçüm verileri gösterilir *olay tarihi-saati* yerine her kaydın değerini *TimeDate* gösteren kaydın oluşturulduğu zaman damgası. Şu anda, genel IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>Güncelleştirmeyi indirin

Azure Stack 1809 güncelleştirme paketinden indirebileceğiniz [burada](https://aka.ms/azurestackupdatedownload).
  

## <a name="next-steps"></a>Sonraki adımlar

- Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere bakım ilkeyi gözden geçirmek için bkz: [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).  
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).  
- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).  
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).  
