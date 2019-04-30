---
title: include dosyası
description: include dosyası
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 07/26/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 0b2b6814ad0b55d8f56998f6d4c9b6bb88663e04
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61446368"
---
## <a name="state-behavior"></a>Durum davranışı

[Benzetimi](../articles/iot-accelerators/iot-accelerators-remote-monitoring-device-schema.md#simulation) bölümü cihaz modeli şemasını, bir sanal cihaz iç durumunu tanımlar:

- `InitialState` ilk cihaz durum nesnesinin tüm özelliklerinin değerlerini tanımlar.
- `Script` cihaz durumunu güncelleştirmek için bir zamanlamaya göre çalışan bir JavaScript dosyası tanımlar.

Aşağıdaki örnek sanal Soğutucu cihaz için cihaz durum nesnesinin tanımı gösterilmektedir:

```json
"Simulation": {
  "InitialState": {
    "online": true,
    "temperature": 75.0,
    "temperature_unit": "F",
    "humidity": 70.0,
    "humidity_unit": "%",
    "pressure": 150.0,
    "pressure_unit": "psig",
    "simulation_state": "normal_pressure"
  },
  "Interval": "00:00:05",
  "Scripts": {
    "Type": "javascript",
    "Path": "chiller-01-state.js"
  }
}
```

Tanımlanan sanal cihaz durumunu `InitialState` bölümünde, benzetim hizmeti tarafından bellekte tutulur. Durum bilgileri için giriş olarak geçirilir `main` tanımlanan işlevi **Soğutucu 01 state.js**. Bu örnekte benzetim Hizmeti'ni çalıştıran **Soğutucu 01 state.js** her beş saniyede bir dosya. Betik, sanal cihaz durumunu değiştirebilirsiniz.

Aşağıdaki tipik bir özetini gösterir `main` işlevi:

```javascript
function main(context, previousState, previousProperties) {

  // Use the previous device state to
  // generate the new device state
  // returned by the function.

  return state;
}
```

`context` Parametresi, aşağıdaki özelliklere sahiptir:

- `currentTime` bir dize olarak biçimine sahip `yyyy-MM-dd'T'HH:mm:sszzz`
- `deviceId`, örneğin `Simulated.Chiller.123`
- `deviceModel`, örneğin `Chiller`

`state` Cihaz benzetimi hizmet tarafından gibi cihaz durumu parametresi içerir. Bu değer `state` önceki çağrı tarafından döndürülen nesne `main`.

Tipik bir uygulaması aşağıdaki örnekte `main` benzetimi hizmet tarafından cihaz durumunu işlemek için yöntemi:

```javascript
// Default state
var state = {
  online: true,
  temperature: 75.0,
  temperature_unit: "F",
  humidity: 70.0,
  humidity_unit: "%",
  pressure: 150.0,
  pressure_unit: "psig",
  simulation_state: "normal_pressure"
};

function restoreState(previousState) {
  // If the previous state is null, force a default state
  if (previousState !== undefined && previousState !== null) {
      state = previousState;
  } else {
      log("Using default state");
  }
}

function main(context, previousState, previousProperties) {

  restoreState(previousState);

  // Update state

  return state;
}
```

Aşağıdaki örnekte gösterildiği nasıl `main` yöntemi zamanla değişir telemetri değerleri benzetimini:

```javascript
/**
 * Simple formula generating a random value
 * around the average and between min and max
 */
function vary(avg, percentage, min, max) {
  var value = avg * (1 + ((percentage / 100) * (2 * Math.random() - 1)));
  value = Math.max(value, min);
  value = Math.min(value, max);
  return value;
}


function main(context, previousState, previousProperties) {

    restoreState(previousState);

    // 75F +/- 5%,  Min 25F, Max 100F
    state.temperature = vary(75, 5, 25, 100);

    // 70% +/- 5%,  Min 2%, Max 99%
    state.humidity = vary(70, 5, 2, 99);

    log("Simulation state: " + state.simulation_state);
    if (state.simulation_state === "high_pressure") {
        // 250 psig +/- 25%,  Min 50 psig, Max 300 psig
        state.pressure = vary(250, 25, 50, 300);
    } else {
        // 150 psig +/- 10%,  Min 50 psig, Max 300 psig
        state.pressure = vary(150, 10, 50, 300);
    }

    return state;
}
```

Tam görüntüleyebilirsiniz [Soğutucu 01 state.js](https://github.com/Azure/device-simulation-dotnet/blob/master/Services/data/devicemodels/scripts/chiller-01-state.js) GitHub üzerinde.

## <a name="method-behavior"></a>Yöntem davranışı

[CloudToDeviceMethods](../articles/iot-accelerators/iot-accelerators-remote-monitoring-device-schema.md#cloudtodevicemethods) bölümü cihaz modeli şemasını, yanıt veren bir sanal cihaz için yöntemleri tanımlar.

Aşağıdaki örnek, bir sanal Soğutucu cihaz tarafından desteklenen yöntemlerin listesi gösterir:

```json
"CloudToDeviceMethods": {
  "Reboot": {
    "Type": "javascript",
    "Path": "Reboot-method.js"
  },
  "FirmwareUpdate": {
    "Type": "javascript",
    "Path": "FirmwareUpdate-method.js"
  },
  "EmergencyValveRelease": {
    "Type": "javascript",
    "Path": "EmergencyValveRelease-method.js"
  },
  "IncreasePressure": {
    "Type": "javascript",
    "Path": "IncreasePressure-method.js"
  }
}
```

Her yöntemin yöntem davranışını uygulayan ilişkili olan bir JavaScript dosyası vardır.

Tanımlanan sanal cihaz durumunu `InitialState` bölüm şeması, benzetim hizmeti tarafından bellekte tutulur. Durum bilgileri için giriş olarak geçirilir `main` yöntem çağrıldığında JavaScript dosyasında tanımlanan işlevi. Betik, sanal cihaz durumunu değiştirebilirsiniz.

Aşağıdaki tipik bir özetini gösterir `main` işlevi:

```javascript
function main(context, previousState, previousProperties) {

}
```

`context` Parametresi, aşağıdaki özelliklere sahiptir:

- `currentTime` bir dize olarak biçimine sahip `yyyy-MM-dd'T'HH:mm:sszzz`
- `deviceId`, örneğin `Simulated.Chiller.123`
- `deviceModel`, örneğin `Chiller`

`state` Cihaz benzetimi hizmet tarafından gibi cihaz durumu parametresi içerir.

`properties` Parametresi bildirilen özellikleri IOT Hub cihaz ikizine yazılan cihaz özelliklerini içerir.

Yöntemin davranışını uygulamak için kullanabileceğiniz üç genel işlevleri vardır:

- `updateState` benzetim hizmet tarafından tutulan durumu güncelleştirilemedi.
- `updateProperty` tek bir cihaz özelliği güncelleştirilecek.
- `sleep` uzun süre çalışan bir görev benzetimini yapmak için yürütme duraklatmak için.

Aşağıdaki örnekte kısaltılmış sürümü gösterilmektedir **IncreasePressure method.js** sanal Soğutucu cihazlar tarafından kullanılan betiği:

```javascript
function main(context, previousState, previousProperties) {

    log("Starting 'Increase Pressure' method simulation (5 seconds)");

    // Pause the simulation and change the simulation mode so that the
    // temperature will fluctuate at ~250 when it resumes
    var state = {
        simulation_state: "high_pressure",
        CalculateRandomizedTelemetry: false
    };
    updateState(state);

    // Increase
    state.pressure = 210;
    updateState(state);
    log("Pressure increased to " + state.pressure);
    sleep(1000);

    // Increase
    state.pressure = 250;
    updateState(state);
    log("Pressure increased to " + state.pressure);
    sleep(1000);

    // Resume the simulation
    state.CalculateRandomizedTelemetry = true;
    updateState(state);

    log("'Increase Pressure' method simulation completed");
}
```

## <a name="debugging-script-files"></a>Komut dosyası hata ayıklama

Bir hata ayıklayıcının durumu ve yöntemi komut dosyalarını çalıştırmak için cihaz benzetimi hizmeti tarafından kullanılan Javascript yorumlayıcı mümkün değildir. Ancak, hizmeti oturum açma bilgileri kaydedebilirsiniz. Yerleşik `log()` işlevi izlemek ve işlev yürütme hata ayıklama bilgileri tasarruf etmenize olanak tanır.

Yorumlayıcı başarısız olursa söz dizimi hatası varsa ve yazan bir `Jint.Runtime.JavaScriptException` hizmet günlüğüne giriş.

[Hizmeti yerel olarak çalışan](https://github.com/Azure/device-simulation-dotnet#running-the-service-locally-eg-for-development-tasks) makalede GitHub üzerindeki cihaz benzetimi hizmeti yerel olarak çalıştırmak nasıl gösterir. Hizmet yerel olarak çalışan sanal cihazlarınızı buluta dağıtmadan önce ayıklamanızı kolaylaştırır.