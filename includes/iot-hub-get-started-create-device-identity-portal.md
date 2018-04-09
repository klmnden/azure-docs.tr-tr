---
title: include dosyası
description: include dosyası
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 04/05/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: b08bfcd4cb9e85f9e682efe0f599b6dd88897962
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
## <a name="create-a-device-identity"></a>Cihaz kimliği oluşturma

Bu bölümde, kullandığınız [Azure portal] [ lnk-azure-portal] IOT hub'ınızdaki kimlik kayıt defterinde bir cihaz kimliği oluşturmak için. Kimlik kayıt defterinde girişi olmayan bir cihaz IoT hub'ına bağlanamaz. Daha fazla bilgi için [IoT Hub geliştirici kılavuzunun][lnk-devguide-identity] "Kimlik kayıt defteri" bölümüne bakın. Kullanmak **IOT cihazları** benzersiz cihaz kimliği oluşturur ve kendisini IOT Hub'ına tanıtmak için kullanılacak cihazınız için anahtar portal panelinde. Cihaz kimlikleri büyük/küçük harfe duyarlıdır.

1. Emin olun oturum açmaya için [Azure portal][lnk-azure-portal].

1. Çubuğunda **tüm kaynakları** ve IOT hub'ı kaynağınız bulabilirsiniz.

    ![IOT hub'ına gidin][img-find-iothub]

1. IOT hub'ı kaynağınız açıldığında tıklatın **IOT cihazları** aracı ve ardından **Ekle** üstünde. İçin yeni Cihazınızı gibi ad **myDeviceId**, tıklatıp **kaydetmek**.

    ![Portalda cihaz kimliği oluşturma][img-create-device]

   Bu eylem, IOT hub'ınız için yeni bir cihaz kimliği oluşturur.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. İçinde **IOT cihazları**'s aygıt listesinde yeni oluşturulan cihaz ve Not **bağlantı dizesi---birincil anahtar**.

    ![Cihaz bağlantı dizesi][img-connection-string]

> [!NOTE]
> IoT Hub kimlik kayıt defteri, yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz kimliklerini depolar. Güvenlik kimlik bilgileri olarak kullanılmak üzere cihaz kimliklerini ve anahtarlarını ve tek bir cihaza erişimi devre dışı bırakmak için kullanabileceğiniz etkin/devre dışı bayrağını depolar. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Daha fazla bilgi için bkz. [IoT Hub geliştirici kılavuzu][lnk-devguide-identity].

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-create-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png


<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

