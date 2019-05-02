---
title: Azure IoT Central uygulamasına gerçek bir cihaz ekleme | Microsoft Docs
description: Bir işleç olarak Azure IoT Central uygulamanıza gerçek bir cihaz ekleyin.
author: sandeeppujar
ms.author: sandeepu
ms.date: 04/23/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: e9378f8d2b31bfed4c464951c427b1e9d00b7893
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64699368"
---
# <a name="tutorial-add-a-real-device-to-your-azure-iot-central-application"></a>Öğretici: Azure IoT Central uygulamanıza gerçek bir cihaz ekleme

Bu öğretici Microsoft Azure IoT Central uygulamanıza gerçek bir cihaz eklemeyi ve bunu yapılandırmayı gösterir.

Bu öğretici iki bölümden oluşur:

1. İlk olarak, bir operatör olarak Azure IoT Central uygulamanıza gerçek bir cihaz eklemeyi ve yapılandırmayı öğrenirsiniz. Bu bölümün sonunda, ikinci bölümde kullanmak için bir bağlantı dizesi alırsınız.
2. Daha sonra, cihaz geliştirici olarak, gerçek cihazınızdaki kod hakkında bilgi edinirsiniz. Örnek kodun ilk bölümünden bağlantı dizesini eklersiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni gerçek cihaz ekleme
> * Gerçek cihazı yapılandırma
> * Gerçek cihaz için uygulamadan bağlantı dizesi alma
> * İstemci kodunun uygulamaya nasıl eşlendiğini anlama
> * Gerçek cihaz için istemci kodu yapılandırma

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, oluşturucunun Azure IoT Central uygulamasını oluşturmak için en az ilk oluşturucu öğreticisini tamamlaması gerekir:

* [Yeni bir cihaz türü belirleme](tutorial-define-device-type.md) (Gerekli)
* [Cihazınız için kurallar ve eylemler yapılandırma](tutorial-configure-rules.md) (İsteğe bağlı)
* [İşlecin görünümlerini özelleştirme](tutorial-customize-operator.md) (İsteğe bağlı)

