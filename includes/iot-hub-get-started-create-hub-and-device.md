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
ms.openlocfilehash: 9f7fee71fb2b80be93d978569791dbb57f137949
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346843"
---
## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-create-hub](iot-hub-create-hub.md)]

Bir IoT hub oluşturduktan sonra cihazları ve uygulamaları IoT hub’ınıza bağlamak için kullandığınız önemli bilgileri bulun. 

IOT hub'ı Gezinti menüsünde açın **paylaşılan erişim ilkeleri**. Seçin **iothubowner** ilkeyi ve ardından kopyalama **bağlantı dizesi---birincil anahtar** IOT hub'ınızın. Daha fazla bilgi için bkz. [IoT Hub'a erişimi denetleme](../articles/iot-hub/iot-hub-devguide-security.md).

   > [!NOTE] 
   > Bu kurulum öğreticisinde bu iothubowner bağlantı dizesi gerekmez. Bu kurulum tamamlandıktan sonra ancak bazı öğreticiler veya farklı IOT senaryolarınız için ihtiyacınız.

   ![IoT hub bağlantı dizenizi alma](./media/iot-hub-get-started-create-hub-and-device/create-iot-hub5.png)

## <a name="register-your-device-in-the-iot-hub"></a>Cihazınızı IOT hub'ı Kaydet

1. IOT hub'ı Gezinti menüsünde açın **IOT cihazları**, ardından **Ekle** IOT hub'ına cihaz kaydetmek için.

   ![IOT hub'ınızın IOT cihazları bir cihaz ekleyin](./media/iot-hub-get-started-create-hub-and-device/create-identity-portal.png)

2. Girin bir **cihaz kimliği** yeni cihaz için. Cihaz Kimlikleri büyük/küçük harfe duyarlıdır.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

3. **Kaydet**’e tıklayın.

4. Cihaz oluşturulduktan sonra cihaz listesindeki açık **IOT cihazları** bölmesi.

5. Kopyalama **bağlantı dizesi---birincil anahtar** daha sonra kullanmak üzere.

   ![Cihaz bağlantı dizesini alma](./media/iot-hub-get-started-create-hub-and-device/device-connection-string.png)
