---
title: Kullanım ve Tahmini maliyetler Azure İzleyici'de izleme
description: Azure İzleyici kullanım ve Tahmini maliyetler sayfasında kullanma işlemine genel bakış
author: dalekoetke
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: mbullwin
ms.reviewer: Dale.Koetke
ms.subservice: ''
ms.openlocfilehash: 7117e7287f601b306893cb02dc5d7599d7c6224d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60453825"
---
# <a name="monitoring-usage-and-estimated-costs-in-azure-monitor"></a>Kullanım ve Tahmini maliyetler Azure İzleyici'de izleme

> [!NOTE]
> Bu makalede, kullanım ve Tahmini maliyetler arasında farklı fiyatlandırma modelleri için birden çok Azure İzleme özelliklerini görüntülemek açıklar.  İlgili bilgiler için aşağıdaki makalelere göz atın.
> - [Veri hacmi ve saklama Log analytics'te kontrol ederek maliyet yönetme](manage-cost-storage.md) veri saklama döneminizin değiştirerek maliyetlerinizi denetlemek nasıl açıklar.
> - [Log analytics'te veri kullanımını çözümleme](../../azure-monitor/platform/data-usage.md) analiz ve veri kullanımınızı uyarı açıklar.
> - [Application ınsights fiyatlandırma ve veri hacmini yönetme](../../azure-monitor/app/pricing.md) Application ınsights'ta veri kullanımını çözümleme açıklar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure portal'ın İzleyici hub'ında **kullanım ve Tahmini maliyetler** sayfası izleme özellikleri gibi Çekirdek kullanımını açıklar [bildirimleri ölçümleri, uyarılar](https://azure.microsoft.com/pricing/details/monitor/), [Azure Log Analytics ](https://azure.microsoft.com/pricing/details/log-analytics/), ve [Azure Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). Nisan 2018 tarihinden önce kullanılabilir fiyatlandırma planları müşteriler, bu öngörüleri satın alınan Log Analytics kullanımı da içerir ve analizi sunar.

Bu sayfada, kullanıcılar kendi kaynak kullanımı son 31 gün için abonelik başına toplanan görüntüleyebilir. Bağlantılı bileşenler 31 günlük dönem boyunca kullanım eğilimlerini gösterir. Çok fazla veri için bu tahmin, bu nedenle birlikte gelen gerektiğinde sayfa yüklenirken lütfen sabırlı olun.

Bu örnek, izleme kullanımını ve elde edilen maliyet tahmini gösterir:

![Kullanım ve Tahmini maliyetler portalı ekran görüntüsü](./media/usage-estimated-costs/001.png)

Son 31 gün dönem boyunca kullanım eğilimlerini gösteren bir grafiği'ni açmak için aylık kullanım sütunundaki bağlantıyı seçin:

![Çubuk grafik ekran düğüm başına dahil edilen](./media/usage-estimated-costs/002.png)

İşte başka bir benzer kullanım ve maliyet özeti. Bu örnekte, yeni Nisan 2018 tüketim tabanlı fiyatlandırma modeliyle bir abonelik gösterilmektedir. Tüm düğüm kullanıma dayalı faturalandırma eksikliği unutmayın. Veri alımı ve Log Analytics ve Application Insights için bekletme artık yeni bir ortak ölçüm olarak raporlanır.

![Kullanım ve Tahmini maliyetler portalı ekran görüntüsü - Nisan 2018 fiyatlandırma](./media/usage-estimated-costs/003.png)

## <a name="new-pricing-model"></a>Yeni fiyatlandırma modeli

Nisan 2018'de bir [yeni fiyatlandırma modeli izleme bırakıldığını](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/).  Bu, bulut kullanımı kolay, tüketim tabanlı fiyatlandırma sunar. Yalnızca, düğüm tabanlı taahhüt kullandıklarınız için ödeme yaparsınız. Yeni fiyatlandırma modeline ayrıntılarını kullanılabilir [bildirimleri ölçümleri, uyarılar](https://azure.microsoft.com/pricing/details/monitor/), [Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) ve [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). 

Log Analytics veya Application ınsights'ı 2 Nisan 2018 tarihinden sonra müşterilerin eklenmesi için yeni fiyatlandırma modeline tek seçenektir. Bu hizmetler kullanan müşteriler için yeni fiyatlandırma modeline taşıma isteğe bağlıdır.

## <a name="assessing-the-impact-of-the-new-pricing-model"></a>Yeni fiyatlandırma modeline etkisini değerlendirme
Yeni fiyatlandırma modeli, kendi izleme kullanım düzenlerini esas alarak her müşteri farklı etkiler olacaktır. Log Analytics veya Application ınsights'ı 2 Nisan 2018'den önce kullanan müşteriler için **kullanım ve tahmini maliyet** Azure İzleyicisi'nde sayfa bunlar yeni fiyatlandırma modeline geçmeniz durumunda herhangi bir değişiklik maliyetleri tahmin eder. Yeni modele bir aboneliği taşımak için bir yol sunar. Çoğu müşteri için yeni fiyatlandırma modeline yararlı olacaktır. Özellikle yüksek veri kullanım düzenlerine sahip ya da daha yüksek maliyetli bölgelerindeki müşteriler için bu durumda olmayabilir.

Tahmini maliyetlerinizi abonelik üzerinde seçtiğiniz görmek için **kullanım ve Tahmini maliyetler** sayfasında, sayfanın üst kısmındaki mavi renkli bir başlık seçin. Yeni fiyatlandırma modeline önemsenmeksizin devralınabilir düzeyi, çünkü bu bir aboneliği bir zaman yapmanız önerilir.

![Kullanım ve Tahmini maliyetler yeni fiyatlandırma modeli ekran izleme](./media/usage-estimated-costs/004.png)

Yeni sayfa, önceki sayfanın yeşil başlığı ile benzer bir sürümü göstermektedir:

![Kullanım ve Tahmini maliyetler geçerli fiyatlandırma modeli ekran izleme](./media/usage-estimated-costs/005.png)

Sayfa, yeni fiyatlandırma modeline karşılık gelen ölçümleri farklı bir kümesini de gösterir. Bu liste örneği verilmiştir:

- İçgörü ve düğüm başına Analytics\Overage
- İçgörü ve düğüm başına Analytics\Included
- Uygulama Insights\Basic fazlalık veri
- Uygulama Insights\Included verileri

Yeni fiyatlandırma modelinin düğüm tabanlı dahil edilen veri ayırma yok. Bu nedenle, bu veri alımı ölçümlerine adlı yeni ortak veri alımı bir ölçüm birleştirilir **paylaşılan Services\Data alımı**. 

Log Analytics veya Application ınsights'ı daha yüksek maliyetleri bölgelerde alınan verileri başka bir değişiklik yoktur. Bu yüksek maliyetli bölgelere verilerini yeni bölgesel ölçümlere ile gösterilir. Bir örnek **veri alımı (ABD Orta Batı)**.

> [!NOTE]
> Abonelik başına maliyetleri değil hesap düzeyinde düğüm yetkilendirmelerini Operations Management Suite (OMS) abonelik başına içine faktörü tahmini. Hesap temsilcinizle için daha ayrıntılı bir tartışma yeni fiyatlandırma modeli, bu durumda başvurun.

## <a name="new-pricing-model-and-operations-management-suite-subscription-entitlements"></a>Yeni fiyatlandırma modeli ve Operations Management Suite aboneliği destek hakları

Microsoft Operations Management Suite E1 ve E2 satın alan müşteriler için düğüm başına veri alımı yetkilendirmeler için uygun [Log Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite) ve [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-pricing). Belirli bir abonelikte Bu destek haklarını Log Analytics çalışma alanları veya Application Insights kaynakları almak için: 

- Aboneliğin fiyatlandırma modeli, Nisan 2018 öncesi modele kalmalıdır.
- Log Analytics çalışma alanları "fiyatlandırma katmanında düğüm başına (OMS)" kullanmanız gerekir.
- Application Insights kaynakları "Kurumsal" fiyatlandırma planını kullanmanız gerekir.

Kuruluşunuzun satın aldığı, paketin düğüm sayısına bağlı olarak bazı taşıma, yeni fiyatlandırma modeline abonelikleri yararlı olabilir ancak bunu dikkatli gerektirir. Genel olarak, yalnızca yukarıda açıklanan şekilde pre-Nisan 2018 modelinde kalmak için tavsiye edilir.

> [!WARNING]
> Kuruluşunuz Microsoft Operations Management Suite E1 ve E2 satın aldıysa aboneliklerinizi pre-Nisan 2018 fiyatlandırma modelinde tutmak en iyisidir. 
>

## <a name="changes-when-youre-moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeline geçerken değişiklikleri

Log Analytics ve Application Insights fiyatlandırma seçenekleri yalnızca tek bir katman (veya planı) için yeni fiyatlandırma modeline basitleştirir. Bir aboneliği yeni fiyatlandırma modeli taşıma:

- (Azure Resource Manager'da "pergb2018" olarak adlandırılır) yeni GB başına katmanı her bir Log Analytics için fiyatlandırma katmanını değiştirme
- Kurumsal plandaki tüm Application Insights kaynakları temel plana değiştirilir.

Maliyet tahmini, bu değişikliklerin etkisini gösterir.

> [!WARNING]
> Buradan dağıtmak için Azure Resource Manager'ı veya PowerShell'i kullanıyorsanız, önemli bir Not [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-template-workspace-configuration) veya [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-powershell) fiyatlandırma modeli yeni taşınmış bir abonelik. Bir fiyatlandırma katmanı/planı "pergb2018" dışında için Log Analytics veya "Temel" için Application ınsights'ı belirtirseniz, yerine geçersiz bir fiyatlandırma katmanı/planını belirten nedeniyle dağıtım başarısız başarılı **ancak yalnızca geçerli kullanın Fiyatlandırma katmanı/planını** (Bu geçersiz bir fiyatlandırma katmanı iletisi burada oluşturulur Log Analytics'i ücretsiz katmanı için geçerli değildir).
>

## <a name="moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeline taşıma

Her Application Insights kaynağına, açık, belirli bir aboneliği yeni fiyatlandırma modeline benimsemeye karar verdiyseniz, Git **kullanım ve Tahmini maliyetler** temel fiyatlandırma katmanını olduğundan emin olun ve her Log Analytics'e Git Çalışma alanı, açık **fiyatlandırma katmanı** sayfasında ve değiştirmek **GB (2018) başına** fiyatlandırma katmanı. 

> [!NOTE]
> Tüm Application Insights kaynaklarını ve Log Analytics çalışma alanları belirli bir aboneliği yeni fiyatlandırma modeli benimsemek gereksinim artık, daha fazla esneklik ve daha kolay yapılandırma olanağı kaldırılmıştır. 

## <a name="automate-moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeline taşıma otomatikleştirin

Yukarıda belirtildiği gibi artık tüm izleme kaynakları bir aboneliği yeni fiyatlandırma modeline aynı anda taşımak için bir gereksinim değildir ve dolayısıyla ``migratetonewpricingmodel`` eylemi artık herhangi bir etkisi yoktur. Artık Application Insights kaynaklarını ve Log Analytics çalışma alanlarını ayrı olarak yeni fiyatlandırma katmanlarına geçebilirsiniz.  

Bu değişiklik otomatikleştirme kullanarak Application Insights için belgelenen [kümesi AzureRmApplicationInsightsPricingPlan](https://docs.microsoft.com/powershell/module/azurerm.applicationinsights/set-azurermapplicationinsightspricingplan) ile ``-PricingPlan "Basic"`` ve Log Analytics kullanarak [Set-Azurermoperationalınsightsworkspace](https://docs.microsoft.com/powershell/module/AzureRM.OperationalInsights/Set-AzureRmOperationalInsightsWorkspace) ile ``-sku "PerGB2018"``. 

