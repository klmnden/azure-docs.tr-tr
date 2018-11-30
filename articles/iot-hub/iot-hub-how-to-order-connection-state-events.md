---
title: Azure Cosmos DB kullanarak Azure IOT Hub aracılığıyla sipariş cihaz bağlantısı etkinlikleri | Microsoft Docs
description: Bu makalede, sipariş ve en son bağlantı durumunu korumak üzere Azure Cosmos DB kullanarak Azure IOT hub cihaz bağlantısı etkinlikleri kaydetmek açıklanır
services: iot-hub
ms.service: iot-hub
author: ash2017
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: asrastog
ms.openlocfilehash: 77615705ade42a2afcc8e3a9f662b0551a2411fd
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582464"
---
# <a name="order-device-connection-events-from-azure-iot-hub-using-azure-cosmos-db"></a>Bağlantı olayları Azure Cosmos DB kullanarak Azure IOT hub'a cihaz sipariş

Azure Event Grid, olay tabanlı uygulamalar oluşturmanıza ve kolayca işletme çözümlerinizi IOT olayları tümleştirmenize yardımcı olur. Bu makalede izleme ve en son cihaz bağlantı durumu Cosmos DB'de depolamak için kullanılan bir kurulum size kılavuzluk eder. Bağlı cihaz ve cihazın bağlantısı olayları kullanılabilir sıra numarası kullanın ve en son durumu Cosmos DB'de depolamak ederiz. Cosmos DB koleksiyonunda karşı yürütülen bir uygulama mantığı bir saklı yordam kullanmak için kullanacağız.

Sıra numarası onaltılık düzendeki bir sayıyı bir dize gösterimidir. Dize karşılaştırma büyük numarasını belirlemek için kullanabilirsiniz. Onaltılık dize dönüştürüyorsanız sayının 256 bitlik bir sayı olması. Sıra numarası kesin olarak artmaktadır ve en son olay diğer olayları daha yüksek bir değer alacaktır. Sık varsa cihaz bağlanır bağlantısını keser ve Azure Event Grid olayların sırası desteklemediğinden, yalnızca en son olay bir aşağı akış eylemi tetiklemek için kullanılır sağlamak istediğinizde bu kullanışlıdır.

## <a name="prerequisites"></a>Önkoşullar

* Etkin bir Azure hesabı. Hesabınız yoksa [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/pricing/free-trial/).

