---
title: "Apache Spark Azure hdınsight'ta Event Hubs ile akış kullanın | Microsoft Docs"
description: "Örnek veri akışı Azure Event Hub'ına olayları alıp göndermek sonra bu scala uygulaması kullanarak Hdınsight Spark kümesinde konusunda akış bir Apache Spark oluşturun."
keywords: "Apache spark akış, spark akış, spark örnek, apache spark akış örneği, olay hub'ı azure örneği, spark örnek"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 68894e75-3ffa-47bd-8982-96cdad38b7d0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: jgao
ms.openlocfilehash: 43ae956ca284485cc68f8120a31af1c493c0b254
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="apache-spark-streaming-process-data-from-azure-event-hubs-with-spark-cluster-on-hdinsight"></a>Apache Spark akış: Hdınsight'ta Spark ile Azure Event Hubs işlem verilerini küme

Bu makalede, aşağıdaki adımları içerir örnek akış bir Apache Spark oluşturun:

1. Bir Azure olay Hub'ına iletileri almak için bir tek başına uygulamayı kullanın.

2. İki farklı yaklaşım ile gerçek zamanlı Azure Hdınsight'ta Spark kümesinde çalışan bir uygulama kullanarak olay Hub'ından iletileri alır.

3. Verileri farklı saklama sistemlerine kalıcı hale getirmek için analitik komut zincirleri akış yapı veya anında verilerinden öngörü edinme.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Hdınsight'ta bir Apache Spark kümesi. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark oluşturmak kümeleri](apache-spark-jupyter-spark-sql.md).

## <a name="spark-streaming-concepts"></a>Spark akış kavramları

Spark akış ayrıntılı bir açıklaması için bkz: [Apache Spark genel bakış akış](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). Hdınsight, Azure üzerinde bir Spark kümesi için aynı akış özellikleri getirir.  

## <a name="what-does-this-solution-do"></a>Bu çözüm ne yapar?

Bu makaledeki örnek akış Spark oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Bir akış olayların alacak bir Azure Event Hub oluşturun.

2. Olaylar oluşturur ve Azure Event Hub'ına iter yerel tek başına uygulamayı çalıştırın. Bu örnek uygulama yayımlanma [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).

3. Çeşitli veri işleme/analiz gerçekleştirmek ve bir akış uygulaması Azure olay Hub'ından akış olayları okur bir Spark kümesi üzerinde uzaktan çalıştırabilirsiniz.

## <a name="create-an-azure-event-hub"></a>Bir Azure olay hub'ı Oluştur

