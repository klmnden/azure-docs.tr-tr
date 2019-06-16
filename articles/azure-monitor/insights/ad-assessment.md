---
title: Active Directory ortamınızı Azure İzleyici ile en iyi duruma getirme | Microsoft Docs
description: Düzenli bir aralıkta, ortamlarının sistem durumunu ve riskini değerlendirmek için Active Directory sistem durumu denetimi çözümü kullanabilirsiniz.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 81eb41b8-eb62-4eb2-9f7b-fde5c89c9b47
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/27/2017
ms.author: magoedte
ms.openlocfilehash: 3b5da6c9046fc694bd5eb0f55cf031b82b6d0103
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60919829"
---
# <a name="optimize-your-active-directory-environment-with-the-active-directory-health-check-solution-in-azure-monitor"></a>Active Directory ortamınızı Azure İzleyici'de Active Directory sistem durumu denetimi çözümü ile en iyi duruma getirme

![AD sistem durumu denetimi simgesi](./media/ad-assessment/ad-assessment-symbol.png)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Düzenli bir aralıkta, server ortamlarının sistem durumunu ve riskini değerlendirmek için Active Directory sistem durumu denetimi çözümü kullanabilirsiniz. Bu makalede, yükleme ve çözüm kullanabilir, böylece olası sorunlar için düzeltici eylemleri gerçekleştirebilirsiniz yardımcı olur.

Bu çözüm, Önceliklendirilmiş öneriler dağıtılan sunucu altyapınızı belirli listesini sağlar. Öneriler dört arasında ayrılır odak alanlarına odaklanmak hızlı bir şekilde yardımcı riskini anlamak ve işlem yapın.

Öneriler bilgi ve müşteri binlerce Microsoft mühendisinin göre kazanılan deneyimi temel alır. Her öneri, önerilen değişiklikleri uygulamak nasıl bir sorun için neden önemli ve ilgili yönergeleri sağlar.

Ücretsiz ve sağlam bir risk ortamında çalışan kendi ilerleme durumlarını izlemek ve kuruluşunuz için en önemli odak alanlarına odaklanmak seçebilirsiniz.

Çözüm ekledikten sonra bir denetimi tamamlandı ve Özet bilgi odak alanlarına odaklanmak için gösterilir **AD sistem durumu denetimi** ortamınızdaki altyapısı için Pano. Aşağıdaki bölümlerde bilgileri kullanmak üzere nasıl **AD sistem durumu denetimi** Pano, burada görüntüleyebilir ve sonra gerçekleştirin Eylemler Active Directory sunucusu altyapınız için önerilen.  

![AD sistem durumu denetimi kutucuk görüntüsü](./media/ad-assessment/ad-healthcheck-summary-tile.png)

![AD sistem durumu denetimi panosunun görüntüsü](./media/ad-assessment/ad-healthcheck-dashboard-01.png)

## <a name="prerequisites"></a>Önkoşullar

* Active Directory sistem durumu denetimi çözümü, desteklenen bir .NET Framework 4.5.2 sürümü gerekir veya yukarıda Microsoft Monitoring Agent (yüklenmiş MMA) olan her bilgisayarda yüklü.  MMA aracısını, System Center 2016 - Operations Manager ve Operations Manager 2012 R2 ve Azure İzleyici tarafından kullanılır.
* Çözüm, Windows Server 2008 ve 2008 R2, Windows Server 2012 ve 2012 R2 ve Windows Server 2016 çalıştıran etki alanı denetleyicilerini destekler.
* Azure portalında Azure Market'ten Active Directory sistem durumu denetimi çözümü eklemek için bir Log Analytics çalışma alanı.  Başka bir yapılandırma işlemi gerekmez.

  > [!NOTE]
  > Çözüm ekledikten sonra aracılar sunucularıyla AdvisorAssessment.exe dosyası eklenir. Yapılandırma verilerini okuyun ve ardından Azure İzleyici bulutta işleme için gönderilen. Mantıksal alınan verilere uygulanır ve bulut hizmeti olan verileri kaydeder.
  >
  >

