---
title: "İşlem olayları Event Hubs'dan Storm - Azure Hdınsight | Microsoft Docs"
description: "Visual Studio'da, Visual Studio için Hdınsight araçları kullanılarak oluşturulan bir C# Storm topolojisi ile Azure Event Hubs verilerini işlemek öğrenin."
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 67f9d08c-eea0-401b-952b-db765655dad0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/27/2017
ms.author: larryfr
ms.openlocfilehash: 9ad160377a8779ae917e6fd2d605ee01b12c3e2a
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-c"></a>Azure Event hubs'tan (C#) hdınsight'ta Storm işlem olayları

Azure Event Hubs'tan gelen Hdınsight üzerinde Apache Storm ile çalışmayı öğrenin. Bu belge, okumak ve Evbent hub'larından veri yazmak için C# Storm topolojisini kullanır.

> [!NOTE]
> Bu proje Java sürümü için bkz: [işlem Azure Event Hubs (Java) hdınsight'ta Storm olaylarından](https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/).

## <a name="scpnet"></a>SCP.NET

Bu belgede yer alan adımlar SCP.NET, C# topolojileri ve bileşenleri kullanmak için hdınsight'ta Storm oluşturmak kolaylaştıran bir NuGet paketi kullanın.

> [!IMPORTANT]
> Bu belgede yer alan adımlar Windows geliştirme ortamına Visual Studio ile kullanır, ancak derlenmiş proje Linux kullanan Hdınsight kümesi üzerinde Storm gönderilebilir. Yalnızca Linux tabanlı kümelerde 28 Ekim 2016'dan sonra oluşturulan SCP.NET topolojileri destekler.

Hdınsight 3.4 ve büyük Mono C# topolojileri çalıştırmak için kullanabilirsiniz. Bu belgede kullanılan örnekte, Hdınsight 3.6 ile çalışır. Hdınsight için kendi .NET çözümleri oluşturma üzerinde planlıyorsanız, denetleme [Mono Uyumluluk](http://www.mono-project.com/docs/about-mono/compatibility/) olası uyumsuzlukları belge.

> [!WARNING]
> SCP.NET sürüm kullanan projeleri oluşturma sorunları karşılaşmanız halinde 1.0.0.x, lütfen Yardım için Microsoft Destek'e başvurun.

### <a name="cluster-versioning"></a>Küme sürüm oluşturma

Projeniz için kullandığınız Microsoft.SCP.Net.SDK NuGet paketi yüklü Hdınsight üzerinde Storm ana sürümü aynı olmalıdır. Hdınsight sürüm 3.5 ve 3.6 kullanmak Storm 1.x bu kümeleriyle SCP.NET sürüm 1.0.x.x kullanmalısınız.

> [!IMPORTANT]
> Bu belgedeki örnek bir Hdınsight 3.5 veya 3,6 küme bekliyor.
>
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

C# topolojileri ayrıca .NET 4.5 hedeflemesi gerekir.

## <a name="how-to-work-with-event-hubs"></a>Event Hubs ile çalışmaya nasıl

Microsoft bir Storm topolojisinin Event Hubs'dan ile iletişim kurmak için kullanılan Java bileşenleri kümesi sağlar. Bu bileşenler bir Hdınsight 3.6 uyumlu sürümünü içeren Java arşiv (JAR) dosyasını bulabilirsiniz [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).

> [!IMPORTANT]
> Bileşenleri Java'da yazılmış olsa da, bunları bir C# topolojisi kolayca kullanabilirsiniz.

Bu örnekte aşağıdaki bileşenleri kullanılır:

* __EventHubSpout__: olay hub'larından verileri okur.
* __EventHubBolt__: Event Hubs'a veri yazar.
* __EventHubSpoutConfig__: EventHubSpout yapılandırmak için kullanılır.
* __EventHubBoltConfig__: EventHubBolt yapılandırmak için kullanılır.

### <a name="example-spout-usage"></a>Örnek spout kullanım

SCP.NET bir EventHubSpout topolojinizi eklemek için yöntemler sağlar. Bu yöntemler, bir Java bileşeni eklemek için genel yöntemler kullanmaktan spout eklemek kolaylaştırır. Aşağıdaki örnek, bir spout kullanarak oluşturmak gösterilmiştir __SetEventHubSpout__ ve **EventHubSpoutConfig** SCP.NET tarafından sağlanan yöntemleri:

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

Önceki örnek adlı yeni bir spout bileşeni oluşturur __EventHubSpout__ve bir event hub ile iletişim kurmak için yapılandırır. Bileşeni için paralellik ipucu bölüm sayısı olay hub'ı ayarlanır. Bu ayar her bölüm için bileşeninin bir örneğini oluşturmak Storm sağlar.

### <a name="example-bolt-usage"></a>Örnek Cıvata kullanım

Kullanım **JavaComponmentConstructor** yöntemi Cıvata örneği oluşturun. Aşağıdaki örnek oluşturmak ve yeni bir örneğini yapılandırmak gösterilmiştir **EventHubBolt**:

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
> Bu örneği kullanmak yerine bir dize olarak geçirilen Clojure ifade kullanır **JavaComponentConstructor** oluşturmak için bir **EventHubBoltConfig**, spout örnek yaptığınız gibi. Her iki yöntem çalışır. Sizin için en iyi hissi yöntemini kullanın.

## <a name="download-the-completed-project"></a>Tamamlanan projenizi indirin

Bu öğreticide oluşturulan proje tam bir sürümünü yükleyebilirsiniz [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub). Ancak, yine Bu öğreticide adımları izleyerek yapılandırma ayarlarını sağlamak gerekir.

### <a name="prerequisites"></a>Ön koşullar

* Bir [Hdınsight kümesi sürüm 3.5 veya 3.6 üzerinde Apache Storm](apache-storm-tutorial-get-started-linux.md).

    > [!WARNING]
    > Bu belgede kullanılan örnekte sürüm 3.5 ya da 3.6 hdınsight'ta Storm gerektirir. Bu, Hdınsight, eski sürümleri ile yeni sınıf adı değişiklikler nedeniyle çalışmaz. Eski kümeleriyle çalışır Bu örnek sürümü için bkz: [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).

* Bir [Azure olay hub'ı](../../event-hubs/event-hubs-create.md).

* [Azure .NET SDK'sı](http://azure.microsoft.com/downloads/).

* [Visual Studio için Hdınsight Araçları](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

* Java JDK 1.8 veya sonraki geliştirme ortamınızı. JDK yüklemeleri web'da [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

  * **JAVA_HOME** ortam değişkeni Java içeren dizine işaret etmelidir.
  * **%JAVA_HOME%/bin** dizin yolu olması gerekir.

## <a name="download-the-event-hubs-components"></a>Olay hub'ları bileşenlerini yükle

Event Hubs spout indirin ve Cıvata bileşeninden [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).

Adlı bir dizin oluşturun `eventhubspout`ve dizine dosyayı kaydedin.

## <a name="configure-event-hubs"></a>Olay hub'ları yapılandırma

Olay hub'ları bu örnek için veri kaynağı değil. "Bir olay hub'ı oluşturma" bölümündeki bilgileri kullanın [Event Hubs ile çalışmaya başlama](../../event-hubs/event-hubs-create.md).

1. Olay hub'ı oluşturulduktan sonra görüntülemek **EventHub** dikey penceresinde Azure portal ve select **paylaşılan erişim ilkeleri**. Seçin **+ Ekle** aşağıdaki ilkeleri eklemek için:

   | Ad | İzinler |
   | --- | --- |
   | Yazıcı |Gönder |
   | Okuyucu |Dinleme |

    ![Ekran görüntüsü, paylaşım erişim ilkeleri penceresi](./media/apache-storm-develop-csharp-event-hub-topology/sas.png)

2. Seçin **okuyucu** ve **yazan** ilkeleri. Kopyalayın ve bu değerleri daha sonra kullanılmak üzere birincil anahtar değeri için her iki ilkeyi kaydedin.

## <a name="configure-the-eventhubwriter"></a>EventHubWriter yapılandırın

1. Visual Studio için Hdınsight araçları en son sürümünü yüklemediyseniz, bkz: [Visual Studio için Hdınsight araçları kullanarak çalışmaya başlama](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

2. Çözüm karşıdan [eventhub storm karma](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).

3. İçinde **EventHubWriter** proje, açık **App.config** dosya. Aşağıdaki anahtarlar için değer doldurmak için daha önce yapılandırılmış olay hub'ı bulunan bilgileri kullanın:

   | Anahtar | Değer |
   | --- | --- |
   | EventHubPolicyName |Yazıcı (ilkeyle için farklı bir ad kullandıysanız *Gönder* izni, bunun yerine kullanın.) |
   | EventHubPolicyKey |Yazıcı ilkesi için anahtar. |
   | EventHubNamespace |Olay hub'ınızı içeren ad alanı. |
   | EventHubName |Olay hub'ı adınız. |
   | EventHubPartitionCount |Olay hub'ınızı bölüm sayısı. |

4. Kaydet ve Kapat **App.config** dosya.

## <a name="configure-the-eventhubreader"></a>EventHubReader yapılandırın

1. Açık **EventHubReader** projesi.

2. Açık **App.config** dosya **EventHubReader**. Aşağıdaki anahtarlar için değer doldurmak için daha önce yapılandırılmış olay hub'ı bulunan bilgileri kullanın:

   | Anahtar | Değer |
   | --- | --- |
   | EventHubPolicyName |Okuyucu (ilkeyle için farklı bir ad kullandıysanız *dinleme* izni, bunun yerine kullanın.) |
   | EventHubPolicyKey |Okuyucu ilkesi için anahtar. |
   | EventHubNamespace |Olay hub'ınızı içeren ad alanı. |
   | EventHubName |Olay hub'ı adınız. |
   | EventHubPartitionCount |Olay hub'ınızı bölüm sayısı. |

3. Kaydet ve Kapat **App.config** dosya.

## <a name="deploy-the-topologies"></a>Topolojileri dağıtma

1. Gelen **Çözüm Gezgini**, sağ **EventHubReader** proje ve seçin **Hdınsight üzerinde Storm Gönder**.

    ![Ekran görüntüsü, Çözüm Gezgini ile vurgulanan Hdınsight üzerinde Storm Gönder](./media/apache-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. Üzerinde **gönderme topoloji** iletişim kutusunda, **Storm kümesi**. Genişletme **ek yapılandırmalar**seçin **Java dosya yolları**seçin **...** ve daha önce indirdiğiniz JAR dosyasını içeren dizini seçin. Son olarak, tıklatın **gönderme**.

    ![Gönderme topoloji ekran iletişim kutusu](./media/apache-storm-develop-csharp-event-hub-topology/submit.png)

3. Topoloji gönderildiğinde **Storm topolojileri Görüntüleyicisi** görüntülenir. Topolojisi ile ilgili bilgileri görüntülemek için seçin **EventHubReader** sol bölmede topolojisi.

    ![Storm topolojileri Görüntüleyicisi'nin ekran görüntüsü](./media/apache-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. Gelen **Çözüm Gezgini**, sağ **EventHubWriter** proje ve seçin **Hdınsight üzerinde Storm Gönder**.

5. Üzerinde **gönderme topoloji** iletişim kutusunda, **Storm kümesi**. Genişletme **ek yapılandırmalar**seçin **Java dosya yolları**seçin **...** ve daha önce indirdiğiniz JAR dosyasını içeren dizini seçin. Son olarak, tıklatın **gönderme**.

6. Topoloji gönderildiğinde topoloji listesinde yenileme **Storm topolojileri Görüntüleyicisi** her iki topolojide küme üzerinde çalıştığını doğrulayın.

7. İçinde **Storm topolojileri Görüntüleyicisi**seçin **EventHubReader** topolojisi.

8. Cıvata için Özet bileşeni açmak için çift **LogBolt** diyagramda bileşen.

9. İçinde **yürütücüler** bölümünde, bağlantılardan birini seçin **bağlantı noktası** sütun. Bu bileşen tarafından günlüğe kaydedilen bilgi görüntüler. Günlüğe kaydedilen bilgileri aşağıdakine benzer:

        2017-03-02 14:51:29.255 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,255 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1830978598,"deviceId":"8566ccbc-034d-45db-883d-d8a31f34068e"}
        2017-03-02 14:51:29.283 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,283 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1756413275,"deviceId":"647a5eff-823d-482f-a8b4-b95b35ae570b"}
        2017-03-02 14:51:29.313 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,312 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1108478910,"deviceId":"206a68fa-8264-4d61-9100-bfdb68ee8f0a"}

## <a name="stop-the-topologies"></a>Topolojileri Durdur

Topolojileri durdurmak için her topolojisinde seçin **Storm topolojisini Görüntüleyicisi**, ardından **KILL**.

![Ekran görüntüsü, Storm topolojisini Görüntüleyici, KILL düğmesi vurgulanan](./media/apache-storm-develop-csharp-event-hub-topology/killtopology.png)

## <a name="delete-your-cluster"></a>Kümenizi Sil

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Java Event Hubs spout ve Azure Event Hubs verilerle çalışmak için bir C# topolojisi gelen Cıvata nasıl kullanılacağı öğrendiniz. C# topolojileri oluşturma hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Visual Studio kullanarak Hdınsight üzerinde Apache Storm için C# topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md)
* [SCP Programlama Kılavuzu](apache-storm-scp-programming-guide.md)
* [HDInsight üzerinde Storm için örnek topolojiler](apache-storm-example-topology.md)
