---
title: Azure İzleyici verilerini Görselleştirme | Microsoft Docs
description: Ölçüm görselleştirin ve Azure İzleyici'de depolanan verileri günlüğe kaydetmek için kullanılabilen yöntemler bir özetini sağlar.
author: bwren
manager: carmonm
editor: ''
services: azure-monitor
documentationcenter: azure-monitor
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/24/2018
ms.author: bwren
ms.openlocfilehash: 4e9bb9bd86e22efa5dd514e742481911b09c5e17
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61088293"
---
# <a name="visualizing-data-from-azure-monitor"></a>Azure İzleyici verilerini Görselleştirme
Bu makalede, günlük ve ölçüm verilerini Azure İzleyici'de depolanan görselleştirmek için kullanılabilen yöntemler bir özetini sağlar.

Grafikler gibi görselleştirmelerin detaya gitme sorunları ve desenleri tanımlamak için izleme verilerinizi analiz etmenize yardımcı olabilir. Kullandığınız araç bağlı olarak, görselleştirmeler içinde hem de kuruluşunuz dışındaki diğer kullanıcılarla paylaşmak için seçenek olabilir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="azure-dashboards"></a>Azure panoları
[Azure panoları](../azure-portal/azure-portal-dashboards.md) Azure için birincil yönelik Kompozit teknolojileridir. Bunlar bölmeden cam hizmetlerini önemli sorunları hızlıca tanımlamanıza olanak sağlar ve Azure altyapı sağlama konusunda özellikle yararlıdır.

![Pano](media/visualizations/dashboard.png)

### <a name="advantages"></a>Yararları
- Azure kapsamlı tümleştirme. Görselleştirmeler, ölçüm Gezgini, Log Analytics ve Application Insights gibi birden çok Azure sayfalarından panolara sabitlenebilir.
- Ölçüm ve günlükleri hem destekler.
- Çıkışı dahil olmak üzere birden çok kaynaktan alınan verileri birleştirin [ölçüm Gezgini](platform/metrics-charts.md), [oturum sorguları](log-query/log-query-overview.md), ve [eşler](app/app-map.md) ve Application ınsights kullanılabilirlik.
- Kişisel veya paylaşılan panolar için seçenek. Azure ile tümleşik [rol tabanlı kimlik doğrulaması (RBAC)](../role-based-access-control/overview.md).
- Otomatik yenileme. Ölçümleri Yenile zaman aralığı en az beş dakika ile bağlıdır. Günlükleri bir dakikada yenileyin.
- Zaman damgası ve özel parametrelerle parametreli ölçümleri panolar.
- Esnek Düzen Seçenekleri.
- Tam ekran modu.


### <a name="limitations"></a>Sınırlamalar
- Sınırlı günlük görselleştirmeler veri tablolarının desteği olmadan denetime. Veri serisi toplam sayısı 10'a sınırlı altında gruplandırılmış daha fazla veri serisi ile bir _diğer_ demet.
- Özel parametre için günlük grafikleri destekler.
- Son 30 gün için günlük grafikleri sınırlıdır.
- Günlük grafikler yalnızca paylaşılan panolara sabitlenebilir.
- Pano verileriyle etkileşim yok.
- Sınırlı bağlamsal detaya gitme.

## <a name="azure-monitor-views"></a>Azure İzleyici görünümleri
[Azure İzleyici görünümlerde](platform/view-designer.md) günlük verileri ile özel görselleştirmeler oluşturmanıza imkan tanır. Tarafından kullanılan [izleme çözümleri](insights/solutions.md) bunlar topladığı verileri sunmak için.

![Görünüm](media/visualizations/view.png)

### <a name="advantages"></a>Yararları
- Günlük verileri için zengin görselleştirmeler.
- Dışarı aktarma ve diğer kaynak gruplarında ve Aboneliklerde aktarmak için görünümler içeri aktarın.
- Azure İzleyici yönetim modeliyle çalışma alanları ve izleme çözümlerinin tümleştirir.
- [Filtreler](platform/view-designer-filters.md) özel parametreler için.
- Etkileşimli, destekleyen çok düzeyli ayrıntıya içinde (başka bir görünüme ayrıntılı açıklanmıştır görünümü)

### <a name="limitations"></a>Sınırlamalar
- Günlükleri destekler ancak ölçümleri değil.
- Kişisel Görünüm yok. Çalışma alanına erişimi olan tüm kullanıcılar için kullanılabilir.
- Otomatik Yenile yoktur.
- Düzen Seçenekleri sınırlıdır.
- Birden çok çalışma alanında veya Application Insights uygulamaları'nı sorgulamak için destek yok.
- Sorgular, yanıt boyutu 110 saniye 8 MB ve sorgu yürütme süresini sınırlıdır.


## <a name="workbooks"></a>Çalışma Kitapları
[Çalışma kitapları](../azure-monitor/app/usage-workbooks.md) , veri araştırma ve ekip içindeki işbirliğini derin Öngörüler sağlayan etkileşimli belgelerdir. Çalışma kitapları kullanışlı olduğu belirli örnekler, Kılavuzlar ve olay postmortem giderirken.