Değerlendirilecek etki alanının üyesi olan etki alanı denetleyicilerinizin sistem durumu denetimi gerçekleştirmek için bunlar bir aracı ve Azure İzleyici'de aşağıdaki desteklenen yöntemlerden birini kullanarak bağlantı gerektirir:

1. Yükleme [Microsoft Monitoring Agent (MMA)](../../azure-monitor/platform/agent-windows.md) , etki alanı denetleyicisi zaten System Center 2016 - Operations Manager veya Operations Manager 2012 R2 tarafından izlenmiyor.
2. System Center 2016 - Operations Manager veya Operations Manager 2012 R2 ile izlenir ve yönetim grubu, Azure İzleyici ile tümleşiktir değil, etki alanı denetleyicisi verileri toplamak ve hala Hizmeti'ne iletmek için Azure İzleyici ile birden çok girişli olabilir Operations Manager tarafından izlenecek.  
3. Operations Manager yönetim grubunuzun hizmeti ile tümleşikse, aksi takdirde, etki alanı denetleyicileri için veri toplama altındaki adımları izleyerek hizmet eklemek ihtiyacınız [aracıyla yönetilen bilgisayarlar ekleme](../../azure-monitor/platform/om-agents.md#connecting-operations-manager-to-azure-monitor) seçeneğini etkinleştirdikten sonra çalışma alanınızda çözümün.  

Etki alanı denetleyicinize bir Operations Manager yönetim grubu için hangi raporların verileri toplayan aracıda kendi atanan yönetim sunucusuna iletir ve doğrudan yönetim sunucusundan Azure İzleyicisi'ne gönderilir.  Operations Manager veritabanları için veriler yazılmaz.  

## <a name="active-directory-health-check-data-collection-details"></a>Active Directory sistem durumu denetimi veri koleksiyonu ayrıntıları

Active Directory sistem durumu denetimi veri etkinleştirdiğiniz aracısını kullanarak aşağıdaki kaynaklardan toplar:

- Kayıt defteri
- LDAP
- .NET Framework
- Olay günlüğü
- Active Directory hizmet arabirimi (ADSI)
- Windows PowerShell
- Dosya verileri
- Windows Yönetim Araçları (WMI)
- DCDIAG aracı API'si
- Dosya Çoğaltma Hizmeti (NTFRS) API'si
- Özel C# kodu

Veri etki alanı denetleyicisinde toplanır ve Azure İzleyici her yedi günde iletilir.  

## <a name="understanding-how-recommendations-are-prioritized"></a>Öneriler nasıl önceliklendirilir anlama
Yaptığınız her öneri, öneri göreceli önemini tanımlayan bir ağırlık değeri verilir. Yalnızca en önemli 10 önerileri gösterilir.

### <a name="how-weights-are-calculated"></a>Ağırlıklar nasıl hesaplanır
Ağırlık belirlemeyi üç anahtar etkenlere göre toplam değerler şunlardır:

* *Olasılık* tanımlanan bir sorunun sorunlara neden olur. Daha yüksek bir olasılık öneri için daha büyük bir genel puan karşılık gelir.
* *Etkisi* kuruluşunuzdaki bir soruna neden olursa sorunun. Daha yüksek bir etkisi öneri için daha büyük bir genel puan karşılık gelir.
* *Çaba* öneriyi uygulamak için gereklidir. Daha yüksek bir çaba öneri için daha küçük bir genel puan karşılık gelir.

Her öneri için hesaplamasının her odak alanı için kullanılabilir toplam puanı yüzdesi olarak ifade edilir. Örneğin, bu öneri uygulandıktan bir puan % 5, güvenlik ve uyumluluk odak alanında bir öneri varsa, genel güvenlik ve uyumluluk puanı tarafından %5 artırır.

### <a name="focus-areas"></a>Odak alanları
**Güvenlik ve Uyumluluk** -bu odak alanı olası güvenlik tehditlerini ve ihlallerinden, şirket ilkelerini ve uyumluluk teknik, yasal ve kanuni gereksinimleri için öneriler gösterir.

**Kullanılabilirlik ve iş sürekliliği** -hizmet kullanılabilirlik, dayanıklılık, altyapı ve işletme korumasına yönelik öneriler bu odak alanı gösterir.

**Performans ve ölçeklenebilirlik** -bu odak alanı kuruluşunuzun yardımcı olacak öneriler gösterir BT altyapısı arttıkça, BT ortamınızı geçerli performans gereksinimlerini karşıladığından ve değişen altyapı mümkün olduğundan emin olun gerekir.

**Yükseltme, geçiş ve dağıtım** -bu odak alanı yükseltme, geçiş ve Active Directory mevcut altyapınızı dağıtmak yardımcı olacak öneriler gösterir.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>% 100 her odak alanında puanlamak için indirmeyi amaçlamanız gerekir?
Olmayabilir. Öneriler bilgi ve müşteri binlerce Microsoft mühendisleri tarafından elde edilen deneyimler temel alır. Ancak, hiçbir iki sunucu altyapıları aynıdır ve öneriler için daha az veya uygun olabilir. Örneğin, bazı güvenlik önerilerini sanal makinelerinize Internet'e açık değildir, daha az ilgili olabilir. Bazı kullanılabilirlik önerisi düşük öncelikli işler için geçici veri toplama ve raporlama sağladığı hizmetler için daha uygun olabilir. Olgun bir işletmeye için önemli olan sorunları için bir başlangıç daha az önemli olabilir. Önceliklerinizle odak alanları tanımlamak ve puanları, zaman içinde nasıl değiştiğini ardından aramak isteyebilirsiniz.

Her öneri neden önemli olduğu ile ilgili yönergeler içerir. Öneri uygulandıktan verilen BT hizmetlerinizin doğasını ve kuruluşunuzun iş gereksinimlerini, sizin için uygun olup olmadığını değerlendirmek için bu yönergeleri kullanmanız gerekir.

## <a name="use-health-check-focus-area-recommendations"></a>Odak alanı önerileri kullanım durumunu denetleme
Yüklendikten sonra çözüm sayfasında Azure portalındaki sistem durumu denetimi kutucuk kullanarak önerileri özetini görüntüleyebilirsiniz.

Altyapınız ve ardından-ayrıntıya önerileri için Özet uyumluluk değerlendirmesi görüntüleyin.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Odak alanı için önerileri görüntüleme ve düzeltme eylemi
[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

1. Üzerinde **genel bakış** sayfasında **Active Directory sistem durumu denetimi** Döşe.
1. Üzerinde **sistem durumu denetimi** sayfasında odak alanı dikey pencereleri biriyle özet bilgilerini gözden geçirin ve ardından bu odak alanı için önerileri görmek için tıklatın.
1. Herhangi bir odak alanı sayfalar üzerinde ortamınız için yapılan Önceliklendirilmiş öneriler görüntüleyebilirsiniz. Altında bir öneriye tıklayabilir **etkilenen nesneler** öneri neden yapılır hakkında ayrıntılı bilgi görüntülemek için.<br><br> ![Sistem durumu denetimi önerileri görüntüsü](./media/ad-assessment/ad-healthcheck-dashboard-02.png)
1. Önerilen düzeltici eylemleri gerçekleştirebilirsiniz **önerilen eylemleri**. Öğe ele alındığında, önerilen eylemler sonraki değerlendirmeler kayıtları alınan ve uyumluluk puanınız artacaktır. Düzeltilen öğeler görünür olarak **geçirilen nesneleri**.

## <a name="ignore-recommendations"></a>Öneriler yoksay
Yok saymak için istediğiniz önerilerini varsa, Azure İzleyici, öneriler, değerlendirme sonuçlarında görüntülenmesini önlemek için kullanacağı bir metin dosyası oluşturabilirsiniz.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Göz ardı eder önerileri tanımlamak için
[!INCLUDE [azure-monitor-log-queries](../../../includes/azure-monitor-log-queries.md)]

Ortamınızdaki bilgisayarları için başarısız olan liste öneriler için aşağıdaki sorguyu kullanın.

```
ADAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation
```

Günlük sorgusu gösteren ekran görüntüsü aşağıda verilmiştir:<br><br> ![başarısız önerileri](media/ad-assessment/ad-failed-recommendations.png)

Yok saymak için istediğiniz önerilerini seçin. Sonraki yordamda Recommendationıd için değerleri kullanacaksınız.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Oluşturma ve bir IgnoreRecommendations.txt metin dosyası kullanma
1. IgnoreRecommendations.txt adlı bir dosya oluşturun.
2. Yapıştırın veya Azure İzleyici'nın ayrı bir satıra yoksay ve sonra dosyayı kaydedip kapatın istediğiniz her bir öneri için her Recommendationıd yazın.
3. Dosya, aşağıdaki klasörde, önerileri yok saymak için Azure İzleyici istediğiniz her bilgisayara yerleştirin.
   * Bilgisayarlarda Microsoft izleme (doğrudan veya Operations Manager üzerinden bağlı) aracısı ile - *SystemDrive*: \Program Monitoring Agent\Agent
   * Operations Manager 2012 R2 yönetim sunucusunda - *SystemDrive*: \Program System Center 2012 R2\Operations Manager\Server
   * Operations Manager 2016 yönetim sunucusunda - *SystemDrive*: \Program System Center 2016\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Öneriler göz ardı edilir doğrulamak için
Sonraki, varsayılan olarak yedi günde sistem durumu denetimi çalıştırır, zamanlanmış sonra belirtilen önerileri işaretlenmiş *yoksayıldı* ve Panoda görünmez.

1. Yoksayılan tüm önerilere listelemek için aşağıdaki günlük sorguları kullanabilirsiniz.

    ```
    ADAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation
    ```

2. Daha sonra yoksayılan önerileri görmek istediğinize karar verirseniz, IgnoreRecommendations.txt dosyaları kaldırın veya bunları RecommendationIDs kaldırabilirsiniz.

## <a name="ad-health-check-solutions-faq"></a>AD durumu denetimi çözümleri hakkında SSS
*Sistem durumu denetimi ne kadar sıklıkla çalışır?*

* Denetim yedi günde bir çalıştırılır.

*Sistem durumu denetimini çalışma sıklığını yapılandırmak için bir yol var mı?*

* Şu anda değil.

*Eğer başka bir sunucuya, ben bir sistem durumu denetimi çözümü ekledikten sonra bulunan için denetlenmesi*

* Evet, yedi günde bir, daha sonra gelen iade edilmeden bulununca.

*Ne zaman bir sunucu kullanımdan alındıktan hemen ise sistem durumu denetiminin dışında kaldırılacak?*

* Bir sunucu 3 haftaya kadar veri göndermez, kaldırılır.

*Veri koleksiyonu yapan işlemin adı nedir?*

* AdvisorAssessment.exe

*Ne kadar toplanacak veri için sürer?*

* Gerçek veri toplama sunucusuna yaklaşık 1 saat sürer. Bu çok sayıda Active Directory sunucuları sahip sunucularda daha uzun sürebilir.

*Verileri toplandığında yapılandırmak için bir yol var mı?*

* Şu anda değil.

*Neden yalnızca ilk 10 önerileri görüntülensin mi?*

* Büyük kapsamlı bir liste görevlerin vermek yerine, Önceliklendirilmiş öneriler adresleme odaklanmak öneririz. Bunları adres sonra ek öneriler kullanılabilir hale gelir. Ayrıntılı listesini görmek isterseniz, bir günlük sorgusu kullanarak tüm önerileri görüntüleyebilirsiniz.

*Bir öneri yok saymak için bir yol var mı?*

* Evet, bkz: [önerileri yoksay](#ignore-recommendations) yukarıdaki bölümde.

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [Azure İzleyici günlük sorguları](../log-query/log-query-overview.md) ayrıntılı veri AD sistem durumunu denetleyin ve önerileri çözümleme hakkında bilgi edinmek için.
