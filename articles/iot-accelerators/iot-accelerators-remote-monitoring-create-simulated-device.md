---
title: Cihaz benzetimi, IOT, Uzaktan izleme - Azure | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda ile Uzaktan izleme çözüm Hızlandırıcısını cihaz simülatörü kullanma işlemini gösterir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: conceptual
ms.openlocfilehash: 5044f8b85e59911633a4ffab509efc000948144a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65832589"
---
# <a name="create-and-test-a-new-simulated-device"></a>Oluşturma ve yeni bir simülasyon cihazı test etme

Uzaktan izleme çözüm Hızlandırıcısını sanal cihazlarınızı tanımlamanızı sağlar. Bu makalede yeni bir sanal ampul cihaz tanımlayın ve ardından yerel olarak test gösterilmektedir. Çözüm Hızlandırıcısını chillers ve kamyon gibi sanal cihazları içerir. Ancak, IOT çözümlerinizi gerçek cihazları dağıtmadan önce test etmek için kendi sanal cihazlar tanımlayabilirsiniz.

> [!NOTE]
> Bu makalede, sanal cihazlar cihaz benzetimi hizmette barındırılan kullanmayı açıklar. Gerçek bir cihaz oluşturmak istiyorsanız, bkz. [Cihazınızı Uzaktan izleme çözüm hızlandırıcısına bağlamayı](iot-accelerators-connecting-devices.md).

Bu nasıl yapılır kılavuzunda, cihaz benzetimi mikro hizmet özelleştirme işlemini göstermektedir. Bu mikro hizmet Uzaktan izleme çözüm Hızlandırıcısını bir parçasıdır. Cihaz benzetimi özellikleri göstermek için bu nasıl yapılır kılavuzunda Contoso IOT uygulamada iki senaryoda kullanır:

Bu senaryoda, Contoso için yeni bir telemetri türünü, mevcut ekleme **Soğutucu** cihaz türü.

İkinci senaryoda Contoso yeni bir akıllı ampul cihazı test etmek istiyor. Testleri çalıştırmak için aşağıdaki özelliklere sahip yeni bir simülasyon cihazı oluşturun:

*Özellikleri*

| Ad                     | Değerler                      |
| ------------------------ | --------------------------- |
| Renk                    | Beyaz, kırmızı, mavi            |
| Parlaklık               | 0 ile 100 arası                    |
| Tahmini kalan kullanım ömrü | Geri sayım 10.000 saat |

*Telemetri*

Aşağıdaki tabloda, bir veri akışı olarak buluta ampul rapor verilerini gösterir:

| Ad   | Değerler      |
| ------ | ----------- |
| Durum | "açık", "kapalı" |
| Sıcaklık | Derece F |
| Çevrimiçi | TRUE, false |

> [!NOTE]
> **Çevrimiçi** telemetri değeri tüm sanal türleri için zorunludur.

*Methods*

Aşağıdaki tablo, yeni cihazın desteklediği eylemleri gösterir:

| Ad        |
| ----------- |
| Geçiş   |
| Kapat  |

*Başlangıç durumu*

Aşağıdaki tabloda, cihaz ilk durumunu gösterir:

| Ad                     | Değerler |
| ------------------------ | -------|
| İlk renk            | Beyaz  |
| İlk parlaklık       | 75     |
| İlk kalan kullanım ömrü   | 10,000 |
| İlk telemetri durumu | "on"   |
| İlk telemetri sıcaklık | 200   |

Nasıl yapılır bu kılavuzdaki adımları tamamlamak için etkin bir Azure aboneliği gerekir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunda takip etmek için ihtiyacınız vardır:

