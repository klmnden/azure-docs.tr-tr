---
title: Azure IOT Central uygulamanızı SensorTile.box cihaz bağlayın | Microsoft Docs
description: Bir cihaz geliştirici olarak, Azure IOT Central uygulamanıza SensorTile.box cihaz bağlanmayı öğreneceksiniz.
author: sarahhubbard
ms.author: sahubbar
ms.date: 04/24/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: sandeep.pujar
ms.openlocfilehash: 8c1b4a4ab834b2203a7e0b6e4e9e366c3fc38774
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65472250"
---
# <a name="connect-sensortilebox-device-to-your-azure-iot-central-application"></a>Azure IOT Central uygulamanıza SensorTile.box cihazı bağlayın

Bu makalede, Microsoft Azure IOT Central uygulamanıza SensorTile.box cihaz bağlayamama ek olarak, cihaz geliştirici olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için aşağıdaki kaynakları gerekir:

* Bir SensorTile.box cihaz. Daha fazla bilgi için [SensorTile.box](https://www.st.com/content/st_com/en/products/evaluation-tools/product-evaluation-tools/mems-motion-sensor-eval-boards/steval-mksbox1v1.html).
* Android Cihazınızda yüklü ST BLO algılayıcı uygulama yapabilecekleriniz [buradan indirin](https://play.google.com/store/apps/details?id=com.st.bluems). Daha fazla bilgi için bkz.: [ST BLO algılayıcısı](https://www.st.com/stblesensor)
* Oluşturulan bir Azure IOT Central uygulamasına **DevKits** uygulama şablonu. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
* Ekleme **SensorTile.box** cihaz şablonu ederek, IOT Central uygulamasına **cihaz şablonları** tıklayarak sayfasında **+ yeni**, seçerek**SensorTile.box** şablonu.

### <a name="get-your-device-connection-details"></a>Cihazınızı bağlantı ayrıntılarını Al

Azure IOT Central uygulamanızda gerçek bir CİHAZDAN ekleme **SensorTile.box** cihaz şablonu ve cihaz bağlantı ayrıntılarını not olun: **Kapsam kimliği**, **cihaz kimliği**, ve **birincil anahtar**:

1. Device Explorer bir cihaz ekleyin. Seçin **+ yeni > gerçek** gerçek bir cihaz eklemek için.

    * Bir küçük harf girin **cihaz kimliği**, veya önerilen **cihaz kimliği**.
    * Girin bir **cihaz adı**, ya da önerilen adı kullanın

    ![Cihaz Ekleme](media/howto-connect-sensortile/real-device.png)

1. Cihaz bağlantı ayrıntılarını almak için **kapsam kimliği**, **cihaz kimliği**, ve **birincil anahtar**seçin **Connect** cihaz sayfasında.

    ![Bağlantı ayrıntıları](media/howto-connect-sensortile/connect-device.png)

1. Bağlantı ayrıntılarını not edin. Sonraki adımda DevKit Cihazınızı hazırlarken geçici olarak internet'ten kesilirse.

## <a name="set-up-the-sensortilebox-with-the-mobile-application"></a>Mobil uygulama ile SensorTile.box ayarlama

Bu bölümde, cihaza uygulama bellenim gönderme konusunda bilgi edinin. Ardından cihaz verilerini IOT Central Bluetooth düşük enerji (etkinleştir) bağlantısı kullanarak ST BLO algılayıcı mobil uygulamada gönderme.

1. ST BLO algılayıcı uygulama açıp tuşuna **yeni uygulama oluştur** düğmesi.

    ![uygulama oluşturma](media/howto-connect-sensortile/create-app.png)

1. Seçin **barometre** uygulama.
1. Karşıya yükle düğmesine basın.

    ![Barometre karşıya yükleme](media/howto-connect-sensortile/barometer-upload.png)

1. İle SensorTile.box ilişkili YÜRÜT düğmesine basın.
1. İşlem tamamlandığında SensorTile.box BLO üzerinde sıcaklık, baskısı ve nem akışları.

## <a name="connect-the-sensortilebox-to-the-cloud"></a>SensorTile.box buluta bağlayın

Bu bölümde SensorTile.box mobil uygulamaya bağlanmak ve mobil uygulamanızı buluta bağlayın öğrenin.

1. Sol menüyü kullanarak seçin **bulut günlüğü** düğmesi.

    ![Bulut günlüğü](media/howto-connect-sensortile/cloud-logging.png)

1. Seçin **Azure IOT Central** bulut sağlayıcısı olarak.
1. Cihaz kimliği ve daha önce belirtildiği kapsam kimliği ekleyin.

    ![Kimlik Bilgileri](media/howto-connect-sensortile/credentials.png)

1. Seçin **uygulama anahtarı** radyo düğmesi.
1. Tıklayın **Connect** ve telemetri verileri karşıya yüklemek istediğiniz seçin.
1. Birkaç saniye sonra verileri IOT Central uygulama panosunda görünür.

## <a name="sensortilebox-device-template-details"></a>SensorTile.box cihaz şablonu ayrıntıları

Aşağıdaki özelliklere sahip SensorTile.box cihaz şablondan oluşturulan bir uygulama:

### <a name="telemetry"></a>Telemetri

| Alan adı     | Birimler  | Minimum | Maksimum | Ondalık basamak sayısı |
| -------------- | ------ | ------- | ------- | -------------- |
| Nem oranı       | %      | 30       | 90     | 1              |
| Temp           | °C     | 0     | 40     | 1              |
| basınç       | mbar    | 900     | 1100    | 2              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| accelerometerX | Yönetim grubu     | -2000   | 2000    | 0              |
| accelerometerY | Yönetim grubu     | -2000   | 2000    | 0              |
| accelerometerZ | Yönetim grubu     | -2000   | 2000    | 0              |
| gyroscopeX     | dps   | -3276   | 3276    | 1              |
| gyroscopeY     | dps   | -3276   | 3276    | 1              |
| gyroscopeZ     | dps   | -3276   | 3276    | 1              |
| FFT_X     |    |    |     |               |
| FFT_Y     |    |    |     |               |
| FFT_Z     |    |    |     |               |

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central uygulamanıza bir SensorTile.box bağlanmak öğrendiniz, önerilen sonraki adıma öğrenmektir [nasıl bir özel cihaz şablonu ayarlama](howto-set-up-template.md) kendi IOT cihazını için.
