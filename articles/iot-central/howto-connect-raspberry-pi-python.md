---
title: Raspberry Pi'yi Azure IOT Central uygulamanızı (Python) bağlayın | Microsoft Docs
description: Bir cihaz geliştirici olarak Python kullanarak Azure IOT Central uygulamanızı Raspberry Pi'yi bağlanma.
author: dominicbetts
ms.author: dobett
ms.date: 01/23/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 9f39832b50ed983e7d8a0bfc0a06366870717fa3
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54051994"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Raspberry Pi'yi bağlanmak, Azure IOT Central uygulamasına (Python)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Bu makalede, Raspberry Pi'yi Python programlama dili kullanarak, Microsoft Azure IOT Central uygulamasına bağlanmak için bir cihaz geliştirici olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için aşağıdaki bileşenleri gerekir:

* Oluşturulan bir Azure IOT Central uygulamasına **örnek Devkits** uygulama şablonu. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
* Raspbian işletim sistemi çalıştıran bir Raspberry Pi cihaz. Monitör, klavye ve fare Raspberry Pi'yi bağlı GUI ortama erişmek için gereksinim duyduğunuz. Raspberry Pi şunları [İnternet'e](https://www.raspberrypi.org/learning/software-guide/wifi/).
* İsteğe bağlı olarak, bir [algılama Hat](https://www.raspberrypi.org/products/sense-hat/) Raspberry Pi eklenti Panosu. Bu pano, Azure IOT Central uygulamasına göndermek için çeşitli sensörlerden alınan telemetri verileri toplar. Yoksa bir **algılama Hat** Panosu, bunun yerine bir öykünücü kullanabilirsiniz (kullanılabilir Raspberry Pi görüntünün parçası olarak).

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

Cihaz şablon yapılandırmasının tam ayrıntılarını başvurmak [Raspberry PI cihaz şablonu ayrıntıları](howto-connect-raspberry-pi-python.md#raspberry-pi-device-template-details)
    

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IOT Central uygulamanızda gerçek bir CİHAZDAN ekleme **Raspberry Pi** cihaz bağlantı ayrıntılarının cihaz şablonu ve İzle (**kapsam kimliği, cihaz kimliği, birincil anahtar**). Daha fazla bilgi için [Azure IOT Central uygulamanıza gerçek bir cihaz eklemek](tutorial-add-device.md).


### <a name="configure-the-raspberry-pi"></a>Raspberry Pi yapılandırın

Aşağıdaki adımlar, indirme ve örnek Python uygulamasını github'dan yapılandırma açıklanmaktadır. Bu örnek uygulama:

* Telemetri ve özellik değerleri, Azure IOT Central uygulamasına gönderir.
* Azure IOT Central yapılan değişiklikleri ayarını yanıtlar.

Cihaz yapılandırma [GitHub üzerinde adım adım yönergeleri izleyin.](https://aka.ms/iotcentral-docs-Raspi-releases)


> [!NOTE]
> Raspberry Pi Python örneği hakkında daha fazla bilgi için bkz: [Benioku](https://aka.ms/iotcentral-docs-Raspi-releases) github'da dosya.


1. Cihaz yapılandırıldıktan sonra Cihazınızı veriler kısa bir süre içinde Azure IOT Central için gönderme başlamanız gerekir.
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
