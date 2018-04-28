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
ms.openlocfilehash: f25c39b602449be3ab9d1cd7e67d6fcfc78afb17
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
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

Nisan 2018 içinde yeni bir izleme fiyatlandırma modeli yayımlanmıştır. Bu, bulut kolay, tüketim tabanlı fiyatlandırma özellikleri. Yalnızca ne, düğüm tabanlı taahhüt kullandığınız için ödeme yaparsınız. Yeni fiyatlandırma modeli ayrıntılarını kullanılabilir [bildirimleri ölçümleri, uyarı](https://azure.microsoft.com/pricing/details/monitor/), [günlük analizi](https://azure.microsoft.com/pricing/details/log-analytics/), ve [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/).

Günlük analizi veya Application Insights 2 Nisan 2018 sonra kullanmaya başlamak müşteriler yeni fiyatlandırma modelini tek seçenektir. Bu hizmetler zaten kullanan müşteriler için yeni fiyatlandırma modeli taşıma isteğe bağlıdır.

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

Microsoft Operations Management Suite E1 ve E2 satın alan müşteriler için düğüm veri alım yetkilendirmeleri başına için uygun [günlük analizi](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite) ve [Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-pricing#the-price-plans). Günlük analizi çalışma alanları veya Application Insights için bu yetkilendirmeleri almak için kaynaklar abonelik fiyatlandırma modeli belirli bir aboneliğe öncesi Nisan 2018 fiyatlandırma modeli kalmalıdır. Günlük analizi "Fiyatlandırma katmanı ve uygulama fiyatlandırma planı Öngörüler"Kurumsal"düğüm başına (OMS)" kullanılabildiği olmasıdır. Düğümler, kuruluşunuzun satın aldığı paket sayısına bağlı olarak, bazı abonelikler yeni fiyatlandırma modeli için hala yararlı olabilir taşıma. Ancak bu dikkat gerektirir.

## <a name="changes-when-youre-moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeli taşırken değişiklikleri

Bir abonelik yeni fiyatlandırma modeline taşıma yeni GB başına katmana fiyatlandırma katmanı her günlük analizi için değiştirir ve tüm (çağrılan "pergb2018" Azure Kaynak Yöneticisi'nde) taşır. Bu taşıma da herhangi bir Application Insights kaynağı kurumsal planda temel plana değiştirir. Maliyet tahmini bu değişikliklerin etkisini gösterir.

## <a name="moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeline taşıma

Bir abonelik için yeni fiyatlandırma modeli benimsemeye karar verdiyseniz, seçin **model seçimi fiyatlandırma** seçeneği en üstündeki **kullanım ve tahmini maliyetleri** sayfa:

![Kullanımını izleme ve tahmini maliyetleri yeni fiyatlandırma modeli ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/006.png)

**Model seçimi fiyatlandırma** sayfası açılır. Her önceki sayfada görüntülenen Aboneliklerin listesini gösterir:

![Fiyatlandırma modeli seçimi ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/007.png)

Yeni fiyatlandırma modeli bir aboneliği taşımak için yalnızca kutusunu seçin ve ardından **kaydetmek**. Bu gibi durumlarda, eski fiyatlandırma modeli geri aynı şekilde taşıyabilirsiniz. Bu abonelik sahibi göz önünde bulundurun veya katkıda bulunan izinleri fiyatlandırma modeli değiştirmek için gereklidir.
