---
title: Azure IOT Central uygulamanızı Windows IOT Core cihazı bağlayın | Microsoft Docs
description: Bir cihaz geliştirici olarak, Azure IOT Central uygulamanıza MXChip IOT DevKit cihaz bağlanmayı öğreneceksiniz.
author: miriambrus
ms.author: miriamb
ms.date: 04/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: e8d4ab46c598580a3a87f4344202f2700926bf5c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65510334"
---
# <a name="connect-a-windows-iot-core-device-to-your-azure-iot-central-application"></a>Azure IOT Central uygulamanızı Windows IOT Core cihazı bağlayın

Bu makalede, Microsoft Azure IOT Central uygulamanızı Windows IOT Core cihazı bağlamak için bir cihaz geliştirici olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

- Oluşturulan bir Azure IOT Central uygulamasına **örnek Devkits** uygulama şablonu. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).

- Windows 10 IoT Core işletim sistemi çalıştıran bir cihaz. Daha fazla bilgi için [Windows 10 IoT Core cihazınız ayarlanıyor](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup).

- Bir geliştirme makinesi ile [Node.js](https://nodejs.org/) 8.0.0 sürümü veya sonraki bir sürümü yüklü. Çalıştırabileceğiniz `node --version` sürümünüzü denetlemek için komut satırına. Node.js çeşitli işletim sistemleri için kullanılabilir.

## <a name="the-sample-devkits-application"></a>Örnek Devkits uygulama

Oluşturulan uygulama **örnek Devkits** uygulama şablonu içeren bir **Windows IOT Core** cihaz şablonu aşağıdaki özelliklere sahip:

- Cihazın telemetri ölçümleri: **Nem**, **sıcaklık**, ve **baskısı**.
- Denetlemek için bu ayarı **fanı hızı**.
- Bir cihaz özelliği **sayı öldürmüş** ve bulut özelliği **konumu**.

Cihaz şablon yapılandırması üzerinde tam ayrıntıları için bkz. [Windows IOT Core cihazı şablon ayrıntılarını](#device-template-details).

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IOT Central uygulamanızda kullanmak **Device Explorer** gerçek bir CİHAZDAN eklemek için sayfa **Windows 10 IoT Core** cihaz şablonu. Bağlantı ayrıntıları cihazın not edin (**kapsam kimliği**, **cihaz kimliği**, ve **birincil anahtar**). Daha fazla bilgi için [bağlantı bilgilerini almak](howto-generate-connection-string.md#get-connection-information).

## <a name="prepare-the-device"></a>Cihazı hazırlama

Cihaz IOT Central bağlanmak bir bağlantı dizesi gerekir.

[!INCLUDE [iot-central-howto-connection-string](../../includes/iot-central-howto-connection-string.md)]

İsteğe bağlı olarak cihaz kodu bağlantı dizesini erişmeye adlı bir dosyaya kaydedin **connection.string.iothub** klasöründe `C:\Data\Users\DefaultAccount\Documents\` Windows 10 IoT Core Cihazınızda.

Kopyalanacak **connection.string.iothub** Masaüstü makinenize dosyasından `C:\Data\Users\DefaultAccount\Documents\` kullanabileceğiniz klasör Cihazınızda [Windows Device Portal](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal):

1. Cihazınızda Windows Device Portal gitmek için web tarayıcınızı kullanın.
1. Cihazınızda dosyalara göz atmak için seçin **uygulamaları > Dosya Gezgini**.
1. Gidin **kullanıcı Folders\Documents**. Ardından karşıya **connection.string.iothub** dosyası:

    ![Bağlantı dizesi karşıya yükleme](media/howto-connect-windowsiotcore/device-portal.png)

## <a name="deploy-and-run"></a>Dağıtma ve çalıştırma

Dağıtma ve örnek uygulamayı Cihazınızda çalıştırmak için kullanabileceğiniz [Windows Device Portal](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal):

1. Cihazınızda Windows Device Portal gitmek için web tarayıcınızı kullanın.
1. Dağıtmak ve çalıştırmak için **Azure IOT Hub istemci** uygulaması seçin **uygulamaları > Hızlı çalıştırma örnekleri**. Ardından **Azure IOT Hub istemci**.
1. Ardından **dağıtma ve çalıştırma**.

    ![Dağıtma ve çalıştırma](media/howto-connect-windowsiotcore/quick-run.png)

Birkaç dakika sonra cihazınızın IOT Central uygulamanızda telemetriyi görüntüleyebilirsiniz.

[Windows Device Portal](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal) Cihazınızda sorun gidermesi için kullanabileceğiniz araçları içerir:

- **Uygulamaları manager** sayfa Cihazınızda çalıştıran uygulamaları denetlemenize olanak sağlar.
- Cihazınıza bağlı bir izleyici yoksa, kullanabileceğiniz **cihaz ayarları** cihazınızdan ekran görüntülerini yakalamak için sayfa. Örneğin:

    ![Uygulama ekran görüntüsü](media/howto-connect-windowsiotcore/iot-hub-foreground-client.png)

## <a name="download-the-source-code"></a>Kaynak kodunu indirebilir

Keşfedin ve istemci uygulaması için kaynak kodunu değiştirmek istiyorsanız, buradan indirebilirsiniz [Windows iotcore örnekleri GitHub deposunda](https://github.com/Microsoft/Windows-iotcore-samples/blob/master/Samples/Azure/IoTHubClients).

## <a name="device-template-details"></a>Cihaz şablonu ayrıntıları

Oluşturulan uygulama **örnek Devkits** uygulama şablonu içeren bir **Windows IOT Core** cihaz şablonu aşağıdaki özelliklere sahip:

### <a name="telemetry-measurements"></a>Telemetri ölçümleri

| Alan adı     | Birimler  | Minimum | Maksimum | Ondalık basamak sayısı |
| -------------- | ------ | ------- | ------- | -------------- |
| Nem oranı       | %      | 0       | 100     | 0              |
| Temp           | °C     | -40     | 120     | 0              |
| basınç       | hPa    | 260     | 1260    | 0              |

### <a name="settings"></a>Ayarlar

Sayısal ayarları

| Display name | Alan adı | Birimler | Ondalık basamak sayısı | Minimum | Maksimum | İlk |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Fan hızı    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

### <a name="properties"></a>Özellikler

| Tür            | Display name | Alan adı | Veri türü |
| --------------- | ------------ | ---------- | --------- |
| Cihaz özelliği | Sayı öldürmüş   | dieNumber  | number    |
| Text            | Location     | location   | Yok       |

## <a name="next-steps"></a>Sonraki adımlar

Windows IOT Core cihazı Azure IOT Central uygulamanızı bağlamak öğrendiniz, önerilen sonraki adıma öğrenmektir nasıl [bir özel cihaz şablonu ayarlama](howto-set-up-template.md) kendi IOT cihazını için.
