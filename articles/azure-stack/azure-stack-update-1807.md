---
title: Azure Stack 1807 güncelleştirme | Microsoft Docs
description: Bilinen sorunlar da dahil olmak üzere Azure Stack tümleşik sistemleri için 1807 güncelleştirmesindeki yenilikler ve güncelleştirme karşıdan yükleme konumu hakkında bilgi edinin.
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
ms.date: 09/26/2018
ms.author: sethm
ms.reviewer: justini
ms.openlocfilehash: bde8ea74d2b6d36cd070598d0542e5be4bdef244
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816183"
---
# <a name="azure-stack-1807-update"></a>Azure Stack 1807 güncelleştirme

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Bu makalede 1807 güncelleştirme paketinin içeriğini açıklar. Bu güncelleştirme, Azure Stack nerede güncelleştirmeyi indirin ve bu sürüm için bilinen sorunlar geliştirmeleri ve düzeltmeleri içerir. Bilinen sorunlar için güncelleştirme işlemini doğrudan ilgili sorunları ve derleme (yükleme sonrası) ile ilgili sorunlar ayrılır.

> [!IMPORTANT]  
> Yalnızca Azure Stack tümleşik sistemleri için bu güncelleştirme paketidir. Bu güncelleştirme paketi için Azure Stack geliştirme Seti'ni geçerli değildir.

## <a name="build-reference"></a>Yapı Başvurusu

Azure Stack 1807 güncelleştirmenin yapı numarasıdır **1.1807.0.76**.  

### <a name="new-features"></a>Yeni Özellikler

Bu güncelleştirme Azure Stack için aşağıdaki geliştirmeleri içerir.

<!-- 1658937 | ASDK, IS --> 
- **Önceden tanımlanmış bir zamanlamaya göre yedeklemeleri başlatmak** -gereçlerden biri Azure Stack artık otomatik olarak düzenli aralıklarla insan müdahalesi ortadan kaldırmak için altyapı yedekleme tetikleyebilirsiniz. Azure Stack tanımlanan saklama süresinden daha eski yedeklemeler için dış paylaşımı da otomatik olarak temizler. Daha fazla bilgi için [PowerShell ile Azure Stack için yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-powershell.md).

<!-- 2496385 | ASDK, IS -->  
- **Eklenen veri aktarımı için toplam yedek zamanına zaman.** Daha fazla bilgi için [PowerShell ile Azure Stack için yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-powershell.md).

<!-- 1702130 | ASDK, IS -->  
- **Yedekleme dış kapasite artık dış paylaşım doğru kapasitesini gösterir.** (Daha önce bu kodu sabit-10 GB.) Daha fazla bilgi için [PowerShell ile Azure Stack için yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-powershell.md).

<!-- 2508488 |  IS   -->
- **Kapasiteyi genişletmeniz** tarafından [ek ölçek birimi düğüm ekleme](azure-stack-add-scale-node.md).

<!-- 2753130 |  IS, ASDK   -->  
- **Azure Resource Manager şablonları artık destek koşulu öğesi** -artık bir kaynak bir koşulunu kullanarak bir Azure Kaynak Yöneticisi şablonu olarak dağıtabilirsiniz. Bir parametre değeri varsa, değerlendirme gibi bir koşula bağlı olarak kaynak dağıtma için şablonunuzu tasarlayabilirsiniz. Bir koşul olarak bir şablon kullanma hakkında daha fazla bilgi için bkz: [koşullu olarak kaynak dağıtma](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/conditional-deploy) ve [değişkenler bölümü Azure Resource Manager şablonlarının](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-templates-variables) Azure belgelerinde. 

   Şablonlar için de kullanabilirsiniz [birden fazla abonelik veya kaynak grubu için kaynakları dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-cross-resource-group-deployment).  

<!--2753073 | IS, ASDK -->  
- **Microsoft.Network API kaynak sürüm desteği güncelleştirildi** API Sürüm 2017-10-01 2015-06-15 arasında Azure Stack ağ kaynakları için desteği eklenecek.  Bu sürümde 2017-10-01 2015-06-15 arasındaki kaynak sürümleri desteği dahil edilmez.  Lütfen [Azure Stack ağ iletişimi için Değerlendirmeler](user/azure-stack-network-differences.md) işlev farklılıkları için.

<!-- 2272116 | IS, ASDK   -->  
- **Azure Stack, harici olarak Azure Stack altyapı uç noktalarına yönelik için ters DNS araması için destek ekledi** (değil portal, adminportal, yönetim ve adminmanagement için). Bu, bir IP adresinden çözülmesi Azure Stack dış uç nokta adları sağlar.

