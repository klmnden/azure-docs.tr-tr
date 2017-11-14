---
title: "Uzaktan izleme çözümü - Azure aygıt benzetim | Microsoft Docs"
description: "Bu öğretici ile Uzaktan izleme önceden yapılandırılmış çözümü aygıt benzeticisi kullanmayı gösterir."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 11/10/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 426b7ff6114fd0b79a6af71a78705f11b80862bf
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="test-your-solution-with-simulated-devices"></a>Çözümünüzü sanal cihazlar ile test

Bu öğretici nasıl kullanılacağını gösterir önceden yapılandırılmış Uzaktan izleme çözümü ile aygıt benzeticisi mikro özelleştirin. Aygıt benzeticisi özelliklerini göstermek için Bu öğretici Contoso IOT uygulamada iki senaryo kullanır.

İlk senaryoda, Contoso yeni bir akıllı ampul aygıt test istiyor. Testleri gerçekleştirmek için aşağıdaki özelliklere sahip yeni bir sanal cihaz oluşturun:

*Özellikleri*

| Ad                     | Değerler                      |
| ------------------------ | --------------------------- |
| Renk                    | Beyaz, kırmızı, mavi            |
| Parlaklığını               | 0 ile 100 arası                    |
| Tahmini kalan kullanım ömrü | Geri sayım 10.000 saat |

*Telemetri*

Aşağıdaki tabloda, bir veri akışı olarak buluta ligthbulb raporları veri gösterilmektedir:

| Ad   | Değerler      |
| ------ | ----------- |
| Durum | "açık", "kapalı" |

*Yöntemleri*

Aşağıdaki tabloda yeni cihaz destekler eylemleri gösterir:

| Ad        |
| ----------- |
| Geçiş   |
| Kapat  |

*İlk durumu*

Aşağıdaki tabloda cihaz ilk durumunu gösterir:

| Ad                     | Değerler |
| ------------------------ | -------|
| İlk rengi            | Beyaz  |
| İlk parlaklığını       | 75     |
| İlk kalan kullanım ömrü   | 10,000 |
| İlk telemetri durumu | "açık"   |

İkinci senaryoda, yeni bir telemetri türü contoso varolan eklemek **Soğutucu** aygıt.

Bu öğretici ile Uzaktan izleme önceden yapılandırılmış çözümü aygıt benzeticisi kullanmayı gösterir:

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
> * Yeni bir aygıt türü oluşturma
> * Özel aygıt davranışı benzetimi
> * Yeni bir cihaz türü için Pano ekleyin
> * Var olan bir cihaz türünden özel telemetri Gönder

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi izlemek için Azure aboneliğinizde Uzaktan izleme çözümü dağıtılan bir örneğini gerekir.

Uzaktan izleme çözümü dağıtılan henüz henüz tamamlanmış olmalıdır, [önceden yapılandırılmış Uzaktan izleme çözümü dağıtma](iot-suite-remote-monitoring-deploy.md) Öğreticisi.

<!-- Dominic please this use as your reference https://github.com/Azure/device-simulation-dotnet/wiki/Device-Models -->

## <a name="the-device-simulation-service"></a>Cihaz benzetimi hizmeti

Önceden yapılandırılmış çözümü cihaz benzetimi hizmetinde yerleşik sanal cihaz türleri için değişiklikleri yapın ve yeni sanal cihaz türleri oluşturmanıza olanak sağlar. Özel cihaz türlerini Uzaktan izleme çözümünün davranışını çözüme fiziksel aygıtlarınızı bağlanmadan önce test etmek için kullanabilirsiniz.

## <a name="create-a-simulated-device-type"></a>Bir sanal cihaz türü oluşturma

Yeni bir aygıt türü benzetimi mikro oluşturmanın kolay kopyalamak ve mevcut bir türle değiştirmek için yoludur. Aşağıdaki adımlar nasıl yerleşik kopyalanacağını gösterir **Soğutucu** yeni bir aygıta **ampul** aygıt:

1. Kopyalamak için aşağıdaki komutu kullanın **aygıt benzetimi** GitHub deposunu yerel makinenize:

    ```cmd/sh
    git clone https://github.com/Azure/device-simulation-dotnet.git
    ```

