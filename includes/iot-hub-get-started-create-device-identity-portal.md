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
ms.openlocfilehash: 1df3e188b71b8fa2d5223bad8bc5914513e26286
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "34371210"
---
## <a name="create-a-device-identity"></a>Cihaz kimliği oluşturma

Bu bölümde, kullandığınız [Azure portalında] [ lnk-azure-portal] IOT hub'ınızdaki kimlik kayıt defterinde bir cihaz kimliği oluşturma. Kimlik kayıt defterinde girişi olmayan bir cihaz IoT hub'ına bağlanamaz. Daha fazla bilgi için [IoT Hub geliştirici kılavuzunun][lnk-devguide-identity] "Kimlik kayıt defteri" bölümüne bakın. Kullanma **IOT cihazları** cihazınızın kendisini IOT Hub'ına tanıtmak için kullanılacak anahtarı ve benzersiz cihaz Kimliğini oluşturmak için portaldaki paneli. Cihaz kimlikleri büyük/küçük harfe duyarlıdır.

1. [Azure portalında][lnk-azure-portal] oturum açın.

1. Seçin **tüm kaynakları** ve IOT hub'ı kaynağınızı bulun.

1. IOT hub'ı kaynağınız açıldığında tıklayın **IOT cihazları** aracı ve ardından **Ekle** en üstünde. 

    ![Portalı'nda cihaz kimliği oluşturma][img-add-device]

1. Gibi yeni cihaz için bir ad verin **myDeviceId**, tıklatıp **Kaydet**. Bu eylem, IOT hub'ınız için yeni bir cihaz kimliği oluşturur.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

   ![Yeni bir cihaz ekleyin][img-create-device]

1. Cihaz listesinde yeni oluşturulan bir cihaz ve kopyalama tıklayın **bağlantı dizesi---birincil anahtar** daha sonra kullanmak üzere.

    ![Cihaz bağlantı dizesi][img-connection-string]

> [!NOTE]
> IoT Hub kimlik kayıt defteri, yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz kimliklerini depolar. Güvenlik kimlik bilgileri olarak kullanılmak üzere cihaz kimliklerini ve anahtarlarını ve tek bir cihaza erişimi devre dışı bırakmak için kullanabileceğiniz etkin/devre dışı bayrağını depolar. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Daha fazla bilgi için bkz. [IoT Hub geliştirici kılavuzu][lnk-devguide-identity].

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-add-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png
[img-create-device]:./media/iot-hub-get-started-create-device-identity-portal/add-device.png

<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