![Çalışma kitabı](media/visualizations/workbook.png)

### <a name="advantages"></a>Yararları
- Ölçüm ve günlükleri hem destekler.
- İlişkili grafikler ve görselleştirmeler etkileşimli raporları nereye bir tablodaki bir öğe seçtiğinizde dinamik olarak olacak etkinleştirme destekler parametreleri güncelleştirin.
- Belge benzeri akış.
- Kişisel veya paylaşılan çalışma kitapları için seçenek.
- Kolay ve işbirliğine dayalı, kullanımı kolay geliştirme deneyimi.
- Genel GitHub tabanlı Şablon Galerisi şablonları destekler.

### <a name="limitations"></a>Sınırlamalar
- Otomatik Yenile yoktur.
- Panolar, çalışma kitapları olarak tek bir cam bölmeyle daha kullanışlı hale getirmek gibi yoğun düzeni yok. Daha fazla bilgi için daha kapsamlı içgörüler sağlayan yöneliktir.


## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) iş odaklı panolar ve raporların yanı sıra, uzun vadeli KPI eğilimlerini çözümleme raporları oluşturmak için özellikle yararlıdır. Yapabilecekleriniz [günlük sorgusu sonuçlarını alma](platform/powerbi.md) Power BI kümesine böylece farklı kaynaklardan verileri birleştirme ve raporları Web'de ve mobil cihazlarda paylaşma gibi özelliklerinden yararlanabilir.

![Power BI](media/visualizations/power-bi.png)

### <a name="advantages"></a>Yararları
- Zengin görselleştirmeler.
- Yakınlaştırma da dahil olmak üzere etkileşim ve çapraz filtreleme kapsamlı.
- Kuruluşunuz genelinde paylaşmak kolaylaşır.
- Birden çok veri kaynaklarından alınan diğer verileri ile tümleştirme.
- Küp içinde bir önbelleğe alınan sonuçları ile daha iyi performans.


### <a name="limitations"></a>Sınırlamalar
- Günlükleri destekler ancak ölçümleri değil.
- Azure tümleştirme yoktur. Panolar ve Azure Resource Manager aracılığıyla modelleri yönetemez.
- Sorgu sonuçları yapılandırmak için Power BI modele aktarılmış gerekir. Sonuç boyutu ve yenileme sınırlama.
- Sınırlı veri, her gün sekiz katı yenileyin.


## <a name="grafana"></a>Grafana
[Grafana](https://grafana.com/) işletimsel panolar excels açık bir platformdur. Algılama ve yalıtma ve işletimsel olaylar önceliklendirmek için özellikle yararlıdır. Ekleyebileceğiniz [Grafana Azure İzleyicisi veri kaynağı eklentisi](platform/grafana-plugin.md) Azure aboneliğinizde Azure ölçüler verilerinizi görselleştirmek için.

![Grafana](media/visualizations/grafana.png)

### <a name="advantages"></a>Yararları
- Zengin görselleştirmeler.
- Veri kaynakları oluşan zengin ekosistem.
- Etkileşimli veriler dahil olmak üzere yakınlaştırın.
- Parametreleri destekler.

### <a name="limitations"></a>Sınırlamalar
- Azure tümleştirme yoktur. Panolar ve Azure Resource Manager aracılığıyla modelleri yönetemez.
- Grafana bulut için ek Grafana altyapı veya ek bir maliyet desteklemek için maliyet.


## <a name="build-your-own-custom-application"></a>Kendi özel uygulamanızı oluşturun
Kendi özel Web siteleri ve uygulamalar oluşturmanıza olanak sağlayan bir REST istemcisi kullanarak API'leri verileri günlük ve ölçüm verilerini Azure İzleyici'de erişebilirsiniz.

### <a name="advantages"></a>Yararları
- Kullanıcı Arabirimi, görselleştirme, etkileşim ve özelliklerin tam esneklik sağlar.
- Ölçümleri birleştirin ve diğer veri kaynaklarıyla verilerini günlüğe kaydedebilirsiniz.

### <a name="disadvantages"></a>Dezavantajları
- Önemli mühendislik çabası gerekir.


## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [Azure İzleyici tarafından toplanan veriler](platform/data-platform.md).
- Hakkında bilgi edinin [Azure panoları](../azure-portal/azure-portal-dashboards.md).
- Hakkında bilgi edinin [Azure İzleyici görünümlerde](platform/view-designer.md).
- Hakkında bilgi edinin [çalışma kitapları](../azure-monitor/app/usage-workbooks.md).
- Hakkında bilgi edinin [günlük verilerini Power BI'a aktarma](../azure-monitor/platform/powerbi.md).
- Hakkında bilgi edinin [Grafana Azure İzleyicisi veri kaynağı eklentisi](../azure-monitor/platform/grafana-plugin.md).

