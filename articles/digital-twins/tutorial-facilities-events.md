---
title: 'Öğretici: Azure dijital İkizlerini alanı olayları yakalama | Microsoft Docs'
description: Azure Digital Twins'i Logic Apps ile tümleştirerek alanlarınızdan bildirim almayı öğrenmek için bu öğreticideki adımları izleyin.
services: digital-twins
author: alinamstanciu
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 12/18/2018
ms.author: alinast
ms.openlocfilehash: 2b84fa2fd8053ca4dc7ef0ad246d29b2bba3dae5
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484706"
---
# <a name="tutorial-receive-notifications-from-your-azure-digital-twins-spaces-by-using-logic-apps"></a>Öğretici: Logic Apps'i kullanarak, Azure dijital İkizlerini boşlukları bildirimleri alma

Azure dijital İkizlerini örneğinizi dağıtma alanlarınıza sağlama ve belirli koşulları izlemek için özel işlevler uygulamak sonra izlenen koşulların gerçekleşmesine office yöneticinizin e-posta ile bildirimde bulunabilir.

İçinde [ilk öğreticide](tutorial-facilities-setup.md), sanal bir yapı uzamsal grafiği yapılandırılmış. Yapı odada hareket, tasarruf edilen karbon dioksit ve sıcaklık algılayıcıları içerir. İçinde [ikinci öğreticide](tutorial-facilities-udf.md), grafiğiniz ve bu algılayıcı değerlerini izlemek için bir kullanıcı tanımlı işlev sağladığınız ve odası, boş ve sıcaklık ve tasarruf edilen karbon dioksit olduğunda bildirimlerini tetiklemesini deneyimli bir aralık içinde olan. 

Bu öğreticide, oda uygun olduğunda e-posta göndermek için bu bildirimleri Azure Logic Apps ile tümleştirme adımları gösterilmektedir. Bir ofis yöneticisi bu bilgileri kullanarak çalışanların en verimli toplantı odasını ayırmalarına yardımcı olabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Olayları Azure Event Grid ile tümleştirin.
> * Logic Apps ile olayları bildirin.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide Azure Digital Twins kurulumunu [yapılandırmış](tutorial-facilities-setup.md) ve [sağlamış](tutorial-facilities-udf.md) olduğunuz kabul edilmektedir. Devam etmeden önce aşağıdakilere sahip olduğunuzdan emin olun:

