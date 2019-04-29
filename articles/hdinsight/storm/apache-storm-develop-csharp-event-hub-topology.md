---
title: Storm - Azure HDInsight ile Event hubs'tan olay işleme
description: Visual Studio için HDInsight Araçları'nı kullanarak Visual Studio'da oluşturulan bir C# Storm topolojisi ile Azure Event hubs'tan veri işleme hakkında bilgi edinin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/27/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: b02945197b20c7fe704d0f8cfa9201a5b9cbc292
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125213"
---
# <a name="process-events-from-azure-event-hubs-with-apache-storm-on-hdinsight-c"></a>HDInsight üzerinde Apache Storm ile Azure Event hubs'tan olay işleme (C#)

Azure Event Hubs'dan ile çalışmayı öğrenin [Apache Storm](https://storm.apache.org/) HDInsight üzerinde. Bu belgede, Event Hubs'dan veri yazma ve okuma için bir C# Storm topolojisi kullanır.

> [!NOTE]  
> Bu proje Java sürümü için bkz: [(Java) HDInsight üzerinde Apache Storm ile Azure Event hubs'tan olay işleme](https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/).

## <a name="scpnet"></a>SCP.NET

Bu belgedeki adımlarda SCP.NET, HDInsight üzerinde Storm ile C# topolojileri ve kullanmak için bileşenler oluşturmanızı kolaylaştıran bir NuGet paketini kullanın.

> [!IMPORTANT]  
> Bu belgedeki adımlarda Visual Studio ile bir Windows geliştirme ortamı kullanır, ancak Linux kullanan HDInsight kümesi üzerinde Storm için derlenmiş proje gönderilebilir. Yalnızca Linux tabanlı kümeler 28 Ekim 2016'dan sonra oluşturulan SCP.NET topolojileri destekler.

C# topolojileri çalıştırmak için HDInsight 3.4 ve Mono büyük kullanın. Bu belgede kullanılan örnekte, HDInsight 3.6 ile birlikte çalışır. HDInsight için kendi .NET çözümleri oluşturmayı planlıyorsanız, kontrol [Mono uyumluluğu](https://www.mono-project.com/docs/about-mono/compatibility/) olası uyumsuzluklar için belge.

### <a name="cluster-versioning"></a>Küme sürümü oluşturma

Projeniz için kullandığınız kullandığı Microsoft.SCP.Net.SDK NuGet paketini HDInsight üzerinde yüklü olan Storm ana sürümüyle eşleşmesi gerekir. HDInsight sürüm 3.5 ve 3.6 kullanın: Storm 1.x, dolayısıyla bu kümeleriyle SCP.NET sürüm 1.0.x.x izlemeniz gerekir.

> [!IMPORTANT]  
> Örnekte, bu belgede, HDInsight 3.5 veya 3.6 kümesi bekliyor.
>
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

C# topolojileri ayrıca .NET 4.5 hedeflemesi gerekir.

## <a name="how-to-work-with-event-hubs"></a>Event Hubs ile çalışma

Microsoft, Storm topolojisindeki Event Hubs ile iletişim kurmak için kullanılan bir Java bileşenlerini sunmaktadır. Bu bileşenler bir HDInsight 3.6 uyumlu sürümü içeren Java arşiv (JAR) dosyasını bulabilirsiniz [ https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar ](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).

> [!IMPORTANT]  
> Bileşenleri Java dilinde yazılır, ancak bunları bir C# topolojisi kolayca kullanabilirsiniz.

Bu örnekte aşağıdaki bileşenleri kullanılır:

* __EventHubSpout__: Event Hubs'dan veri okur.
* __EventHubBolt__: Event Hubs'a veri yazar.
* __EventHubSpoutConfig__: EventHubSpout yapılandırmak için kullanılır.
* __EventHubBoltConfig__: EventHubBolt yapılandırmak için kullanılır.

### <a name="example-spout-usage"></a>Örnek spout kullanım

SCP.NET topolojinize bir EventHubSpout eklemek için yöntemler sağlar. Bu yöntemler bir spout Java bileşen eklemek için genel yöntemler kullanarak daha eklemek kolaylaştırır. Aşağıdaki örnek, bir spout kullanarak oluşturmak gösterilmiştir __SetEventHubSpout__ ve **EventHubSpoutConfig** SCP.NET tarafından sağlanan yöntemleri:

```csharp
 topologyBuilder.SetEventHubSpout(
    "EventHubSpout",
    new EventHubSpoutConfig(
        ConfigurationManager.AppSettings["EventHubSharedAccessKeyName"],
        ConfigurationManager.AppSettings["EventHubSharedAccessKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        ConfigurationManager.AppSettings["EventHubEntityPath"],
        eventHubPartitions),
    eventHubPartitions);
```

Önceki örnek adlı yeni bir spout bileşeni oluşturur __EventHubSpout__ve bir event hub ile iletişim kurmak için yapılandırır. Bileşen için paralellik ipucu bölüm sayısı, olay hub'ı ayarlanır. Bu ayar, her bölüm için bileşenin bir örneğini oluşturmak Storm sağlar.

### <a name="example-bolt-usage"></a>Örnek bolt kullanım

Kullanım **JavaComponmentConstructor** bolt bir örneğini oluşturmak için yöntemi. Aşağıdaki örnek, oluşturmak ve yeni bir örneğini yapılandırmak gösterilmiştir **EventHubBolt**:

```csharp
// Java construcvtor for the Event Hub Bolt
JavaComponentConstructor constructor = JavaComponentConstructor.CreateFromClojureExpr(
    String.Format(@"(org.apache.storm.eventhubs.bolt.EventHubBolt. (org.apache.storm.eventhubs.bolt.EventHubBoltConfig. " +
        @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
        ConfigurationManager.AppSettings["EventHubPolicyName"],
        ConfigurationManager.AppSettings["EventHubPolicyKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        "servicebus.windows.net",
        ConfigurationManager.AppSettings["EventHubName"],
        "true"));

// Set the bolt to subscribe to data from the spout
topologyBuilder.SetJavaBolt(
    "eventhubbolt",
    constructor,
    partitionCount)
        .shuffleGrouping("Spout");
```

> [!NOTE]  
> Bu örnekte kullanmak yerine, bir dize olarak geçirilen bir Clojure ifade **JavaComponentConstructor** oluşturmak için bir **EventHubBoltConfig**, spout örnek yaptığınız gibi. Her iki yöntem çalışır. Sizin için en iyi hissettirir yöntemi kullanın.

## <a name="download-the-completed-project"></a>Projeyi yükle

Bu öğreticide oluşturulan proje tam sürümü indirebilirsiniz [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub). Bununla birlikte, yine de bu öğreticideki adımları izleyerek yapılandırma ayarları sağlamanız gerekir.

### <a name="prerequisites"></a>Önkoşullar

* Bir [HDInsight kümesi sürüm 3.5 veya 3.6 üzerinde Apache Storm](apache-storm-tutorial-get-started-linux.md).

    > [!WARNING]  
    > Bu belgede kullanılan örnekte, HDInsight sürüm 3.5 veya 3.6 üzerinde Storm gerektirir. Bu, HDInsight, eski sürümleriyle sınıfı adı değişiklikler nedeniyle çalışmaz. Bu örnek daha eski kümeleri ile birlikte çalışan sürümü için bkz: [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).

* Bir [Azure olay hub'ı](../../event-hubs/event-hubs-create.md).

* [Azure .NET SDK'sı](https://azure.microsoft.com/downloads/).

* [Visual Studio için HDInsight Araçları](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

* Java JDK 1.8 veya sonraki bir sürümü, geliştirme ortamı. JDK indirmeler sitesinden edinilebilir [Oracle](https://aka.ms/azure-jdks).

  * **JAVA_HOME** ortam değişkenini Java içeren dizine işaret etmelidir.
  * **%JAVA_HOME%/bin** dizin yolu olması gerekir.

## <a name="download-the-event-hubs-components"></a>Event Hubs bileşenlerini yükle

İndirme Event Hubs spout ve bolt bileşeni [ https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar ](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).

Adlı bir dizin oluşturmak `eventhubspout`ve dosyayı dizine kaydedin.

## <a name="configure-event-hubs"></a>Event hubs'ı Yapılandır

Event Hubs, bu örnek için veri kaynağı. Bilgileri kullanın "bir olay hub'ı oluşturma" bölümünde [Event Hubs ile çalışmaya başlama](../../event-hubs/event-hubs-create.md).

1. Olay hub'ı oluşturulduktan sonra görüntülemek **EventHub** Azure portal ve select ayarlarında **paylaşılan erişim ilkeleri**. Seçin **+ Ekle** aşağıdaki ilkeleri eklemek için:

   | Ad | İzinler |
   | --- | --- |
   | yazıcı |Gönder |
   | okuyucu |Dinle |

    ![Ekran görüntüsü, paylaşım erişim ilkeleri penceresi](./media/apache-storm-develop-csharp-event-hub-topology/sas.png)

2. Seçin **okuyucu** ve **yazıcı** ilkeleri. Kopyalayın ve bu değerler daha sonra kullanılmak üzere birincil anahtar değeri için her iki ilkeyi kaydedin.

## <a name="configure-the-eventhubwriter"></a>EventHubWriter yapılandırın

1. Visual Studio için HDInsight Araçları'nı en son sürümünü yüklemediyseniz, bkz. [Visual Studio için HDInsight araçlarını kullanmaya başlama](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

2. Çözümü indirme [eventhub storm karma](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).

3. İçinde **EventHubWriter** projesini açarsanız **App.config** dosya. Aşağıdaki anahtarlar için değer doldurmak için daha önce yapılandırılmış olay hub'ı yer alan bilgileri kullanın:

   | Anahtar | Değer |
   | --- | --- |
   | EventHubPolicyName |Yazıcı (ilkeyle için farklı bir ad kullandıysanız *Gönder* izni, bunun yerine kullanın.) |
   | EventHubPolicyKey |Yazıcı ilkesi için anahtar. |
   | EventHubNamespace |Olay hub'ınızı içeren ad uzayı. |
   | EventHubName |Olay hub'ı adı. |
   | EventHubPartitionCount |Olay hub'ındaki bölüm sayısı. |

4. Kaydet ve Kapat **App.config** dosya.

## <a name="configure-the-eventhubreader"></a>EventHubReader yapılandırın

1. Açık **EventHubReader** proje.

2. Açık **App.config** dosya **EventHubReader**. Aşağıdaki anahtarlar için değer doldurmak için daha önce yapılandırılmış olay hub'ı yer alan bilgileri kullanın:

   | Anahtar | Değer |
   | --- | --- |
   | EventHubPolicyName |Okuyucu (ilkeyle için farklı bir ad kullandıysanız *dinleme* izni, bunun yerine kullanın.) |
   | EventHubPolicyKey |Okuyucu ilkesi için anahtar. |
   | EventHubNamespace |Olay hub'ınızı içeren ad uzayı. |
   | EventHubName |Olay hub'ı adı. |
   | EventHubPartitionCount |Olay hub'ındaki bölüm sayısı. |

3. Kaydet ve Kapat **App.config** dosya.

## <a name="deploy-the-topologies"></a>Topolojileri dağıtma

1. Gelen **Çözüm Gezgini**, sağ **EventHubReader** proje ve seçin **HDInsight üzerindeki storm'a Gönder**.

    ![Çözüm Gezgini'nin ekran görüntüsü, ile vurgulanmış HDInsight üzerindeki storm'a Gönder](./media/apache-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. Üzerinde **topoloji Gönder** iletişim kutusunda, **Storm kümesi**. Genişletin **ek yapılandırmalar**seçin **Java dosya yolları**seçin **...** ve daha önce indirdiğiniz JAR dosyasını içeren dizini seçin. Son olarak, tıklayın **Gönder**.

    ![Gönderme topolojisi ekran iletişim kutusu](./media/apache-storm-develop-csharp-event-hub-topology/submit.png)

3. Topoloji gönderildiğinde **Storm topolojileri Görüntüleyicisi** görünür. Topoloji hakkında daha fazla bilgi görüntülemek için seçin **EventHubReader** sol bölmede topolojisi.

    ![Storm topolojileri Görüntüleyici'nin ekran görüntüsü](./media/apache-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. Gelen **Çözüm Gezgini**, sağ **EventHubWriter** proje ve seçin **HDInsight üzerindeki storm'a Gönder**.

5. Üzerinde **topoloji Gönder** iletişim kutusunda, **Storm kümesi**. Genişletin **ek yapılandırmalar**seçin **Java dosya yolları**seçin **...** ve daha önce yüklediğiniz JAR dosyasını içeren dizini seçin. Son olarak, tıklayın **Gönder**.

6. Topoloji gönderildiğinde topolojisi listesinde Yenile **Storm topolojileri Görüntüleyicisi** her iki topolojide kümesinde çalıştığını doğrulayın.

7. İçinde **Storm topolojileri Görüntüleyicisi**seçin **EventHubReader** topolojisi.

8. Bolt için Özet bileşeni'ni açmak için çift **LogBolt** diyagramda bileşen.

9. İçinde **yürütücüler** bölümünde, bağlantıları birini **bağlantı noktası** sütun. Bu bileşen tarafından günlüğe kaydedilen bilgi görüntüler. Günlüğe kaydedilen bilgileri aşağıdaki metne benzer:

        2017-03-02 14:51:29.255 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,255 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1830978598,"deviceId":"8566ccbc-034d-45db-883d-d8a31f34068e"}
        2017-03-02 14:51:29.283 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,283 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1756413275,"deviceId":"647a5eff-823d-482f-a8b4-b95b35ae570b"}
        2017-03-02 14:51:29.313 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,312 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1108478910,"deviceId":"206a68fa-8264-4d61-9100-bfdb68ee8f0a"}

## <a name="stop-the-topologies"></a>Topolojileri durdurma

Topolojileri durdurmak için her bir topolojide seçin **Storm topolojisi Görüntüleyicisi**, ardından **KILL**.

![Ekran görüntüsü, Storm topolojisi Görüntüleyicisi, sonlandırma düğmesi vurgulanan](./media/apache-storm-develop-csharp-event-hub-topology/killtopology.png)

## <a name="delete-your-cluster"></a>Kümenizi Sil

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Event Hubs Java spout ve bolt gelen Azure olay hub'larındaki verilerle çalışmak için C# topolojisi nasıl kullanılacağını öğrendiniz. C# topolojileri oluşturma hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Visual Studio kullanarak HDInsight üzerinde Apache Storm için C# topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md)
* [SCP Programlama Kılavuzu](apache-storm-scp-programming-guide.md)
* [HDInsight üzerinde Apache Storm için örnek topolojiler](apache-storm-example-topology.md)
