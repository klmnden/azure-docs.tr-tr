---
title: Apache Storm topolojilerini Visual Studio ve C# - Azure HDInsight ile
description: C# Storm topolojileri oluşturmayı öğrenin. Basit bir sözcük sayımı topolojisini, Visual Studio için Hadoop araçlarını kullanarak Visual Studio'da oluşturun.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 11/27/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 9d459f88cd252303384acb4a72d0af0cce6ee226
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67428461"
---
# <a name="develop-c-topologies-for-apache-storm-by-using-the-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake araçları kullanarak Apache Storm için C# topolojileri geliştirme

Oluşturmayı bir C# Visual Studio için Azure Data Lake (Apache Hadoop) araçlarını kullanarak Apache Storm topolojisi. Bu belgede bir Azure HDInsight kümesi üzerinde Apache storm'a dağıtma Visual Studio'da bir Storm projesi oluşturma ve yerel olarak test etme işleminde size yol gösterir.

Ayrıca C# ve Java bileşenlerini kullanan karma topolojiler oluşturmayı öğrenin.

> [!NOTE]  
> Bu belgedeki adımlarda Visual Studio ile bir Windows geliştirme ortamı kullanır, ancak bir Linux veya Windows tabanlı HDInsight kümesine derlenmiş proje gönderilebilir. Yalnızca Linux tabanlı kümeler 28 Ekim 2016'dan sonra oluşturulan SCP.NET topolojileri destekler.

Bir Linux tabanlı kümeyle C# topolojisi kullanmak için proje tarafından kullanılan sürümüne 0.10.0.6 veya üzeri kullandığı Microsoft.SCP.Net.SDK NuGet paketini güncelleştirmeniz gerekir. Paketin sürümünün ayrıca HDInsight üzerinde yüklü olan Storm ana sürümüyle eşleşmesi gerekir.

| HDInsight sürümü | Apache Storm sürümü | SCP.NET sürümü | Mono varsayılan sürüm |
|:-----------------:|:-------------:|:---------------:|:--------------------:|
| 3.3 |0.10.x sürümü |0.10.x.x</br>(yalnızca Windows tabanlı HDInsight üzerinde) | NA |
| 3.4 | 0.10.0.x | 0.10.0.x | 3.2.8 |
| 3,5 | 1.0.2.x | 1.0.0.x | 4.2.1 |
| 3.6 | 1.1.0.x | 1.0.0.x | 4.2.8 |

> [!IMPORTANT]  
> Linux tabanlı kümelerdeki C# topolojilerinin .NET 4.5 kullanması ve HDInsight kümesi üzerinde çalışması için Mono kullanması gerekir. Denetleme [Mono uyumluluğu](https://www.mono-project.com/docs/about-mono/compatibility/) olası uyumsuzluklar için.

## <a name="install-visual-studio"></a>Visual Studio yükleme

Visual Studio sürümlerinden birini kullanarak SCP.NET ile C# topolojileri geliştirme yapabilirsiniz:

* Visual Studio 2012 güncelleştirme 4

