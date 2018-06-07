---
title: Azure IOT merkezi DevKit aygıt bağlamak | Microsoft Docs
description: Cihaz geliştiricisi olarak, Azure IOT merkezi uygulamanıza MXChip IOT DevKit aygıt bağlayacağınızı öğrenin.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: af5cfc2f598893328bc8d4acc979f6d777114f99
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628802"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Azure IOT merkezi uygulamanıza bir MXChip IOT DevKit cihazı bağlayın

Bu makalede, Microsoft Azure IOT merkezi uygulamanıza bir MXChip IOT DevKit (DevKit) aygıtı bağlamak için bir aygıt geliştiricisi olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

1. Oluşturulan Azure IOT Merkezi uygulama **örnek Devkits** uygulama şablonu. Daha fazla bilgi için bkz: [Azure IOT merkezi uygulamanızı oluşturma](howto-create-application.md).
1. DevKit aygıt. DevKit aygıt satın almak için ziyaret [MXChip IOT DevKit](http://mxchip.com/az3166).

Oluşturulan bir uygulamayı **örnek Devkits** uygulama şablonu içeren bir **MXChip** cihaz şablonu aşağıdaki özelliklere sahip:

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

### <a name="states"></a>durumları 

| Ad          | Görünen ad   | NORMAL | UYARI | TEHLİKE | 
| ------------- | -------------- | ------ | ------- | ------ | 
| DeviceState   | Cihaz durumu   | Yeşil  | Orange  | Kırmızı    | 

### <a name="events"></a>Olaylar 

| Ad             | Görünen ad      | 
| ---------------- | ----------------- | 
| ButtonBPressed   | Basılan düğme B  | 

### <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IOT merkezi uygulamanızda gerçek bir aygıttan ekleyin **MXChip** cihaz şablonu ve cihaz bağlantı dizesini Not. Daha fazla bilgi için bkz: [gerçek bir cihazı Azure IOT merkezi uygulamanıza eklemek](tutorial-add-device.md).

## <a name="prepare-the-devkit-device"></a>DevKit cihazı hazırlama

> [!TIP]
> Sorun giderme kılavuzluğu DevKit aygıt için bkz: [IOT DevKit Başlarken](https://microsoft.github.io/azure-iot-developer-kit/docs/get-started/).

DevKit cihazı hazırlamak için:

1. Önceden derlenmiş en son Azure IOT merkezi bellenim MXChip indirin [serbest](https://github.com/Azure/iot-central-firmware/releases) GitHub sayfasında. İndirme filename sürümleri sayfasında benzer `AZ3166-IoT-Central-X.X.X.bin`.

1. DevKit cihazı bir USB kablosu kullanarak geliştirme makinenize bağlayın. Windows'da DevKit cihazdaki depolama alanına eşlenen bir sürücüde dosya Gezgini penceresi açılır. Örneğin, sürücü olarak adlandırılabilir **AZ3166 (D:)**.

1. Sürükleme **iotCentral.bin** sürücü penceresi üzerine dosya. Kopyalama tamamlandıktan sonra aygıtın yeni yazılımıyla yeniden başlatır.

1. DevKit aygıt yeniden başlatıldığında, aşağıdaki ekran görüntüler:

    ```
    Connect HotSpot:
    AZ3166_??????
    go-> 192.168.0.1 
    PIN CODE xxxxx
    ```

    > [!NOTE]
    > Ekran başka bir şey gösterirse, basın **sıfırlama** cihazın düğmesini. 

1. Aygıt erişim noktası (AP) modunda sunulmuştur. Bilgisayar ya da mobil cihaz bu WiFi erişim noktasına bağlanabilir.

1. Bilgisayarınızı, telefon veya tablet cihaza ekranda gösterilen WiFi ağ adı bağlayın. Bu ağa bağlandığında, internet erişimi yoktur. Bu durum beklenen bir durumdur ve cihaz yapılandırması sırasında yalnızca kısa bir süre için bu ağa bağlı.

1. Web tarayıcınızı açın ve gidin [ http://192.168.0.1/start ](http://192.168.0.1/start). Aşağıdaki web sayfasında görüntüler:

    ![Aygıt yapılandırma sayfası](media/howto-connect-devkit/configpage.png)

    Web sayfasında: 
    - WiFi ağınızın adı ekleyin 
    - WiFi ağ parolanızı 
    - PIN LCD aygıtta gösterilen kodu 
    - Cihazınızı bağlantı dizesi. 
      Bağlantı dizesi @ bulabilirsiniz `https://apps.iotcentral.com`  ->  `Device Explorer`  ->  `Device`  ->  `Select or Create a new Real Device`  ->  `Connect this device` (üzerinde sağ üstte) 
    - Tüm kullanılabilir telemetri ölçüleri seçin! 

1. Seçtiğiniz sonra **aygıtı Yapılandır**, bu sayfaya bakın:

    ![Yapılandırılan aygıtı](media/howto-connect-devkit/deviceconfigured.png)

1. Tuşuna **sıfırlama** aygıtınızda düğmesi.

> [!NOTE]
> Farklı WiFi ağı, bağlantı dizesi veya telemetri ölçüm kullanmak için aygıt yapılandırmak için her ikisi de basın **A** ve **B** panosunda aynı anda düğmeler. İşe yaramazsa, basın **sıfırlama** düğmesine tıklayın ve yeniden deneyin. 

## <a name="view-the-telemetry"></a>Telemetriyi görüntüleyebilir

DevKit aygıt yeniden başlatıldığında, cihaz ekranda gösterir:

* Gönderilen telemetri iletilerinin sayısı.
* Başarısızlık sayısı.
* Alınan istenen özellikleri sayısı ve gönderilen bildirilen özelliklerin sayısı.

Cihaz artışı gönderilen bildirilen özellikleri sayısı sallama. Rastgele bir sayı olarak cihaz gönderir **numara öldürmüş** cihaz özelliği.

Bildirilen özellik değerlerini telemetri ölçümleri görüntüleyebilir ve ayarlarını Azure IOT merkezi yapılandırın:

1. Kullanım **aygıt Explorer** gitmek için **ölçümleri** eklediğiniz gerçek MXChip cihaz sayfasında:

    ![Gerçek cihaza gidin](media/howto-connect-devkit/realdevice.png)

1. Üzerinde **ölçümleri** sayfasında MXChip aygıttan gelen telemetri görebilirsiniz:

    ![Gerçek cihaz telemetrisinden görünümü](media/howto-connect-devkit/realtelemetry.png)

1. Üzerinde **özellikleri** sayfasında, cihaz tarafından bildirilen son numara görüntüleyebilirsiniz:

    ![Cihaz özellikleri görüntüle](media/howto-connect-devkit/deviceproperties.png)

1. Üzerinde **ayarları** sayfasında, ayarları MXChip aygıtta güncelleştirebilirsiniz:

    ![Cihaz ayarlarını görüntüle](media/howto-connect-devkit/settings.png)

## <a name="download-the-source-code"></a>Kaynak kodu indirme

Keşfetmek ve aygıt kodunu değiştirmek istiyorsanız, Github'dan indirin. Kod değiştirmenin planlıyorsanız, bu yönergeleri izlemeniz gereken [geliştirme ortamını hazırlayın](https://microsoft.github.io/azure-iot-developer-kit/docs/get-started/#step-5-prepare-the-development-environment) masaüstü işletim sistemi için.

Kaynak kodu indirmek için Masaüstü makinenizde aşağıdaki komutu çalıştırın:

```cmd/sh
git clone https://github.com/Azure/iot-central-firmware
```

Önceki komutu kaynak kodunu adlı bir klasöre indirir `iot-central-firmware`. 

> [!NOTE]
> Varsa **git** yüklü değil, geliştirme ortamınızda buradan indirebilirsiniz [ https://git-scm.com/download ](https://git-scm.com/download).

## <a name="review-the-code"></a>Kodu gözden geçirin

Visual Studio açmak için geliştirme ortamınızı hazırlandığınızda yüklendiği kod kullanmak `AZ3166` klasöründe `iot-central-firmware` klasörü: 

![Visual Studio Code](media/howto-connect-devkit/vscodeview.png)

Telemetri Azure IOT merkezi uygulamaya nasıl gönderileceğini görmek için açın **main_telemetry.cpp** kaynak klasördeki dosya.

İşlev `buildTelemetryPayload` cihazda algılayıcı verilerini kullanarak JSON telemetri yükü oluşturur.

İşlev `sendTelemetryPayload` çağrıları `sendTelemetry` içinde **iotHubClient.cpp** JSON yükü, Azure IOT merkezi uygulamanız tarafından kullanılan IOT Hub'ına göndermek için.

Özellik değerlerini Azure IOT merkezi uygulamaya nasıl bildirilir görmek için açın **main_telemetry.cpp** kaynak klasördeki dosya.

İşlev `telemetryLoop` gönderir **doubleTap** accelerometer çift dokunun algıladığında özelliği bildirdi. Kullandığı `sendReportedProperty` işlevi **iotHubClient.cpp** kaynak dosyası.

Kodda **iotHubClient.cpp** kaynak dosyasını kullanan işlevlerden [ Microsoft Azure IOT SDK'ları ve kitaplıklarını c](https://github.com/Azure/azure-iot-sdk-c) IOT Hub ile etkileşim kurmak için.

Değiştirme, yapı ve örnek kod aygıtınıza karşıya yükleme hakkında daha fazla bilgi için bkz: **readme.md** dosyasını `AZ3166` klasör.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT merkezi uygulamanıza DevKit aygıta bağlanmayı öğrendiniz, önerilen sonraki adımlar şunlardır:

* [Raspberry Pi'yi hazırlama ve bağlama](howto-connect-raspberry-pi-python.md)