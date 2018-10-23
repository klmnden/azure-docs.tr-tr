---
title: Azure Digital Twins kurulumundan olay analizi yapma | Microsoft Docs
description: Bu öğreticideki adımları kullanarak Azure Time Series Insights'ı kullanarak Azure Digital Twins alanlarınızdan gelen olayları görselleştirmeyi ve analiz etmeyi öğrenebilirsiniz.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 1366cafe5d2c526e86905c108b9c7b865aac8f69
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323469"
---
# <a name="tutorial-visualize-and-analyze-events-from-your-azure-digital-twins-spaces-using-time-series-insights"></a>Öğretici: Time Series Insights'ı kullanarak Azure Digital Twins alanlarınızdan gelen olayları görselleştirme ve analiz etme

Azure Digital Twins örneğinizi dağıttıktan, alanlarınızı sağladıktan ve belirli koşulları izleme amacıyla özel işlev uyguladıktan sonra alanlarınızdan gelen olayları ve verileri görselleştirerek eğilimleri ve anomalileri tespit edebilirsiniz. 

[Birinci öğreticide](tutorial-facilities-setup.md) hareket, karbondioksit ve sıcaklık sensörleri bulunan bir odaya sahip olan hayali bir binanın uzamsal grafını yapılandırdınız. [İkinci öğreticide](tutorial-facilities-udf.md) grafınızı ve bir kullanıcı tanımlı işlev sağladınız. İşlev, bu sensörlerden gelen değerleri izleyerek doğru koşullar oluştuğunda bildirimleri tetikler. Burada doğru koşullar, odanın boş, sıcaklık ve karbondioksit seviyelerinin de normal olmasıdır. Bu öğreticide Digital Twins kurulumunuzdan gelen bildirimlerle verileri Azure Time Series Insights ile tümleştirme adımları gösterilmektedir. Bu tümleştirmeden sonra sensör verilerinin zaman içindeki değişimini görselleştirebilir, en çok kullanılan oda ve günün en yoğun saatleri gibi eğilimleri tespit edebilirsiniz. Ayrıca daha dolu ve daha sıcak hissedilen odaları veya binanızda sürekli yüksek sıcaklık değerleri gönderen ve kliması arızalı olabilecek bir alanı belirleyebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Event Hubs kullanarak veri akışı sağlama
> * Time Series Insights ile analiz gerçekleştirme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide Azure Digital Twins kurulumunu [yapılandırmış](tutorial-facilities-setup.md) ve [sağlamış](tutorial-facilities-udf.md) olduğunuz kabul edilmektedir. Devam etmeden önce aşağıdakilere sahip olduğunuzdan emin olun:
- Bir [Azure hesabı](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Çalışan bir Digital Twins örneği.
- Çalışma makinenize indirilmiş ve ayıklanmış [Digital Twins C# örnekleri](https://github.com/Azure-Samples/digital-twins-samples-csharp).
- Örneği çalıştırmak için geliştirme makinenize yüklenmiş [.NET Core SDK sürüm 2.1.403 veya üzeri](https://www.microsoft.com/net/download). Doğru sürümün yüklü olup olmadığını denetlemek için `dotnet --version` komutunu çalıştırın. 


## <a name="stream-data-using-event-hubs"></a>Event Hubs kullanarak veri akışı sağlama
[Event Hubs](../event-hubs/event-hubs-about.md) hizmeti, veri akışı yapmak üzere bir işlem hattı oluşturmanızı sağlar. Bu bölümde, Digital Twins ile TSI örnekleriniz arasında bağlayıcı görevi görecek bir olay hub'ı oluşturma adımları gösterilmektedir.

### <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

1. [Azure portalda](https://portal.azure.com) oturum açın.

1. Sol gezinti panelinde **Kaynak oluştur**'a tıklayın. 

1. **Event Hubs** araması yapın ve sonuçlardan seçin. **Oluştur**’a tıklayın.

1. Event Hubs ad alanınız için bir **Ad** girin, *Standart* **Fiyatlandırma katmanı** girişini, **Abonelik** bilginizi ve Digital Twins örneğinizde kullandığınız **Kaynak grubu** ve **Konum** girişlerini seçin. **Oluştur**’a tıklayın. 

1. Dağıtım tamamlandıktan sonra Event Hubs ad alanı *dağıtımına* gidin ve **KAYNAK** bölümündeki ad alanına tıklayın.

    ![Olay hub'ı ad alanı](./media/tutorial-facilities-analyze/open-event-hub-ns.png)


1. Event Hubs ad alanı **Genel Bakış** bölmesinin üst kısmındaki **Olay Hub'ı** düğmesine tıklayın. 
    ![Olay Hub’ı](./media/tutorial-facilities-analyze/create-event-hub.png)

1. Olay hub'ınız için bir **Ad** girin ve **Oluştur**'a tıklayın. Dağıtım tamamlandıktan sonra Event Hubs ad alanının **Olay Hub'ları** bölmesinde *Etkin* **DURUM** ile gösterilir. Bu olay hub'ına tıklayarak **Genel Bakış** bölmesini açın.

1. Üstteki **Tüketici grubu** düğmesine tıklayın ve tüketici grubuna *tsievents* gibi benzersiz bir ad girin. **Oluştur**’a tıklayın.
    ![Olay Hub'ı tüketici grubu](./media/tutorial-facilities-analyze/event-hub-consumer-group.png)

   Oluşturma işlemi tamamlandıktan sonra tüketici grubu olay hub'ının **Genel Bakış** bölmesinin altındaki listede görünür. 

1. Olay hub'ınızın **Paylaşılan erişim ilkeleri** bölmesini açın ve **Ekle** düğmesine tıklayın. *ManageSend* adlı bir ilke **oluşturun** ve tüm onay kutularının işaretlenmiş olduğundan emin olun. 

    ![Olay Hub'ı bağlantı dizeleri](./media/tutorial-facilities-analyze/event-hub-connection-strings.png)

1. Oluşturduğunuz *ManageSend* ilkesini açın ve **Bağlantı dizesi--birincil anahtar** ile **Bağlantı dizesi--ikincil anahtar** değerlerini geçici bir dosyaya kopyalayın. Bu değerleri bir sonraki bölümde olay hub'ı için bir uç nokta oluşturma sırasında kullanacaksınız.

### <a name="create-endpoint-for-the-event-hub"></a>Olay hub'ı için uç nokta oluşturma

1. Komut penceresinde Digital Twins örneğinizin **_occupancy-quickstart\src** klasöründe olduğunuzdan emin olun.

1. **_actions\createEndpoints.yaml_** adlı dosyayı düzenleyicinizde açın. İçeriğini aşağıdakilerle değiştirin:

    ```yaml
    - type: EventHub
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hubs_namespace
    - type: EventHub
      eventTypes:
      - DeviceMessage
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hubs_namespace
    ```

1. `Primary_connection_string_for_your_event_hub` yer tutucularını olay hub'ınızın **Bağlantı dizesi--birincil anahtar** değeriyle değiştirin. Bağlantı dizesinin aşağıdaki biçimde olduğundan emin olun:
```
Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey1GUID;EntityPath=nameOfYourEventHub
```

1. `Secondary_connection_string_for_your_event_hub` yer tutucularını olay hub'ınızın **Bağlantı dizesi--ikincil anahtar** değeriyle değiştirin. Bağlantı dizesinin aşağıdaki biçimde olduğundan emin olun: 
```
Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey2GUID;EntityPath=nameOfYourEventHub
```

1. `Name_of_your_Event_Hubs_namespace` yer tutucularının yerine Event Hubs ad alanınızı yazın.

    > [!IMPORTANT]
    > Değerleri girerken tırnak işaretlerini dahil etmeyin. *YAML* dosyasında iki nokta sonrasında en az bir boşluk karakteri olduğundan emin olun. *YAML* dosyanızın içeriğini [bu araç](https://onlineyamltools.com/validate-yaml) gibi çevrimiçi YAML doğrulayıcılarını kullanarak da doğrulayabilirsiniz.


1. Dosyayı kaydedin ve kapatın. Komut penceresinde aşağıdaki komutu çalıştırın ve istendiğinde Azure hesabınızda oturum açın.

    ```cmd/sh
    dotnet run CreateEndpoints
    ```
   
   Bu komut, olay hub'ınız için iki uç nokta oluşturur.

   ![Event Hubs uç noktaları](./media/tutorial-facilities-analyze/dotnet-create-endpoints.png)

## <a name="analyze-with-time-series-insights"></a>Time Series Insights ile analiz gerçekleştirme

1. [Azure portalın](https://portal.azure.com) sol gezinti bölmesinde **Kaynak oluştur**'a tıklayın. 

1. **Time Series Insights** araması yapın ve yeni bir kaynak oluşturun. **Oluştur**’a tıklayın.

1. Time Series Insights örneğiniz için bir **Ad** girin ve **Abonelik** girişinizi seçin. Digital Twins örneğiniz için kullandığınız **Kaynak grubu** ve **Konum** girişlerini seçin. **Oluştur**’a tıklayın.

    ![TSI oluşturma](./media/tutorial-facilities-analyze/create-tsi.png)

1. Dağıtım tamamlandıktan sonra Time Series Insights ortamını ve ardından **Olay Kaynakları** bölmesini açın. Tüketici grubu eklemek için en üstteki **Ekle** düğmesine tıklayın.

1. **Yeni olay kaynağı** bölmesinde bir **Ad** girin ve diğer değerlerin doğru şekilde seçildiğinden emin olun. **Olay hub'ı ilke adı** olarak *ManageSend* girişini seçin ve ardından **Olay hub'ı tüketici grubu** için önceki bölümde oluşturduğunuz *tüketici grubunu* seçin. **Oluştur**’a tıklayın.

    ![TSI olay kaynağı](./media/tutorial-facilities-analyze/tsi-event-source.png)

1. Time Series Insights ortamınızın **Genel Bakış** bölmesini açın ve en üstteki **Ortama Git** düğmesine tıklayın. *Veri erişimi uyarısı* alırsanız TSI örneğinizin **Veri Erişim İlkeleri** bölmesini açın, **Ekle**'ye tıklayın, **Katkıda bulunan** rolünü seçin ve uygun kullanıcıyı belirleyin.

1. **Ortama Git** düğmesine tıkladığınızda [Time Series Insights Gezgini](../time-series-insights/time-series-insights-explorer.md) açılır. Gezginde hiçbir olay görünmüyorsa Digital Twins örneğinizin **_device-connectivity_** projesine gidip `dotnet run` komutunu çalıştırarak cihaz olayı simülasyonunu başlatın.

1. Birkaç sanal olay oluşturulduktan sonra Time Series Insights gezginine dönün ve üstteki yenile düğmesine tıklayın. Sanal sensör verilerinizle analitik grafiklerin oluşturulduğunu görmeniz gerekir. 

    ![TSI gezgini](./media/tutorial-facilities-analyze/tsi-explorer.png)

1. Time Series Insights gezgininde odalarınız, sensörleriniz ve diğer kaynaklarınızdan gelen farklı olaylar ve veriler için grafikler ve ısı haritaları oluşturabilirsiniz. Sol taraftaki **ÖLÇÜ** ve **BÖLME ÖLÇÜTÜ** açılır kutularına tıklayarak kendi görselleştirmelerinizi oluşturabilirsiniz. Örneğin **ÖLÇÜ** alanında *Olaylar* ve **BÖLME ÖLÇÜTÜ** alanında *DigitalTwins-SensorHardwareId* seçimi yaparak aşağıdaki görüntüde olduğu gibi sensörlerinizin her biri için bir ısı haritası oluşturun:

    ![TSI gezgini](./media/tutorial-facilities-analyze/tsi-explorer-heatmap.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kadar Azure Digital Twins incelemesi sizin için yeterliyse bu öğreticide oluşturulan kaynakları silebilirsiniz:

1. [Azure portalda](http://portal.azure.com) sol taraftaki menüden **Tüm kaynaklar**'a tıklayın, Digital Twins kaynak grubunuzu ve ardından **Sil**'i seçin.
2. Gerekirse çalışma makinenizdeki örnek uygulamaları da silebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

Azure Digital Twins hizmetindeki uzamsal zeka grafları ve nesne modelleri hakkında daha fazla bilgi edinmek için bir sonraki makaleye geçin. 
> [!div class="nextstepaction"]
> [Digital Twins nesne modellerini ve uzamsal zeka grafını anlama](concepts-objectmodel-spatialgraph.md)

