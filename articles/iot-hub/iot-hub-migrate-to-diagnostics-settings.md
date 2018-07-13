---
title: Azure IOT hub'ı geçirmek için tanılama ayarları | Microsoft Docs
description: Azure IOT Hub'ı, işlemleri gerçek zamanlı IOT hub'ınızdaki işlemlerin durumunu izlemek için izleme yerine Azure tanılama ayarlarını kullanmak için güncelleştirme yapma.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.author: kgremban
ms.openlocfilehash: 85bdb4b4802283c591e4d7a9e8b14ae74fa44e8d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38666731"
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>IOT Hub'ınız için tanılama ayarları izleme işlemleri geçiş

Kullanan müşteriler [işlem izleme] [ lnk-opsmon] IOT hub'ında işlemlerinin durumunu izlemek için iş akışının geçirebilirsiniz [Azure tanılama ayarları] [ lnk-diagnostics-settings], Azure İzleyici bir özelliğidir. Tanılama ayarları çoğu Azure hizmeti için kaynak düzeyinde tanılama bilgileri sağlayın.

İşlemleri izleme işlevselliğini IOT Hub'ın kullanım dışıdır ve gelecekte kaldırılacaktır. Bu makalede izleme için tanılama ayarları işlemlerden iş yüklerinizi taşımak için adımlar sağlar. Kullanımdan kaldırma zaman çizelgesini hakkında daha fazla bilgi için bkz: [Azure IOT çözümlerinizi Azure İzleyici ve Azure kaynak durumu izleme][lnk-blog-announcement].

## <a name="update-iot-hub"></a>IOT Hub'ı güncelleştirme

Azure portalındaki IOT Hub'ınıza güncelleştirmek için ilk tanılama ayarları etkinleştirin ve izleme işlemleri açın.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>İşlem izleme devre dışı Aç

Akışınıza yeni tanılama ayarları test ettikten sonra işlemleri izleme özelliğini kapatabilirsiniz. 

1. IOT hub'ı menüde **işlem izleme**.
1. Her izleme kategorisi altında seçin **hiçbiri**.
1. Değişiklik izleme işlemleri kaydedin.

## <a name="update-applications-that-use-operations-monitoring"></a>İşlem izleme kullanan uygulamaları güncelleştirme

İşlem izleme ve tanılama ayarları için şemalar biraz farklılık gösterir. Tanılama ayarları tarafından kullanılan şemayı eşlemek için işlem bugün izleme kullanan uygulamaları güncelleştirme önemlidir. 

Beş yeni kategori için izleme de, tanılama ayarları sunar. Uygulamalar için varolan şema güncelleştirdikten sonra yeni kategori de ekleyin:

- Bulut-cihaz ikizi işlemleri
- CİHAZDAN buluta ikizi işlemleri
- Çifti sorguları
- İş işlemleri
- Doğrudan yöntemler

Belirli bir şema yapıları için bkz: [tanılama ayarları için şemayı anlamak][lnk-diagnostics-schema].

## <a name="next-steps"></a>Sonraki adımlar

- [Azure IOT Hub durumunu izleyin ve sorunları hızla tanılayın][lnk-monitor]

[lnk-opsmon]: iot-hub-operations-monitoring.md
[lnk-diagnostics-settings]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[lnk-diagnostics-schema]: iot-hub-monitor-resource-health.md#understand-the-logs
[lnk-blog-announcement]: https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health/
[lnk-monitor]: iot-hub-monitor-resource-health.md
