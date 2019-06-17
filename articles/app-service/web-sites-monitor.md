---
title: Uygulamalar - Azure uygulama hizmeti izleme | Microsoft Docs
description: Azure portalını kullanarak Azure App Service'te uygulamaları izleme hakkında bilgi edinin.
services: app-service
documentationcenter: ''
author: btardif
manager: erikre
editor: ''
ms.assetid: d273da4e-07de-48e0-b99d-4020d84a425e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2019
ms.author: byvinyal
ms.custom: seodec18
ms.openlocfilehash: a5d4d13d8e60cd7f273363a9bc385098e15cbb71
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60832599"
---
# <a name="monitor-apps-in-azure-app-service"></a>Azure App Service'te uygulamaları izleme
[Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) web uygulamaları, mobil arka uçlar ve API uygulamaları, yerleşik izleme işlevselliği sağlayan [Azure portalında](https://portal.azure.com).

Azure portalında inceleyebilirsiniz *kotalar* ve *ölçümleri* bir uygulama için App Service planını gözden geçirin ve otomatik olarak ayarlanan *uyarılar* ve *ölçeklendirme* ölçümlerine bağlıdır.

## <a name="understand-quotas"></a>Kotaları anlama

Belirli sınırları kullanabilmeleri için kaynaklar üzerinde App Service'te barındırılan uygulamaları tabidir. Uygulama ile ilişkili App Service planı sınırları tanımlanır.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Uygulama içinde barındırılıyorsa bir *ücretsiz* veya *paylaşılan* planı, app kullanabileceği kaynakları barındırabileceğiniz kotalar tarafından tanımlanır.

Uygulama içinde barındırılıyorsa bir *temel*, *standart*, veya *Premium* planı sınırları kullanabilmeleri için kaynaklar ayarlanır *boyutu* () Küçük, Orta, büyük) ve *örnek sayısı* (1, 2, 3 ve benzeri) App Service planı.

Ücretsiz veya paylaşılan uygulamalar için kotalar şunlardır:

| Kota | Açıklama |
| --- | --- |
| **CPU (kısa)** | Bu uygulama için 5 dakikalık bir aralığı izin verilen CPU miktarı. Bu kota, beş dakikada sıfırlar. |
| **CPU (gün)** | Bu uygulama için bir gün içinde izin verilen CPU toplam miktarı. Bu kota, 24 saatte gece yarısı UTC sıfırlar. |
| **Bellek** | Bu uygulama için izin verilen bellek miktarı. |
| **Bant genişliği** | Bu uygulama için bir gün içinde izin verilen giden bant genişliğinin toplam miktarı. Bu kota, 24 saatte gece yarısı UTC sıfırlar. |
| **dosya sistemi** | İzin verilen depolama alanı miktarı. |

İçinde barındırılan uygulamalar için geçerli tek kota *temel*, *standart*, ve *Premium* planları olan dosya sistemi.

Belirli kotalar, sınırlar ve çeşitli App Service SKU'ları için kullanılabilen özellikler hakkında daha fazla bilgi için bkz: [Azure abonelik hizmeti limitleri](../azure-subscription-service-limits.md#app-service-limits).

### <a name="quota-enforcement"></a>Kota uygulama

Bir uygulama aşarsa *CPU (kısa)* , *CPU (gün)* , veya *bant genişliği* kota sıfırlar kadar kotası, uygulama durduruldu. Bu süre boyunca tüm gelen istekleri bir HTTP 403 hata neden olur.

![403 hata iletisi][http403]

Uygulama bellek kotası aşıldığında, uygulamayı yeniden başlatılır.

Dosya sistemi kotası aşıldığında, herhangi bir işlem başarısız yazma. Günlükler için herhangi bir yazma işlemi hatalarını dahil yazın.

App Service planınızı yükselterek kotaları uygulamanızdan kaldırmak veya artırabilirsiniz.

## <a name="understand-metrics"></a>Ölçüleri anlama

Ölçümler, planın davranışını uygulama veya App Service hakkında bilgi sağlar.

Bir uygulama için kullanılabilir ölçümleri şunlardır:

