---
title: 'Öğretici: Azure Digital Twins kurulumundan olay analizi yapma | Microsoft Docs'
description: Görselleştirme ve Bu öğreticide adımları kullanarak, Azure dijital İkizlerini boşluklarla Azure Time Series Insights, olaylarından analiz etme hakkında bilgi edinin.
services: digital-twins
author: alinamstanciu
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 12/18/2018
ms.author: alinast
ms.openlocfilehash: 3f6111457d3438b80ace8cd557747ab8c799efd3
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484748"
---
# <a name="tutorial-visualize-and-analyze-events-from-your-azure-digital-twins-spaces-by-using-time-series-insights"></a>Öğretici: Time Series Insights'ı kullanarak Azure dijital İkizlerini alanlarınıza olayları çözümleyin

Azure dijital İkizlerini örneğinizi dağıtma alanlarınıza sağlama ve belirli koşulları izlemek için özel bir işlev uygulamak sonra olayları ve eğilimleri ve anormallikleri için aramak için bir boşluk gelen veri görselleştirebilirsiniz.

İçinde [ilk öğreticide](tutorial-facilities-setup.md), sanal bir yapı uzamsal grafiği yapılandırılmış, hareket ve tasarruf edilen karbon dioksit sıcaklık algılayıcıları içeren bir odayla. [İkinci öğreticide](tutorial-facilities-udf.md) grafınızı ve bir kullanıcı tanımlı işlev sağladınız. İşlevi, bu algılayıcı değerlerini izler ve bildirimleri doğru koşulları için tetikler. Diğer bir deyişle, boş bir yer ve sıcaklık ve tasarruf edilen karbon dioksit düzeyleri normal.

Bu öğretici, Azure Time Series Insights ile Azure dijital İkizlerini kurulumunuzu gelen veri ve bildirimleri nasıl tümleştirebilirsiniz gösterir. Ardından, zaman içinde algılayıcı değerlerini görselleştirebilirsiniz. Hangi odası, çoğu kullanım alma ve günün en yoğun zamanları olduğu gibi eğilimlerini bakabilirsiniz. Ayrıca, hatalı havalandırma gösteren anormallikleri hangi odaları düşünüyor stuffier ve hotter veya bir alan, binada tutarlı bir şekilde yüksek sıcaklık değerleri olup gönderme gibi algılayabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Event Hubs kullanarak veri Stream.
> * Time Series Insights ile analiz edin.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide Azure Digital Twins kurulumunu [yapılandırmış](tutorial-facilities-setup.md) ve [sağlamış](tutorial-facilities-udf.md) olduğunuz kabul edilmektedir. Devam etmeden önce aşağıdakilere sahip olduğunuzdan emin olun:

