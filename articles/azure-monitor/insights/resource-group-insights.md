---
title: Azure İzleyici kaynak grubu öngörüleri | Microsoft Docs
description: Sistem durumu ve dağıtılmış uygulamalar ve Hizmetleri Azure İzleyici ile kaynak grubu düzeyinde performansını anlama
services: azure-monitor
author: NumberByColors
manager: carmonm
ms.service: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/19/2018
ms.reviewer: mbullwin
ms.author: daviste
ms.openlocfilehash: d5c07e0d4aca8bda42ea9f78a1475ea7bb5861f0
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62119431"
---
# <a name="monitor-resource-groups-with-azure-monitor-preview"></a>Kaynak grupları, Azure İzleyici (Önizleme) ile izleme

Modern uygulamalar genellikle karmaşık ve yüksek oranda dağıtılmış bir hizmet sunmak için birlikte çalışan çok sayıda ayrı bölümleri. Bu karmaşıklığı tanıma, Azure İzleyici kaynak grupları için izleme Öngörüler sağlar. Bu, önceliklendirmenize ve tanılamanıza, kaynakların karşılaştığınız, durumunu ve performansını kaynak grubunun bağlam sunmaya devam ederken herhangi bir sorunu kolaylaştırır&mdash;ve uygulamanızı&mdash;bir bütün olarak.

## <a name="access-insights-for-resource-groups"></a>Kaynak grupları için erişim ınsights

1. Seçin **kaynak grupları** sol taraftaki gezinti çubuğundan.
2. Kaynak gruplarınızın keşfetmek istediğiniz birini seçin. (Varsa çok sayıda kaynak grupları aboneliğe göre filtreleme bazen yararlı olabilir.)
3. Bir kaynak grubu için ınsights erişmek için tıklayın **Insights** herhangi bir kaynak grubu, sol taraftaki menüde.

![Kaynak grubu ınsights genel bakış sayfasının ekran görüntüsü](./media/resource-group-insights/0001-overview.png)

## <a name="resources-with-active-alerts-and-health-issues"></a>Kaynakları etkin uyarıları ve sistem durumu sorunları

Genel bakış sayfasında kaç uyarıları harekete ve geçerli Azure kaynak durumu yanı sıra her bir kaynağın hala etkin olduğunu gösterir. Birlikte, bu bilgiler hızlı bir şekilde sorun yaşayan herhangi bir kaynağa nokta yardımcı olabilir. Uyarılar kodunuzu ve altyapınızı nasıl yapılandırdığınıza sorunları belirlemenize yardımcı olur. Azure kaynak durumu yüzeyleri sorunu Azure platformu ile birlikte, tek tek uygulamalarınıza özgü değildir.

![Azure kaynak durumu ekran bölmesi](./media/resource-group-insights/0002-overview.png)

### <a name="azure-resource-health"></a>Azure Kaynak Durumu

Azure kaynak durumu görüntülemek için kontrol **Azure kaynak durumu göster** kutusunun üstünde tablo. Bu sütun, hızlı yükleme sayfası yardımcı olmak için varsayılan olarak gizlidir.

![Eklenen kaynak sistem durumu grafiği ekran görüntüsü](./media/resource-group-insights/0003-overview.png)

Varsayılan olarak, kaynaklar, uygulama katmanı ve kaynak türüne göre gruplandırılır. **Uygulama katmanı** yalnızca kaynak grubu ınsights'a genel bakış sayfasının bağlamı içinde var olan basit bir kategori kaynak türlerinin olduğu. İşlem altyapısı, ağ, depolama + veritabanları, uygulama koduna ilgili kaynak türleri vardır. Yönetim Araçları edinin, kendi uygulama katmanları ve her bir kaynağın ait olarak kategorilere ayrılır **diğer** uygulama katmanı. Bu gruplandırma, hangi alt sistemler, uygulamanızın sağlıklı ve sağlıksız bir bakışta görmenize yardımcı olabilir.

## <a name="diagnose-issues-in-your-resource-group"></a>Kaynak grubunuzda sorunlarını tanılayın

