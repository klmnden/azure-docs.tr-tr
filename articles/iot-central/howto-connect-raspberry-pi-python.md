---
title: Bağlantı Raspberry Pi'yi Azure IOT merkezi uygulamanıza (Python) | Microsoft Docs
description: Bir aygıt geliştiricisi olarak Python kullanarak Azure IOT merkezi uygulamanızı Raspberry Pi'yi bağlanma.
author: dominicbetts
ms.author: dobett
ms.date: 01/23/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: e9c2d18a518bd5c98fcc35efdb0dff36970a49b2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629074"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Azure IOT merkezi uygulamanıza (Python) Raspberry Pi'yi Bağlan

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Bu makalede Raspberry Pi'yi Python programlama dili kullanarak Microsoft Azure IOT merkezi bağlamak için bir aygıt geliştiricisi olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* Oluşturulan Azure IOT Merkezi uygulama **örnek Devkits** uygulama şablonu. Daha fazla bilgi için bkz: [Azure IOT merkezi uygulamanızı oluşturma](howto-create-application.md).
* Raspbian işletim sistemi çalıştıran bir Raspberry Pi'yi aygıt. Monitör, klavye ve fare, Raspberry Pi'yi bağlı GUI ortamı erişmek için gerekir. Raspberry Pi'yi yapabileceksiniz [Internet'e](https://www.raspberrypi.org/learning/software-guide/wifi/).
* İsteğe bağlı olarak, bir [algılama Hat](https://www.raspberrypi.org/products/sense-hat/) Raspberry Pi'yi eklenti Panosu. Bu kart Azure IOT merkezi uygulamanıza göndermek için çeşitli algılayıcılar telemetri verileri toplar. Yoksa bir **algılama Hat** Panosu, bunun yerine bir öykünücü kullanabilirsiniz.

Oluşturulan bir uygulamayı **örnek Devkits** uygulama şablonu içeren bir **Raspberry Pi'yi** cihaz şablonu aşağıdaki özelliklere sahip:

### <a name="telemetry-measurements"></a>Telemetri ölçümleri

| Alan adı     | Birimler  | Minimum | Maksimum | Ondalık basamak sayısı |
| -------------- | ------ | ------- | ------- | -------------- |
| nem oranı       | %      | 0       | 100     | 0              |
| Temp           | ° C     | -40     | 120     | 0              |
| basınç       | hPa    | 260     | 1260    | 0              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| accelerometerX | MG     | -2000   | 2000    | 0              |
| accelerometerY | MG     | -2000   | 2000    | 0              |
| accelerometerZ | MG     | -2000   | 2000    | 0              |
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

### <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IOT merkezi uygulamanızda gerçek bir aygıttan ekleyin **Raspberry Pi'yi** cihaz şablonu ve cihaz bağlantı dizesini Not. Daha fazla bilgi için bkz: [gerçek bir cihazı Azure IOT merkezi uygulamanıza eklemek](tutorial-add-device.md).

## <a name="configure-the-raspberry-pi"></a>Böğürtlenli Pi yapılandırın

Aşağıdaki adımlar, indirme ve github'dan örnek Python uygulama yapılandırma açıklar. Bu örnek uygulama:

* Telemetri ve özellik değerleri Azure IOT merkezi gönderir.
* Azure IOT Merkezi içinde yapılan değişiklikler ayarını yanıt verir.

> [!NOTE]
> Raspberry Pi Python örneği hakkında daha fazla bilgi için bkz: [Benioku](https://github.com/Microsoft/microsoft-iot-central-firmware/blob/master/RaspberryPi/README.md) github'da dosya.

1. Web tarayıcısı gitmek için de Raspberry Pi'yi Masaüstü kullanın [Azure IOT merkezi bellenim sürümleri](https://github.com/Microsoft/microsoft-iot-central-firmware/releases) sayfası.

1. Ev klasörünüze Raspberry Pi'yi üzerindeki en son bellenim içeren zip dosyasını indirin. Dosya adı arar gibi `RaspberryPi-IoTCentral-X.X.X.zip`.

1. Bellenim dosyanın sıkıştırmasını açmak için kullanmak **Dosya Yöneticisi** Raspberry Pi'yi Masaüstü. Zip dosyasını sağ tıklatın ve seçin **burada ayıklamak**. Bu işlem adlı bir klasör oluşturur `RaspberryPi-IoTCentral-X.X.X` ev klasörünüzdeki.

1. Yoksa bir **algılama Hat** , Raspberry Pi'yi bağlı Panosu öykünücü etkinleştirmeniz gerekir:
    1. İçinde **Dosya Yöneticisi**, `RaspberryPi-IoTCentral-X.X.X` klasörünü sağ tıklatın **config.iot** dosya ve seçin **metin düzenleyici**.
    1. Satırı değiştirin `"simulateSenseHat": false,` için `"simulateSenseHat": true,`.
    1. Değişiklikleri kaydedin ve kapatın **metin düzenleyici**.

1. Başlangıç bir **Terminal** oturum ve kullanım `cd` önceki adımda oluşturduğunuz klasöre gitmek için komutu.

1. Çalışan örnek uygulamayı başlatmak için şunu yazın `./start.sh` içinde **Terminal** penceresi. Kullanıyorsanız **algılama HAT öykünücüsü**, kendi GUI görüntüler. Azure IOT merkezi uygulamanıza gönderilen telemetri değerlerini değiştirmek için GUI kullanabilirsiniz.

1. **Terminal** penceresi benzer bir ileti görüntüler `Device information being served at http://192.168.0.60:8080`. URL, ortamınızda farklı olabilir. URL'yi kopyalayın ve yapılandırma sayfasına gitmek için Web tarayıcısını kullanın:

    ![Aygıt yapılandırma](media/howto-connect-raspberry-pi-python/configure.png)

1. Gerçek bir cihazı Azure IOT merkezi uygulamanıza zaman eklediğiniz bir not yapılan cihaz bağlantı dizesini girin. Ardından **aygıtı Yapılandır**. Bir ileti görür **yapılandırılan aygıtı, Cihazınızı verileri kısa bir süre içinde Azure IOT merkezi gönderme başlamalıdır**.

1. Azure IOT merkezi uygulamanızda Raspberry Pi'yi üzerinde çalışan kodu uygulama ile nasıl etkileşim kurduğu bakın:

    * Üzerinde **ölçümleri** sayfa gerçek cihazınız için Raspberry Pi'yi gönderilen telemetriyi görebilirsiniz. Kullanıyorsanız **algılama HAT öykünücüsü**, GUI Raspberry Pi'yi üzerinde telemetri değerleri değiştirebilirsiniz.
    * Üzerinde **özellikleri** sayfasında, bildirilen değeri görebilirsiniz **öldürmüş numarası** özelliği.
    * Üzerinde **ayarları** sayfasında Raspberry Pi'yi voltaj ve fan hızı gibi çeşitli ayarları değiştirebilirsiniz. Raspberry Pi'yi değişikliği kabul ettikten sonra ayarı olarak gösterir **eşitlenen** Azure IOT merkezi olarak.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT merkezi uygulamanıza Raspberry Pi'yi bağlanma öğrendiniz, önerilen sonraki adımlar şunlardır:

* [Bir genel Node.js istemci uygulamaya Azure IOT merkezi bağlama](howto-connect-nodejs.md)