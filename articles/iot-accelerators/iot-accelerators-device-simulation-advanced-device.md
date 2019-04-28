---
title: Azure Gelişmiş sanal cihaz modeli için - Oluştur | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda, bir kullanım için Gelişmiş cihaz modeli ile cihaz benzetimi Çözüm Hızlandırıcısı oluşturmayı öğrenin.
author: troyhopwood
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.custom: mvc
ms.date: 03/18/2019
ms.author: troyhop
ms.openlocfilehash: 4401d4b93a27e76554368ce72d256b38de61df4c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61449057"
---
# <a name="create-an-advanced-device-model"></a>Gelişmiş cihaz modeli oluşturma

Bu nasıl yapılır kılavuzunda, bir özel cihaz modeli tanımlayan JSON ve JavaScript dosyalarını açıklar. Makale bazı örnek cihaz modeli tanımı dosyaları içerir ve cihaz benzetimi Örneğinize yüklenecek gösterilmektedir. Testiniz için daha gerçekçi cihaz davranışlarının benzetimini yapmak için Gelişmiş cihaz modelleri oluşturabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır Kılavuzu'ndaki adımları takip etmek için Azure aboneliğinizde bir dağıtılan cihaz benzetimi örneğini gerekir.

Cihaz Simülasyonu'nu henüz dağıtmadıysanız, [Azure'da IoT cihaz simülasyonunu dağıtma ve çalıştırma](quickstart-device-simulation-deploy.md) hızlı başlangıç kılavuzunu tamamlamalısınız.

### <a name="open-device-simulation"></a>Cihaz Simülasyonu'nu açma

Tarayıcınızda Cihaz Simülasyonu'nu çalıştırmak için, önce [Microsoft Azure IoT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com)'na gidin.

Azure aboneliği kimlik bilgilerinizi kullanarak oturum açmanız istenebilir.

Ardından **başlatma** cihaz benzetimi, dağıttığınız kutucuğunda bulunan [dağıtma ve çalıştırma bir Azure IOT cihaz benzetimi](quickstart-device-simulation-deploy.md) hızlı başlangıç.

## <a name="device-models"></a>Cihaz modelleri

Her bir sanal cihaz benzetim davranışını tanımlayan bir belirli cihaz modeline ait. Bu davranış, sık telemetri göndermek nasıl, ne tür gönderilecek ileti ve desteklenen yöntemleri içerir.

Bir JSON cihaz tanımı dosyasını ve JavaScript dosyaları kümesini kullanarak bir cihaz modeli tanımlarsınız. Bu JavaScript dosyaları rastgele telemetri ve yöntemi mantığı gibi benzetim davranışını tanımlayın.

Genel cihaz modeli vardır:

* Her cihaz modeli (örneğin, elevator.json) için bir JSON dosyası.
* Her cihaz modeli (örneğin, Asansör-state.js) için bir JavaScript davranışı betik dosyası
* Her bir cihaz yöntemin bir JavaScript yöntemini betik dosyasını (örneğin, Asansör Git down.js)

> [!NOTE]
> Tüm cihaz modelleri yöntemleri tanımlar. Bu nedenle cihaz modeli olabilir veya yöntemi betikleri olmayabilir. Ancak, tüm cihaz modelleri bir davranış komut dosyası olması gerekir.

## <a name="device-definition-file"></a>Cihaz tanımı dosyası

Her bir cihaz tanımı dosyasını, ayrıntılarını aşağıdaki bilgiler dahil olmak üzere bir sanal cihaz modeli içerir:

* Cihazın model adı: dize.
* Protokol: AMQP | MQTT | HTTP.
* İlk cihaz durumu.
* Genellikle cihaz durumunu yenilemek nasıl.
* Cihaz durumunu yenilemek için kullanılacak hangi JavaScript dosyası.
* Telemetri iletilerini göndermek için belirli bir sıklık ile her bir listesi.
* Arka uç uygulaması tarafından alınan telemetri ayrıştırmak için kullanılan telemetri iletilerini şeması.
* Desteklenen yöntemleri ve her yöntem benzetimini yapmak için kullanılacak JavaScript dosyasını listesi.

### <a name="file-schema"></a>Dosya şeması

