---
title: "Active Directory ortamınızı Azure günlük analizi ile en iyi duruma getirme | Microsoft Docs"
description: "Düzenli bir aralıkta risk ve sunucu ortamlarınızın durumunu değerlendirmek için Active Directory değerlendirme çözümü kullanabilirsiniz."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 81eb41b8-eb62-4eb2-9f7b-fde5c89c9b47
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 97368f0b9e89ffd0cd982b6e8670d5a1f62ad42c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-active-directory-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a>Active Directory ortamınızı günlük analizi Active Directory değerlendirme çözümde ile en iyi duruma getirme

![AD değerlendirmesi simgesi](./media/log-analytics-ad-assessment/ad-assessment-symbol.png)

Düzenli bir aralıkta risk ve sunucu ortamlarınızın durumunu değerlendirmek için Active Directory değerlendirme çözümü kullanabilirsiniz. Bu makalede, yükleme ve olası sorunlar için düzeltme eylemleri yararlanabilmeniz çözüm kullanma yardımcı olur.

Bu çözüm önerileri dağıtılan sunucu altyapınızı belirli öncelikli listesi sağlar. Öneriler dört arasında ayrılır hızlı bir şekilde Yardım odak alanlarına riski anlamak ve gerekli adımları uygulayın.

Öneriler bilgi ve müşteri ziyaretleriniz binlerce Microsoft mühendisleri tarafından elde edilen deneyimlerden dayanır. Her bir öneri, bir sorun için neden önemli ve önerilen değişiklikleri uygulamak nasıl hakkında rehberlik sağlar.

Kuruluşunuz için en önemli ve ücretsiz ve sağlam bir risk ortam çalıştıran doğru ilerleme durumunuzu izlemenize odak alanlarına seçebilirsiniz.

Çözüm ekledik ve bir değerlendirme tamamlanan, Özet sonra odak alanlarına odaklanmak için bilgi gösterilir **AD değerlendirme** Panosu ortamınızdaki altyapısı için. Aşağıdaki bölümlerde bilgileri üzerinde nasıl kullanacağınızı **AD değerlendirme** Pano, burada görüntüleyebilir ve ardından uygulayın önerilen eylemler için Active Directory sunucu altyapınızı.

![SQL değerlendirmesi döşeme görüntüsü](./media/log-analytics-ad-assessment/ad-tile.png)

![SQL değerlendirmesi Pano görüntüsü](./media/log-analytics-ad-assessment/ad-assessment.png)

## <a name="installing-and-configuring-the-solution"></a>Yükleme ve çözüm yapılandırılıyor
Yüklemek ve çözümleri yapılandırmak için aşağıdaki bilgileri kullanın.

* Aracıları değerlendirilecek etki alanının üyesi olan etki alanı denetleyicilerinde yüklü olması gerekir.
* Active Directory değerlendirme çözümü .NET Framework 4'ün desteklenen bir sürümünü gerektirir (4.5.2 veya üstü) bir OMS aracısı olan her bilgisayarda yüklü.
* OMS çalışma alanından Active Directory değerlendirme çözümü eklemek [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ADAssessmentOMS?tab=Overview) veya açıklanan işlemi kullanarak [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).  Başka bir yapılandırma işlemi gerekmez.

  > [!NOTE]
  > Çözüm ekledikten sonra aracıları sunucularıyla AdvisorAssessment.exe dosyası eklenir. Yapılandırma verilerini okuma ve işleme için bulutta OMS hizmetine gönderilir. Mantığı alınan verilere uygulanır ve bulut hizmeti verilerini kaydeder.
  >
  >

## <a name="active-directory-assessment-data-collection-details"></a>Active Directory değerlendirme veri toplama ayrıntıları

Active Directory değerlendirme etkinleştirdiğiniz aracıları kullanarak aşağıdaki kaynaklardan toplar:

- Kayıt defteri toplayıcıları
- LDAP toplayıcıları
- .NET framework
- Olay günlüğü toplayıcıları
- Active Directory Hizmeti Arabirimleri (ADSI)
- Windows PowerShell
- Dosya veri toplayıcıları
- Windows Yönetim Araçları (WMI)
- DCDIAG aracı API'si
- Dosya Çoğaltma Hizmeti (NTFRS) API'si
- Özel C# kodu


Aşağıdaki tablo, Operations Manager (SCOM) gerekli olup olmadığını ve nasıl aracılar için veri toplama yöntemleri yapılandırmayı gösterir. genellikle veri bir aracı tarafından toplanır.

| Platform | Doğrudan Aracısı | SCOM Aracısı | Azure Storage | SCOM gerekli? | Yönetim grubu gönderilen SCOM Aracısı verileri | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |7 gün |

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

