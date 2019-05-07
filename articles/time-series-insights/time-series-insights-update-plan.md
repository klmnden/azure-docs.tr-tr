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
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: b3fab86b2b2f0ad892e02cd089dbd7c45ce601d6
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205763"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Azure zaman serisi öngörüleri önizlemesi ortamınızı planlama

Bu makalede planlamak ve hızlı bir şekilde Azure zaman serisi öngörüleri önizlemesi ile çalışmaya başlamak için en iyi uygulamaları açıklar.

> [!NOTE]
> Bkz: [Azure zaman serisi öngörüleri GA ortamınızı planlama](time-series-insights-environment-planning.md), genel kullanılabilirlik TSI örneğini planlamak en iyi uygulamalar için.

## <a name="best-practices-for-planning-and-preparation"></a>Planlama ve hazırlık için en iyi uygulamalar

Anlarsanız, zaman serisi görüşleri ile çalışmaya başlamak için en iyisidir:

* Ne zaman elde, [bir zaman serisi öngörüleri Önizleme ortamı sağlama](#the-preview-environment).
* Nelerin sizin [zaman serisi kimlikleri ve zaman damgası özellikleri](#configure-time-series-ids-and-timestamp-properties).
* Hangi yeni [zaman serisi modeli](#understand-the-time-series-model)ve nasıl kendi uzantınızı oluşturun.
* Nasıl yapılır [olayları JSON'da verimli bir şekilde gönderme](#shape-your-events).
* Zaman serisi öngörüleri [iş olağanüstü durum kurtarma seçenekleri](#business-disaster-recovery).

Azure Time Series Insights, Kullandıkça Öde iş modeli kullanır. Ücretleri ve kapasite hakkında daha fazla bilgi için bkz: [Time Series Insights fiyatlandırması](https://azure.microsoft.com/pricing/details/time-series-insights/).

## <a name="the-preview-environment"></a>Önizleme ortamı

Bir zaman serisi öngörüleri Önizleme ortamı sağladığınızda, iki Azure kaynaklarını oluşturun:

* Azure zaman serisi öngörüleri Önizleme ortamı
* Bir Azure depolama genel amaçlı V1 hesabı

Başlamak için üç ek öğeler gerekir:

* A [zaman serisi modeli](./time-series-insights-update-tsm.md).
* Bir [için zaman serisi görüşleri olay kaynağı bağlı](./time-series-insights-how-to-add-an-event-source-iothub.md).
* [Olay kaynağına giden olayları](./time-series-insights-send-events.md) hem de modele eşlenir ve geçerli JSON biçimindedir.

## <a name="configure-time-series-ids-and-timestamp-properties"></a>Zaman serisi kimlikleri ve zaman damgası özelliklerini yapılandırın

Yeni bir zaman serisi görüşleri ortamı oluşturmak için Seç bir **zaman serisi kimliği**. Bunun yapılması, bu nedenle verileriniz için bir mantıksal bölüm olarak görev yapar. Belirtildiği gibi zaman serisi kimliklerinizi hazır olduğundan emin olun.

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
* Zaman serisi modu, örnek alanları ve olayları yalnızca gerekli bilgileri gibi dahil bir **zaman serisi kimliği** veya **zaman damgası**.

Daha fazla bilgi için [Şekil olayları](./time-series-insights-send-events.md#json).

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [depolama ve giriş](./time-series-insights-update-storage-ingress.md) zaman serisi öngörüleri önizlemede.

- Hakkında bilgi edinin [veri modelleme](./time-series-insights-update-tsm.md) zaman serisi öngörüleri önizlemede.