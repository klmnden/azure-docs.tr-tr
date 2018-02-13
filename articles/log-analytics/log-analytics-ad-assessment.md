---
title: "Active Directory ortamınızı Azure günlük analizi ile en iyi duruma getirme | Microsoft Docs"
description: "Düzenli bir aralıkta risk ve ortamlarınızın durumunu değerlendirmek için Active Directory sistem durumu denetimi çözüm kullanabilirsiniz."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 81eb41b8-eb62-4eb2-9f7b-fde5c89c9b47
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f026c605b84c5f2b6420e975a06d7c02227efbd9
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="optimize-your-active-directory-environment-with-the-active-directory-health-check-solution-in-log-analytics"></a>Active Directory ortamınızı günlük analizi Active Directory sistem durumu denetimi çözümde ile en iyi duruma getirme

![AD sistem durumu denetimi simgesi](./media/log-analytics-ad-assessment/ad-assessment-symbol.png)

Düzenli bir aralıkta risk ve sunucu ortamlarınızın durumunu değerlendirmek için Active Directory sistem durumu denetimi çözüm kullanabilirsiniz. Bu makalede, yükleme ve olası sorunlar için düzeltme eylemleri yararlanabilmeniz çözüm kullanma yardımcı olur.

Bu çözüm önerileri dağıtılan sunucu altyapınızı belirli öncelikli listesi sağlar. Öneriler dört arasında ayrılır hızlı bir şekilde Yardım odak alanlarına riski anlamak ve gerekli adımları uygulayın.

Öneriler bilgi ve müşteri ziyaretleriniz binlerce Microsoft mühendisleri tarafından elde edilen deneyimlerden dayanır. Her bir öneri, bir sorun için neden önemli ve önerilen değişiklikleri uygulamak nasıl hakkında rehberlik sağlar.

Kuruluşunuz için en önemli ve ücretsiz ve sağlam bir risk ortam çalıştıran doğru ilerleme durumunuzu izlemenize odak alanlarına seçebilirsiniz.

Çözüm ekledik ve onay tamamlanan, Özet sonra odak alanlarına odaklanmak için bilgi gösterilir **AD sistem durumu denetimi** Panosu ortamınızdaki altyapısı için. Aşağıdaki bölümlerde bilgileri üzerinde nasıl kullanacağınızı **AD sistem durumu denetimi** Pano, burada görüntüleyebilir ve ardından uygulayın önerilen eylemler için Active Directory sunucu altyapınızı.  

![Döşeme AD sistem durumu denetimi görüntüsü](./media/log-analytics-ad-assessment/ad-healthcheck-summary-tile.png)

![Pano AD sistem durumu denetimi görüntüsü](./media/log-analytics-ad-assessment/ad-healthcheck-dashboard-01.png)

## <a name="prerequisites"></a>Önkoşullar

* Active Directory sistem durumu denetimi çözüm .NET Framework 4.5.2 desteklenen bir sürümünü gerektirir veya yukarıdaki Microsoft İzleme Aracısı (yüklenmiş MMA) olan her bilgisayarda yüklü.  MMA Aracısı System Center 2016 - Operations Manager ve Operations Manager 2012 R2 ile günlük analizi hizmeti tarafından kullanılır.
* Çözüm, Windows Server 2008 ve 2008 R2, Windows Server 2012 ve 2012 R2 ve Windows Server 2016 çalıştıran etki alanı denetleyicilerini destekler.
* Azure portalında Azure Marketi'nden Active Directory sistem durumu denetimi çözümü eklemek için bir günlük analizi çalışma alanı.  Başka bir yapılandırma işlemi gerekmez.

  > [!NOTE]
  > Çözüm ekledikten sonra aracıları sunucularıyla AdvisorAssessment.exe dosyası eklenir. Yapılandırma verileri okumak ve işlemek için günlük analizi hizmet olarak bulutta sonra gönderilir. Mantığı alınan verilere uygulanır ve bulut hizmeti verilerini kaydeder.
  >
  >