1. Oturum [Azure Portal](https://ms.portal.azure.com), tıklatıp **yeni** en üst ekranın sol.

2. **Nesnelerin İnterneti**’ne ve ardından **Event Hubs**’a tıklayın.

    ![Örnek Spark akış Create event hub](./media/apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "örnek Spark akış Create event hub")

3. **Ad alanı oluştur** dikey penceresine bir ad alanı adı girin. Fiyatlandırma Katmanı (temel veya standart) seçin. Ayrıca, bir Azure aboneliği, kaynak grubu ve kaynağın oluşturulacağı konumu seçin. Ad alanını oluşturmak için **Oluştur**’a tıklayın.

      ![Bir örnek Spark akış için olay hub'ı ad](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "örnek Spark akış için bir olay hub'ı adı belirtin")

    > [!NOTE]
    > Aynı seçmelisiniz **konumu** gecikme süresini ve maliyetleri azaltmak hdınsight'ta Apache Spark kümeniz.
    >
    >

4. Event Hubs ad alanı listesinde yeni oluşturulan ad alanına tıklayın.      


5. Ad alanı dikey penceresinde tıklayın **Event Hubs**ve ardından **+ olay hub'ı** yeni bir olay hub'ı oluşturmak için.
   
    ![Örnek Spark akış Create event hub](./media/apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "örnek Spark akış Create event hub")

6. Olay Hub'ınız için bir ad yazın, bölüm sayısı 10 ve ileti bekletme 1 olarak ayarlayın. Geri kalan varsayılan olarak bırakın ve ardından Biz bu çözüm iletilerinde arşivleme değil **oluşturma**.
   
    ![Olay hub'ı ayrıntılarını sağlamak için örnek akış Spark](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "için Spark akış örnek olay hub'ı ayrıntılarını sağlayın")

7. Yeni oluşturulan olay hub'ı olay Hub dikey penceresinde listelenir.
    
     ![Olay hub'ı görüntülemek için örnek akış Spark](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "örnek akış Spark için Görünüm olay hub'ı")

8. Ad alanı dikey penceresine (ilgili Olay Hub’ı dikey penceresine değil) geri dönerek **Paylaşılan erişim ilkeleri** ve ardından **RootManageSharedAccessKey** öğesine tıklayın.
    
     ![Olay hub'ı ilkeler için örnek akış Spark ayarlama](./media/apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "örnek akış Spark için Event Hub'ı ayarlamak ilkeleri")

9. Kopyalamak için Kopyala düğmesini tıklatın **RootManageSharedAccessKey** birincil anahtar ve bağlantı dizesini panoya. Bu öğreticide daha sonra kullanmak üzere kaydedin.
    
     ![Olay hub'ı İlkesi anahtarları için örnek akış Spark görüntülemek](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "görünüm olay hub'ı ilke anahtarları için Spark akış örneği")

## <a name="send-messages-to-azure-event-hub-using-a-sample-scala-application"></a>Azure Event kullanan bir örnek Scala uygulamaları Hub'ına iletileri gönder

Bu bölümdeki olayların bir akış oluşturur ve Azure Event Hub için daha önce oluşturduğunuz gönderen bir tek başına yerel Scala uygulama kullanın. Bu uygulama github'da kullanılabilir [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer). Adımlar burada bu GitHub deposunu zaten çatallanmış varsayar.

1. Bu uygulamayı çalıştırdığınız bilgisayarda yüklü olduğundan emin olun.

    * Oracle Java Geliştirme Seti. Şuradan yükleyebilirsiniz [burada](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
    * Apache Maven. Buradan indirebilirsiniz [burada](https://maven.apache.org/download.cgi). Maven yüklemeye yönelik yönergeleri [burada](https://maven.apache.org/install.html).

2. Bir komut istemi açın ve uygulama oluşturmak için aşağıdaki komutu çalıştırın ve örnek Scala uygulama için GitHub deposuna kopyalanabilir konumuna gidin.

        mvn package

3. Uygulama için çıktı jar **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, altında oluşturulan **/target** dizin. Bu makalenin sonraki bölümlerinde bu JAR eksiksiz çözüm sınamak için kullanın.

## <a name="create-application-to-receive-messages-from-event-hub-into-a-spark-cluster"></a>Olay Hub'ından bir Spark küme içinde iletileri almak için uygulama oluşturma 

Biz, Spark akış ve Azure Event Hubs, alıcı tabanlı bağlantısı ve doğrudan-DStream tabanlı bağlantısı bağlanmak için iki yaklaşım vardır. Doğrudan-DStream tabanlı 2017 Oca üzerinde sunulan 2.0.3 serbest bırakın. Daha fazla kullanıcı olduğu gibi özgün alıcı tabanlı bağlantıyı değiştirmek için beklenen ve verimli kaynak. Daha fazla ayrıntı bulunan [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs). Doğrudan DStream yalnızca Spark 2.0 + destekler.

### <a name="build-applications-with-the-dependency-to-spark-eventhubs-connector"></a>Spark eventhubs bağlayıcı bağımlılık uygulamaları derleme

Spark EventHubs hazırlama sürümünü de Github'da yayımlar. Spark EventHubs hazırlama sürümünü kullanmak için ilk adım GitHub kaynak deposu pom.xml için şu girdiyi ekleyerek belirtmektir:

```xml
<repository>
      <id>spark-eventhubs</id>
      <url>https://raw.github.com/hdinsight/spark-eventhubs/maven-repo/</url>
      <snapshots>
        <enabled>true</enabled>
        <updatePolicy>always</updatePolicy>
      </snapshots>
</repository>
```

Yayın öncesi sürüm yapılacak projenize aşağıdaki bağımlılığı daha sonra ekleyebilirsiniz.

Maven bağımlılığı

```xml
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11 -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-streaming-eventhubs_2.11</artifactId>
    <version>2.0.4</version>
</dependency>
```

SBT bağımlılık

```
// https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11
libraryDependencies += "com.microsoft.azure" % "spark-streaming-eventhubs_2.11" % "2.0.4"
```

### <a name="direct-dstream-connection"></a>Doğrudan DStream bağlantı

İçinde doğrudan DStream kullanan örnekler içeren bir önceden derlenmiş jar dosyasını karşıdan [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).

Kaynak kodu kullanılabilir üç örnekler jar dosyasını içeren [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).

Alma [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) bir örnek olarak:

```scala
private def createStreamingContext(
  sparkCheckpointDir: String,
  batchDuration: Int,
  namespace: String,
  eventHunName: String,
  eventhubParams: Map[String, String],
  progressDir: String) = {
val ssc = new StreamingContext(new SparkContext(), Seconds(batchDuration))
ssc.checkpoint(sparkCheckpointDir)
val inputDirectStream = EventHubsUtils.createDirectStreams(
  ssc,
  namespace,
  progressDir,
  Map(eventHunName -> eventhubParams))

inputDirectStream.map(receivedRecord => (new String(receivedRecord.getBody), 1)).
  reduceByKeyAndWindow((v1, v2) => v1 + v2, (v1, v2) => v1 - v2, Seconds(batchDuration * 3),
    Seconds(batchDuration)).print()

ssc
}

def main(args: Array[String]): Unit = {

if (args.length != 8) {
  println("Usage: program progressDir PolicyName PolicyKey EventHubNamespace EventHubName" +
    " BatchDuration(seconds) Spark_Checkpoint_Directory maxRate")
  sys.exit(1)
}

val progressDir = args(0)
val policyName = args(1)
val policykey = args(2)
val namespace = args(3)
val name = args(4)
val batchDuration = args(5).toInt
val sparkCheckpointDir = args(6)
val maxRate = args(7)

val eventhubParameters = Map[String, String] (
  "eventhubs.policyname" -> policyName,
  "eventhubs.policykey" -> policykey,
  "eventhubs.namespace" -> namespace,
  "eventhubs.name" -> name,
  "eventhubs.partition.count" -> "32",
  "eventhubs.consumergroup" -> "$Default",
  "eventhubs.maxRate" -> s"$maxRate"
)

val ssc = StreamingContext.getOrCreate(sparkCheckpointDir,
  () => createStreamingContext(sparkCheckpointDir, batchDuration, namespace, name,
    eventhubParameters, progressDir))

ssc.start()
ssc.awaitTermination()
}
```

Yukarıdaki örnekte, `eventhubParameters` parametreleri tek bir EventHubs örnek özeldir ve ona geçirmek zorunda `createDirectStreams` bir olay hub'ları ad alanı için doğrudan DStream nesne eşleme oluşturur API. Doğrudan DStream nesnesi üzerinde Spark akış API çerçevesi tarafından sağlanan herhangi bir DStream API'yi çağırabilirsiniz. Bu örnekte, biz son 3 mikro toplu aralıkları her sözcüğün sıklığını hesaplayın.

### <a name="receiver-based-connection"></a>Alıcı tabanlı bağlantı

Örnek uygulama olaylarını alır ve farklı hedefler yönlendirmek, Scala içinde yazılmış akış Spark şu adresten edinilebilir [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples). Olay hub'ı yapılandırmanızı için uygulamayı güncelleştir ve çıktı jar oluşturmak için aşağıdaki adımları izleyin.

1. Intellij Idea başlatın ve başlatma ekranından seçin **sürüm denetiminden kullanıma** ve ardından **Git**.
   
    ![Apache Spark örnek - Git get kaynaklardan akış](./media/apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark örnek - Git get kaynaklardan akış")

2. İçinde **kopya deposu** iletişim kutusunda, kopyası, için kopyalayın ve ardından dizin belirlemek için Git deposu URL'sini sağlayın **kopya**.
   
    ![Apache Spark örnek - Git kopya akış](./media/apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark örnek - Git kopya akış")
3. Proje tamamen kopyalanması kadar istemleri izleyin. Tuşuna **Alt + 1** açmak için **proje görünümü**. Aşağıdakine benzemelidir.
   
    ![Apache Spark örnek - proje görünümü akış](./media/apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark akış örnek - proje görünümü")
4. Uygulama kodu ile Java8 derlendiğinden emin olun. Bunu sağlamak için **dosya**, tıklatın **Proje yapısı**ve **proje** sekmesinde, dil düzeyi ayarlandığından emin projeyi **8 - Lambda'lar, türü Ek açıklamalar, vb**.
   
    ![Apache Spark örnek - kümesi derleyici akış](./media/apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark örnek - kümesi derleyici akış")
5. Açık **pom.xml** ve Spark sürüm doğru olduğundan emin olun. Altında `<properties>` düğümü için aşağıdaki kod parçacığında arayın ve Spark sürüm doğrulayın.

        <scala.version>2.11.8</scala.version>
        <scala.compat.version>2.11.8</scala.compat.version>
        <scala.binary.version>2.11</scala.binary.version>
        <spark.version>2.0.0</spark.version>

6. Uygulama adı verilen bir bağımlılık jar gerektiriyor **JDBC sürücüsü jar**. Bu, bir Azure SQL veritabanına olay Hub'ından alınan iletileri yazmak için gereklidir. Bu jar yükleyebilirsiniz (v4.1 veya sonrası) gelen [burada](https://msdn.microsoft.com/sqlserver/aa937724.aspx). Proje Kitaplığı'nda bu jar başvuru ekleyin. Aşağıdaki adımları gerçekleştirin:
     
     1. Sahip olduğu açık uygulama Intellij Idea penceresinden tıklatın **dosya**, tıklatın **proje yapısını**ve ardından **kitaplıkları**. 
     2. Ekle simgesini tıklatın (![Ekle simgesi](./media/apache-spark-eventhub-streaming/add-icon.png)), tıklatın **Java**ve ardından JDBC sürücüsü jar indirdiğiniz konuma gidin. Proje kitaplığına jar dosyasına eklemek için istemleri izleyin.

         ![eksik bağımlılıkları ekleyin](./media/apache-spark-eventhub-streaming/add-missing-dependency-jars.png "eksik bağımlılık Kavanoz ekleme")
     3. **Uygula**'ya tıklayın.

7. Çıktı jar dosyasını oluşturun. Aşağıdaki adımları gerçekleştirin.

   1. İçinde **proje yapısını** iletişim kutusu, tıklatın **yapıları** artı simgesine tıklayın. Açılan iletişim kutusundan tıklatın **JAR**ve ardından **bağımlılıkları olan modüller gelen**.      
       
       ![Apache Spark akış örnek - JAR oluşturma](./media/apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark akış örnek - JAR oluşturma")
   2. İçinde **oluşturma JAR modüllerden** iletişim kutusunda, üç nokta işaretine (![üç nokta](./media/apache-spark-eventhub-streaming/ellipsis.png)) karşı **ana sınıfı**.
   3. İçinde **ana sınıftan** iletişim kutusu, kullanılabilir sınıfların birini seçin ve ardından **Tamam**.
      
       ![Apache Spark örnek - select sınıfı jar için akış](./media/apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark akış örnek - jar için select sınıfı")
   4. İçinde **oluşturma JAR modüllerden** iletişim kutusunda, olduğundan emin olun seçeneği **hedef JAR ayıklamak** seçilir ve ardından **Tamam**. Bu, tüm bağımlılıkları olan tek bir JAR oluşturur.
      
       ![Apache Spark akış örnek - jar modüllerden oluşturma](./media/apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark akış örnek - jar modüllerden oluşturma")
   5. **Çıkış düzeni** sekmesi Maven projenin bir parçası olarak dahil tüm Kavanoz listeler. Seçin ve üretileceği olanları Sil Scala uygulama doğrudan hiçbir bağımlılık içeriyor. Biz burada oluşturma uygulama için tüm sonuncu kaldırabilirsiniz (**spark-akış-data-Kalıcılık-örnekleri derleme çıktı**). Silin ve ardından Kavanoz seçin **silmek** simgesi (![Sil simgesi](./media/apache-spark-eventhub-streaming/delete-icon.png)).
      
       ![Apache Spark örnek - ayıklanan delete Kavanoz akış](./media/apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark örnek - ayıklanan delete Kavanoz akış")
      
       Emin olun **olun yapı** kutusu seçiliyse, proje oluşturulmuş veya güncelleştirilmiş her zaman jar oluşturulur sağlar. **Uygula**'ya tıklayın.
   6. İçinde **çıkış düzeni** sağ ekranın alt kısmındaki sekme **kullanılabilir öğeleri** kutusunda proje Kitaplığı'na daha önce eklediğiniz SQL JDBC jar sahip. Bu eklemelisiniz **çıkış düzeni** sekmesi. Jar dosyasını sağ tıklatın ve ardından **ayıklamak halinde çıkış kök**.
      
       ![Apache Spark örnek - extract bağımlılık jar akış](./media/apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark örnek - extract bağımlılık jar akış")  
      
       **Çıkış düzeni** sekmesini şimdi şöyle görünmelidir.
      
       ![Apache Spark örnek - son çıktı sekmesi akış](./media/apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark akış örnek - son çıktı sekmesi")        
      
       İçinde **proje yapısını** iletişim kutusu, tıklatın **Uygula** ve ardından **Tamam**.    
   7. Menü çubuğundaki **yapı**ve ardından **olun proje**. Tıklatarak **derleme yapıları** jar oluşturmak için. Çıktı jar altında oluşturulan **\classes\artifacts**.
      
       ![Apache Spark akış örnek - çıktı JAR](./media/apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "Apache Spark akış örnek - çıktı JAR")

## <a name="run-the-application-remotely-on-a-spark-cluster-using-livy"></a>Uygulamayı uzaktan Livy kullanarak Spark kümesinde çalıştırın

Bu makalede bir Spark kümesinde uzaktan uygulama yayınlamayı Apache Spark çalıştırmak için Livy kullanın. Livy Hdınsight Spark kümesinde ile kullanma hakkında ayrıntılı bilgi için bkz: [gönderme işleri uzaktan bir Apache Spark küme Azure Hdınsight'ta](apache-spark-livy-rest-interface.md). Uygulama yayınlamayı Spark çalıştıran başlamadan önce şunları yapmalısınız vardır:

1. Olay Hub'ına gönderilen ve olaylar oluşturmak için yerel tek başına uygulamayı başlatın. Bunu yapmak için aşağıdaki komutu kullanın:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "mysbnamespace" --eventhubs-name "myeventhub" --policy-name "mysendpolicy" --policy-key "<policy key>" --message-length 32 --thread-count 32 --message-count -1

2. Akış jar kopyalayın (**spark akış-data-Kalıcılık-examples.jar**) kümesi ile ilişkili Azure Blob Depolama. Bu jar Livy için erişilebilir hale getirir. Kullanabileceğiniz [ **AzCopy**](../../storage/common/storage-use-azcopy.md), bunu yapmak için bir komut satırı yardımcı programı. Verileri yüklemek için kullanabileceğiniz diğer istemcilerin çok vardır. Onları hakkında daha fazla bilgiyi [hdınsight'ta Hadoop işleri için verileri karşıya yükleme](../hdinsight-upload-data.md).
3. CURL Bu uygulamalardan çalıştırdığınız bilgisayara yükleyin. İşlerini uzaktan çalıştırmak için Livy uç noktaları çağrılacak CURL kullanırız.

### <a name="run-the-spark-streaming-application-to-receive-the-events-into-an-azure-storage-blob-as-text"></a>Bir Azure Storage Blobuna metin olarak içine olaylarını almak için uygulama yayınlamayı Spark çalıştırın

Bir komut istemi açın, CURL yüklediğiniz dizine gidin ve aşağıdaki komutu (kullanıcı adı/parola ve küme adı değiştir) çalıştırın:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputBlob.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Parametreler dosyasında **inputBlob.txt** şu şekilde tanımlanır:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsEventCount", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Bize giriş dosyası parametrelerinde neler olduğunu anlama:

* **Dosya** kümesi ile ilişkili Azure depolama hesabı üzerinde uygulama jar dosyasını yoludur.
* **className** jar sınıfında adıdır.
* **bağımsız değişken** sınıfı tarafından gerekli bağımsız değişkenleri listesi
* **numExecutors** tarafından Spark akış uygulamayı çalıştırmak için kullanılan çekirdek sayısı. Bu, her zaman en az iki kez olay hub'ı bölüm sayısı olmalıdır.
* **executorMemory**, **executorCores**, **driverMemory** gerekli kaynakları akış uygulamaya atamak için kullanılan parametreler bulunur.

> [!NOTE]
> Parametre olarak kullanılan çıkış klasörler (EventCheckpoint, EventCount/EventCount10) oluşturmanız gerekmez. Akış uygulama bunları sizin için oluşturur.
>
>

Komutu çalıştırdığınızda, aşağıdakine benzer bir çıktı görmeniz gerekir:

    < HTTP/1.1 201 Created
    < Content-Type: application/json; charset=UTF-8
    < Location: /18
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Tue, 01 Dec 2015 05:39:10 GMT
    < Content-Length: 37
    <
    {"id":1,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

Çıkış ('1'. Bu örnekte) son satırında toplu iş kimliği not edin. Uygulamanın başarıyla çalıştığını doğrulamak için Azure depolama hesabınızın kümeyle ilişkili bakabilirsiniz ve görmelisiniz **/EventCount/EventCount10** oluşturulan klasör. Bu klasör, belirtilen süre içinde parametresi için işlenen olay sayısı yakalar BLOB'ları içermelidir **saniye içinde toplu iş aralığı**.

Uygulama yayınlamayı Spark, KILL kadar çalışmaya devam eder. Bunu yapmak için aşağıdaki komutu kullanın:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/1"

### <a name="run-the-applications-to-receive-the-events-into-an-azure-storage-blob-as-json"></a>Bir Azure Storage blobu JSON olarak içine olaylarını almak için uygulamaları çalıştırma
Bir komut istemi açın, CURL yüklediğiniz dizine gidin ve aşağıdaki komutu (kullanıcı adı/parola ve küme adı değiştir) çalıştırın:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputJSON.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Parametreler dosyasında **inputJSON.txt** şu şekilde tanımlanır:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureBlobAsJSON", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-store-folder", "/EventStore10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Parametreleri ne için metin çıktısı önceki adımda belirttiğiniz için benzerdir. Yeniden, parametre olarak kullanılan çıkış klasörler (EventCheckpoint, EventCount/EventCount10) oluşturmanız gerekmez. Akış uygulama bunları sizin için oluşturur.

 Komutu çalıştırın, kümeyle ilişkili Azure depolama hesabınızın bakabilir ve görmeniz gerekir sonra **/EventStore10** oluşturulan klasör. Açık herhangi bir dosyayı önekine sahip **bölümü -** ve bir JSON biçiminde işlenen olayların görmeniz gerekir.

### <a name="run-the-applications-to-receive-the-events-into-a-hive-table"></a>Hive tabloya olaylarını almak için uygulamaları çalıştırma
Bir Hive tabloya olayları akışları uygulama yayınlamayı Spark çalıştırmak için bazı ek bileşenleri gerekir. Bunlar:

* API jdo 3.2.6.jar datanucleus
* datanucleus rdbms 3.2.9.jar
* datanucleus çekirdek 3.2.10.jar
* Hive-site.xml

**.Jar** konumunda Hdınsight Spark kümenize dosyaları kullanılabilir `/usr/hdp/current/spark-client/lib`. **Hive-site.xml** şu adresten edinilebilir `/usr/hdp/current/spark-client/conf`.

Kullanabileceğiniz [WinScp](http://winscp.net/eng/download.php) bu dosyalar, yerel bilgisayarınıza kümeden kopyalamak için. Ardından, bu dosyalar üzerinde kümeyle ilişkili depolama hesabınıza kopyalamak için araçları da kullanabilirsiniz. Depolama hesabı dosyaları karşıya yükleme hakkında daha fazla bilgi için bkz: [hdınsight'ta Hadoop işleri için verileri karşıya yükleme](../hdinsight-upload-data.md).

Azure depolama hesabınıza dosyalar kopyalandıktan sonra bir komut istemi açın, CURL yüklediğiniz dizine gidin ve aşağıdaki komutu (kullanıcı adı/parola ve küme adı değiştir) çalıştırın:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputHive.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Parametreler dosyasında **inputHive.txt** şu şekilde tanımlanır:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToHiveTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-hive-table", "EventHiveTable10" ], "jars":["wasb:///example/jars/datanucleus-api-jdo-3.2.6.jar", "wasb:///example/jars/datanucleus-rdbms-3.2.9.jar", "wasb:///example/jars/datanucleus-core-3.2.10.jar"], "files":["wasb:///example/jars/hive-site.xml"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Parametreleri ne için metin çıktısı, önceki adımda belirttiğiniz için benzerdir. Yeniden Çıkış klasörleri (EventCheckpoint, EventCount/EventCount10) veya çıkış oluşturma gerekmez Hive tablosu (EventHiveTable10) parametreleri kullanılır. Akış uygulama bunları sizin için oluşturur. Unutmayın **Kavanoz** ve **dosyaları** seçenek .jar dosya ve depolama hesabına üzerinden kopyalanan hive-site.xml yollara içerir.

Hive tablosu başarıyla oluşturulduğunu doğrulamak için kullanılan [Ambari Hive görünümünü](../hadoop/apache-hadoop-use-hive-ambari-view.md). Tablosunun içeriğini görüntülemek için SELECT bir sorgu çalıştırabilirsiniz.

    SELECT * FROM eventhivetable10 LIMIT 10;

Aşağıdaki gibi bir çıktı görmeniz gerekir:

    ZN90apUSQODDTx7n6Toh6jDbuPngqT4c
    sor2M7xsFwmaRW8W8NDwMneFNMrOVkW1
    o2HcsU735ejSi2bGEcbUSB4btCFmI1lW
    TLuibq4rbj0T9st9eEzIWJwNGtMWYoYS
    HKCpPlWFWAJILwR69MAq863nCWYzDEw6
    Mvx0GQOPYvPR7ezBEpIHYKTKiEhYammQ
    85dRppSBSbZgThLr1s0GMgKqynDUqudr
    5LAWkNqorLj3ZN9a2mfWr9rZqeXKN4pF
    ulf9wSFNjD7BZXCyunozecov9QpEIYmJ
    vWzM3nvOja8DhYcwn0n5eTfOItZ966pa
    Time taken: 4.434 seconds, Fetched: 10 row(s)


### <a name="run-the-applications-to-receive-the-events-into-an-azure-sql-database-table"></a>Bir Azure SQL veritabanı tablosuna olaylarını almak için uygulamaları çalıştırma
Bu adımı çalıştırmadan önce oluşturulan bir Azure SQL veritabanı olduğundan emin olun. Yönergeler için bkz: [dakika içinde bir SQL veritabanı oluşturma](../../sql-database/sql-database-get-started-portal.md). Bu bölümde tamamlamak için değerleri veritabanı adı, veritabanı sunucusu adını ve veritabanı yöneticisi kimlik bilgileri için parametre olarak gerekir. Veritabanı tablosu oluştur gerekmez. Uygulama yayınlamayı Spark, sizin için oluşturur.

Bir komut istemi açın, CURL yüklediğiniz dizine gidin ve aşağıdaki komutu çalıştırın:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputSQL.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Parametreler dosyasında **inputSQL.txt** şu şekilde tanımlanır:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureSQLTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--sql-server-fqdn", "<database-server-name>.database.windows.net", "--sql-database-name", "mysparkdatabase", "--database-username", "sparkdbadmin", "--database-password", "<put-password-here>", "--event-sql-table", "EventContent" ], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Uygulamanın başarıyla çalıştığını doğrulamak için SQL Server Management Studio'yu kullanarak Azure SQL veritabanına bağlanabilir. Bunun hakkında daha fazla yönerge için bkz: [SQL Server Management Studio ile SQL veritabanına bağlanma](../../sql-database/sql-database-connect-query-ssms.md). Veritabanına bağlandıktan sonra gidebilirsiniz **EventContent** akış uygulaması tarafından oluşturulmuş tablo. Tablodan veri almak için hızlı bir sorgu çalıştırabilirsiniz. Aşağıdaki sorguyu çalıştırın:

    SELECT * FROM EventCount

Aşağıdakine benzer bir çıktı görmeniz gerekir:

    00046b0f-2552-4980-9c3f-8bba5647c8ee
    000b7530-12f9-4081-8e19-90acd26f9c0c
    000bc521-9c1b-4a42-ab08-dc1893b83f3b
    00123a2a-e00d-496a-9104-108920955718
    0017c68f-7a4e-452d-97ad-5cb1fe5ba81b
    001KsmqL2gfu5ZcuQuTqTxQvVyGCqPp9
    001vIZgOStka4DXtud0e3tX7XbfMnZrN
    00220586-3e1a-4d2d-a89b-05c5892e541a
    0029e309-9e54-4e1b-84be-cd04e6fce5ec
    003333cf-874f-4045-9da3-9f98c2b4ea49
    0043c07e-8d73-420a-9af7-1fcb94575356
    004a11a9-0c2c-4bc0-a7d5-2e0ebd947ab9


## <a name="seealso"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)
* [Alıcı tabanlı bağlantı ve doğrudan DStream tasarımı](https://www.slideshare.net/NanZhu/seattle-sparkmeetup032317)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](../hdinsight-apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]: ../storage-create-storage-account/