| Ölçüm | Açıklama |
| --- | --- |
| **Ortalama yanıt süresi** | Milisaniye cinsinden istek, hizmet uygulaması için geçen ortalama süre. |
| **Ortalama bellek çalışma kümesi** | Ortalama megabayt (MIB), uygulama tarafından kullanılan bellek miktarı. |
| **Bağlantılar** | (W3wp.exe ve onun alt işlemleri) korumalı alanında var olan ilişkili yuva sayısı.  Bağlanmış bir yuva bind()/connect() API'lerini çağırarak oluşturulur ve söz konusu yuvası ile CloseHandle()/closesocket() kapatılana kadar kalır. |
| **CPU süresi** | Saniyeler içinde uygulama tarafından tüketilen CPU miktarı. Bu ölçüm hakkında daha fazla bilgi için bkz: [CPU zamanı vs CPU yüzdesi](#cpu-time-vs-cpu-percentage). |
| **Geçerli derlemeler** | Bu uygulamadaki tüm uygulama etki alanları arasında yüklenen derlemelerin geçerli sayısı. |
| **Verileri** | Uygulamada MIB tarafından gelen bant genişliği miktarı. |
| **Veri çıkışı** | Uygulamada MIB tarafından tüketilen giden bant genişliği miktarı. |
| **Gen 0 atık toplamaları** | Nesil 0 çöp nesnelerdir sayıda uygulama işlemi başladığından bu yana toplanmadı. Daha yüksek nesil GC'ler tüm alt nesil GC'ler içerir.|
| **Gen 1 çöp koleksiyonları** | 1\. nesil nesneler çöp olan sayıda uygulama işlemi başladığından bu yana toplanır. Daha yüksek nesil GC'ler tüm alt nesil GC'ler içerir.|
| **Gen 2 Atık toplamaları** | 2\. nesil nesneler çöp olan sayıda uygulama işlemi başladığından bu yana toplanır.|
| **Tanıtıcı sayısı** | Tanıtıcıları toplam sayısı, uygulama işlem tarafından şu anda açık.|
| **HTTP 2xx** | Bir HTTP durum kodunda ≥ 200 ancak < 300 kaynaklanan isteklerin sayısı. |
| **HTTP 3xx** | Bir HTTP durum kodunda ≥ 300 ancak < 400 kaynaklanan isteklerin sayısı. |
| **HTTP 401** | HTTP 401 durum kodunu kaynaklanan isteklerin sayısı. |
| **HTTP 403** | HTTP 403 durum kodunda sonuçlanan isteklerinin sayısı. |
| **HTTP 404** | HTTP 404 durum kodu kaynaklanan isteklerin sayısı. |
| **HTTP 406** | HTTP 406 durum kodunda sonuçlanan isteklerinin sayısı. |
| **HTTP 4xx** | Bir HTTP durum kodunda ≥ 400 ancak < 500 kaynaklanan isteklerin sayısı. |
| **HTTP sunucu hataları** | Bir HTTP durum kodunda ≥ 500 ancak < 600 kaynaklanan isteklerin sayısı. |
| **GÇ diğer bayt / saniye** | Hangi uygulama işlemini bayt denetimi işlemleri gibi verileri içermeyen g/ç işlemleri için oranıdır.|
| **GÇ diğer işlemler / saniye** | Uygulama işlemi okuma ve yazma işlemlerinin g/ç işlemleri kesme oranı.|
| **GÇ Okunan Bayt / saniye** | Uygulama işlemi g/ç işlemlerinde bayt okuduğu oranı.|
| **GÇ Okuma işlemi / saniye** | Okuma g/ç işlemleri, uygulama işlemi gerçekleştirme hızıdır.|
| **GÇ yazılan bayt / saniye** | Uygulama işlemi için g/ç işlemleri bayt yazma oranı.|
| **GÇ Yazma işlemi / saniye** | Uygulama işlemi yazma g/ç işlemleri kesme oranı.|
| **Bellek çalışma kümesi** | Uygulamada MIB tarafından kullanılan bellek miktarı. |
| **Özel bayt sayısı** | Özel baytlar diğer işlemlerle paylaşılamayan uygulama işleminin ayırdığı bellek bayt cinsinden geçerli boyutudur.|
| **İstekleri** | Sonuçta elde edilen HTTP durum kodlarını ne olursa olsun istekleri toplam sayısı. |
| **Uygulama kuyruğundaki istekler** | İçindeki uygulama istek sırasındaki isteklerin sayısı.|
| **İş parçacığı sayısı** | Uygulama işleminde şu anda etkin iş parçacığı sayısı.|
| **Toplam uygulama etki alanları** | Bu uygulamada yüklenen geçerli uygulama etki alanları sayısı.|
| **Yüklenmemiş toplam uygulama etki alanları** | Toplam uygulama etki alanları sayısı, uygulama başlangıcından kaldırıldı.|


Bir App Service planı için mevcut ölçümleri şunlardır:

> [!NOTE]
> App Service planı ölçümleri yalnızca planlarında kullanılabilir *temel*, *standart*, ve *Premium* katmanları.
> 

| Ölçüm | Açıklama |
| --- | --- |
| **CPU yüzdesi** | Ortalama CPU planının tüm örneklerinde kullanılır. |
| **Bellek yüzdesi** | Ortalama bellek planının tüm örneklerinde kullanılır. |
| **Verileri** | Planın tüm örneklerde kullanılan ortalama gelen bant genişliği. |
| **Veri çıkışı** | Giden bant genişliği planının tüm örneklerinde kullanılan ortalama. |
| **Disk kuyruğu uzunluğu** | Ortalama sayısı hem okuma hem de depolama üzerinde sıraya alınan istek yazma. Yüksek disk sırası uzunluğu aşırı disk g/ç nedeniyle yavaşlamasıdır bir uygulamanın göstergesidir. |
| **HTTP kuyruk uzunluğu** | Yerine önce kuyrukta oturmak olan HTTP isteklerinin ortalama sayısı. Yükselen bir HTTP kuyruk uzunluğu, ağır yük altında bir plan belirtisidir. |

### <a name="cpu-time-vs-cpu-percentage"></a>CPU zamanı vs CPU yüzdesi
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

CPU kullanımı yansıtan iki ölçüm vardır:

**CPU süresi**: Uygulamaları ücretsiz barındırılan veya kotalarını birini uygulama tarafından kullanılan CPU dakikalar içinde tanımlandığından, planları, paylaşılan için kullanışlıdır.

**CPU yüzdesi**: Temel, standart ve Premium planlardaki dışa Genişletilebilir çünkü barındırılan uygulamalar için yararlıdır. CPU yüzdesi kullanımı genel bir göstergesidir tüm örneklerinde ' dir.

## <a name="metrics-granularity-and-retention-policy"></a>Ölçümleri ayrıntı düzeyi ve bekletme ilkesi
Bir uygulama ve app service planı ölçümleri günlüğe ve aşağıdaki ayrıntı düzeyi ve bekletme ilkeleri, hizmet tarafından toplanan:

* **Dakika** ayrıntı düzeyi ölçümler, 30 saat boyunca bekletilir.
* **Saat** ayrıntı düzeyi ölçümler, 30 gün boyunca bekletilir.
* **Gün** ayrıntı düzeyi ölçümler, 30 gün boyunca bekletilir.

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Kotalar ve Azure portalında ölçümleri izleme
Çeşitli kotalarını ve uygulama etkileyen ölçümleri durumunu gözden geçirmek için Git [Azure portalında](https://portal.azure.com).

![Azure portalında kotalar grafiği][quotas]

Kotalar bulmak için seçin **ayarları** > **kotalar**. Grafikte gözden geçirebilirsiniz: 
1. Kota adı.
1. Sıfırlama aralığı.
1. Geçerli sınırı.
1. Geçerli değeri.

![Azure portalında ölçüm grafiğini][metrics] ölçümleri doğrudan erişebileceğiniz **kaynak** sayfası. Grafik özelleştirmek için: 
1. Grafiği seçin.
1. Seçin **grafiği Düzenle**.
1. Düzen **zaman aralığı**.
1. Düzen **grafik türü**.
1. Görüntülemek istediğiniz ölçümleri düzenleyin.  

Ölçümler hakkında daha fazla bilgi edinmek için [hizmet ölçümlerini izleme](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Uyarıları ve otomatik ölçeklendirme
Bir uygulama veya bir App Service planı ölçümleri için uyarılar bağlanabilir. Daha fazla bilgi için bkz. [Uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-alerts-portal.md).

Temel, standart veya Premium App Service planları destek autoscale barındırılan app Service uygulamaları. Otomatik ölçeklendirme sayesinde, App Service planı ölçümleri izleme kuralları yapılandırabilirsiniz. Kuralları artırabilir veya gerektiği gibi ek kaynaklar sağlayan örnek sayısını azaltabilirsiniz. Kurallar, uygulamanın aşırı sağlanan tam kapasiteye ulaşmadığında tasarruf da yardımcı olabilir.

Otomatik ölçeklendirme hakkında daha fazla bilgi için bkz: [ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md) ve [Azure İzleyici otomatik ölçeklendirme için en iyi yöntemler](../azure-monitor/platform/autoscale-best-practices.md).

[fzilla]:https://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:https://go.microsoft.com/fwlink/?LinkID=309169

<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png