* Etkin bir Azure Cosmos DB SQL API hesabı. Bir henüz oluşturmadıysanız bkz [bir veritabanı hesabı oluşturma](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-dotnet#create-a-database-account) kılavuz.

* Veritabanı koleksiyonu. Bkz: [bir koleksiyon Ekle](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-dotnet#add-a-collection) kılavuz.

* Azure'da bir IoT Hub'ı. Henüz oluşturmadıysanız, yönergeler için bkz. [IoT Hub'ı kullanmaya başlama](../iot-hub/iot-hub-csharp-csharp-getstarted.md). 

## <a name="create-a-stored-procedure"></a>Bir saklı yordam oluşturma

İlk olarak, bir saklı yordam oluşturmak ve ayarlamak için gelen olayların sıra numaraları karşılaştırır ve en son olay cihaz başına veritabanında kaydeden bir mantığının çalışması ayarlayın.

1. Cosmos DB SQL API içinde seçin **Veri Gezgini** > **öğeleri** > **yeni saklı yordam**.

   ![Saklı yordam oluştur](./media/iot-hub-how-to-order-connection-state-events/create-stored-procedure.png)

2. Bir saklı yordam kimliği girin ve "saklı yordam gövde" aşağıdakini yapıştırın. Bu kodu mevcut kodlar saklı yordam gövde yerini aldığını unutmayın. Bu kod, cihaz kimliği başına bir satır tutar ve en yüksek sıra numarası tanımlayarak bu cihaz kimliği son bağlantı durumunu kaydeder. 

    ```javascript
    // SAMPLE STORED PROCEDURE
    function UpdateDevice(deviceId, moduleId, hubName, connectionState, connectionStateUpdatedTime, sequenceNumber) {
      var collection = getContext().getCollection();
      var response = {};
      
      var docLink = getDocumentLink(deviceId, moduleId);

      var isAccepted = collection.readDocument(docLink, function(err, doc) {
        if (err) {
          console.log('Cannot find device ' + docLink + ' - ');
          createDocument();
        } else {
          console.log('Document Found - ');
          replaceDocument(doc);
        }
      });

      function replaceDocument(document) {
        console.log(
          'Old Seq :' +
            document.sequenceNumber +
            ' New Seq: ' +
            sequenceNumber +
            ' - '
        );
        if (sequenceNumber > document.sequenceNumber) {
          document.connectionState = connectionState;
          document.connectionStateUpdatedTime = connectionStateUpdatedTime;
          document.sequenceNumber = sequenceNumber;

          console.log('replace doc - ');

          isAccepted = collection.replaceDocument(docLink, document, function(
            err,
            updated
          ) {
            if (err) {
              getContext()
                .getResponse()
                .setBody(err);
            } else {
              getContext()
                .getResponse()
                .setBody(updated);
            }
          });
        } else {
          getContext()
            .getResponse()
            .setBody('Old Event - current: ' + document.sequenceNumber + ' Incoming: ' + sequenceNumber);
        }
      }
      function createDocument() {
        document = {
          id: deviceId + '-' + moduleId,
          deviceId: deviceId,
          moduleId: moduleId,
          hubName: hubName,
          connectionState: connectionState,
          connectionStateUpdatedTime: connectionStateUpdatedTime,
          sequenceNumber: sequenceNumber
        };
        console.log('Add new device - ' + collection.getAltLink());
        isAccepted = collection.createDocument(
          collection.getAltLink(),
          document,
          function(err, doc) {
            if (err) {
              getContext()
                .getResponse()
                .setBody(err);
            } else {
              getContext()
                .getResponse()
                .setBody(doc);
            }
          }
        );
      }

      function getDocumentLink(deviceId, moduleId) {
        return collection.getAltLink() + '/docs/' + deviceId + '-' + moduleId;
      }
    }
    ```

3. Saklı yordamı kaydedin: 

    ![saklı yordamı kaydedin](./media/iot-hub-how-to-order-connection-state-events/save-stored-procedure.png)

## <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

İlk olarak, bir mantıksal uygulama oluşturun ve sanal makineniz için kaynak grubunu izleyen bir Event Grid tetikleyicisi ekleyin. 

### <a name="create-a-logic-app-resource"></a>Mantıksal uygulama kaynağı oluşturma

1. [Azure portal](https://portal.azure.com)'da, **Yeni** > **Tümleştirme** > **Logic App**'i seçin.

   ![Mantıksal uygulama oluşturma](./media/iot-hub-how-to-order-connection-state-events/select-logic-app.png)

2. Mantıksal uygulamanıza aboneliğiniz içinde benzersiz olan bir ad verin, ardından IoT Hub'ınızla aynı aboneliği, kaynak grubunu ve konumu seçin. 

3. Seçin **panoya Sabitle**, ardından **Oluştur**.

   Mantıksal uygulamanız için bir Azure kaynağı oluşturdunuz. Azure mantıksal uygulamanızı dağıttıktan sonra Logic Apps Tasarımcısı'nda hızlı bir başlangıç yapmanıza yardımcı olacak ortak desen şablonları gösterilir.

   > [!NOTE] 
   > Seçtiğinizde, **panoya Sabitle**, mantıksal uygulamanızı Logic Apps Tasarımcısı'nda otomatik olarak açılır. Aksi takdirde mantıksal uygulamanızı kendiniz bulup açabilirsiniz.

4. Mantıksal uygulamanızı sıfırdan oluşturabilmek için Logic App Tasarımcısı'nda **Şablonlar**'ın altından **Boş Mantıksal Uygulama**'yı seçin.

### <a name="select-a-trigger"></a>Tetikleyici seçme

Tetikleyici, mantıksal uygulamanızı başlatan belirli bir olaydır. Bu öğreticide, iş akışını başlatan tetikleyici HTTP üzerinden bir istek alır.  

1. Bağlayıcılar ve tetikleyiciler arama çubuğunda **HTTP** yazın.

2. Tetikleyici olarak **İstek - Bir HTTP isteği alındığında**'yı seçin. 

   ![HTTP istek tetikleyicisini seçme](./media/iot-hub-how-to-order-connection-state-events/http-request-trigger.png)

3. **Şema oluşturmak için örnek yük kullanma** öğesini seçin. 

   ![Bir şema oluşturmak için örnek yük kullanma](./media/iot-hub-how-to-order-connection-state-events/sample-payload.png)

4. Aşağıdaki örnek JSON kodunu metin kutusuna yapıştırın ve **Bitti**'yi seçin:

   ```json
   [{
    "id": "fbfd8ee1-cf78-74c6-dbcf-e1c58638ccbd",
    "topic":
      "/SUBSCRIPTIONS/DEMO5CDD-8DAB-4CF4-9B2F-C22E8A755472/RESOURCEGROUPS/EGTESTRG/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/MYIOTHUB",
    "subject": "devices/Demo-Device-1",
    "eventType": "Microsoft.Devices.DeviceConnected",
    "eventTime": "2018-07-03T23:20:11.6921933+00:00",
    "data": {
      "deviceConnectionStateEventInfo": {
        "sequenceNumber":
          "000000000000000001D4132452F67CE200000002000000000000000000000001"
      },
      "hubName": "MYIOTHUB",
      "deviceId": "48e44e11-1437-4907-83b1-4a8d7e89859e",
      "moduleId": ""
    },
    "dataVersion": "1",
    "metadataVersion": "1"
   }]
   ```

5. **İsteğinize Uygulama/JSON olarak ayarlanmış bir Content-Type üst bilgisi eklemeyi unutmayın** önerisinin bulunduğu bir açılan bildirim alabilirsiniz. Bu öneriyi güvenle yoksayabilir ve sonraki bölüme geçebilirsiniz. 

### <a name="create-a-condition"></a>Bir koşul oluşturun

Mantıksal uygulama iş akışında, belirli bir koşul denetimini geçtikten sonra belirli eylemleri çalıştırma koşulları yardımcı olur. İstenen eylem koşulu karşılandığında tanımlanabilir. Bu öğreticide, olay türü veya bağlantısı kesilmiş aygıtı bağlı olup olmadığını denetlemek için durumdur. Eylem, veritabanında saklı yordamı yürütmek için olacaktır. 

1. Seçin **yeni adım** ardından **yerleşik olanları** ve **koşul**. 

2. Koşul, yalnızca bu bağlı cihaz ve cihazın bağlantısı olayları yürütmek için aşağıda gösterildiği gibi doldurun:

  * Bir değer seçin: **olay türü**
  * Değişiklik "eşittir" için **şununla biter**
  * Bir değer seçin: **nected**

   ![Koşul doldurun](./media/iot-hub-how-to-order-connection-state-events/condition-detail.png)

3. Koşul true ise, tıklayın **Eylem Ekle**.
  
   ![True ise eylem ekleme](./media/iot-hub-how-to-order-connection-state-events/action-if-true.png)

4. Cosmos DB için arama yapın ve tıklayarak **Azure Cosmos DB - saklı yordamı yürütme**

   ![CosmosDB arayın](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-search.png)

5. Formun doldurulması için yürütme depolanan değerleri veritabanınızdan seçerek tedarik edin. Aşağıda gösterildiği gibi parametreleri ve bölüm anahtarı değerini girin. 

   ![mantıksal uygulama eylemi Doldur](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure.png)

6. Mantıksal uygulamanızı kaydedin. 

### <a name="copy-the-http-url"></a>HTTP URL'sini kopyalama

Logic Apps Tasarımcısı'nda ayrılmadan önce mantıksal uygulamanız için bir tetikleyici dinlediği URL'yi kopyalayın. Bu URL'yi, Event Grid'i yapılandırmak için kullanırsınız. 

1. **Bir HTTP isteği alındığında** tetikleyici yapılandırma kutusunu tıklayarak genişletin. 

2. Yanındaki kopyala düğmesini seçerek **HTTP POST URL** değerini kopyalayın. 

   ![HTTP POST URL değerini kopyalama](./media/iot-hub-how-to-order-connection-state-events/copy-url.png)

3. Sonraki bölümde başvurabilmek için bu URL'yi saklayın. 

## <a name="configure-subscription-for-iot-hub-events"></a>IoT Hub olayları için aboneliği yapılandırma

Bu bölümde, IoT Hub'ınızı gerçekleşen olayları yayımlamak için yapılandıracaksınız. 

1. Azure portalında IoT Hub'ınıza gidin. 

2. **Olaylar**'ı seçin.

   ![Event Grid ayrıntılarını açma](./media/iot-hub-how-to-order-connection-state-events/event-grid.png)

3. **Olay aboneliği**’ni seçin. 

   ![Yeni olay aboneliği oluşturma](./media/iot-hub-how-to-order-connection-state-events/event-subscription.png)

4. Olay aboneliğini aşağıdaki değerlerle oluşturun: 

   * **Olay türü**: tüm olay türleri ve seçim işaretini kaldırın abone **cihaz bağlı** ve **cihaz bağlantısı kesildi** menüsünde.

   * **Uç Nokta Ayrıntıları**: Uç Nokta Türü olarak **Web Kancası**'nı seçin, Uç nokta seçin'e tıklayın, mantıksal uygulamanızdan kopyaladığınız URL'yi yapıştırın ve seçimi onaylayın.

       ![uç nokta URL'si seçme](./media/iot-hub-how-to-order-connection-state-events/endpoint-url.png)

   * **Olay aboneliği ayrıntıları**: açıklayıcı bir ad girin ve seçin **Event Grid şema**.
   Form, aşağıdaki örneğe benzer: 

       ![Örnek olay aboneliği formu](./media/iot-hub-how-to-order-connection-state-events/subscription-form.png)

5. Olay aboneliğini kaydetmek için **Oluştur**'u seçin.

## <a name="observe-events"></a>Olayları gözlemektir

Şimdi olay aboneliğinizi ayarlamak, bir cihaz bağlanarak test edin.

### <a name="register-a-device-in-iot-hub"></a>IOT Hub'ında cihaz kaydetme

1. IoT Hub'ınızda **IoT Cihazları**'nı seçin. 

2. **Add (Ekle)** seçeneğini belirleyin.

3. **Cihaz Kimliği** için `Demo-Device-1` girin.

4. **Kaydet**’i seçin. 

5. Farklı cihaz kimlikleri ile birden çok cihaz ekleyebilirsiniz.

   ![Nasıl yapılır sonucu](./media/iot-hub-how-to-order-connection-state-events/AddIoTDevice.png)

6. Kopyalama **bağlantı dizesi – birincil anahtar** daha sonra kullanmak için.

   ![Nasıl yapılır sonucu](./media/iot-hub-how-to-order-connection-state-events/DeviceConnString.png)

### <a name="start-raspberry-pi-simulator"></a>Raspberry Pi'yi simülatörü başlatın

1. Raspberry Pi web simülatörü cihaz bağlantı benzetimini yapmak için kullanalım.

[Raspberry Pi'yi simülatörü başlatın](https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted)

### <a name="run-a-sample-application-on-the-raspberry-pi-web-simulator"></a>Raspberry Pi web simülatörü hakkında bir örnek uygulamayı çalıştırma

Bu cihaz bağlı bir olayı tetikler.

1. Kodlama alanında satır 15 yer tutucuyu, Azure IOT Hub cihaz bağlantı dizesiyle değiştirin.

   ![Nasıl yapılır sonucu](./media/iot-hub-how-to-order-connection-state-events/raspconnstring.png)

2. Uygulamayı tıklayarak çalıştırmak **çalıştırma**.

Algılayıcı verilerini ve IOT hub'ınıza gönderdiği iletileri gösterir aşağıdaki çıktıyı görmeniz gerekir.

   ![Nasıl yapılır sonucu](./media/iot-hub-how-to-order-connection-state-events/raspmsg.png)

   Tıklayın **Durdur** simülatör ve tetikleyiciyi durdurmak için bir **cihazın bağlantısı** olay.

Artık algılayıcı verilerini toplamak ve IOT hub'ına göndermek için örnek bir uygulama da çalıştırmanız gerekir. 

### <a name="observe-events-in-cosmos-db"></a>Cosmos DB'de olayları gözlemektir

Cosmos DB belge içinde yürütülen saklı yordamının sonuçları görebilirsiniz. İşte gibi görünüyor. Her satır, cihaz başına en son cihaz bağlantı durumu içerir.

   ![Nasıl yapılır sonucu](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-outcome.png)

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Yerine [Azure portalında](http://portal.azure.com), Azure CLI kullanarak IOT hub'ı adımları gerçekleştirebilirsiniz. Azure CLI sayfaları için Ayrıntılar için bkz [bir olay aboneliği oluşturma](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) ve [IOT cihazına oluşturma](/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity#ext-azure-cli-iot-ext-az-iot-hub-device-identity-create).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide Azure aboneliğinize ücret uygulanmasına neden olan kaynaklar kullanılmıştır. Öğreticideki denemeleriniz ve sonuçlarınızın testi bittiğinde, korumak istemediğiniz kaynakları devre dışı bırakın veya silin. 

Mantıksal uygulamanızda yapılan çalışmayı kaybetmek istemiyorsanız, bunu silmek yerine devre dışı bırakın. 

1. Mantıksal uygulamanıza gidin.

2. Üzerinde **genel bakış** dikey penceresinde **Sil** veya **devre dışı**. 

Her aboneliğin tek bir ücretsiz IoT Hub'ı olabilir. Bu öğretici için ücretsiz bir hub oluşturduysanız, ücretleri önlemek için bunu silmeniz gerekmez.

1. IoT Hub'ınıza gidin. 

2. Üzerinde **genel bakış** dikey penceresinde **Sil**. 

IoT Hub'ınızı korusanız bile, oluşturduğunuz olay aboneliğini silmek isteyebilirsiniz. 

1. IoT Hub'ınızda **Event Grid**'i seçin.

2. Kaldırmak istediğiniz olay aboneliğini seçin. 

3. **Sil**’i seçin. 

Bir Azure Cosmos DB hesabını Azure Portalından kaldırmak için hesap adına sağ tıklayın ve **hesabını Sil**. Ayrıntılı yönergeler için bkz: [Azure Cosmos DB hesabı silme](https://docs.microsoft.com/azure/cosmos-db/manage-account#delete).

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [tetikleyici eylemlere Event Grid kullanarak IOT Hub olaylarına tepki verme](../iot-hub/iot-hub-event-grid.md)

* [IOT Hub olaylarını öğreticisini deneyin](../event-grid/publish-iot-hub-events-to-logic-apps.md)

* Başka ne hakkında ile yapabileceklerinizi öğrenin [Event Grid](../event-grid/overview.md)