Şema sürümü her zaman "1.0.0" ve bu dosya biçimi için geçerlidir:

```json
"SchemaVersion": "1.0.0"
```

### <a name="device-model-description"></a>Cihaz modeli açıklaması

Aşağıdaki özellikleri, cihaz modeli açıklar. Her türünün benzersiz bir tanımlayıcı, semantik sürüm, bir ad ve açıklama vardır:

```json
"Id": "chiller-01",
"Version": "0.0.1",
"Name": "Chiller",
"Description": "Chiller with external temperature and humidity sensors."
```

### <a name="iot-protocol"></a>IOT Protokolü

IOT cihazları, farklı protokoller kullanarak bağlantı kurabilir. Benzetim ya da kullanmanıza olanak tanıyan **AMQP**, **MQTT**, veya **HTTP**:

```json
"Protocol": "AMQP"
```

### <a name="simulated-device-state"></a>Sanal cihaz durumu

Her bir sanal cihaz tanımlanmalıdır bir iç durum vardır. Durumu, telemetriyi de bildirilebilir özellikleri tanımlar. Örneğin, bir Soğutucu başlangıç durumu gibi olabilir:

```json
"InitialState": {
    "temperature": 50,
    "humidity": 40
},
```

Taşıma bir cihazı çeşitli sensör ile daha fazla özellik, örneğin olabilir:

```json
"InitialState": {
    "latitude": 47.445301,
    "longitude": -122.296307,
    "speed": 30.0,
    "speed_unit": "mph",
    "cargotemperature": 38.0,
    "cargotemperature_unit": "F"
}
```

Cihaz durumu, benzetim hizmeti tarafından bellekte tutulur ve JavaScript işlevi için giriş olarak sağlanan. JavaScript işlevinin karar:

* İçin durumu yok saymak ve bazı rastgele veri oluşturun.
* Cihaz durumu, belirli bir senaryo için gerçekçi bir şekilde güncelleştirmek için.

Durumu oluşturan işlevi de giriş olarak alır:

* Cihaz kimliği.
* Cihaz modeli.
* Geçerli saat. Bu değer, zaman ve cihaz tarafından farklı verileri oluşturmak mümkün kılar.

### <a name="generating-telemetry-messages"></a>Telemetri iletilerini oluşturma

Benzetim hizmet, her cihaz için birden fazla telemetri türlerini gönderebilir. Genellikle, telemetri, cihaz durumu verileri içerir. Örneğin, benzetilmiş bir oda sıcaklık ve nem hakkında bilgi 10 saniyede gönderebilir. Cihaz durumu alınan değerlerle otomatik olarak değiştirilir yer tutucular aşağıdaki kod parçacığında dikkat edin:

```json
"Telemetry": [
    {
        "Interval": "00:00:10",
        "MessageTemplate":
            "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\",\"humidity\":\"${humidity}\"}",
        "MessageSchema": {
            "Name": "RoomComfort;v1",
            "Format": "JSON",
            "Fields": {
                "temperature": "double",
                "temperature_unit": "text",
                "humidity": "integer"
            }
        }
    }
],
```

Yer tutucuları kullanan özel bir sözdizimi **${NAME}** burada **adı** JavaScript tarafından döndürülen cihaz durum nesnesinden bir anahtar **ana** işlevi. Sayı karakteri sırada, dizeleri tırnak.

#### <a name="message-schema"></a>İleti şeması

Her ileti türü, iyi tanımlanmış bir şema olması gerekir. Arka uç uygulamalarını gelen telemetriyi yorumlamak için bilgileri yeniden ileti şeması da IOT Hub'ına yayımlanır.

Şema kolay ayrıştırma, dönüştürme ve analiz için çeşitli sistemlerini ve hizmetlerinizde sağlayan JSON biçimini destekler.

Şemada listelenen alanları aşağıdaki türde olabilir:

* JSON'ı kullanarak seri durumdan çıkarılmış nesne-
* İkili - base64 kullanarak seri hale getirilmiş
* Text
* Boolean
* Tamsayı
* Double
* DateTime

### <a name="supported-methods"></a>Desteklenen yöntemler

