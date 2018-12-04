---
title: Azure Time Series Insights (Önizleme) ortamınızı planlama | Microsoft Docs
description: Azure Time Series Insights (Önizleme) ortamınızı planlama
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: 438d71997d2c92e377cd068615d274af6b8b5edb
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856951"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Azure Time Series Insights (Önizleme) ortamınızı planlama

Bu makalede planlamak ve hızlı bir şekilde Azure Time Series Insights (Önizleme) kullanmaya başlamak için en iyi uygulamaları açıklar.

## <a name="best-practices-for-planning-and-preparation"></a>Planlama ve hazırlık için en iyi uygulamalar

Hazır başlamadan önce aşağıdaki öğelere sahip en iyisidir:

* Tespit ettik, **zaman serisi kimlikleri**
* Sahip olduğunuz, **zaman damgası** özelliği hazır
* Derlediğiniz, **zaman serisi modeli**
* JSON'da verimli bir şekilde normalleştirilmişlikten çıkarılmış olayları gönderme işlemini anlama

Planlama ve hazırlık basitleştirmek için bu öğeleri hazır yardımcı olması. Ayrıca, önceden planlayın ve iş olağanüstü durum kurtarma (BCDR) gerektiğini örneğinizi oluşturmadan önce akıllıca olur (ve daha sonra değil). Bunun yapılması önceden örneğinizin maximally hazırlanmış sağlamaya yardımcı olur.

> [!TIP]
> Ortamınızı önce ve sonra örneğinizin oluşturmamayı BCDR gereksinimlerinize uyacak şekilde yapılandırın.

