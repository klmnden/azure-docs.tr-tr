---
title: Azure IOT hub'ı geçirmek için tanılama ayarları | Microsoft Docs
description: Azure IOT Hub'ı, işlemleri gerçek zamanlı IOT hub'ınızdaki işlemlerin durumunu izlemek için izleme yerine Azure tanılama ayarlarını kullanmak için güncelleştirme yapma.
author: kgremban
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.author: kgremban
ms.openlocfilehash: 3cb0f91f3143e6a4828548f3a15678b3814cba17
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50154870"
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>IOT Hub'ınız için tanılama ayarları izleme işlemleri geçiş

Kullanan müşteriler [işlem izleme](iot-hub-operations-monitoring.md) IOT hub'ında işlemlerinin durumunu izlemek için iş akışının geçirebilirsiniz [Azure tanılama ayarları](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md), Azure İzleyici bir özelliğidir. Tanılama ayarları çoğu Azure hizmeti için kaynak düzeyinde tanılama bilgileri sağlayın.

İşlemleri izleme işlevselliğini IOT Hub'ın kullanım dışıdır ve gelecekte kaldırılacaktır. Bu makalede izleme için tanılama ayarları işlemlerden iş yüklerinizi taşımak için adımlar sağlar. Kullanımdan kaldırma zaman çizelgesini hakkında daha fazla bilgi için bkz: [Azure IOT çözümlerinizi Azure İzleyici ve Azure kaynak durumu izleme](https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health/).

## <a name="update-iot-hub"></a>IOT Hub'ı güncelleştirme

Azure portalındaki IOT Hub'ınıza güncelleştirmek için ilk tanılama ayarları etkinleştirin ve izleme işlemleri açın.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>İşlem izleme devre dışı Aç

Akışınıza yeni tanılama ayarları test ettikten sonra işlemleri izleme özelliğini kapatabilirsiniz. 

1. IOT hub'ı menüde **işlem izleme**.

2. Her izleme kategorisi altında seçin **hiçbiri**.

3. Değişiklik izleme işlemleri kaydedin.

## <a name="update-applications-that-use-operations-monitoring"></a>İşlem izleme kullanan uygulamaları güncelleştirme

İşlem izleme ve tanılama ayarları için şemalar biraz farklılık gösterir. Tanılama ayarları tarafından kullanılan şemayı eşlemek için işlem bugün izleme kullanan uygulamaları güncelleştirme önemlidir. 

Beş yeni kategori için izleme de, tanılama ayarları sunar. Uygulamalar için varolan şema güncelleştirdikten sonra yeni kategori de ekleyin:

* Bulut-cihaz ikizi işlemleri
* CİHAZDAN buluta ikizi işlemleri
* Çifti sorguları
* İş işlemleri
* Doğrudan yöntemler

Belirli bir şema yapıları için bkz: [tanılama ayarları için şemayı anlamak](iot-hub-monitor-resource-health.md#understand-the-logs).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure IoT Hub durumunu izleyin ve sorunları hızla tanılayın](iot-hub-monitor-resource-health.md)