Sanal cihazlar da yöntem çağrılarını tepki verebilir, durumda bunlar mantığa yürütün ve bazı yanıt sağlayın. Benzer şekilde, benzetim yöntemi mantıksal bir JavaScript dosyasında depolanır ve cihaz durumu ile etkileşim kurabilir. Örneğin:

```json
"CloudToDeviceMethods": {
    "Start": {
        "Type": "JavaScript",
        "Path": "release-pressure.js"
    }
}
```

## <a name="create-a-device-definition-file"></a>Bir cihaz tanımı dosyası oluşturma

Bu Yardım-How-to-Kılavuzu'nda bir insansız hava aracı için bir cihaz modeli oluşturma bakın. İnsansız hava aracı ile rastgele bir başlangıç konumu ve yükseklik koordinatlarını kümesi uç.

Aşağıdaki JSON bir metin düzenleyicisine kopyalayın ve kaydedileceği **drone.json**.

### <a name="device-definition-json-example"></a>Cihaz tanımı JSON örneği

```json
{
  "SchemaVersion": "1.0.0",
  "Id": "drone",
  "Version": "0.0.1",
  "Name": "Drone",
  "Description": "Simple drone.",
  "Protocol": "AMQP",
  "Simulation": {
    "InitialState": {
      "velocity": 0.0,
      "velocity_unit": "mm/sec",
      "acceleration": 0.0,
      "acceleration_unit": "mm/sec^2",
      "latitude": 47.476075,
      "longitude": -122.192026,
      "altitude": 0.0
    },
    "Interval": "00:00:05",
    "Scripts": [{
      "Type": "JavaScript",
      "Path": "drone-state.js"
    }]
  },
  "Properties": {
    "Type": "Drone",
    "Firmware": "1.0",
    "Model": "P-96"
  },
  "Tags": {
    "Owner": "Contoso"
  },
  "Telemetry": [{
      "Interval": "00:00:05",
      "MessageTemplate": "{\"velocity\":\"${velocity}\",\"acceleration\":\"${acceleration}\",\"position\":\"${latitude}|${longitude}|${altitude}\"}",
      "MessageSchema": {
        "Name": "drone-event-sensor;v1",
        "Format": "JSON",
        "Fields": {
          "velocity": "double",
          "velocity_unit": "text",
          "acceleration": "double",
          "acceleration_unit": "text",
          "latitude": "double",
          "longitude": "double",
          "altitude": "double"
        }
      }
    }
  ],
    "CloudToDeviceMethods": {
        "RecallDrone": {
            "Type": "JavaScript",
            "Path": "droneRecall-method.js"
        }
    }
}
```

## <a name="behavior-script-files"></a>Davranış komut dosyaları

Davranış komut dosyası kodu insansız hava aracı ile taşır. Betik, cihazın bellek durumunda işleyerek insansız hava'nın yükseltme ve konumunu değiştirir.

JavaScript dosyaları olmalıdır bir **ana** iki parametre kabul eden bir işlev:

* A **bağlam** üç özellik içeren nesne:
    * **currentTime** biçiminde bir dize olarak **yyyy-aa-dd'T'HH:mm:sszzz**.
    * **DeviceID**. Örneğin, **Simulated.Elevator.123**.
    * **deviceModel**. Örneğin, **Asansör**.
* A **durumu** önceki çağrı işlevi tarafından döndürülen değer nesnesi. Bu cihaz durumu, benzetim hizmeti tarafından tutulan ve telemetri iletilerini oluşturmak için kullanılan.

**Ana** işlevi yeni cihaz durumunu döndürür. Örneğin:

```JavaScript
function main(context, state) {

    // Use context if the simulation depends on
    // time or device details.
    // Execute some logic, updating 'state'

    return state;
}
```

## <a name="create-a-behavior-script-file"></a>Bir davranış betik dosyası oluştur

Bir metin düzenleyicisine şu JavaScript kopyalayın ve kaydedileceği **insansız hava aracı ile state.js**.

### <a name="device-model-javascript-simulation-example"></a>Cihaz model JavaScript benzetimi örneği

