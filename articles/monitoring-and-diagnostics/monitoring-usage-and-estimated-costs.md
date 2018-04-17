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
ms.openlocfilehash: ce295c449b01de4fa99df9198805a6b0727c0d18
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="monitoring-usage-and-estimated-costs"></a>Kullanım ve tahmini maliyetleri izleme

Azure portal'ın İzleyici hub **kullanım ve tahmini maliyetleri** sayfa çekirdek özellikler gibi izleme kullanımdan anlamanıza yardımcı olması için tasarlanmıştır [bildirimleri ölçümleri, uyarı](https://azure.microsoft.com/pricing/details/monitor/), [Oturum Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) ve [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). Nisan 2018 önce kullanılabilir fiyatlandırma planları müşteriler için Analytics sunar ve bu da Öngörüler satın alınan günlük analizi kullanım içerir.

Bu sayfada, kullanıcıların bu kaynakların kullanımı son 31 gün için bu dönemi boyunca kullanım eğilimi görmek için detaya bileşenleri ile abonelik başına toplanan görüntüleyebilirsiniz. Çok sayıda birlikte çekmek ve bu tahmin yapmak için gereken veri yoktur şekilde sayfa yüklerken lütfen bekleyin.
İzleme kullanımı ve sonuçta elde edilen maliyet tahmini gösteren bir örnek aşağıda verilmiştir:

![Kullanım ve tahmini maliyetleri portalı ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/001.png)

Aylık kullanım sütun bağlantıya tıklandığında son 31 gün süresi içinde kullanım eğilimleri gösteren bir grafik açılır:

![Düğüm ekran dahil 671.47 GB](./media/monitoring-usage-and-estimated-costs/002.png)

Başka bir benzer kullanım işte ve bu durumda yeni Nisan 2018 tüketim tabanlı fiyatlandırma modeli bir abonelik için Özet, maliyet. Tüm düğüm tabanlı faturalama ve bu veri alımı ve saklama olmaması için günlük analizi inceleyin ve Application Insights artık yeni bir ortak ölçerde bildirilir.

![Kullanım ve tahmini maliyetleri portalı ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/003.png)

## <a name="new-pricing-model"></a>Yeni bir fiyatlandırma modeli