Kaynak grubu içgörüler sayfasında sorunları tanılamanıza yardımcı olacak kapsamlı çeşitli araçlar sağlar.

   |         |          |
   | ---------------- |:-----|
   | [**Uyarıları**](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts)      |  Görüntüleme, oluşturma ve uyarılarınızı yönetme. |
   | [**Ölçümleri**](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) | Görselleştirme ve ölçüm tabanlı verilerinizi keşfedin.    |
   | [**Etkinlik günlükleri**](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) | Azure'da gerçekleşen abonelik düzeyinde olaylar.  |
   | [**Uygulama eşlemesi**](https://docs.microsoft.com/azure/application-insights/app-insights-app-map) | Performans sorunlarını veya hata etkin noktaları tanımlamak için Dağıtılmış uygulamanızın topolojisini gidin. |

## <a name="failures-and-performance"></a>Hataları ve performans

Uygulamanızın ne etmişsinizdir yavaş çalışıyor veya kullanıcılar hata bildirmiştir? Bu zaman tüm sorunları yalıtmak için kaynaklarınızın aramak için alır.

**Performans** ve **hataları** sekmeler, performans ve başarısızlık tanı görünümlerini pek çok ortak kaynak türleri için bir araya tarafından bu işlem basitleştirin.

Çoğu kaynak türleri, Azure İzleyici çalışma galeri şablonları açılır. Oluşturduğunuz her bir çalışma kitabının kaydedilmiş, ekibinizle paylaşılan ve gelecekte benzer sorunları tanılamak için yeniden kullanılan özelleştirilebilir.

### <a name="investigate-failures"></a>Hataları araştır

Hataları sekme seçimine test etmek için **hataları** altında **Araştır** sol menüdeki.

Yeni seçenekler sunan seçiminizi yapıldıktan sonra sol taraftaki menüde çubuğunu değiştirir.

![Başarısız ekran genel bakış bölmesi](./media/resource-group-insights/00004-failures.png)

App Service seçildiğinde, Azure İzleyici çalışma galeri şablonları ile sunulur.

![Uygulama çalışma kitabı galeri görüntüsü](./media/resource-group-insights/0005-failure-insights-workbook.png)

Hata Öngörüler için şablon seçme, çalışma kitabını açın.

![Hata raporunun ekran görüntüsü](./media/resource-group-insights/0006-failure-visual.png)

Herhangi bir satır seçebilirsiniz. Seçimi daha sonra bir grafik Ayrıntıları görünümünde görüntülenir.

![Hata ayrıntılarının ekran görüntüsü](./media/resource-group-insights/0007-failure-details.png)

Çalışma kitapları, özel raporlar ve görselleştirmeler bir kolayca kullanılabilir biçime oluşturmanın zor işi hemen soyut. Çalışma kitapları, önceden oluşturulmuş parametrelerini ayarlamak bazı kullanıcılar yalnızca isteyebilirsiniz, ancak tamamen özelleştirilebilir.

Bu çalışma kitabı dahili olarak nasıl çalıştığını bir fikir almak için seçin **Düzenle** üst çubuktaki.

![Ek düzenleme seçeneğinin ekran görüntüsü](./media/resource-group-insights/0008-failure-edit.png)

Bir dizi **Düzenle** kutuları çalışma kitabında çeşitli öğelerin yanında görünür. Seçin **Düzenle** işlemlerinin tablonun altındaki kutusu.

![Düzenleme kutularının ekran görüntüsü](./media/resource-group-insights/0009-failure-edit-graph.png)

Bu tablo görselleştirmesine yönlendiren temel alınan günlük sorgusu ortaya çıkarır.

 ![Günlük Sorgu penceresinin ekran görüntüsü](./media/resource-group-insights/0010-failure-edit-query.png)

Sorguyu doğrudan değiştirebilirsiniz. Referans olarak kullanın ve bundan kendi özel parametreli çalışma kitabı tasarlarken ödünç alın.

### <a name="investigate-performance"></a>Performansı araştır

Çalışma kitaplarının kendi galeri performans sunar. App Service için önceden oluşturulmuş uygulama performansını çalışma kitabı aşağıdaki görünümü sunar:

 ![Performans görünümünün ekran görüntüsü](./media/resource-group-insights/0011-performance.png)

Bu durumda, düzen seçerseniz bu görselleştirmeler kümesini Azure İzleyici ölçümleri tarafından güçlendirilmiştir görürsünüz.

 ![Azure ölçümleri ile performans görünümünün ekran görüntüsü](./media/resource-group-insights/0012-performance-metrics.png)

## <a name="troubleshooting"></a>Sorun giderme

### <a name="enabling-access-to-alerts"></a>Uyarılar erişimini etkinleştirme

Kaynak gruplar için Azure İzleyici'de uyarıları görmek için birisi bu aboneliğin sahibi veya katkıda bulunan rolüne sahip herhangi bir kaynak grubu aboneliği için kaynak gruplar için Azure İzleyici açması gerekir. Bu Abonelikteki kaynak gruplarının tüm kaynak grupları için Azure İzleyici'de uyarıları görmek için okuma erişimi olan herkes olanak sağlar. Bir sahibi veya katkıda bulunan rolü varsa, birkaç dakika sonra bu sayfayı yenileyin.

Kaynak grupları için Azure İzleyici uyarı durumu almak için Azure İzleyici uyarı yönetim sisteminde kullanır. Uyarılar Yönetimi varsayılan olarak her bir kaynak grubu ve abonelik için yapılandırılmamış ve sahibi veya katkıda bulunan rolüne sahip bir kişi tarafından yalnızca etkinleştirilebilir. Bu olabilir ya da etkin:
* Azure İzleyici kaynak grupları için herhangi bir kaynak grubu aboneliği için açılıyor.
* Veya tıklayarak aboneliğe giderek **kaynak sağlayıcıları**, ardından **kaydetmek için Alerts.Management**.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İzleyici çalışma kitapları](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)
- [Azure Kaynak Durumu](https://docs.microsoft.com/azure/service-health/resource-health-overview)
- [Azure İzleyici uyarıları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts)
