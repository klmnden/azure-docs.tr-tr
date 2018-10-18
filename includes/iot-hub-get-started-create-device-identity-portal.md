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
ms.openlocfilehash: ffd5da239f8e271a8c9b2aaf3f6d5fd9f885c79c
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49400144"
---
## <a name="create-a-device-identity"></a>Cihaz kimliği oluşturma

Bu bölümde, kullandığınız [Azure portalında](https://portal.azure.com) IOT hub'ınızdaki kimlik kayıt defterinde bir cihaz kimliği oluşturma. Kimlik kayıt defterinde girişi olmayan bir cihaz IoT hub'ına bağlanamaz. Daha fazla bilgi için "Kimlik kayıt defteri" bölümüne bakın. [IOT Hub Geliştirici kılavuzunun](../articles/iot-hub/iot-hub-devguide-identity-registry.md) kullanın **IOT cihazları** Cihazınızı tanımlamak için kullanılacak anahtarı ve benzersiz cihaz Kimliğini oluşturmak için portaldaki paneli IOT hub'ı kendisine. Cihaz kimlikleri büyük/küçük harfe duyarlıdır.

1. [Azure portalda](https://portal.azure.com) oturum açma

2. Seçin **tüm kaynakları** ve IOT hub'ı kaynağınızı bulun.

3. IOT hub'ı kaynağınız açıldığında tıklayın **IOT cihazları** aracı ve ardından **Ekle** en üstünde. 

    ![Portalı'nda cihaz kimliği oluşturma](./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png)

4. Gibi yeni cihaz için bir ad verin **myDeviceId**, tıklatıp **Kaydet**. Bu eylem, IOT hub'ınız için yeni bir cihaz kimliği oluşturur.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

   ![Yeni bir cihaz ekleyin](./media/iot-hub-get-started-create-device-identity-portal/create-a-device.png)

5. Cihaz listesinde yeni oluşturulan bir cihaz ve kopyalama tıklayın **bağlantı dizesi---birincil anahtar** daha sonra kullanmak üzere.

    ![Cihaz bağlantı dizesi](./media/iot-hub-get-started-create-device-identity-portal/device-details.png)

> [!NOTE]
> IoT Hub kimlik kayıt defteri, yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz kimliklerini depolar. Güvenlik kimlik bilgileri olarak kullanılmak üzere cihaz kimliklerini ve anahtarlarını ve tek bir cihaza erişimi devre dışı bırakmak için kullanabileceğiniz etkin/devre dışı bayrağını depolar. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Daha fazla bilgi için [IOT Hub Geliştirici kılavuzunun](../articles/iot-hub/iot-hub-devguide-identity-registry.md).
