---
title: Kullanım ve Azure İzleyicisi'nde tahmini maliyetleri izleme | Microsoft Docs
description: Azure monitör kullanımı ve tahmini maliyetleri sayfa kullanmanın işlemine genel bakış
author: dalekoetke
manager: carmonmills
editor: mrbullwinkle
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.author: Dale.Koetke;mbullwin
ms.openlocfilehash: 08991565d56ffbf7d798944f108a1b86e4463c58
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="monitoring-usage-and-estimated-costs"></a>Kullanım ve tahmini maliyetleri izleme

Azure portal'ın İzleyici hub **kullanım ve tahmini maliyetleri** sayfa çekirdek özellikler gibi izleme kullanımını açıklar [bildirimleri ölçümleri, uyarı](https://azure.microsoft.com/pricing/details/monitor/), [Azure günlük analizi ](https://azure.microsoft.com/pricing/details/log-analytics/), ve [Azure Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). Nisan 2018 önce kullanılabilir fiyatlandırma planları müşteriler için Analytics sunar ve bu da Öngörüler satın alınan günlük analizi kullanım içerir.

Bu sayfada, kullanıcılar kendi kaynak kullanımı son 31 gün için abonelik başına toplanan görüntüleyebilir. Detaylandırma bileşenler 31 gün süresi içinde kullanım eğilimlerini gösterir. Çok miktarda veri birlikte bu tahmin, bu nedenle alınması gerekip sayfa yüklerken lütfen bekleyin.

Bu örnek, izleme kullanımı ve sonuçta elde edilen maliyet tahmini gösterir:

![Kullanım ve tahmini maliyetleri portalı ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/001.png)

Son 31 gün süresi içinde kullanım eğilimleri gösteren bir grafik açmak için aylık kullanım sütununda bağlantıyı seçin:

![Çubuk grafik ekran düğüm başına dahil](./media/monitoring-usage-and-estimated-costs/002.png)

Başka bir benzer kullanım işte ve maliyet özeti. Bu örnek, bir abonelik yeni Nisan 2018 tüketim tabanlı fiyatlandırma modelde gösterir. Tüm düğüm tabanlı faturalama eksikliği unutmayın. Şimdi yeni bir ortak ölçerde veri alımı ve günlük analizi ve Application Insights için bekletme raporlanır.

![Kullanım ve tahmini maliyetleri portalı ekran görüntüsü - Nisan 2018 fiyatlandırma](./media/monitoring-usage-and-estimated-costs/003.png)

## <a name="new-pricing-model"></a>Yeni bir fiyatlandırma modeli