```JavaScript
"use strict";

// Position control
const DefaultLatitude = 47.476075;
const DefaultLongitude = -122.192026;
const DistanceVariation = 10;

// Altitude control
const DefaultAltitude = 0.0;
const AverageAltitude = 499.99;
const AltitudeVariation = 5;

// Velocity control
const AverageVelocity = 60.00;
const MinVelocity = 20.00;
const MaxVelocity = 120.00;
const VelocityVariation = 5;

// Acceleration control
const AverageAcceleration = 2.50;
const MinAcceleration = 0.01;
const MaxAcceleration = 9.99;
const AccelerationVariation = 1;

// Display control for position and other attributes
const GeoSpatialPrecision = 6;
const DecimalPrecision = 2;

// Default state
var state = {
    velocity: 0.0,
    velocity_unit: "mm/sec",
    acceleration: 0.0,
    acceleration_unit: "mm/sec^2",
    latitude: DefaultLatitude,
    longitude: DefaultLongitude,
    altitude: DefaultAltitude
};

// Default device properties
var properties = {};

/**
 * Restore the global state using data from the previous iteration.
 *
 * @param previousState device state from the previous iteration
 * @param previousProperties device properties from the previous iteration
 */
function restoreSimulation(previousState, previousProperties) {
    // If the previous state is null, force a default state
    if (previousState) {
        state = previousState;
    } else {
        log("Using default state");
    }

    if (previousProperties) {
        properties = previousProperties;
    } else {
        log("Using default properties");
    }
}

/**
 * Simple formula generating a random value around the average
 * in between min and max
 */
function vary(avg, percentage, min, max) {
    var value = avg * (1 + ((percentage / 100) * (2 * Math.random() - 1)));
    value = Math.max(value, min);
    value = Math.min(value, max);
    return value;
}

/**
 * Entry point function called by the simulation engine.
 * Returns updated simulation state.
 * Device property updates must call updateProperties() to persist.
 *
 * @param context             The context contains current time, device model and id
 * @param previousState       The device state since the last iteration
 * @param previousProperties  The device properties since the last iteration
 */
/*jslint unparam: true*/
function main(context, previousState, previousProperties) {

    // Restore the global state before generating the new telemetry, so that
    // the telemetry can apply changes using the previous function state.
    restoreSimulation(previousState, previousProperties);

    state.acceleration = vary(AverageAcceleration, AccelerationVariation, MinAcceleration, MaxAcceleration).toFixed(DecimalPrecision);
    state.velocity = vary(AverageVelocity, VelocityVariation, MinVelocity, MaxVelocity).toFixed(DecimalPrecision);

    // Use the last coordinates to calculate the next set with a given variation
    var coords = varylocation(Number(state.latitude), Number(state.longitude), DistanceVariation);
    state.latitude = Number(coords.latitude).toFixed(GeoSpatialPrecision);
    state.longitude = Number(coords.longitude).toFixed(GeoSpatialPrecision);

    // Fluctuate altitude between given variation constant by more or less
    state.altitude = vary(AverageAltitude, AltitudeVariation, AverageAltitude - AltitudeVariation, AverageAltitude + AltitudeVariation).toFixed(DecimalPrecision);

    return state;
}

/**
 * Generate a random geolocation at some distance (in miles)
 * from a given location
 */
function varylocation(latitude, longitude, distance) {
    // Convert to meters, use Earth radius, convert to radians
    var radians = (distance * 1609.344 / 6378137) * (180 / Math.PI);
    return {
        latitude: latitude + radians,
        longitude: longitude + radians / Math.cos(latitude * Math.PI / 180)
    };
}
```

## <a name="create-a-method-script-file"></a>Yöntemi komut dosyası oluşturma

Yöntemi komut davranışını betikleri benzerdir. Cihaz yöntemi belirli bir buluta çağrıldığında bunlar cihaz davranışını tanımlayın.

İnsansız hava aracı ile geri çağırma betik giriş döndüren bir insansız hava aracı benzetimini yapmak için sabit bir nokta insansız hava'nın koordinatlarını ayarlar.

Bir metin düzenleyicisine şu JavaScript kopyalayın ve kaydedileceği **droneRecall method.js**.

### <a name="device-model-javascript-simulation-example"></a>Cihaz model JavaScript benzetimi örneği

