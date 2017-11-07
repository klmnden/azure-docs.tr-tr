---
title: "Dinamik telemetri kullanın | Microsoft Docs"
description: "Azure IOT paketi Uzaktan izleme çözümü dinamik telemetri kullanmayı öğrenmek için bu öğreticiyi izleyin."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 60e9ee00fabf15a62e782c70bca251b1a8e617c3
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Önceden yapılandırılmış Uzaktan izleme çözümü ile dinamik telemetri kullanın

Dinamik telemetri için Uzaktan izleme önceden yapılandırılmış çözümü gönderilen telemetri görselleştirmek etkinleştirir. Önceden yapılandırılmış çözümü ile dağıttığınız sanal cihazlar Panoda görselleştirebilirsiniz sıcaklık ve nem telemetrisi gönderin. Varolan sanal cihazlar özelleştirme, yeni sanal cihaz oluşturma ya da fiziksel cihazları önceden yapılandırılmış çözümü arasında bağlantı dış sıcaklığı, RPM veya windspeed gibi diğer telemetri değerleri gönderebilirsiniz. Ardından, bu ek telemetri Panoda görselleştirebilirsiniz.

Bu öğretici dinamik telemetri ile denemek için kolaylıkla değiştirebileceğiniz basit Node.js sanal bir cihaz kullanır.

Bu öğreticiyi tamamlamak için ihtiyacınız vardır:

* Etkin bir Azure aboneliği. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk_free_trial].
* [Node.js] [ lnk-node] sürüm 0.12.x sürümü veya sonraki bir sürümü.

Bu öğretici Windows veya Linux, Node.js yükleyebileceğiniz gibi herhangi bir işletim sistemini tamamlayabilirsiniz.

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-v1-send-external-temperature](../../includes/iot-suite-v1-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a>Telemetri türü ekleme

Sonraki adım, yeni bir değer kümesiyle Node.js sanal cihaz tarafından oluşturulan telemetri değiştirmektir:

1. Node.js sanal cihaz yazmayı **Ctrl + C** komut istemi veya kabuk.
2. Remote_monitoring.js dosyasında varolan sıcaklık, nem ve dış sıcaklığı telemetri temel veri değerlerini görebilirsiniz. İçin bir taban veri değer Ekle **rpm** gibi:

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Node.js benzetimli aygıt kullanır **generateRandomIncrement** temel veri değerleri için rastgele bir artış eklemek için remote_monitoring.js dosyasındaki işlevi. Rasgele yap **rpm** sonra varolan randomizations şu şekilde bir kod satırı ekleyerek değeri:

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Cihaz IOT Hub'ına gönderir JSON yükü yeni rpm değerin ekleyin:

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

6. Pano grafiği görüntüler yeni RPM telemetri türü inceleyin:

![Panoya RPM ekleyin][image3]

> [!NOTE]
> Devre dışı bırakın ve Node.js cihaz ardından etkinleştirmek gerekebilir **aygıtları** değişikliği hemen görmek için Pano sayfası.

## <a name="customize-the-dashboard-display"></a>Pano ekranını özelleştirme

**Device-Info** ileti, cihaz IOT Hub'ına gönderebilirsiniz telemetri hakkındaki meta verileri içerebilir. Bu meta veriler cihazın gönderdiği telemetri türlerini belirtebilirsiniz. Değiştirme **deviceMetaData** remote_monitoring.js dosyasını içerecek şekilde değerinde bir **Telemetri** tanımı aşağıdaki **komutları** tanımı. Aşağıdaki kod parçacığında gösterildiği **komutları** tanımı (eklediğinizden emin olun bir `,` sonra **komutları** tanımı):

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
> Uzaktan izleme çözümü küçük harf olarak eşleşen telemetri akışına verileriyle meta veri açıklamasında karşılaştırmak için kullanır.


Ekleme bir **Telemetri** Yukarıdaki kod parçacığında gösterildiği gibi tanımı Pano davranışını değiştirmez. Meta verileri de içerir ancak, bir **DisplayName** panosunda görüntüsünü özelleştirmek için öznitelik. Güncelleştirme **Telemetri** aşağıdaki kod parçacığında gösterildiği gibi meta veri tanımı:

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

Aşağıdaki ekran görüntüsü, bu değişikliği Panoda grafik gösterge nasıl değiştirdiğini gösterir:

![Grafik göstergesini özelleştirme][image4]

> [!NOTE]
> Devre dışı bırakın ve Node.js cihaz ardından etkinleştirmek gerekebilir **aygıtları** değişikliği hemen görmek için Pano sayfası.

## <a name="filter-the-telemetry-types"></a>Telemetri türlerini filtre

Varsayılan olarak, grafik Panoda telemetri akışta her veri serisi gösterir. Kullanabileceğiniz **Device-Info** belirli telemetri türlerini grafik görüntüsünü gizlemek için meta verileri. 

Yalnızca sıcaklık ve nem telemetrisi Göster grafik yapmak için atlayın **ExternalTemperature** gelen **Device-Info** **Telemetri** aşağıdaki gibi meta veriler:

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

**Dış sıcaklığı** artık grafik görüntüler:

![Panoda telemetri filtreleme][image5]

Bu değişiklik, yalnızca grafik görüntüleme etkiler. **ExternalTemperature** veri değerleri hala depolanır ve tüm arka uç işleme için kullanılabilir.

> [!NOTE]
> Devre dışı bırakın ve Node.js cihaz ardından etkinleştirmek gerekebilir **aygıtları** değişikliği hemen görmek için Pano sayfası.

## <a name="handle-errors"></a>Hata işleme

Grafikte görüntülenecek veri akışı için kendi **türü** içinde **Device-Info** meta verileri, telemetri değerleri veri türüyle eşleşmelidir. Meta veriler belirtiyorsa, örneğin, **türü** nem veridir **int** ve **çift** nem telemetrisi grafikte görüntülemez sonra telemetri akışında bulunamadı. Ancak, **nem** değerleri hala depolanır ve tüm arka uç işleme için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Dinamik telemetri kullanmayı gördünüz, önceden yapılandırılmış çözümler cihaz bilgilerini kullanma hakkında daha fazla bilgi edinebilirsiniz: [Uzaktan izleme aygıt bilgileri meta verileri önceden yapılandırılmış çözüm][lnk-devinfo].

[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[image3]: media/iot-suite-v1-dynamic-telemetry/image3.png
[image4]: media/iot-suite-v1-dynamic-telemetry/image4.png
[image5]: media/iot-suite-v1-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
