---
title: Azure İzleyicisi'nde ölçümler | Microsoft Docs
description: Azure İzleyici'de izleme verileri gerçek zamanlı senaryoları destekleme kapasitesine sahip hafif ölçümleri açıklar.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2019
ms.author: bwren
ms.openlocfilehash: 487f70e4055f16c56092f2f970d2a34238e7febe
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851994"
---
# <a name="metrics-in-azure-monitor"></a>Azure İzleyicisi'nde ölçümler

> [!NOTE]
> Azure İzleyicisi'ni bir veri platformu, iki temel veri türleri üzerinde temel alır: Ölçüm ve günlükleri. Bu makalede ölçümleri açıklanır. Başvurmak [Azure İzleyici'de oturum](data-platform-logs.md) günlükleri ve çok ayrıntılı bir açıklaması için [Azure İzleyici veri platforn](data-platform.md) iki bir karşılaştırması.


Azure İzleyicisi'nde ölçümler, basit ve neredeyse gerçek zamanlı senaryoları için özellikle yararlı hale getirme destekleme yeteneği sorunlarını uyarılar ve hızlı algılanması. Bu makalede nasıl ölçümleri, bunları ile yapabileceklerinizi yapılandırılmıştır açıklar ve ölçümleri, veri depolama farklı veri kaynaklarını tanımlar.

## <a name="what-are-metrics"></a>Ölçümler nelerdir?
Sayısal değerleri, belirli bir zamanda bir sistem bazı yönlerini açıklamak ölçümleridir. Ölçümler, düzenli aralıklarla toplanır ve sık örneklenebilir ve bir uyarı ile göreceli olarak basit bir mantıksal hızla harekete çünkü uyarmak için yararlıdır.

## <a name="what-can-you-do-with-azure-monitor-metrics"></a>Azure İzleyici ölçümleri ile neler?
Aşağıdaki tabloda, ölçüm verilerini Azure İzleyici'de kullanabileceğiniz farklı yollarını listeler.

