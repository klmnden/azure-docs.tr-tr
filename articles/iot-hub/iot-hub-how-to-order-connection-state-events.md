---
title: Azure Cosmos DB kullanarak Azure IOT Hub aracılığıyla sipariş cihaz bağlantısı etkinlikleri | Microsoft Docs
description: Bu makalede, sipariş ve en son bağlantı durumunu korumak üzere Azure Cosmos DB kullanarak Azure IOT hub cihaz bağlantısı etkinlikleri kaydetmek açıklanır
services: iot-hub
ms.service: iot-hub
author: ash2017
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: asrastog
ms.openlocfilehash: f4baab6e0909144efc613572207e7f24c4b4fe1f
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743313"
---
# <a name="order-device-connection-events-from-azure-iot-hub-using-azure-cosmos-db"></a>Bağlantı olayları Azure Cosmos DB kullanarak Azure IOT hub'a cihaz sipariş

Azure Event Grid, olay tabanlı uygulamalar oluşturmanıza ve kolayca işletme çözümlerinizi IOT olayları tümleştirmenize yardımcı olur. Bu makalede izleme ve en son cihaz bağlantı durumu Cosmos DB'de depolamak için kullanılan bir kurulum size kılavuzluk eder. Bağlı cihaz ve cihazın bağlantısı olayları kullanılabilir sıra numarası kullanın ve en son durumu Cosmos DB'de depolamak ederiz. Cosmos DB koleksiyonunda karşı yürütülen bir uygulama mantığı bir saklı yordam kullanmak için kullanacağız.

Sıra numarası onaltılık düzendeki bir sayıyı bir dize gösterimidir. Dize karşılaştırma büyük numarasını belirlemek için kullanabilirsiniz. Onaltılık dize dönüştürüyorsanız sayının 256 bitlik bir sayı olması. Sıra numarası kesin olarak artmaktadır ve en son olay diğer olayları daha yüksek bir değer alacaktır. Sık varsa cihaz bağlanır bağlantısını keser ve Azure Event Grid olayların sırası desteklemediğinden, yalnızca en son olay bir aşağı akış eylemi tetiklemek için kullanılır sağlamak istediğinizde bu kullanışlıdır.

## <a name="prerequisites"></a>Önkoşullar

* Etkin bir Azure hesabı. Hesabınız yoksa [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/pricing/free-trial/).

