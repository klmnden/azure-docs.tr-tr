---
title: Tetikleyici Azure Logic Apps için IOT hub'ı olaylarını kullanma | Microsoft Docs
description: Azure olay kılavuzunun olay yönlendirme hizmeti kullanarak, IOT hub'ı olaylara dayanarak Azure mantıksal uygulamalar eylemleri gerçekleştirmek için otomatik işlemleri oluşturun.
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.service: iot-hub
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2018
ms.author: kgremban
ms.openlocfilehash: 4fed42a45f8d291bd3ba1e4fd5d636b7d0b0fbfc
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="send-email-notifications-about-azure-iot-hub-events-using-logic-apps"></a>Logic Apps kullanarak Azure IOT Hub olayları hakkında e-posta bildirimleri gönder

Azure olay kılavuz aşağı akış iş uygulamalarınız içinde eylemler tetikleme tarafından IOT Hub'ındaki olayları tepki sağlar.

Bu makalede, IOT Hub ve olay kılavuz kullanan örnek bir yapılandırma anlatılmaktadır. Son, bir cihaz IOT hub'ınıza eklenen her zaman bir bildirim e-posta göndermek için ayarlamanız bir Azure mantıksal uygulama gerekir. 

## <a name="prerequisites"></a>Önkoşullar