1. Her cihaz türü bir JSON modeli dosyası ve ilişkili komut dosyalarında sahip `Services/data/devicemodels` klasör. Kopya **Soğutucu** oluşturacak şekilde dosyaları **ampul** dosyaları aşağıdaki tabloda gösterildiği gibi:

    | Kaynak                      | Hedef                   |
    | --------------------------- | ----------------------------- |
    | Soğutucu 01.json             | Ampul 01.json             |
    | komut dosyalarını/Soğutucu-01-state.js | komut dosyalarını/ampul-01-state.js |
    | yeniden başlatma/betikleri-method.js    | SwitchOn/betikleri-method.js    |

### <a name="define-the-characteristics-of-the-new-device-type"></a>Yeni cihaz türünün özelliklerini tanımlayın

`lightbulb-01.json` Dosya türünün özelliklerini tanımlar, telemetri gibi oluşturur ve yöntemleri destekler. Aşağıdaki adımlar güncelleştirme `lightbulb-01.json` tanımlamak için dosya **ampul** aygıt:

1. İçinde `lightbulb-01.json` dosya, cihaz meta verilerini aşağıdaki kod parçacığında gösterildiği gibi güncelleştirin:

    ```json
    "SchemaVersion": "1.0.0",
    "Id": "lightbulb-01",
    "Version": "0.0.1",
    "Name": "Lightbulb",
    "Description": "Smart lightbulb device.",
    "Protocol": "MQTT",
    ```

1. İçinde `lightbulb-01.json` dosya, aşağıdaki kod parçacığında gösterildiği gibi benzetimi tanımını güncelleştirin:

    ```json
    "Simulation": {
      "InitialState": {
        "online": true,
        "status": "on"
      },
      "Script": {
        "Type": "javascript",
        "Path": "lightbulb-01-state.js",
        "Interval": "00:00:20"
      }
    },
    ```

1. İçinde `lightbulb-01.json` dosya, aygıt türü özellikleri aşağıdaki kod parçacığında gösterildiği gibi güncelleştirin:

    ```json
    "Properties": {
      "Type": "Lightbulb",
      "Color": "White",
      "Brightness": 75,
      "EstimatedRemainingLife": 10000
    },
    ```

1. İçinde `lightbulb-01.json` dosya, aygıt türü telemetri tanımları aşağıdaki kod parçacığında gösterildiği gibi güncelleştirin:

    ```json
    "Telemetry": [
      {
        "Interval": "00:00:20",
        "MessageTemplate": "{\"status\":\"${status}\"}",
        "MessageSchema": {
          "Name": "lightbulb-status;v1",
          "Format": "JSON",
          "Fields": {
            "status": "text"
          }
        }
      }
    ],
    ```

1. İçinde `lightbulb-01.json` dosya, aygıt türü yöntemleri aşağıdaki kod parçacığında gösterildiği gibi güncelleştirin:

    ```json
    "CloudToDeviceMethods": {
      "SwitchOn": {
        "Type": "javascript",
        "Path": "SwitchOn-method.js"
      },
      "SwitchOff": {
        "Type": "javascript",
        "Path": "SwitchOff-method.js"
      }
    }
    ```

1. Kaydet `lightbulb-01.json` dosya.

### <a name="simulate-custom-device-behavior"></a>Özel aygıt davranışı benzetimi

`scripts/lightbulb-01-state.js` Dosya benzetimi davranışını tanımlar **ampul** türü. Aşağıdaki adımlar güncelleştirme `scripts/lightbulb-01-state.js` davranışını tanımlamak için dosya **ampul** aygıt:

1. Durum tanımı'nda Düzenle `scripts/lightbulb-01-state.js` dosya aşağıdaki kod parçacığında gösterildiği gibi:

    ```js
    // Default state
    var state = {
      online: true,
      status: "on"
    };
    ```

1. Değiştir **farklılık** aşağıdaki işleviyle **çevir** işlevi:

    ```js
    /**
    * Simple formula that sometimes flips the status of the lightbulb
    */
    function flip(value) {
      if (Math.random() < 0.2) {
        return (value == "on") ? "off" : "on"
      }
      return value;
    }
    ```

1. Düzen **ana** işlevi aşağıdaki kod parçacığında gösterildiği gibi davranış uygulamak için:

    ```js
    function main(context, previousState) {

      // Restore the global state before generating the new telemetry, so that
      // the telemetry can apply changes using the previous function state.
      restoreState(previousState);

      // Make this flip every so often
      state.status = flip(state.status);

      return state;
    }
    ```

