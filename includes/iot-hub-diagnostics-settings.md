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
ms.openlocfilehash: 1c7f006c066a4f1505a642af04a1ef027fde0a44
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34666951"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Tanılama ayarları ile günlük kaydını etkinleştir

1. Oturum [Azure portal] [ lnk-portal] ve IOT Hub'ına gidin.
1. Seçin **tanılama ayarları**.
1. Seçin **tanılamayı açın**.

   ![Tanılamayı açma][1]

1. Tanılama ayarları bir ad verin.
1. Seçmek istediğiniz günlükleri göndermek. Üç seçenekten herhangi bir birleşimini seçebilirsiniz:
   * Bir depolama hesabına arşivle
   * Bir olay hub'ına akış yap
   * Log Analytics’e gönderme
1. İzlemek istediğiniz işlemleri seçin ve bu işlemler için günlüklerini etkinleştirin. Tanılama ayarları hakkında rapor işlemleri şunlardır:
   * Bağlantılar
   * Cihaz telemetrisi
   * Bulut-cihaz iletilerini
   * Aygıt Kimliği işlemleri
   * Dosya yüklemeleri
   * İleti yönlendirme
   * Bulut-cihaz çifti işlemleri
   * Cihaz bulut çifti işlemleri
   * Twin işlemleri
   * İş işlemleri
   * Doğrudan yöntemler  
1. Yeni ayarları kaydedin. 

Tanılama ayarları PowerShell ile etkinleştirmek istiyorsanız, aşağıdaki kodu kullanın:

```azurepowershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzureRmDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Yeni ayarları yaklaşık 10 dakika içinde etkinleşir. Bundan sonra günlükler yapılandırılmış arşivleme hedef görünür **tanılama ayarları** dikey. Tanılama yapılandırma hakkında daha fazla bilgi için bkz: [toplamak ve azure kaynaklarınızdan günlük verilerini tüketen][lnk-diagnostics-settings].

<!-- Images -->
[1]: ./media/iot-hub-diagnostics-settings/turnondiagnostics.png

<!-- Links -->
[lnk-portal]: https://portal.azure.com
[lnk-diagnostics-settings]: ../articles/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
