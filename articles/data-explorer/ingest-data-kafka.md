---
title: "Hızlı Başlangıç: Azure veri Gezgini'ne kafka'dan veri alma"
description: Bu hızlı başlangıçta, Azure kafka'dan Veri Gezgini içinde (yükle) veri alma öğrenin.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 11/19/2018
ms.openlocfilehash: 5c7d533cbd8a69b8fd9dcc704e7b83b0e476e499
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756638"
---
# <a name="quickstart-ingest-data-from-kafka-into-azure-data-explorer"></a>Hızlı Başlangıç: Azure veri Gezgini'ne kafka'dan veri alma
 
Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve yüksek oranda ölçeklenebilir veri keşfetme hizmetidir. Azure Veri Gezgini, Kafka'dan alma (veriler yükleniyor) olanağı sağlar. Kafka güvenilir bir şekilde sistemleri veya uygulamalar arasında veri taşıma gerçek zamanlı akış verisi işlem hatları oluşturmayı sağlayan bir dağıtılmış akış platformudur.
 
## <a name="prerequisites"></a>Önkoşullar
 
* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun. 
 
* [Test kümesi ve veritabanı](create-cluster-database-portal.md)
 
* [Örnek bir uygulama](https://github.com/Azure/azure-kusto-samples-dotnet/tree/master/kafka) veri üretir ve Kafka'ya gönderir

* Örnek uygulamayı çalıştırmak için [Visual Studio 2017 sürüm 15.3.2 veya üzeri](https://www.visualstudio.com/vs/)
 
## <a name="kafka-connector-setup"></a>Kafka bağlayıcı Kurulumu

Kafka bağlanmak için ölçeklenebilir ve güvenilir Apache Kafka ve diğer sistemler arasında veri akışını bir araçtır. Büyük veri koleksiyonlarını içine ve dışına Kafka taşıma bağlayıcılar hızlıca tanımlamak basit sağlar. Kafka ADX havuz Kafka'dan bağlayıcı olarak görev yapar.
 
### <a name="bundle"></a>Paket

Kafka yükleyebilirsiniz bir `.jar` özel bağlayıcı olarak davranan bir eklenti olarak. Örneğin üretmek için bir `.jar`, biz kodu yerel olarak kopyalayın ve Maven kullanarak oluşturun. 

#### <a name="clone"></a>Kopyala

```bash
git clone git://github.com:Azure/kafka-sink-azure-kusto.git
cd ./kafka-sink-azure-kusto/kafka/
```

#### <a name="build"></a>Oluşturma

Yerel olarak oluşturmak için Maven ile derleme bir `.jar` bağımlılıkları ile tamamlandı.

* JDK > 1.8 = [indirin](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Maven [indirin](https://maven.apache.org/install.html)
 

Kök dizin içinde *havuz azure kusto kafka*çalıştırın:

```bash
mvn clean compile assembly:single
```

### <a name="deploy"></a>Dağıtma 

Kafka eklentisini yükleyin. Docker'ı kullanarak bir dağıtım örneği şu yolda bulunabilir: [havuz azure kusto kafka](https://github.com/Azure/kafka-sink-azure-kusto#deploy)
 

Ayrıntılı belgeler üzerinde Kafka bağlayıcılar ve bunları dağıtma altında bulunabilir [Kafka bağlanma](https://kafka.apache.org/documentation/#connect) 

### <a name="example-configuration"></a>Örnek yapılandırma 
 
```config
name=KustoSinkConnector 
connector.class=com.microsoft.azure.kusto.kafka.connect.sink.KustoSinkConnector 
kusto.sink.flush_interval_ms=300000 
key.converter=org.apache.kafka.connect.storage.StringConverter 
value.converter=org.apache.kafka.connect.storage.StringConverter 
tasks.max=1 
topics=testing1 
kusto.tables.topics_mapping=[{'topic': 'testing1','db': 'daniel', 'table': 'TestTable','format': 'json', 'mapping':'TestMapping'}] 
kusto.auth.authority=XXX 
kusto.url=https://ingest-{mycluster}.kusto.windows.net/ 
kusto.auth.appid=XXX 
kusto.auth.appkey=XXX 
kusto.sink.tempdir=/var/tmp/ 
kusto.sink.flush_size=1000
```
 
## <a name="create-a-target-table-in-adx"></a>İçinde ADX hedef tablo oluşturma
 
İçinde ADX Kafka veri göndermek bir tablo oluşturun. Tablo kümeyi oluşturmak ve veritabanı sağlanan içinde **önkoşulları**.
 
1. Azure portalında, seçin ve küme gidin **sorgu**.
 
    ![Sorgu uygulama bağlantısı](media/ingest-data-event-hub/query-explorer-link.png)
 
1. Aşağıdaki komutu pencereye kopyalayıp **Çalıştır**'ı seçin.
 
    ```Kusto
    .create table TestTable (TimeStamp: datetime, Name: string, Metric: int, Source:string)
    ```
 
    ![Oluşturma sorgusunu çalıştırma](media/ingest-data-event-hub/run-create-query.png)
 
1. Aşağıdaki komutu pencereye kopyalayıp **Çalıştır**'ı seçin.
 
    ```Kusto
    .create table TestTable ingestion json mapping 'TestMapping' '[{"column":"TimeStamp","path":"$.timeStamp","datatype":"datetime"},{"column":"Name","path":"$.name","datatype":"string"},{"column":"Metric","path":"$.metric","datatype":"int"},{"column":"Source","path":"$.source","datatype":"string"}]'
    ```

    Bu komut gelen JSON verilerini tablonun (TestTable) sütun adları ve veri türleriyle eşler.


## <a name="generate-sample-data"></a>Örnek veri oluşturma

Kafka kümesi için ADX bağlıysa, kullanın [örnek uygulaması](https://github.com/Azure-Samples/event-hubs-dotnet-ingest) verileri oluşturmak için indirilen.

### <a name="clone"></a>Kopyala

Örnek uygulamayı yerel olarak kopyalayın:

```cmd
git clone git://github.com:Azure/azure-kusto-samples-dotnet.git
cd ./azure-kusto-samples-dotnet/kafka/
```

### <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Örnek uygulama çözümünü Visual Studio'da açın.

1. İçinde `Program.cs` dosyası, güncelleştirme `connectionString` Kafka bağlantı dizenizi için sabit.

    ```csharp    
    const string connectionString = @"<YourConnectionString>";
    ```

1. Uygulamayı derleyin ve çalıştırın. Uygulama için Kafka kümesinin iletileri gönderir ve 10 saniyede durumunu yazdırır.

1. Uygulamayı birkaç ileti gönderdikten sonra sonraki adıma geçin.
 
## <a name="query-and-review-the-data"></a>Sorgulamak ve verileri gözden geçirin

1. Emin olmak için alımı sırasında hata oluştu:

    ```Kusto
    .show ingestion failures
    ```

1. Yeni alınan verileri görmek için:

    ```Kusto
    TestTable 
    | count
    ```

1. İleti içeriğini görmek için:
 
    ```Kusto
    TestTable
    ```
 
    Sonuç kümesi, aşağıdaki gibi görünmelidir:
 
    ![İleti sonuç kümesi](media/ingest-data-event-hub/message-result-set.png)
 
## <a name="next-steps"></a>Sonraki adımlar
 
> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure veri Gezgini'nde verileri Sorgulama](web-query-data.md)