1. Kaydet `scripts/lightbulb-01-state.js` dosya.

`scripts/SwitchOn-method.js` Dosya uygulayan **anahtar üzerinde** yönteminde bir **ampul** aygıt. Aşağıdaki adımlar güncelleştirme `scripts/SwitchOn-method.js` dosyası:

1. Durum tanımı'nda Düzenle `scripts/SwitchOn-method.js` dosya aşağıdaki kod parçacığında gösterildiği gibi:

    ```js
    var state = {
       status: "on"
    };
    ```

1. Ampul geçmek için düzenleme **ana** gibi işlev:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch On method.");
        state.status = "on";
        updateState(state);
    }
    ```

1. Kaydet `scripts/SwitchOn-method.js` dosya.

1. Bir kopya `scripts/SwitchOn-method.js` adlı dosyaya `scripts/SwitchOff-method.js`.

1. Ampul Kapat için Düzenle **ana** işlevi `scripts/SwitchOff-method.js` gibi dosya:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch Off method.");
        state.status = "off";
        updateState(state);
    }
    ```

1. Kaydet `scripts/SwitchOff-method.js` dosya.

### <a name="test-the-lightbulb-device-type"></a>Ampul aygıt türü test

Test etmek için **ampul** aygıt türü, ilk test edebilirsiniz, cihaz türünüz yerel bir kopyasını çalıştırarak beklendiği gibi davranır **aygıt benzetimi** hizmet. Test ve yeni cihaz türünüz yerel olarak hata ayıklaması, kapsayıcı derlemenizi ve yeniden dağıtmanızı **aygıt benzetimi** Azure hizmet.

Test ve değişikliklerinizi yerel olarak hata ayıklama için bkz: [cihaz benzetimi genel bakış](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md).

Yeni kopyalamak için projeyi yapılandırmak **ampul** aygıt dosyalar çıkış dizinine.

Yeni cihaz dağıtılan bir çözümde sınamak için aşağıdakilerden birini bakın:

