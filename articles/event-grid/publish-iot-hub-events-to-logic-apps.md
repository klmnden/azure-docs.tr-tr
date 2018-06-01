---
title: IoT Hub olaylarını kullanarak Azure Logic Apps'i tetikleme | Microsoft Docs
description: Azure Event Grid'in olay yönlendirme hizmetini kullanarak, IoT Hub olayları temelinde Azure Logic Apps eylemleri gerçekleştiren otomatik işlemler oluşturun.
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.service: iot-hub
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2018
ms.author: kgremban
ms.openlocfilehash: aab674f16fcc3fd4869f24f72f66878a8751d892
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/18/2018
ms.locfileid: "34301493"
---
# <a name="send-email-notifications-about-azure-iot-hub-events-using-logic-apps"></a>Logic Apps kullanarak Azure IoT Hub olayları hakkında e-posta bildirimleri gönderme

Azure Event Grid, aşağı akış iş uygulamalarınızda eylemler tetikleyerek IoT Hub'daki olaylara karşılık vermenize olanak tanır.

Bu makale IoT Hub ve Event Grid kullanan örnek bir yapılandırmada size yol gösterir. Sonuna geldiğinizde, IoT hub'ınıza her cihaz eklendiğinde bildirim e-postası gönderecek şekilde ayarlanmış bir Azure mantıksal uygulamanız olacaktır. 

## <a name="prerequisites"></a>Ön koşullar

