---
title: Azure IoT Central uygulamasına gerçek bir cihaz ekleme | Microsoft Docs
description: Bir işleç olarak Azure IoT Central uygulamanıza gerçek bir cihaz ekleyin.
author: sandeeppujar
ms.author: sandeepu
ms.date: 04/16/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: dd68b65825c9c22453e0191d42a0fcce3b65ca64
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236095"
---
# <a name="tutorial-add-a-real-device-to-your-azure-iot-central-application"></a>Öğretici: Azure IoT Central uygulamanıza gerçek bir cihaz ekleme

Bu öğretici Microsoft Azure IoT Central uygulamanıza gerçek bir cihaz eklemeyi ve bunu yapılandırmayı gösterir.

Bu öğretici iki bölümden oluşur:

1. İlk olarak, bir operatör olarak Azure IoT Central uygulamanıza gerçek bir cihaz eklemeyi ve yapılandırmayı öğrenirsiniz. Bu bölümün sonunda, ikinci bölümde kullanmak için bir bağlantı dizesi alırsınız.
2. Daha sonra, cihaz geliştirici olarak, gerçek cihazınızdaki kod hakkında bilgi edinirsiniz. Örnek kodun ilk bölümünden bağlantı dizesini eklersiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni gerçek cihaz ekleme
> * Yeni cihazı yapılandırma
> * Gerçek cihaz için uygulamadan bağlantı dizesi alma
> * İstemci kodunun uygulamaya nasıl eşlendiğini anlama
> * Gerçek cihaz için istemci kodu yapılandırma

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, oluşturucunun Azure IoT Central uygulamasını oluşturmak için en az ilk oluşturucu öğreticisini tamamlaması gerekir:

* [Yeni bir cihaz türü belirleme](tutorial-define-device-type.md) (Gerekli)
* [Cihazınız için kurallar ve eylemler yapılandırma](tutorial-configure-rules.md) (İsteğe bağlı)
* [İşlecin görünümlerini özelleştirme](tutorial-customize-operator.md) (İsteğe bağlı)

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Uygulamanıza gerçek bir cihaz eklemek için, [Yeni bir cihaz türü belirleme](tutorial-define-device-type.md) öğreticisinde oluşturduğunuz **Bağlı Klima** cihaz şablonunu kullanırsınız.

1. İşleç olarak yeni bir cihaz eklemek için sol gezinti menüsünde **Device Explorer**’ı seçin:

   ![Bağlı klimayı gösteren Device Explorer sayfası](media/tutorial-add-device/explorer.png)

   **Device Explorer**, **Bağlı Klima** cihaz şablonunu ve oluşturucu cihaz şablonunu oluşturduğunda otomatik olarak eklenen simülasyon cihazını gösterir.

2. Gerçek bir bağlı klima cihazını bağlamaya başlamak için, **Yeni** ve ardından **Gerçek** seçeneklerini belirleyin:

   ![Yeni, gerçek bir klima cihazı eklemeye başlama](media/tutorial-add-device/newreal.png)

3. İsteğe bağlı olarak, cihaz adını seçip değeri düzenleyerek yeni cihazınızı yeniden adlandırabilirsiniz:

   ![Cihazı yeniden adlandırma](media/tutorial-add-device/rename.png)

## <a name="configure-a-real-device"></a>Gerçek bir cihaz yapılandırma

Gerçek cihaz **Bağlı Klima** cihaz şablonundan oluşturulur. Oluşturucu olarak, cihazınızı yapılandırmak ve cihazınız hakkında bilgileri kaydetmek üzere özellik değerlerini ayarlamak için **Ayarlar**’ı kullanabilirsiniz.

1. **Ayarlar** sayfasında, **Sıcaklık Ayarla** ayar durumunun **güncelleştirme yok** olduğuna dikkat edin. Gerçek cihaz bağlanıp bu ayar üzerine işlem gerçekleştirdiğini tanıyana kadar bu durumda kalır:

    ![Ayarları gösterme eşitleniyor](media/tutorial-add-device/settingssyncing.png)

2. Yeni, gerçek bağlı klima cihazınızın **Özellikler** sayfasında, **Seri Numarası**’nı **rcac0010**, ve **Bellenim sürümü**’nü 9.75 olarak ayarlayın. Ardından **Kaydet**'i seçin:

    ![Gerçek cihaz özelliklerini ayarlayın](media/tutorial-add-device/setproperties.png)

