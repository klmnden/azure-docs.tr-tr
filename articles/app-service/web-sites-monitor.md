---
title: Azure App Service'te uygulamaları izleme | Microsoft Docs
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
ms.date: 11/28/2017
ms.author: byvinyal
ms.openlocfilehash: 6334b4cc50bfa6dca709fdc9d65938f0fec3ad1c
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956781"
---
# <a name="how-to-monitor-apps-in-azure-app-service"></a>Nasıl yapılır: Azure App Service'te uygulamaları izleme
[App Service](https://go.microsoft.com/fwlink/?LinkId=529714) yerleşik izleme işlevselliği sağlayan [Azure portalında](https://portal.azure.com).
Azure portalında, gözden geçirme olanağı bulunur **kotalar** ve **ölçümleri** ayarlama, App Service planı yanı sıra bir uygulama için **uyarılar** ve hatta **ölçeklendirme**  bu ölçümlere göre otomatik olarak.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Kotaları anlama ve ölçümler
### <a name="quotas"></a>Kotalar
App Service'te barındırılan uygulamalar olan belirli tabi *sınırları* kullanabilecekleri kaynaklar. Sınırları tarafından tanımlanan **App Service planı** uygulama ile ilişkili.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Uygulama içinde barındırılıyorsa bir **ücretsiz** veya **paylaşılan** uygulama kullanabileceğiniz kaynaklar üzerindeki sınırları tarafından tanımlanan sonra plan **kotalar**.

Uygulama içinde barındırılıyorsa bir **temel**, **standart** veya **Premium** kullanabilecekleri kaynaklarını barındırabileceğiniz belirlediği sonra plan **boyutu**(Küçük, Orta, büyük) ve **örnek sayısı** (1, 2, 3,...), **App Service planı**.

**Kotalar** için **ücretsiz** veya **paylaşılan** uygulamalardır:

* **CPU(short)**
  * Bu uygulamada 5 dakikalık bir aralık için izin verilen CPU miktarı. Bu kota, beş dakikada sıfırlar.
* **CPU(Day)**
  * Bu uygulama bir gün için izin verilen CPU toplam miktarı. Bu kota, 24 saatte gece yarısı UTC sıfırlar.
* **Bellek**
  * Bu uygulama için izin verilen bellek toplam miktarı.
* **Bant genişliği**
  * Bu uygulama bir gün için izin verilen giden bant genişliğinin toplam miktarı.
    Bu kota, 24 saatte gece yarısı UTC sıfırlar.
* **dosya sistemi**
  * İzin verilen depolama alanı miktarı.

Barındırılan uygulamalar için geçerli tek kota **temel**, **standart**, ve **Premium** planları olduğu **dosya sistemi**.

Belirli kotalar, sınırlar ve farklı uygulama hizmeti SKU'ları için kullanılabilen özellikler hakkında daha fazla bilgi burada bulunabilir: [Azure abonelik hizmeti limitleri](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Kota uygulama
Bir uygulama aşarsa **CPU (kısa)**, **CPU (gün)**, veya **bant genişliği** kota sıfırlar kadar kota uygulama durduruldu. Bu süre boyunca tüm gelen istekler sonucunda bir **HTTP 403**.
![][http403]

Uygulama **bellek** kota aşıldı, ardından uygulamayı yeniden başlatılır.

Varsa **dosya sistemi** kotası aşıldı ve herhangi bir içeren günlükleri için herhangi bir yazma işlemi başarısız oldu, yazma.

Kota artırabilir veya App Service planınızı yükselterek uygulamanızdan kaldırıldı.

### <a name="metrics"></a>Ölçümler
**Ölçümleri** uygulama veya App Service planının davranışı hakkında bilgi sağlar.

İçin bir **uygulama**, kullanılabilir ölçümler şunlardır:

* **Ortalama yanıt süresi**
  * Ms isteklere hizmet uygulaması için geçen ortalama süre.
* **Ortalama bellek çalışma kümesi**
  * Ortalama MIB uygulama tarafından kullanılan bellek miktarı.
* **CPU süresi**
  * CPU miktarını saniye olarak uygulama tarafından kullanılan. Bu ölçüm hakkında daha fazla bilgi için bkz: [CPU zamanı vs CPU yüzdesi](#cpu-time-vs-cpu-percentage)
* **Verileri**
  * MIB uygulama tarafından kullanılan gelen bant genişliği miktarı.
* **Veri çıkışı**
  * MIB uygulama tarafından tüketilen giden bant genişliği miktarı.
* **HTTP 2xx**
  * Bir HTTP durum kodunda sonuçlanan isteklerinin sayısı > = 200 ancak < 300.
* **HTTP 3xx**
  * Bir HTTP durum kodunda sonuçlanan isteklerinin sayısı > = 300 ancak < 400.
* **HTTP 401**
  * HTTP 401 durum kodunu kaynaklanan isteklerin sayısı.
* **HTTP 403**
  * HTTP 403 durum kodunda sonuçlanan isteklerinin sayısı.
* **HTTP 404**
  * HTTP 404 durum kodu kaynaklanan isteklerin sayısı.
* **HTTP 406**
  * HTTP 406 durum kodunda sonuçlanan isteklerinin sayısı.
* **HTTP 4xx**
  * Bir HTTP durum kodunda sonuçlanan isteklerinin sayısı > = < 500, ancak 400.
* **HTTP sunucu hataları**
  * Bir HTTP durum kodunda sonuçlanan isteklerinin sayısı > 500 ancak 600 < =.
* **Bellek çalışma kümesi**
  * Geçerli uygulama MIB tarafından kullanılan bellek miktarı.
* **İstekleri**
  * Elde edilen HTTP durum kodlarını bağımsız olarak isteklerinin toplam sayısı.

İçin bir **App Service planı**, kullanılabilir ölçümler şunlardır:

> [!NOTE]
> App Service planı ölçümleri planlarında kullanılabilir yalnızca **temel**, **standart**, ve **Premium** katmanları.
> 
> 

* **CPU yüzdesi**
  * Ortalama CPU planının tüm örneklerinde kullanılır.
* **Bellek yüzdesi**
  * Ortalama bellek planının tüm örneklerinde kullanılır.
* **Verileri**
  * Planın tüm örneklerde kullanılan ortalama gelen bant genişliği.
* **Veri çıkışı**
  * Giden bant genişliği planının tüm örneklerinde kullanılan ortalama.
* **Disk kuyruğu uzunluğu**
  * Ortalama sayısı hem okuma hem de depolama üzerinde sıraya alınan istek yazma. Yüksek disk sırası uzunluğu aşırı disk g/ç nedeniyle yavaşlamasıdır bir uygulamanın göstergesidir.
* **HTTP kuyruk uzunluğu**
  * Yerine önce kuyrukta oturmak olan HTTP isteklerinin ortalama sayısı. Yükselen bir HTTP kuyruk uzunluğu, ağır yük altında bir plan belirtisidir.

### <a name="cpu-time-vs-cpu-percentage"></a>CPU zamanı vs CPU yüzdesi
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

CPU kullanımı yansıtan iki ölçüm vardır. **CPU süresi** ve **CPU yüzdesi**

**CPU süresi** barındırılan uygulamalar için yararlıdır **ücretsiz** veya **paylaşılan** kotalarını birini uygulama tarafından kullanılan CPU dakikalar içinde tanımlı olduğundan planları.

**CPU yüzdesi** barındırılan uygulamalar için yararlıdır **temel**, **standart**, ve **premium** dışa Genişletilebilir olduğundan planları. CPU yüzdesi kullanımı genel bir göstergesidir tüm örneklerinde ' dir.

## <a name="metrics-granularity-and-retention-policy"></a>Ölçümleri ayrıntı düzeyi ve bekletme ilkesi
Bir uygulama ve app service planı ölçümleri günlüğe ve aşağıdaki ayrıntı düzeyi ve bekletme ilkeleri hizmet göre toplanır:

* **Dakika** ayrıntı düzeyi ölçümler için korunur **30 saat**
* **Saat** ayrıntı düzeyi ölçümler için korunur **30 gün**
* **Gün** ayrıntı düzeyi ölçümler için korunur **30 gün**

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Azure portalında, kotalar ve ölçümleri izleme.
Farklı durumunu gözden geçirebilirsiniz **kotalar** ve **ölçümleri** uygulamada etkileyen [Azure portalında](https://portal.azure.com).

![][quotas]
**Kotalar** ayarlar altında bulunabilir >**kotalar**. UX gözden geçirmenizi sağlar: (1) kotaları adı, (2), sıfırlama aralığı, (3), geçerli sınır ve (4) geçerli bir değer.

![][metrics]
**Ölçümleri** doğrudan kaynak sayfasından erişilebilir. Grafiği özelleştirebilirsiniz: (1) **tıklayın** ve seçin (2) **grafiği Düzenle**.
Buradan, (3) değiştirebilirsiniz **zaman aralığı**, (4) **grafik türü**ve (5) **ölçümleri** görüntülenecek.  

Buradaki ölçümler hakkında daha fazla bilgi edinebilirsiniz: [hizmet ölçümlerini izleme](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Uyarıları ve otomatik ölçeklendirme
Bir uygulama veya App Service planı ölçümleri için uyarılar bağlanabilir. Hakkında daha fazla bilgi edinmek için [uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-alerts-portal.md).

App Service uygulamaları temel, standart veya premium App Service planları destek barındırılan **otomatik ölçeklendirme**. Otomatik ölçeklendirme, App Service planı ölçümleri izleme kuralları yapılandırmanıza olanak sağlar. Kuralları artırabilir veya gerektiği gibi ek kaynaklar sağlayan örnek sayısını azaltabilirsiniz. Kurallar, uygulamanın aşırı sağlanan tam kapasiteye ulaşmadığında tasarruf da yardımcı olabilir. Otomatik ölçeklendirme hakkında daha fazla bilgi edinebilirsiniz: [ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md) ve burada [Azure İzleyici otomatik ölçeklendirme için en iyi yöntemler](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://azure.microsoft.com/try/app-service/) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
> 
> 

[fzilla]:https://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:https://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
