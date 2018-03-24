---
title: Azure Web Apps analitik verileri görüntüleme | Microsoft Docs
description: Tüm Azure Web uygulaması kaynaklar arasında farklı ölçümleri toplayarak, Azure Web Apps hakkında Öngörüler elde etmek için Azure Web Apps analiz çözümü kullanabilirsiniz.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: 20ff337f-b1a3-4696-9b5a-d39727a94220
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2018
ms.author: magoedte
ms.openlocfilehash: b70b626ca618fbfb7cbe25a4fcbc9aae797ce157
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="view-analytic-data-for-metrics-across-all-your-azure-web-app-resources"></a>Tüm Azure Web uygulaması kaynaklarına arasında ölçümleri ilişkin analitik verileri görüntüle

![Web uygulamaları simgesi](./media/log-analytics-azure-web-apps-analytics/azure-web-apps-analytics-symbol.png)  
Azure Web uygulamaları analizi (Önizleme) çözümü Öngörüler sunar, [Azure Web Apps](../app-service/app-service-web-overview.md) tüm Azure Web uygulaması kaynaklar arasında farklı ölçümleri toplayarak. Çözümüyle çözümlemek ve web uygulaması kaynak ölçüm verileri arayabilirsiniz.

Çözüm kullanarak görüntüleyebilirsiniz:

- En yüksek yanıt süresiyle üst Web uygulamaları
- Başarılı ve başarısız istekleri dahil olmak üzere, Web uygulamalarında istek sayısı
- En yüksek gelen ve giden trafiği ile üst Web uygulamaları
- Yüksek CPU ve bellek kullanımı ile üst hizmet planları
- Azure Web Apps etkinlik günlük işlemleri

## <a name="connected-sources"></a>Bağlı kaynaklar

Çoğu diğer günlük analizi çözümlerden farklı Azure Web uygulamaları için aracıları tarafından toplanan veriler değil. Çözüm tarafından kullanılan tüm verileri doğrudan Azure'dan gelir.

| Bağlı Kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| [Windows aracıları](log-analytics-windows-agent.md) | Hayır | Çözüm Windows aracılardan bilgi toplamaz. |
| [Linux aracıları](log-analytics-linux-agents.md) | Hayır | Çözüm, Linux aracılarını bilgi toplamaz. |
| [SCOM yönetim grubu](log-analytics-om-agents.md) | Hayır | Çözüm bağlı SCOM yönetim grubunda aracılardan gelen bilgiler toplamaz. |
| [Azure depolama hesabı](log-analytics-azure-storage.md) | Hayır | Çözüm koleksiyonu bilgileri Azure depolama biriminden değil yapar. |

## <a name="prerequisites"></a>Önkoşullar

- Azure Web uygulaması kaynak ölçüm bilgilere erişmek için bir Azure aboneliğinizin olması gerekir.

## <a name="configuration"></a>Yapılandırma

Alanlarınızı için Azure Web Apps analiz çözümü yapılandırmak için aşağıdaki adımları gerçekleştirin.

1. Azure Web Apps Analytics çözümden etkinleştirmek [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureWebAppsAnalyticsOMS?tab=Overview) veya açıklanan işlemi kullanarak [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).
2. [PowerShell kullanarak OMS için Azure kaynak ölçümleri günlük kaydını etkinleştir](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell).

Azure Web Apps analiz çözümü Azure'dan iki kümesi ölçümleri toplar:

- Azure Web Apps ölçümleri
  - Ortalama Bellek Çalışma Kümesi
  - Ortalama Yanıt Süresi
  - Bayt alınan/gönderilen
  - CPU Süresi
  - İstekler
  - Bellek çalışma kümesi
  - Httpxxx
- Uygulama hizmeti planı ölçümleri
  - Bayt alınan/gönderilen
  - CPU Yüzdesi
  - Disk Kuyruğu Uzunluğu
  - Http Kuyruk Uzunluğu
  - Bellek Yüzdesi

Ayrılmış hizmet planı kullanıyorsanız, uygulama hizmeti planı ölçümleri yalnızca toplanır. Bu ücretsiz veya paylaşılan uygulama hizmeti planları için geçerli değil.

OMS Portalı'nı kullanarak çözüm eklerseniz, aşağıdaki kutucuk görürsünüz. Yapmanız [OMS PowerShell kullanarak Azure kaynak ölçümleri günlüğünü etkinleştirme](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell).

![Değerlendirme bildirim gerçekleştirme](./media/log-analytics-azure-web-apps-analytics/performing-assessment.png)

Çözüm yapılandırdıktan sonra verileri 15 dakika içinde alanınıza akan başlamanız gerekir.

