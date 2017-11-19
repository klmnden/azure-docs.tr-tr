---
title: "Azure uygulama hizmetinde uygulamaları izleme | Microsoft Docs"
description: "Azure portalı kullanarak Azure uygulama hizmetinde uygulamaları izleme hakkında bilgi edinin."
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: d273da4e-07de-48e0-b99d-4020d84a425e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: byvinyal
ms.openlocfilehash: 283428c603cc73d23f0afa94670a23dbb45068d5
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="how-to-monitor-apps-in-azure-app-service"></a>Nasıl yapılır: Azure uygulama hizmetinde uygulamaları izleme
[Uygulama hizmeti](http://go.microsoft.com/fwlink/?LinkId=529714) yerleşik izleme işlevselliği sağlayan [Azure portal](https://portal.azure.com).
Bu gözden yeteneğini içerir **kotaları** ve **ölçümleri** ayarlama uygulama hizmeti planı yanı sıra, bir uygulama için **uyarıları** ve hatta **ölçeklendirme**bu ölçümleri göre otomatik olarak.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Anlama kotalar ve ölçümleri
### <a name="quotas"></a>Kotalar
App Service içinde barındırılan uygulamalar belirli tabi olan *sınırları* kullanabilecekleri kaynaklar. Sınırları tarafından tanımlanan **uygulama hizmeti planı** uygulamayla ilişkili.

Uygulama içinde barındırılıyorsa bir **serbest** veya **paylaşılan** planlama uygulama kullanabileceğiniz kaynaklar sınırları tarafından tanımlanan sonra **kotaları**.

Uygulama içinde barındırılıyorsa bir **temel**, **standart** veya **Premium** planlama sınırları kullanabilecekleri kaynaklar tarafından belirlenen sonra **boyutu**(Küçük, Orta, büyük) ve **örnek sayısını** (1, 2, 3,...), **uygulama hizmeti planı**.

**Kotalar** için **serbest** veya **paylaşılan** uygulamalar şunlardır:

* **CPU(short)**
  * Bu uygulamada 5 dakikalık bir zaman aralığı için izin verilen CPU miktarı. Bu kota her 5 dakikada bir sıfırlar.
* **CPU(Day)**
  * CPU bir gün içinde bu uygulama için izin verilen toplam miktarı. Bu kota 24 saatte UTC gece sıfırlar.
* **Bellek**
  * Bu uygulama için izin verilen bellek toplam miktarı.
* **Bant genişliği**
  * Giden bant genişliği bir gün içinde bu uygulama için izin verilen toplam miktarı.
    Bu kota 24 saatte UTC gece sıfırlar.
* **Dosya sistemi**
  * İzin verilen depolama alanı toplam miktarı.

Üzerinde barındırılan uygulamalar için geçerli tek kota **temel**, **standart** ve **Premium** planları olan **dosya sistemi**.

Belirli kotaları, sınırlar ve farklı uygulama hizmeti SKU'ları için kullanılabilen özellikleri hakkında daha fazla bilgi şurada bulunabilir: [Azure aboneliği hizmet sınırları](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Kota zorlama
Kullanım uygulamada aşarsa **CPU (kısa)**, **CPU (gün)**, veya **bant genişliği** kota sıfırlar kadar kota uygulama durduruldu. Bu süre boyunca, tüm gelen istekleri sonucunda bir **HTTP 403**.
![][http403]

Uygulama **bellek** kota aşıldı sonra uygulama yeniden.

Varsa **Filesystem** kota aşıldı sonra günlükleri tüm yazma işlemlerini içeren herhangi bir işlemi başarısız oldu, yazma.

Kotalar artırılabilir veya uygulama hizmeti planınızı yükselterek uygulamanızdan kaldırıldı.

### <a name="metrics"></a>Ölçümler
**Ölçümleri** uygulama veya App Service planının davranışı hakkında bilgi sağlar.

İçin bir **uygulama**, kullanılabilir ölçümler şunlardır:

* **Ortalama yanıt süresi**
  * Milisaniye olarak isteklere hizmet uygulaması için geçen ortalama süre.
* **Ortalama bellek çalışma kümesi**
  * Uygulama tarafından kullanılan MIB bellekte ortalama miktarı.
* **CPU süresi**
  * CPU miktarını uygulama tarafından kullanılan saniye cinsinden. Bu ölçüm hakkında daha fazla bilgi için bkz: [CPU zamanı vs CPU yüzdesi](#cpu-time-vs-cpu-percentage)
* **Verileri**
  * MIB uygulamada tarafından tüketilen gelen bant genişliği miktarı.
* **Giden veriler**
  * MIB uygulamada tarafından tüketilen giden bant genişliği miktarı.
* **HTTP 2xx**
  * Bir HTTP durum kodunu kaynaklanan isteklerin sayısı > = 200 ancak < 300.
* **HTTP 3xx**
  * Bir HTTP durum kodunu kaynaklanan isteklerin sayısı > = 300 ancak < 400.
* **HTTP 401**
  * HTTP 401 durum kodunu kaynaklanan isteklerin sayısı.
* **HTTP 403**
  * HTTP 403 durum kodunu kaynaklanan isteklerin sayısı.
* **HTTP 404**
  * HTTP 404 durum kodunu kaynaklanan isteklerin sayısı.
* **HTTP 406**
  * HTTP 406 durum kodunu kaynaklanan isteklerin sayısı.
* **HTTP 4xx**
  * Bir HTTP durum kodunu kaynaklanan isteklerin sayısı > = 400 ancak < 500.
* **HTTP sunucu hataları**
  * Bir HTTP durum kodunu kaynaklanan isteklerin sayısı > = 500 ancak < 600.
* **Bellek çalışma kümesi**
  * Geçerli MIB uygulama tarafından kullanılan bellek miktarı.
* **İstekleri**
  * Sonuçta elde edilen HTTP durum kodunu ne olursa olsun istekleri toplam sayısı.

İçin bir **uygulama hizmeti planı**, kullanılabilir ölçümler şunlardır:

> [!NOTE]
> Uygulama hizmeti planı ölçümleri planları için kullanılabilir yalnızca **temel**, **standart**, ve **Premium** katmanları.
> 
> 

* **CPU yüzdesi**
  * Planın tüm örneklerde kullanılan ortalama CPU.
* **Bellek yüzdesi**
  * Planın tüm örneklerde kullanılan ortalama bellek.
* **Verileri**
  * Planın tüm örneklerde kullanılan ortalama gelen bant genişliği.
* **Giden veriler**
  * Planın tüm örneklerde kullanılan bant genişliği giden ortalama.
* **Disk Sırası Uzunluğu**
  * Ortalama sayısı okuma ve yazma depolama üzerinde sıraya alınan istekleri. Yüksek disk sırası uzunluğu aşırı disk g/ç nedeniyle yavaşlamadan bir uygulamanın göstergesidir.
* **HTTP sırası uzunluğu**
  * Yerine getirilmesini önce sıraya sit gerekiyordu HTTP isteklerinin ortalama sayısı. Yüksek veya artan HTTP sırası uzunluğu, yoğun yük altında bir planı belirtisidir.

### <a name="cpu-time-vs-cpu-percentage"></a>CPU zamanı vs CPU yüzdesi
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

CPU kullanımı yansıtacak iki ölçüm vardır. **CPU süresi** ve **CPU yüzdesi**

**CPU süresi** barındırılan uygulamalar için yararlıdır **serbest** veya **paylaşılan** kendi kotaları birini uygulama tarafından kullanılan CPU dakika cinsinden tanımlanır beri planları.

**CPU yüzdesi** barındırılan uygulamalar için yararlıdır **temel**, **standart** ve **premium** dışa genişletilebilir ve bu ölçüm bir göstergesidir olduğundan planları tüm örneklerde genel kullanımı.

## <a name="metrics-granularity-and-retention-policy"></a>Ölçümleri ayrıntı düzeyi ve bekletme ilkesi
Bir uygulama ve uygulama hizmeti planı ölçümleri oturum ve aşağıdaki ayrıntı düzeyi ve bekletme ilkeleri hizmet tarafından toplanan:

* **Dakika** ayrıntı düzeyi ölçümleri için korunur **48 saat**
* **Saat** ayrıntı düzeyi ölçümleri için korunur **30 gün**
* **Gün** ayrıntı düzeyi ölçümleri için korunur **90 gün**

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Azure portalında, kotalar ve ölçümleri izleme.
Farklı durumunu gözden geçirebilirsiniz **kotaları** ve **ölçümleri** bir uygulamada etkileyen [Azure portal](https://portal.azure.com).

![][quotas]
**Kotalar** ayarlar altında bulunabilir >**kotaları**. UX gözden geçirmenizi sağlar: (1 kotaları adı, (2), sıfırlama aralığı, (3), geçerli sınırı ve (4) geçerli değeri.

![][metrics]
**Ölçümleri** doğrudan kaynak sayfasından erişim olabilir. Grafik tarafından da özelleştirebilirsiniz: (1) **tıklatın** ve seçin (2) üzerinde **grafiği Düzenle**.
Buradan, (3) değiştirebilirsiniz **zaman aralığı**, (4) **grafik türü**ve (5) **ölçümleri** görüntülemek için.  

Burada ölçümler hakkında daha fazla bilgi edinebilirsiniz: [izleme hizmeti ölçümleri](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Uyarılar ve otomatik ölçeklendirme
Bir uygulama veya uygulama hizmeti planı ölçümleri için uyarıları bağlanabilir. Hakkında daha fazla bilgi edinmek için [uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-alerts-portal.md).

App Service uygulamalarının temel, standart veya premium uygulama hizmeti planları destek barındırılan **otomatik ölçeklendirme**. Uygulama fazla sağlama ise, uygulama hizmeti planı ölçümleri izleyin ve artırabilir veya gerektiği gibi ek kaynaklar sağlayan örnek sayısı azaltabilirsiniz kuralları yapılandırın veya kaydetme para sağlar. Otomatik ölçek burada hakkında daha fazla bilgi edinebilirsiniz: [ölçek nasıl](../monitoring-and-diagnostics/insights-how-to-scale.md) ve burada [Azure İzleyici otomatik ölçeklendirmeyi yönelik en iyi uygulamalar](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://azure.microsoft.com/try/app-service/) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
> 
> 

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
