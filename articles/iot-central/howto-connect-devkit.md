---
title: Azure IOT Central uygulamanızı DevKit cihaz bağlayın | Microsoft Docs
description: Bir cihaz geliştirici olarak, Azure IOT Central uygulamanıza MXChip IOT DevKit cihaz bağlanmayı öğreneceksiniz.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: ea9ff8f93ede3b9ec5e7eed83c6049b0c23de7e8
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205468"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Azure IOT Central uygulamanıza bir MXChip IOT DevKit cihazı bağlayın

Bu makalede, Microsoft Azure IOT Central uygulamanıza MXChip IOT DevKit (DevKit) cihaz bağlayamama ek olarak, cihaz geliştirici olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

1. Oluşturulan bir Azure IOT Central uygulamasına **örnek Devkits** uygulama şablonu. Daha fazla bilgi için [Azure IOT Central uygulaması oluşturmayı](howto-create-application.md).
1. Bir DevKit cihaz. DevKit cihaz satın almak için ziyaret [MXChip IOT DevKit](http://mxchip.com/az3166).


## <a name="sample-devkits-application"></a>**Örnek Devkits** uygulama

Oluşturulan uygulama **örnek Devkits** uygulama şablonu içeren bir **MXChip** cihaz şablonu aşağıdaki özelliklere sahip: 

- Cihaz için ölçüler içeren telemetri **nem**, **sıcaklık**, **baskısı**, **Magnometer** (X ölçülür. Y, Z ekseni), **Accelorometer** (ölçülen X, Y, Z ekseni) ve **jiroskop** (X, Y ölçülür Z ekseni).
- Bir örnek ölçüm için içeren durumu **cihaz durumu**.
- Olay ölçümü ile bir **B düğmesi basılı** olay. 
- Ayarları gösteren **voltaj**, **geçerli**, **fanı hızı**ve bir **IR** Aç/Kapat.
- Cihaz özelliği içeren özellik **sayı öldürmüş** ve **cihaz konumu** olarak de location özelliği olan bir **üretilen içinde** bulut özelliği. 


Yapılandırması hakkında tam Ayrıntılar için bkz [MXChip cihaz şablonu ayrıntıları](howto-connect-devkit.md#mxchip-device-template-details)


## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IOT Central uygulamanızda gerçek bir CİHAZDAN ekleme **MXChip** cihaz şablonu ve cihaz bağlantı dizesini Not olun. Daha fazla bilgi için [Azure IOT Central uygulamanıza gerçek bir cihaz eklemek](tutorial-add-device.md).

### <a name="prepare-the-devkit-device"></a>DevKit cihazı hazırlama

> [!NOTE]
> Cihaz daha önce kullandınız ve kimlik bilgileri depolanır ve farklı bir WiFi ağına, bağlantı dizesi veya telemetri ölçüm kullanacak şekilde cihazı yeniden yapılandırmak istediğiniz Wi-Fi varsa, her ikisi de basın **A** ve **B** Pano üzerinde aynı anda düğmeler. Bu işe yaramazsa, basın **sıfırlama** düğmesine tıklayın ve yeniden deneyin.

#### <a name="before-you-start-configuring-the-device"></a>Cihaz yapılandırma başlamadan önce:
1. İçinde IOT Central **örnek Devkits** Git `Device Explorer` ->  `select MXChip Template`  ->  `Click on +New and choose **Real** Device`  ->  `Connect this device` (sağ üst kısımdaki) 
2. Birincil bağlantı dizesini kopyalayın
3. Bağlantı dizesi kaydettiğinizden emin olun DevKit cihazı hazırlama gibi gibi temporaritly internet'ten bağlantıları kesilir. 


#### <a name="to-prepare-the-devkit-device"></a>DevKit cihazı hazırlamak için:


1. MXChip için önceden oluşturulmuş en son Azure IOT Central bellenim indirme [sürümleri](https://github.com/Azure/iot-central-firmware/releases) GitHub sayfasında. Sürümler sayfasından indirme dosya benzer `AZ3166-IoT-Central-X.X.X.bin`.

1. DevKit cihazı bir USB kablosu kullanarak, geliştirme makinenize bağlayın. Windows içinde DevKit cihazdaki depolama alanına eşlenmiş sürücüsünde bir dosya Gezgini penceresi açılır. Örneğin, sürücü olarak adlandırılabilir **AZ3166 (D:)**.

1. Sürükleme **iotCentral.bin** sürücü pencerenin üzerine dosya. Kopyalama tamamlandığında, cihazı yeni bellenim ile yeniden başlatır.

1. DevKit cihaz yeniden başlatıldığında, aşağıdaki ekranda görüntüler:

    ```
    Connect HotSpot:
    AZ3166_??????
    go-> 192.168.0.1 
    PIN CODE xxxxx
    ```

    > [!NOTE]
    > Başka bir şey ekran görüntüleri, basın **A** ve **B** cihazı yeniden başlatmak için aynı anda cihazda düğme. 

1. Cihaz erişim noktası (AP) modunda sunulmuştur. Bu Wi-Fi erişim noktasına, bilgisayar veya mobil CİHAZDAN bağlanabilirsiniz.

1. Cihazın ekranda gösterilen Wi-Fi ağ adı için bilgisayar, telefon veya tablet bağlanın. Bu ağa bağlanmak, internet erişimi yok. Bu durum beklenir ve cihaz yapılandırırken yalnızca kısa bir süre için bu ağa bağlıyken.

1. Web tarayıcınızı açın ve gidin [ http://192.168.0.1/start ](http://192.168.0.1/start). Aşağıdaki web sayfasında görüntüler:

    ![Cihaz yapılandırma sayfası](media/howto-connect-devkit/configpage.png)

    Web sayfasında: 
    - Wi-Fi ağınıza adını ekleyin 
    - Wi-Fi ağ parolanızı
    - PIN LCD cihazda gösterilen kodu 
    - bağlantı dizesi (, zaten kaydettiğiniz bu adımları izleyerek) cihazınızın zamanında bağlantı dizenizi bulabilirsiniz `https://apps.iotcentral.com` -> `Device Explorer` -> `Device` -> `Select or Create a new Real Device` -> `Connect this device` (sağ üst kısımdaki)
    - Tüm mevcut telemetri ölçümleri seçin! 

1. Seçtiğiniz sonra **cihazı yapılandırma**, bu sayfayı görürsünüz:

    ![Yapılandırılmış cihaz](media/howto-connect-devkit/deviceconfigured.png)

1. Tuşuna **sıfırlama** Cihazınızda düğmesi.



## <a name="view-the-telemetry"></a>Telemetri görüntüleme

DevKit cihaz yeniden başlatıldığında, cihaz ekranı gösterilmektedir:

* Gönderilen telemetri iletilerini sayısı.
* Başarısızlık sayısı.
* İstenen özellik alınan sayısı ve gönderilen bildirilen özellikler sayısı.

Cihaz artırma gönderilen bildirilen özellikler sayısını sallayın. Rastgele bir sayı olarak cihazın gönderdiği **sayı öldürmüş** cihaz özelliği.

Bildirilen özellik değerleri ve telemetri ölçümleri görüntüleyebilir ve ayarlarını Azure IOT Central yapılandırma:

1. Kullanım **Device Explorer** gitmek için **ölçümleri** eklediğiniz gerçek MXChip cihaz sayfası:

    ![Gerçek cihaza gidin](media/howto-connect-devkit/realdevicenew.png)

1. Üzerinde **ölçümleri** sayfasında MXChip CİHAZDAN gelen telemetriyi görebilirsiniz:

    ![Gerçek bir CİHAZDAN telemetri görüntüleme](media/howto-connect-devkit/devicetelemetrynew.png)

1. Üzerinde **özellikleri** sayfasında son zar numarasını ve cihaz tarafından raporlanan cihaz konumu görüntüleyebilirsiniz:

    ![Cihaz özelliklerini görüntüleme](media/howto-connect-devkit/devicepropertynew.png)

1. Üzerinde **ayarları** sayfasında, ayarları MXChip cihazda güncelleştirebilirsiniz:

    ![Cihaz ayarlarını görüntüle](media/howto-connect-devkit/devicesettingsnew.png)

1. Üzerinde **Pano** sayfası, harita konumu görebilirsiniz

    ![Cihaz panoyu görüntüle](media/howto-connect-devkit/devicedashboardnew.png)


## <a name="download-the-source-code"></a>Kaynak kodunu indirebilir

Keşfedin ve cihaz kodunu değiştirmek istiyorsanız, Github'dan indirin. Kodu değiştirin planlıyorsanız, bu yönergeleri izlemelidir [geliştirme ortamınızı hazırlama](https://microsoft.github.io/azure-iot-developer-kit/docs/get-started/#step-5-prepare-the-development-environment) masaüstü işletim sisteminiz için.

Kaynak kodu indirmek için Masaüstü makinenizde aşağıdaki komutu çalıştırın:

```cmd/sh
git clone https://github.com/Azure/iot-central-firmware
```

Önceki komut kaynak kodu olarak adlandırılan bir klasöre indirir. `iot-central-firmware`. 

> [!NOTE]
> Varsa **git** yüklü geliştirme ortamınızda buradan indirebilirsiniz [ https://git-scm.com/download ](https://git-scm.com/download).

## <a name="review-the-code"></a>Kodu gözden geçirin

Açmak için geliştirme ortamınızı hazırladığınız bağlandığınızda yüklendi ve Visual Studio Code'u, `AZ3166` klasöründe `iot-central-firmware` klasörü: 

![Visual Studio Code](media/howto-connect-devkit/vscodeview.png)

Telemetri için Azure IOT Central uygulamasına nasıl gönderileceğini görmek için **main_telemetry.cpp** kaynak klasördeki bir dosya.

İşlev `buildTelemetryPayload` sensörlerden alınan verilerin cihazda kullanarak JSON telemetri yükü oluşturur.

İşlev `sendTelemetryPayload` çağrıları `sendTelemetry` içinde **iotHubClient.cpp** JSON yükü, Azure IOT Central, uygulamanız tarafından kullanılan IOT Hub'ına göndermek için.

Özellik değerleri için Azure IOT Central uygulamasına nasıl bildirildiğini görmek için **main_telemetry.cpp** kaynak klasördeki bir dosya.

İşlev `telemetryLoop` gönderir **doubleTap** ivme ölçer, bir çift dokunarak algıladığında özelliği bildirdi. Kullandığı `sendReportedProperty` işlevi **iotHubClient.cpp** kaynak dosyası.

Kodda **iotHubClient.cpp** kaynak dosyası kullanan işlevlerden [ Microsoft Azure IOT SDK'ları ve kitaplıkları c](https://github.com/Azure/azure-iot-sdk-c) IOT hub'ı ile etkileşim kurmak için.

Değiştirmek için derleme ve örnek kod, cihazınıza karşıya yükleme hakkında daha fazla bilgi için bkz. **readme.md** dosyası `AZ3166` klasör.

## <a name="mxchip-device-template-details"></a>MXChip cihaz şablonu ayrıntıları 

Örnek Devkits uygulama şablondan oluşturulan bir uygulama aşağıdaki özelliklere sahip bir MXChip cihaz şablonu içerir:

### <a name="measurements"></a>Ölçümler

#### <a name="telemetry"></a>Telemetri 

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


#### <a name="states"></a>Durumlar 
| Ad          | Görünen ad   | NORMAL | UYARI | TEHLİKE | 
| ------------- | -------------- | ------ | ------- | ------ | 
| DeviceState   | Cihaz durumu   | Yeşil  | Orange  | Kırmızı    | 

#### <a name="events"></a>Olaylar 
| Ad             | Görünen ad      | 
| ---------------- | ----------------- | 
| ButtonBPressed   | Basılan düğme B  | 

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
| Cihaz özelliği | Cihaz konumu   | location  | location    |
| Metin            | İçinde üretilen     | manufacturedIn   | Yok       |



## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central uygulamanıza DevKit cihaz bağlayamama öğrendiniz, önerilen sonraki adımlar şunlardır:

* [Raspberry Pi'yi hazırlama ve bağlama](howto-connect-raspberry-pi-python.md)
