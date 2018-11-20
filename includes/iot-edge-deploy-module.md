---
title: include dosyası
description: include dosyası
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 10/14/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 536857a5fe3ec3cc037f21835a4152f93197bcb8
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2018
ms.locfileid: "51977412"
---
Azure IoT Edge'in önemli özelliklerinden biri buluttan IoT Edge cihazlarınıza modüller dağıtabilmektir. IoT Edge modülü, kapsayıcı olarak uygulanan yürütülebilir bir pakettir. Bu bölümde, simülasyon cihazınız için telemetri oluşturacak bir modülün dağıtımı yapacaksınız.

1. Azure portalında IoT Hub'ınıza gidin.

2. **Otomatik Cihaz Yönetimi** altında **IoT Edge**’e gidin ve IoT Edge cihazınızı seçin.

3. **Modülleri Ayarlama**'yı seçin. Modüller ekleme, yollar belirleme ve dağıtım gözden geçirme sırasında size kılavuzluk eder Portalı'nda bir üç adımlık Sihirbazı açılır. 

4. İçinde **Ekle modülleri** adım bulma Sihirbazı'nın **dağıtım modülleri** bölümü. Tıklayın **Ekle** seçip **IOT Edge Modülü**.

   ![Yeni bir IOT Edge Modülü Ekle](./media/iot-edge-deploy-module/add-module.png)

5. **Ad** alanına `tempSensor` girin.

6. **Görüntü URI'si** alanına `mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0` girin.

7. Diğer ayarları değiştirmeden bırakın ve **Kaydet**'i seçin.

   ![Adı ve görüntü URI'sini girdikten sonra IoT Edge modülünü kaydedin](./media/iot-edge-deploy-module/name-image.png)

8. Geri ilk adımda Sihirbazı seçin **sonraki**.

9. İçinde **yolları belirtin** adım Sihirbazı'nın tüm iletileri tüm modüllerden IOT Hub'ına gönderir ve varsayılan bir yol olmalıdır. Yoksa aşağıdaki kodu ekleyip **İleri**'yi seçin.

   ```json
   {
       "routes": {
           "route": "FROM /messages/* INTO $upstream"
       }
   }
   ```

10. İçinde **gözden geçirme dağıtım** seçin, adım **Gönder**.

11. Cihaz ayrıntıları sayfasına dönüp **Yenile**’yi seçin. Hizmeti ilk kez başlattığınızda oluşturulan edgeAgent modülüne ek olarak **edgeHub** ve **tempSensor** adlı başka bir çalışma zamanını modülünü görmeniz gerekir.

   Bu, gösterilecek yeni modüller için birkaç dakika sürebilir. IOT Edge cihazı buluttan yeni dağıtım bilgilerini almak için kapsayıcıları başlatın ve ardından yeni durumuna IOT Hub'ına rapor gerekir. 

   ![Dağıtılan modüller listesinde tempSensor modülünü görüntüleme](./media/iot-edge-deploy-module/deployed-modules.png)
