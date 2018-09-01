---
title: Apache Kafka için Azure Stream Analytics'i kullanarak Event Hubs olay işleme | Microsoft Docs
description: Bu makalede, Azure Stream Analytics kullanarak event hubs'ı alınan Kafka olayları işlemek gösterilir
services: event-hubs
documentationcenter: ''
author: spelluru
manager: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2018
ms.author: spelluru
ms.openlocfilehash: a066d2a55f6949eea316eaf0a2956500667a996f
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43340280"
---
# <a name="process-apache-kafka-for-event-hubs-events-using-stream-analytics"></a>Stream analytics kullanarak Event Hubs olay için işlem Apache Kafka 
Bu makalede, veri akışı Kafka özellikli Event Hubs'a ve Azure Stream Analytics ile işlemek gösterilmektedir. Aşağıdaki adımları gösterilmektedir: 

1. Oluşturma bir Kafka Event Hubs ad alanı etkin.
2. Olay hub'ına ileti gönderen bir Kafka istemci oluşturun.
3. Olay hub'ından bir Azure blob depolama alanına veri kopyalayan bir Stream Analytics işi oluşturun. 

Protokol istemcilerinize değiştirmek veya bir olay hub'ı tarafından kullanıma sunulan Kafka uç noktası kullandığınızda, kendi kümeleri çalıştırın gerekmez. Azure Event Hubs [Apache Kafka sürüm 1.0](https://kafka.apache.org/10/documentation.html)’ı destekler. ve üstü. 


## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* Bir Maven ikili arşivini [indirin](http://maven.apache.org/download.cgi) ve [yükleyin](http://maven.apache.org/install.html).
* [Git](https://www.git-scm.com/)
* Bir **Azure depolama hesabı**. Biri yoksa [oluşturmak](../storage/common/storage-create-storage-account.md#create-a-storage-account) devam etmeden önce. Bu izlenecek yolda Stream Analytics işi çıktı verilerini bir Azure blob depolama alanında depolar. 


## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Kafka etkin Event Hubs ad alanı oluşturma

1. Oturum [Azure portalında](https://portal.azure.com), tıklatıp **kaynak Oluştur** , ekranın sol üst köşesindeki.
2. Arama **Event Hubs** ve burada gösterilen Seçenekler'i seçin:
    
    ![Portalda Event Hubs arama](./media/event-hubs-kafka-stream-analytics/select-event-hubs.png) 
3. Üzerinde **Event Hubs** sayfasında **Oluştur**.
4. Üzerinde **oluşturma Namespace** sayfasında, aşağıdaki eylemleri gerçekleştirin: 
    1. Benzersiz bir sağlamak **adı** ad alanı. 
    2. Seçin bir **fiyatlandırma katmanı**. 
    3. Seçin **Kafka etkinleştirme**. Bu adım, bir **önemli** adım. 
    4. Seçin, **abonelik** oluşturulacak olay hub'ı ad alanı istediğiniz. 
    5. Yeni bir **kaynak grubu** veya mevcut bir kaynak grubunu seçin. 
    6. Seçin bir **konumu**. 
    7. **Oluştur**’a tıklayın.
    
        ![Ad alanı oluşturma](./media/event-hubs-kafka-stream-analytics/create-event-hub-namespace-page.png) 
4. İçinde **bildirim iletisi**seçin **kaynak grubu adı**. 

    ![Ad alanı oluşturma](./media/event-hubs-kafka-stream-analytics/creation-station-message.png)
1. Seçin **olay hub'ı ad alanı** kaynak grubunda. 
2. Ad alanı oluşturduktan sonra seçin **paylaşılan erişim ilkeleri** altında **ayarları**.

    ![Paylaşılan erişim ilkeleri’ne tıklayın.](./media/event-hubs-kafka-stream-analytics/shared-access-policies.png)
5. Varsayılan **RootManageSharedAccessKey** ilkesini seçebilir veya yeni bir ilke ekleyebilirsiniz. Kopyalama ve ilke adını tıklayın **bağlantı dizesi**. Kafka istemciyi yapılandırmak için bağlantı dizesini kullanın. 
    
    ![İlke seçme](./media/event-hubs-kafka-stream-analytics/connection-string.png)  

Artık Kafka protokolünü kullanan uygulamalarınızdaki olayların akışını Event Hubs'a yapabilirsiniz.

## <a name="send-messages-with-kafka-in-event-hubs"></a>Olay hub'ları, Kafka ile iletileri gönder

1. Kopya [Azure Event Hubs depo](https://github.com/Azure/azure-event-hubs) makinenize.
2. Klasöre gidin: `azure-event-hubs/samples/kafka/quickstart/producer`. 
4. İçinde üretici için yapılandırma ayrıntılarını güncelleştirme `src/main/resources/producer.config`. Belirtin **adı** ve **bağlantı dizesi** için **olay hub'ı ad alanı**. 

    ```xml
    bootstrap.servers={EVENT HUB NAMESPACE}.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{CONNECTION STRING for EVENT HUB NAMESPACE}";
    ```

5. Gidin `azure-event-hubs/samples/kafka/quickstart/producer/src/main/java/com/example/app`açın **TestDataReporter.java** tercih ettiğiniz bir düzenleyicide dosya. 
6. Aşağıdaki kod satırını açıklama satırı yapın:

    ```java
                //final ProducerRecord<Long, String> record = new ProducerRecord<Long, String>(TOPIC, time, "Test Data " + i);
    ```
3. Aşağıdaki açıklama eklenen kod yerine kod satırını ekleyin: 

    ```java
                final ProducerRecord<Long, String> record = new ProducerRecord<Long, String>(TOPIC, time, "{ \"eventData\": \"Test Data " + i + "\" }");            
    ```

    Bu kod olay verileri gönderir **JSON** biçimi. Stream Analytics işine ilişkin giriş yapılandırdığınızda, giriş veri biçimi olarak JSON belirtin. 
7. **Üretici çalıştırma** ve Kafka özellikli Event hubs'ta akışa. Kullanırken Windows makinesinde, bir **Node.js komut istemi**, geçiş `azure-event-hubs/samples/kafka/quickstart/producer` bu komutları çalıştırmadan önce klasörü. 
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```

## <a name="verify-that-event-hub-receives-the-data"></a>Bu olay hub'ın verileri alan doğrulayın

1. Seçin **olay hub'ları** altında **varlıkları**. Adlı bir olay hub'ı gördüğünüzü onaylayın **test**. 

    ![Olay hub'ı - test etme](./media/event-hubs-kafka-stream-analytics/test-event-hub.png)
2. Olay hub'ına gelen iletileri gördüğünüzü onaylayın. 

    ![Olay hub'ı - iletileri](./media/event-hubs-kafka-stream-analytics/confirm-event-hub-messages.png)

## <a name="process-event-data-using-a-stream-analytics-job"></a>Bir Stream Analytics işi kullanarak olay verilerini işleme
Bu bölümde, Azure Stream Analytics işi oluşturun. Kafka istemci, olayları olay hub'ına gönderir. Olay verileri girdi olarak alır ve bir Azure blob depolama alanına çıkaran bir Stream Analytics işi oluşturun. Yoksa bir **Azure depolama hesabı**, [oluşturmak](../storage/common/storage-create-storage-account.md#create-a-storage-account).

Stream Analytics işinde sorgu, herhangi bir analiz yapmadan aracılığıyla verileri geçirir. Farklı bir biçimde veya elde edildi öngörülerle çıkış verileri üretmek üzere giriş verilerini dönüştüren bir sorgu oluşturabilirsiniz.  

### <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma 

1. Seçin **+ kaynak Oluştur** içinde [Azure portalında](https://portal.azure.com).
2. Seçin **Analytics** içinde **Azure Marketi** seçin ve menü **Stream Analytics işi**. 
3. Üzerinde **yeni Stream Analytics** sayfasında, aşağıdaki eylemleri gerçekleştirin: 
    1. Girin bir **adı** iş için. 
    2. Seçin, **abonelik**.
    3. Seçin **Yeni Oluştur** için **kaynak grubu** ve adını girin. Ayrıca **varolan kullanın** kaynak grubu. 
    4. Seçin bir **konumu** iş için.
    5. Seçin **Oluştur** işi oluşturmak için. 

        ![Yeni Stream Analytics işi](./media/event-hubs-kafka-stream-analytics/new-stream-analytics-job.png)

### <a name="configure-job-input"></a>İş girişi yapılandırma

1. Bildirim iletisinde seçin ** kaynak görmek için ** için Git **Stream Analytics işi** sayfası. 
2. Seçin **girişleri** içinde **iş TOPOLOJİSİ** sol menüde bölümü.
3. Seçin **akış Girişi Ekle**ve ardından **olay hub'ı**. 

    ![Girdi olarak Event hub'ı Ekle](./media/event-hubs-kafka-stream-analytics/select-event-hub-input.png)
4. Üzerinde **olay hub'ı giriş** yapılandırma sayfasında, aşağıdaki eylemleri gerçekleştirin: 

    1. Belirtin bir **diğer** giriş. 
    2. **Azure aboneliğinizi** seçin.
    3. Seçin **olay hub'ı ad alanı** oluşturduğunuz daha önce. 
    4. Seçin **test** için **olay hub'ı**. 
    5. **Kaydet**’i seçin. 

        ![Olay hub'ı giriş yapılandırma](./media/event-hubs-kafka-stream-analytics/event-hub-input-configuration.png)

### <a name="configure-job-output"></a>İş çıkışını yapılandırma 

1. Seçin **çıkışları** içinde **iş TOPOLOJİSİ** menüsünde bölümü. 
2. Seçin **+ Ekle** araç ve seçim **Blob Depolama**
3. Blob Depolama çıkış Ayarları sayfasında, aşağıdaki eylemleri gerçekleştirin: 
    1. Belirtin bir **diğer** çıktı. 
    2. Azure **aboneliğinizi** seçin. 
    3. Seçin, **Azure depolama hesabı**. 
    4. Girin bir **kapsayıcı adı** , Stream Analytics sorgu çıktı verilerini depolar.
    5. **Kaydet**’i seçin.

        ![BLOB Depolama çıktı yapılandırma](./media/event-hubs-kafka-stream-analytics/output-blob-settings.png)
 

### <a name="define-a-query"></a>Bir sorgu tanımlama
Gelen bir veri akışını okumak için bir Stream Analytics işi ayarladıktan sonraki adım, verileri gerçek zamanlı olarak analiz eden bir dönüştürme oluşturmaktır. Dönüştürme sorgusunu [Stream Analytics sorgu dilini](https://msdn.microsoft.com/library/dn834998.aspx) kullanarak tanımlarsınız. Bu izlenecek yolda, herhangi bir dönüştürme gerçekleştirmeden veri aktaran bir sorgu tanımlayabilirsiniz.

1. Seçin **sorgu**.
2. Sorgu penceresinde değiştirin `[YourOutputAlias]` ile daha önce oluşturduğunuz çıkış diğer adı.
3. Değiştirin `[YourInputAlias]` ile daha önce oluşturduğunuz giriş diğer adı. 
4. Araç çubuğunda **Kaydet**’i seçin. 

    ![Sorgu](./media/event-hubs-kafka-stream-analytics/query.png)


### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

1. Seçin **genel bakış** sol menüsünde. 
2. Seçin **Başlat**. 

    ![Başlat menüsü](./media/event-hubs-kafka-stream-analytics/start-menu.png)
1. Üzerinde **başlangıç işi** sayfasında **Başlat**. 

    ![Başlangıç işi sayfası](./media/event-hubs-kafka-stream-analytics/start-job-page.png)
1. İşin durumunu değişinceye kadar bekleyin **başlangıç** için **çalıştıran**. 

    ![İş durumu - çalışan](./media/event-hubs-kafka-stream-analytics/running.png)

## <a name="test-the-scenario"></a>Test senaryosu
1. Çalıştırma **Kafka üretici** olay hub'ına olayları yeniden gönderilecek. 

    ```shell
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
1. Gördüğünüzü onaylayın **çıktı verilerini** içinde oluşturulan **Azure blob depolama**. Aşağıdaki örnek satırlar gibi ara 100 satır kapsayıcıdaki bir JSON dosyası görürsünüz: 

    ```
    {"eventData":"Test Data 0","EventProcessedUtcTime":"2018-08-30T03:27:23.1592910Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    {"eventData":"Test Data 1","EventProcessedUtcTime":"2018-08-30T03:27:23.3936511Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    {"eventData":"Test Data 2","EventProcessedUtcTime":"2018-08-30T03:27:23.3936511Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    ```

    Azure Stream Analytics işi, giriş verileri olay hub'ından alınan ve bu senaryoda Azure blob depolamada depolanan. 



## <a name="next-steps"></a>Sonraki adımlar
Bu makalede protokol istemcilerinizi değiştirmenize veya kendi kümelerinizi çalıştırmanıza gerek kalmadan Kafka etkin Event Hubs’a nasıl akış oluşturacağınızı öğrendiniz. Daha fazla bilgi edinmek için aşağıdaki öğreticiyle devam edin:

> [!div class="nextstepaction"]
> [Event Hubs ile Kafka MirrorMaker'ı kullanma](event-hubs-kafka-mirror-maker-tutorial.md)