* Etkin bir Azure Cosmos DB SQL API hesabı. Bir henüz oluşturmadıysanız bkz [bir veritabanı hesabı oluşturma](../cosmos-db/create-sql-api-dotnet.md#create-an-azure-cosmos-db-account) kılavuz.

* Veritabanı koleksiyonu. Bkz: [bir koleksiyon Ekle](../cosmos-db/create-sql-api-dotnet.md#add-a-database-and-a-collection) kılavuz. Koleksiyonunuzu oluştururken `/id` bölüm anahtarı.

* Azure'da bir IoT Hub'ı. Henüz oluşturmadıysanız, yönergeler için bkz. [IoT Hub'ı kullanmaya başlama](iot-hub-csharp-csharp-getstarted.md).

## <a name="create-a-stored-procedure"></a>Bir saklı yordam oluşturma

İlk olarak, bir saklı yordam oluşturmak ve ayarlamak için gelen olayların sıra numaraları karşılaştırır ve en son olay cihaz başına veritabanında kaydeden bir mantığının çalışması ayarlayın.

1. Cosmos DB SQL API içinde seçin **Veri Gezgini** > **öğeleri** > **yeni saklı yordam**.

   ![Saklı yordam oluşturma](./media/iot-hub-how-to-order-connection-state-events/create-stored-procedure.png)

2. Girin **LatestDeviceConnectionState** saklı yordamın kimliği için aşağıdakileri yapıştırın **saklı yordam gövde**. Bu kodu mevcut kodlar saklı yordam gövde yerini aldığını unutmayın. Bu kod, cihaz kimliği başına bir satır tutar ve en yüksek sıra numarası tanımlayarak bu cihaz kimliği son bağlantı durumunu kaydeder.

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

1. İçinde [Azure portalında](https://portal.azure.com)seçin **+ kaynak Oluştur**seçin **tümleştirme** ardından **mantıksal uygulama**.

   ![Mantıksal uygulama oluşturma](./media/iot-hub-how-to-order-connection-state-events/select-logic-app.png)

2. Mantıksal uygulamanıza aboneliğiniz içinde benzersiz olan bir ad verin, ardından IoT Hub'ınızla aynı aboneliği, kaynak grubunu ve konumu seçin.

   ![Yeni mantıksal uygulama](./media/iot-hub-how-to-order-connection-state-events/new-logic-app.png)

3. Seçin **Oluştur** mantıksal uygulamanızı oluşturmak için.

   Mantıksal uygulamanız için bir Azure kaynağı oluşturdunuz. Azure mantıksal uygulamanızı dağıttıktan sonra Logic Apps Tasarımcısı'nda hızlı bir başlangıç yapmanıza yardımcı olacak ortak desen şablonları gösterilir.

   > [!NOTE]
   > Bulmak ve mantıksal uygulamanızı yeniden açmak için seçin **kaynak grupları** ve bu nasıl yapılır konuları için kullandığınız kaynak grubunu seçin. Ardından, yeni mantıksal uygulamanızı seçin. Bu mantıksal Uygulama Tasarımcısı açılır.

4. Logic Apps Tasarımcısı'nda, sık kullanılan Tetikleyicileri görene kadar sağa kaydırın. Altında **şablonları**, seçin **boş mantıksal uygulama** böylece, mantıksal uygulamanızı sıfırdan oluşturabilirsiniz.

### <a name="select-a-trigger"></a>Tetikleyici seçme

Tetikleyici, mantıksal uygulamanızı başlatan belirli bir olaydır. Bu öğreticide, iş akışını başlatan tetikleyici HTTP üzerinden bir istek alır.

1. Bağlayıcılar ve tetikleyiciler Arama çubuğuna yazın **HTTP** ve Enter tuşuna basın.

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

   ![Örnek JSON yükü Yapıştır](./media/iot-hub-how-to-order-connection-state-events/paste-sample-payload.png)

5. **İsteğinize Uygulama/JSON olarak ayarlanmış bir Content-Type üst bilgisi eklemeyi unutmayın** önerisinin bulunduğu bir açılan bildirim alabilirsiniz. Bu öneriyi güvenle yoksayabilir ve sonraki bölüme geçebilirsiniz.

### <a name="create-a-condition"></a>Bir koşul oluşturun

Mantıksal uygulama iş akışında, belirli bir koşul denetimini geçtikten sonra belirli eylemleri çalıştırma koşulları yardımcı olur. İstenen eylem koşulu karşılandığında tanımlanabilir. Bu öğreticide, olay türü veya bağlantısı kesilmiş aygıtı bağlı olup olmadığını denetlemek için durumdur. Eylem, veritabanında saklı yordamı yürütmek için olacaktır.

1. Seçin **+ yeni adım** ardından **yerleşik**, ardından bulmak ve seçmek **koşul**. Tıklayın **bir değer seçin** ve dinamik içerik--seçilebilir alanlar gösteren bir kutusu açılır. Yalnızca bu bağlı cihaz ve cihazın bağlantısı olayları yürütmek için aşağıda gösterildiği gibi alanları doldurun:

   * Bir değer seçin: **eventType** --Bu, bu alana tıkladığınızda görüntülenen dinamik içerik alanları seçin.
   * Değişiklik "eşittir" için **ile biten**.
   * Bir değer seçin: **nected**.

     ![Koşul doldurun](./media/iot-hub-how-to-order-connection-state-events/condition-detail.png)

2. İçinde **doğruysa** iletişim kutusunda, tıklayarak **Eylem Ekle**.
  
   ![True ise eylem ekleme](./media/iot-hub-how-to-order-connection-state-events/action-if-true.png)

3. Cosmos DB için arama yapın ve seçin **Azure Cosmos DB - saklı yordamı yürütme**

   ![CosmosDB arayın](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-search.png)

4. Doldurun **cosmosdb bağlantı** için **bağlantı adı** ve Giriş tablosunda seçip **Oluştur**. Gördüğünüz **saklı yordam yürütme** paneli. Alanlar için değerleri girin:

   **Veritabanı kimliği**: ToDoList

   **Koleksiyon kimliği**: Öğeler

   **Sproc kimliği**: LatestDeviceConnectionState

5. Seçin **yeni parametre Ekle**. Görünen açılır menüde, yanındaki onay kutularını işaretleyin **bölüm anahtarı** ve **saklı yordamın parametreleri**, ardından ekranda başka bir yerde; için bir alan ve bölüm anahtarı değeri için bir alan ekler saklı yordamın parametreleri.

   ![mantıksal uygulama eylemi Doldur](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure.png)

6. Artık parametreler ve bölüm anahtarı değerini aşağıda gösterildiği gibi girin. Köşeli ayraçlar ve çift gösterildiği gibi tırnak yerleştirdiğinizden emin olun. Öğesine tıklamanız gerekebilir **dinamik içerik Ekle** burada kullanabileceğiniz geçerli değerler alınamıyor.

   ![mantıksal uygulama eylemi Doldur](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure-2.png)

7. Burada yazan bölmesinin üst **her**altında **önceki adımlardan bir çıkış seçin**, emin olun **gövdesi** seçilir.

   ![mantıksal uygulama için-her Doldur](./media/iot-hub-how-to-order-connection-state-events/logicapp-foreach-body.png)

8. Mantıksal uygulamanızı kaydedin.

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

3. Seçin **+ olay aboneliği**.

   ![Yeni olay aboneliği oluşturma](./media/iot-hub-how-to-order-connection-state-events/event-subscription.png)

4. Doldurun **olay aboneliği ayrıntıları**: Açıklayıcı bir ad girin ve seçin **Event Grid şema**.

5. Doldurun **olay türleri** alanları. Açılan listede yalnızca seçin **cihaz bağlı** ve **cihazın bağlantısı** menüsünde. Ekranda listenin kapatıp seçimlerinizi kaydetmek için başka bir yerde tıklayın.

   ![Aranacak olay türlerini ayarlayın](./media/iot-hub-how-to-order-connection-state-events/set-event-types.png)

6. İçin **uç noktası ayrıntıları**, uç nokta türü olarak seçin **Web kancası** ve select uç noktasına tıklayın ve mantıksal uygulamanızdan kopyaladığınız URL'yi yapıştırın ve seçimi onaylayın.

   ![uç nokta URL'si seçin](./media/iot-hub-how-to-order-connection-state-events/endpoint-select.png)

7. Form artık aşağıdaki örneğe benzer görünmelidir:

   ![Örnek olay aboneliği formu](./media/iot-hub-how-to-order-connection-state-events/subscription-form.png)

   Olay aboneliğini kaydetmek için **Oluştur**'u seçin.

## <a name="observe-events"></a>Olayları gözlemektir

Şimdi olay aboneliğinizi ayarlamak, bir cihaz bağlanarak test edin.

### <a name="register-a-device-in-iot-hub"></a>IOT Hub'ında cihaz kaydetme

1. IoT Hub'ınızda **IoT Cihazları**'nı seçin.

2. Seçin **+ Ekle** bölmenin üstünde.

3. **Cihaz Kimliği** için `Demo-Device-1` girin.

4. **Kaydet**’i seçin.

5. Farklı cihaz kimlikleri ile birden çok cihaz ekleyebilirsiniz.

   ![Cihazlar hub'ına eklendi](./media/iot-hub-how-to-order-connection-state-events/AddIoTDevice.png)

6. Cihazda yeniden tıklayın; artık anahtarları ve bağlantı dizeleri doldurulur. Kopyalama **bağlantı dizesi – birincil anahtar** daha sonra kullanmak için.

   ![Cihaz için bağlantı dizesi](./media/iot-hub-how-to-order-connection-state-events/DeviceConnString.png)

### <a name="start-raspberry-pi-simulator"></a>Raspberry Pi'yi simülatörü başlatın

Raspberry Pi web simülatörü cihaz bağlantı benzetimini yapmak için kullanalım.

[Raspberry Pi'yi simülatörü başlatın](https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted)

### <a name="run-a-sample-application-on-the-raspberry-pi-web-simulator"></a>Raspberry Pi web simülatörü hakkında bir örnek uygulamayı çalıştırma

Bu cihaz bağlı bir olayı tetikler.

1. Kodlama alanında satır 15 yer tutucunun önceki bölümde sonunda kaydettiğiniz, Azure IOT Hub cihaz bağlantı dizesiyle değiştirin.

   ![Cihaz bağlantı dizesini yapıştırın.](./media/iot-hub-how-to-order-connection-state-events/raspconnstring.png)

2. Seçerek uygulama çalıştırma **çalıştırma**.

Algılayıcı verilerini ve IOT hub'ınıza gönderdiği iletileri gösterir aşağıdaki çıktıya benzer bir şey görürsünüz.

   ![Uygulamayı çalıştırma](./media/iot-hub-how-to-order-connection-state-events/raspmsg.png)

   Tıklayın **Durdur** simülatör ve tetikleyiciyi durdurmak için bir **cihazın bağlantısı** olay.

Artık algılayıcı verilerini toplamak ve IOT hub'ına göndermek için örnek bir uygulama da çalıştırmanız gerekir.

### <a name="observe-events-in-cosmos-db"></a>Cosmos DB'de olayları gözlemektir

Cosmos DB belge içinde yürütülen saklı yordamının sonuçları görebilirsiniz. İşte gibi görünüyor. Her satır, cihaz başına en son cihaz bağlantı durumu içerir.

   ![Nasıl yapılır sonucu](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-outcome.png)

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Yerine [Azure portalında](https://portal.azure.com), Azure CLI kullanarak IOT hub'ı adımları gerçekleştirebilirsiniz. Azure CLI sayfaları için Ayrıntılar için bkz [bir olay aboneliği oluşturma](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) ve [IOT cihazına oluşturma](/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity#ext-azure-cli-iot-ext-az-iot-hub-device-identity-create).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide Azure aboneliğinize ücret uygulanmasına neden olan kaynaklar kullanılmıştır. İşiniz bittiğinde Eğitim programını çalışıyor ve test sonuçlarınız, devre dışı bırakın veya tutmak istemediğiniz kaynakları silin.

Mantıksal uygulamanızda yapılan çalışmayı kaybetmek istemiyorsanız, bunu silmek yerine devre dışı bırakın.

1. Mantıksal uygulamanıza gidin.

2. Üzerinde **genel bakış** dikey penceresinde **Sil** veya **devre dışı**.

    Her aboneliğin tek bir ücretsiz IoT Hub'ı olabilir. Bu öğretici için ücretsiz bir hub oluşturduysanız, ücretleri önlemek için bunu silmeniz gerekmez.

3. IoT Hub'ınıza gidin.

4. Üzerinde **genel bakış** dikey penceresinde **Sil**.

    IoT Hub'ınızı korusanız bile, oluşturduğunuz olay aboneliğini silmek isteyebilirsiniz.

5. IoT Hub'ınızda **Event Grid**'i seçin.

6. Kaldırmak istediğiniz olay aboneliğini seçin.

7. **Sil**’i seçin.

Bir Azure Cosmos DB hesabını Azure Portalından kaldırmak için hesap adına sağ tıklayın ve **hesabını Sil**. Ayrıntılı yönergeler için bkz: [Azure Cosmos DB hesabı silme](https://docs.microsoft.com/azure/cosmos-db/manage-account).

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [tetikleyici eylemlere Event Grid kullanarak IOT Hub olaylarına tepki verme](../iot-hub/iot-hub-event-grid.md)

* [IOT Hub olaylarını öğreticisini deneyin](../event-grid/publish-iot-hub-events-to-logic-apps.md)

* Başka ne hakkında ile yapabileceklerinizi öğrenin [Event Grid](../event-grid/overview.md)