<!-- 2780899 |  IS, ASDK   --> 
- **Azure Stack artık mevcut bir VM'ye ek ağ arabirimi eklenmesine izin verir.**  Bu işlev, portal, PowerShell ve CLI kullanarak kullanılabilir. Daha fazla bilgi için [ekleme veya kaldırma ağ arabirimleri](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-vm) Azure belgeleri. 

<!-- 2222444 | IS, ASDK   -->  
- **Kullanım ölçümleri ağ doğruluk ve dayanıklılık geliştirmeleri yapıldı**.  Ağ kullanım ölçümleri, artık daha doğru ve hesabı askıya alınmış Abonelikleri, kesinti süreleri ve yarış durumları alın.

<!-- 2753080 | IS -->  
- **Kullanılabilir bildirim güncelleştirin.** Azure dağıtımlar artık düzenli olarak güvenli bir uç nokta denetler ve bulut için bir güncelleştirme olup olmadığını belirlemek Stack bağlı. El ile denetleme ve yeni bir güncelleştirme içeri aktarma sonrasında olduğu gibi bu bildirim güncelleştirme kutucuğunda görünür. Daha fazla bilgi edinin [Azure Stack için güncelleştirmeleri yönetme](azure-stack-updates.md).

<!-- 2297790 | IS, ASDK -->  
- **Azure Stack syslog istemciye (Önizleme özelliği) geliştirmeleri**. Bu istemci, Denetim ve Azure Stack altyapısının bir syslog sunucusu veya güvenlik bilgileri ve Olay yönetimi (SIEM) yazılım Azure Stack'e dış ilgili günlükleri iletilmesini sağlar. Syslog istemci, artık düz metin veya varsayılan yapılandırmayı ikinci olan, TLS 1.2 şifrelemeyi TCP protokolünü destekler. TLS bağlantısını yalnızca sunucu ya da karşılıklı kimlik doğrulaması ile yapılandırabilirsiniz.

  Nasıl syslog istemci (protokol, şifreleme ve kimlik doğrulaması) syslog sunucusu ile iletişim kurar yapılandırmak için kullanın *kümesi SyslogServer* cmdlet'i. Bu cmdlet, ayrıcalıklı uç noktasından (CESARETLENDİRİCİ) kullanılabilir.

  Syslog istemci TLS 1.2 karşılıklı kimlik doğrulaması için istemci tarafı sertifika eklemek için CESARETLENDİRİCİ kümesi SyslogClient cmdlet'ini kullanın.

  Bu önizleme ile çok fazla sayıda denetimleri ve uyarılar görebilirsiniz. 

  Bu özellik hala Önizleme aşamasında olduğundan, üzerinde üretim ortamlarına güvenmeyin.

  Daha fazla bilgi için [Azure Stack syslog iletmeyi](azure-stack-integrate-security.md).