Nisan 2018 içinde yeni bir izleme fiyatlandırma modeli yayımlanmıştır.  Bu, bulut kolay, tüketim tabanlı fiyatlandırma özellikleri. Yalnızca ne, düğüm tabanlı taahhüt kullandığınız için ödeme yaparsınız. Yeni fiyatlandırma modeli ayrıntılarını kullanılabilir [bildirimleri ölçümleri, uyarı](https://azure.microsoft.com/pricing/details/monitor/), [günlük analizi](https://azure.microsoft.com/pricing/details/log-analytics/) ve [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/).

Günlük analizi veya Application Insights 2 Nisan 2018 sonra müşteriler eklenmesi için yeni bir fiyatlandırma modelini tek seçenektir. Zaten bu hizmetleri kullanan müşteriler için yeni fiyatlandırma modeli taşıma isteğe bağlıdır.

## <a name="assessing-the-impact-of-the-new-pricing-model"></a>Yeni fiyatlandırma modeli etkisini değerlendirme

Yeni fiyatlandırma modeli, kendi izleme kullanım düzenlerini esas alarak her müşteri için farklı etkileri sahip olur. Günlük analizi veya Application Insights 2 Nisan 2018 önce zaten kullanan müşteriler için **kullanım ve tahmini maliyet** sayfa Azure İzleyicisi'nde yeni fiyatlandırma modeli taşırsanız herhangi bir değişikliği maliyetlerini tahmin etmek için bir yöntem sunar ve sağlar Yeni modele bir aboneliği taşımak için yol. Çoğu müşteri için yeni fiyatlandırma modeli yararlı olacaktır, ancak özellikle yüksek veri kullanım desenlerini veya daha yüksek maliyet bölgelerdeki müşteriler için bu durumda olmayabilir.

Maliyetlerinizi seçili abonelikler için tahmini görmek için **kullanım ve tahmini maliyetleri** sayfasında, sayfanın üstüne yakın mavi başlığını tıklatın. Bu, her seferinde bir abonelik, en yeni fiyatlandırma modeli benimsenmesi düzeyi olduğundan yapmak en iyisidir.

![Fiyatlandırma modeli seçme ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/004.png)

Şimdi bu sayfanın yeşil bir başlık ile benzer bir sürümünü görürsünüz:

![Fiyatlandırma modeli seçme ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/005.png)

Burada ölçümler – yeni fiyatlandırma modeli karşılık gelen ölçümler farklı bir dizi görürsünüz. Örneğin, veri alım ölçümler gibi

1. Insight ve düğüm başına Analytics\Overage
2. Insight ve düğüm başına Analytics\Included
3. Uygulama Insights\Basic fazla kullanım verileri
4. Uygulama Insights\Included verileri

adlı bir yeni ortak veri alım ölçer birleştirilir **paylaşılan Services\Data alım** yeni fiyatlandırma modeli düğüm başına dahil edilen veri yetkilendirmeler olmadığından.

Günlük analizi alınan verilerin görürsünüz: başka bir değişiklik olduğunu veya Application Insights daha yüksek maliyetleri bölgelerde düzgün Bu, örneğin yansıtacak şekilde yeni bölgesel ölçümler ile gösterilecek **"veri alımı (ABD Batı Orta)**.

> [!NOTE]
> Abonelik başına tahmini maliyetleri Operations Management Suite (OMS) abonelik hesabı düzey düğüm başına yetkilendirmeler öğeli değil. Lütfen bu durumda yeni fiyatlandırma modeli daha ayrıntılı bir tartışma için hesabı temsilcinize başvurun.

## <a name="new-pricing-model-and-operations-management-suite-subscription-entitlements"></a>Yeni fiyatlandırma modeli ve Operations Management Suite aboneliği yetkilendirmeler

Microsoft Operations Management Suite E1 ve E2 satın alan müşteriler için düğüm başına veri alım yetkilendirmeleri uygun [günlük analizi](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite) ve [Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-pricing#the-price-plans). Günlük analizi için bu yetkilendirmeleri almak için çalışma alanları veya Application Insights kaynaklar abonelik fiyatlandırma modeli belirli bir aboneliğe gerekir kalır öncesi Nisan 2018 fiyatlandırma where günlük analizi "fiyatlandırma katmanı düğüm başına (OMS)" model ve Plan fiyatlandırması application Insights "Kurumsal" kullanılabilir. Kuruluşunuzun satın aldığı, paketi düğümlerinin sayısına bağlı olarak bazı taşımayı yeni fiyatlandırma modeli abonelikleri hala yararlı olabilir, ancak bu dikkat gerektirir. 

## <a name="changes-when-moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeli taşırken değişiklikleri

Bir abonelik yeni fiyatlandırma modeline taşıma yeni GB başına katmana fiyatlandırma katmanı her günlük analizi için değiştirir ve tüm (çağrılan "pergb2018" Azure Kaynak Yöneticisi'nde) taşır. Bu taşıma da herhangi bir Application Insights kaynağı kurumsal planda temel plana değiştirir. Yukarıda açıklanan maliyetini tahmin üzerinde bu etkilerini gösterilmektedir. 

## <a name="moving-to-the-new-pricing-model"></a>Yeni fiyatlandırma modeline taşıma

Bir abonelik için yeni fiyatlandırma modeli benimsemeye karar verdiyseniz, tıklatın **model seçimi fiyatlandırma** seçeneği en üstündeki **kullanım ve tahmini maliyetleri** sayfa:

![Yeni fiyatlandırma modeli ekran görüntüsü bir tahmini maliyetleri kullanımını izleme](./media/monitoring-usage-and-estimated-costs/006.png)

Bu açılır **model seçimi fiyatlandırma** sayfasında, her biri önceki sayfada görüntülemekte olduğunuz abonelik listesi:

![Fiyatlandırma modeli seçimi ekran görüntüsü](./media/monitoring-usage-and-estimated-costs/007.png)

Yeni fiyatlandırma modeli bir aboneliği taşımak için yalnızca onay kutusunu işaretleyin ve tıklatın **kaydetmek**.  Bu gibi durumlarda, eski fiyatlandırma modeli geri aynı şekilde taşıyabilirsiniz. Abonelik sahibi veya katkıda bulunan izinleri fiyatlandırma modeli değiştirmek için gerekli olduğunu göz önünde bulundurun.