3. Oluşturucu olarak, gerçek cihazınız için **Ölçüler**, **Kurallar** ve **Pano** sayfalarını görüntüleyebilirsiniz.

## <a name="get-connection-string-for-real-device-from-application"></a>Gerçek cihaz için uygulamadan bağlantı dizesi alma

Bir cihaz geliştiricisinin gerçek cihazınız için *bağlantı dizesini* cihazda çalışan kodda eklemesi gerekir. Bağlantı dizesi cihazın Azure IoT Central uygulamanıza güvenli bir şekilde bağlanmasını sağlar. Her cihaz örneğinin benzersiz bir bağlantı dizesi vardır. Aşağıdaki adımlar uygulamanızdaki bir cihaz örneği için bağlantı dizesini bulmayı gösterir:

1. Gerçek bağlı klima cihazınızın **Cihaz** ekranında, **Bu cihazı bağla**’yı seçin:

    ![Bağlantı bilgilerini görüntüleme bağlantısını gösteren cihaz sayfası](media/tutorial-add-device/connectionlink.png)

2. **Bağlan** sayfasında, **Birincil bağlantı dizesini** kopyalayın ve kaydedin. Bu değeri bu öğreticinin ikinci yarısında kullanırsınız. Bir cihaz geliştirici bu değeri cihazda çalışan istemci uygulamasında kullanır:

    ![Bağlantı dizesi değerleri](media/tutorial-add-device/connectionstring.png)

## <a name="prepare-the-client-code"></a>İstemci kodu hazırlama

