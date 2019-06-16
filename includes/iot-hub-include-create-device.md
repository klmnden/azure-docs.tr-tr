---
title: include dosyası
description: include dosyası
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 11/06/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: d70544866b9e321d98acd3978da145276e036025
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66146527"
---
<!-- put the ## header in the file that includes this file -->

Bu bölümde, IOT hub'ına kimlik kayıt defterinde cihaz kimliği oluşturma. Kimlik kayıt defterinde girişi olmayan bir cihaz IoT hub'ına bağlanamaz. Daha fazla bilgi için "Kimlik kayıt defteri" bölümüne bakın. [IOT Hub Geliştirici Kılavuzu](../articles/iot-hub/iot-hub-devguide-identity-registry.md) 

1. IOT hub'ı Gezinti menüsünde açın **IOT cihazları**, ardından **Ekle** yeni bir cihaz IOT hub'ınıza kaydetmek için.

    ![Portalı'nda cihaz kimliği oluşturma](./media/iot-hub-include-create-device/create-identity-portal.png)

1. Gibi yeni cihaz için bir ad verin **myDeviceId**seçip **Kaydet**. Bu eylem, IOT hub'ınız için yeni bir cihaz kimliği oluşturur.

   ![Yeni bir cihaz ekleyin](./media/iot-hub-include-create-device/create-a-device.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]


1. Cihaz oluşturulduktan sonra cihaz listesindeki açık **IOT cihazları** bölmesi. Kopyalama **bağlantı dizesi---birincil anahtar** daha sonra kullanmak üzere.

    ![Cihaz bağlantı dizesi](./media/iot-hub-include-create-device/device-details.png)

> [!NOTE]
> IoT Hub kimlik kayıt defteri, yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz kimliklerini depolar. Güvenlik kimlik bilgileri olarak kullanılmak üzere cihaz kimliklerini ve anahtarlarını ve tek bir cihaza erişimi devre dışı bırakmak için kullanabileceğiniz etkin/devre dışı bayrağını depolar. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Daha fazla bilgi için [IOT Hub Geliştirici kılavuzunun](../articles/iot-hub/iot-hub-devguide-identity-registry.md).
