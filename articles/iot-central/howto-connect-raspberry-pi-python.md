---
title: Raspberry Pi'yi Azure IOT Central uygulamanızı (Python) bağlayın | Microsoft Docs
description: Bir cihaz geliştirici olarak Python kullanarak Azure IOT Central uygulamanızı Raspberry Pi'yi bağlanma.
author: dominicbetts
ms.author: dobett
ms.date: 04/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: eccc4100c89c971e264b9b915cd17b9f5ce4477b
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59617456"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Raspberry Pi'yi bağlanmak, Azure IOT Central uygulamasına (Python)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Bu makalede, Raspberry Pi'yi Python programlama dili kullanarak, Microsoft Azure IOT Central uygulamasına bağlanmak için bir cihaz geliştirici olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için aşağıdaki bileşenleri gerekir:

* Oluşturulan bir Azure IOT Central uygulamasına **örnek Devkits** uygulama şablonu. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
* Raspbian işletim sistemi çalıştıran bir Raspberry Pi cihaz. Raspberry Pi internet'e bağlanabiliyor olmanız gerekir. Daha fazla bilgi için [Raspberry Pi'yi ayarlama](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/3).

## <a name="sample-devkits-application"></a>**Örnek Devkits** uygulama

Oluşturulan uygulama **örnek Devkits** uygulama şablonu içeren bir **Raspberry Pi** cihaz şablonu aşağıdaki özelliklere sahip:

- Telemetri, cihaz toplayacak aşağıdaki ölçüleri içerir:
  - Nem oranı
  - Sıcaklık
  - Basınç
  - Magnetometer (X, Y, Z)
  - İvme ölçer (X, Y, Z)
  - Jiroskop (X, Y, Z)
- Ayarlar
  - Voltaj
  - Geçerli
  - Fan hızı
  - IR Aç/Kapat.
- Özellikler
  - Numara cihaz özelliği öldürmüş
  - Konum bulut özelliği

Cihaz şablon yapılandırmasının tam Ayrıntılar için bkz. [Raspberry Pi cihaz şablon ayrıntılarını](howto-connect-raspberry-pi-python.md#raspberry-pi-device-template-details).

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IOT Central uygulamanızda gerçek bir CİHAZDAN ekleme **Raspberry Pi** cihaz şablonu. Bağlantı ayrıntıları cihazın not edin (**kapsam kimliği**, **cihaz kimliği**, ve **birincil anahtar**). Daha fazla bilgi için [Azure IOT Central uygulamanıza gerçek bir cihaz eklemek](tutorial-add-device.md).

### <a name="configure-the-raspberry-pi"></a>Raspberry Pi yapılandırın

Aşağıdaki adımlar, indirme ve örnek Python uygulamasını github'dan yapılandırma açıklanmaktadır. Bu örnek uygulama:

* Telemetri ve özellik değerleri, Azure IOT Central uygulamasına gönderir.
* Azure IOT Central yapılan değişiklikleri ayarını yanıtlar.

Cihaz yapılandırma [GitHub üzerinde adım adım yönergeleri](https://github.com/Azure/iot-central-firmware/blob/master/RaspberryPi/README.md).

1. Cihaz yapılandırıldığında, Cihazınızı Azure IOT Central için telemetri ölçümleri gönderme başlatır.
1. Azure IOT Central uygulamanızda Raspberry Pi üzerinde çalışan kodu uygulaması ile nasıl etkileşim kurduğunu görebilirsiniz:

    * Üzerinde **ölçümleri** sayfa gerçek cihazınız için Raspberry Pi'dan gönderilen telemetriyi görebilirsiniz.
    * Üzerinde **ayarları** sayfasında, voltaj ve giriş hızı gibi Raspberry Pi üzerinde ayarlarını değiştirebilirsiniz. Raspberry Pi değişikliği bildirir, ayarı olarak gösterir **eşitlenen**.

## <a name="raspberry-pi-device-template-details"></a>Raspberry Pi cihaz şablonu ayrıntıları

Oluşturulan uygulama **örnek Devkits** uygulama şablonu içeren bir **Raspberry Pi** cihaz şablonu aşağıdaki özelliklere sahip:

### <a name="telemetry-measurements"></a>Telemetri ölçümleri

| Alan adı     | Birimler  | Minimum | Maksimum | Ondalık basamak sayısı |
| -------------- | ------ | ------- | ------- | -------------- |
| Nem oranı       | %      | 0       | 100     | 0              |
| Temp           | °C     | -40     | 120     | 0              |
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

| Type            | Görünen ad | Alan adı | Veri türü |
| --------------- | ------------ | ---------- | --------- |
| Cihaz özelliği | Sayı öldürmüş   | dieNumber  | number    |
| Metin            | Konum     | location   | Yok       |

## <a name="next-steps"></a>Sonraki adımlar

Raspberry Pi'yi, Azure IOT Central uygulamasına bağlanmak öğrendiniz, önerilen sonraki adıma öğrenmektir nasıl [bir özel cihaz şablonu ayarlama](howto-set-up-template.md) kendi IOT cihazını için.