Nisan 2018 içinde bir [fiyatlandırma modeli yeni izleme yayımlanan](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/).  Bu, bulut kolay, tüketim tabanlı fiyatlandırma özellikleri. Yalnızca ne, düğüm tabanlı taahhüt kullandığınız için ödeme yaparsınız. Yeni fiyatlandırma modeli ayrıntılarını kullanılabilir [bildirimleri ölçümleri, uyarı](https://azure.microsoft.com/pricing/details/monitor/), [günlük analizi](https://azure.microsoft.com/pricing/details/log-analytics/) ve [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). 

Günlük analizi veya Application Insights 2 Nisan 2018 sonra müşteriler eklenmesi için yeni bir fiyatlandırma modelini tek seçenektir. Bu hizmetler zaten kullanan müşteriler için yeni fiyatlandırma modeli taşıma isteğe bağlıdır.

## <a name="assessing-the-impact-of-the-new-pricing-model"></a>Yeni fiyatlandırma modeli etkisini değerlendirme
Yeni fiyatlandırma modeli, kendi izleme kullanım düzenlerini esas alarak her müşteri farklı etkileri sahip olur. Günlük analizi veya Application Insights 2 Nisan 2018 önce kullanmakta olduğunuz müşteriler **kullanım ve tahmini maliyet** sayfa Azure İzleyicisi'nde yeni fiyatlandırma modeli taşırsanız maliyetleri herhangi bir değişikliği tahmin eder. Bir abonelik yeni modeline taşıma olanağı sağlar. Çoğu müşteri için yeni fiyatlandırma modeli yararlı olacaktır. Özellikle yüksek veri kullanım desenlerini veya daha yüksek maliyet bölgelerdeki müşteriler için bu durumda geçerli olmayabilir.

Üzerinde seçtiğiniz maliyetleriniz abonelikler için tahmini görmek için **kullanım ve tahmini maliyetleri** sayfasında, sayfanın üstüne yakın mavi başlığını seçin. En yeni fiyatlandırma modeli benimsenmesi düzeyi olduğu için her seferinde bir Bu abonelik yapmak en iyisidir.

![Kullanımını izleme ve tahmini maliyetleri yeni fiyatlandırma modeli ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/004.png)

Yeni sayfa, önceki sayfanın yeşil bir başlık ile benzer bir sürümünü gösterir:

![Kullanımını izleme ve tahmini maliyetleri geçerli fiyatlandırma modeli ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/005.png)

Sayfa ayrıca yeni fiyatlandırma modeli karşılık ölçümler farklı bir kümesini gösterir. Bu liste bir örnek verilmiştir:

- Insight ve düğüm başına Analytics\Overage
- Insight ve düğüm başına Analytics\Included
- Uygulama Insights\Basic fazla kullanım verileri
- Uygulama Insights\Included verileri

Yeni fiyatlandırma modeli düğümü tabanlı dahil edilen veri ayırma sahip değil. Bu nedenle, bu veri alım ölçümler adlı bir yeni ortak veri alım ölçer birleştirilir **paylaşılan Services\Data alım**. 

Günlük analizi ya da daha yüksek maliyetleri bölgelerdeki Application Insights alınan verileri başka bir değişiklik yoktur. Bu yüksek maliyetli bölgeler için veri Yeni bölgesel ölçümler ile gösterilir. Örnek **veri alımı (BİZE Batı Merkezi)**.

> [!NOTE]
> Abonelik başına maliyetleri değil düğümü yetkilendirmeler Operations Management Suite (OMS) abonelik başına hesap düzeyinde içine faktörü tahmini. Hesabı temsilcinize için daha ayrıntılı bir tartışma yeni fiyatlandırma modeli Bu durumda başvurun.

## <a name="new-pricing-model-and-operations-management-suite-subscription-entitlements"></a>Yeni fiyatlandırma modeli ve Operations Management Suite aboneliği yetkilendirmeler

Microsoft Operations Management Suite E1 ve E2 satın alan müşteriler için düğüm başına veri alım yetkilendirmeleri uygun [günlük analizi](https://www.microsoft.com/cloud-platform/operations-management-suite) ve [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-pricing#the-price-plans). Bu yetkilendirmeler günlük analizi çalışma alanları veya Application Insights kaynaklar için belirli bir aboneliğe almak için: 

- Aboneliğin fiyatlandırma modeli öncesi Nisan 2018 modelinde kalmalıdır.
- Günlük analizi çalışma alanları "fiyatlandırma katmanı düğüm başına (OMS)" kullanmanız gerekir.
- Application Insights kaynakları "planı fiyatlandırma Kurumsal" kullanmalısınız.

Kuruluşunuzun satın aldığı, paketi düğümlerinin sayısına bağlı olarak bazı taşımayı abonelikleri yeni fiyatlandırma modeli için yararlı olabilir, ancak bu dikkat gerektirir. Genel olarak, yalnızca yukarıda açıklandığı gibi öncesi Nisan 2018 modelinde kalmak için önerilir.

> [!WARNING]
> Kuruluşunuz Microsoft Operations Management Suite E1 ve E2 satın aldıysa, aboneliklerinizi öncesi Nisan 2018 fiyatlandırma modeli korumak en iyisidir. 
>

## <a name="changes-when-youre-moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeli taşırken değişiklikleri

Yeni fiyatlandırma modeli günlük analizi ve Application Insights seçenekleri yalnızca tek bir katman (veya plan) fiyatlandırma basitleştirir. Bir abonelik yeni fiyatlandırma modeli taşıma:

- (Azure Kaynak Yöneticisi'nde "pergb2018" olarak adlandırılır) yeni bir GB başına katmanına her günlük analizi için fiyatlandırma katmanını değiştirin
- Tüm kurumsal planı Application Insights kaynaklarında temel plana değiştirilir.

Maliyet tahmini bu değişikliklerin etkisini gösterir.

> [!WARNING]
> Burada dağıtmak için Azure Resource Manager veya PowerShell kullanırsanız, önemli bir Not [günlük analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-template-workspace-configuration) veya [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-powershell) yeni taşınmış bir abonelikte fiyatlandırma modeli. Bir fiyatlandırma katmanı/planı "pergb2018" dışında için günlük analizi veya "Temel" için Application Insights belirtirseniz, yerine fiyatlandırma katmanı/plan, geçersiz bir belirtme nedeniyle dağıtım başarısız olan bu başarılı olur **ancak yalnızca geçerli kullanın Fiyatlandırma katmanı/plan**. 
>

## <a name="moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeline taşıma

Bir abonelik için yeni fiyatlandırma modeli benimsemeye karar verdiyseniz, seçin **model seçimi fiyatlandırma** seçeneği en üstündeki **kullanım ve tahmini maliyetleri** sayfa:

![Kullanımını izleme ve tahmini maliyetleri yeni fiyatlandırma modeli ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/006.png)

**Model seçimi fiyatlandırma** sayfası açılır. Her önceki sayfada görüntülenen Aboneliklerin listesini gösterir:

![Fiyatlandırma modeli seçimi ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/007.png)

Yeni fiyatlandırma modeli bir aboneliği taşımak için yalnızca kutusunu seçin ve ardından **kaydetmek**. Bu gibi durumlarda, eski fiyatlandırma modeli geri aynı şekilde taşıyabilirsiniz. Bu abonelik sahibi göz önünde bulundurun veya katkıda bulunan izinleri fiyatlandırma modeli değiştirmek için gereklidir.