## <a name="use-assessment-focus-area-recommendations"></a>Değerlendirme odak alanı önerileri kullanın
OMS içinde bir değerlendirme çözümü kullanmadan önce çözümü yüklenmiş olması gerekir. Daha fazla bilgi için çözümleri yükleme hakkında bkz [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md). Yüklendikten sonra OMS genel bakış sayfasında değerlendirme döşeme kullanarak önerileri özetini görüntüleyebilirsiniz.

Altyapınız ve ardından-ayrıntıya önerileri için özetlenmiş uyumluluk değerlendirmesi görüntüleyin.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Odak alanı için öneriler görüntülemek ve düzeltici işlemleri için
1. Üzerinde **genel bakış** sayfasında, **değerlendirme** döşeme sunucusu altyapınız için.
2. Üzerinde **değerlendirme** sayfasında odak alanı Kanatlar birinde özet bilgilerini inceleyin ve sonra bu odak alanı için öneriler görüntülemek için tıklatın.
3. Odak alanı sayfaları hiçbirinde ortamınız için öncelikli önerilerin görüntüleyebilirsiniz. Önerinin altında tıklatın **etkilenen nesneleri** öneri neden yapılan hakkında ayrıntıları görüntülemek için.  
    ![Değerlendirmesi önerileri görüntüsü](./media/log-analytics-ad-assessment/ad-focus.png)
4. Önerilen düzeltici eylemleri gerçekleştirebilirsiniz **önerilen eylemleri**. Öğe ele önerilen eylemleri sonraki değerlendirmeleri kayıtları gerçekleştirilen ve uyumluluk puan artmasına neden olur. Düzeltilmiş öğeler görünür olarak **geçirilen nesneleri**.

## <a name="ignore-recommendations"></a>Öneriler yoksay
Yoksay istediğiniz önerileri varsa, OMS önerileri değerlendirme sonuçlarında görünmesini engellemek için kullanacağı bir metin dosyası oluşturabilirsiniz.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Göz ardı eder önerileri tanımlamak için
1. Sunucunuzdan çalışma alanınıza oturum açın ve günlük arama açın. Ortamınızdaki bilgisayarları için başarısız olan liste önerileri için aşağıdaki sorguyu kullanın.

   ```
   Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
>[!NOTE]
> Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), sonra da yukarıdaki sorguda şu şekilde değiştirir.
>
> `ADAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

   Günlük arama sorgusu gösteren ekran görüntüsü şöyledir: ![önerileri başarısız oldu](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)
2. Yoksay istediğiniz önerileri seçin. Sonraki yordamda Recommendationıd için değerleri kullanacaksınız.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Oluşturma ve bir IgnoreRecommendations.txt metin dosyası kullanma
1. IgnoreRecommendations.txt adlı bir dosya oluşturun.
2. Her Recommendationıd ayrı bir satırda yoksay ve sonra dosyayı kaydedip kapatın için günlük analizi istediğiniz her bir öneri için yazın veya yapıştırın.
3. Dosya aşağıdaki klasörde önerileri yoksaymak için OMS istediğiniz her bilgisayara yerleştirin.
   * Microsoft Monitoring (doğrudan veya Operations Manager aracılığıyla bağlı) Agent - olan bilgisayarlarda *SystemDrive*: \Program izleme Agent\Agent
   * Operations Manager yönetim sunucusunda - *SystemDrive*: \Program System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Öneriler göz ardı edilir doğrulamak için
Sonraki değerlendirme çalıştığında, varsayılan olarak 7 günde zamanlanmış sonra belirtilen önerileri işaretlenmiş *yoksayıldı* ve değerlendirme Panoda görünmez.

1. Tüm yoksayılan öneriler listelemek için aşağıdaki günlük arama sorgularını kullanabilirsiniz.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
>[!NOTE]
> Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), sonra da yukarıdaki sorguda şu şekilde değiştirir.
>
> `ADAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

2. Daha sonra yoksayılan önerileri görmek istediğiniz karar verirseniz, tüm IgnoreRecommendations.txt dosyaları silin veya bunları RecommendationIDs kaldırabilirsiniz.

## <a name="ad-assessment-solutions-faq"></a>AD değerlendirmesi çözümleri hakkında SSS
*Sıklıkla bir değerlendirme çalışıyor mu?*

* Değerlendirme 7 günde bir çalışır.

*Ne sıklıkta değerlendirme çalıştıran yapılandırmak için yolu var mı?*

* Şu anda değil.

*I değerlendirme çözümünü ekledikten sonra başka bir sunucu için belirlediyseniz değerlendirileceğini?*

* Evet, bunu daha sonra gelen her 7 günde değerlendirildiği bulunduktan sonra.

*Ne zaman bir sunucu kullanımdan alındığında değerlendirme kaldırılacak?*

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
* Kullanım [günlük analizi aramaları oturum](log-analytics-log-searches.md) ayrıntılı AD Değerlendirme verileri ve öneriler görüntülemek için.
