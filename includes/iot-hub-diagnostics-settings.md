---
title: include dosyası
description: include dosyası
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 02/20/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 3893b79cee96c3928897f64f3601ebe4c490ebdd
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188479"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Tanılama ayarları ile günlük kaydını etkinleştirme

[!INCLUDE [updated-for-az](./updated-for-az.md)]

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT hub'ınıza gidin.

2. Seçin **tanılama ayarları**.

3. Seçin **tanılamayı Aç**.

   ![Tanılamayı açma](./media/iot-hub-diagnostics-settings/turnondiagnostics.png)

4. Tanılama ayarlarını, bir ad verin.

5. Seçmek istediğiniz günlükleri göndermek. Üç seçenekten herhangi bir birleşimini seçebilirsiniz:

   * Bir depolama hesabına arşivle
   * Olay hub'ına Stream
   * Log Analytics’e gönderme

6. İzlemek istediğiniz işlemleri seçin ve bu işlemler için günlüklerini etkinleştirin. Tanılama Ayarları'nın Raporlama yapabileceği işlemleri şunlardır:

   * Bağlantılar
   * Cihaz telemetrisi
   * Bulut-cihaz iletilerini
   * Cihaz kimlik işlemleri
   * Dosya yüklemeleri
   * İleti yönlendirme
   * Bulut-cihaz ikizi işlemleri
   * CİHAZDAN buluta ikizi işlemleri
   * İkiz işlemleri
   * İş işlemleri
   * Doğrudan yöntemler  
   * Dağıtılmış izleme (Önizleme)
   * Yapılandırmalar
   * Cihaz akışları
   * Cihaz ölçümleri

6. Yeni ayarları kaydedin. 

PowerShell ile tanılama ayarları etkinleştirmek istiyorsanız, aşağıdaki kodu kullanın:

```azurepowershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Yeni ayarları yaklaşık 10 dakika içinde etkinleşir. Sonra günlükleri yapılandırılmış arşivleme hedef üzerinde görünen **tanılama ayarları** dikey penceresi. Tanılama yapılandırma hakkında daha fazla bilgi için bkz. [toplamak ve azure kaynaklarınızdan günlük verilerini kullanma](../articles/azure-monitor/platform/diagnostic-logs-overview.md).
