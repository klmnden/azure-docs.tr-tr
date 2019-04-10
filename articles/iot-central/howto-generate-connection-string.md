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
ms.openlocfilehash: 3a5e8d15d9a705892fe54c50e9b79e6d42af78d9
ms.sourcegitcommit: ef20235daa0eb98a468576899b590c0bc1a38394
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59426741"
---
# <a name="generate-a-device-connection-string-to-connect-to-an-azure-iot-central-application"></a>Bir Azure IOT Central uygulamasına bağlanmak için bir cihaz bağlantı dizesi oluştur

Bu makalede, bir IOT Central uygulamasına bağlanmak için gereken cihaz için bir bağlantı dizesi oluşturmak için bir cihaz geliştirici olarak nasıl. Bu makalede açıklanan yordam, tek bir cihaz bir paylaşılan erişim imzası (SAS) kullanarak hızlı bir şekilde bağlanmak nasıl gösterir. Bu yaklaşım, IOT Central ile denemeler ya da cihazları test yararlıdır. Bir üretim ortamında kullanmak alternatif yaklaşımlar için bkz: [Azure IOT Central, cihaz bağlantısı](concepts-connectivity.md).

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

1. Azure IOT Central bir uygulamadır. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
1. Bir geliştirme makinesi ile [Node.js](https://nodejs.org/) 8.0.0 sürümü veya sonraki bir sürümü yüklü. Çalıştırabileceğiniz `node --version` sürümünüzü denetlemek için komut satırına. Node.js çeşitli işletim sistemleri için kullanılabilir.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

Aşağıdaki adımlar, bir cihaz için bir SAS bağlantı dizesi oluşturmak ihtiyacınız olan bilgileri almak nasıl açıklar:

1. İçinde **Gezgini**, gerçek cihaz uygulamanızı bağlamak istediğiniz bulun:

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
* [Raspberry Pi'yi (Python) hazırlama ve bağlama](howto-connect-raspberry-pi-python.md)
* [Raspberry Pi'yi (C#) hazırlama ve bağlama](howto-connect-raspberry-pi-csharp.md)
* [Hazırlama ve Windows 10 IOT core cihazı bağlayın (C#)](howto-connect-windowsiotcore.md)
* [Genel bir Node.js istemcisi, Azure IOT Central uygulamanızı bağlayın](howto-connect-nodejs.md)