* Visual Studio Code. Yapabilecekleriniz [Mac, Linux ve Windows için Visual Studio Code'u indirin](https://code.visualstudio.com/download).
* .NET core. İndirebileceğiniz [Mac, Linux ve Windows için .NET Core](https://www.microsoft.com/net/download).
* [Visual Studio Code için C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* Postman. İndirebileceğiniz [Mac, Windows veya Linux için Postman](https://www.getpostman.com/apps).
* Bir [IOT hub'ı Azure aboneliğinize dağıtılır](../../articles/iot-hub/iot-hub-create-through-portal.md). Bu kılavuzdaki adımları tamamlamak için IOT hub'ınızın bağlantı dizesi gerekir. Bağlantı dizesini Azure portalından alabilirsiniz.
* SQL API'si kullanan ve için yapılandırılmış bir Cosmos DB veritabanı [güçlü tutarlılık](../../articles/cosmos-db/how-to-manage-database-account.md). Bu kılavuzdaki adımları tamamlamak için Cosmos DB veritabanı bağlantı dizesi gerekir. Bağlantı dizesini Azure portalından alabilirsiniz.

## <a name="prepare-your-development-environment"></a>Geliştirme ortamınızı hazırlama

Geliştirme ortamınızı hazırlamak üzere aşağıdaki görevleri tamamlayın:

* Cihaz benzetimi mikro Hizmet kaynağı indirin.
* Depolama bağdaştırıcısı mikro Hizmet kaynağı indirin.
* Depolama bağdaştırıcısı mikro hizmet yerel olarak çalıştırın.

Bu makaledeki yönergeler, Windows kullanmakta olduğunuz varsayılır. Başka bir işletim sistemi kullanıyorsanız, bazı dosya yolları ve komutları ortamınıza uyacak şekilde ayarlamanız gerekebilir.

### <a name="download-the-microservices"></a>Mikro hizmetler indirin

İndirip sıkıştırmasını [uzaktan mikro hizmetler izleme](https://github.com/Azure/remote-monitoring-services-dotnet/archive/master.zip) yerel makinenizde uygun bir konuma github'dan. Bu klasörün adı olduğu varsayılır **Uzaktan izleme-services-dotnet-yöneticili**.

İndirip sıkıştırmasını [cihaz benzetimi mikro hizmet](https://github.com/Azure/device-simulation-dotnet/archive/master.zip) yerel makinenizde uygun bir konuma github'dan. Bu klasörün adı olduğu varsayılır **cihaz-simülasyon-dotnet-master**.

### <a name="run-the-storage-adapter-microservice"></a>Depolama bağdaştırıcısı mikro hizmet çalıştırma

Açık **remote-monitoring-services-dotnet-master\storage-adapter** Visual Studio code'da klasörü. Tıklatın **geri** herhangi düzeltmek için düğmeler çözümlenmemiş bağımlılıklar.

Açık **storage-adapter/WebService/appsettings.ini** dosya ve Cosmos DB bağlantı dizenizi atama **documentDBConnectionString** değişkeni.

Mikro hizmet yerel olarak çalıştırmak için tıklayın **hata ayıklama > hata ayıklamayı Başlat**.

**Terminal** penceresi Visual Studio code'da web hizmetinin sistem durumu denetimi için bir URL dahil olmak üzere çalışan mikro hizmet çıktısını gösterir: [ http://127.0.0.1:9022/v1/status ](http://127.0.0.1:9022/v1/status). Bu adrese gidin, durumu olmalıdır "Tamam: Canlı ve iyi".

Sonraki adımları tamamlarken Visual Studio Code'nın bu örneğinde çalışan depolama bağdaştırıcısı mikro hizmet bırakın.

## <a name="modify-the-chiller"></a>Soğutucu değiştirme

Bu bölümde, yeni bir ekleme **iç ortam sıcaklığı** varolan telemetri türü **Soğutucu** cihaz türü:

1. Yeni bir klasör oluşturun **C:\temp\devicemodels** yerel makinenizde.

1. Aşağıdaki dosyaları cihaz benzetimi mikro hizmet indirilen kopyadan yeni klasörünüze kopyalayın:

    | source | Hedef |
    | ------ | ----------- |
    | Services\data\devicemodels\chiller-01.json | C:\temp\devicemodels\chiller-01.json |
    | Services\data\devicemodels\scripts\chiller-01-state.js | C:\temp\devicemodels\scripts\chiller-01-state.js |
    | Services\data\devicemodels\scripts\Reboot-method.js | C:\temp\devicemodels\scripts\Reboot-method.js |
    | Services\data\devicemodels\scripts\FirmwareUpdate-method.js | C:\temp\devicemodels\scripts\FirmwareUpdate-method.js |
    | Services\data\devicemodels\scripts\EmergencyValveRelease-method.js | C:\temp\devicemodels\scripts\EmergencyValveRelease-method.js |
    | Services\data\devicemodels\scripts\IncreasePressure-method.js | C:\temp\devicemodels\scripts\IncreasePressure-method.js |

1. Açık **C:\temp\devicemodels\chiller-01.json** dosya.

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

1. Kaydet **C:\temp\devicemodels\chiller-01.json** dosya.

1. Açık **C:\temp\devicemodels\scripts\chiller-01-state.js** dosya.

1. Aşağıdaki alanları ekleyin **durumu** değişkeni:

    ```js
    internal_temperature: 65.0,
    internal_temperature_unit: "F",
    ```

1. Güncelleştirme **ana** gibi işlev:

    ```js
    function main(context, previousState, previousProperties) {

        // Restore the global state before generating the new telemetry, so that
        // the telemetry can apply changes using the previous function state.
        restoreSimulation(previousState, previousProperties);

        // 75F +/- 5%,  Min 25F, Max 100F
        state.temperature = vary(75, 5, 25, 100);

        // 70% +/- 5%,  Min 2%, Max 99%
        state.humidity = vary(70, 5, 2, 99);

        // 65F +/- 2%,  Min 15F, Max 125F
        state.internal_temperature = vary(65, 2, 15, 125);

        log("Simulation state: " + state.simulation_state);
        if (state.simulation_state === "high_pressure") {
            // 250 psig +/- 25%,  Min 50 psig, Max 300 psig
            state.pressure = vary(250, 25, 50, 300);
        } else {
            // 150 psig +/- 10%,  Min 50 psig, Max 300 psig
            state.pressure = vary(150, 10, 50, 300);
        }

        updateState(state);
        return state;
    }
    ```

1. Kaydet **C:\temp\devicemodels\scripts\chiller-01-state.js** dosya.

## <a name="create-the-lightbulb"></a>Ampul oluşturma

Bu bölümde, yeni bir tanımlama **ampul** cihaz türü:

1. Bir dosya oluşturun **C:\temp\devicemodels\lightbulb-01.json** ve aşağıdaki içeriği ekleyin:

    ```json
    {
      "SchemaVersion": "1.0.0",
      "Id": "lightbulb-01",
      "Version": "0.0.1",
      "Name": "Lightbulb",
      "Description": "Smart lightbulb device.",
      "Protocol": "MQTT",
      "Simulation": {
        "InitialState": {
          "online": true,
          "temperature": 200.0,
          "temperature_unit": "F",
          "status": "on"
        },
        "Interval": "00:00:20",
        "Scripts": [
          {
            "Type": "javascript",
            "Path": "lightbulb-01-state.js"
          }
        ]
      },
      "Properties": {
        "Type": "Lightbulb",
        "Color": "White",
        "Brightness": 75,
        "EstimatedRemainingLife": 10000
      },
      "Tags": {
        "Location": "Building 2",
        "Floor": "2",
        "Campus": "Redmond"
      },
      "Telemetry": [
        {
          "Interval": "00:00:20",
          "MessageTemplate": "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\",\"status\":\"${status}\"}",
          "MessageSchema": {
            "Name": "lightbulb-status;v1",
            "Format": "JSON",
            "Fields": {
              "temperature": "double",
              "temperature_unit": "text",
              "status": "text"
            }
          }
        }
      ],
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
    }
    ```

    Değişiklikleri kaydetmek **C:\temp\devicemodels\lightbulb-01.json**.

1. Bir dosya oluşturun **C:\temp\devicemodels\scripts\lightbulb-01-state.js** ve aşağıdaki içeriği ekleyin:

    ```javascript
    "use strict";

    // Default state
    var state = {
      online: true,
      temperature: 200.0,
      temperature_unit: "F",
      status: "on"
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
     *
     * @returns random value with given parameters
     */
    function vary(avg, percentage, min, max) {
      var value = avg * (1 + ((percentage / 100) * (2 * Math.random() - 1)));
      value = Math.max(value, min);
      value = Math.min(value, max);
      return value;
    }

    /**
     * Simple formula that sometimes flips the status of the lightbulb
     */
    function flip(value) {
      if (Math.random() < 0.2) {
        return (value == "on") ? "off" : "on"
      }
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
    function main(context, previousState, previousProperties) {

      // Restore the global device properties and the global state before
      // generating the new telemetry, so that the telemetry can apply changes
      // using the previous function state.
      restoreSimulation(previousState, previousProperties);

      state.temperature = vary(200, 5, 150, 250);

      // Make this flip every so often
      state.status = flip(state.status);

      updateState(state);

      return state;
    }
    ```

    Değişiklikleri kaydetmek **C:\temp\devicemodels\scripts\lightbulb-01-state.js**.

1. Bir dosya oluşturun **C:\temp\devicemodels\scripts\SwitchOn-method.js** ve aşağıdaki içeriği ekleyin:

    ```javascript
    "use strict";

    // Default state
    var state = {
      status: "on"
    };

    /**
     * Entry point function called by the method.
     *
     * @param context        The context contains current time, device model and id
     * @param previousState  The device state since the last iteration
     * @param previousProperties  The device properties since the last iteration
     */
    function main(context, previousState) {
      log("Executing lightbulb Switch On method.");
      state.status = "on";
      updateState(state);
    }
    ```

    Değişiklikleri kaydetmek **C:\temp\devicemodels\scripts\SwitchOn-method.js**.

1. Bir dosya oluşturun **C:\temp\devicemodels\scripts\SwitchOff-method.js** ve aşağıdaki içeriği ekleyin:

    ```javascript
    "use strict";

    // Default state
    var state = {
      status: "on"
    };

    /**
     * Entry point function called by the method.
     *
     * @param context        The context contains current time, device model and id
     * @param previousState  The device state since the last iteration
     * @param previousProperties  The device properties since the last iteration
     */
    function main(context, previousState) {
      log("Executing lightbulb Switch Off method.");
      state.status = "off";
      updateState(state);
    }
    ```

    Değişiklikleri kaydetmek **C:\temp\devicemodels\scripts\SwitchOff-method.js**.

Özelleştirilmiş bir sürümünü oluşturdunuz **Soğutucu** cihaz türü ve yeni oluşturulan **ampul** cihaz türü.

## <a name="test-the-devices"></a>Test cihazları

Bu bölümde, yerel olarak önceki bölümde oluşturduğunuz cihaz türlerini test edin.

### <a name="run-the-device-simulation-microservice"></a>Cihaz benzetimi mikro hizmet çalıştırma

Açık **cihaz-simülasyon-dotnet-master** Visual Studio Code yeni bir örneğini github'dan indirdiğiniz klasörü. Tıklatın **geri** herhangi düzeltmek için düğmeler çözümlenmemiş bağımlılıklar.

Açık **WebService/appsettings.ini** dosya ve Cosmos DB bağlantı dizenizi atama **documentdb_connstring** değişkeni ve da ayarları aşağıdaki gibi değiştirin:

```ini
device_models_folder = C:\temp\devicemodels\

device_models_scripts_folder = C:\temp\devicemodels\scripts\
```

Mikro hizmet yerel olarak çalıştırmak için tıklayın **hata ayıklama > hata ayıklamayı Başlat**.

**Terminal** penceresi Visual Studio code'da çalışan mikro hizmet çıktısını gösterir.

Sonraki adımları tamamlarken Visual Studio Code'nın bu örneğinde çalışan cihaz benzetimi mikro hizmet bırakın.

### <a name="set-up-a-monitor-for-device-events"></a>Bir izleyici cihaz olayları için ayarlama

Bu bölümde, IOT hub'ınıza bağlı cihazlardan gönderilen telemetri verilerini görüntülemek için bir Olay İzleyicisi ayarlamak için Azure CLI'yı kullanın.

Aşağıdaki betik, IOT hub'ınızın adı olduğunu varsayar **cihaz benzetimi test**.

```azurecli-interactive
# Install the IoT extension if it's not already installed
az extension add --name azure-cli-iot-ext

# Monitor telemetry sent to your hub
az iot hub monitor-events --hub-name device-simulation-test
```

Sanal cihazlar test ederken çalıştıran Olay İzleyici bırakın.

### <a name="create-a-simulation-with-the-updated-chiller-device-type"></a>Güncelleştirilmiş Soğutucu cihaz türü ile bir simülasyon oluşturma

Bu bölümde, güncelleştirilmiş Soğutucu cihaz türünü kullanarak bir simülasyon çalıştırma için cihaz benzetimi mikro hizmet istemek için Postman Aracı'nı kullanın. Postman, REST istekleri göndermek için bir web hizmeti sağlayan bir araçtır. Gereksinim duyduğunuz Postman yapılandırma dosyaları, yerel kopyasında olan **cihaz benzetimi dotnet** depo.

Postman'ı ayarlamak için:

1. Yerel makinenizde Postman'i açın.

1. Tıklayın **Dosya > içeri aktarma**. Ardından **dosya seçin**.

1. Gidin **cihaz-simülasyon-dotnet-master/docs/postman** klasör. Seçin **Azure IOT cihaz benzetimi çözüm accelerator.postman_collection** ve **Azure IOT cihaz benzetimi çözüm accelerator.postman_environment** tıklatıp **açın**.

1. Genişletin **Azure IOT cihaz benzetimi Çözüm Hızlandırıcısı** istekleri gönderebilirsiniz.

1. Tıklayın **yok ortam** seçip **Azure IOT cihaz benzetimi Çözüm Hızlandırıcısı**.

Artık bir koleksiyon ve cihaz benzetimi mikro hizmet ile etkileşim kurmak için kullanabileceğiniz Postman çalışma alanınızda yüklenen ortam vardır.

Yapılandırma ve benzetim çalıştırmak için:

1. Postman koleksiyonuna seçin **Oluştur değiştiren Soğutucu benzetimi** tıklatıp **Gönder**. Bu isteği dört sanal Soğutucu cihaz türü örneklerini oluşturur.

1. Azure CLI penceresinde Olay İzleyicisi çıkışını yeni dahil olmak üzere sanal cihazlar, gelen telemetriyi gösterir **internal_temperature** değerleri.

Benzetimi durdurmak için seçin **benzetimi Durdur** isteği Postman tıklayıp **Gönder**.

### <a name="create-a-simulation-with-the-lightbulb-device-type"></a>Ampul cihaz türü ile bir simülasyon oluşturma

Bu bölümde, ampul cihaz türünü kullanarak bir simülasyon çalıştırma için cihaz benzetimi mikro hizmet istemek için Postman Aracı'nı kullanın. Postman, REST istekleri göndermek için bir web hizmeti sağlayan bir araçtır.

Yapılandırma ve benzetim çalıştırmak için:

1. Postman koleksiyonuna seçin **ampul benzetimi oluşturmak** tıklatıp **Gönder**. Bu isteği benzetimli Ampul cihaz türünün iki örneği oluşturur.

1. Azure CLI penceresinde Olay İzleme çıktısı, sanal lightbulbs gelen telemetriyi gösterir.

Benzetimi durdurmak için seçin **benzetimi Durdur** isteği Postman tıklayıp **Gönder**.

## <a name="clean-up-resources"></a>Kaynakları temizleme

İki yerel olarak çalışan mikro hizmetler, Visual Studio kod örneklerindeki durdurabilirsiniz (**hata ayıklama > hata ayıklamayı Durdur**).

Artık IOT Hub ve Cosmos DB örnekleri gerektiriyorsa, gereksiz ücretlerden kaçınmak için Azure aboneliğinizden silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuz, türü özel bir simülasyon cihazı oluşturma ve cihaz benzetimi mikro hizmet yerel olarak çalıştırarak test nasıl oluşturulacağını gösterir.

Özel sanal cihazı türlerinizi dağıtma hakkında bilgi edinmek için önerilen sonraki adım olan [Uzaktan izleme çözüm hızlandırıcısının](iot-accelerators-remote-monitoring-deploy-simulated-device.md).
