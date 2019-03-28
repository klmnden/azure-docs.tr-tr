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
ms.openlocfilehash: dd5dc53311c8611a4ca4d174401bba797fe5c4b1
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58505916"
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
   * Bir olay hub'ına akış yap
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
