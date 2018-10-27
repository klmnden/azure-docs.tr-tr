---
title: include dosyası
description: include dosyası
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 05/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 0039c4eff3571a96cba1ab36136e0a588d78eb75
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50159137"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Tanılama ayarları ile günlük kaydını etkinleştirme

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT Hub'ınıza gidin.

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

6. Yeni ayarları kaydedin. 

PowerShell ile tanılama ayarları etkinleştirmek istiyorsanız, aşağıdaki kodu kullanın:

```azurepowershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzureRmDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Yeni ayarları yaklaşık 10 dakika içinde etkinleşir. Sonra günlükleri yapılandırılmış arşivleme hedef üzerinde görünen **tanılama ayarları** dikey penceresi. Tanılama yapılandırma hakkında daha fazla bilgi için bkz. [toplamak ve azure kaynaklarınızdan günlük verilerini kullanma](../articles/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).