## <a name="using-the-solution"></a>Çözümü kullanma

Azure Web Apps analiz çözümü, çalışma alanına eklediğinizde **Azure Web Apps Analytics** kutucuğu, genel bakış Panosu'na eklenir. Bu kutucuğu, Azure Web uygulamaları Azure aboneliğinizde çözümü erişimi olduğundan sayısını görüntüler.

![Azure Web Apps Analytics döşeme](./media/log-analytics-azure-web-apps-analytics/azure-web-apps-analytics-tile.png)

### <a name="view-azure-web-apps-analytics-information"></a>Azure Web uygulamaları analizi bilgilerini görüntüleme

Tıklatın **Azure Web Apps Analytics** açmak için kutucuğa **Azure Web Apps Analytics** Pano. Pano Kanatlar aşağıdaki tabloda içerir. Her dikey penceresinde belirtilen kapsam ve zaman aralığı için o dikey 's ölçütlerle eşleşen en fazla on öğeleri listeler. Tıklayarak tüm kayıtları döndüren bir günlük arama çalıştırabilirsiniz **tümünü görmek** alt dikey veya dikey başlığını tıklatarak.


| Sütun | Açıklama |
| --- | --- |
| Azure Webapps |   |
| Web uygulamaları istek eğilimleri | Web uygulamaları istek eğilim seçtiğiniz tarih aralığı için bir çizgi grafiği gösterir ve üst on web isteklerinin listesini gösterir. Günlük aramasını çalıştırmak için çizgi grafiği tıklatın <code>AzureMetrics &#124; where ResourceId == "/MICROSOFT.WEB/SITES/" and (MetricName == "Requests" or MetricName startswith_cs "Http") &#124; summarize AggregatedValue = avg(Average) by MetricName, bin(TimeGenerated, 1h)</code> <br>İstek web isteği ölçüm eğilim için günlük arama çalıştırmak için bir web isteği öğesini tıklatın. |
| Web uygulamaları yanıt süresi | Bir çizgi grafiği, seçtiğiniz tarih aralığı için Web Apps yanıt süresini gösterir. Ayrıca bir listesini üst listesini gösterir on Web Apps yanıt süreleri. Günlük aramasını çalıştırmak için grafiği tıklatın <code>AzureMetrics &#124; where ResourceId == "/MICROSOFT.WEB/SITES/" and MetricName == "AverageResponseTime" &#124; summarize AggregatedValue = avg(Average) by Resource, bin(TimeGenerated, 1h)</code><br> Web uygulaması için yanıt sürelerini döndüren bir günlük arama çalıştırmak için bir Web uygulaması'i tıklatın. |
| Web Apps Traffic | MB cinsinden Web Apps trafiği için bir çizgi grafiği gösterir ve Web uygulamaları trafiği üst listeler. Günlük aramasını çalıştırmak için grafiği tıklatın <code>AzureMetrics &#124; where ResourceId == "/MICROSOFT.WEB/SITES/" and (MetricName == "BytesSent" or MetricName == "BytesReceived") &#124; summarize AggregatedValue = sum(Average) by Resource, bin(TimeGenerated, 1h)</code><br> Bu trafiği ile tüm Web uygulamaları için son dakika gösterir. Web uygulaması için gönderilen ve alınan bayt gösteren bir günlük arama çalıştırmak için bir Web uygulaması'ı tıklatın. |
| Azure uygulama hizmeti planları |   |
| Uygulama hizmeti planları CPU kullanımı ile &gt; % 80 | CPU kullanımı % 80'den büyük olan App Service planları toplam sayısını gösterir ve CPU kullanımı üst 10 App Service planları listeler. Toplam alan için bir günlük arama çalıştırmak için tıklatın <code>AzureMetrics &#124; where ResourceId == "/MICROSOFT.WEB/SERVERFARMS/" and MetricName == "CpuPercentage" &#124; summarize AggregatedValue = avg(Average) by Resource</code><br> App Service planları ve ortalama CPU kullanımlarını listesini gösterir. Bir App Service, ortalama CPU kullanımını gösteren bir günlük arama çalıştırmayı planladığınız'ı tıklatın. |
| Uygulama hizmeti planları bellek kullanımı ile &gt; % 80 | App Service bellek kullanımı % 80'den büyük olan planları toplam sayısını gösterir ve bellek kullanımı üst 10 App Service planları listeler. Toplam alan için bir günlük arama çalıştırmak için tıklatın <code>AzureMetrics &#124; where ResourceId == "/MICROSOFT.WEB/SERVERFARMS/" and MetricName == "MemoryPercentage" &#124; summarize AggregatedValue = avg(Average) by Resource</code><br> App Service planları ve bunların ortalama bellek kullanımı listesini gösterir. Bir App Service, ortalama bellek kullanımı gösteren bir günlük arama çalıştırmayı planladığınız'ı tıklatın. |
| Azure Web Apps etkinlik günlükleri |   |
| Azure Web Apps etkinlik denetim | Web uygulamaları ile toplam sayısını gösterir [etkinlik günlükleri](log-analytics-activity.md) ve ilk 10 etkinlik günlüğü işlemleri listeler. Toplam alan için bir günlük arama çalıştırmak için tıklatın <code>AzureActivity #124; where ResourceProvider == "Azure Web Sites" #124; summarize AggregatedValue = count() by OperationName</code><br> Etkinlik günlüğü işlemleri listesini gösterir. Kayıt işlemi için listeler günlük arama çalıştırmak için bir etkinlik günlüğü işlemi'ı tıklatın. |



