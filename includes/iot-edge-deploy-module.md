---
title: include dosyası
description: include dosyası
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 01/04/2019
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 019d6120f8f3769d2d974270d575278da0370ef0
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54057433"
---
Azure IoT Edge'in önemli özelliklerinden biri buluttan IoT Edge cihazlarınıza modüller dağıtabilmektir. IoT Edge modülü, kapsayıcı olarak uygulanan yürütülebilir bir pakettir. Bu bölümde, biz önceden derlenmiş bir modülden dağıtma [Azure Marketi bölümünde IOT Edge modülleri](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules). Bu modül, IOT Edge cihazınız için sanal telemetri oluşturur.

1. İçinde [Azure portalında](https://portal.azure.com), girin **benzetimli sıcaklık algılayıcısı** arama ve açık Market sonucu.

   ![Azure portal Search'te benzetimli sıcaklık algılayıcısı](./media/iot-edge-deploy-module/search-for-temperature-sensor.png)

2. Bu modül almak için bir IOT Edge cihazı seçin. Üzerinde **hedef cihazlar IOT Edge modülü için** sayfasında, aşağıdaki bilgileri sağlayın:

   1. **Abonelik**: kullanmakta olduğunuz IOT hub'ı içeren aboneliği seçin.

   2. **IOT hub'ı**: kullanmakta olduğunuz IOT hub'ı adını seçin.

   3. **IOT Edge cihaz adı**: Bu hızlı başlangıçta daha önce önerilen cihaz adını kullandıysanız, girin **myEdgeDevice**. Ya da seçin **cihaz bulma** IOT Edge cihazlarının IOT hub'ınızda bir listesinden seçmek için. 
   
   4. **Oluştur**’u seçin.

3. Azure Marketi'nden bir IOT Edge modülü seçtiniz ve modül almak için bir IOT Edge cihazı seçmiş göre tam olarak nasıl modülü dağıtılacak tanımlamanıza yardımcı olan üç adımlık Sihirbazı yönlendirilirsiniz. İçinde **Ekle modülleri** adım Sihirbazı'nın dikkat **SimulatedTemperatureSensor** girdiğinizde modülüdür. Öğreticilerde dağıtımınıza ek modül eklemek için bu sayfayı kullanacaksınız. Bu hızlı başlangıçta, yalnızca bu bir modül dağıtın. Seçin **sonraki** sihirbazın sonraki adıma devam etmek için.

4. İçinde **yolları belirtin** adım Sihirbazı'nın, IOT hub'ına modüller arasında iletileri nasıl geçirildiğini tanımlayın. Hızlı Başlangıç için istediğiniz tüm modüllerdeki tüm iletileri IOT Hub'ına gidin (`$upstream`). Paneldeki değilse, aşağıdaki kodu ekleyin, ardından seçin **sonraki**:

   ```json
    {
    "routes": {
        "route": "FROM /messages/* INTO $upstream"
        }
    }
   ```

5. İçinde **gözden geçirme dağıtım** adım Sihirbazı'nın IOT Edge cihazınıza dağıtıldığı tüm modülleri tanımlayan JSON dosyasını önizlemesini görebilirsiniz. Dikkat **SimulatedTemperatureSensor** modülü dahil de adlı iki ek sistem modül olarak **edgeAgent** ve **edgeHub**. Seçin **Gönder** bitirdiğinizde gözden geçirme.

   IOT Edge cihazı için yeni bir dağıtım gönderdiğinizde, hiçbir şey cihazınıza gönderilir. Bunun yerine, cihaz IOT hub'ı yeni yönergeleri için düzenli olarak sorgular. Yeni dağıtım bilgilerini görür, aygıt, buluttan modül görüntüleri çekmek için kullanır ve modüllerini yerel olarak çalışmaya başlar. Bu işlem birkaç dakika sürebilir. 

6. Modül dağıtım ayrıntıları gönderdikten sonra sihirbazın döndürür **IOT Edge** IOT hub'ınızın sayfası. Ayrıntılarını görmek için IOT Edge cihaz listesinden Cihazınızı seçin. 

7. Cihaz Ayrıntıları sayfasında aşağı kaydırarak **modülleri** bölümü. Üç modül listelenir: $edgeAgent $edgeHub ve SimulatedTemperatureSensor. Bir veya daha fazla modülleri dağıtımında belirtilen listede ancak IOT Edge Cihazınızı yine de bunları başlattığını anlamına gelir cihaz tarafından bildirilen değil. Birkaç dakika bekleyin ve seçin **Yenile** sayfanın üstünde. 

   ![Dağıtılan modüller listesinde SimulatedTemperatureSensor görüntüle](./media/iot-edge-deploy-module/deployed-modules-marketplace.png)
