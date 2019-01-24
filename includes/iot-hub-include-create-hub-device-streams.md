---
title: içerme dosyasını (cihaz akışları Önizleme)
description: içerme dosyasını (cihaz akışları Önizleme)
author: rezas
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 01/15/2019
ms.author: rezas
ms.custom: include file
ms.openlocfilehash: 53d8df7e2366cfbf2f62e79fc99c8ef2f9b48ba1
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54830247"
---
Bu bölümde bir IOT hub'ı kullanarak oluşturmayı açıklar [Azure portalında](https://portal.azure.com).

1. [Azure Portal](https://portal.azure.com)’da oturum açın. 

2. Seçin +**kaynak Oluştur**, ardından **nesnelerin interneti**.

3. Tıklayın **IOT hub'ı** sağdaki listeden. IOT hub'ı oluşturmak için ilk ekran görürsünüz.

   ![Azure portalında bir hub'ı oluşturma gösteren ekran görüntüsü](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-1.png)

   Şu alanları doldurun:

   **Abonelik**: IOT hub'ınız için kullanılacak aboneliği seçin.

   **Kaynak grubu**: Yeni bir kaynak grubu oluşturun veya var olanı kullanın. Yeni bir tane oluşturmak için tıklayın **Yeni Oluştur** ve kullanmak istediğiniz adı girin. Mevcut bir kaynak grubunu kullanmak için **var olanı kullan** ve açılır listeden kaynak grubunu seçin. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../articles/azure-resource-manager/resource-group-portal.md).

   **Bölge**: Bu hub'ınıza yer almasını istediğiniz bölgedir. (Örneğin, Orta ABD veya Orta ABD EUAP) desteklenen bir bölge seçtiğinizden emin olun.

   **IOT hub'ı adı**: IOT Hub'ınızın adını yerleştirin. Bu adın küresel olarak benzersiz olması gerekir. Girdiğiniz ad kullanılabilir durumdaysa yeşil bir onay işareti görünür.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

4. Tıklayın **sonraki: Boyut ve ölçek** IOT hub'ınıza oluşturmaya devam etmek için.

   ![Ayarı boyut ve ölçek için Azure portalını kullanarak yeni bir IOT hub'ı gösteren ekran görüntüsü](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-2-free.png)


   Bu ekranda varsayılan değerleri alabilir ve tıklamanız yeterli **gözden geçir + Oluştur** altındaki. 

   **Fiyatlandırma ve ölçek katmanı**: Standart (S1, S2, S3) veya (F1) ücretsiz katmanı seçtiğinizden emin olun. Bu seçenek, Filo boyutuna ve hub'ınıza (örneğin, telemetri iletilerini) beklediğiniz akış dışı iş yükleri tarafından da destekli. Örneğin, ücretsiz katmanı, sınama ve değerlendirme için tasarlanmıştır. Günlük ileti 8000'en fazla ve IOT hub'ına bağlanması 500 CİHAZDAN sağlar. Her Azure aboneliğinin bir IOT Hub ücretsiz katmanında oluşturabilirsiniz. 

   **IOT Hub birimlerinin**: Bu seçim olmayan akış iş yükü hub'ınıza beklediğiniz bağlıdır: şu an için 1'i seçebilirsiniz.

   Bir katman seçenekleri hakkında daha fazla ayrıntı için bkz: [doğru IOT Hub katmanını seçme](../articles/iot-hub/iot-hub-scaling.md).

5. Tıklayın **gözden + Oluştur** seçimlerinizi gözden geçirmek için. Bu ekranda gösterilene benzer bir şey görürsünüz.

   ![Yeni IOT hub'ı oluşturmak için bilgileri gözden geçirdikten ekran görüntüsü](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-3-free.png)

6. Tıklayın **Oluştur** yeni IOT hub'ınızı oluşturmak için. Hub'ı oluşturuluyor, birkaç dakika sürer.
