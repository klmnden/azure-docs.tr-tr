---
title: Azure IOT hub'ı modül kimlik ve modül ikizi ile (Node.js) kullanmaya başlayın | Microsoft Docs
description: Modül kimliği oluşturma ve Node.js için IOT SDK'ları kullanarak modül ikizi güncelleştirme hakkında bilgi edinin.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: node
ms.topic: conceptual
ms.date: 04/26/2018
ms.openlocfilehash: 312d3abad2ee2c9e668f8b354aaba96f8a652698
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59259295"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-nodejs-back-end-and-nodejs-device"></a>Node.js arka ucu ile Node.js cihaz IOT hub'ı modül kimlik ve modül ikizi ile çalışmaya başlama

> [!NOTE]
> [Modül kimlikleri ve modül ikizleri](iot-hub-devguide-module-twins.md), Azure IoT Hub cihaz kimliğine ve cihaz ikizine benzer, ancak daha hassas ayrıntı düzeyi sağlar. Azure IoT Hub cihaz kimliği ve cihaz ikizi, arka uç uygulamasının bir cihaz yapılandırmasına imkan tanıyıp cihazın koşullarına yönelik görünürlük sağlarken, modül kimliği ve modül ikizi de bir cihazın tek tek bileşenleri için bu özellikleri sağlar. İşletim sistemi tabanlı cihazlar veya üretici yazılımı cihazları gibi, birden fazla bileşen içeren ve bu özelliklere sahip cihazlarda her bir bileşen için yalıtılmış yapılandırma ve koşullara olanak sağlar.

Bu öğreticinin sonunda iki Node.js uygulamaları vardır:

* Cihaz ve modül istemcilerinizi bağlamak için bir cihaz kimliği, bir modül kimliği ve ilişkili güvenlik anahtarı oluşturan **CreateIdentities**.

* Güncelleştirilmiş modül ikizi tarafından raporlanan özelliklerini IoT Hub’ınıza gönderen **UpdateModuleTwinReportedProperties**.

