---
title: Dinamik telemetri kullanma | Microsoft Docs
description: Azure IOT paketi Uzaktan izleme çözümü dinamik telemetri kullanma hakkında bilgi edinmek için bu öğreticiden yararlanın.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 60e9ee00fabf15a62e782c70bca251b1a8e617c3
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47096625"
---
# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Uzaktan izleme önceden yapılandırılmış çözümüyle dinamik telemetri kullanma

Dinamik telemetri Uzaktan izleme önceden yapılandırılmış çözüme gönderilen tüm telemetri görselleştirmenize olanak sağlar. Önceden yapılandırılmış çözümü ile dağıttığınız sanal cihazlar Panoda görselleştirebilirsiniz sıcaklık ve nem telemetrisi gönderin. Mevcut sanal cihazlar özelleştirme, yeni bir sanal cihaz oluşturma veya önceden yapılandırılmış çözüme fiziksel cihazları bağlama, dış ortam sıcaklığı, RPM veya windspeed gibi diğer telemetri değerleri gönderebilirsiniz. Ardından, bu ek telemetri Panoda görselleştirebilirsiniz.

Bu öğreticide, kolayca dinamik telemetri ile denemek için değiştirebileceğiniz bir basit Node.js sanal cihaz kullanılır.

Bu öğreticiyi tamamlamak için ihtiyacınız olacak:

* Etkin bir Azure aboneliği. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk_free_trial].
* [Node.js] [ lnk-node] sürüm 0.12.x sürümü veya üzeri.

Windows veya Linux Node.js yükleyebileceği gibi herhangi bir işletim sisteminde, bu öğreticiyi tamamlayabilirsiniz.

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-v1-send-external-temperature](../../includes/iot-suite-v1-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a>Telemetri türü Ekle

Sonraki adım, yeni bir dizi ile Node.js sanal cihaz tarafından üretilen telemetri değiştirmektir:

1. Node.js sanal cihaz yazmayı **Ctrl + C** komut istemi veya kabuk.
2. Remote_monitoring.js dosyasında mevcut sıcaklık, nem ve dış ortam sıcaklığı telemetri için temel veri değerleri görebilirsiniz. Eklemek için bir temel veri değer **rpm** gibi:

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Node.js sanal cihaz kullanan **generateRandomIncrement** remote_monitoring.js dosyasında rastgele bir artış için temel veri değerleri eklemek için işlevi. Rastgele yap **rpm** kod satırının sonra mevcut randomizations gibi ekleyerek değeri:

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. IOT Hub'ına CİHAZDAN gönderilen JSON yükü yeni rpm değeri ekleyin:

    ```nodejs
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Aşağıdaki komutu kullanarak Node.js sanal cihaz çalıştırın:

    `node remote_monitoring.js`

6. Grafiği Panoda görüntüleyen yeni RPM telemetri türü inceleyin:

![RPM panoya ekleme][image3]

> [!NOTE]
> Devre dışı bırakın ve sonra üzerinde Node.js cihaz etkinleştirmeniz gerekebilir **cihazları** değişikliği hemen görmek için Pano sayfasında.

## <a name="customize-the-dashboard-display"></a>Pano ekranını özelleştirme

**Device-Info** ileti, cihaz IOT Hub'ına gönderebilirsiniz telemetri hakkındaki meta verileri içerebilir. Bu meta veriler, cihazın gönderdiği telemetri türlerini belirtebilirsiniz. Değiştirme **deviceMetaData** içerecek şekilde remote_monitoring.js dosyasındaki bir **Telemetri** tanımı aşağıdaki **komutları** tanımı. Aşağıdaki kod parçacığı gösterir **komutları** tanımı (eklediğinizden emin olun bir `,` sonra **komutları** tanımı):

```nodejs
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [!NOTE]
> Uzaktan izleme çözümü harf olarak eşleşen meta verisi tanımı telemetri akış verileri ile karşılaştırmak için kullanır.


Ekleme bir **Telemetri** önceki kod parçacığında gösterildiği gibi tanım Pano davranışını değiştirmez. Ancak, meta verileri de içerebilir bir **DisplayName** panosunda görüntüsünü özelleştirmek için özniteliği. Güncelleştirme **Telemetri** aşağıdaki kod parçacığında gösterildiği gibi meta verisi tanımı:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

Bu değişiklik panoya Grafik göstergesi nasıl değiştirdiğini aşağıdaki ekran görüntüsünde gösterilmektedir:

![Grafik göstergesini özelleştirme][image4]

> [!NOTE]
> Devre dışı bırakın ve sonra üzerinde Node.js cihaz etkinleştirmeniz gerekebilir **cihazları** değişikliği hemen görmek için Pano sayfasında.

## <a name="filter-the-telemetry-types"></a>Telemetri türlerini Filtrele

Varsayılan olarak, Pano grafiği, her veri serisi telemetrisi akışını gösterir. Kullanabileceğiniz **Device-Info** bastır grafikteki belirli telemetri türleri için meta verileri. 

Grafik yalnızca sıcaklık ve nem telemetrisi yapmak atlamak **ExternalTemperature** gelen **Device-Info** **Telemetri** aşağıdaki gibi meta veriler:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

**Dış sıcaklığı** artık grafiği görüntüler:

![Panoda telemetri filtreleme][image5]

Bu değişiklik yalnızca grafik görüntüleme etkiler. **ExternalTemperature** veri değerleri yine de depolanan ve herhangi bir arka uç işleme için kullanılabilir.

> [!NOTE]
> Devre dışı bırakın ve sonra üzerinde Node.js cihaz etkinleştirmeniz gerekebilir **cihazları** değişikliği hemen görmek için Pano sayfasında.

## <a name="handle-errors"></a>Hatalarını işleme

Grafikte görüntülenecek bir veri akışı kendi **türü** içinde **Device-Info** meta verileri, telemetri değerleri veri türüyle eşleşmelidir. Örneğin, meta verileri belirtir **türü** nem verilerdir **int** ve **çift** nem telemetrisini aşağıdaki dotnetclıtools'u görüntülemiyor sonra telemetri akışta bulunamadı Grafik üzerinde. Ancak, **nem** değerleri yine de depolanan ve herhangi bir arka uç işleme için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Dinamik telemetri kullanma gördüğünüze göre önceden yapılandırılmış çözümler, cihaz bilgilerini kullanma hakkında daha fazla bilgi edinebilirsiniz: [cihaz bilgileri meta verilerini Uzaktan izleme çözümüne] [ lnk-devinfo].

[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[image3]: media/iot-suite-v1-dynamic-telemetry/image3.png
[image4]: media/iot-suite-v1-dynamic-telemetry/image4.png
[image5]: media/iot-suite-v1-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
