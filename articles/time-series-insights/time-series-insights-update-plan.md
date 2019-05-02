---
title: Azure zaman serisi öngörüleri önizlemesi ortamınızı planlama | Microsoft Docs
description: Azure zaman serisi öngörüleri önizlemesi ortamınızı planlayın.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: 0472f53d11ec4c990fcf6face633444fe66ba937
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64702341"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Azure zaman serisi öngörüleri önizlemesi ortamınızı planlama

Bu makalede planlamak ve hızlı bir şekilde Azure zaman serisi öngörüleri önizlemesi ile çalışmaya başlamak için en iyi uygulamaları açıklar.

## <a name="best-practices-for-planning-and-preparation"></a>Planlama ve hazırlık için en iyi uygulamalar

Anlarsanız, zaman serisi görüşleri ile çalışmaya başlamak için en iyisidir:

* Bir zaman serisi öngörüleri Önizleme ortamı sağlama tıkladığınızda bunları edinirsiniz.
* Hangi, zaman serisi kimlikleri ve zaman damgası özelliklerdir.
* Yeni zaman serisi modeli nedir ve nasıl kendi uzantınızı oluşturun.
* Olayları JSON olarak verimli bir şekilde göndermek nasıl. 
* Zaman serisi görüşleri iş olağanüstü durum kurtarma seçenekleri.

