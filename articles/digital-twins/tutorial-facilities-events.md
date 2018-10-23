---
title: Azure Digital Twins alanından olay yakalama | Microsoft Docs
description: Azure Digital Twins'i Logic Apps ile tümleştirerek alanlarınızdan bildirim almayı öğrenmek için bu öğreticideki adımları izleyin.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 91dd16938efbd1e24419352f66e3238646a77e8a
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323437"
---
# <a name="tutorial-receive-notifications-from-your-azure-digital-twins-spaces-using-logic-apps"></a>Öğretici: Logic Apps'i kullanarak Azure Digital Twins alanlarınızdan bildirim alma

Azure Digital Twins örneğinizi dağıttıktan, alanlarınızı sağladıktan ve belirli koşulları izlemek için özel işlevler uyguladıktan sonra izlenen koşullar ortaya çıktığında ofis yöneticinizi e-posta ile bilgilendirebilirsiniz. 

[Birinci öğreticide](tutorial-facilities-setup.md) hareket, karbondioksit ve sıcaklık sensörleri bulunan bir odaya sahip olan hayali bir binanın uzamsal grafını yapılandırdınız. [İkinci öğreticide](tutorial-facilities-udf.md) grafınızı ve bu sensör değerlerini izleyip oda boş olduğunda ve sıcaklık ile karbondioksit düzeyleri uygun aralıkta olduğunda bildirimleri tetikleyen kullanıcı tanımlı işlevi çağıran özel bir işlev sağladınız. Bu öğreticide, oda uygun olduğunda e-posta göndermek için bu bildirimleri Azure Logic Apps ile tümleştirme adımları gösterilmektedir. Bir ofis yöneticisi bu bilgileri kullanarak çalışanların en verimli toplantı odasını ayırmalarına yardımcı olabilir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Olayları Event Grid ile tümleştirme
> * Mantıksal Uygulama ile olaylardan bildirim oluşturma
    
## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide Azure Digital Twins kurulumunu [yapılandırmış](tutorial-facilities-setup.md) ve [sağlamış](tutorial-facilities-udf.md) olduğunuz kabul edilmektedir. Devam etmeden önce aşağıdakilere sahip olduğunuzdan emin olun:
- Bir [Azure hesabı](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Çalışan bir Digital Twins örneği.
- Çalışma makinenize indirilmiş ve ayıklanmış [Digital Twins C# örnekleri](https://github.com/Azure-Samples/digital-twins-samples-csharp).
- Örneği çalıştırmak için geliştirme makinenize yüklenmiş [.NET Core SDK sürüm 2.1.403 veya üzeri](https://www.microsoft.com/net/download). Doğru sürümün yüklü olup olmadığını denetlemek için `dotnet --version` komutunu çalıştırın. 
- Bildirim e-postalarını göndermek için Office 365 hesabı.

## <a name="integrate-events-with-event-grid"></a>Olayları Event Grid ile tümleştirme 
Bu bölümde bir [Event Grid](../event-grid/overview.md) örneği oluşturarak Digital Twins örneğinizden gelen olayları toplayacak ve bunları Logic Apps gibi bir [olay işleyicisine](../event-grid/event-handlers.md) yönlendireceksiniz.

### <a name="create-event-grid-topic"></a>Event Grid konu başlığı oluşturma
[Event Grid Konu Başlıkları](../event-grid/concepts.md#topics), kullanıcı tanımlı işlev tarafından oluşturulan olayların yönlendirilmesi için kullanılabilecek bir arabirim sağlar. 

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol gezinti panelinde **Kaynak oluştur**'a tıklayın. 

1. **Event Grid Konu Başlığı** araması yapın ve sonuçlardan seçin. **Oluştur**’a tıklayın.

1. Event Grid konu başlığınız için bir **Ad** girin ve **Abonelik** seçimi yapın. Digital Twins örneğiniz için kullandığınız veya oluşturduğunuz **Kaynak grubu** ve **Konum** girişlerini seçin. **Oluştur**’a tıklayın. 

    ![Event Grid konu başlığı oluşturma](./media/tutorial-facilities-events/create-event-grid-topic.png)

1. Kaynak grubunuzdan Event Grid konu başlığına gidin, **Genel bakış**'a tıklayın ve **Konu Başlığı Uç Noktası** değerini geçici bir dosyaya kopyalayın. Bu URL'yi aşağıdaki bölümde kullanacaksınız. 

1. **Erişim anahtarları**'na tıklayıp **Anahtar 1** ve **Anahtar 2** değerlerini geçici bir dosyaya kopyalayın. Bu değerleri aşağıdaki bölümde uç noktayı oluştururken kullanacaksınız.

    ![Event Grid Anahtarları](./media/tutorial-facilities-events/event-grid-keys.png)

### <a name="create-an-endpoint-for-the-event-grid-topic"></a>Event Grid Konu Başlığı için uç nokta oluşturma

1. Komut penceresinde Digital Twins örneğinizin **_occupancy-quickstart\src_** klasöründe olduğunuzdan emin olun.

1. **_actions\createEndpoints.yaml_** adlı dosyayı Visual Studio Code düzenleyicinizde açın. Aşağıdaki içeriğe sahip olduğundan emin olun:

    ```yaml
    - type: EventGrid
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: Primary_connection_string_for_your_Event_Grid
      secondaryConnectionString: Secondary_connection_string_for_your_Event_Grid
      path: Event_Grid_Topic_Path
    ```

1. `Primary_connection_string_for_your_Event_Grid` yer tutucusunu **Anahtar 1** değeriyle değiştirin. 

1. `Secondary_connection_string_for_your_Event_Grid` yer tutucusunu **Anahtar 2** değeriyle değiştirin.

1. `Event_Grid_Topic_Path` yer tutucusunu Event Grid konu başlığının yoluyla değiştirin. Bu yolu almak için **Konu Başlığı Uç Noktası** URL'sinin *https://* bölümünü ve sonrasındaki kaynak yollarını silin. Şu biçime benzer görünmelidir: **EventGridAdı.Konumunuz.eventgrid.azure.net**. 

    > [!IMPORTANT]
    > Değerleri girerken tırnak işaretlerini dahil etmeyin. *YAML* dosyasında iki nokta sonrasında en az bir boşluk karakteri olduğundan emin olun. *YAML* dosyanızın içeriğini [bu araç](https://onlineyamltools.com/validate-yaml) gibi çevrimiçi YAML doğrulayıcılarını kullanarak da doğrulayabilirsiniz.

1. Dosyayı kaydedin ve kapatın. Komut penceresinde aşağıdaki komutu çalıştırın ve istendiğinde oturum açın. 

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Bu komut Event Grid için gerekli uç noktayı oluşturur. 

   ![Event Grid uç noktaları](./media/tutorial-facilities-events/dotnet-create-endpoints.png)


## <a name="notify-events-with-logic-app"></a>Mantıksal Uygulama ile olaylardan bildirim oluşturma
[Azure Logic Apps](../logic-apps/logic-apps-overview.md) hizmeti, diğer hizmetlerden alınan olaylar için otomatik görev oluşturmanızı sağlar. Bu bölümde bir [Event Grid Konu Başlığının](../event-grid/overview.md) yardımıyla uzamsal sensörlerinizden yönlendirilen olaylar için e-posta bildirimleri oluşturmak üzere gerekli Logic Apps ayarlarını yapılandıracaksınız.

1. [Azure portalın](https://portal.azure.com) sol gezinti bölmesinde **Kaynak oluştur**'a tıklayın.

1. **Mantıksal Uygulama** araması yapın ve yeni bir kaynak oluşturun. **Oluştur**’a tıklayın.

1. Mantıksal Uygulamanız için bir **Ad** girin ve ardından **Abonelik**, **Kaynak grubu** ve **Konum** seçimlerinizi yapın. **Oluştur**’a tıklayın.

    ![Mantıksal Uygulama oluşturma](./media/tutorial-facilities-events/create-logic-app.png)

1. Dağıtılan Mantıksal Uygulamanızı ve ardından **Mantıksal Uygulama Tasarımcısı** bölmesini açın. 

1. **When an Event Grid event occurs** (Bir Event Grid olayı gerçekleştiğinde) tetikleyicisini seçin. İstendiğinde Azure hesabınızla kiracınızda **oturum açın**. İstendiğinde Event Grid için **erişime izin verin**. **Devam**’a tıklayın.

1. **When a resource event occurs (Preview)** (Bir kaynak olayı gerçekleştiğinde (Önizleme)) penceresinde, 
    1. Event Grid için kullandığınız **Abonelik** girişini seçin,

    1. **Kaynak Türü** olarak **Microsoft.EventGrid.Topics** seçeneğini belirleyin,

    1. **Kaynak Adı** açılan kutusunda Event Grid kaynağınızı seçin.

    ![Mantıksal Uygulama oluşturma](./media/tutorial-facilities-events/logic-app-resource-event.png)

1. **Yeni adım** düğmesine tıklayın.

1. **Eylem seçin** penceresinde,
    1. *parse json* araması yapın ve **JSON Ayrıştır** eylemini seçin.

    1. **İçerik** alanına tıklayın ve **Dinamik içerik** listesinden **Gövde** girişini seçin.

    1. **Şema oluşturmak için örnek yük kullanma** girişine tıklayın. Aşağıdaki JSON yükünü yapıştırın ve **Bitti**'ye tıklayın.

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
    
    Bu yükte kurgusal değerler bulunur. Mantıksal Uygulama, bu örneği kullanarak bir **Şema** oluşturur.
    
    ![Mantıksal Uygulama Event Grid için JSON Ayrıştırma](./media/tutorial-facilities-events/logic-app-parse-json.png)

1. **Yeni adım** düğmesine tıklayın.

1. **Eylem seçin** penceresinde,
    1. **Koşul Denetimi** araması yapın ve **Eylemler** listesinden seçin. 

    1. İlk **Bir değer seçin** metin kutusunun içine tıklayın ve **JSON Ayrıştır** penceresinin **Dinamik içerik** listesinden **eventType** girişini seçin.

    1. İkinci **Bir değer seçin** metin kutusunun içine tıklayın ve *UdfCustom* yazın.

    ![Mantıksal Uygulama Event Grid için JSON Ayrıştırma](./media/tutorial-facilities-events/logic-app-condition.png)

1. **Doğruysa** penceresinde,
    1. **Eylem ekle**'ye tıklayıp *Office 365 Outlook*'u seçin.

    1. **Eylemler** listesinden **E-posta gönder**'i seçin. **Oturum aç**'a tıklayıp e-posta hesabınızın kimlik bilgilerini girin. Sorulduğunda **Erişime izin ver**'e tıklayın.

    1. **Alıcı** kutusuna bildirimlerin gönderilmesi için e-posta adresinizi yazın. **Konu** alanına *Digital Twins alanda düşük hava kalitesi bildirimi* metnini girin ve **JSON Ayrıştır** için **Dinamik içerik** listesinden **TopologyObjectId** girişini seçin.

    1. Aynı penceredeki **Gövde** bölümüne şuna benzer bir metin girin: *Odalardan birinde düşük hava kalitesi algılandı ve sıcaklığın ayarlanması gerekiyor*. Aşağıda gösterilen şekilde **Dinamik içerik** listesindeki diğer öğeleri de kullanabilirsiniz.

    ![Mantıksal Uygulama E-posta gönderme](./media/tutorial-facilities-events/logic-app-send-email.png)

1. **Mantıksal Uygulama Tasarımcısı** bölmesinin en üstünde bulunan **Kaydet** düğmesine tıklayın.

1. Bir komut penceresinde Digital Twins örneğinin **_device-connectivity_** klasörüne gidip `dotnet run` komutunu çalıştırarak sensör verilerinin simülasyonunu yapmayı unutmayın.

Bu Mantıksal Uygulama birkaç dakika içinde e-posta göndermeye başlamalıdır. 

   ![Mantıksal Uygulama E-posta gönderme](./media/tutorial-facilities-events/logic-app-notification.png)

Bu e-postaları almayı durdurmak için portalda **Mantıksal Uygulamanıza** gidin ve **Genel bakış** bölmesini seçin. **Devre dışı bırak**'ı seçin.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kadar Azure Digital Twins incelemesi sizin için yeterliyse bu öğreticide oluşturulan kaynakları silebilirsiniz:

1. [Azure portalda](http://portal.azure.com) sol taraftaki menüden **Tüm kaynaklar**'a tıklayın, Digital Twins kaynak grubunuzu ve ardından **Sil**'i seçin.
2. Gerekirse çalışma makinenizdeki örnek uygulamaları da silebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

Sensör verilerinizi görselleştirmeyi, eğilimleri analiz etmeyi ve anomalileri bulmayı öğrenmek için bir sonraki öğreticiye geçebilirsiniz. 
> [!div class="nextstepaction"]
> [Öğretici: Time Series Insights'ı kullanarak Azure Digital Twins alanlarınızdan gelen olayları görselleştirme ve analiz etme](tutorial-facilities-analyze.md)

Ayrıca dilerseniz Azure Digital Twins hizmetindeki uzamsal zeka grafları ve nesne modelleri hakkında daha fazla bilgi edinebilirsiniz. 
> [!div class="nextstepaction"]
> [Digital Twins nesne modellerini ve uzamsal zeka grafını anlama](concepts-objectmodel-spatialgraph.md)