* [Özel docker hub'a hesabından kapsayıcıları dağıtma](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploying-containers-from-custom-docker-hub-account)
* [Dağıtılan bir kapsayıcı el ile kopyalama aracılığıyla güncelleştirme](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#update-a-deployed-container-via-manual-copy)

Üzerinde **aygıtları** sayfasında, yeni türünüzü örnekleri sağlayabilirsiniz:

![Kullanılabilir benzetimleri listesini görüntüleyin](media/iot-suite-remote-monitoring-test/devicesmodellist.png)

Sanal cihaz telemetrisinden görüntüleyebilirsiniz:

![Görünüm ampul telemetri](media/iot-suite-remote-monitoring-test/devicestelemetry.png)

Çağırabilirsiniz **SwitchOn** ve **SwitchOff** aygıtınızda yöntemleri:

![Ampul yöntemlerini çağırın](media/iot-suite-remote-monitoring-test/devicesmethods.png)

Azure'a dağıtımı için yeni cihaz türü ile bir Docker görüntüsü oluşturmak için bkz: [özelleştirilmiş bir Docker görüntü oluşturma](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#building-a-customized-docker-image).

## <a name="add-a-new-telemetry-type"></a>Yeni bir telemetri türü ekleme

Bu bölümde, yeni bir telemetri türünü desteklemek için varolan bir sanal cihaz türünü değiştirme açıklar.

### <a name="locate-the-chiller-device-type-files"></a>Soğutucu aygıt türü dosyalarını bulun

Aşağıdaki adımlar yerleşik tanımlama dosyaları bulmak nasıl gösterir **Soğutucu** aygıt:

1. Zaten yapmadıysanız, kopyalamak için aşağıdaki komutu kullanın **aygıt benzetimi** GitHub deposunu yerel makinenize:

    ```cmd/sh
    git clone https://github.com/Azure/device-simulation-dotnet.git
    ```

1. Her cihaz türü bir JSON modeli dosyası ve ilişkili komut dosyalarında sahip `Services/data/devicemodels` klasör. Benzetim tanımlayan dosyaların **Soğutucu** aygıt türü şunlardır:
    * `Services/data/devicemodels/chiller-01.json`
    * `Services/data/devicemodels/scripts/chiller-01-state.js`

### <a name="specify-the-new-telemetry-type"></a>Yeni telemetri türünü belirtin

Aşağıdaki adımlar yeni bir ekleme gösterir **iç sıcaklık** için yazın **Soğutucu** aygıt türü:

1. `chiller-01.json` dosyasını açın.

1. Güncelleştirme **SchemaVersion** gibi değer:

    ```json
    "SchemaVersion": "1.1.0",
    ```

1. İçinde **ilk durum** bölümünde, aşağıdaki iki tanımları ekleyin:

    ```json
    "internal_temperature": 65.0,
    "internal_temperature_unit": "F",
    ```

1. İçinde **Telemetri** dizisi, aşağıdaki tanımını ekleyin:

    ```json
    {
      "Interval": "00:00:05",
      "MessageTemplate": "{\"internal_temperature\":${internal_temperature},\"internal_temperature_unit\":\"${internal_temperature_unit}\"}",
      "MessageSchema": {
        "Name": "chiller-internal-temperature;v1",
        "Format": "JSON",
        "Fields": {
          "temperature": "double",
          "temperature_unit": "text"
        }
      }
    },
    ```

1. Kaydet `chiller-01.json` dosya.

1. `scripts/chiller-01-state.js` dosyasını açın.

1. Aşağıdaki alanları ekleyin **durumu** değişkeni:

    ```js
    internal_temperature: 65.0,
    internal_temperature_unit: "F",
    ```

1. Aşağıdaki satırı ekleyin **ana** işlevi:

    ```js
    state.internal_temperature = vary(65, 2, 15, 125);
    ```

1. Kaydet `scripts/chiller-01-state.js` dosya.

### <a name="test-the-chiller-device-type"></a>Soğutucu aygıt türü test

Güncelleştirilmiş test etmek için **Soğutucu** aygıt türü, ilk test edebilirsiniz, cihaz türünüz yerel bir kopyasını çalıştırarak beklendiği gibi davranır **aygıt benzetimi** hizmet. Test ve güncelleştirilmiş cihaz türünüz yerel olarak hata ayıklaması, kapsayıcı derlemenizi ve yeniden dağıtmanızı **aygıt benzetimi** Azure hizmet.

Çalıştırdığınızda **aygıt benzetimi** Uzaktan izleme çözümünüz için telemetri gönderir hizmeti yerel olarak. Üzerinde **aygıtları** sayfasında, güncelleştirilmiş türünüz örnekleri sağlayabilirsiniz.

Test ve değişikliklerinizi yerel olarak hata ayıklama için bkz: [Visual Studio ile hizmetini çalıştıran](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#running-the-service-with-visual-studio) veya [derleme ve komut satırından çalıştırma](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#build-and-run-from-the-command-line).

Yeni cihaz dağıtılan bir çözümde sınamak için aşağıdakilerden birini bakın:

* [Özel docker hub'a hesabından kapsayıcıları dağıtma](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploying-containers-from-custom-docker-hub-account)
* [Dağıtılan bir kapsayıcı el ile kopyalama aracılığıyla güncelleştirme](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#update-a-deployed-container-via-manual-copy)

Üzerinde **aygıtları** sayfasında, güncelleştirilmiş türünüz örnekleri sağlayabilirsiniz:

![Güncelleştirilmiş Soğutucu ekleme](media/iot-suite-remote-monitoring-test/devicesupdatedchiller.png)

Yeni görüntüleyebilirsiniz **iç sıcaklık** sanal cihaz telemetrisinden.

Azure'a dağıtımı için yeni cihaz türü ile bir Docker görüntüsü oluşturmak için bkz: [özelleştirilmiş bir Docker görüntü oluşturma](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#building-a-customized-docker-image).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide gösterilen, nasıl için:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Yeni bir aygıt türü oluşturma
> * Özel aygıt davranışı benzetimi
> * Yeni bir cihaz türü için Pano ekleyin
> * Var olan bir cihaz türünden özel telemetri Gönder

Cihaz benzetimi hizmetini kullanmayı öğrendiniz artık, önerilen sonraki adıma edinmektir nasıl [bir fiziksel aygıt, Uzaktan izleme çözümüne bağlama](iot-suite-connecting-devices-node.md).

Uzaktan izleme çözümü hakkında daha fazla Geliştirici bilgi için bkz:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici Sorun Giderme Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->