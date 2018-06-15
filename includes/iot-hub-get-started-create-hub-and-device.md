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
ms.openlocfilehash: f8cd78e63099f864c5fc54b6268f6e558d738626
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34371380"
---
## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-create-hub](iot-hub-create-hub.md)]

IOT hub'ı oluşturduğunuza göre cihazlar ve uygulamalar IOT hub'ınıza bağlanmak için kullanacağınız önemli bilgiler bulun. 

IOT hub Gezinti menüsünde açmak **paylaşılan erişim ilkeleri**.
Seçin **iothubowner** ilke ve kopyalayın **bağlantı dizesi---birincil anahtar** IOT hub'ınızın. Daha fazla bilgi için bkz. [IoT Hub'a erişimi denetleme](../articles/iot-hub/iot-hub-devguide-security.md).

   > [!NOTE] 
   > Bu kurulum öğretici için bu iothubowner bağlantı dizesi gerekmez. Bu kurulum tamamlandıktan sonra ancak, bazı farklı IOT senaryolarını ve öğreticiler için ihtiyacınız.

   ![IoT hub bağlantı dizenizi alma](./media/iot-hub-get-started-create-hub-and-device/create-iot-hub5.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a>Cihazınız için IoT hub’a cihaz kaydetme

1. IOT hub Gezinti menüsünde açmak **IOT cihazları**, ardından **Ekle** IOT hub'ınıza bir cihazı kaydetmek için.

   ![Bir cihaz IOT hub'ınızın IOT cihazları ekleme](./media/iot-hub-get-started-create-hub-and-device/create-identity-portal.png)

2. Girin bir **cihaz kimliği** yeni cihaz için. Cihaz Kimlikleri büyük/küçük harfe duyarlıdır.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. **Kaydet**’e tıklayın.
5. Cihaz oluşturulduktan sonra aygıt listesinden açmak **IOT cihazları** bölmesi.
6. Kopya **bağlantı dizesi---birincil anahtar** daha sonra kullanmak üzere.

   ![Cihaz bağlantı dizesini alma](./media/iot-hub-get-started-create-hub-and-device/device-connection-string.png)
