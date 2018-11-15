---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 11/02/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: b1f1f8144ec5b65c21c5ddcecd278a018260047c
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2018
ms.locfileid: "51516022"
---
Bu bölümde bir IOT hub'ı kullanarak oluşturmayı açıklar [Azure portalında](https://portal.azure.com).

1. [Azure Portal](https://portal.azure.com)’da oturum açın. 

2. Seçin +**kaynak Oluştur**, ardından **nesnelerin interneti**.

3. Tıklayın **IOT hub'ı** sağdaki listeden. IOT hub'ı oluşturmak için ilk ekran görürsünüz.

   ![Azure portalında bir hub'ı oluşturma gösteren ekran görüntüsü](./media/iot-hub-include-create-hub/iot-hub-create-screen-basics.png)

   Alanları doldurun.

   **Abonelik**: IOT hub'ınız için kullanılacak aboneliği seçin.

   **Kaynak grubu**: yeni bir kaynak grubu oluşturun veya var olanı kullanın. Yeni bir tane oluşturmak için tıklayın **Yeni Oluştur** ve kullanmak istediğiniz adı girin. Mevcut bir kaynak grubunu kullanmak için **var olanı kullan** ve açılır listeden kaynak grubunu seçin. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../articles/azure-resource-manager/resource-group-portal.md).

   **Bölge**: hub'ınıza yer almasını istediğiniz bölgeyi budur. Açılır listeden size en yakın konumu seçin.

   **IOT hub'ı adı**: IOT Hub'ınızın adını yerleştirin. Bu adın küresel olarak benzersiz olması gerekir. Girdiğiniz ad kullanılabilir durumdaysa yeşil bir onay işareti görünür.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

4. Tıklayın **sonraki: boyut ve ölçek** IOT hub'ınıza oluşturmaya devam etmek için.

   ![Ayarı boyut ve ölçek için Azure portalını kullanarak yeni bir IOT hub'ı gösteren ekran görüntüsü](./media/iot-hub-include-create-hub/iot-hub-create-screen-size-scale.png)

   Bu ekranda varsayılan değerleri alabilir ve tıklamanız yeterli **gözden geçir + Oluştur** altındaki. 

   **Fiyatlandırma ve ölçek katmanı**: bağlı olarak kaç özellikleri birkaç katmanı arasından seçim yapabilirsiniz istediğiniz ve kaç iletileri gönder günde çözümünüz aracılığıyla. Ücretsiz katman, test ve değerlendirme için tasarlanmıştır. Günlük ileti 8000'en fazla ve IOT hub'ına bağlanması 500 CİHAZDAN sağlar. Her Azure aboneliğinin bir IOT Hub ücretsiz katmanında oluşturabilirsiniz. 

   **IOT Hub birimlerinin**: günlük birim başına izin verilen ileti sayısı, hub'ın fiyatlandırma katmanına bağlıdır. Örneğin, IOT hub'ı 700.000 iletilerinin giriş desteklemek için isterseniz, iki adet S1 katmanı birimi seçin.

   Bir katman seçenekleri hakkında daha fazla ayrıntı için bkz: [doğru IOT Hub katmanını seçme](../articles/iot-hub/iot-hub-scaling.md).

   **Gelişmiş / CİHAZDAN buluta bölümler**: Bu özellik CİHAZDAN buluta iletileri iletilerin eşzamanlı okuyucu sayısıyla ilgilidir. Çoğu IOT hub'ları, yalnızca dört bölüm gerekir. 

5. Tıklayın **gözden + Oluştur** seçimlerinizi gözden geçirmek için. Bu ekranda gösterilene benzer bir şey görürsünüz.

   ![Yeni IOT hub'ı oluşturmak için bilgileri gözden geçirdikten ekran görüntüsü](./media/iot-hub-include-create-hub/iot-hub-create-review.png)

6. Tıklayın **Oluştur** yeni IOT hub'ınızı oluşturmak için. Hub'ı oluşturuluyor, birkaç dakika sürer.