```JavaScript
"use strict";

// Default state
var state = {
    velocity: 0.0,
    velocity_unit: "mm/sec",
    acceleration: 0.0,
    acceleration_unit: "mm/sec^2",
    latitude: 0.0,
    longitude: 0.0,
    altitude: 0.0
};

// Default device properties
var properties = {};

/**
 * Restore the global state using data from the previous iteration.
 *
 * @param previousState device state from the previous iteration
 * @param previousProperties device properties from the previous iteration
 */
function restoreSimulation(previousState, previousProperties) {
    // If the previous state is null, force a default state
    if (previousState) {
        state = previousState;
    } else {
        log("Using default state");
    }

    if (previousProperties) {
        properties = previousProperties;
    } else {
        log("Using default properties");
    }
}

/**
 * Entry point function called by the simulation engine.
 *
 * @param context        The context contains current time, device model and id, not used
 * @param previousState  The device state since the last iteration, not used
 * @param previousProperties  The device properties since the last iteration
 */
/*jslint unparam: true*/
function main(context, previousState, previousProperties) {

    // Restore the global device properties and the global state before
    // generating the new telemetry, so that the telemetry can apply changes
    // using the previous function state.
    restoreSimulation(previousState, previousProperties);

    //simulate the behavior of a drone when recalled
  state.latitude = 47.476075;
  state.longitude = -122.192026;
  return state;
}
```

## <a name="debugging-script-files"></a>Komut dosyası hata ayıklama

Çalışan bir davranış dosyasına ayıklayıcının olsa da, bilgi hizmeti kullanarak günlük yazmak mümkündür **günlük** işlevi. Sözdizimi hataları yorumlayıcı başarısız olur ve özel durumla ilgili bilgileri günlüğe yazar.

Günlük örnek:

```JavaScript
function main(context, state) {

    log("This message will appear in the service logs.");

    log(context.deviceId);

    if (typeof(state) !== "undefined" && state !== null) {
        log("Previous value: " + state.temperature);
    }

    // ...

    return updateState;
}
```

## <a name="deploy-an-advanced-device-model"></a>Gelişmiş cihaz model dağıtma

Gelişmiş cihazınızın modeline dağıtmak için cihaz benzetimi örneğinizin dosyaları karşıya yükle:

Menü çubuğunda **Cihaz modelleri**'ni seçin. **Cihaz modelleri** sayfası, bu cihaz benzetimi örneğinde kullanılabilir cihaz modelleri listeler:

![Cihaz modelleri](media/iot-accelerators-device-simulation-advanced-device/devicemodelnav.png)

Sayfanın sağ üst köşesindeki **+ Cihaz Modeli Ekle**'ye tıklayın:

![Cihaz modeli ekleme](media/iot-accelerators-device-simulation-advanced-device/devicemodels.png)

Tıklayın **Gelişmiş** Gelişmiş cihaz modeli sekmesini açmak için:

![Gelişmiş sekmesi](media/iot-accelerators-device-simulation-advanced-device/advancedtab.png)

Tıklayın **Gözat** ve oluşturduğunuz JSON ve JavaScript dosyaları seçin. Üç dosya seçtiğinizden emin olun. Herhangi bir dosya eksik olduğundan doğrulama başarısız olur:

![Dosyalara göz atın](media/iot-accelerators-device-simulation-advanced-device/browse.png)

Dosyalarınızı doğrulama testlerini geçerse, tıklayın **Kaydet** ve cihazınızın modeline bir benzetim kullanılmak üzere hazırdır. Aksi takdirde, varsa hataları düzeltin ve yeniden karşıya dosya:

![Kaydet](media/iot-accelerators-device-simulation-advanced-device/validated.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır kılavuzunda cihaz benzetimi ve nasıl bir Gelişmiş cihaz modeli oluşturmak kullanılan cihaz model dosyaları hakkında bilgi edindiniz. Ardından, incelemek isteyebilirsiniz nasıl [cihaz benzetimi çözüm hızlandırıcıdan gönderilen telemetri görselleştirmek için kullanım Time Series Insights](https://docs.microsoft.com/azure/iot-accelerators/iot-accelerators-device-simulation-time-series-insights).