* Güncelleştirme 4 ile Visual Studio 2013 veya [Visual Studio 2013 Community](https://go.microsoft.com/fwlink/?LinkId=517284)

* Visual Studio 2015 veya [Visual Studio 2015 Community](https://go.microsoft.com/fwlink/?LinkId=532606)

* Visual Studio 2017 (herhangi bir sürümü)

## <a name="install-data-lake-tools-for-visual-studio"></a>Visual Studio için yükleme Data Lake araçları

Visual Studio için Data Lake Araçları'nı yüklemek için adımları izleyin. [Visual Studio için Data Lake araçlarını kullanmaya başlama](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

## <a name="install-java"></a>Java'yı yükleme

Visual Studio bir Storm topolojisinden gönderdiğinizde, SCP.NET için topoloji ve bağımlılıklarını içeren bir zip dosyası oluşturur. Java, Linux tabanlı kümeler ile daha uyumlu bir biçimde kullandığından bu zip dosyaları oluşturmak için kullanılır.

1. Geliştirme ortamınızı Java Developer Kit (JDK) 7 veya sonraki bir sürümü yükleyin. Oracle JDK'den alabilirsiniz [Oracle](https://aka.ms/azure-jdks). Ayrıca [diğer Java dağıtımları](https://openjdk.java.net/).

2. `JAVA_HOME` Ortam değişkenini Java içeren dizine işaret etmelidir.

3. `PATH` Gerekir INCLUDE ortam değişkeni `%JAVA_HOME%\bin` dizin.

Aşağıdaki C# konsol uygulaması, Java ve JDK düzgün yüklendiğini doğrulamak için kullanabilirsiniz:

```csharp
using System;
using System.IO;
namespace ConsoleApplication2
{
   class Program
   {
       static void Main(string[] args)
       {
           string javaHome = Environment.GetEnvironmentVariable("JAVA_HOME");
           if (!string.IsNullOrEmpty(javaHome))
           {
               string jarExe = Path.Combine(javaHome + @"\bin", "jar.exe");
               if (File.Exists(jarExe))
               {
                   Console.WriteLine("JAVA Is Installed properly");
                    return;
               }
               else
               {
                   Console.WriteLine("A valid JAVA JDK is not found. Looks like JRE is installed instead of JDK.");
               }
           }
           else
           {
             Console.WriteLine("A valid JAVA JDK is not found. JAVA_HOME environment variable is not set.");
           }
       }  
   }
}
```

## <a name="apache-storm-templates"></a>Apache Storm şablonları

Aşağıdaki şablonlar Visual Studio için Data Lake araçları sağlar:

| Proje türü | Gösteriler |
| --- | --- |
| Bir Storm uygulaması |Boş bir Storm topolojisi proje. |
| Azure SQL yazıcı örnek Storm |Azure SQL veritabanına nasıl. |
| Azure Cosmos DB okuyucu örnek Storm |Azure Cosmos DB'den okumak nasıl. |
| Azure Cosmos DB yazan örnek Storm |Azure Cosmos DB'ye yazılmak üzere nasıl. |
| Storm EventHub okuyucu örnek |Azure Event Hubs'dan okumayı öğrenin. |
| Storm EventHub yazan örnek |Azure Event Hubs'a yazmak nasıl. |
| Storm, HBase okuyucu örnek |HDInsight üzerinde HBase okuma kümeleri. |
| Storm, HBase yazıcı örnek |HDInsight üzerinde HBase için yazma kümeleri. |
| Storm karma örnek |Bir Java bileşeni kullanma |
| Storm örneği |Temel bir sözcük sayımı topolojisini. |

> [!WARNING]  
> Tüm şablonları, Linux tabanlı HDInsight ile çalışır. NuGet paketlerini şablonu tarafından kullanılan Mono ile uyumlu olmayabilir. Denetleme [Mono uyumluluğu](https://www.mono-project.com/docs/about-mono/compatibility/) kullanın ve belge [.NET Portability Analyzer](../hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) olası sorunlar belirlemek için.

Bu belgedeki adımlarda bir topoloji oluşturmak için temel bir Storm uygulaması proje türü kullanın.

### <a name="apache-hbase-templates-notes"></a>Apache HBase şablonları notları

HBase okuyucu ve yazıcı şablonları HBase REST API, HBase Java API değil, HDInsight kümesindeki bir HBase ile iletişim kurmak için kullanın.

### <a name="eventhub-templates-notes"></a>EventHub şablonları notları

> [!IMPORTANT]  
> EventHub okuyucu şablonla birlikte verilen Java tabanlı EventHub spout bileşeni HDInsight 3.5 veya sonraki bir sürümü üzerinde Storm ile çalışmayabilir. Bu bileşen güncelleştirilmiş bir sürümü kullanılabilir [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).

Bu kullanan örnek bir topoloji için bkz: bileşen ve HDInsight 3.5 üzerinde Storm ile çalışır [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).

## <a name="create-a-c-topology"></a>C# topolojisi oluşturma

1. Visual Studio'yu açın, **dosya** > **yeni**ve ardından **proje**.

2. Gelen **yeni proje** penceresini genişletin **yüklü** > **şablonları**seçip **Azure Data Lake**. Şablonlar listesinden **bir Storm uygulaması**. Ekranın alt kısmında girin **WordCount** uygulamanın adı.

    ![Yeni Proje ekran penceresi](./media/apache-storm-develop-csharp-visual-studio-topology/new-project.png)

3. Projeyi oluşturduktan sonra şu dosyalar bulunmalıdır:

   * **Program.cs**: Bu dosya, projeniz için topoloji tanımlar. Bir spout ve bolt bir oluşan bir varsayılan topolojisi, varsayılan olarak oluşturulur.

   * **Spout.cs**: Rastgele sayılar yayan bir örnek spout.

   * **Bolt.cs**: Bir spout'un yayılan sayıların sayısını tutar örnek bolt.

     Proje oluşturduğunuzda, en son NuGet indirir [SCP.NET paket](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).

     [!INCLUDE [scp.net version important](../../../includes/hdinsight-storm-scpdotnet-version.md)]

### <a name="implement-the-spout"></a>Spout uygulayın

1. Açık **Spout.cs**. Spout bir dış kaynaktan bir topoloji verileri okumak için kullanılır. Bir spout için ana bileşenleri şunlardır:

   * **NextTuple**: Spout yeni bir tanımlama grubu yayma izin Storm tarafından çağrılır.

   * **ACK** (yalnızca işlem topolojisi): Spout gönderilen diziler için topolojideki başka bileşenler tarafından başlatılan bir onayları işler. Bir demet sıkan aşağı akış bileşenleri tarafından başarıyla işlendi bilmeniz spout olanak tanır.

   * **Başarısız** (yalnızca işlem topolojisi): Başarısız topolojisi içindeki diğer bileşenlere işleme diziler işler. Fail yöntemi uygulama, böylece yeniden işlenebilir yeniden tanımlama grubu yayma olanak tanır.

2. Öğesinin içeriğini değiştirin **Spout** aşağıdaki metinle sınıfı: Bu spout, rastgele bir cümle topolojiye yayar.

    ```csharp
    private Context ctx;
    private Random r = new Random();
    string[] sentences = new string[] {
        "the cow jumped over the moon",
        "an apple a day keeps the doctor away",
        "four score and seven years ago",
        "snow white and the seven dwarfs",
        "i am at two with nature"
    };

    public Spout(Context ctx)
    {
        // Set the instance context
        this.ctx = ctx;

        Context.Logger.Info("Generator constructor called");

        // Declare Output schema
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // The schema for the default output stream is
        // a tuple that contains a string field
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
    }

    // Get an instance of the spout
    public static Spout Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Spout(ctx);
    }

    public void NextTuple(Dictionary<string, Object> parms)
    {
        Context.Logger.Info("NextTuple enter");
        // The sentence to be emitted
        string sentence;

        // Get a random sentence
        sentence = sentences[r.Next(0, sentences.Length - 1)];
        Context.Logger.Info("Emit: {0}", sentence);
        // Emit it
        this.ctx.Emit(new Values(sentence));

        Context.Logger.Info("NextTuple exit");
    }

    public void Ack(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }
    ```

### <a name="implement-the-bolts"></a>Boltlar uygulayın

1. Var olanı Sil **Bolt.cs** proje dosyası.

2. İçinde **Çözüm Gezgini**projeye sağ tıklayıp seçin **Ekle** > **yeni öğe**. Listesinden **Storm Bolt**girin **Splitter.cs** adı. Adlı ikinci bir bolt oluşturmak için bu işlemi yineleyin **Counter.cs**.

   * **Splitter.cs**: Cümleleri tek tek sözcüklere böler ve sözcük yeni akış yayar bir bolta uygular.

   * **Counter.cs**: Her sözcüğün sayar ve yeni bir akışı her sözcük sayısını ve sözcük yayan bir bolta uygular.

     > [!NOTE]  
     > Bu bolt okuma ve akışlara yazma, ancak bir veritabanı ya da hizmeti gibi kaynakları ile iletişim kurmak için de kullanabilirsiniz.

3. Açık **Splitter.cs**. Bu, varsayılan olarak yalnızca bir yöntemi vardır: **Yürütme**. Yürütme yönteminin bolt işleme için bir demet aldığında çağrılır. Burada okuyabilirsiniz gelen tanımlama grubu işlemek ve giden tanımlama grubu yayma.

4. Öğesinin içeriğini değiştirin **Bölümlendirici** aşağıdaki kodla sınıfı:

    ```csharp
    private Context ctx;

    // Constructor
    public Splitter(Context ctx)
    {
        Context.Logger.Info("Splitter constructor called");
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // Input contains a tuple with a string field (the sentence)
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // Outbound contains a tuple with a string field (the word)
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance of the bolt
    public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Splitter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get the sentence from the tuple
        string sentence = tuple.GetString(0);
        // Split at space characters
        foreach (string word in sentence.Split(' '))
        {
            Context.Logger.Info("Emit: {0}", word);
            //Emit each word
            this.ctx.Emit(new Values(word));
        }

        Context.Logger.Info("Execute exit");
    }
    ```

5. Açık **Counter.cs**ve sınıfı içeriğini aşağıdaki kodla değiştirin:

    ```csharp
    private Context ctx;

    // Dictionary for holding words and counts
    private Dictionary<string, int> counts = new Dictionary<string, int>();

    // Constructor
    public Counter(Context ctx)
    {
        Context.Logger.Info("Counter constructor called");
        // Set instance context
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string field - the word
        inputSchema.Add("default", new List<Type>() { typeof(string) });

        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string and integer field - the word and the word count
        outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance
    public static Counter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Counter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get the word from the tuple
        string word = tuple.GetString(0);
        // Do we already have an entry for the word in the dictionary?
        // If no, create one with a count of 0
        int count = counts.ContainsKey(word) ? counts[word] : 0;
        // Increment the count
        count++;
        // Update the count in the dictionary
        counts[word] = count;

        Context.Logger.Info("Emit: {0}, count: {1}", word, count);
        // Emit the word and count information
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
        Context.Logger.Info("Execute exit");
    }
    ```

### <a name="define-the-topology"></a>Topolojisi tanımlayın

Spout ve bolt bileşenleri arasında veri akışını tanımlayan bir grafik halinde düzenlenir. Bu topoloji için grafiği aşağıdaki gibidir:

![Bileşenlerini nasıl düzenlenir diyagramı](./media/apache-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

Cümleleri spout gönderilir ve ayırıcı bolt örneklerine dağıtılır. Bölümlendirici bolt cümleleri sayacı bolt'a dağıtılmış bir deyişle, içine keser.

Sözcük sayısını sayaç örneğinde yerel olarak tutulmadığından belirli sözcükleri aynı sayaç bolt örneğine akış emin olmanız gerekir. Her örnek, belirli sözcükleri izler. Bölümlendirici bolt hiçbir durumunu koruyan olduğundan, gerçekten Bölümlendirici hangi örneğinin hangi cümle alır önemi yoktur.

Açık **Program.cs**. En önemli yöntemdir **GetTopologyBuilder**, gönderilen topolojisi için Storm tanımlamak için kullanılır. Öğesinin içeriğini değiştirin **GetTopologyBuilder** daha önce açıklanan topolojiyi uygulamak için aşağıdaki kod ile:

```csharp
// Create a new topology named 'WordCount'
TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

// Add the spout to the topology.
// Name the component 'sentences'
// Name the field that is emitted as 'sentence'
topologyBuilder.SetSpout(
    "sentences",
    Spout.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
    },
    1);
// Add the splitter bolt to the topology.
// Name the component 'splitter'
// Name the field that is emitted 'word'
// Use suffleGrouping to distribute incoming tuples
//   from the 'sentences' spout across instances
//   of the splitter
topologyBuilder.SetBolt(
    "splitter",
    Splitter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
    },
    1).shuffleGrouping("sentences");

// Add the counter bolt to the topology.
// Name the component 'counter'
// Name the fields that are emitted 'word' and 'count'
// Use fieldsGrouping to ensure that tuples are routed
//   to counter instances based on the contents of field
//   position 0 (the word). This could also have been
//   List<string>(){"word"}.
//   This ensures that the word 'jumped', for example, will always
//   go to the same instance
topologyBuilder.SetBolt(
    "counter",
    Counter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
    },
    1).fieldsGrouping("splitter", new List<int>() { 0 });

// Add topology config
topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
{
    {"topology.kryo.register","[\"[B\"]"}
});

return topologyBuilder;
```

## <a name="submit-the-topology"></a>Topoloji Gönder

1. İçinde **Çözüm Gezgini**projeye sağ tıklayıp seçin **HDInsight üzerindeki storm'a Gönder**.

   > [!NOTE]  
   > İstenirse, Azure aboneliğiniz için kimlik bilgilerini girin. Birden fazla aboneliğiniz varsa, HDInsight kümesi üzerinde Storm'a içeren oturum açın.

2. HDInsight kümesi üzerinde Storm'a seçin **Storm kümesi** aşağı açılan liste ve ardından **Gönder**. Gönderim kullanarak başarılı olursa izleyebilirsiniz **çıkış** penceresi.

3. Topoloji başarıyla gönderildi, **Storm topolojilerini** küme görünür. Seçin **WordCount** çalışan topolojiyi hakkındaki bilgileri görüntülemek için listeden topolojisi.

   > [!NOTE]  
   > Ayrıca görüntüleyebilirsiniz **Storm topolojilerini** gelen **Sunucu Gezgini**. Genişletin **Azure** > **HDInsight**, HDInsight kümesinde Storm sağ tıklayın ve ardından **Storm topolojilerini görüntüle**.

    Topolojide bileşenleri hakkında bilgi görüntülemek için diyagramdaki bileşene çift tıklayın.

4. Gelen **topoloji özeti** yi **KILL** topoloji durdurmak için.

   > [!NOTE]  
   > Storm topolojileri devre dışı bırakılır veya küme silinene kadar çalışmaya devam eder.

## <a name="transactional-topology"></a>İşlem topolojisi

Önceki işlem olmayan topolojidir. Topolojisindeki bileşenler iletileri yeniden yürüterek işlevselliği kullanılmaz. Bir işlem topolojisi örneği, bir proje oluşturun ve seçin **Storm örnek** proje türü.

İşlem topolojileri yeniden yürütme veri desteklemek için aşağıdakileri uygulayın:

* **Meta verileri önbelleğe alma**: Spout ve böylece veriler alınır ve bir arıza oluşması durumunda yeniden yayılan yayılan veriler hakkında meta veri depolamanız gerekir. Örnek tarafından yayılan veriler küçük olduğundan, her demet için ham verileri yeniden yürütme için bir sözlükte depolanır.

* **ACK**: Topolojide her bolt çağırabilirsiniz `this.ctx.Ack(tuple)` tanımlama grubu başarıyla işlediği anladığınızı belirtmek için. Tüm Cıvatalar demet onayladıktan olduğunda `Ack` spout yöntemi çağrılır. `Ack` Spout yeniden yürütme için önbelleğe ilişkili verileri kaldırmak yöntem sağlar.

* **Başarısız**: Her bolt çağırabilirsiniz `this.ctx.Fail(tuple)` işleme bir demet için başarısız olduğunu göstermek için. İçin hata yayar `Fail` burada demet durumdayken kullanarak spout yöntemi önbelleğe alınan meta verileri.

* **Sıra kimliği**: Bir tanımlama grubu yayma, benzersiz sırası kimliği belirtilebilir. Bu değer, yeniden yürütme (Ack ve başarısızlık) işleme için bir tanımlama grubu tanımlar. Spout gibi **Storm örnek** proje veri gösterilirken aşağıdakileri kullanır:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    İçindeki sırası kimlik değerine sahip bir cümle için varsayılan akışı içeren bir tanımlama grubu bu kodu yayan **lastSeqId**. Bu örnekte, **lastSeqId** yayılan her demet için artırılır.

Örnekte gösterildiği şekilde **Storm örnek** projesi, bir bileşenin işlem olup yapılandırmasına bağlı olarak, çalışma zamanında ayarlanmış olabilir.

## <a name="hybrid-topology-with-c-and-java"></a>C# ve Java ile karma topolojisi

Burada bazı bileşenler C# ve diğerleri Java karma topolojiler oluşturmak için Visual Studio için Data Lake araçları da kullanabilirsiniz.

Karma bir topolojide ilişkin bir örnek, bir proje oluşturun ve seçin **Storm karma örnek**. Bu örnek türü aşağıdaki kavramları göstermektedir:

* **Java spout** ve  **C# bolt**: İçinde tanımlı **HybridTopology_javaSpout_csharpBolt**.

    * İçinde tanımlanan bir işlem sürüm **HybridTopologyTx_javaSpout_csharpBolt**.

* **C#spout** ve **Java bolt**: İçinde tanımlı **HybridTopology_csharpSpout_javaBolt**.

    * İçinde tanımlanan bir işlem sürüm **HybridTopologyTx_csharpSpout_javaBolt**.

  > [!NOTE]  
  > Bu sürüm ayrıca Clojure kodu bir metin dosyasından bir Java bileşeni olarak kullanma işlemini gösterir.


Proje gönderildiğinde kullanılan topolojisine geçin Taşı `[Active(true)]` kümeye göndermeden önce kullanmak istediğiniz topolojiye deyimi.

> [!NOTE]  
> Bu projede bir parçası olarak gerekli olan tüm Java dosyaları sağlanan **JavaDependency** klasör.

Oluşturma ve gönderme karma bir topolojide aşağıdakileri dikkate alın:

* Kullanım **JavaComponentConstructor** bolt veya spout bir için Java sınıfı örneğini oluşturmak için.

* Kullanım **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** veri içine veya dışına Java bileşenlerini Java nesnelerini json'a seri hale.

* Sunucuya topoloji gönderirken kullanmalısınız **ek yapılandırmalar** belirtmek için seçeneği **Java dosya yolları**. Belirtilen yol, Java sınıfları içeren JAR dosyaları içeren dizine olmalıdır.

### <a name="azure-event-hubs"></a>Azure Event Hubs

SCP.NET sürüm 0.9.4.203, yeni bir sınıf ve olay hub'ı spout (Event Hubs'dan okuyan bir Java spout) ile çalışmak için özellikle yöntemi içerir. Bir olay hub'ı spout kullanan bir topolojisi oluşturduğunuzda, aşağıdaki yöntemleri kullanın:

* **EventHubSpoutConfig** class: Spout bileşeni için yapılandırma içeren bir nesne oluşturur.

* **TopologyBuilder.SetEventHubSpout** yöntemi: Olay hub'ı spout bileşeni topolojiye ekler.

> [!NOTE]  
> Yine de kullanmalısınız **CustomizedInteropJSONSerializer** spout tarafından üretilen veriler seri hale.

## <a id="configurationmanager"></a>ConfigurationManager kullanın

Kullanmayın **ConfigurationManager** bolt yapılandırma değerlerini almak ve spout bileşenleri. Bunun yapılması, bir null işaretçi özel durumu oluşabilir. Bunun yerine, yapılandırma, projeniz için Storm topolojiye topolojisi bağlamda bir anahtar ve değer çifti olarak geçirilir. Yapılandırma değerlerine dayanan her bileşenin bunları başlatma sırasında bağlamdan almanız gerekir.

Aşağıdaki kod, bu değerlerin nasıl alınacağını göstermektedir:

```csharp
public class MyComponent : ISCPBolt
{
    // To hold configuration information loaded from context
    Configuration configuration;
    ...
    public MyComponent(Context ctx, Dictionary<string, Object> parms)
    {
        // Save a copy of the context for this component instance
        this.ctx = ctx;
        // If it exists, load the configuration for the component
        if(parms.ContainsKey(Constants.USER_CONFIG))
        {
            this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
        }
        // Retrieve the value of "Foo" from configuration
        var foo = this.configuration.AppSettings.Settings["Foo"].Value;
    }
    ...
}
```

Kullanıyorsanız bir `Get` yöntemi, bileşenin bir örneğini döndürülecek her ikisi de geçirir emin olmanız gerekir `Context` ve `Dictionary<string, Object>` oluşturucusuna parametre. Aşağıdaki örnek, temel bir `Get` düzgün bir şekilde bu değerleri geçirir yöntemi:

```csharp
public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
{
    return new MyComponent(ctx, parms);
}
```

## <a name="how-to-update-scpnet"></a>SCP.NET güncelleştirme

NuGet paket yükseltme SCP.NET son sürümlerini destekler. Yeni bir güncelleştirme kullanılabilir olduğunda, bir yükseltme bildirimi alırsınız. Yükseltme için el ile denetlemek için şu adımları izleyin:

1. **Çözüm Gezgini**’nde projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.

2. Paket Yöneticisi'nden seçin **güncelleştirmeleri**. Bir güncelleştirme varsa, listelenir. Tıklayın **güncelleştirme** paketini yüklemek için.

> [!IMPORTANT]  
> Projenizin NuGet kullanmayan SCP.NET önceki bir sürümü ile oluşturulmuş olsa bile, yeni bir sürüme güncelleştirmek için aşağıdaki adımları gerçekleştirmeniz gerekir:
>
> 1. **Çözüm Gezgini**’nde projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
> 2. Kullanarak **arama** alan, arayın ve ardından eklemek **kullandığı Microsoft.SCP.Net.SDK** projeye.

## <a name="troubleshoot-common-issues-with-topologies"></a>Topolojileri ile ilgili yaygın sorunları giderme

### <a name="null-pointer-exceptions"></a>Null işaretçi özel durumları

Linux tabanlı HDInsight kümesi ile bir C# topolojisi kullanırken bolt ve kullanan bileşenleri spout **ConfigurationManager** zamanında yapılandırma ayarları, null işaretçi özel durumları döndürebilir okumak için.

Yapılandırma, projeniz için Storm topolojiye topolojisi bağlamda bir anahtar ve değer çifti olarak geçirilir. Bunlar hazırlarken bileşenleriniz için geçirilen sözlük nesnesi alınabilir.

Daha fazla bilgi için [ConfigurationManager](#configurationmanager) bu belgenin bölüm.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

C# topolojisi ile Linux tabanlı HDInsight kümesi kullanırken, şu hatayla karşılaşabilirsiniz:

    System.TypeLoadException: Failure has occurred while loading a type.

Mono'ı destekleyen .NET sürümü ile uyumlu değil bir ikili kullandığınızda bu hata oluşur.

Linux tabanlı HDInsight kümeleri için projeniz .NET 4.5 için derlenmiş ikili dosyaları kullandığından emin olun.

### <a name="test-a-topology-locally"></a>Bir topoloji yerel olarak test etme

Bazı durumlarda, bir kümeye topoloji bir kolayca olmasına rağmen bir topoloji yerel olarak test gerekebilir. Ve yerel olarak geliştirme ortamınızda bu makaledeki örnek topoloji test çalıştırmak için aşağıdaki adımları kullanın.

> [!WARNING]  
> Yerel test yalnızca çalışır basic için C#-yalnızca topolojileri. Yerel test karma topolojiler veya birden çok akışı kullanan Topolojileri için kullanamazsınız.

1. İçinde **Çözüm Gezgini**projeye sağ tıklayıp seçin **özellikleri**. Proje özelliklerinde değişiklik **çıkış türü** için **konsol uygulaması**.

    ![Çıkış türü vurgulanmış olan proje özellikleri ekran görüntüsü](./media/apache-storm-develop-csharp-visual-studio-topology/outputtype.png)

   > [!NOTE]
   > Değiştirmeyi unutmayın **çıkış türü** geri **sınıf kitaplığı** bir kümeye topoloji dağıtmadan önce.

2. İçinde **Çözüm Gezgini**projeye sağ tıklayın ve ardından **Ekle** > **yeni öğe**. Seçin **sınıfı**girin **LocalTest.cs** sınıfı adı. Son olarak, tıklayın **Ekle**.

3. Açık **LocalTest.cs**ve aşağıdakileri ekleyin **kullanarak** üst deyimi:

    ```csharp
    using Microsoft.SCP;
    ```

4. İçeriği aşağıdaki kodu kullanın **LocalTest** sınıfı:

    ```csharp
    // Drives the topology components
    public void RunTestCase()
    {
        // An empty dictionary for use when creating components
        Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

        #region Test the spout
        {
            Console.WriteLine("Starting spout");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext spoutCtx = LocalContext.Get();
            // Get a new instance of the spout, using the local context
            Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

            // Emit 10 tuples
            for (int i = 0; i < 10; i++)
            {
                sentences.NextTuple(emptyDictionary);
            }
            // Use LocalContext to persist the data stream to file
            spoutCtx.WriteMsgQueueToFile("sentences.txt");
            Console.WriteLine("Spout finished");
        }
        #endregion

        #region Test the splitter bolt
        {
            Console.WriteLine("Starting splitter bolt");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext splitterCtx = LocalContext.Get();
            // Get a new instance of the bolt
            Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);

            // Set the data stream to the data created by the spout
            splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
            // Get a batch of tuples from the stream
            List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
            // Process each tuple in the batch
            foreach (SCPTuple tuple in batch)
            {
                splitter.Execute(tuple);
            }
            // Use LocalContext to persist the data stream to file
            splitterCtx.WriteMsgQueueToFile("splitter.txt");
            Console.WriteLine("Splitter bolt finished");
        }
        #endregion

        #region Test the counter bolt
        {
            Console.WriteLine("Starting counter bolt");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext counterCtx = LocalContext.Get();
            // Get a new instance of the bolt
            Counter counter = Counter.Get(counterCtx, emptyDictionary);

            // Set the data stream to the data created by splitter bolt
            counterCtx.ReadFromFileToMsgQueue("splitter.txt");
            // Get a batch of tuples from the stream
            List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
            // Process each tuple in the batch
            foreach (SCPTuple tuple in batch)
            {
                counter.Execute(tuple);
            }
            // Use LocalContext to persist the data stream to file
            counterCtx.WriteMsgQueueToFile("counter.txt");
            Console.WriteLine("Counter bolt finished");
        }
        #endregion
    }
    ```

    Kod açıklamaları okumak için bir dakikamızı ayıralım. Bu kod **LocalContext** bileşenleri geliştirme ortamını ve bunun çalıştırmak için metin dosyaları yerel sürücüde bileşenleri arasındaki veri akışını sürdürür.

1. Açık **Program.cs**, aşağıdaki ekleyin **ana** yöntemi:

    ```csharp
    Console.WriteLine("Starting tests");
    System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
    // Initialize the runtime
    SCPRuntime.Initialize();

    //If we are not running under the local context, throw an error
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
    {
        throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
    }
    // Create test instance
    LocalTest tests = new LocalTest();
    // Run tests
    tests.RunTestCase();
    Console.WriteLine("Tests finished");
    Console.ReadKey();
    ```

2. Değişiklikleri kaydedin ve ardından **F5** veya **hata ayıklama** > **hata ayıklamayı Başlat** projeyi başlatın. Bir konsol penceresi görünür ve günlük testleri ilerleme durumunun gerekir. Zaman **testler bitene** görünürse, pencereyi kapatmak için herhangi bir tuşa basın.

3. Kullanım **Windows Explorer** projenizi içeren dizini bulunamadı. Örneğin: **C:\Users\<your_user_name > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. Bu dizinde açın **Bin**ve ardından **hata ayıklama**. Testleri çalıştırdığınızda üretilmiş olan metin dosyaları görmeniz gerekir: sentences.txt counter.txt ve splitter.txt. Her metin dosyasını açın ve verileri inceleyin.

   > [!NOTE]  
   > Dize verileri, bu dosyaları ondalık değerleri dizisi olarak devam ettirir. Örneğin, \[[97,103,111]] içinde **splitter.txt** word dosyasıdır *ve*.

> [!NOTE]  
> Ayarladığınızdan emin olun **proje türü** geri **sınıf kitaplığı** HDInsight kümesinde Storm için dağıtmadan önce.

### <a name="log-information"></a>Günlük bilgileri

Kullanarak bilgi topolojisi bileşenlerinizi kolayca kaydedebilirsiniz `Context.Logger`. Örneğin, aşağıdaki komut, bilgilendirici günlük girişi oluşturur:

```csharp
Context.Logger.Info("Component started");
```

Günlüğe kaydedilen bilgileri görüntülenebilir **Hadoop hizmeti günlüğünü**, içinde bulunan **Sunucu Gezgini**. HDInsight kümesi üzerinde Storm'a için giriş genişletin ve ardından **Hadoop hizmeti günlüğünü**. Son olarak, günlük dosyasını görüntülemek için seçin.

> [!NOTE]  
> Günlükler, kümeniz tarafından kullanılan Azure depolama hesabında depolanır. Visual Studio'da günlükleri görüntülemek için depolama hesabı sahibi Azure aboneliği için oturum açmanız gerekir.

### <a name="view-error-information"></a>Hata bilgilerini görüntüle

Çalışan bir topolojide ortaya çıkan hataları görüntülemek için aşağıdaki adımları kullanın:

1. Gelen **Sunucu Gezgini**, HDInsight kümesinde Storm sağ tıklatın ve seçin **görünümü Storm topolojilerini**.

2. İçin **Spout** ve **Cıvatalar**, **son hata** sütun son hata hakkında bilgi içerir.

3. Seçin **Spout kimliği** veya **Bolt kimliği** listelenen hatalı bileşeni. Ayrıntılar sayfasında görüntülenen, ek hata bilgileri listelenir **hataları** sayfanın alt kısmındaki bölümde.

4. Daha fazla bilgi edinmek için seçin bir **bağlantı noktası** gelen **yürütücüler** Storm çalışan günlüğü için son birkaç dakikada görmek için bu sayfanın bölümü.

### <a name="errors-submitting-topologies"></a>Topolojileri gönderme hataları

Bir topoloji için HDInsight gönderilirken hatalarla karşılaşırsanız, HDInsight kümenizi topolojisi gönderimini işleyen sunucu tarafı bileşenleri için günlükleri bulabilirsiniz. Bu günlükleri almak için bir komut satırından aşağıdaki komutu kullanın:

    scp sshuser@clustername-ssh.azurehdinsight.net:/var/log/hdinsight-scpwebapi/hdinsight-scpwebapi.out .

Değiştirin __sshuser__ küme SSH kullanıcı hesabı ile. Değiştirin __clustername__ ile HDInsight kümesinin adı. Kullanma hakkında daha fazla bilgi için `scp` ve `ssh` HDInsight ile bkz [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

Gönderimler birden çok nedenden dolayı başarısız olabilir:

* JDK yüklü değil veya yolu değil.
* Gerekli Java bağımlılıkları gönderisine dahil edilmez.
* Uyumsuz bağımlılıklara.
* Yinelenen topolojisi adları.

Varsa `hdinsight-scpwebapi.out` günlük içeren bir `FileNotFoundException`, bu aşağıdaki durumlar neden:

* JDK geliştirme ortamını yolda değil. Geliştirme ortamını ve aynı JDK'nin yüklü olduğunu doğrulayın `%JAVA_HOME%/bin` yolda.
* Bir Java bağımlılığı eksik. Tüm gerekli .jar dosyalarını gönderim bir parçası olarak dahil olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Bir örneği verileri işleme için Event hubs bkz [HDInsight üzerinde Storm ile Azure Event hubs'tan olay işleme](apache-storm-develop-csharp-event-hub-topology.md).

Birden çok akış veri akışı ayıran bir C# topolojisi örneği için bkz: [C# Storm örneği](https://github.com/Blackmist/csharp-storm-example).

C# topolojileri oluşturma hakkında daha fazla bilgi bulmak için bkz: [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

HDInsight ile çalışmak daha yollarını ve HDInsight örnekleri üzerinde daha fazla Storm için aşağıdaki belgelere bakın:

**Microsoft SCP.NET**

* [SCP Programlama Kılavuzu](apache-storm-scp-programming-guide.md)

**HDInsight üzerinde Apache Storm**

* [HDInsight üzerinde Apache Storm topolojileri dağıtma ve izleme](apache-storm-deploy-monitor-topology.md)
* [HDInsight üzerinde Apache Storm için örnek topolojiler](apache-storm-example-topology.md)

**HDInsight üzerinde Apache Hadoop**

* [HDInsight üzerinde Apache Hadoop ile Apache Hive'ı kullanma](../hadoop/hdinsight-use-hive.md)
* [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma](../hadoop/hdinsight-use-pig.md)
* [HDInsight üzerinde Apache Hadoop ile Apache Hadoop MapReduce kullanma](../hadoop/hdinsight-use-mapreduce.md)

**HDInsight üzerinde Apache HBase**

* [HDInsight üzerinde Apache HBase kullanmaya başlama](../hbase/apache-hbase-tutorial-get-started-linux.md)