Bu makaledeki örnek kod [Node.js](https://nodejs.org/) ile yazılmıştır ve aşağıdakiler için yeterli kodu gösterir:

* Azure IoT Central uygulamanıza cihaz olarak bağlanın.
* Bağlı bir klima cihazı olarak sıcaklık telemetrisi gönderin.
* **Sıcaklık Ayarla** ayarını kullana bir işleci yanıtlayın.

[Sonraki Adımlar](#next-steps) bölümünde başvurulan “Nasıl Yapılır” makaleleri daha ayrıntılı örnekler sağlar ve diğer programlama dillerinin kullanımını gösterir. Cihazların Azure IoT Central’a bağlanması hakkında daha fazla bilgi için [Cihaz bağlantısı](concepts-connectivity.md) adlı makaleye bakın.

Aşağıdaki adımlar [Node.js](https://nodejs.org/) örneğinin nasıl hazırlanacağını gösterir:

1. Makinenize [Node.js](https://nodejs.org/) sürüm 4.0.x veya üzerini yükleyin. Node.js çeşitli işletim sistemleri için kullanılabilir.

2. Makinenizde `connectedairconditioner` adlı bir klasör oluşturun.

3. Komut satırı ortamınızda, oluşturduğunuz `connectedairconditioner` klasörüne gidin.

4. Node.js projenizi başlatmak için, tüm varsayılanları kabul ederek aşağıdaki komutu çalıştırın:

   ```cmd/sh
   npm init
   ```

5. Gerekli paketleri yüklemek için, aşağıdaki komutu çalıştırın:

   ```cmd/sh
   npm install azure-iot-device azure-iot-device-mqtt --save
   ```

6. Bir metin düzenleyicisi kullanarak, `connectedairconditioner` klasöründe **ConnectedAirConditioner.js** adlı bir dosya oluşturun.

7. Aşağıdaki `require` deyimlerini **ConnectedAirConditioner.js** dosyasının başlangıcına ekleyin:

   ```javascript
   'use strict';

   var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
   var Message = require('azure-iot-device').Message;
   var ConnectionString = require('azure-iot-device').ConnectionString;
   ```

8. Aşağıdaki değişken bildirimlerini dosyaya ekleyin:

   ```javascript
   var connectionString = '{your device connection string}';
   var targetTemperature = 0;
   var client = clientFromConnectionString(connectionString);
   ```

   > [!NOTE]
   > Yer tutucu `{your device connection string}` öğesini sonraki bir adımda güncelleştirirsiniz.

9. Şu ana kadar yaptığınız değişiklikleri kaydedin ancak dosyayı açık tutun.

## <a name="understand-how-client-code-maps-to-the-application"></a>İstemci kodunun uygulamaya nasıl eşlendiğini anlama

Önceki bölümde, Azure IoT Central uygulamanıza bağlanan bir uygulama için bir çatı Node.js projesi oluşturdunuz. Bu bölümde şunun için kodu eklersiniz:

* Azure IoT Central uygulamanıza bağlanın.
* Azure IoT Central uygulamanıza telemetri gönderin.
* Azure IoT Central uygulamanızdan ayarları alın.

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

2. Cihazınızın desteklediği **setTemperature** gibi ayarları tanımlamak için, aşağıdaki tanımı ekleyin:

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

3. Azure IoT Central’dan gönderilen ayarları işlemek için, uygun cihaz kodunu bulan ve yürüten aşağıdaki işlevi ekleyin:

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

4. Azure IoT Central bağlantısını tamamlamak ve istemci kodundaki işlevleri bağlamak için aşağıdaki kodu ekleyin:

   ```javascript
   // Handle device connection to Azure IoT Central.
   var connectCallback = (err) => {
     if (err) {
       console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
     } else {
       console.log('Device successfully connected to Azure IoT Central');
        // Send telemetry measurements to Azure IoT Central every 1 second.
       setInterval(sendTelemetry, 1000);
        // Get device twin from Azure IoT Central.
       client.getTwin((err, twin) => {
         if (err) {
           console.log(`Error getting device twin: ${err.toString()}`);
         } else {
           // Apply device settings and handle changes to device settings.
           handleSettings(twin);
         }
       });
     }
   };

   client.open(connectCallback);
   ```

5. Şu ana kadar yaptığınız değişiklikleri kaydedin ancak dosyayı açık tutun.

## <a name="configure-client-code-for-the-real-device"></a>Gerçek cihaz için istemci kodu yapılandırma

<!-- Add the connection string to the sample code, build, and run -->
İstemci kodunuzu Azure IoT Central uygulamanıza bağlanmak için yapılandırmak üzere, gerçek cihazınız için bu öğreticinin başında not ettiğiniz bağlantı dizesini eklemeniz gerekir.

1. **ConnectedAirConditioner.js** dosyasında, aşağıdaki kod satırını bulun:

   ```javascript
   var connectionString = '{your device connection string}';
   ```

2. `{your device connection string}` değerini gerçek cihazınızın bağlantı dizesiyle değiştirin. “Uygulamadan gerçek cihaz için bağlantı dizesini alma” bölümünün sonundaki bağlantı dizesini not ettiniz.

3. Değişiklikleri **ConnectedAirConditioner.js** dosyasına kaydedin.

4. Örneği çalıştırmak için, komut satırı ortamınıza aşağıdaki komutu girin:

   ```cmd/sh
   node ConnectedAirConditioner.js
   ```

   > [!NOTE]
   > Bu komutu çalıştırdığınızda `connectedairconditioner` klasöründe olduğunuzdan emin olun.

5. Uygulama çıkışı konsola yazdırır:

   ![İstemci uygulaması çıkışı](media/tutorial-add-device/output.png)

6. 30 saniyeden sonra, cihaz **Ölçüm** sayfasında telemetriyi görürsünüz:

   ![Gerçek telemetri](media/tutorial-add-device/realtelemetry.png)

7. **Ayarlar** sayfasında, ayarın şimdi eşitlenmiş olduğunu görebilirsiniz. Cihaz ilk bağlandığında, ayar değerini aldı ve değişikliği onayladı:

   ![Ayarlar eşitlendi](media/tutorial-add-device/settingsynced.png)

8. **Ayarlar** sayfasında, cihaz sıcaklığını **95** olarak ayarlayıp **Cihazı güncelleştir**’i seçin. Örnek uygulamanız bu değişikliği alır ve işler:

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

Artık Azure IoT Central uygulamanıza gerçek bir cihaz bağladığınıza göre, önerilen sonraki adımlar şunlardır:

Bir operatör olarak, aşağıdakileri yapmayı öğrenebilirsiniz:

* [Cihazlarınızı yönetme](howto-manage-devices.md)
* [Cihaz kümelerini kullanma](howto-use-device-sets.md)
* [Özel analiz oluşturma](howto-create-analytics.md)

Cihaz geliştiricisi olarak, şunları yapmayı öğrenebilirsiniz:

* [DevKit’i hazırlama ve bağlama](howto-connect-devkit.md)
* [Raspberry Pi'yi hazırlama ve bağlama](howto-connect-raspberry-pi-python.md)
* [Genel bir Node.js istemcisini Azure IoT Central uygulamanıza bağlama](howto-connect-nodejs.md)