- Bir [Azure hesabı](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Çalışan bir Digital Twins örneği.
- Çalışma makinenize indirilmiş ve ayıklanmış [Digital Twins C# örnekleri](https://github.com/Azure-Samples/digital-twins-samples-csharp).
- [.NET core SDK'sı sürüm 2.1.403 veya üzeri](https://www.microsoft.com/net/download) geliştirme makinenizde örneği çalıştırmak için. Çalıştırma `dotnet --version` doğru sürümünün yüklü olduğunu doğrulayın.
- Bildirim e-postalarını göndermek için Office 365 hesabı.

## <a name="integrate-events-with-event-grid"></a>Olayları Event Grid ile tümleştirme

Bu bölümde, ayarladığınız [Event Grid](../event-grid/overview.md) Azure dijital İkizlerini örneğinizin olaylarını toplayan ve bunları yeniden yönlendirmek için bir [olay işleyicisi](../event-grid/event-handlers.md) Logic Apps gibi.

### <a name="create-an-event-grid-topic"></a>Bir olay Kılavuzu konusu oluşturma

Bir [olay Kılavuzu konusu](../event-grid/concepts.md#topics) kullanıcı tanımlı işlev tarafından oluşturulan olayları yönlendirmek için bir arabirim sağlar. 

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol bölmede seçin **kaynak Oluştur**. 

1. **Event Grid Konu Başlığı** araması yapın ve sonuçlardan seçin. **Oluştur**’u seçin.

1. Event Grid konu başlığınız için bir **Ad** girin ve **Abonelik** seçimi yapın. Seçin **kaynak grubu** kullanılan veya dijital İkizlerini Örneğiniz için oluşturulan ve **konumu**. **Oluştur**’u seçin. 

    ![Bir olay Kılavuzu konusu oluşturma](./media/tutorial-facilities-events/create-event-grid-topic.png)

1. Olay Kılavuzu konu başlığına göz atın, kaynak grubunuzdan, select **genel bakış**ve değeri kopyalayın **konu başlığı uç noktası** geçici bir dosya için. Sonraki bölümde bu URL gerekir. 

1. Seçin **erişim anahtarları**, kopyalayıp **YOUR_KEY_1** ve **YOUR_KEY_2** geçici bir dosya için. Sonraki bölümde uç noktası oluşturmak için bu değerlere ihtiyacınız olacak.

    ![Event Grid anahtarları](./media/tutorial-facilities-events/event-grid-keys.png)

### <a name="create-an-endpoint-for-the-event-grid-topic"></a>Olay Kılavuzu konusu için bir uç nokta oluşturma

1. Komut penceresinde nde olduğunuzdan emin olun **doluluk quickstart\src** dijital İkizlerini örnek klasörü.

1. Dosyayı açmak **actions\createEndpoints.yaml** , Visual Studio Kod Düzenleyicisi'nde. Aşağıdaki içeriğe sahip olduğundan emin olun:

    ```yaml
    - type: EventGrid
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: <Primary connection string for your Event Grid>
      secondaryConnectionString: <Secondary connection string for your Event Grid>
      path: <Event Grid Topic Name without https:// and /api/events, e.g. eventgridname.region.eventgrid.azure.net>
    ```

1. Yer tutucusunu değiştirin `<Primary connection string for your Event Grid>` değeriyle **YOUR_KEY_1**.

1. Yer tutucusunu değiştirin `<Secondary connection string for your Event Grid>` değeriyle **YOUR_KEY_2**.

1. Yer tutucusunu değiştirin **yolu** yoluyla olay ızgarası konu. Bu yolu kaldırarak almak **https://** ve sondaki kaynak yolları **konu başlığı uç noktası** URL'si. Şu biçime benzer görünmelidir: *EventGridAdı.Konumunuz.eventgrid.azure.net*.

    > [!IMPORTANT]
    > Değerleri girerken tırnak işaretlerini dahil etmeyin. YAML dosyası iki nokta üst üste sonra en az bir boşluk karakteri olduğundan emin olun. Tüm çevrimiçi YAML Doğrulayıcı gibi kullanarak, YAML dosyası içeriği doğrulayabilirsiniz [bu araç](https://onlineyamltools.com/validate-yaml).

1. Dosyayı kaydedin ve kapatın. Komut penceresinde aşağıdaki komutu çalıştırın ve istendiğinde oturum açın. 

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Bu komut, Event Grid için uç nokta oluşturur. 

   ![Event Grid uç noktaları](./media/tutorial-facilities-events/dotnet-create-endpoints.png)

## <a name="notify-events-with-logic-apps"></a>Logic Apps ile olayları bildir

Kullanabileceğiniz [Azure Logic Apps](../logic-apps/logic-apps-overview.md) diğer hizmetlerden alınan olayları için otomatik görevler oluşturmak için hizmet. Bu bölümde, Yardım, uzamsal, sensörlerden alınan yönlendirilmiş olaylar için e-posta bildirimleri oluşturmak için mantıksal uygulamalarını ayarlama bir [olay Kılavuzu konusu](../event-grid/overview.md).

1. Sol bölmesinde [Azure portalında](https://portal.azure.com)seçin **kaynak Oluştur**.

1. **Mantıksal Uygulama** araması yapın ve yeni bir kaynak oluşturun. **Oluştur**’u seçin.

1. Girin bir **adı** seçin ve mantıksal uygulama kaynağı için kendi **abonelik**, **kaynak grubu**, ve **konumu**. **Oluştur**’u seçin.

    ![Logic Apps kaynak oluştur](./media/tutorial-facilities-events/create-logic-app.png)

1. Açık dağıtıldığında, Logic Apps kaynak ve açın **mantıksal Uygulama Tasarımcısı** bölmesi. 

1. Seçin **bir Event Grid, kaynak gerçekleştiğinde** tetikleyici. Kiracınızın istendiğinde Azure hesabınızda oturum açın. Seçin **erişime izin ver** istenirse Event Grid kaynağınızın. Seçin **devam**.

1. İçinde **(Önizleme) kaynak olayı gerçekleştiğinde** penceresi: 
   
   a. Seçin **abonelik** olay Kılavuzu konusu oluşturma için kullanılan.

   b. Seçin **Microsoft.EventGrid.Topics** için **kaynak türü**.

   c. Aşağı açılan kutudan için Event Grid kaynağınızı seçin **kaynak adı**.

   ![Mantıksal Uygulama Tasarımcısı bölmesi](./media/tutorial-facilities-events/logic-app-resource-event.png)

1. Seçin **yeni adım** düğmesi.

1. İçinde **eylem seçin** penceresi:

   a. **parse json** araması yapın ve **JSON Ayrıştır** eylemini seçin.

   b. İçinde **içeriği** alanın, Seç **gövdesi** gelen **dinamik içerik** listesi.

   c. **Şema oluşturmak için örnek yük kullanma** öğesini seçin. Aşağıdaki JSON yükü yapıştırın ve ardından **Bitti**.

    ```JSON
    {
    "id": "32162f00-a8f1-4d37-aee2-9312aabba0fd",
    "subject": "UdfCustom",
    "data": {
      "TopologyObjectId": "20efd3a8-34cb-4d96-a502-e02bffdabb14",
      "ResourceType": "Space",
      "Payload": "\"Air quality is poor.\"",
      "CorrelationId": "32162f00-a8f1-4d37-aee2-9312aabba0fd"
    },
    "eventType": "UdfCustom",
    "eventTime": "0001-01-01T00:00:00Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/a382ee71-b48e-4382-b6be-eec7540cf271/resourceGroups/HOL/providers/Microsoft.EventGrid/topics/DigitalTwinEventGrid"
    }
    ```

    Bu yükte kurgusal değerler bulunur. Mantıksal uygulamalar oluşturmak için bu örnek yük kullanan bir *şema*.

    ![Event Grid için Logic Apps JSON Ayrıştır penceresi](./media/tutorial-facilities-events/logic-app-parse-json.png)

1. Seçin **yeni adım** düğmesi.

1. İçinde **eylem seçin** penceresi:

   a. Seçin **denetim > koşul** veya arama **koşul** gelen **eylemleri** listesi. 

   b. İlk **bir değer seçin** metin kutusunda **eventType** gelen **dinamik içerik** için liste **JSON Ayrıştır** penceresi.

   c. İkinci **bir değer seçin** metin kutusuna `UdfCustom`.

   ![Seçili koşulları](./media/tutorial-facilities-events/logic-app-condition.png)

1. İçinde **doğruysa** penceresi:

   a. Seçin **Eylem Ekle**seçip **Office 365 Outlook**.

   b. Gelen **eylemleri** listesinden **bir e-posta**. Seçin **oturum** ve e-posta hesabı kimlik bilgilerinizi kullanın. Seçin **erişime izin ver** istenirse.

   c. **Alıcı** kutusuna bildirimlerin gönderilmesi için e-posta adresinizi yazın. İçinde **konu**, metin girin **dijital İkizlerini bildirim alanında kötü uzaktan kalite için**. Ardından **TopologyObjectId** gelen **dinamik içerik** için liste **JSON Ayrıştır**.

   d. Altında **gövdesi** aynı penceresinde şuna benzer bir metin girin: **Zayıf uzaktan kalite bir odada algılandı ve sıcaklık ayarlanması gerekiyor**. Öğeleri kullanarak özenli çekinmeyin **dinamik içerik** listesi.

   ![Logic Apps "e-posta Gönder" seçimleri](./media/tutorial-facilities-events/logic-app-send-email.png)

1. Seçin **Kaydet** üst kısmındaki düğmeye **mantıksal Uygulama Tasarımcısı** bölmesi.

1. Sensör verilerini göz atarak benzetimini yapmak emin **cihaz bağlantısı** dijital İkizlerini örnek bir komut penceresi ve çalışan klasörü `dotnet run`.

Birkaç dakika içinde e-posta bildirimleri bu Logic Apps kaynak alma başlamanız gerekir. 

   ![E-posta ile bildirim](./media/tutorial-facilities-events/logic-app-notification.png)

Bu e-postaları almayı durdurmak için portalında Logic Apps kaynağınıza gidin ve seçin **genel bakış** bölmesi. Seçin **devre dışı**.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu noktada Azure dijital İkizlerini keşfetmeye durdurmak istiyorsanız, bu öğreticide oluşturulan kaynakları silmek çekinmeyin:

1. Sol menüden [Azure portalında](https://portal.azure.com)seçin **tüm kaynakları**dijital İkizlerini kaynak grubunuzu seçin ve seçin **Sil**.

    > [!TIP]
    > Dijital İkizlerini örneğinizin silme sorun olduysa, bir hizmet güncelleştirmesi düzeltme alındı. Örneğiniz silme yeniden deneyin.

2. Gerekirse, iş makinenizde örnek uygulamaları silin.

## <a name="next-steps"></a>Sonraki adımlar

Sensör verilerinizi görselleştirin, eğilimleri ve anormallikleri analiz öğrenmek için sonraki öğreticiye geçin:

> [!div class="nextstepaction"]
> [Öğretici: Time Series Insights'ı kullanarak Azure dijital İkizlerini alanlarınıza olayları çözümleyin](tutorial-facilities-analyze.md)

Ayrıca Azure dijital İkizlerini nesne modellerinde ve uzamsal zeka grafikler hakkında daha fazla bilgi edinebilirsiniz:

> [!div class="nextstepaction"]
> [Digital Twins nesne modellerini ve uzamsal zeka grafını anlama](concepts-objectmodel-spatialgraph.md)
