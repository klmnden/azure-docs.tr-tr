---
title: "Node.js ile Azure IOT kenar modülü oluşturma | Microsoft Docs"
description: "Bu öğreticide en son Azure IOT kenar NPM paket ve Yeoman kullanarak bir bırak veri dönüştürücü modülü yazma paylaşan Oluşturucu."
services: iot-hub
author: sushi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: js
ms.topic: article
ms.date: 06/28/2017
ms.author: sushi
ms.openlocfilehash: ba466f47e157d805600c41fa3d84ed5a0363969c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-iot-edge-module-with-nodejs"></a>Node.js ile Azure IOT kenar modülü oluşturun

Bu öğretici Azure IOT Edge'de JS için bir modül oluşturma gösterir.

Ortam kurulumu ve nasıl yazılacağını Bu öğreticide, biz yol bir [bırak](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) veri dönüştürücü modülü en son Azure IOT kenar NPM paketleri kullanma.

## <a name="prerequisites"></a>Ön koşullar

Bu bölümde IOT kenar modülü geliştirme ortamınızı ayarlayın. Her ikisi de geçerli *64-bit Windows* ve *64-bit Linux (Ubuntu 14 +)* işletim sistemleri.

Aşağıdaki yazılımlar gereklidir:
* [Git istemci](https://git-scm.com/downloads).
* [Düğüm LTS](https://nodejs.org).
* `npm install -g yo`.
* `npm install -g generator-az-iot-gw-module`

## <a name="architecture"></a>Mimari

Azure IOT kenar platform yoğun uyarlar [Von Neumann mimarisi](https://en.wikipedia.org/wiki/Von_Neumann_architecture). Hangi tüm Azure IOT kenar mimarisi girişi işler ve bir çıktı üretir sistem anlamına gelir; ve tek tek her modül küçük bir giriş-çıkış alt olduğunu. Bu öğreticide, aşağıdaki iki modüller tanıtmaktadır:

1. Sanal alan modül [bırak](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) sinyal ve biçimlendirilmiş dönüştürür [JSON](https://en.wikipedia.org/wiki/JSON) ileti.
2. Alınan yazdırır bir modül [JSON](https://en.wikipedia.org/wiki/JSON) ileti.

Aşağıdaki resimde bu proje için tipik uçtan uca veri akışı görüntüler:

![Veri akışı üç modülleri arasında](media/iot-hub-iot-edge-create-module/dataflow.png "giriş: benzetimli bırak Modülü; İşlemci: Dönüştürücü Modülü; Çıktı: Yazıcı Modülü")

## <a name="set-up-the-environment"></a>Ortamını ayarlama
Aşağıda, ilk bırak dönüştürücü modülü JS ile yazmaya başlamak için ortamı hızlı bir şekilde ayarlamak nasıl gösteriyoruz.

### <a name="create-module-project"></a>Modül projesi oluşturma
1. Bir komut satırı penceresi açın, çalıştırmak `yo az-iot-gw-module`.
2. Modül projenizin başlatma tamamlamak için ekrandaki adımları izleyin.

### <a name="project-structure"></a>Proje yapısı
JS modül proje aşağıdaki bileşenlerden oluşur:

`modules`-Özelleştirilmiş JS modül kaynak dosyaları. Varsayılan değiştirme `sensor.js` ve `printer.js` kendi modülü dosyalarla.

`app.js`-Kenar örneği başlatmak için giriş dosyası.

`gw.config.json`-Edge tarafından yüklenmesi için modülleri özelleştirmek için yapılandırma dosyası.

`package.json`-Modülü projesi meta veri bilgilerini.

`README.md`-Temel belgeleri modülü projesi.


### <a name="package-file"></a>Paket dosyası

Bu `package.json` adı, sürüm, giriş, komut dosyaları, çalışma zamanı ve geliştirme bağımlılıklar içeren bir modülü projesi tarafından gerekli olan tüm meta veri bilgileri bildirir.

Aşağıdaki kod parçacığını bırak dönüştürücü örnek projesi için yapılandırma gösterilmektedir.
```json
{
  "name": "converter",
  "version": "1.0.0",
  "description": "BLE data converter sample for Azure IoT Edge.",
  "repository": {
    "type": "git",
    "url": "https://github.com/Azure-Samples/iot-edge-samples"
  },
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "author": "Microsoft Corporation",
  "license": "MIT",
  "dependencies": {
  },
  "devDependencies": {
    "azure-iot-gateway": "~1.1.3"
  }
}
```


### <a name="entry-file"></a>Girdi dosyası
`app.js` Kenar örneği başlatmak için biçimini tanımlar. Burada size herhangi bir değişiklik yapmak gerekmez.

```javascript
(function() {
  'use strict';

  const Gateway = require('azure-iot-gateway');
  let config_path = './gw.config.json';

  // node app.js
  if (process.argv.length < 2) {
    throw 'Calling pattern should be node app.js.';
  }

  const gw = new Gateway(config_path);
  gw.run();
})();
```

### <a name="interface-of-module"></a>Modülün arabirimi
Bir veri işleri için işlemci olarak Azure IOT kenar modülü davranabilirsiniz: girişi almak, işlemesi ve çıktı üretir.

Giriş (gibi hareket algılayıcısı) donanım verilerden bir ileti diğer modüller ya da (örneğin, düzenli aralıklarla bir Zamanlayıcı tarafından oluşturulan bir rastgele sayı) başka bir şey olabilir.

Giriş benzer bir çıkış, tetikleyebilir donanım davranış (gibi yanıp sönen LED), diğer modüller ya da (örneğin, yazdırma konsoluna) başka bir şey için bir ileti.

Modüller birbirleri ile kullanarak iletişim `message` nesnesi. **İçerik** , bir `message` istediğiniz verilerin herhangi bir tür temsil etme özelliğine sahip bir bayt dizisi. **Özellikler** mevcuttur `message` ve yalnızca bir dize-dize eşleme. Düşünebilirsiniz **özellikleri** bir HTTP isteğinin ya da meta veri dosyasının başlığı olarak.

JS Azure IOT kenar modülünde geliştirmek için gereken yöntemleri uygulayan yeni bir modül nesnesi oluşturmak ihtiyacınız `receive()`. Bu noktada, ayrıca isteğe bağlı uygulamayı seçebilir `create()` veya `start()`, veya `destroy()` de yöntemleri. Aşağıdaki kod parçacığını JS modül nesne iskelesini gösterir.

```javascript
'use strict';

module.exports = {
  broker: null,
  configuration: null,

  create: function (broker, configuration) {
    // Default implementation.
    this.broker = broker;
    this.configuration = configuration;

    return true;
  },

  start: function () {
    // Produce
  },

  receive: function (message) {
    // Consume
  },

  destroy: function () {
  }
};
```

### <a name="converter-module"></a>Dönüştürücü Modülü
| Girdi                    | İşlemci                              | Çıktı                 | Kaynak dosya            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Sıcaklık veri iletisi | Ayrıştırma ve yeni bir JSON ileti oluşturun | Yapı JSON iletisi | `converter.js` |

Bu modül tipik bir Azure IOT kenar modüldür. Diğer modüllerden sıcaklık iletileri kabul eder (bir donanım modülü veya bu bizim benzetimli bırak modülü durumda); ve (sıcaklık uyarı tetiklemesi ve benzeri için ihtiyacımız özelliğini ayarlama ileti kimliği ekleme dahil) yapılandırılmış bir JSON iletisi sıcaklık iletisinde normalleştirir.

```javascript
receive: function (message) {
  // Initialize the messageCount in global object at first time.
  if (!global.messageCount) {
    global.messageCount = 0;
  }

  // Read the content and properties objects from message.
  let rawContent = JSON.parse(Buffer.from(message.content).toString('utf8'));
  let rawProperties = message.properties;

  // Generate new properties object.
  let newProperties = {
    source: rawProperties.source,
    macAddress: rawProperties.macAddress,
    temperatureAlert: rawContent.temperature > 30 ? 'true' : 'false'
  };

  // Generate new content object.
  let newContent = {
    deviceId: 'Intel NUC Gateway',
    messageId: ++global.messageCount,
    temperature: rawContent.temperature
  };

  // Publish the new message to broker.
  this.broker.publish(
    {
      properties: newProperties,
      content: new Uint8Array(Buffer.from(JSON.stringify(newContent), 'utf8'))
    }
  );
},
```

### <a name="printer-module"></a>Yazıcı Modülü
| Girdi                          | İşlemci | Çıktı                     | Kaynak dosya          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Diğer modüllerden herhangi bir iletisi | Yok       | İleti konsola oturum | `printer.js` |

Bu modül basit ve açıklayıcı, hangi alınan iletiler (özelliği, içerik) terminal penceresine çıkarır.

```javascript
receive: function (message) {
  let properties = JSON.stringify(message.properties);
  let content = Buffer.from(message.content).toString('utf8');

  console.log(`printer.receive.properties - ${properties}`);
  console.log(`printer.receive.content - ${content}\n`);
}
```

### <a name="configuration"></a>Yapılandırma
Modülleri çalıştırmadan önce son adım, Azure IOT kenar yapılandırmak ve modülleri arasında bağlantı kurmak için ' dir.

İlk bildirmek ihtiyacımız bizim `node` (itibaren Azure IOT kenar destekler yükleyicilerini farklı dillerde) tarafından başvurulan yükleyicisi kendi `name` bölümlerde daha sonra.

```json
"loaders": [
  {
    "type": "node",
    "name": "node"
  }
]
```

Biz bizim yükleyicilerini bildirdikten sonra biz de bizim modüller de bildirmek gerekir. Benzer yükleyicileri bildirmek için bunlar ayrıca tarafından başvurulabilir kendi `name` özniteliği. Bir modül bildirirken (hangi önce tanımladığımız biri olmalıdır) kullanması gereken yükleyicisi belirtmek ihtiyacımız ve-(bizim modül normalleştirilmiş sınıf adı olmalıdır) için giriş noktası her modül. `simulated_device` Azure IOT kenar çekirdek çalışma zamanı paketinde bulunan bir yerel modül modülüdür. Dahil `args` JSON'dosyası, olsa bile `null`.

```json
"modules": [
  {
    "name": "simulated_device",
    "loader": {
      "name": "native",
      "entrypoint": {
        "module.path": "simulated_device"
      }
    },
    "args": {
      "macAddress": "01:02:03:03:02:01",
      "messagePeriod": 500
    }
  },
  {
    "name": "converter",
    "loader": {
      "name": "node",
      "entrypoint": {
        "main.path": "modules/converter.js"
      }
    },
    "args": null
  },
  {
    "name": "printer",
    "loader": {
      "name": "node",
      "entrypoint": {
        "main.path": "modules/printer.js"
      }
    },
    "args": null
  }
]
```

Yapılandırma sonunda bağlantı kurun. Her bağlantı ile ifade edilen `source` ve `sink`. Önceden tanımlanmış bir modül her ikisi de başvurmalısınız. Çıktı ileti `source` modülü girişi için iletilen `sink` modülü.

```json
"links": [
  {
    "source": "simulated_device",
    "sink": "converter"
  },
  {
    "source": "converter",
    "sink": "printer"
  }
]
```

## <a name="running-the-modules"></a>Modülleri çalıştırma
1. `npm install`
2. `npm start`

Uygulamayı istiyorsanız, basın `<Enter>` anahtarı.

> [!IMPORTANT]
> IOT kenar sonlandırmak için Ctrl + C kullanmak için önerilmez. Bu şekilde aşırı ciddiyeti işlemin neden olarak.