|  |  |
|:---|:---|
| Çözümleme | Kullanım [ölçüm Gezgini](metrics-charts.md) bir grafikteki toplanan ölçümlerin analiz edin ve farklı kaynaklardan ölçümleri karşılaştırmak için. |
| Görselleştirme | Ölçüm Gezgini için bir grafik sabitleme bir [Azure panosuna](../learn/tutorial-app-dashboards.md).<br>Oluşturma bir [çalışma kitabı](../app/usage-workbooks.md) birden çok etkileşimli bir rapordaki veri kümesi ile birleştirilecek. Sorgu sonuçlarını dışarı aktarma [Grafana](grafana-plugin.md) kendi yönelik Kompozit yararlanın ve diğer veri kaynaklarıyla birleştirmek için. |
| Uyarı | Yapılandırma bir [ölçüm uyarısı kuralının](alerts-metric.md) bildirim gönderen veya alan [eylemi otomatik](action-groups.md) ölçüm değeri bir eşiği aştığında zaman. |
| Otomatikleştirme |  Kullanım [otomatik ölçeklendirme](autoscale-overview.md) artırabilir veya azaltabilirsiniz bir Eşiği aşan bir ölçüm değeri temel alarak kaynakları. |
| Dışarı Aktarma | [Rota ölçümleri günlüklerine](diagnostic-logs-stream-log-store.md) Azure İzleyici ölçümleri verileri birlikte Azure İzleyici günlüklerine verileri analiz etmek ve ölçüm değerleri 93 günden daha uzun süre saklamak için.<br>Stream için ölçümleri bir [olay hub'ı](stream-monitoring-data-event-hubs.md) dış sisteme yönlendirmek. |
| Al | Erişim ölçüm değerleri kullanarak bir komut satırı [PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.insights/?view=azurermps-6.7.0)<br>Ölçüm değerleri kullanarak özel uygulama erişimi [REST API](rest-api-walkthrough.md).<br>Erişim ölçüm değerleri kullanarak bir komut satırı [CLI](/azure/monitor/metrics). |
| Arşiv | [Arşiv](..//learn/tutorial-archive-data.md) kaynağınızın denetim ya da çevrimdışı raporlamaya uyumluluk, performans veya sistem durumu geçmişi. |


## <a name="how-is-data-in-azure-monitor-metrics-structured"></a>Azure İzleyici ölçümleri yapılandırılmış verileri nasıl mi?
Azure İzleyici ölçümleri tarafından toplanan veriler, zaman damgası veri çözümlemesi için iyileştirilmiş bir zaman serisi veritabanına depolanır. Her ölçüm değerleri aşağıdaki özelliklere sahip bir zaman serisi kümesidir:

* Değer toplandığı zaman
* Kaynak değeri ile ilişkili
* Ölçüm için bir kategori gibi davranan bir ad alanı
* Bir ölçüm adı
* Değer
* Bazı ölçümler açıklandığı gibi birden çok boyutta olabilir [çok boyutlu ölçümler](#multi-dimensional-metrics). Özel ölçümler, en fazla 10 boyuta sahip olabilir.

Azure'da ölçümleri 93 gün boyunca saklanır. Yapabilecekleriniz [platformu Azure İzleyici kaynak ölçümlerini bir Log Analytics çalışma alanına gönderme](diagnostic-logs-stream-log-store.md) uzun vadeli eğilimleri belirlemek için.

## <a name="multi-dimensional-metrics"></a>Çok boyutlu ölçümleri
Ölçüm verilerini zorlukları bilgileri için toplanan değerler bağlam sağlamak için çoğunlukla sınırlıdır biridir. Azure İzleyici, çok boyutlu ölçümler ile bu sorunu giderir. Bir ölçüm boyutlarını ölçüm değeri tanımlamak için ek veri taşıyan ad-değer çiftleridir. Örneğin, bir ölçüm _kullanılabilir disk alanı_ adlı bir boyutun olabilir _sürücü_ değerlerle _C:_, _D:_, hangi görüntüleme izin veya kullanılabilir disk alanı tüm sürücüler her biri için ayrı ayrı sürücü.

Aşağıdaki örnekte adlı kuramsal bir ölçüm için iki veri kümesi gösterildiği _ağ aktarım hızı_. İlk veri kümesi herhangi bir boyutu vardır. İki boyutlu değerlerle ikinci bir veri kümesi gösterir _IP adresi_ ve _yönü_:

### <a name="network-throughput"></a>Ağ aktarım hızı

| Zaman damgası     | Ölçüm değeri |
| ------------- |:-------------|
| 8/9/2017 8:14 | 1,331.8 KB/sn |
| 8/9/2017 8:15 | 1,141.4 KB/sn |
| 8/9/2017 8:16 | 1,110.2 KB/sn |

Temel bir soru cevap ister yalnızca "my ağ aktarım hızı belirli bir zamanda neydi?" boyutlu Bu ölçüm olabilir.

### <a name="network-throughput--two-dimensions-ip-and-direction"></a>Ağ aktarım hızı + iki boyutlu ("IP" ve "Yönü")

| Zaman damgası     | Boyut "IP"   | Boyut "Yönü" | Ölçüm değeri|
| ------------- |:-----------------|:------------------- |:-----------|
| 8/9/2017 8:14 | IP "192.168.5.2" = | Yön "Gönder" =    | 646.5 KB/sn |
| 8/9/2017 8:14 | IP "192.168.5.2" = | Yön "Al" = | 420.1 KB/sn |
| 8/9/2017 8:14 | IP "10.24.2.15" =  | Yön "Gönder" =    | 150.0 KB/sn |
| 8/9/2017 8:14 | IP "10.24.2.15" =  | Yön "Al" = | 115.2 KB/sn |
| 8/9/2017 8:15 | IP "192.168.5.2" = | Yön "Gönder" =    | 515.2 KB/sn |
| 8/9/2017 8:15 | IP "192.168.5.2" = | Yön "Al" = | 371.1 KB/sn |
| 8/9/2017 8:15 | IP "10.24.2.15" =  | Yön "Gönder" =    | 155.0 KB/sn |
| 8/9/2017 8:15 | IP "10.24.2.15" =  | Yön "Al" = | 100.1 KB/sn |

Bu ölçüm, "ağ aktarım hızı için her bir IP adresi neydi?" ve "karşı gönderilen veri miktarını alındı?" gibi soruları yanıtlayabilirsiniz Çok boyutlu ölçümler, boyutsuz ölçümler için kıyasla ek analiz ve tanılama değer taşır.

## <a name="interacting-with-azure-monitor-metrics"></a>Azure İzleyici ölçümleri ile etkileşim kurma
Kullanım [ölçüm Gezgini](metrics-charts.md) etkileşimli olarak ölçüm veritabanınızdaki verileri analiz etme ve zaman içinde birden çok ölçüm değerleri grafik. Grafikler ile diğer görselleştirmeleri görüntülemek için panoya sabitleyebilirsiniz. Ölçümleri kullanarak da alabilirsiniz [Azure REST API izleme](rest-api-walkthrough.md).

![Ölçüm Gezgini](media/data-platform/metrics-explorer.png)

## <a name="sources-of-azure-monitor-metrics"></a>Azure İzleyici ölçümleri kaynakları
Azure İzleyici tarafından toplanan ölçümleri üç temel kaynakları vardır. Bu ölçümler Azure İzleyici ölçüm veritabanında toplandıktan sonra kaynak bağımsız olarak birlikte değerlendirilebilir.

**Platform ölçümleri** Azure kaynakları tarafından oluşturulur ve bunların sistem durumu ve performans görünürlük sağlar. Her kaynak türünü oluşturur bir [farklı ölçüm kümesini](metrics-supported.md) gerekli herhangi bir yapılandırma olmadan. Platform ölçümleri ölçüm 's tanımında aksi belirtilmediği sürece, bir dakikalık sıklıkta Azure kaynaklarından toplanır. 

**Konuk işletim sistemi ölçümleri** bir sanal makine konuk işletim sisteminden toplanır. İle Windows sanal makineler için konuk işletim sistemi ölçümlerini etkinleştirmeniz [Windows Tanılama uzantısı (WAD)](../platform/diagnostics-extension-overview.md) ve Linux sanal makineleri ile [InfluxData Telegraf aracı](https://www.influxdata.com/time-series-platform/telegraf/).

**Uygulama ölçümleri** performans sorunları tespit edin ve eğilimler, uygulamanızın nasıl kullanıldığını izlemenize yardımcı olur ve izlenen uygulamalar için Application Insights tarafından oluşturulur. Bu tür değerleri olarak içerir _sunucu yanıt süresi_ ve _tarayıcı özel durumları_.

**Özel ölçümler** otomatik olarak kullanılabilir olan standart ölçümlerin yanı sıra tanımladığınız ölçümleridir. Yapabilecekleriniz [uygulamanızda özel ölçümler tanımlayın](../app/api-custom-events-metrics.md) kullanarak bir Azure hizmeti için özel ölçümleri oluşturma veya Application Insights tarafından izlenen [özel ölçümler API](metrics-store-custom-rest-api.md).


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [Azure İzleyici, veri platformu](data-platform.md).
- Hakkında bilgi edinin [günlük verilerini Azure İzleyici'de](data-platform-logs.md).
- Hakkında bilgi edinin [izleme verilerini kullanılabilir](data-sources.md) azure'daki farklı kaynakları.
