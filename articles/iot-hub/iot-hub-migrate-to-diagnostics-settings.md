---
title: Azure IOT Hub geçirmek için tanılama ayarları | Microsoft Docs
description: Azure IOT Hub'ı gerçek zamanlı IOT hub'ınızı işlemlerinin durumunu izlemek için izleme işlemleri yerine Azure tanılama ayarları kullanacak şekilde güncelleştirme konusunda.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.author: kgremban
ms.openlocfilehash: 85bdb4b4802283c591e4d7a9e8b14ae74fa44e8d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34633596"
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>IOT Hub'ınız için tanılama ayarları izleme işlemleri geçiş

Kullanan müşteriler [izleme işlemleri] [ lnk-opsmon] IOT Hub'ındaki işlemlerinin durumunu izlemek için bu iş akışı geçirebilirsiniz [Azure tanılama ayarları] [ lnk-diagnostics-settings], Azure İzleyici özelliğidir. Tanılama ayarları çoğu Azure hizmeti için kaynak düzeyi tanılama bilgilerini sağlayın.

IOT hub'ı izleme işlevselliğini kullanım dışıdır ve gelecekte kaldırılacaktır işlemleri. Bu makalede izleme tanılama ayarlarını Operations iş yüklerinizi taşımak için adımlar sağlar. Kullanımdan kaldırma zaman çizelgesi hakkında daha fazla bilgi için bkz: [Azure İzleyici ve Azure kaynak durumu ile Azure IOT çözümlerinizi izlemek][lnk-blog-announcement].

## <a name="update-iot-hub"></a>IOT hub'ı güncelleştirme

Azure portalında IOT Hub'ınızı güncelleştirmek için önce tanılama ayarlarını etkinleştir ve izleme işlemleri devre dışı bırakma.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>İzleme işlemleri devre dışı bırakma

İş akışınızı üzerinde yeni tanılama ayarları test ettikten sonra işlem izleme özelliğini devre dışı kapatabilirsiniz. 

1. IOT hub'ı menünüze seçin **izleme işlemleri**.
1. Her izleme kategorisi altında seçin **hiçbiri**.
1. Değişiklikleri izleme işlemleri kaydedin.

## <a name="update-applications-that-use-operations-monitoring"></a>İzleme işlemleri kullanan uygulamaları güncelleştirme

İşlem izleme ve tanılama ayarları şemaları biraz farklılık gösterir. Tanılama ayarları tarafından kullanılan şema eşlemek için bugün izleme işlemleri kullanan uygulamalar güncelleştirme önemlidir. 

Beş yeni kategori için izleme Ayrıca, tanılama ayarlarını sağlar. Varolan şema uygulamaları güncelleştirdikten sonra yeni kategoriler de ekleyin:

- Bulut-cihaz çifti işlemleri
- Cihaz bulut çifti işlemleri
- Twin sorguları
- İş işlemleri
- Doğrudan yöntemleri

Belirli şema yapıları için bkz: [tanılama ayarları için şemayı anlamak][lnk-diagnostics-schema].

## <a name="next-steps"></a>Sonraki adımlar

- [Azure IOT Hub durumunu izlemenize ve sorunları hızla tanılamak][lnk-monitor]

[lnk-opsmon]: iot-hub-operations-monitoring.md
[lnk-diagnostics-settings]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[lnk-diagnostics-schema]: iot-hub-monitor-resource-health.md#understand-the-logs
[lnk-blog-announcement]: https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health/
[lnk-monitor]: iot-hub-monitor-resource-health.md