(Önizleme) Azure TSI, Kullandıkça Öde iş modeli kullanır. Ücretleri ve kapasite hakkında daha fazla bilgi için bkz: [Time Series Insights fiyatlandırması](https://azure.microsoft.com/pricing/details/time-series-insights).

## <a name="configure-your-time-series-ids-and-timestamp-properties"></a>Zaman serisi kimlikleri ve zaman damgası özelliklerinizi yapılandırın

Yeni bir TSI ortamı oluşturmak için Seç bir **zaman serisi kimliği**. Bunun yapılması, bu nedenle verileriniz için bir mantıksal bölüm olarak görev yapar. Belirtildiği gibi bulunduğundan emin olun, **zaman serisi kimlikleri** hazır.

> [!IMPORTANT]
> **Zaman serisi kimlikleri** olan **değişmez** ve **daha sonra değiştirilemez**. Her son seçim önce doğrulayın ve ilk kullanın.

En fazla seçebileceğiniz **üç** kaynaklarınızı benzersiz olarak ayırt etmek için (3) anahtarları. Okuma [bir zaman serisi kimliği seçmek için en iyi yöntemler](./time-series-insights-update-how-to-id.md) makale daha fazla bilgi için.

İsteğe bağlı her bir olay kaynağı olan **zaman damgası** zamanla izleme olay kaynakları için kullanılan özellik. **Zaman damgası** değerler büyük küçük harfe duyarlıdır ve tek tek her bir olay kaynağı belirtimi için biçimlendirilmiş olması gerekir.

> [!TIP]
> Olay kaynakları, biçimlendirme ve ayrıştırma gereksinimlerini doğrulayın.

Boş bırakıldığına **olay sıraya alma süresi** olay kaynağı olay kullanılır **zaman damgası**. Geçmiş verileri veya toplu olayları gönderiyorsanız, büyük olasılıkla özelleştirme bulursunuz **zaman damgası** varsayılandan daha yararlı olacak şekilde özelliği **olay sıraya alma süresi**. Hakkında daha fazla bilgi için okuma [IOT hub'ına olay kaynakları ekleme](./time-series-insights-how-to-add-an-event-source-iothub.md).  

## <a name="understand-the-time-series-model"></a>Zaman serisi anlama modeli

TSI ortamınızın artık yapılandırabilirsiniz **zaman serisi modeli**. Yeni model bulun ve IOT verilerini analiz etmek kolaylaştırır. Seçki, Bakım ve zaman serisi verilerini iyileştirmesini sağlayarak bunu yapar ve kullanıma hazır veri kümeleri hazırlamak için yardımcı olur. Modeli kullanan **zaman serisi kimlikleri**, benzersiz kaynak hiyerarşileri ve değişkenleri (türleri olarak da bilinir) ile ilişkilendirme örneğine eşleyin. Yeni hakkında okuyun [zaman serisi modeli](./time-series-insights-update-tsm.md).

Oluşturulan için herhangi bir zamanda dinamik bir modeldir. Ancak, oluşturulan ve size TSI veri göndermeye başlamadan önce karşıya daha hızlı bir şekilde kullanmaya başlamak mümkün olacaktır. Modelinizi oluşturmak için gözden [zaman serisi modeli](./time-series-insights-update-tsm.md) makalesi.

Birçok müşteri için bekliyoruz **zaman serisi modeli** bir mevcut varlık modeli veya zaten yerinde ERP sistemi eşlemek için. Mevcut bir model sahip olmayan müşteriler için önceden oluşturulmuş kullanıcı deneyimidir [sağlanan](https://github.com/Microsoft/tsiclient) çalışmaya hızlıca almak için.

## <a name="shaping-your-events"></a>Olaylarınızı şekillendirme

TSI için olayları gönderirsiniz şekilde doğrulamak önemlidir. İdeal olarak, olaylarınızı da ve verimli bir şekilde çıkarılmışsa.

Bir iyi kural karşısında:

* Meta veri olarak depolanması, **zaman serisi modeli**
* **Zaman serisi modu;**  örneği alanlar ve olaylar gereken yalnızca gerekli bilgileri gibi:
  * **Zaman serisi kimliği**
  * **Zaman damgası**

Gözden geçirme [Şekil olayları nasıl](./time-series-insights-update-how-to-shape-events.md) makale daha fazla ayrıntı için.

## <a name="business-disaster-recovery"></a>İş olağanüstü durum kurtarma

Bir Azure hizmeti TSI Azure bölgesi düzeyinde fazlalıkları kullanarak bir yüksek kullanılabilirlik (HA) hizmetidir. Yapılandırma, devralınmış bu işlevleri kullanmak için gereklidir. Microsoft Azure platformu, olağanüstü durum kurtarma (DR) özellikleri veya bölgeler arası kullanılabilirlik ile çözümler oluşturmanıza yardımcı olacak özellikler de içerir. Genel sağlamak istiyorsanız, bölgeler arası HA cihazları veya kullanıcıları için bu Azure DR özelliklerden yararlanın. Makaleyi [Azure iş sürekliliği teknik rehberlik](https://docs.microsoft.com/azure/resiliency/resiliency-technical-guidance) iş sürekliliği ve olağanüstü durum kurtarma için azure'da yerleşik özellikler açıklanmaktadır. [Olağanüstü durum](https://docs.microsoft.com/azure/architecture/resiliency/index) kurtarma ve kağıt Azure uygulamaları için yüksek kullanılabilirlik stratejileri HA ve DR elde etmek, Azure uygulamaları için hakkında yönelik mimari yönergeleri sağlar.

> [!NOTE]
> Azure Time Series Insights yerleşik BCDR yok.
> Varsayılan olarak, Azure depolama, Azure IOT Hub ve Event Hubs yerleşik kurtarma sahiptir.

Daha fazla bilgi için:

* Hakkında bilgi edinin [Azure Depolama'nın yedeklilik](https://docs.microsoft.com/azure/storage/common/storage-redundancy).
* Hakkında bilgi edinin [IOT Hub'ın HA DR](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr).
* Hakkında bilgi edinin [olay Hub'ın ilkeleri](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr).

Bununla birlikte, BCDR gerektiren müşteriler yedekleme Azure bölgesi içinde ikinci bir TSI ortamı oluşturarak bir kurtarma stratejisi uygulayabilir. Müşteriler olayları ikincil bu ortama birincil olay kaynağından ikinci bir adanmış bir tüketici grubu ve bu olay kaynağının BCDR yönergeleri gönderin.

Bunu gerçekleştirmek için belirli adımlar aşağıdaki gibidir:

1. İkinci bir bölgede bir ortam oluşturun. Hakkında bilgi edinin [TSI ortamları](./time-series-insights-get-started.md).
1. Olay kaynağınız için ikinci bir adanmış bir tüketici grubu oluşturun ve bu olay kaynak yeni ortama bağlanın. İkinci ve adanmış bir tüketici grubu tanımlamak emin olun. Daha fazla bilgi [IOT Hub belgeleri](./time-series-insights-how-to-add-an-event-source-iothub.md) veya [olay hub'ı belgeleri](./time-series-insights-data-access.md).
1. Bir olağanüstü durum olayı sırasında birincil bölge etkilendiğini, yedekleme TSI ortam işlemleri yeniden yönlendir.

> [!IMPORTANT]
> * Bir yük devretme durumunda bir gecikme yaşadı olduğunu unutmayın.
> * Yük devretme işlemleri yönlendirilir gibi iletisi işlenirken bir mometary ani da neden olabilir.
> * Daha fazla bilgi için gözden [TSI içindeki gecikme süresi azaltma](./time-series-insights-environment-mitigate-latency.md).

## <a name="next-steps"></a>Sonraki adımlar

Okuma [Azure TSI (Önizleme) depolama ve giriş](./time-series-insights-update-storage-ingress.md).

Yeni hakkında okuyun [zaman serisi modeli](./time-series-insights-update-tsm.md).