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
ms.openlocfilehash: c20a14ef2bc74d73b93ab39ee52fe1be8a5f984f
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188489"
---
Azure IOT Edge özellikleri birini kod IOT Edge cihazlarınıza buluttan dağıtma çağrılabilmesidir. **IOT Edge modülleri** kapsayıcılar uygulanan yürütülebilir paketler. Bu bölümde, önceden oluşturulmuş bir modülden dağıttığınız [Azure Marketi bölümünde IOT Edge modülleri](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules). 

Bu bölümde dağıttığınız modül algılayıcı benzetimini yapar ve oluşturulan verileri gönderir. Geliştirme ve test için benzetilmiş data özelliğini kullanabildiğinden, IOT Edge ile çalışmaya başlarken bu modül kod yararlı bir parçasıdır. Bu modül yaptığı tam olarak görmek istiyorsanız, görüntüleyebileceğiniz [benzetimli sıcaklık algılayıcısı kaynak kodu](https://github.com/Azure/iotedge/blob/027a509549a248647ed41ca7fe1dc508771c8123/edge-modules/SimulatedTemperatureSensor/src/Program.cs). 

Azure Market'te sunulan ilk modülünüzde dağıtmak için aşağıdaki adımları kullanın:

1. İçinde [Azure portalında](https://portal.azure.com), girin **benzetimli sıcaklık algılayıcısı** arama ve açık Market sonucu.

   ![Azure portal Search'te benzetimli sıcaklık algılayıcısı](./media/iot-edge-deploy-module/search-for-temperature-sensor.png)

2. Bu modül almak için bir IOT Edge cihazı seçin. Üzerinde **hedef cihazlar IOT Edge modülü için** sayfasında, aşağıdaki bilgileri sağlayın:

   1. **Abonelik**: kullanmakta olduğunuz IOT hub'ı içeren aboneliği seçin.

   2. **IOT hub'ı**: kullanmakta olduğunuz IOT hub'ı adını seçin.

   3. **IOT Edge cihaz adı**: Bu hızlı başlangıçta daha önce önerilen cihaz adını kullandıysanız, girin **myEdgeDevice**. Ya da seçin **cihaz bulma** IOT Edge cihazlarının IOT hub'ınızda bir listesinden seçmek için. 
   
   4. **Oluştur**’u seçin.

3. Azure Marketi'nden bir IOT Edge modülü seçtiniz ve modül almak için bir IOT Edge cihazı seçmiş göre modül tam olarak nasıl dağıtılacağı tanımlamanıza yardımcı olan üç adımlık Sihirbazı yönlendirilirsiniz. İçinde **Ekle modülleri** adım Sihirbazı'nın dikkat **SimulatedTemperatureSensor** girdiğinizde modülüdür. Öğreticilerde dağıtımınıza ek modül eklemek için bu sayfayı kullanın. Bu hızlı başlangıçta, yalnızca bu bir modül dağıtın. Seçin **sonraki** sihirbazın sonraki adıma devam etmek için.

4. İçinde **yolları belirtin** adım Sihirbazı'nın, IOT hub'ına modüller arasında iletileri nasıl geçirildiğini tanımlayın. Hızlı Başlangıç için istediğiniz tüm modüllerdeki tüm iletileri IOT Hub'ına gidin (`$upstream`). Paneldeki değilse, aşağıdaki kodu ekleyin, ardından seçin **sonraki**:

   ```json
    {
    "routes": {
        "route": "FROM /messages/* INTO $upstream"
        }
    }
   ```

5. İçinde **gözden geçirme dağıtım** adım Sihirbazı'nın IOT Edge cihazınıza dağıtıldığı tüm modülleri tanımlayan JSON dosyasını önizlemesini görebilirsiniz. Dikkat **SimulatedTemperatureSensor** modülü bulunur ve iki ek sistem modülleri adlı **edgeAgent** ve **edgeHub**. Seçin **Gönder** bitirdiğinizde gözden geçirme.

   IOT Edge cihazı için yeni bir dağıtım gönderdiğinizde, hiçbir şey cihazınıza gönderilir. Bunun yerine, cihaz IOT hub'ı yeni yönergeleri için düzenli olarak sorgular. Cihaz güncelleştirilmiş dağıtım bildirimini bulursa, buluttan modül görüntüleri çekmek için yeni dağıtım hakkında bilgi kullanır ardından modüllerini yerel olarak çalışmaya başlar. Bu işlem birkaç dakika sürebilir. 

6. Modül dağıtım ayrıntıları gönderdikten sonra sihirbazın döndürür **IOT Edge** IOT hub'ınızın sayfası. Ayrıntılarını görmek için IOT Edge cihaz listesinden Cihazınızı seçin. 

7. Cihaz Ayrıntıları sayfasında aşağı kaydırarak **modülleri** bölümü. Üç modül listelenir: $edgeAgent $edgeHub ve SimulatedTemperatureSensor. Bir veya daha çok modülü olarak listede yoksa, IOT Edge cihaz tarafından bildirilen değil ancak dağıtımda belirtilen cihaz hala bunları başlatılıyor. Birkaç dakika bekleyin ve seçin **Yenile** sayfanın üstünde. 

   ![Dağıtılan modüller listesinde SimulatedTemperatureSensor görüntüle](./media/iot-edge-deploy-module/deployed-modules-marketplace.png)
