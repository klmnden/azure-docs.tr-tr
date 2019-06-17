---
title: Azure IOT Central için bir cihaz bağlantı dizesi oluştur | Microsoft Docs
description: Bir cihaz geliştirici olarak, bir IOT Central uygulamasına bağlanmak için gereken cihaz için bir bağlantı dizesi nasıl miyim oluşturmaz?
author: dominicbetts
ms.author: dobett
ms.date: 04/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: f302cbfa7152ae30be434f560c0c39056d40f9f4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60885657"
---
# <a name="generate-a-device-connection-string-to-connect-to-an-azure-iot-central-application"></a>Bir Azure IOT Central uygulamasına bağlanmak için bir cihaz bağlantı dizesi oluştur

Bu makalede, bir IOT Central uygulamasına bağlanmak için gereken cihaz için bir bağlantı dizesi oluşturmak için bir cihaz geliştirici olarak nasıl. Bu makalede açıklanan yordam, tek bir cihaz bir paylaşılan erişim imzası (SAS) kullanarak hızlı bir şekilde bağlanın gösterilir. Bu yaklaşım, IOT Central ile denemeler ya da cihazları test yararlıdır. Bir üretim ortamında kullanmak alternatif yaklaşımlar için bkz: [Azure IOT Central, cihaz bağlantısı](concepts-connectivity.md).

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

- Azure IOT Central bir uygulamadır. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
- Bir geliştirme makinesi ile [Node.js](https://nodejs.org/) 8.0.0 sürümü veya sonraki bir sürümü yüklü. Çalıştırabileceğiniz `node --version` sürümünüzü denetlemek için komut satırına. Node.js çeşitli işletim sistemleri için kullanılabilir.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

Aşağıdaki adımlar, bir cihaz için bir SAS bağlantı dizesi oluşturmak ihtiyacınız olan bilgileri almak nasıl açıklar:

1. İçinde **Device Explorer**, gerçek cihaz uygulamanızı bağlamak istediğiniz bulun:

    ![Gerçek bir cihaz seçin](media/howto-generate-connection-string/real-devices.png)

1. Üzerinde **cihaz** sayfasında **Connect**:

    ![Select bağlanma](media/howto-generate-connection-string/connect.png)

1. Bağlantı ayrıntılarını Not **kapsam kimliği**, **cihaz kimliği**, ve **cihaz birincil anahtar**, aşağıdaki adımları kullanın:

    ![Bağlantı ayrıntıları](media/howto-generate-connection-string/device-connect.png)

    Bu sayfada kaydetmek için değerleri kopyalayabilirsiniz.

## <a name="generate-the-connection-string"></a>Bağlantı dizesi oluştur

[!INCLUDE [iot-central-howto-connection-string](../../includes/iot-central-howto-connection-string.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central uygulamanızı bağlamak gerçek bir cihaz için bir bağlantı dizesi oluşturduğunuza göre önerilen sonraki adımlar şunlardır:

* [Hazırlama ve DevKit cihazı (C) bağlayın](howto-connect-devkit.md)
* [Hazırlama ve Raspberry Pi'yi (Python) bağlanma](howto-connect-raspberry-pi-python.md)
* [Hazırlama ve Raspberry Pi'yi bağlanın (C#)](howto-connect-raspberry-pi-csharp.md)
* [Hazırlama ve Windows 10 IOT core cihazı bağlayın (C#)](howto-connect-windowsiotcore.md)
* [Genel bir Node.js istemcisini Azure IoT Central uygulamanıza bağlama](howto-connect-nodejs.md)