> [!NOTE]
> Cihazlar ve çözüm arka ucunuz çalıştırılacak hem uygulamalar oluşturmak için kullanabileceğiniz Azure IOT SDK'ları hakkında daha fazla bilgi için bkz. [Azure IOT SDK'ları](iot-hub-devguide-sdks.md).

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. (Hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)
* IOT hub'ı.
* Son yükleme [Node.js SDK'sı](https://github.com/Azure/azure-iot-sdk-node).

IoT hub'ınızı oluşturdunuz ve bu öğreticinin geri kalanını tamamlamak için gereken ana bilgisayar adı ve IoT Hub bağlantı dizesine sahipsiniz.

## <a name="create-a-device-identity-and-a-module-identity-in-iot-hub"></a>Bir cihaz kimliği ve bir modül kimliği, IOT hub'ı oluşturma

Bu bölümde, IOT hub'ınızdaki kimlik kayıt defterinde bir cihaz kimliği ve bir modül kimliği oluşturan bir Node.js uygulaması oluşturun. Kimlik kayıt defterinde girişi olmayan bir cihaz veya modül, IoT hub'ına bağlanamaz. Daha fazla bilgi için "Kimlik kayıt defteri" bölümüne bakın. [IOT Hub Geliştirici kılavuzunun](iot-hub-devguide-identity-registry.md). Bu konsol uygulamasını çalıştırdığınızda, hem cihaz hem de modül için benzersiz bir kimlik ve anahtar oluşturur. Cihazınız ve modülünüz, IoT Hub’ına cihazdan buluta iletileri gönderdiğinde kendisini tanımlamak için bu değerleri kullanır. Kimlikler büyük/küçük harfe duyarlıdır.

1. Kodunuzu saklamak için bir dizin oluşturun.

2. Bu dizin içinde ilk çalıştırma **npm init -y** boş bir package.json varsayılan değerlerle oluşturma. Bu işlev, kodunuz için proje dosyasıdır.

3. Çalıştırma **npm yükleme -S azure-iothub\@modülleri Önizleme** hizmeti SDK'sını yüklemek için içinde **node_modules** alt.

    > [!NOTE]
    > Alt ad node_modules "düğümü kitaplığı" auto'yu word modülü kullanır. IOT hub'ı olan modüller ilgisi terimini buraya sahiptir.

4. Dizininizde aşağıdaki .js dosyası oluşturun. Bu çağrı **add.js**. Kopyalayıp hub adını ve hub bağlantı dizesini yapıştırın.

    ```javascript
    var Registry = require('azure-iothub').Registry;
    var uuid = require('uuid');
    // Copy/paste your connection string and hub name here
    var serviceConnectionString = '<hub connection string from portal>';
    var hubName = '<hub name>.azure-devices.net';
    // Create an instance of the IoTHub registry
    var registry = Registry.fromConnectionString(serviceConnectionString);
    // Insert your device ID and moduleId here.
    var deviceId = 'myFirstDevice';
    var moduleId = 'myFirstModule';
    // Create your device as a SAS authentication device
    var primaryKey = new Buffer(uuid.v4()).toString('base64');
    var secondaryKey = new Buffer(uuid.v4()).toString('base64');
    var deviceDescription = {
      deviceId: deviceId,
      status: 'enabled',
      authentication: {
        type: 'sas',
        symmetricKey: {
          primaryKey: primaryKey,
          secondaryKey: secondaryKey
        }
      }
    };

    // First, create a device identity
    registry.create(deviceDescription, function(err) {
      if (err) {
        console.log('Error creating device identity: ' + err);
        process.exit(1);
      }
      console.log('device connection string = "HostName=' + hubName + ';DeviceId=' + deviceId + ';SharedAccessKey=' + primaryKey + '"');

      // Then add a module to that device
      registry.addModule({ deviceId: deviceId, moduleId: moduleId }, function(err) {
        if (err) {
          console.log('Error creating module identity: ' + err);
          process.exit(1);
        }

        // Finally, retrieve the module details from the hub so we can construct the connection string
        registry.getModule(deviceId, moduleId, function(err, foundModule) {
          if (err) {
            console.log('Error getting module back from hub: ' + err);
            process.exit(1);
          }
          console.log('module connection string = "HostName=' + hubName + ';DeviceId=' + foundModule.deviceId + ';ModuleId='+foundModule.moduleId+';SharedAccessKey=' + foundModule.authentication.symmetricKey.primaryKey + '"');
          process.exit(0);
        });
      });
    });

    ```

Bu uygulama Kimliğine sahip bir cihaz kimliği oluşturan **myFirstDevice** ve Kimliğe sahip bir modül kimliği **myFirstModule** cihaz altında **myFirstDevice**. (Bu modül kimliği, kimlik kayıt defterinde zaten varsa, kod yalnızca mevcut modül bilgilerini alır.) Bu durumda uygulama, bu kimliğin birincil anahtarını görüntüler. IoT hub'ınıza bağlanmak için sanal modül uygulamasında bu anahtarı kullanırsınız.

Bu düğüm add.js kullanarak çalıştırın. Bu size bir bağlantı dizesi, cihaz kimliği ve başka bir modül kimliğiniz için sunar.

> [!NOTE]
> IoT Hub kimlik kayıt defteri yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz ve modül kimliklerini depolar. Kimlik kayıt defteri, cihaz kimliklerini ve anahtarlarını güvenlik kimlik bilgileri olarak kullanmak için depolar. Kimlik kayıt defterinin her cihaz için depoladığı etkin/devre dışı bayrağını kullanarak, ilgili cihaza erişimi devre dışı bırakabilirsiniz. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Modül kimlikleri için etkin/devre dışı bayrağı yoktur. Daha fazla bilgi için [IOT Hub Geliştirici kılavuzunun](iot-hub-devguide-identity-registry.md).

## <a name="update-the-module-twin-using-nodejs-device-sdk"></a>Node.js cihaz SDK'sını kullanarak modül ikizi güncelleştir

Bu bölümde, oluşturduğunuz bir Node.js uygulamasını sanal Cihazınızda modül ikizi güncelleştirmeleri bildirilen özellikler.

1. **Modülü bağlantı dizenizi alma** --oturum [Azure portalında](https://portal.azure.com/). IoT Hub’ınıza gidin ve IoT Cihazları’na tıklayın. Bul myFirstDevice, açık myFirstModule göreceksiniz başarıyla oluşturuldu. Modül bağlantı dizesini kopyalayın. Sonraki adımda gerekecektir.

   ![Azure portalı modül ayrıntısı](./media/iot-hub-node-node-module-twin-getstarted/module-detail.png)

2. Benzer şekilde, Yukarıdaki adımda yaptığınız, cihaz kodunuz için bir dizin oluşturun ve bunu başlatmak ve cihaz SDK'sını yüklemek için NPM kullanın (**npm yükleme -S azure-iot-device-amqp\@modülleri Önizleme**).

   > [!NOTE]
   > Npm yükleme komutu düşünüyor yavaş. Sabırlı olun, onu bir paket deposundaki kodu çok sayıda çekiyor.

   > [!NOTE]
   > Npm hata bildiren bir hata görürseniz! kayıt defteri hata ayrıştırma json, bunu yoksaymak güvenlidir. Npm hata bildiren bir hata görürseniz! kayıt defteri hata ayrıştırma json, bunu yoksaymak güvenlidir.

3. Twin.js adlı bir dosya oluşturun. Kopyalayın ve yapıştırın, modül kimliği dizesi.

    ```javascript
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-amqp').Amqp;
    // Copy/paste your module connection string here.
    var connectionString = '<insert module connection string here>';
    // Create a client using the Amqp protocol.
    var client = Client.fromConnectionString(connectionString, Protocol);
    client.on('error', function (err) {
      console.error(err.message);
    });
    // connect to the hub
    client.open(function(err) {
      if (err) {
        console.error('error connecting to hub: ' + err);
        process.exit(1);
      }
      console.log('client opened');
    // Create device Twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('error getting twin: ' + err);
          process.exit(1);
        }
        // Output the current properties
        console.log('twin contents:');
        console.log(twin.properties);
        // Add a handler for desired property changes
        twin.on('properties.desired', function(delta) {
            console.log('new desired properties received:');
            console.log(JSON.stringify(delta));
        });
        // create a patch to send to the hub
        var patch = {
          updateTime: new Date().toString(),
          firmwareVersion:'1.2.1',
          weather:{
            temperature: 72,
            humidity: 17
          }
        };
        // send the patch
        twin.properties.reported.update(patch, function(err) {
          if (err) throw err;
          console.log('twin state reported');
        });
      });
    });
    ```

4. Şimdi, bu komutla çalıştırın **düğüm twin.js**.

    ```
    F:\temp\module_twin>node twin.js
    client opened
    twin contents:
    { reported: { update: [Function: update], '$version': 1 },
      desired: { '$version': 1 } }
    new desired properties received:
    {"$version":1}
    twin state reported
    ```

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [Cihaz yönetimi ile çalışmaya başlama](iot-hub-node-node-device-management-get-started.md)

* [IOT Edge'i kullanmaya başlama](../iot-edge/tutorial-simulate-device-linux.md)