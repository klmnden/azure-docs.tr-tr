---
title: Azure IOT Central uygulamanızı DevKit cihaz bağlayın | Microsoft Docs
description: Bir cihaz geliştirici olarak, Azure IOT Central uygulamanıza MXChip IOT DevKit cihaz bağlanmayı öğreneceksiniz.
author: dominicbetts
ms.author: dobett
ms.date: 03/22/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 82222dd927f46761941a6a750d96222cc626e71b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60887341"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Azure IOT Central uygulamanıza bir MXChip IOT DevKit cihazı bağlayın

Bu makalede, Microsoft Azure IOT Central uygulamanıza MXChip IOT DevKit (DevKit) cihaz bağlayamama ek olarak, cihaz geliştirici olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için aşağıdaki kaynakları gerekir:

1. Oluşturulan bir Azure IOT Central uygulamasına **örnek Devkits** uygulama şablonu. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
1. Bir DevKit cihaz. DevKit cihaz satın almak için ziyaret [MXChip IOT DevKit](https://microsoft.github.io/azure-iot-developer-kit/).

## <a name="sample-devkits-application"></a>Örnek Devkits uygulaması

Oluşturulan uygulama **örnek Devkits** uygulama şablonu içeren bir **MXChip** aşağıdaki cihaz özelliklerini tanımlayan cihaz şablonu:

- Telemetri ölçümleri için **nem**, **sıcaklık**, **baskısı**, **Magnetometer** (ölçülen X, Y, Z ekseni), **İvme ölçer** (X, Y ölçülür Z ekseni), ve **jiroskop** (ölçülen X, Y, Z ekseni).
- Ölçüm için durum **cihaz durumu**.
- Olay ölçümü için **B düğmesi basılı**.
- Ayarları **voltaj**, **geçerli**, **fanı hızı**ve bir **IR** Aç/Kapat.
- Cihaz özellikleri **sayı öldürmüş** ve **cihaz konumu**, location özelliği olduğu.
- Bulut özelliği **üretilen içinde**.
- Komutları **Yankı** ve **geri sayım**. Gerçek bir cihaz aldığında bir **Yankı** komutu, bunu göstermektedir gönderilen değeri cihazın ekranı. Gerçek bir cihaz aldığında bir **geri sayım** komutu, bir desen LED başlatıldıkları ve cihaz geri sayım değerleri IOT Central için geri gönderir.

Yapılandırması hakkında tam Ayrıntılar için bkz [MXChip cihaz şablonu ayrıntıları](#mxchip-device-template-details)

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

### <a name="get-your-device-connection-details"></a>Cihazınızı bağlantı ayrıntılarını Al

Azure IOT Central uygulamanızda gerçek bir CİHAZDAN ekleme **MXChip** cihaz şablonu ve cihaz bağlantı ayrıntılarını not olun: **Kapsam kimliği, cihaz kimliği ve birincil anahtar**:

1. Ekleme bir **gerçek cihaz** Device Explorer seçin **+ yeni > gerçek** gerçek bir cihaz eklemek için.

    * Bir küçük harf girin **cihaz kimliği**, veya önerilen **cihaz kimliği**.
    * Girin bir **cihaz adı**, ya da önerilen adı kullanın

    ![Cihaz Ekleme](media/howto-connect-devkit/add-device.png)

1. Cihaz bağlantı ayrıntılarını almak için **kapsam kimliği**, **cihaz kimliği**, ve **birincil anahtar**seçin **Connect** cihaz sayfasında.

    ![Bağlantı ayrıntıları](media/howto-connect-devkit/device-connect.png)

1. Bağlantı ayrıntılarını not edin. Sonraki adımda DevKit Cihazınızı hazırlarken geçici olarak internet'ten kesilirse.

### <a name="prepare-the-devkit-device"></a>DevKit cihazı hazırlama

Farklı bir WiFi ağına, bağlantı dizesi veya telemetri ölçüm kullanacak şekilde yapılandırmak için cihaz ve isterseniz daha önce kullandınız, her ikisi de basın **A** ve **B** aynı anda düğmeleri. Bu işe yaramazsa, basın **sıfırlama** düğmesine tıklayın ve yeniden deneyin.

#### <a name="to-prepare-the-devkit-device"></a>DevKit cihazı hazırlamak için

1. MXChip için önceden oluşturulmuş en son Azure IOT Central bellenim indirme [sürümleri](https://aka.ms/iotcentral-docs-MXChip-releases) GitHub sayfasında.
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
    > Başka bir şey ekran görüntüleri, tuşuna basın ve cihaz sıfırlama **A** ve **B** cihazı yeniden başlatmak için aynı anda cihazda düğme.

1. Cihaz erişim noktası (AP) modunda sunulmuştur. Bu Wi-Fi erişim noktasına, bilgisayar veya mobil CİHAZDAN bağlanabilirsiniz.

1. Cihazın ekranda gösterilen Wi-Fi ağ adı için bilgisayar, telefon veya tablet bağlanın. Bu ağa bağlanırken internet erişiminiz yok. Bu durum beklenir ve cihaz yapılandırırken yalnızca kısa bir süre için bu ağ bağlandınız.

1. Web tarayıcınızı açın ve gidin [ http://192.168.0.1/start ](http://192.168.0.1/start). Aşağıdaki web sayfasında görüntüler:

    ![Cihaz yapılandırma sayfası](media/howto-connect-devkit/configpage.png)

    Web sayfasında girin:
    - Wi-Fi ağ adı
    - Wi-Fi ağ parolanızı
    - Cihazın ekranda gösterilen PIN kodu
    - Bağlantı ayrıntıları **kapsam kimliği**, **cihaz kimliği**, ve **birincil anahtar** cihazınızın (, zaten kaydettiğiniz bu adımları izleyerek)
    - Tüm mevcut telemetri ölçümleri seçin

1. Seçtiğiniz sonra **cihazı yapılandırma**, bu sayfayı görürsünüz:

    ![Yapılandırılmış cihaz](media/howto-connect-devkit/deviceconfigured.png)

1. Tuşuna **sıfırlama** Cihazınızda düğmesi.

## <a name="view-the-telemetry"></a>Telemetri görüntüleme

DevKit cihaz yeniden başlatıldığında, cihaz ekranı gösterilmektedir:

* Gönderilen telemetri iletilerini sayısı.
* Başarısızlık sayısı.
* İstenen özellik alınan sayısı ve gönderilen bildirilen özellikler sayısı.

> [!NOTE]
> Aygıt bağlanmayı denediğinde döngüsü görünüyorsa, cihaz olup olmadığını kontrol edin **bloke** IOT Central, ve **Engellemeyi Kaldır** uygulamaya bağlanabilmesi için cihaz.

Bildirilen özellik göndermek için cihazı sallayın. Rastgele bir sayı olarak cihazın gönderdiği **sayı öldürmüş** cihaz özelliği.

Bildirilen özellik değerleri ve telemetri ölçümleri görüntüleyebilir ve ayarlarını Azure IOT Central yapılandırma:

1. Kullanım **Device Explorer** gitmek için **ölçümleri** eklediğiniz gerçek MXChip cihaz sayfası:

    ![Gerçek cihaza gidin](media/howto-connect-devkit/realdevicenew.png)

1. Üzerinde **ölçümleri** sayfasında MXChip CİHAZDAN gelen telemetriyi görebilirsiniz:

    ![Gerçek bir CİHAZDAN telemetri görüntüleme](media/howto-connect-devkit/devicetelemetrynew.png)

1. Üzerinde **özellikleri** sayfasında son zar numarasını ve cihaz tarafından raporlanan cihaz konumu görüntüleyebilirsiniz:

    ![Cihaz özelliklerini görüntüleme](media/howto-connect-devkit/devicepropertynew.png)

1. Üzerinde **ayarları** sayfasında, ayarları MXChip cihazda güncelleştirebilirsiniz:

    ![Cihaz ayarlarını görüntüle](media/howto-connect-devkit/devicesettingsnew.png)

1. Üzerinde **komutları** sayfasında çağırabilirsiniz **Yankı** ve **geri sayım** komutları:

    ![Çağrı komutları](media/howto-connect-devkit/devicecommands.png)

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

Açmak için Visual Studio Code'u kullanın `MXCHIP/mxchip_advanced` klasöründe `iot-central-firmware` klasörü:

![Visual Studio Code](media/howto-connect-devkit/vscodeview.png)

Telemetri için Azure IOT Central uygulamasına nasıl gönderileceğini görmek için **telemetry.cpp** dosyası `src` klasörü:

- İşlev `TelemetryController::buildTelemetryPayload` sensörlerden alınan verilerin cihazda kullanarak JSON telemetri yükü oluşturur.

- İşlev `TelemetryController::sendTelemetryPayload` çağrıları `sendTelemetry` içinde **AzureIOTClient.cpp** JSON yükü, Azure IOT Central, uygulamanız tarafından kullanılan IOT Hub'ına göndermek için.

Özellik değerleri için Azure IOT Central uygulamasına nasıl bildirildiğini görmek için **telemetry.cpp** dosyası `src` klasörü:

- İşlev `TelemetryController::loop` gönderir **konumu** özelliği her 30 saniyede yaklaşık bildirdi. Kullandığı `sendReportedProperty` işlevi **AzureIOTClient.cpp** kaynak dosyası.

- İşlev `TelemetryController::loop` gönderir **dieNumber** cihaz ivme ölçer, bir çift dokunarak algıladığında özelliği bildirdi. Kullandığı `sendReportedProperty` işlevi **AzureIOTClient.cpp** kaynak dosyası.

Cihaz IOT Central uygulamadan çağrılan komutları nasıl yanıt verdiğini görmek için **registeredMethodHandlers.cpp** dosyası `src` klasörü:

- **DmEcho** işlevidir işleyicisi **Yankı** komutu. Bu gösterir **displayedValue** cihazın ekranındaki yükteki Dosyalanan.

- **DmCountdown** işlevidir işleyicisi **geri sayım** komutu. Bu cihazın LED rengini değiştirir ve bildirilen özellik geri sayım değeri IOT Central uygulamasına geri göndermek için kullanır. Bildirilen özellik komut aynı ada sahip. İşlev kullanır `sendReportedProperty` işlevi **AzureIOTClient.cpp** kaynak dosyası.

Kodda **AzureIOTClient.cpp** kaynak dosyası kullanan işlevlerden [Microsoft Azure IOT SDK'ları ve kitaplıkları c](https://github.com/Azure/azure-iot-sdk-c) IOT hub'ı ile etkileşim kurmak için.

Değiştirmek için derleme ve örnek kod, cihazınıza karşıya yükleme hakkında daha fazla bilgi için bkz. **readme.md** dosyası `MXCHIP/mxchip_advanced` klasör.

## <a name="mxchip-device-template-details"></a>MXChip cihaz şablonu ayrıntıları

Örnek Devkits uygulama şablondan oluşturulan bir uygulama aşağıdaki özelliklere sahip bir MXChip cihaz şablonu içerir:

### <a name="measurements"></a>Ölçümler

#### <a name="telemetry"></a>Telemetri

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

#### <a name="states"></a>Durumlar 
| Ad          | Görünen ad   | NORMAL | UYARI | DANGER | 
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
| Text            | İçinde üretilen     | manufacturedIn   | Yok       |

### <a name="commands"></a>Komutlar

| Görünen ad | Alan adı | Dönüş türü | Giriş alanının görünen adı | Giriş alan adı | Giriş alanı türü |
| ------------ | ---------- | ----------- | ------------------------ | ---------------- | ---------------- |
| echo         | echo       | metin        | görüntülenecek değer         | displayedValue   | metin             |
| geri sayım    | Geri sayım  | number      | Gelen sayısı               | countFrom        | number           |

## <a name="next-steps"></a>Sonraki adımlar

Raspberry Pi'yi, Azure IOT Central uygulamasına bağlanmak öğrendiniz, önerilen sonraki adıma öğrenmektir nasıl [bir özel cihaz şablonu ayarlama](howto-set-up-template.md) kendi IOT cihazını için.
