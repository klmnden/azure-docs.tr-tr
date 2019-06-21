---
title: include dosyası
description: include dosyası
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 09/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: c79b6f854dc78670a7eb8a1275c3e2fc46fcdd99
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188806"
---
### <a name="code-walkthrough"></a>Kod gözden geçirme

Bu bölümde bazı örnek kodun temel kısımları ve Uzaktan izleme çözüm hızlandırıcısının teklifiyle ne açıklar.

Aşağıdaki kod parçacığı, cihazın özelliklerini açıklayan bildirilen özellikleri nasıl tanımlandığını gösterir. Bu özellikler şunları içerir:

- Cihaz haritaya eklemek çözüm Hızlandırıcısını etkinleştirmek için cihaz konumu.
- Geçerli üretici yazılımı sürümü.
- Cihazın desteklediği yöntemlerin listesi.
- Cihaz tarafından gönderilen telemetri iletilerini şeması.

[!code-cpp[Define data structures for Chiller](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=datadefinition "Define data structures for Chiller")]

Örnek içeren bir **serializeToJson** Parson kitaplığını kullanarak bu veri yapısını serileştiren işlevi.

Örnek istemci çözüm Hızlandırıcı ile etkileşim kurdukça, bilgileri konsola yazdırma birkaç geri çağırma işlevleri içerir:

- **connection_status_callback**
- **send_confirm_callback**
- **reported_state_callback**
- **device_method_callback**

Aşağıdaki kod parçacığında gösterildiği **device_method_callback** işlevi. Bu işlev bir yöntem çağrısının çözüm hızlandırıcıdan aldığında gerçekleştirilecek eylemi belirler. İşlev bir başvuru alır **Soğutucu** veri yapısı içinde **userContextCallback** parametresi. Değerini **userContextCallback** geri çağırma işlevine olarak yapılandırıldığında ayarlanır **ana** işlevi:

[!code-cpp[Device method callback](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=devicemethodcallback "Device method callback")]

Çözüm Hızlandırıcısını üretici yazılımı güncelleştirme yöntemini çağırdığında, örnek JSON yükü seri durumdan çıkarır ve güncelleştirme işlemini tamamlamak için bir arka plan iş parçacığı başlatılır. Aşağıdaki kod parçacığında gösterildiği **do_firmware_update** iş parçacığında çalışır:

[!code-cpp[Firmware update thread](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=firmwareupdate "Firmware update thread")]

Aşağıdaki kod parçacığını nasıl istemci çözüm hızlandırıcısına bir telemetri iletisi gönderen gösterir. Çözüm Hızlandırıcısını telemetrisini Panoda görüntüleme amacıyla ileti şeması ileti özellikleri şunlardır:

[!code-cpp[Send telemetry](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=sendmessage "Send telemetry")]

**Ana** işlevi örneğinde:

- Başlatır ve SDK'sı alt kapatır.
- Başlatır **Soğutucu** veri yapısı.
- Çözüm hızlandırıcısına bildirilen özellikleri gönderir.
- Cihaz yöntemi geri çağırma işlevi yapılandırır.
- Çözüm Hızlandırıcısını telemetri değerleri gönderen benzetimi.

[!code-cpp[Main](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=main "Main")]
