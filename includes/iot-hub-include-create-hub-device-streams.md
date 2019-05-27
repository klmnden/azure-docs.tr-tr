---
title: içerme dosyasını (cihaz akışları Önizleme)
description: içerme dosyasını (cihaz akışları Önizleme)
author: rezas
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/14/2019
ms.author: rezas
ms.custom: include file
ms.openlocfilehash: ede897054a6cbef254c06bd1d810b933ec09016a
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66158651"
---
Bu bölümde bir IOT hub'ı kullanarak oluşturmayı açıklar [Azure portalında](https://portal.azure.com).

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Seçin +**kaynak Oluştur**, ardından **nesnelerin interneti**.

3. Tıklayın **IOT hub'ı** sağdaki listeden. IOT hub'ı oluşturmak için ilk ekran görürsünüz.

   ![Azure portalında bir hub'ı oluşturma gösteren ekran görüntüsü](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-01.png)

   Şu alanları doldurun:

   **Abonelik**: IOT hub'ınız için kullanılacak aboneliği seçin.

   **Kaynak grubu**: Yeni bir kaynak grubu oluşturun veya var olanı kullanın. Yeni bir tane oluşturmak için tıklayın **Yeni Oluştur** ve kullanmak istediğiniz adı girin. Mevcut bir kaynak grubunu kullanmak için **var olanı kullan** ve açılır listeden kaynak grubunu seçin. Daha fazla bilgi için [yönetme Azure Resource Manager kaynak grupları](../articles/azure-resource-manager/manage-resource-groups-portal.md).

   **Bölge**: Bu hub'ınıza yer almasını istediğiniz bölgedir. IOT Hub cihaz akışları Önizleme, Orta ABD veya Orta ABD EUAP destekleyen bir bölge seçin.

   **IOT hub'ı adı**: IOT Hub'ınızın adını yerleştirin. Bu adın küresel olarak benzersiz olması gerekir. Girdiğiniz ad kullanılabilir durumdaysa yeşil bir onay işareti görünür.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

4. Tıklayın **sonraki: Boyut ve ölçek** IOT hub'ınıza oluşturmaya devam etmek için.

   ![Ayarı boyut ve ölçek için Azure portalını kullanarak yeni bir IOT hub'ı gösteren ekran görüntüsü](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-02.png)

   Bu ekranda varsayılan değerleri alabilir ve tıklamanız yeterli **gözden geçir + Oluştur** altındaki.

   **Fiyatlandırma ve ölçek katmanı**: Standart (S1, S2, S3) veya (F1) ücretsiz katmanı seçtiğinizden emin olun. Bu seçenek, Filo boyutuna ve hub'ınıza (örneğin, telemetri iletilerini) beklediğiniz akış dışı iş yükleri tarafından da destekli. Örneğin, ücretsiz katmanı, sınama ve değerlendirme için tasarlanmıştır. Günlük ileti 8000'en fazla ve IOT hub'ına bağlanması 500 CİHAZDAN sağlar. Her Azure aboneliğinin bir IOT Hub ücretsiz katmanında oluşturabilirsiniz. 

   **IOT Hub birimlerinin**: Bu seçim olmayan akış iş yükü hub'ınıza beklediğiniz bağlıdır: şu an için 1'i seçebilirsiniz.

   Bir katman seçenekleri hakkında daha fazla ayrıntı için bkz: [doğru IOT Hub katmanını seçme](../articles/iot-hub/iot-hub-scaling.md).

5. Tıklayın **gözden + Oluştur** seçimlerinizi gözden geçirmek için. Bu ekranda gösterilene benzer bir şey görürsünüz.

   ![Yeni IOT hub'ı oluşturmak için bilgileri gözden geçirdikten ekran görüntüsü](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-03.png)

6. Tıklayın **Oluştur** yeni IOT hub'ınızı oluşturmak için. Hub'ı oluşturuluyor, birkaç dakika sürer.