Değerlendirilecek etki alanının üyesi olan etki alanı denetleyicileriniz karşı sistem durumu denetimi gerçekleştirmek için bunlar bir aracı ve aşağıdaki desteklenen yöntemlerden birini kullanarak günlük analizi için bağlantı gerektir:

1. Yükleme [Microsoft İzleme Aracısı'nı (MMA)](log-analytics-windows-agent.md) etki alanı denetleyicisi zaten System Center 2016 - Operations Manager veya Operations Manager 2012 R2 tarafından izleniyorsa değil.
2. System Center 2016 - Operations Manager veya Operations Manager 2012 R2 ile izlenir ve yönetim grubu günlük analizi hizmeti ile tümleşik değil, etki alanı denetleyicisi veri toplamak ve iletmek için günlük analizi ile çok konaklı olabilir Hizmet ve Operations Manager tarafından yine izlenmelidir.  
3. Operations Manager yönetim grubunuzu hizmeti ile tümleşik çalışıyorsa, aksi takdirde, veri toplama için etki alanı denetleyicileri altındaki adımları izleyerek hizmeti tarafından eklemeniz gerekir [aracıyla yönetilen bilgisayarlar eklemek](log-analytics-om-agents.md#connecting-operations-manager-to-oms) etkinleştirdikten sonra Çalışma alanınızı çözümde.  

Bir Operations Manager yönetim grubu için hangi raporların toplar, etki alanı denetleyicinizde Aracısı kendi atanmış yönetim sunucusuna iletir ve doğrudan yönetim sunucusundan günlük analizi hizmetine gönderilir.  Verileri Operations Manager veritabanları yazılmaz.  

## <a name="active-directory-health-check-data-collection-details"></a>Active Directory sistem durumu denetimi veri toplama ayrıntıları

Active Directory sistem durumu denetimi etkinleştirdiğiniz aracısını kullanarak aşağıdaki kaynaklardan toplar:

- Kayıt Defteri
- LDAP
- .NET Framework
- Olay günlüğü
- Active Directory Hizmeti Arabirimleri (ADSI)
- Windows PowerShell
- Dosya verileri
- Windows Management Instrumentation (WMI)
- DCDIAG aracı API'si
- Dosya Çoğaltma Hizmeti (NTFRS) API'si
- Özel C# kodu

Veri etki alanı denetleyicisinde toplanır ve günlük analizi için her yedi günde iletilir.  

## <a name="understanding-how-recommendations-are-prioritized"></a>Önerilerin nasıl önceliklendirilir anlama
Yapılan her öneri öneri göreceli önemini tanımlayan bir ağırlıklı değer verilir. Yalnızca 10 en önemli öneriler gösterilir.

### <a name="how-weights-are-calculated"></a>Ağırlıkları nasıl hesaplanır
Ağırlık belirlemeyi üç anahtar faktörlerini temel alarak toplam değerler şunlardır:

* *Olasılık* tanımlanan bir sorunun sorunlara neden olur. Daha yüksek olasılık öneri için daha büyük bir genel puan karşılık gelir.
* *Etkisi* kuruluşunuzdaki bir soruna neden olursa sorun. Daha yüksek bir etkisi öneri için daha büyük bir genel puan karşılık gelir.
* *Çaba* öneriyi uygulamak için gereklidir. Daha yüksek çaba öneri için daha küçük bir genel puan karşılık gelir.

Her öneri ağırlıklı her odak alanı için kullanılabilir toplam puanı yüzdesi olarak ifade edilir. Örneğin, bu öneri uygulama güvenlik ve uyumluluk odak alanında bir öneri %5 puanı varsa, genel güvenlik ve uyumluluk puan tarafından %5 artırır.

### <a name="focus-areas"></a>Odak alanları
**Güvenlik ve Uyumluluk** -bu odak alanı olası güvenlik tehditlerini ve ihlallerinden, şirket ilkelerini ve teknik ve yasal uyumluluk gereksinimleri için öneriler gösterir.

**Kullanılabilirlik ve iş sürekliliği** -bu odaklanılan alan hizmet kullanılabilirliği, altyapı ve iş koruması dayanıklılık için öneriler gösterir.

**Performans ve ölçeklenebilirlik** -bu odak alanı kuruluşunuzun yardımcı olacak öneriler gösterir BT altyapısı arttıkça, BT ortamınız geçerli performans gereksinimlerini karşıladığından ve altyapı değiştirmeye yanıt verebilmesini olduğundan emin olun gerekir.

**Yükseltme, geçiş ve dağıtım** -bu odak alanı, yükseltme, geçirmek ve Active Directory mevcut altyapınızda dağıtmanıza yardımcı olacak öneriler gösterir.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Her odaklanılan alan % 100 puanlı hedefleyin?
Olmayabilir. Öneriler bilgi ve müşteri ziyaretleriniz binlerce arasında Microsoft mühendisleri tarafından elde edilen deneyimleri temel alır. Ancak, iki sunucu altyapılar aynıdır ve özel öneriler için daha az veya uygun olabilir. Örneğin, bazı güvenlik önerileri sanal makinelerinizi Internet'e açık değildir, daha az ilgili olabilir. Bazı kullanılabilirlik öneriler düşük öncelik geçici veri toplama ve raporlama sağladığı hizmetler için daha az uygun olmayabilir. Olgun bir iş için önemli olan sorunları bir başlangıcından daha az önemli olabilir. Hangi odak alanların önceliklerinizden olduğunu belirlemek ve, puanları zamanla nasıl değiştiğini adresindeki Ara isteyebilirsiniz.

Her öneri neden önemli olduğu hakkında yönergeler içerir. Öneri uygulama verilen BT hizmetlerinizi yapısını ve kuruluşunuzun iş gereksinimlerini sizin için uygun olup olmadığını değerlendirmek için bu yönergeleri kullanmanız.

## <a name="use-health-check-focus-area-recommendations"></a>Kullanım durumu Denetim odak alanı önerileri
Yüklendikten sonra çözüm sayfasında Azure Portalı'ndaki Sistem durumu denetimi döşeme kullanarak önerileri özetini görüntüleyebilirsiniz.

Altyapınız ve ardından-ayrıntıya önerileri için özetlenmiş uyumluluk değerlendirmesi görüntüleyin.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Odak alanı için öneriler görüntülemek ve düzeltici işlemleri için
3. Tıklatın **genel bakış** döşeme Azure portalında günlük analizi çalışma alanınız için.
4. Üzerinde **genel bakış** sayfasında, **Active Directory sistem durumu denetimi** döşeme.
5. Üzerinde **sistem durumu denetimi** sayfasında odak alanı Kanatlar birinde özet bilgilerini inceleyin ve sonra bu odak alanı için öneriler görüntülemek için tıklatın.
6. Odak alanı sayfaları hiçbirinde ortamınız için öncelikli önerilerin görüntüleyebilirsiniz. Önerinin altında tıklatın **etkilenen nesneleri** öneri neden yapılan hakkında ayrıntıları görüntülemek için.<br><br> ![Sistem durumu denetimi önerileri görüntüsü](./media/log-analytics-ad-assessment/ad-healthcheck-dashboard-02.png)
7. Önerilen düzeltici eylemleri gerçekleştirebilirsiniz **önerilen eylemleri**. Öğe ele önerilen eylemleri sonraki değerlendirmeleri kayıtları gerçekleştirilen ve uyumluluk puan artmasına neden olur. Düzeltilmiş öğeler görünür olarak **geçirilen nesneleri**.

## <a name="ignore-recommendations"></a>Öneriler yoksay
Yoksay istediğiniz önerileri varsa, günlük analizi önerileri değerlendirme sonuçlarında görünmesini engellemek için kullanacağı bir metin dosyası oluşturabilirsiniz.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Göz ardı eder önerileri tanımlamak için
1. Günlük analizi çalışma sayfasında seçilen çalışma alanınız için Azure portalında tıklatın **günlük arama** döşeme.
2. Ortamınızdaki bilgisayarları için başarısız olan liste önerileri için aşağıdaki sorguyu kullanın.

    ```
    ADAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation
    ```
    Günlük arama sorgusu gösteren ekran görüntüsü aşağıda verilmiştir:<br><br> ![başarısız önerileri](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)

3. Yoksay istediğiniz önerileri seçin. Sonraki yordamda Recommendationıd için değerleri kullanacaksınız.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Oluşturma ve bir IgnoreRecommendations.txt metin dosyası kullanma
1. IgnoreRecommendations.txt adlı bir dosya oluşturun.
2. Her Recommendationıd ayrı bir satırda yoksay ve sonra dosyayı kaydedip kapatın için günlük analizi istediğiniz her bir öneri için yazın veya yapıştırın.
3. Dosya aşağıdaki klasörde önerileri yoksaymak için günlük analizi istediğiniz her bilgisayara yerleştirin.
   * Microsoft Monitoring (doğrudan veya Operations Manager aracılığıyla bağlı) Agent - olan bilgisayarlarda *SystemDrive*: \Program izleme Agent\Agent
   * Operations Manager 2012 R2 yönetim sunucusundaki - *SystemDrive*: \Program System Center 2012 R2\Operations Manager\Server
   * Operations Manager 2016 yönetim sunucusundaki - *SystemDrive*: \Program System Center 2016\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Öneriler göz ardı edilir doğrulamak için
Varsayılan olarak yedi günde sistem durumu denetimi çalıştığında, sonraki zamanlanmış sonra belirtilen önerileri işaretlenmiş *yoksayıldı* ve Panoda görünmez.

1. Tüm yoksayılan öneriler listelemek için aşağıdaki günlük arama sorgularını kullanabilirsiniz.

    ```
    ADAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation
    ```

2. Daha sonra yoksayılan önerileri görmek istediğiniz karar verirseniz, tüm IgnoreRecommendations.txt dosyaları silin veya bunları RecommendationIDs kaldırabilirsiniz.

## <a name="ad-health-check-solutions-faq"></a>AD sistem durumu denetimi çözümleri hakkında SSS
*Sıklıkla bir sistem durumu denetimi çalışıyor mu?*

* Onay yedi günde bir çalışır.

*Sistem durumu denetimi ne sıklıkta çalıştıran yapılandırmak için yolu var mı?*

* Şu anda değil.

*Bir sistem durumu denetimi çözüm ekledim sonra bulunan için başka bir sunucuya gerçekleştirir denetlenmesi*

* Evet, her yedi günde, ardından gelen işaretlenmesi bulunduktan sonra.

*Ne zaman bir sunucu kullanımdan alındığında sistem durumu denetiminin dışında kaldırılacak?*

* Bir sunucu için 3 hafta veri göndermez, kaldırılır.

*Veri toplama mu işlemin adı nedir?*

* AdvisorAssessment.exe

*Ne kadar süreyle verilerinin toplanmasını sürer?*

* Gerçek veri toplama sunucusundaki yaklaşık 1 saat sürer. Bu çok sayıda Active Directory sunucularını sahip sunucularda uzun sürebilir.

*Verileri toplandığında yapılandırmak için yolu var mı?*

* Şu anda değil.

*Neden yalnızca ilk 10 önerileri görüntülemek?*

* Görevler kapsamlı bir zorlamayı listesi vermek yerine, Önceliklendirilmiş önerileri adresleme odaklanmak öneririz. Bunları çözün sonra ek öneriler kullanılabilir hale gelir. Ayrıntılı listesini görmek isterseniz, tüm önerileri günlük arama özelliğini kullanarak görüntüleyebilirsiniz.

*Bir öneri yoksaymak için yolu var mı?*

* Evet, bkz: [önerileri yoksay](#ignore-recommendations) yukarıdaki bölümde.

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [günlük analizi aramaları oturum](log-analytics-log-searches.md) ayrıntılı veri AD sistem durumunu denetleyin ve önerileri çözümleme hakkında bilgi edinmek için.