Yükleme [Node.js](https://nodejs.org/) geliştirme makinenize 8.0.0 sürümü veya sonraki bir sürümü. Çalıştırabileceğiniz `node --version` sürümünüzü denetlemek için komut satırına. Node.js çeşitli işletim sistemleri için kullanılabilir.

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Uygulamanıza gerçek bir cihaz eklemek için, [Yeni bir cihaz türü belirleme](tutorial-define-device-type.md) öğreticisinde oluşturduğunuz **Bağlı Klima** cihaz şablonunu kullanırsınız.

1. İşleç olarak yeni bir cihaz eklemek için sol gezinti menüsünde **Device Explorer**’ı seçin:

   ![Bağlı klimayı gösteren Device Explorer sayfası](media/tutorial-add-device/explorer.png)

   **Device Explorer** gösterir **bağlı bir klima** cihaz şablonu ve sanal bir cihaz. Bir cihaz şablonu oluşturduğunuzda, IOT Central bir simülasyon cihazı otomatik olarak oluşturur.

2. Gerçek bağlı bir klima cihaz bağlanma başlamak için seçim **+**, ardından **gerçek**:

   ![Yeni, gerçek bir klima cihazı eklemeye başlama](media/tutorial-add-device/newreal.png)

3. (Küçük olmalıdır) cihaz kimliği girin veya önerilen cihaz kimliğini kullanması Ayrıca yeni cihazınız için ad da girebilirsiniz. Ardından **Oluştur**'u seçin.

   ![Cihazı yeniden adlandırma](media/tutorial-add-device/rename.png)

## <a name="configure-a-real-device"></a>Gerçek bir cihaz yapılandırma

Gerçek cihaz **Bağlı Klima** cihaz şablonundan oluşturulur. Cihazınızı yapılandırmak ve cihazınız hakkında bilgileri kaydetmek üzere özellik değerlerini ayarlamak için **Ayarlar**’ı kullanabilirsiniz.

1. **Ayarlar** sayfasında, **Sıcaklık Ayarla** ayar durumunun **güncelleştirme yok** olduğuna dikkat edin. Gerçek cihaz uygulamaya bağlanıp bu ayar üzerine işlem gerçekleştirdiğini tanıyana kadar bu durumda kalır.

    ![Ayarları gösterme eşitleniyor](media/tutorial-add-device/settingssyncing.png)

2. Üzerinde **özellikleri** sayfası, yeni, gerçek bir cihaz, her iki konum hizmeti ve son hizmet tarihi düzenlenebilir özelliklerdir. Cihaz uygulamaya bağlanana kadar seri numarası ve üretici yazılımı sürümü alanları boş kalır. Bu salt okunur değerleri, CİHAZDAN gönderilen ve düzenlenemez.

    ![Gerçek cihazın Cihaz Özellikleri](media/tutorial-add-device/setproperties1.png)

3. Gerçek cihazınız için **Ölçüler**, **Kurallar** ve **Pano** sayfalarını görüntüleyebilirsiniz.

## <a name="generate-connection-string"></a>Bağlantı dizesi oluştur

Bir cihaz geliştiricisinin gerçek cihazınız için *bağlantı dizesini* cihazda çalışan kodda eklemesi gerekir. Bağlantı dizesini, cihazın, uygulamanız için güvenli bir şekilde bağlanmasına olanak sağlar. Aşağıdaki adımlar, bağlantı dizesi oluşturmak ve Node.js kodunu istemcisini hazırla gösterir.

## <a name="prepare-the-client-code"></a>İstemci kodu hazırlama

Bu makaledeki örnek kodun yazıldığı [Node.js](https://nodejs.org/) ve yeterli kodu gösterir:

* Azure IoT Central uygulamanıza cihaz olarak bağlanın.
* Bağlı bir klima cihazı olarak sıcaklık telemetrisi gönderin.
* Azure IoT Central uygulamanıza cihaz özelliklerini gönderin.
* **Sıcaklık Ayarla** ayarını kullana bir işleci yanıtlayın.
* Azure IoT Central uygulamanızdan Echo komutunu işleyin.

Listelenen makaleleri [sonraki adımlar](#next-steps) bölümünde daha ayrıntılı örnekler yer ve diğer programlama dilini göster. Cihazların Azure IoT Central’a bağlanması hakkında daha fazla bilgi için [Cihaz bağlantısı](concepts-connectivity.md) adlı makaleye bakın.

Aşağıdaki adımlar [Node.js](https://nodejs.org/) örneğinin nasıl hazırlanacağını gösterir:

### <a name="get-the-device-connection-information"></a>Cihaz bağlantı bilgilerini alın

1. Uygulamanızdaki bir cihaz örneğinin bağlantı dizesi, IoT Central tarafından sağlanan cihaz bilgilerine göre oluşturulur.

   Gerçek bağlı klima cihazınızın ekranında **Bağlan**'ı seçin.

   ![Bağlantı bilgilerini görüntüleme bağlantısını gösteren cihaz sayfası](media/tutorial-add-device/connectionlink.png)

1. Cihaz bağlantısı sayfasında Not **kapsam kimliği**, **cihaz kimliği** ve **birincil anahtar** değerleri. Bu değerleri bir sonraki adımda kullanacaksınız.

   ![Bağlantı ayrıntıları](media/tutorial-add-device/device-connect.png)

### <a name="generate-the-connection-string"></a>Bağlantı dizesi oluştur

[!INCLUDE [iot-central-howto-connection-string](../../includes/iot-central-howto-connection-string.md)]

### <a name="prepare-the-nodejs-project"></a>Node.js projesi hazırlama

1. Adlı bir klasör oluşturun `connectedairconditioner` geliştirme makinenizde.

1. Komut satırı ortamınızda, oluşturduğunuz `connectedairconditioner` klasörüne gidin.

1. Node.js projenizi başlatmak için, tüm varsayılanları kabul ederek aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm init
      ```

1. Gerekli paketleri yüklemek için, aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Bir metin düzenleyicisi kullanarak, `connectedairconditioner` klasöründe **ConnectedAirConditioner.js** adlı bir dosya oluşturun.

1. Aşağıdaki `require` deyimlerini **ConnectedAirConditioner.js** dosyasının başlangıcına ekleyin:

    ```javascript
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    ```

1. Aşağıdaki değişken bildirimlerini dosyaya ekleyin:

    ```javascript
    var connectionString = '{your device connection string}';
    var targetTemperature = 0;
    var client = clientFromConnectionString(connectionString);
    ```

    > [!NOTE]
    > Yer tutucu `{your device connection string}` öğesini sonraki bir adımda güncelleştirirsiniz.

1. Şu ana kadar yaptığınız değişiklikleri kaydedin ancak dosyayı açık tutun.

## <a name="review-client-code"></a>İstemci kodu gözden geçirin

Önceki bölümde, Azure IoT Central uygulamanıza bağlanan bir uygulama için bir çatı Node.js projesi oluşturdunuz. Sonraki adım, kodu eklemektir:

* Azure IoT Central uygulamanıza bağlanın.
* Azure IoT Central uygulamanıza telemetri gönderin.
* Azure IoT Central uygulamanıza cihaz özelliklerini gönderin.
* Azure IoT Central uygulamanızdan ayarları alın.
* Azure IoT Central uygulamanızdan Echo komutunu işleyin.

1. Azure IoT Central uygulamanıza sıcaklık telemetrisi göndermek için, aşağıdaki kodu **ConnectedAirConditioner.js** dosyasına ekleyin:

    ```javascript
    // Send device telemetry.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var data = JSON.stringify({ temperature: temperature });
      var message = new Message(data);
      client.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

    Gönderdiğiniz JSON’deki alanın adı cihaz şablonunuzdaki sıcaklık telemetrisi için belirttiğiniz alanın adıyla eşleşmelidir. Bu örnekte, alanın adı **sıcaklıktır**.

1. **firmwareVersion** ve **serialNumber** gibi cihaz özelliklerini göndermek için şu tanımı ekleyin:

    ```javascript
    // Send device properties
    function sendDeviceProperties(twin) {
      var properties = {
        firmwareVersion: "9.75",
        serialNumber: "10001"
      };
      twin.properties.reported.update(properties, (errorMessage) => 
      console.log(` * Sent device properties ` + (errorMessage ? `Error: ${errorMessage.toString()}` : `(success)`)));
    }
    ```

1. Cihazınızın desteklediği **setTemperature** gibi ayarları tanımlamak için, aşağıdaki tanımı ekleyin:

    ```javascript
    // Add any settings your device supports
    // mapped to a function that is called when the setting is changed.
    var settings = {
      'setTemperature': (newValue, callback) => {
        // Simulate the temperature setting taking two steps.
        setTimeout(() => {
          targetTemperature = targetTemperature + (newValue - targetTemperature) / 2;
          callback(targetTemperature, 'pending');
          setTimeout(() => {
            targetTemperature = newValue;
            callback(targetTemperature, 'completed');
          }, 5000);
        }, 5000);
      }
    };
    ```

1. Azure IoT Central’dan gönderilen ayarları işlemek için, uygun cihaz kodunu bulan ve yürüten aşağıdaki işlevi ekleyin:

    ```javascript
    // Handle settings changes that come from Azure IoT Central via the device twin.
    function handleSettings(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (settings[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
            settings[setting](desiredChange[setting].value, (newValue, status, message) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  status: status,
                  desiredVersion: desiredChange.$version,
                  message: message
                }
              }
              twin.properties.reported.update(patch, (err) => console.log(`Sent setting update for ${setting}; ` +
                (err ? `error: ${err.toString()}` : `status: success`)));
            });
          }
        }
      });
    }
    ```

    Bu işlev:

    * İstenen bir özellik gönderen Azure IoT Central’ı izler.
    * Ayar değişikliğini işlemek için çağrılacak uygun işlevi bulur.
    * Azure IoT Central uygulamanıza bir onay gönderir.

1. Azure IoT Central uygulamanızdan gelen **echo** gibi bir komuta yanıt vermek için şu tanımı ekleyin:

    ```javascript
    // Respond to the echo command
    function onCommandEcho(request, response) {
      // Display console info
      console.log(' * Echo command received');
      // Respond
      response.send(10, 'Success', function (errorMessage) {});
    }
    ```

1. Azure IoT Central bağlantısını tamamlamak ve istemci kodundaki işlevleri bağlamak için aşağıdaki kodu ekleyin:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');
        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);
        // Setup device command callbacks
        client.onDeviceMethod('echo', onCommandEcho);
        // Get device twin from Azure IoT Central.
        client.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up
            sendDeviceProperties(twin);
            // Apply device settings and handle changes to device settings.
            handleSettings(twin);
          }
        });
      }
    };

    client.open(connectCallback);
    ```

1. Şu ana kadar yaptığınız değişiklikleri kaydedin ancak dosyayı açık tutun.

## <a name="configure-client-code"></a>İstemci kodu yapılandırma

<!-- Add the connection string to the sample code, build, and run -->
İstemci kodunuzu Azure IoT Central uygulamanıza bağlanmak için yapılandırmak üzere, gerçek cihazınız için bu öğreticinin başında not ettiğiniz bağlantı dizesini eklemeniz gerekir.

1. **ConnectedAirConditioner.js** dosyasında, aşağıdaki kod satırını bulun:

    ```javascript
    var connectionString = '{your device connection string}';
    ```

1. `{your device connection string}` değerini gerçek cihazınızın bağlantı dizesiyle değiştirin. Bir önceki adımda oluşturulan bağlantı dizesi kopyaladığınız.

1. Değişiklikleri **ConnectedAirConditioner.js** dosyasına kaydedin.

1. Örneği çalıştırmak için, komut satırı ortamınıza aşağıdaki komutu girin:

    ```cmd/sh
    node ConnectedAirConditioner.js
    ```

    > [!NOTE]
    > Bu komutu çalıştırdığınızda `connectedairconditioner` klasöründe olduğunuzdan emin olun.

1. Uygulama çıkışı konsola yazdırır:

   ![İstemci uygulaması çıkışı](media/tutorial-add-device/output.png)

1. 30 saniyeden sonra, cihaz **Ölçüm** sayfasında telemetriyi görürsünüz:

   ![Real ~~telemetry](media/tutorial-add-device/realtelemetry.png)

1. **Ayarlar** sayfasında, ayarın şimdi eşitlenmiş olduğunu görebilirsiniz. Cihaz ilk bağlandığında, ayar değerini aldı ve değişikliği onayladı:

   ![Ayarlar eşitlendi](media/tutorial-add-device/settingsynced.png)

1. **Ayarlar** sayfasında, cihaz sıcaklığını **95** olarak ayarlayıp **Cihazı güncelleştir**’i seçin. Örnek uygulamanız bu değişikliği alır ve işler:

   ![Ayarı alma ve işleme](media/tutorial-add-device/receivesetting.png)

   > [!NOTE]
   > İki “ayar güncelleştirmesi” iletisi vardır. `pending` durumu gönderildiğinde bir tane ve `completed` durumu gönderildiğinde bir tane.

1. **Ölçümler** sayfasında cihazın daha yüksek sıcaklık değerleri gönderdiğini görebilirsiniz:

    ![Sıcaklık telemetrisi şimdi daha yüksek](media/tutorial-add-device/highertemperature.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="nextstepaction"]
> * Yeni gerçek cihaz ekleme
> * Yeni cihazı yapılandırma
> * Gerçek cihaz için uygulamadan bağlantı dizesi alma
> * İstemci kodunun uygulamaya nasıl eşlendiğini anlama
> * Gerçek cihaz için istemci kodu yapılandırma

Azure IOT Central uygulamanıza gerçek bir cihaz bağlandığınıza göre önerilen sonraki adımlar şunlardır:

Bir operatör olarak, aşağıdakileri yapmayı öğrenebilirsiniz:

* [Cihazlarınızı yönetme](howto-manage-devices.md)
* [Cihaz kümelerini kullanma](howto-use-device-sets.md)
* [Özel analiz oluşturma](howto-use-device-sets.md)

Cihaz geliştiricisi olarak, şunları yapmayı öğrenebilirsiniz:

* [Hazırlama ve DevKit cihazı (C) bağlayın](howto-connect-devkit.md)
* [Hazırlama ve Raspberry Pi'yi (Python) bağlanma](howto-connect-raspberry-pi-python.md)
* [Hazırlama ve Raspberry Pi'yi bağlanın (C#)](howto-connect-raspberry-pi-csharp.md)
* [Hazırlama ve Windows 10 IOT core cihazı bağlayın (C#)](howto-connect-windowsiotcore.md)
* [Genel bir Node.js istemcisini Azure IoT Central uygulamanıza bağlama](howto-connect-nodejs.md)
