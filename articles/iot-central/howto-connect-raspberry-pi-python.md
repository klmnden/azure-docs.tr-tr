---
title: Azure IOT Central uygulamanıza (Python) Raspberry Pi'yi Connnect | Microsoft Docs
description: Bir cihaz geliştirici olarak Python kullanarak Azure IOT Central uygulamanızı Raspberry Pi'yi bağlanma.
author: dominicbetts
ms.author: dobett
ms.date: 01/23/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: aa2d8f50d8fb4ba356af20a290976b8b32601ebf
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188800"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Raspberry Pi'yi bağlanmak, Azure IOT Central uygulamasına (Python)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Bu makalede, Raspberry Pi'yi Python programlama dili kullanarak, Microsoft Azure IOT Central uygulamasına bağlanmak için bir cihaz geliştirici olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* Oluşturulan bir Azure IOT Central uygulamasına **örnek Devkits** uygulama şablonu. Daha fazla bilgi için [Azure IOT Central uygulaması oluşturmayı](howto-create-application.md).
* Raspbian işletim sistemi çalıştıran bir Raspberry Pi cihaz. Monitör, klavye ve fare Raspberry Pi'yi bağlı GUI ortama erişmek için gereksinim duyduğunuz. Raspberry Pi şunları [İnternet'e](https://www.raspberrypi.org/learning/software-guide/wifi/).
* İsteğe bağlı olarak, bir [algılama Hat](https://www.raspberrypi.org/products/sense-hat/) Raspberry Pi eklenti Panosu. Bu pano, Azure IOT Central uygulamasına göndermek için çeşitli sensörlerden alınan telemetri verileri toplar. Yoksa bir **algılama Hat** Panosu, bunun yerine bir öykünücü kullanabilirsiniz.

## <a name="sample-devkits-application"></a>**Örnek Devkits** uygulama

Oluşturulan uygulama **örnek Devkits** uygulama şablonu içeren bir **Raspberry Pi** cihaz şablonu aşağıdaki özelliklere sahip: 

- Cihaz için ölçüler içeren telemetri **nem**, **sıcaklık**, **baskısı**, **Magnometer** (X ölçülür. Y, Z ekseni), **Accelorometer** (ölçülen X, Y, Z ekseni) ve **jiroskop** (X, Y ölçülür Z ekseni).
- Ayarları gösteren **voltaj**, **geçerli**,**fanı hızı** ve **IR** Aç/Kapat.
- Cihaz özelliği içeren özellik **sayı öldürmüş** ve **konumu** bulut özelliği.


Cihaz şablon yapılandırması hakkında tam Ayrıntılar için bkz [Raspberry PI cihaz şablonu ayrıntıları](howto-connect-raspberry-pi-python.md#raspberry-pi-device-template-details)
    

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IOT Central uygulamanızda gerçek bir CİHAZDAN ekleme **Raspberry Pi** cihaz şablonu ve cihaz bağlantı dizesini Not olun. Daha fazla bilgi için [Azure IOT Central uygulamanıza gerçek bir cihaz eklemek](tutorial-add-device.md).

### <a name="configure-the-raspberry-pi"></a>Raspberry Pi yapılandırın

Aşağıdaki adımlar, indirme ve örnek Python uygulamasını github'dan yapılandırma açıklanmaktadır. Bu örnek uygulama:

* Telemetri ve özellik değerleri, Azure IOT Central uygulamasına gönderir.
* Azure IOT Central yapılan değişiklikleri ayarını yanıtlar.

> [!NOTE]
> Raspberry Pi Python örneği hakkında daha fazla bilgi için bkz: [Benioku](https://github.com/Azure/iot-central-firmware/blob/master/RaspberryPi/README.md) github'da dosya.

1. Web tarayıcısı Raspberry Pi Masaüstü gezinmek için kullanın. [Azure IOT Central bellenim sürümleri](https://github.com/Azure/iot-central-firmware/releases) sayfası.

1. Giriş klasörünüze Raspberry Pi üzerinde en son üretici yazılımı içeren zip dosyasını indirin. Dosya adı gibi görünen `RaspberryPi-IoTCentral-X.X.X.zip`.

1. Üretici yazılımı dosyanın sıkıştırmasını açın kullanın **Dosya Yöneticisi** Raspberry Pi Desktop. Zip dosyasını sağ tıklatın ve seçin **buradan ayıklamak**. Bu işlem, adlı bir klasör oluşturur. `RaspberryPi-IoTCentral-X.X.X` giriş klasörünüzde.

1. Yoksa bir **algılama Hat** Raspberry Pi'yi bağlı Pano öykünücü etkinleştirmeniz gerekir:
    1. İçinde **Dosya Yöneticisi**, `RaspberryPi-IoTCentral-X.X.X` klasörü sağ tıklatın **config.iot** seçin ve dosya **metin düzenleyici**.
    1. Satırını `"simulateSenseHat": false,` için `"simulateSenseHat": true,`.
    1. Değişiklikleri kaydedin ve kapatın **metin düzenleyici**.

1. Başlangıç bir **Terminal** oturumu ve kullanım `cd` komut önceki adımda oluşturduğunuz klasöre gidin.

1. Çalışan örnek uygulamayı başlatmak için aşağıdakileri yazın `./start.sh` içinde **Terminal** penceresi. Kullanıyorsanız **algılama HAT öykünücü**, kendi GUI görüntüler. GUI kullanarak Azure IOT Central uygulamasına gönderilen telemetri değerlerini değiştirmek için kullanabilirsiniz.

1. **Terminal** penceresi şuna benzer bir ileti görüntüler `Device information being served at http://192.168.0.60:8080`. URL, ortamınızda farklı olabilir. URL'yi kopyalayın ve yapılandırma sayfasına gitmek için Web tarayıcısını kullanın:

    ![Cihaz yapılandırma](media/howto-connect-raspberry-pi-python/configure.png)

1. Ne zaman gerçek bir cihaz, Azure IOT Central uygulamaya eklenen not yapılan cihaz bağlantı dizesini girin. Ardından **cihazı yapılandırma**. Bir ileti görürsünüz **cihaz yapılandırılmış, cihazınızın verileri kısa bir süre içinde Azure IOT Central göndermeye başlaması gerektiğini**.

1. Azure IOT Central uygulamanızda Raspberry Pi üzerinde çalışan kodu uygulaması ile nasıl etkileşim kurduğunu bakın:

    * Üzerinde **ölçümleri** sayfa gerçek cihazınız için Raspberry Pi'dan gönderilen telemetriyi görebilirsiniz. Kullanıyorsanız **algılama HAT öykünücü**, Raspberry Pi üzerinde GUI'de telemetri değerleri değiştirebilirsiniz.
    * Üzerinde **özellikleri** sayfasında değeri bildirilen gördüğünüz **öldürmüş numarası** özelliği.
    * Üzerinde **ayarları** sayfasında, Raspberry Pi voltaj ve giriş hızı gibi çeşitli ayarları değiştirebilirsiniz. Raspberry Pi değişikliği bildirir, ayarı olarak gösterir **eşitlenen** Azure IOT Central içinde.


## <a name="raspberry-pi-device-template-details"></a>Raspberry PI cihaz şablonu ayrıntıları

Oluşturulan uygulama **örnek Devkits** uygulama şablonu içeren bir **Raspberry Pi** cihaz şablonu aşağıdaki özelliklere sahip:

### <a name="telemetry-measurements"></a>Telemetri ölçümleri

| Alan adı     | Birimler  | Minimum | Maksimum | Ondalık basamak sayısı |
| -------------- | ------ | ------- | ------- | -------------- |
| Nem oranı       | %      | 0       | 100     | 0              |
| Temp           | ° C     | -40     | 120     | 0              |
| basınç       | hPa    | 260     | 1260    | 0              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| accelerometerX | Yönetim grubu     | -2000   | 2000    | 0              |
| accelerometerY | Yönetim grubu     | -2000   | 2000    | 0              |
| accelerometerZ | Yönetim grubu     | -2000   | 2000    | 0              |
| gyroscopeX     | MDP'ler   | -2000   | 2000    | 0              |
| gyroscopeY     | MDP'ler   | -2000   | 2000    | 0              |
| gyroscopeZ     | MDP'ler   | -2000   | 2000    | 0              |

### <a name="settings"></a>Ayarlar

Sayısal ayarları

| Görünen ad | Alan adı | Birimler | Ondalık basamak sayısı | Minimum | Maksimum | İlk |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Voltaj      | setVoltage | Volt | 0              | 0       | 240     | 0       |
| Geçerli      | setCurrent | Amp  | 0              | 0       | 100     | 0       |
| Fan hızı    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Geçiş ayarları

| Görünen ad | Alan adı | Metni | Metin kapalı | İlk |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | AÇIK      | KAPALI      | Kapalı     |

### <a name="properties"></a>Özellikler

| Tür            | Görünen ad | Alan adı | Veri türü |
| --------------- | ------------ | ---------- | --------- |
| Cihaz özelliği | Sayı öldürmüş   | dieNumber  | number    |
| Metin            | Konum     | location   | Yok       |

## <a name="next-steps"></a>Sonraki adımlar

Raspberry Pi'yi, Azure IOT Central uygulamasına bağlanmak öğrendiniz, önerilen sonraki adımlar şunlardır:

* [Azure IOT Central için genel bir Node.js istemci uygulaması bağlama](howto-connect-nodejs.md)