* Office 365 Outlook, Outlook.com veya Gmail gibi Azure mantıksal uygulamaları tarafından desteklenen herhangi bir e-posta sağlayıcıdan gelen e-posta hesabı. Bu e-posta hesabı, olay bildirimleri göndermek için kullanılır. Desteklenen mantıksal uygulama bağlayıcılar tam bir listesi için bkz: [bağlayıcılar genel bakış](https://docs.microsoft.com/connectors/)
* Etkin bir Azure hesabı. Yoksa, şunları yapabilirsiniz [ücretsiz bir hesap oluşturma](http://azure.microsoft.com/pricing/free-trial/).
* Azure IOT hub. Bir henüz oluşturmadıysanız bkz [IOT Hub ile çalışmaya başlama](../iot-hub/iot-hub-csharp-csharp-getstarted.md) kılavuz. 

## <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

İlk olarak, bir mantıksal uygulama oluşturma ve kaynak grubu, sanal makine için izleyen olay kılavuz tetikleyicisi ekleyin. 

### <a name="create-a-logic-app-resource"></a>Bir mantıksal uygulama kaynağı oluşturun


1. İçinde [Azure portal](https://portal.azure.com)seçin **yeni** > **Kurumsal tümleştirme** > **mantıksal uygulama**.

   ![Mantıksal uygulama oluşturma](./media/publish-iot-hub-events-to-logic-apps/select-logic-app.png)

2. Mantıksal uygulamanızı aboneliğinizde benzersiz bir ad verin, sonra aynı abonelik, kaynak grubunu ve konumu IOT hub'ınızı seçin. 
3. Hazır olduğunuzda, seçin **panoya Sabitle**ve seçin **oluşturma**.

   Mantıksal uygulamanız için bir Azure kaynağı oluşturdunuz. Azure mantıksal uygulamanızı dağıttıktan sonra Logic Apps Tasarımcısı'nda hızlı bir başlangıç yapmanıza yardımcı olacak ortak desen şablonları gösterilir.

   > [!NOTE] 
   > Seçtiğinizde, **panoya Sabitle**, mantıksal uygulamanızı Logic Apps Tasarımcısı'nda otomatik olarak açılır. Aksi takdirde, el ile bulabilir ve mantıksal uygulamanızı açın.

4. Altında mantığı Uygulama Tasarımcısı'nda **şablonları**, seçin **boş mantıksal uygulama** böylece mantıksal uygulamanızı sıfırdan oluşturabilir.

## <a name="select-a-trigger"></a>Bir tetikleyici seçin

Bir tetikleyici mantıksal uygulamanızı başlayan belirli bir olaydır. Bu öğretici için iş akışını devre dışı ayarlar tetikleyici isteği HTTP üzerinden alıyor.  

1. Bağlayıcılar ve Tetikleyicileri arama çubuğu türü **HTTP**.
2. Seçin **isteği - olduğunda bir HTTP isteği alındığında** tetikleyici olarak. 

   ![HTTP isteği Tetikleyici seçin](./media/publish-iot-hub-events-to-logic-apps/http-request-trigger.png)

3. Seçin **şema üretmek için kullanım örnek yük**. 

   ![HTTP isteği Tetikleyici seçin](./media/publish-iot-hub-events-to-logic-apps/sample-payload.png)

4. Aşağıdaki örnek JSON kodunu metin kutusuna ve ardından Yapıştır **Bitti**:

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
5. Şunu açılır bir bildirim alabilirsiniz **uygulama/json isteğinizdeki şekilde ayarlanmış bir Content-Type üstbilgisi dahil etmeyi unutmayın.** Güvenli bir şekilde bu durmasını ve sonraki bölüme geçin. 


### <a name="create-an-action"></a>Bir eylem oluşturun

Mantıksal uygulama iş akışı tetikleyici başladıktan sonra oluşan herhangi bir adım eylemlerdir. Bu öğretici için eylem bir e-posta bildirimi e-posta sağlayıcınızdan göndermektir. 

1. Seçin **yeni adım** sonra **Eylem Ekle**. 

   ![Yeni adım, Eylem Ekle](./media/publish-iot-hub-events-to-logic-apps/new-step.png)

2. Arama **e-posta**. 
3. E-posta sağlayıcınıza uygun bağlayıcıyı bulun ve seçin. Bu öğretici kullanır **Office 365 Outlook**. Diğer e-posta sağlayıcısı için adımları benzerdir. 

   ![E-posta sağlayıcısı bağlayıcıyı seçin](./media/publish-iot-hub-events-to-logic-apps/o365-outlook.png)

4. Seçin **bir e-posta Gönder** eylem. 
5. İstenirse, e-posta hesabınızda oturum açın. 
6. E-posta şablonu oluşturun. 
   * **İçin**: bildirim e-postaları almak için e-posta adresi girin. Bu öğretici için test etmek için erişmek için bir e-posta hesabı kullanın. 
   * **Konu** ve **gövde**: e-posta metni yazın. Olay verilerine dayalı dinamik içerik dahil etmek Seçici aracından JSON Özellikler'i seçin.  

   E-posta şablonu, aşağıdaki örnekte olduğu gibi görünebilir:

   ![E-posta bilgileri doldurun](./media/publish-iot-hub-events-to-logic-apps/email-content.png)

7. Mantıksal uygulamanızı kaydedin. 

### <a name="copy-the-http-url"></a>HTTP URL'sini Kopyala

Logic Apps Tasarımcısı çıkmadan önce mantıksal uygulamalarınızı dinleme yaptığı için bir tetikleyici URL'sini kopyalayın. Olay kılavuz yapılandırmak için bu URL'yi kullanın. 

1. Genişletme **zaman bir HTTP isteği alındığında** üzerinde tıklatarak tetikleyici yapılandırma kutusu. 
2. Değerini kopyalayın **HTTP POST URL** yanında Kopyala düğmesini seçerek. 

   ![HTTP POST URL'sini Kopyala](./media/publish-iot-hub-events-to-logic-apps/copy-url.png)

3. Bu URL kaydetmek için sonraki bölümde başvurabilir. 

## <a name="publish-an-event-from-iot-hub"></a>IOT hub'ı bir olay yayımlama

Bu bölümde, bunlar ortaya çıktığında olayları yayımlamak için IOT Hub'ınızı yapılandırın. 

1. Azure portalında IOT hub'ına gidin. 
2. Seçin **olayları**.

   ![Olay kılavuz Ayrıntıları'nı açın](./media/publish-iot-hub-events-to-logic-apps/event-grid.png)

3. Seçin **olay aboneliği**. 

   ![Yeni olay Abonelik Oluştur](./media/publish-iot-hub-events-to-logic-apps/event-subscription.png)

4. Olay aboneliği aşağıdaki değerlerle oluşturun: 
   * **Ad**: açıklayıcı bir ad sağlayın.
   * **Tüm olay türleri için abone**: onay kutusunun seçimini kaldırın.
   * **Olay türleri**: seçin **DeviceCreated**.
   * **Abone türü**: seçin **Web kancası**.
   * **Abone endpoint**: mantıksal uygulamanızı kopyaladığınız URL'sini yapıştırın. 

   Olay aboneliği burada kaydedin ve IOT hub'ınıza oluşturulan her cihaz için bildirimlerin. Bu öğretici için yine de isteğe bağlı alanları belirli aygıtlar için filtre uygulamak için kullanalım: 

   * **Filtre önek**: girin `devices/Building1_` 1 oluşturmanın cihaz etkinlikleri filtrelemek için.
   * **Sonek filtre**: girin `_Temperature` sıcaklık için ilgili cihaz olayları filtrelemek için.

   İşiniz bittiğinde, form aşağıdaki gibi görünmelidir: 

   ![Örnek olay abonelik formu](./media/publish-iot-hub-events-to-logic-apps/subscription-form.png)

5. Seçin **oluşturma** olay aboneliği kaydetmek için.

## <a name="create-a-new-device"></a>Yeni bir cihaz oluşturma

Bir olay bildirim e-posta tetiklemek için yeni bir cihaz oluşturarak mantıksal uygulamanızı test edin. 

1. IOT hub'dan seçin **IOT cihazları**. 
2. **Add (Ekle)** seçeneğini belirleyin.
3. İçin **cihaz kimliği**, girin `Building1_Floor1_Room1_Temperature`.
4. **Kaydet**’i seçin. 
5. Birden çok farklı cihaz olay abonelik filtreleri test etmek için kimlikleri aygıtlarla ekleyebilirsiniz. Bu örnekler deneyin: 
   * Building1_Floor1_Room1_Light
   * Building1_Floor2_Room2_Temperature
   * Building2_Floor1_Room1_Temperature
   * Building2_Floor1_Room1_Light

IOT hub'ınıza birkaç aygıtları ekledikten sonra mantıksal uygulama hangilerinin tetiklenen görmek için e-postanızı kontrol edin. 

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Azure portalını kullanmak yerine, Azure CLI kullanarak IOT hub'ı adımları gerçekleştirebilirsiniz. Ayrıntılar için bkz: için Azure CLI sayfaları [bir olay aboneliği oluşturma](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) ve [IOT cihaz oluşturma](https://docs.microsoft.com/cli/azure/iot/device)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici, Azure aboneliğinizin üzerinde ücretlendirme kaynakları kullanılır. Öğretici çalışıyor ve test bittiğinde, sonuçlarınızı devre dışı bırakır veya korumak istemediğiniz kaynakları silin. 

Mantıksal uygulamanızı çalışmaları kaybetmek istemiyorsanız, silmek yerine devre dışı bırakın. 

1. Mantıksal uygulamanıza gidin.
2. Üzerinde **genel bakış** dikey seçin **silmek** veya **devre dışı**. 

Her abonelik bir ücretsiz IOT hub olabilir. Bu öğretici için ücretsiz bir hub'ı oluşturduysanız, ücret oluşmasını önlemek için silmeniz gerekmez.

1. IOT hub'ına gidin. 
2. Üzerinde **genel bakış** dikey seçin **silmek**. 

IOT hub'ınızı tutmak olsa bile, oluşturduğunuz olay aboneliği silmek isteyebilirsiniz. 

1. IOT hub, seçin **olay kılavuz**.
2. Kaldırmak istediğiniz olay aboneliği seçin. 
3. **Sil**’i seçin. 

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [tetikleyici eylemleri olay kılavuz kullanarak IOT hub'ı olaylarına tepki](../iot-hub/iot-hub-event-grid.md).

Başka ile neler yapabileceğinizi öğrenin [olay kılavuz](overview.md).