- Bir [Azure hesabı](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Çalışan bir Digital Twins örneği.
- Çalışma makinenize indirilmiş ve ayıklanmış [Digital Twins C# örnekleri](https://github.com/Azure-Samples/digital-twins-samples-csharp).
- [.NET core SDK'sı sürüm 2.1.403 veya üzeri](https://www.microsoft.com/net/download) geliştirme makinenizde örneği çalıştırmak için. Çalıştırma `dotnet --version` doğru sürümünün yüklü olduğunu doğrulayın.

## <a name="stream-data-by-using-event-hubs"></a>Event Hubs kullanarak veri Stream

Kullanabileceğiniz [Event Hubs](../event-hubs/event-hubs-about.md) hizmet, veri akışı için bir işlem hattı oluşturursunuz. Bu bölümde, Azure dijital çiftleri ve Time Series Insights örnekleri arasında bağlayıcı olarak olay hub'ınızı oluşturma işlemini gösterir.

### <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol bölmede seçin **kaynak Oluştur**.

1. **Event Hubs** araması yapın ve sonuçlardan seçin. **Oluştur**’u seçin.

1. Girin bir **adı** Event Hubs ad alanınız için. Seçin **standart** için **fiyatlandırma katmanı**, **abonelik**, **kaynak grubu** dijital İkizlerini Örneğiniz için kullanılan ve **konumu**. **Oluştur**’u seçin.

1. Event Hubs ad alanı dağıtımı seçin **genel bakış** bölmesinde seçip **kaynağa Git**.

    ![Event Hubs ad alanı dağıtımdan sonra](./media/tutorial-facilities-analyze/open-event-hub-ns.png)

1. Event Hubs ad alanında **genel bakış** bölmesinde **olay hub'ı** üstünde düğme.
    ![Olay hub'ı düğmesi](./media/tutorial-facilities-analyze/create-event-hub.png)

1. Girin bir **adı** olay hub'ı ve seçin için **Oluştur**.

   Olay hub'ı dağıtıldıktan sonra görünür **Event Hubs** Bölmesi ile Event Hubs ad alanının bir **etkin** durumu. Açmak için bu olay hub'ı seçin, **genel bakış** bölmesi.

1. Seçin **tüketici grubu** düğmesinin üstünde ve gibi bir ad girin **tsievents** tüketici grubu için. **Oluştur**’u seçin.

    ![Olay hub'ı tüketici grubu](./media/tutorial-facilities-analyze/event-hub-consumer-group.png)

   Tüketici grubu oluşturulduktan sonra olay hub'ın altındaki listede görünür **genel bakış** bölmesi.

1. Açık **paylaşılan erişim ilkeleri** bölmesinde seçin ve olay hub'ı için **Ekle** düğmesi. Girin **ManageSend** tüm onay kutularının seçili olduğundan ve seçin ilke adı olarak emin **Oluştur**.

    ![Olay Hub'ı bağlantı dizeleri](./media/tutorial-facilities-analyze/event-hub-connection-strings.png)

1. Oluşturduğunuz ManageSend İlkesi'ni açın ve değerlerini kopyalayın **bağlantı dizesi – birincil anahtar** ve **bağlantı dizesi--ikincil anahtar** geçici bir dosya için. Sonraki bölümde olay hub'ı bir uç nokta oluşturmak için bu değerlere ihtiyacınız olacak.

### <a name="create-an-endpoint-for-the-event-hub"></a>Olay hub'ı için bir uç nokta oluşturma

1. Komut penceresinde nde olduğunuzdan emin olun **doluluk quickstart\src** Azure dijital İkizlerini örnek klasörü.

1. Dosyayı açmak **actions\createEndpoints.yaml** düzenleyicinizde. İçeriğini aşağıdakilerle değiştirin:

    ```yaml
    - type: EventHub
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hub
    - type: EventHub
      eventTypes:
      - DeviceMessage
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hub
    ```

1. Yer tutucuları değiştirmeniz `Primary_connection_string_for_your_event_hub` değeriyle **bağlantı dizesi – birincil anahtar** olay hub'ı. Bu bağlantı dizesinin biçimi şu şekilde olduğundan emin olun:

   ```plaintext
   Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey1GUID;EntityPath=nameOfYourEventHub
   ```

1. Yer tutucuları değiştirmeniz `Secondary_connection_string_for_your_event_hub` değeriyle **bağlantı dizesi--ikincil anahtar** olay hub'ı. Bu bağlantı dizesinin biçimi şu şekilde olduğundan emin olun: 

   ```plaintext
   Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey2GUID;EntityPath=nameOfYourEventHub
   ```

1. Yer tutucuları değiştirmeniz `Name_of_your_Event_Hub` olay hub'ınızın adıyla.

    > [!IMPORTANT]
    > Değerleri girerken tırnak işaretlerini dahil etmeyin. YAML dosyası iki nokta üst üste sonra en az bir boşluk karakteri olduğundan emin olun. Çevrimiçi bir YAML Doğrulayıcı gibi kullanarak, YAML dosyası içeriği doğrulayabilirsiniz [bu araç](https://onlineyamltools.com/validate-yaml).

1. Dosyayı kaydedin ve kapatın. Komut penceresinde aşağıdaki komutu çalıştırın ve istendiğinde Azure hesabınızda oturum açın.

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Olay hub'ınız için iki uç nokta oluşturur.

   ![Event Hubs uç noktaları](./media/tutorial-facilities-analyze/dotnet-create-endpoints.png)

## <a name="analyze-with-time-series-insights"></a>Time Series Insights ile analiz gerçekleştirme

1. Sol bölmesinde [Azure portalında](https://portal.azure.com)seçin **kaynak Oluştur**. 

1. **Time Series Insights** araması yapın ve yeni bir kaynak oluşturun. **Oluştur**’u seçin.

1. Time Series Insights örneğiniz için bir **Ad** girin ve **Abonelik** girişinizi seçin. Seçin **kaynak grubu** dijital İkizlerini Örneğiniz için kullanılan ve **konumu**. Seçin **sonraki: Olay kaynağı** düğmesini veya **olay kaynağı** sekmesi.

    ![Time Series Insights örneği oluşturma seçimleri](./media/tutorial-facilities-analyze/create-tsi.png)

1. İçinde **olay kaynağı** sekmesinde bir **adı**seçin **olay hub'ı** olarak **kaynak türünü**ve diğer değerleri seçili olduğundan emin olun doğru. Seçin **ManageSend** için **olay hub'ına erişim ilkesi adı**ve ardından önceki bölümünde oluşturduğunuz tüketici grubu seçin **olay hub'ı tüketici grubu**. **İncele ve oluştur**’u seçin.

    ![Olay kaynağını oluşturmak için seçim](./media/tutorial-facilities-analyze/tsi-event-source.png)

1. İçinde **gözden geçir + Oluştur** bölmesinde, girdiğiniz bilgileri gözden geçirin ve seçin **Oluştur**.

1. Dağıtım bölmesinde, az önce oluşturduğunuz Time Series Insights kaynağı seçin. Bu açılır **genel bakış** bölmesinde zaman serisi görüşleri ortamınız için.

1. Seçin **ortama Git** üstünde düğme. Veri erişim uyarı alırsanız, açık **veri erişimi ilkeleri** bölmesinde seçin, zaman serisi görüşleri örneği için **Ekle**seçin **katkıda bulunan** rol ve seçin uygun kullanıcı.

1. **Ortama Git** açılır düğme [Time Series Insights gezgininin](../time-series-insights/time-series-insights-explorer.md). Hiçbir olay görüntülenmiyorsa göz atarak cihaz olaylarının benzetimini yapma **cihaz bağlantısı** dijital İkizlerini örnek ve çalışan proje `dotnet run`.

1. Birkaç sanal olayları oluşturulduktan sonra Time Series Insights Gezgini geri dönün ve üst Yenile düğmesini seçin. Benzetimli sensör verileriniz için oluşturulan analitik grafikleri görmeniz gerekir. 

    ![Time Series Insights Gezgini içinde grafik](./media/tutorial-facilities-analyze/tsi-explorer.png)

1. Time Series Insights Gezgini içinde ardından grafikleri ve ısı haritaları için farklı olayları ve verileri, odaları, algılayıcılar ve diğer kaynakları oluşturabilirsiniz. Sol tarafındaki **ÖLÇÜ** ve **bölme tarafından** kendi görselleştirmelerinizi oluşturmak için açılan kutusu. 

   Örneğin, **olayları** için **ÖLÇÜ** ve **DigitalTwins SensorHardwareId** için **bölme tarafından**, her biri için bir ısı haritası oluşturmak için algılayıcılar. Isı Haritası aşağıdaki görüntüye benzer olacaktır:

   ![Time Series Insights Gezgininde ısı Haritası](./media/tutorial-facilities-analyze/tsi-explorer-heatmap.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure dijital İkizlerini bu noktanın ötesine keşfetmeye durdurmak istiyorsanız, bu öğreticide oluşturulan kaynakları silmek çekinmeyin:

1. Sol menüden [Azure portalında](https://portal.azure.com)seçin **tüm kaynakları**dijital İkizlerini kaynak grubunuzu seçin ve ardından **Sil**.

    > [!TIP]
    > Dijital İkizlerini örneğinizin silme sorun olduysa, bir hizmet güncelleştirmesi düzeltme alındı. Örneğiniz silme yeniden deneyin.

2. Gerekirse, iş makinenizde örnek uygulamaları silin.

## <a name="next-steps"></a>Sonraki adımlar

Uzamsal zeka graflar ve Azure dijital İkizlerini nesne modellerinde hakkında daha fazla bilgi edinmek için sonraki makaleye gidin.

> [!div class="nextstepaction"]
> [Digital Twins nesne modellerini ve uzamsal zeka grafını anlama](concepts-objectmodel-spatialgraph.md)