<!-- ####### | IS, ASDK | --> 
- **Azure Resource Manager, bölge adını içerir.** Bu sürümle birlikte, Azure Resource Manager'dan alınan nesneler artık bölge adı özniteliği içerir. Varolan bir PowerShell komut dosyasını doğrudan nesne başka bir cmdlet'e geçerse, betik hataya neden ve başarısız. Bu, Azure Resource Manager uyumlu davranıştır ve bölge öznitelik çıkarılacak çağıran istemci gerektirir. Azure Resource Manager bakın hakkında daha fazla bilgi için [Azure Resource Manager belgeleri](https://docs.microsoft.com/azure/azure-resource-manager/). 8-10 mdb--> doğrulayın

<!-- TBD | IS, ASDK -->  
- **Sağlayıcı temsilcisi işlevlerindeki değişiklikler.** Temsilci modeli için daha iyi basitleştirilmiştir sağlayıcıları 1807 ile başlayarak, Azure satıcısı modeliyle uyumlu hale getirmek ve temsilci sağlayıcıları diğer temsilci temelde model düzleştirme ve temsilci sağlayıcısı yapmadan sağlayıcıları oluşturmak mümkün olmayacaktır tek bir düzeyde özelliği kullanılabilir. Yeni model ve abonelik yönetimi geçişi etkinleştirmek için kullanıcı aboneliklerini aynı dizin kiracısına ait yeni veya var olan bir temsilci sağlayıcısı abonelikler arasında artık taşınabilir. Kullanıcı abonelikleri varsayılan sağlayıcı aboneliğine ait de aynı dizin kiracısında temsilci sağlayıcı aboneliği için taşınabilir.  Daha fazla bilgi için [temsilci sunan Azure Stack'te](azure-stack-delegated-provider.md).

<!-- 2536808 IS ASDK --> 
- **VM oluşturma zamanı geliştirilmiş** görüntüleri Azure Market'te indir ile oluşturulan sanal makineleri için.

<!-- TBD | IS, ASDK -->  
- **Azure Stack Capacity Planner kullanılabilirlik geliştirmeleri**. Azure Stack [Capacity Planner](http://aka.ms/azstackcapacityplanner) artık S2D önbelleğini ve S2D kapasite SKU'ları çözüm tanımlarken, girişini yaparak için basitleştirilmiş bir deneyim sunar. 1000 VM sınırı kaldırıldı.


### <a name="fixed-issues"></a>Düzeltilen sorunlar

<!-- TBD | ASDK, IS --> 
- Çeşitli iyileştirmeler, daha güvenilir hale getirmek için güncelleştirme işlemi için yapılmıştır. Ayrıca, hangi, güncelleştirme sırasında olan iş yükleri için olası kapalı kalma süresini düzeltmeleri, temel alınan altyapıya'e yapıldı.

<!--2292271 | ASDK, IS --> 
- Burada değiştirilmiş bir kota sınırı mevcut aboneliklerin uygulamadı sorununu düzelttik. Artık, bir teklif parçası olan bir ağ kaynağı için bir kota sınırı artırmak ve kullanıcı abonelikle planı, önceden mevcut abonelikler, aynı zamanda için yeni abonelikler yeni sınır geçerlidir.

<!-- 448955 | IS ASDK --> 
- Etkinlik günlükleri bir UTC + N saat diliminde dağıtılan sistemler için artık başarılı bir şekilde sorgulayabilirsiniz.    

<!-- 2319627 |  ASDK, IS --> 
- Yedekleme yapılandırması parametreleri (yol/kullanıcı adı/parola/şifreleme anahtarı) için ön denetim, artık için yedekleme yapılandırması ayarları yanlış ayarlar. (Daha önce yedeklemeyi ayarları yanlış ayarlanmış ve yedekleme ardından tetiklendiğinde başarısız olur.)

<!-- 2215948 |  ASDK, IS -->
- Dış paylaşımından el ile yedekleme sildiğinizde yedekleme listesinde artık yeniler.

<!-- 2332636 | IS -->  
- Bu sürüm için güncelleştirme artık varsayılan sağlayıcı aboneliği varsayılan sahibi yerleşik olarak sıfırlar **CloudAdmin** AD FS ile dağıttığınızda kullanıcı.

<!-- 2360715 |  ASDK, IS -->  
- Bundan sonra veri merkezi tümleştirmeyi ayarladığınızda, AD FS meta veri dosyası bir Azure dosya paylaşımından erişemezsiniz. Daha fazla bilgi için [Federasyon meta veri dosyası sağlayarak AD FS tümleştirmenin ayarlanması](azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

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

- **Çeşitli düzeltmeleri** performans, kararlılık, güvenlik ve Azure Stack tarafından kullanılan işletim sistemi için.


<!-- ### Additional releases timed with this update    -->

### <a name="common-vulnerabilities-and-exposures"></a>Yaygın güvenlik açıklarına ve exposures'ı
Azure Stack, Windows Server 2016 Sunucu Çekirdeği yüklemeleri için ana anahtar altyapısı kullanır. Bu sürüm, Azure Stack için altyapı sunucularında aşağıdaki Windows Server 2016 güncelleştirmeleri yükler: 
- [CVE-2018-8206](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8206)
- [CVE-2018-8222](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8222)
- [CVE-2018-8282](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8282)
- [CVE-2018-8304](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8304)
- [CVE-2018-8307](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8307)
- [CVE-2018-8308](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8308) 
- [CVE-2018-8309](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8309)
- [CVE-2018-8313](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8313)  

Bu güvenlik açıkları hakkında daha fazla bilgi için yukarıdaki bağlantılara tıklayın veya Microsoft Bilgi Bankası makalelerine bakın [4338814](https://support.microsoft.com/help/4338814) ve [4345418](https://support.microsoft.com/help/4345418).



## <a name="before-you-begin"></a>Başlamadan önce

### <a name="prerequisites"></a>Önkoşullar

- Azure yığını'nı yükleme [1805 güncelleştirme](azure-stack-update-1805.md) Azure Stack 1807 güncelleştirmeyi uygulamadan önce.  Hiçbir güncelleştirme 1806 vardı.  

- En son kullanılabilir yükleme [güncelleştirme veya düzeltme sürümü 1805](azure-stack-update-1805.md#post-update-steps).  
  > [!TIP]  
  > Aşağıdaki abone olmak *RRS* veya *Atom* Azure Stack düzeltmelerle birlikte kalmasını sağlamak için akışları:
  > - RRS: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss ... 
  > - Atom: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom ...


- Bu güncelleştirme yüklemesi başlamadan önce çalıştırması [Test AzureStack](azure-stack-diagnostic-test.md) bulunan tüm çalışma sorunlarını çözün ve Azure Stack durumunu doğrulamak için aşağıdaki parametreleri, tüm uyarılar ve hatalar dahil olmak üzere. Ayrıca etkin Uyarıları gözden geçirin ve eylemi gerektiren tüm çözümleyin.  

  ```PowerShell
  Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
  ``` 

### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar

<!-- 2468613 - IS --> 
- Bu güncelleştirme yüklemesi sırasında başlık uyarılarla görebileceğiniz *hatası – şablon FaultType UserAccounts.New için eksik.*  Bu uyarılar güvenle yok sayabilirsiniz. Bu güncelleştirme yüklemesi tamamlandıktan sonra bu uyarıları otomatik olarak kapatılacak.

<!-- 2489559 - IS --> 
- Bu güncelleştirme yüklemesi sırasında sanal makineleri oluşturmaya çalışmayın. Güncelleştirmeleri yönetme hakkında daha fazla bilgi için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md#plan-for-updates).

<!-- 2830461 - IS --> 
- Bazı durumlarda, bir güncelleştirme dikkat gerektirdiğinde karşılık gelen bir uyarı oluşturulamayabilir. Doğru durumu Portalı'nda yine de yansıtılır ve etkilenmez.

### <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar
Bu güncelleştirme yüklendikten sonra geçerli düzeltmeleri yükleyin. Daha fazla bilgi için aşağıdaki Bilgi Bankası makaleleri görüntülemek hem de bizim [hizmet İlkesi](azure-stack-servicing-policy.md). 
- [KB 4464231 – Azure Stack düzeltme Azure Stack düzeltme 1.1807.1.78](https://support.microsoft.com/help/4464231)

<!-- 2933866 – IS --> Bu güncelleştirme yüklendikten sonra gördüğünüz **geliştirilmiş başarısız güncelleştirme yüklemelerini durumu.** Bu iki yeni durum kategorisi yansıtacak şekilde düzenlendi önceki güncelleştirme yükleme hatalarıyla ilgili bilgiler içerebilir. Yeni durum kategorilerinin *PreparationFailed*, ve *InstallationFailed*.  

## <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

Bu derleme sürümü için yükleme sonrası bilinen sorunlar verilmiştir.

### <a name="portal"></a>Portal

- Azure Stack teknik belgeleri, en son sürüm üzerinde odaklanır. Sürümler arasında Portal değişiklikleri nedeniyle Azure Stack portalı kullanırken gördüğünüz belgelerinde gördüğünüz verilerden değişebilir. 

- Yeteneği [açılan listeden yeni bir destek isteği açın](azure-stack-manage-portals.md#quick-access-to-help-and-support) içinde Yönetici portalı kullanılamıyor. Bunun yerine, için Azure Stack tümleşik sistemleri, aşağıdaki bağlantıyı kullanın: [ https://aka.ms/newsupportrequest ](https://aka.ms/newsupportrequest).

<!-- 2931230 – IS  ASDK --> 
- Kullanıcı aboneliği plan kaldırdığınızda bile, bir kullanıcı abonelikte eklenti planı eklendiği planları silinemiyor. Eklenti planı başvuru abonelikleri de silinene kadar plan kalır. 

<!--2760466 – IS  ASDK --> 
- Bu sürümünü çalıştıran yeni bir Azure Stack ortamına yüklediğinizde, uyarıyı gösterir *etkinleştirme gerekli* görüntülenmeyebilir. [Etkinleştirme](azure-stack-registration.md) Market dağıtım kullanabilmeniz için gereklidir.  

<!-- TBD - IS ASDK --> 
- İki Yönetim abonelik türlerini [1804 sürümü ile sunulan](azure-stack-update-1804.md#new-features) kullanılmamalıdır. Abonelik türleridir **abonelik ölçümü**, ve **tüketim abonelik**. Bu abonelik türlerini 1804 sürümünden başlayarak yeni Azure Stack ortamlarında görülebilir ancak henüz kullanıma sunulmamıştır. Kullanmaya devam etmelidir **varsayılan sağlayıcı** abonelik türü.

<!-- 2403291 - IS ASDK --> 
- Yatay kaydırma çubuğunun alt kısmında bulunan yönetici ve Kullanıcı Portalı'nın kullanımını olmayabilir. Yatay kaydırma çubuğunun erişemiyorsanız, önceki bir dikey pencere portaldaki dikey penceresinin adı seçerek, gitmek için içerik haritaları en üstünde bulunan içerik haritası listeden görüntülemek istediğiniz kullanım portalın sol.

  ![İçerik haritası](media/azure-stack-update-1804/breadcrumb.png)

<!-- TBD - IS --> 
- Yönetici portalı'nda işlem ve depolama kaynaklarını görüntülemek mümkün olmayabilir. Bu sorunun nedeni, yanlış başarılı olarak raporlanacak güncelleştirilecek neden olan güncelleştirme yüklemesi sırasında bir hata var. Bu sorun devam ederse Yardım için Microsoft Müşteri Destek Hizmetleri'ne başvurun.

<!-- TBD - IS --> 
- Bir boş Pano portalında görebilirsiniz. Pano kurtarmak için portalın sağ üst köşesindeki dişli simgesini seçin ve ardından **varsayılan ayarları geri**.

<!-- TBD - IS ASDK --> 
- Kullanıcı abonelikleri sonuçlarında yalnız bırakılmış kaynakları siliniyor. Geçici bir çözüm olarak kullanıcı kaynaklar veya kaynak grubunun tamamını silin ve sonra kullanıcı abonelikleri silin.

<!-- TBD - IS ASDK --> 
- Azure Stack portalı kullanarak aboneliğinize izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.


### <a name="health-and-monitoring"></a>Sistem durumu ve izleme
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

<!-- 2368581 - IS. ASDK --> 
- Düşük bellek uyarısı alırsanız ve Kiracı sanal makinelerini başarısız ile dağıtmak bir Azure Stack operatörü bir **Fabric VM oluşturma hatası**, Azure Stack damga kullanılabilir bellek yetersiz olabilir. Kullanım [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) iş yükleriniz için kapasite en iyi anlamak için.


### <a name="compute"></a>İşlem

<!-- 2724873 - IS --> 
- PowerShell cmdlet'lerini kullanırken **başlangıç AzsScaleUnitNode** veya **Stop-AzsScaleunitNode** ölçek birimleri yönetmek için başlatma veya durdurma ölçek birimi için yapılan ilk girişim başarısız olabilir. İlk çalıştırılmasında cmdlet'i başarısız olursa, cmdlet'in ikinci kez çalıştırın. İkinci çalıştırma işlemi tamamlamak için başarılı olmalıdır. 

<!-- 2494144 - IS, ASDK --> 
- Bir sanal makine dağıtımı için bir sanal makine boyutu seçerken, bazı F serisi VM boyutlarının görünür olmayan bir VM oluşturduğunuzda boyut seçici bir parçası olarak. Aşağıdaki VM boyutları seçicide görünmez: *F8s_v2*, *F16s_v2*, *F32s_v2*, ve *F64s_v2*.  
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


<!-- TBD - IS ASDK --> 
- Ölçeklendirme ayarları sanal makine ölçek kümeleri için portalda kullanılabilir değildir. Geçici çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürüm farklar nedeniyle, kullanmanız gereken `-Name` yerine parametre `-VMScaleSetName`.

<!-- TBD - IS --> 
- Portaldaki giderek bir kullanılabilirlik kümesi oluşturduğunuzda, **yeni** > **işlem** > **kullanılabilirlik kümesi**, oluşturabilmeniz için bir bir hata etki alanı ve 1 güncelleştirme etki alanı ile kullanılabilirlik kümesi. Yeni bir sanal makine oluştururken bir geçici çözüm olarak, PowerShell, CLI kullanarak veya içinden kullanılabilirlik oluşturma portalı.

<!-- TBD - IS ASDK --> 
- Portal, Azure Stack kullanıcı portalında sanal makine oluşturduğunuzda, DS serisi VM ekleyebilirsiniz veri diskleri yanlış sayıda görüntüler. DS serisi VM'ler gibi çok sayıda veri diski Azure yapılandırması sağlayabilir.

<!-- TBD - IS ASDK --> 
- VM dağıtımı üzerinde bir uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemini durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermelisiniz.  

<!-- 1662991 IS ASDK --> 
- Linux VM tanılama Azure Stack'te desteklenmiyor. VM tanılaması etkin bir Linux sanal makinesi dağıttığınızda, dağıtım başarısız olur. Tanılama ayarları aracılığıyla Linux VM temel ölçümleri etkinleştirirseniz, ayrıca dağıtım başarısız olur.  

<!-- 2724961- IS ASDK --> 
- Kaydettiğinizde **Microsoft.Insight** abonelik ayarlarını, kaynak sağlayıcısındaki bir Windows VM oluşturma ve konuk işletim sistemi etkin Tanılama ile VM genel bakış sayfasında ölçüm verilerini da göstermez. 

   Sanal makine için CPU yüzdesi grafik gibi ölçüm verileri bulmak için Git **ölçümleri** dikey penceresinde ve desteklenen tüm Windows VM show Konuk ölçümleri.

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


### <a name="sql-and-mysql"></a>SQL ve MySQL



<!-- No Number - IS, ASDK -->  
- Özel karakterler, boşluk ve nokta dahil olmak üzere desteklenmez **ailesi** adı, SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda. 

<!-- TBD - IS --> 
- Yalnızca kaynak sağlayıcısı, ana bilgisayar SQL veya MySQL sunucuları üzerinde öğeleri oluşturmak için desteklenir. Kaynak sağlayıcısı tarafından oluşturulmamış bir ana bilgisayar sunucusunda oluşturulan öğeler, eşleşmeyen bir duruma neden olabilir.  

<!-- TBD - IS -->
> [!NOTE]   
> Bu Azure Stack sürümüne güncelleştirdikten sonra daha önce dağıttığınız SQL ve MySQL kaynak sağlayıcıları kullanmaya devam edebilirsiniz.  Yeni bir sürüm kullanıma sunulduğunda SQL ve MySQL güncelleştirme öneririz. Azure Stack gibi güncelleştirmeleri sırayla SQL ve MySQL kaynak sağlayıcıları için geçerlidir. Örneğin, 1804. sürümünü kullanıyorsanız, önce sürüm 1805 uygulayın ve ardından 1807 için güncelleştirin.  
>  
> Bu güncelleştirme yüklemesi, SQL veya MySQL kaynak sağlayıcıları kullanıcılarınız tarafından geçerli kullanımını etkilemez. 
> Kullandığınız kaynak sağlayıcıları sürümünden bağımsız olarak kullanıcılar verilerinizi veritabanlarındaki bilgiler değildir ve erişilebilir kalır.  

### <a name="app-service"></a>App Service

<!-- 2352906 - IS ASDK --> 
- Kullanıcılar, bunlar, ilk Azure işlevinizi aboneliği oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

<!-- 2489178 - IS ASDK --> 
- (Çalışan, yönetim, ön uç rollerini) altyapıyı ölçeklendirme için sürüm notları için işlem açıklandığı PowerShell kullanmanız gerekir.

<!-- TBD - IS ASDK --> 
- App Service yalnızca dağıtılabilir içine *varsayılan sağlayıcı aboneliği* şu anda. 


### <a name="usage"></a>Kullanım  
<!-- TBD - IS ASDK --> 
- Kullanım genel IP adresi kullanım ölçümü verilerini gösterir aynı *olay tarihi-saati* yerine her kaydın değerini *TimeDate* gösteren kaydın oluşturulduğu zaman damgası. Şu anda, genel IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>Güncelleştirmeyi indirin
Azure Stack 1807 güncelleştirme paketinden indirebileceğiniz [burada](https://aka.ms/azurestackupdatedownload).


## <a name="next-steps"></a>Sonraki adımlar
- Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere bakım ilkeyi gözden geçirmek için bkz: [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).  
- Ayrıcalıklı uç noktasına (CESARETLENDİRİCİ) izlemek ve güncelleştirmelerini sürdürmek üzere kullanmak için bkz [izleme ayrıcalıklı uç noktayı kullanarak Azure stack'teki güncelleştirmeleri](azure-stack-monitor-update.md).  
- Azure Stack'te güncelleştirme yönetimi genel bakış için bkz. [Azure Stack genel bakış güncelleştirmeleri yönetme](azure-stack-updates.md).  
- Azure Stack güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz. [güncelleştirmelerini Azure Stack'te](azure-stack-apply-updates.md).  