Time Series Insights, Kullandıkça Öde iş modeli kullanır. Ücretleri ve kapasite hakkında daha fazla bilgi için bkz: [Time Series Insights fiyatlandırması](https://azure.microsoft.com/pricing/details/time-series-insights/).

## <a name="the-time-series-insights-preview-environment"></a>Zaman serisi öngörüleri Önizleme ortamı

Bir zaman serisi öngörüleri Önizleme ortamı sağladığınızda, iki Azure kaynaklarını oluşturun:

* Zaman serisi öngörüleri Önizleme ortamı
* Azure depolama genel amaçlı V1 hesabı

Başlamak için üç ek öğeler gerekir:
 
- A [zaman serisi modeli](./time-series-insights-update-tsm.md) 
- Bir [için zaman serisi görüşleri olay kaynağı bağlı](./time-series-insights-how-to-add-an-event-source-iothub.md) 
- [Olay kaynağına giden olayları](./time-series-insights-send-events.md) hem de modele eşlenir ve geçerli JSON biçiminde 

## <a name="configure-your-time-series-ids-and-timestamp-properties"></a>Zaman serisi kimlikleri ve zaman damgası özelliklerinizi yapılandırın

Yeni bir zaman serisi görüşleri ortamı oluşturmak için bir zaman serisi kimliği seçin Bunun yapılması, bu nedenle verileriniz için bir mantıksal bölüm olarak görev yapar. Belirtildiği gibi zaman serisi kimliklerinizi hazır olduğundan emin olun.

> [!IMPORTANT]
> Zaman serisi kimlikleri *değişmez* ve *daha sonra değiştirilemez*. Her biri son seçim önce doğrulayın ve ilk kullanın.

Kaynaklarınızı benzersiz olarak ayırt etmek için en çok üç (3) anahtarları seçebilirsiniz. Daha fazla bilgi için okuma [bir zaman serisi kimliği seçmek için en iyi yöntemler](./time-series-insights-update-how-to-id.md) ve [depolama ve giriş](./time-series-insights-update-storage-ingress.md).

Zaman damgası özelliği ayrıca büyük/küçük harf önemlidir. Olay kaynakları eklediğinizde, bu özelliği belirleyebilirsiniz. Her bir olay kaynağı, zaman içinde kullandığı izleme olay kaynakları için isteğe bağlı bir zaman damgası özelliği vardır. Zaman damgası değerleri büyük/küçük harfe duyarlıdır ve tek tek her bir olay kaynağı belirtimi için biçimlendirilmiş olması gerekir.

> [!TIP]
> Olay kaynakları, biçimlendirme ve ayrıştırma gereksinimlerini doğrulayın.

Boş bırakılırsa, olay kaynağı olay sıraya alma süresi, olayın zaman damgası kullanılır. Geçmiş verileri veya toplu olay gönderme, zaman damgası özelliği özelleştirme olay sıraya alma süresi varsayılandan daha yararlı olur. Hakkında daha fazla bilgi için okuma [IOT hub'ına olay kaynakları ekleme](./time-series-insights-how-to-add-an-event-source-iothub.md). 

## <a name="understand-the-time-series-model"></a>Zaman serisi anlama modeli

Time Series Insights ortamınızın zaman serisi modeli artık yapılandırabilirsiniz. Yeni model bulun ve IOT verilerini analiz etmek kolaylaştırır. Seçki, Bakım ve zaman serisi verilerini iyileştirmesini etkinleştirir ve kullanıma hazır veri kümeleri hazırlamak için yardımcı olur. Modeli kullanan **zaman serisi kimlikleri**, harita örneğine değişkenleri, türleri ve Hiyerarşiler bilinen benzersiz kaynak ilişkilendirir. Yeni hakkında okuyun [zaman serisi modeli](./time-series-insights-update-tsm.md).

Model dinamik olduğundan herhangi bir zamanda oluşturulabilir. Hızlıca kullanmaya başlamak için oluşturun ve Time Series Insights veri göndermeden önce yükleyin. Modelinizi oluşturmak için bkz: [zaman serisi modeli kullanan](./time-series-insights-update-how-to-tsm.md).

Birçok müşteri için bir varolan varlık modeli veya zaten yerinde ERP sistemi zaman serisi modeli eşlenir. Mevcut bir model yoksa, bir önceden oluşturulmuş kullanıcı deneyimidir [sağlanan](https://github.com/Microsoft/tsiclient) çalışmaya hızlıca almak için. Bir model size nasıl yardımcı yararlanmanız için görüntüleyin [örnek Tanıtım ortamı](https://insights.timeseries.azure.com/preview/demo). 

## <a name="shape-your-events"></a>Olaylarınızı Şekil

Olayları zaman serisi öngörüleri için gönderme bir şekilde doğrulayabilirsiniz. İdeal olarak, olaylarınızı da ve verimli bir şekilde normalleştirilmişlikten çıkarılır.

Bir iyi kural karşısında:

* Store meta verileri, zaman serisi modelindeki
* Zaman serisi modu, örnek alanları ve olaylar gibi yalnızca gerekli bilgileri içerir:
  * Zaman Serisi Kimliği
  * Zaman damgası

Daha fazla bilgi için [Şekil olayları](./time-series-insights-send-events.md#json).

## <a name="business-disaster-recovery"></a>İş olağanüstü durum kurtarma

Time Series Insights Azure bölgesi düzeyinde fazlalıkları kullanan bir yüksek kullanılabilirlik hizmetidir. Yapılandırma, devralınmış bu işlevleri kullanmak için gerekli değildir. Microsoft Azure platformu, olağanüstü durum kurtarma özellikleri veya bölgeler arası kullanılabilirlik ile çözümler oluşturmanıza yardımcı olacak özellikler de içerir. Bölgeler arası yüksek kullanılabilirlik için cihazlara veya kullanıcılara, genel sağlamak için bu Azure olağanüstü durum kurtarma özelliklerinden yararlanın. 

İş sürekliliği ve olağanüstü durum kurtarma (BCDR) için azure'da yerleşik özellikleri hakkında daha fazla bilgi için bkz: [Azure iş sürekliliği teknik rehberlik](https://docs.microsoft.com/azure/resiliency/resiliency-technical-guidance). Yüksek kullanılabilirlik ve olağanüstü durum kurtarma sağlamak Azure uygulamaları için stratejileri hakkında Mimari Kılavuzu için üzerinde incelemeye bakın. [olağanüstü durum kurtarma ve Azure uygulamaları için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/architecture/resiliency/index).

> [!NOTE]
> Time Series Insights yerleşik BCDR sahip değil. Varsayılan olarak, Azure depolama, Azure IOT Hub ve Azure Event Hubs, yerleşik kurtarma sahip.

Daha fazla bilgi edinin:

* [Azure depolama yedekliliği](https://docs.microsoft.com/azure/storage/common/storage-redundancy)
* [IOT hub'ı yüksek kullanılabilirlik, olağanüstü durum kurtarma](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr)
* [Olay hub'ı İlkesi](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr)

BCDR gerekiyorsa, bir kurtarma stratejisine yine de uygulayabilirsiniz. İkinci zaman serisi görüşleri ortamına bir yedekten Azure bölgesi oluşturun. Olayları birincil olay kaynağınızdan ikincil bu ortama gönderin. İkinci bir adanmış bir tüketici grubu ve bu olay kaynağının BCDR yönergeleri kullanın.

İkincil bir zaman serisi görüşleri ortamı oluşturma ve kullanma için bu adımları izleyin.

1. İkinci bir bölgede bir ortam oluşturun. Daha fazla bilgi için [zaman serisi görüşleri ortamları](./time-series-insights-get-started.md).
1. Olay kaynağınız için ikinci bir adanmış bir tüketici grubu oluşturun. Bu olay kaynak yeni ortama bağlanın. İkinci adanmış bir tüketici grubu tanımlamak emin olun. Daha fazla bilgi için bkz. [IOT Hub belgeleri](./time-series-insights-how-to-add-an-event-source-iothub.md) veya [olay hub'ı belgeleri](./time-series-insights-data-access.md).
1. Bir olağanüstü durum olayı sırasında birincil bölge etkileniyorsa, yedekleme zaman serisi görüşleri ortamına işlemleri yeniden yönlendir.

> [!IMPORTANT]
> * Not gecikme yük devretmesi durumunda karşılaşmış.
> * Yük devretme işlemleri yönlendirilir gibi iletisi işlenirken içeriklerinin anlık bir depo da neden olabilir.
> * Daha fazla bilgi için [zaman serisi görüşleri'nde gecikme süresini azaltmak](./time-series-insights-environment-mitigate-latency.md).

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [depolama ve giriş](./time-series-insights-update-storage-ingress.md) zaman serisi öngörüleri önizlemede.

- Hakkında bilgi edinin [veri modelleme](./time-series-insights-update-tsm.md) zaman serisi öngörüleri önizlemede.