### <a name="azure-web-apps"></a>Azure Web Apps

Panosunda, Web uygulamaları ölçümlerinizi konusunda daha fazla bilgi almak için ayrıntıya girebilirsiniz. Bu ilk Kanatlar gösterisini hatalar (örneğin, HTTP404), trafik ve ortalama yanıt süresi sayısı Web uygulamaları isteklerin eğilim zamanla ayarlayın. Ayrıca farklı Web uygulamaları için bu ölçümleri dökümünü gösterir.

![Azure Webapps dikey penceresi](./media/log-analytics-azure-web-apps-analytics/web-apps-dash01.png)

Bu verileri gösteren bir temel nedeni, böylelikle yüksek yanıt süresi ile bir Web uygulaması tanımlamak ve kök nedenini bulmak için araştırın. Bir eşik sınırını da daha fazla kolayca olanları sorunları tanımlamanıza yardımcı olması için uygulanır.

- Web uygulamaları kırmızı ile gösterilen yanıt süresi 1 saniyeden daha yüksek olması.
- Web uygulamaları turuncu içinde gösterilen 0.7 ikinci ve 1 saniyeden az maliyetinden daha yüksek bir yanıt süresi vardır.
- Web uygulamaları yeşil renkte gösterilir 0,7'den küçük bir yanıt süresi ikinci gerekir.

Aşağıdaki günlük arama örnek görüntüde, görebilirsiniz *anugup3* web uygulaması, web uygulamaları'den çok daha yüksek bir yanıt süresi vardı.

![Günlük arama örneği](./media/log-analytics-azure-web-apps-analytics/web-app-search-example.png)

### <a name="app-service-plans"></a>App Service Planları

Ayrılmış hizmet planları kullanıyorsanız, App Service planları için ölçümleri de toplayabilir. Bu görünümde, App Service planları yüksek CPU veya bellek kullanımı ile bakın (&gt; % 80). Ayrıca, yüksek bellek veya CPU kullanımı ile üst uygulama hizmetleri gösterir. Benzer şekilde, daha fazla kolayca olanları sorunları tanımlamanıza yardımcı olması için bir eşik sınır uygulanır.

- Kırmızı ile gösterilen App Service planları CPU/bellek kullanımı % 80 ' daha yüksek olması.
- Uygulama hizmeti planları turuncu içinde gösterilen bir CPU/bellek kullanımı % 60 daha yüksek ve % 80'den daha düşük sahip.
- Yeşil renkte gösterilir App Service planları CPU/bellek kullanımı % 60 ' daha düşük sahip.

![Azure App Service planları dikey penceresi](./media/log-analytics-azure-web-apps-analytics/web-apps-dash02.png)

## <a name="azure-web-apps-log-searches"></a>Azure Web Apps Günlük aramalar

**Listesi, popüler Azure Web Apps arama sorguları** Web Apps kaynaklarınız üzerinde gerçekleştirilen işlemler Öngörüler sağlayan Web uygulamaları için tüm ilgili etkinlik günlükleri gösterir. Ayrıca tüm ilgili işlemler ve oluşturulduklarını sayısı listelenir.

Günlük arama sorguları herhangi bir başlangıç noktası olarak kullanarak, bir uyarı kolayca oluşturabilirsiniz. Örneğin, ölçüm 's ortalama yanıt süresi 1 saniyede büyük olduğunda bir uyarı oluşturmak isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Oluşturma bir [uyarı](log-analytics-alerts-creating.md) belirli bir ölçüm için.
- Kullanım [günlük arama](log-analytics-log-searches.md) , etkinlik günlükleri ayrıntılı bilgileri görüntülemek için.