* Azure Logic Apps tarafından desteklenen Office 365 Outlook, Outlook.com veya Gmail gibi bir e-posta sağlayıcısından alınmış e-posta hesabı. Bu e-posta hesabı olay bildirimlerini göndermek için kullanılır. Desteklenen Logic App bağlayıcılarının tam listesi için bkz. [Bağlayıcılara genel bakış](https://docs.microsoft.com/connectors/)
* Etkin bir Azure hesabı. Hesabınız yoksa [ücretsiz bir hesap oluşturabilirsiniz](http://azure.microsoft.com/pricing/free-trial/).
* Azure'da bir Iot hub'ı. Henüz oluşturmadıysanız, yönergeler için bkz. [IoT Hub'ı kullanmaya başlama](../iot-hub/iot-hub-csharp-csharp-getstarted.md). 

## <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

İlk olarak, bir mantıksal uygulama oluşturun ve sanal makineniz için kaynak grubunu izleyen bir Event Grid tetikleyicisi ekleyin. 

### <a name="create-a-logic-app-resource"></a>Mantıksal uygulama kaynağı oluşturma


1. [Azure portal](https://portal.azure.com)’da **Yeni** > **Kurumsal Tümleştirme** > **Mantıksal Uygulama**’yı seçin.

   ![Mantıksal uygulama oluşturma](./media/publish-iot-hub-events-to-logic-apps/select-logic-app.png)

2. Mantıksal uygulamanıza aboneliğiniz içinde benzersiz olan bir ad verin, ardından IoT Hub'ınızla aynı aboneliği, kaynak grubunu ve konumu seçin. 
3. Hazır olduğunuzda **Panoya sabitle**'yi ve ardından **Oluştur**'u seçin.

   Mantıksal uygulamanız için bir Azure kaynağı oluşturdunuz. Azure mantıksal uygulamanızı dağıttıktan sonra Logic Apps Tasarımcısı'nda hızlı bir başlangıç yapmanıza yardımcı olacak ortak desen şablonları gösterilir.

   > [!NOTE] 
   > **Panoya sabitle**’yi seçtiğinizde, mantıksal uygulama otomatik olarak Logic Apps Tasarımcısı’nda açılır. Aksi takdirde mantıksal uygulamanızı kendiniz bulup açabilirsiniz.

4. Mantıksal uygulamanızı sıfırdan oluşturabilmek için Logic App Tasarımcısı'nda **Şablonlar**'ın altından **Boş Mantıksal Uygulama**'yı seçin.

## <a name="select-a-trigger"></a>Tetikleyici seçme

Tetikleyici, mantıksal uygulamanızı başlatan belirli bir olaydır. Bu öğreticide, iş akışını başlatan tetikleyici HTTP üzerinden bir istek alır.  

1. Bağlayıcılar ve tetikleyiciler arama çubuğunda **HTTP** yazın.
2. Tetikleyici olarak **İstek - Bir HTTP isteği alındığında** öğesini seçin. 

   ![HTTP istek tetikleyicisini seçme](./media/publish-iot-hub-events-to-logic-apps/http-request-trigger.png)

3. **Şema oluşturmak için örnek yük kullanma** öğesini seçin. 

   ![HTTP istek tetikleyicisini seçme](./media/publish-iot-hub-events-to-logic-apps/sample-payload.png)

4. Aşağıdaki örnek JSON kodunu metin kutusuna yapıştırın ve **Bitti**'yi seçin:

   ```json
   [{
     "id": "56afc886-767b-d359-d59e-0da7877166b2",
     "topic": "/SUBSCRIPTIONS/<Subscription ID>/RESOURCEGROUPS/<Resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<IoT hub name>",
     "subject": "devices/LogicAppTestDevice",
     "eventType": "Microsoft.Devices.DeviceCreated",
     "eventTime": "2018-01-02T19:17:44.4383997Z",
     "data": {
       "twin": {
         "deviceId": "LogicAppTestDevice",
         "etag": "AAAAAAAAAAE=",
         "status": "enabled",
         "statusUpdateTime": "0001-01-01T00:00:00",
         "connectionState": "Disconnected",
         "lastActivityTime": "0001-01-01T00:00:00",
         "cloudToDeviceMessageCount": 0,
         "authenticationType": "sas",
         "x509Thumbprint": {
           "primaryThumbprint": null,
           "secondaryThumbprint": null
         },
         "version": 2,
         "properties": {
           "desired": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           },
           "reported": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           }
         }
       },
       "hubName": "egtesthub1",
       "deviceId": "LogicAppTestDevice",
       "operationTimestamp": "2018-01-02T19:17:44.4383997Z",
       "opType": "DeviceCreated"
     },
     "dataVersion": "",
     "metadataVersion": "1"
   }]
   ```
5. **İsteğinize Uygulama/JSON olarak ayarlanmış bir Content-Type üst bilgisi eklemeyi unutmayın** önerisinin bulunduğu bir açılan bildirim alabilirsiniz. Bu öneriyi güvenle yoksayabilir ve sonraki bölüme geçebilirsiniz. 


### <a name="create-an-action"></a>Eylem oluşturma

Eylemler, tetikleyici mantıksal uygulama iş yükünü başlattıktan sonra gerçekleşen adımlardır. Bu öğreticide, e-posta sağlayıcınızdan bir e-posta bildirimi gönderme eylemi kullanılır. 

1. **Yeni adım**’ı, sonra **Eylem ekle**’yi seçin. 

   ![Yeni adım, eylem ekle](./media/publish-iot-hub-events-to-logic-apps/new-step.png)

2. **E-posta**'yı arayın. 
3. E-posta sağlayıcınıza uygun bağlayıcıyı bulun ve seçin. Bu öğreticide **Office 365 Outlook** kullanılır. Diğer e-posta sağlayıcılarının adımları da bunlara benzer. 

   ![E-posta sağlayıcısının bağlayıcısını seçme](./media/publish-iot-hub-events-to-logic-apps/o365-outlook.png)

4. **E-posta gönder** eylemini seçin. 
5. İstenirse, e-posta hesabınızda oturum açın. 
6. E-posta şablonunuzu oluşturun. 
   * **Kime**: Bildirim e-postalarını alacak olan e-posta adresini girin. Bu öğreticide, test etmek için erişebileceğiniz bir e-posta hesabı kullanın. 
   * **Konu** ve **Gövde**: E-postanızın metnini yazın. Olay verileri temelinde dinamik içerik eklemek için seçici aracından JSON özelliklerini seçin.  

   E-posta şablonunuz, aşağıdaki örneğe benzer görünebilir:

   ![E-posta bilgilerini doldurma](./media/publish-iot-hub-events-to-logic-apps/email-content.png)

7. Mantıksal uygulamanızı kaydedin. 

### <a name="copy-the-http-url"></a>HTTP URL'sini kopyalama

Logic Apps Tasarımcısı'ndan çıkmadan önce, mantıksal uygulamalarınızın tetikleyici için dinlediği URL'yi kopyalayın. Bu URL'yi, Event Grid'i yapılandırmak için kullanırsınız. 

1. **Bir HTTP isteği alındığında** tetikleyici yapılandırma kutusunu tıklayarak genişletin. 
2. Yanındaki kopyala düğmesini seçerek **HTTP POST URL** değerini kopyalayın. 

   ![HTTP POST URL değerini kopyalama](./media/publish-iot-hub-events-to-logic-apps/copy-url.png)

3. Sonraki bölümde başvurabilmek için bu URL'yi saklayın. 

## <a name="publish-an-event-from-iot-hub"></a>IoT Hub'ından olay yayımlama

Bu bölümde, IoT Hub'ınızı gerçekleşen olayları yayımlamak için yapılandıracaksınız. 

1. Azure portalında IoT Hub'ınıza gidin. 
2. **Olaylar**'ı seçin.

   ![Event Grid ayrıntılarını açma](./media/publish-iot-hub-events-to-logic-apps/event-grid.png)

3. **Olay aboneliği**’ni seçin. 

   ![Yeni olay aboneliği oluşturma](./media/publish-iot-hub-events-to-logic-apps/event-subscription.png)

4. Olay aboneliğini aşağıdaki değerlerle oluşturun: 
   * **Ad**: Açıklayıcı bir ad sağlayın.
   * **Tüm olay türlerine abone ol**: Onay kutusunun işaretini kaldırın.
   * **Olay türleri**: **DeviceCreated** öğesini seçin.
   * **Abone türü**: **Web Kancası**'nı seçin.
   * **Abone uç noktası**: Mantıksal uygulamanızdan kopyaladığınız URL'yi yapıştırın. 

   Olay aboneliğini buraya kaydedebilir ve IoT Hub'ınızda oluşturulan her cihaz için bildirimler alabilirsiniz. Öte yandan bu öğretici için, isteğe bağlı alanları kullanıp belirli cihazları filtreleyelim: 

   * **Ön ek filtresi**: Bina1 kapsamındaki cihaz olaylarını filtrelemek için `devices/Building1_` girin.
   * **Sonek filtresi**: Sıcaklıkla ilgili cihaz olaylarını filtrelemek için `_Temperature` girin.

   İşiniz bittiğinde, form aşağıdaki örnekteki gibi görünmelidir: 

   ![Örnek olay aboneliği formu](./media/publish-iot-hub-events-to-logic-apps/subscription-form.png)

5. Olay aboneliğini kaydetmek için **Oluştur**'u seçin.

## <a name="create-a-new-device"></a>Yeni cihaz oluşturma

Olay bildirim e-postasını tetiklemek için yeni bir cihaz oluşturarak mantıksal uygulamanızı test edin. 

1. IoT Hub'ınızda **IoT Cihazları**'nı seçin. 
2. **Add (Ekle)** seçeneğini belirleyin.
3. **Cihaz Kimliği** için `Building1_Floor1_Room1_Temperature` girin.
4. **Kaydet**’i seçin. 
5. Olay abonelik filtrelerini test etmek için farklı cihaz kimlikleri olan birden çok cihaz ekleyebilirsiniz. Şu örnekleri deneyin: 
   * Building1_Floor1_Room1_Light
   * Building1_Floor2_Room2_Temperature
   * Building2_Floor1_Room1_Temperature
   * Building2_Floor1_Room1_Light

IoT Hub'ınıza birkaç cihaz ekledikten sonra, hangisinin mantıksal uygulamayı tetiklediğini görmek için e-postanızı gözden geçirin. 

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Azure portalı kullanmak yerine, IoT Hub adımlarını Azure CLI'yi kullanarak gerçekleştirebilirsiniz. Ayrıntılar için, Azure CLI'nin [olay aboneliği oluşturma](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) ve [IoT cihazı oluşturma](https://docs.microsoft.com/cli/azure/iot/device) sayfalarına bakın

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide Azure aboneliğinize ücret uygulanmasına neden olan kaynaklar kullanılmıştır. Öğreticideki denemeleriniz ve sonuçlarınızın testi bittiğinde, korumak istemediğiniz kaynakları devre dışı bırakın veya silin. 

Mantıksal uygulamanızda yapılan çalışmayı kaybetmek istemiyorsanız, bunu silmek yerine devre dışı bırakın. 

1. Mantıksal uygulamanıza gidin.
2. **Genel Bakış** dikey penceresinde **Sil**'i veya **Devre Dışı Bırak**'ı seçin. 

Her aboneliğin tek bir ücretsiz IoT Hub'ı olabilir. Bu öğretici için ücretsiz bir hub oluşturduysanız, ücretleri önlemek için bunu silmeniz gerekmez.

1. IoT Hub'ınıza gidin. 
2. **Genel Bakış** dikey penceresinde **Sil**'i seçin. 

IoT Hub'ınızı korusanız bile, oluşturduğunuz olay aboneliğini silmek isteyebilirsiniz. 

1. IoT Hub'ınızda **Event Grid**'i seçin.
2. Kaldırmak istediğiniz olay aboneliğini seçin. 
3. **Sil**’i seçin. 

## <a name="next-steps"></a>Sonraki adımlar

[Event Grid kullanıp olayları tetikleyerek IoT Hub'ı olaylarına tepki verme](../iot-hub/iot-hub-event-grid.md) hakkında daha fazla bilgi edinin.

[Event Grid](overview.md) ile başka neler yapabileceğinizi öğrenin.


