---
title: "Visual Studio ve C# - Azure Hdınsight'ta Apache Storm topolojileri | Microsoft Docs"
description: "Storm topolojilerini C# ' ta oluşturmayı öğrenin. Basit word count topolojisi için Visual Studio Hadoop araçlarını kullanarak Visual Studio'da oluşturun."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 380d804f-a8c5-4b20-9762-593ec4da5a0d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/02/2017
ms.author: larryfr
ms.openlocfilehash: 3ee89b6644ba395e0a6c28ecc2c082c2f7393ac8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="develop-c-topologies-for-apache-storm-by-using-the-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake araçları kullanarak Apache Storm için C# topolojileri geliştirme

Visual Studio için Azure Data Lake (Hadoop) araçları kullanarak C# Storm topolojisini oluşturmayı öğrenin. Bu belge Visual Studio'da bir Storm projesi oluşturma, yerel olarak test ve Azure Hdınsight kümesi üzerinde Apache Storm dağıtma işleminin adım adım anlatılmaktadır.

Ayrıca C# ve Java bileşenlerini kullanan karma topolojiler oluşturmayı öğrenin.

> [!NOTE]
> Bu belgede yer alan adımlar Windows geliştirme ortamına Visual Studio ile kullanır, ancak bir Linux veya Windows tabanlı Hdınsight kümesine derlenmiş proje gönderilebilir. Yalnızca Linux tabanlı kümelerde 28 Ekim 2016'dan sonra oluşturulan SCP.NET topolojileri destekler.

Linux tabanlı bir kümeyle bir C# topolojisi kullanmak için sürüm 0.10.0.6 için projeniz tarafından kullanılan veya üzeri Microsoft.SCP.Net.SDK NuGet paketini güncelleştirmeniz gerekir. Paketin sürümünün ayrıca HDInsight üzerinde yüklü olan Storm ana sürümüyle eşleşmesi gerekir.

| Hdınsight sürümü | Storm sürüm | SCP.NET sürüm | Varsayılan Mono sürümü |
|:-----------------:|:-------------:|:---------------:|:--------------------:|
| 3.3 |0.10.x |0.10.x.x</br>(yalnızca Windows tabanlı Hdınsight üzerinde) | NA |
| 3.4 | 0.10.0.x | 0.10.0.x | 3.2.8 |
| 3,5 | 1.0.2.x | 1.0.0.x | 4.2.1 |
| 3.6 | 1.1.0.x | 1.0.0.x | 4.2.8 |

> [!IMPORTANT]
> Linux tabanlı kümelerdeki C# topolojilerinin .NET 4.5 kullanması ve HDInsight kümesi üzerinde çalışması için Mono kullanması gerekir. Denetleme [Mono Uyumluluk](http://www.mono-project.com/docs/about-mono/compatibility/) olası uyumsuzlukları için.

## <a name="install-visual-studio"></a>Visual Studio yükleme

Visual Studio aşağıdaki sürümlerinden birini kullanarak C# topolojileri SCP.NET ile geliştirme yapabilirsiniz:

* Visual Studio 2012 ile [güncelleştirme 4](http://www.microsoft.com/download/details.aspx?id=39305)

* Visual Studio 2013 [güncelleştirme 4](http://www.microsoft.com/download/details.aspx?id=44921) veya [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)

* Visual Studio 2015 veya [Visual Studio 2015 topluluk](https://go.microsoft.com/fwlink/?LinkId=532606)

* Visual Studio 2017 (herhangi bir sürümünü)

## <a name="install-data-lake-tools-for-visual-studio"></a>Visual Studio için yükleme Data Lake araçları

Visual Studio için Data Lake Araçları'nı yüklemek için adımları [Visual Studio için Data Lake araçları kullanarak çalışmaya başlama](hdinsight-hadoop-visual-studio-tools-get-started.md).

## <a name="install-java"></a>Java'yı yükleme

Visual Studio'dan bir Storm topolojisinin gönderdiğinizde, SCP.NET topoloji ve bağımlılıklarını içeren bir zip dosyası oluşturur. Linux tabanlı kümelerde ile daha uyumlu bir biçimde kullandığından Java bu ZIP dosyaları oluşturmak için kullanılır.

1. Geliştirme ortamınızı Java Geliştirme Seti (JDK) 7 veya sonraki bir sürümü yükleyin. Oracle JDK dan alabileceğiniz [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html). Aynı zamanda [diğer Java dağıtımları](http://openjdk.java.net/).

2. `JAVA_HOME` Ortam değişkeni Java içeren dizine işaret etmelidir.

3. `PATH` Ortam değişkeni içermelidir `%JAVA_HOME%\bin` dizin.

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
           string javaHome = Environment.GetEnvironmentVariable(“JAVA_HOME”);
           if (!string.IsNullOrEmpty(javaHome))
           {
               string jarExe = Path.Combine(javaHome + @”\bin”, “jar.exe”);
               if (File.Exists(jarExe))
               {
                   Console.WriteLine(“JAVA Is Installed properly”);
                    return;
               }
               else
               {
                   Console.WriteLine(“A valid JAVA JDK is not found. Looks like JRE is installed instead of JDK.”);
               }
           }
           else
           {
             Console.WriteLine(“A valid JAVA JDK is not found. JAVA_HOME environment variable is not set.”);
           }
       }  
   }
}
```

## <a name="storm-templates"></a>Storm şablonları

Visual Studio için Data Lake araçları aşağıdaki şablonlar sağlar:

| Proje türü | Gösterir |
| --- | --- |
| Storm uygulaması |Boş bir Storm topolojisini projesi. |
| Storm Azure SQL yazıcı örnek |Azure SQL veritabanına yazmak nasıl. |
| Storm Azure Cosmos DB okuyucu örneği |Azure Cosmos DB'den okumak nasıl. |
| Storm Azure Cosmos DB yazan örnek |Azure Cosmos DB yazma yapma. |
| Storm EventHub okuyucu örneği |Azure olay hub'larından okumayı yapma. |
| Storm EventHub yazan örnek |Azure Event Hubs'a yazmak nasıl. |
| Storm HBase okuyucu örneği |Hdınsight'ta HBase okuma kümeleri. |
| Storm HBase yazan örnek |Hdınsight'ta HBase için yazma kümeleri. |
| Storm karma örnek |Bir Java bileşeni kullanma |
| Storm örnek |Temel word count topolojisi. |

> [!WARNING]
> Tüm Şablonları Linux tabanlı Hdınsight ile çalışır. Şablonlar tarafından kullanılan Nuget paketleri Mono ile uyumlu olmayabilir. Denetleme [Mono Uyumluluk](http://www.mono-project.com/docs/about-mono/compatibility/) belge ve kullanma [.NET taşınabilirlik Çözümleyicisi](hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) olası bir sorunu belirlemek için.

Bu belgede aşağıdaki adımlarda, bir topoloji oluşturmak için temel Storm uygulaması proje türü kullanın.

### <a name="hbase-templates-notes"></a>HBase şablonları notları

HBase okuyucu ve yazıcı şablonları, Hdınsight kümesinde bir HBase ile iletişim kurmak için değil HBase Java API HBase REST API kullanın.

### <a name="eventhub-templates-notes"></a>EventHub şablonları notları

> [!IMPORTANT]
> EventHub okuyucu şablonu ile birlikte gelen Java tabanlı EventHub spout bileşen sürüm 3.5 veya sonrasını hdınsight'ta Storm çalışmayabilir. Bu bileşen güncelleştirilmiş bir sürümü kullanılabilir [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).

Bu kullanan örnek bir topoloji için bkz: bileşen ve Hdınsight 3.5 üzerinde Storm ile works [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).

## <a name="create-a-c-topology"></a>Bir C# topolojisi oluşturma

1. Visual Studio'ni açın, **dosya** > **yeni**ve ardından **proje**.

2. Gelen **yeni proje** penceresinde genişletin **yüklü** > **şablonları**seçip **Azure Data Lake**. Şablonları listesinden seçin **Storm uygulaması**. Ekranın alt kısmındaki girin **WordCount** uygulamanın adı.

    ![Yeni Proje penceresinin ekran penceresi](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

3. Proje oluşturduktan sonra aşağıdaki dosyaları sahip olmanız gerekir:

   * **Program.cs**: Bu dosya, projeniz için topoloji tanımlar. Bir spout ve bir Cıvata oluşan bir varsayılan topolojisi varsayılan olarak oluşturulur.

   * **Spout.cs**: rastgele sayılar yayar bir örnek spout.

   * **Bolt.cs**: spout'un gösterilen sayıların sayısı tutan bir örnek Cıvata.

     NuGet proje oluşturduğunuzda, en son yüklemeleri [SCP.NET paket](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).

     [!INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

### <a name="implement-the-spout"></a>Spout uygulama

1. Açık **Spout.cs**. Spout'lar bir dış kaynaktan topolojisinde verileri okumak için kullanılır. Bir spout için ana bileşeni vardır:

   * **NextTuple**: spout yeni tanımlama grubu yayma izin Storm tarafından çağrılır.

   * **ACK** (yalnızca işlem topolojisi): spout gönderilen diziler için topolojideki başka bileşenler tarafından başlatılan onayları işler. Bir tanımlama grubu aktarımının aşağı akış bileşenleri tarafından başarıyla işlendi bilmeniz spout olanak sağlar.

   * **Başarısız** (yalnızca işlem topolojisi): işleme, başarısız topoloji içindeki diğer bileşenlere işleme tanımlama grubu. Başarısız yöntemi uygulama, böylece yeniden işlenebilir tanımlama grubu yeniden yayma olanak sağlar.

2. Değiştir **Spout** aşağıdaki metinle sınıfı. Bu spout bir cümle rastgele topoloji yayar.

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

### <a name="implement-the-bolts"></a>Cıvatalar uygulama

1. Varolan silme **Bolt.cs** proje dosyasından.

2. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle** > **yeni öğe**. Listesinden **Storm Cıvata**ve girin **Splitter.cs** adı. Adlı ikinci bir Cıvata oluşturmak için bu işlemi yineleyin **Counter.cs**.

   * **Splitter.cs**: cümleleri tek tek sözcüklere böler ve yeni akış sözcüklerin yayar Cıvata uygular.

   * **Counter.cs**: her sözcüğün sayar ve sözcükleri ve sayısı her sözcüğün için yeni bir akış yayar Cıvata uygular.

     > [!NOTE]
     > Bu Cıvatalar okuma ve akışlara yazma, ancak bir veritabanı veya hizmet gibi kaynakları ile iletişim kurmak için de kullanabilirsiniz.

3. Açık **Splitter.cs**. Varsayılan olarak yalnızca bir yöntemi vardır: **yürütme**. Execute yöntemi Cıvata işlemek için bir tanımlama grubu aldığında çağrılır. Burada, okuma ve gelen diziler işlemek ve giden tanımlama grubu yayma.

4. Değiştir **Bölümlendirici** aşağıdaki kodla sınıfı:

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

5. Açık **Counter.cs**ve sınıf içeriğini aşağıdakilerle değiştirin:

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

Spout'lar ve Cıvatalar bileşenler arasında veri akışını tanımlayan bir grafik halinde düzenlenir. Bu topoloji, grafiği aşağıdaki gibidir:

![Bileşenleri nasıl düzenlenir diyagramı](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

Cümleleri spout yayılan ve Bölümlendirici Cıvata örneklerine dağıtılır. Bölümlendirici Cıvata cümleleri sayaç Cıvata dağıtılmış sözcükler içine keser.

Sözcük sayısını yerel olarak sayaç örneğinde tutulmadığından belirli kelimeleri aynı sayacı Cıvata örneğini akış emin olmak istiyoruz. Her bir örnek, belirli kelimeleri izler. Bölümlendirici Cıvata hiçbir durumunu koruyan olduğundan, gerçekten Bölümlendirici hangi örneğinin hangi tümceyi alır önemli değildir.

Açık **Program.cs**. Önemli yöntemi **GetTopologyBuilder**, Storm için gönderilen topoloji tanımlamak için kullanılır. Değiştir **GetTopologyBuilder** daha önce açıklanan topoloji uygulamak için aşağıdaki kod ile:

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

## <a name="submit-the-topology"></a>Topoloji gönderme

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Hdınsight üzerinde Storm Gönder**.

   > [!NOTE]
   > İstenirse, Azure aboneliğiniz için kimlik bilgilerini girin. Birden fazla aboneliğiniz varsa, Hdınsight kümesi üzerinde Storm'a içeren oturum açın.

2. Hdınsight küme üzerinde Storm'a seçin **Storm kümesi** aşağı açılan listeyi ve ardından **gönderme**. Gönderim kullanarak başarılı olursa, izleyebilirsiniz **çıkış** penceresi.

3. Topoloji başarıyla gönderildi, **Storm topolojilerini** küme görünür. Seçin **WordCount** çalışan topolojisi ile ilgili bilgileri görüntülemek için listeden topolojisi.

   > [!NOTE]
   > Ayrıca görüntüleyebilirsiniz **Storm topolojilerini** gelen **Sunucu Gezgini**. Genişletme **Azure** > **Hdınsight**, Hdınsight kümesinde bir Storm sağ tıklayın ve ardından **görünüm Storm topolojilerini**.

    Topolojide bileşenleri hakkında bilgi görüntülemek için diyagramdaki bileşeni çift tıklatın.

4. Gelen **topoloji özeti** görüntülemek için tıklatın **KILL** topoloji durdurmak için.

   > [!NOTE]
   > Storm topolojilerini devre dışı bırakılır veya küme silinene kadar çalışmaya devam eder.

## <a name="transactional-topology"></a>İşlem topolojisi

Önceki işlem olmayan topolojidir. Topoloji bileşenlerinde iletileri yeniden oynatmak için işlevsellik kullanılmaz. Bir işlem topolojisi ilişkin bir örnek, bir proje oluşturmak ve **Storm örnek** proje türü olarak.

İşlem topolojileri yeniden yürütme veri desteklemek için aşağıdakileri uygulayın:

* **Meta verileri önbelleğe alma**: spout meta verileri alınır ve bir hata oluşursa yeniden yayılan böylece yayılan veriler hakkında depolamanız gerekir. Örnek tarafından yayılan veriler küçük olduğundan, her tanımlama grubu için ham verileri yeniden yürütme için bir sözlük depolanır.

* **ACK**: her Cıvata topolojisi içinde çağırabilirsiniz `this.ctx.Ack(tuple)` bir tanımlama grubu başarıyla işlediği bildiremedi. Tüm Cıvatalar tuple acked olduğunda `Ack` spout yöntemi çağrılır. `Ack` Spout yeniden yürütme için önbelleğe verileri kaldırmak bir yöntem sağlar.

* **Başarısız**: her Cıvata çağırabilirsiniz `this.ctx.Fail(tuple)` işleme için bir tanımlama grubu başarısız olduğunu belirtmek için. İçin hata yayar `Fail` burada yer alan tanımlama grubu yeniden kullanarak spout yöntemi önbelleğe alınan meta verileri.

* **Sıra kimliği**: bir tanımlama grubu yayma, benzersiz sıra kimliği belirtilebilir. Bu değer tanımlama grubu yeniden yürütme (Ack ve başarısız) işleme tanımlar. Spout Örneğin, **Storm örnek** project veri yayma olduğunda aşağıdaki kullanır:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    Bu kod içinde yer alan sırası kimliği değerine sahip bir cümle varsayılan akışa içeren bir tanımlama grubu yayar **lastSeqId**. Bu örnek için **lastSeqId** gösterilen her bir tanımlama grubu için artırılır.

Örnekte gösterildiği şekilde **Storm örnek** proje, bir bileşenin işlem olup yapılandırmasını temel alarak zamanında ayarlanmış olabilir.

## <a name="hybrid-topology-with-c-and-java"></a>C# ve Java ile karma topolojisi

Burada bazı bileşenleri C# ve diğerleri Java karma topolojiler oluşturmak için Visual Studio için Data Lake araçları da kullanabilirsiniz.

Bir karma topolojisi ilişkin bir örnek, bir proje oluşturmak ve **Storm karma örnek**. Bu örnek türü aşağıdaki kavramları göstermektedir:

* **Java spout** ve **C# Cıvata**: tanımlıysa **HybridTopology_javaSpout_csharpBolt**.

    * İşlem sürüm tanımlanan **HybridTopologyTx_javaSpout_csharpBolt**.

* **C# spout** ve **Java Cıvata**: tanımlıysa **HybridTopology_csharpSpout_javaBolt**.

    * İşlem sürüm tanımlanan **HybridTopologyTx_csharpSpout_javaBolt**.

  > [!NOTE]
  > Bu sürüm ayrıca Clojure kodu bir metin dosyasından bir Java bileşeni olarak kullanımı gösterilmiştir.


Proje gönderildiğinde, kullanılan topoloji geçiş yapmak için yalnızca taşıma `[Active(true)]` kümeye göndermeden önce kullanmak istediğiniz topoloji ifadesine.

> [!NOTE]
> Gerekli olan tüm Java dosyalar bu projede bir parçası olarak sağlanan **JavaDependency** klasör.

Oluşturma ve karma topolojisi gönderme aşağıdakileri dikkate alın:

* Kullanmalısınız **JavaComponentConstructor** bir spout için Java sınıfının bir örneği oluşturulamadı veya Cıvata.

* Kullanmanız gereken **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** verilerini içine veya dışına Java nesnelerini Java bileşenlerini JSON için seri hale getirmek için.

* Sunucuya topoloji gönderirken kullanmalısınız **ek yapılandırmalar** belirtmek için seçeneği **Java dosya yolları**. Belirtilen yol, Java sınıfları içeren JAR dosyaları içeren dizini olmalıdır.

### <a name="azure-event-hubs"></a>Azure Event Hubs

SCP.NET sürüm 0.9.4.203 bir yeni sınıf ve olay hub'ı spout (olay hub'larından okuyan bir Java spout) ile çalışmak için özellikle yöntemi sunar. Bir olay hub'ı spout kullanan bir topolojisi oluşturduğunuzda, aşağıdaki yöntemleri kullanın:

* **EventHubSpoutConfig** sınıfı: spout bileşeni için yapılandırma içeren bir nesne oluşturur.

* **TopologyBuilder.SetEventHubSpout** yöntemi: olay hub'ı spout bileşeni topolojiye ekler.

> [!NOTE]
> Hala kullanmalısınız **CustomizedInteropJSONSerializer** spout tarafından üretilen veri seri hale getirilemedi.

## <a id="configurationmanager"></a>ConfigurationManager kullanın

Kullanmayan **ConfigurationManager** Cıvata yapılandırma değerlerini almak ve bileşenleri spout. Bunun yapılması bir null işaretçi özel durumu neden olabilir. Bunun yerine, projeniz için yapılandırma Storm topolojisini topoloji bağlamında bir anahtar ve değer çifti olarak geçirilir. Yapılandırma değerlerini kullanır her bileşenin bunları başlatma sırasında bağlamdan almanız gerekir.

Aşağıdaki kod, bu değerleri almaya gösterilmiştir:

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

Kullanırsanız, bir `Get` , bileşeninin bir örneğini döndürülecek yöntemi her ikisi de geçirir emin olmalısınız `Context` ve `Dictionary<string, Object>` Oluşturucusu parametrelerinin. Aşağıdaki örnek temel olan `Get` düzgün bu değerleri geçirir yöntemi:

```csharp
public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
{
    return new MyComponent(ctx, parms);
}
```

## <a name="how-to-update-scpnet"></a>SCP.NET güncelleştirme

Paket yükseltmesi NuGet aracılığıyla SCP.NET en son sürümleri destekler. Yeni bir güncelleştirme kullanılabilir olduğunda, bir yükseltme bildirimi alırsınız. El ile yükseltme için denetlemek için aşağıdaki adımları izleyin:

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **NuGet paketlerini Yönet**.

2. Paket Yöneticisi'nden seçin **güncelleştirmeleri**. Bir güncelleştirme olup olmadığını listelenir. Tıklatın **güncelleştirme** paketi yüklemek için.

> [!IMPORTANT]
> Projenizin NuGet kullanmadı SCP.NET önceki bir sürümüyle oluşturulduysa, daha yeni bir sürüme güncelleştirmek için aşağıdaki adımları gerçekleştirmeniz gerekir:
>
> 1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **NuGet paketlerini Yönet**.
> 2. Kullanarak **arama** alanına arayın ve ardından ekleyin, **Microsoft.SCP.Net.SDK** projeye.

## <a name="troubleshoot-common-issues-with-topologies"></a>Topolojileri ile ilgili genel sorunları giderme

### <a name="null-pointer-exceptions"></a>Null işaretçi özel durumlar

Bir C# topolojisi ile Linux tabanlı Hdınsight kümesi kullanırken, Cıvata ve kullanan bileşenleri spout **ConfigurationManager** yapılandırma ayarlarını çalışma zamanında null işaretçi özel durumlar döndürebilir okunamıyor.

Projeniz için yapılandırma Storm topolojisini topoloji bağlamında bir anahtar ve değer çifti olarak geçirilir. Bunlar hazırlarken bileşenlerinizin geçirilen sözlük nesnesi alınabilir.

Daha fazla bilgi için bkz: [ConfigurationManager](#configurationmanager) bu belgenin bölüm.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

Bir C# topolojisi ile Linux tabanlı Hdınsight kümesi kullanırken, şu hatayla karşılaşabilirsiniz:

    System.TypeLoadException: Failure has occurred while loading a type.

Mono destekleyen .NET sürümü ile uyumlu olmayan bir ikili kullandığınızda bu hata oluşur.

Linux tabanlı Hdınsight kümeleri için projeniz .NET 4. 5'için derlenmiş ikili dosyaları kullandığından emin olun.

### <a name="test-a-topology-locally"></a>Bir topoloji yerel olarak test etme

Bazı durumlarda, bir küme için bir topolojisi dağıtmak kolay olmasına rağmen bir topoloji yerel olarak test gerekebilir. Ve geliştirme ortamınızı yerel olarak bu öğreticideki örnek topoloji test çalıştırmak için aşağıdaki adımları kullanın.

> [!WARNING]
> Yerel test yalnızca çalışır için basic, C#-yalnızca topolojileri. Karma topolojiler veya birden çok akış kullanmak Topolojileri için yerel test kullanamazsınız.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **özellikleri**. Proje Özellikleri'nde değiştirme **çıktı türü** için **konsol uygulaması**.

    ![Vurgulanan çıktı türü ile Proje Özellikleri'nin ekran görüntüsü](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

   > [!NOTE]
   > Değiştirmeyi unutmayın **çıktı türü** başa **sınıf kitaplığı** bir kümeye topoloji dağıtmadan önce.

2. İçinde **Çözüm Gezgini**projeye sağ tıklayın ve ardından **Ekle** > **yeni öğe**. Seçin **sınıfı**ve girin **LocalTest.cs** sınıfı adı. Son olarak, tıklatın **Ekle**.

3. Açık **LocalTest.cs**ve aşağıdakileri ekleyin **kullanarak** deyimini dosyanın üst:

    ```csharp
    using Microsoft.SCP;
    ```

4. Aşağıdaki kod içeriğini kullanmak **LocalTest** sınıfı:

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

    Kod açıklamaları okumak için bir dakikanızı ayırın. Bu kodu kullanır **LocalContext** bileşenleri geliştirme ortamını ve bunu çalıştırmak için yerel sürücü üzerindeki metin dosyaları için bileşenler arasındaki veri akışını devam ettirir.

1. Açık **Program.cs**ve aşağıdakileri ekleyin **ana** yöntemi:

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

2. Değişiklikleri kaydetmek ve ardından **F5** veya seçin **hata ayıklama** > **hata ayıklamayı Başlat** projeyi başlatın. Bir konsol penceresi görünür ve günlük testleri ilerleme durumunun gerekir. Zaman **testleri tamamlandı** görünür, pencereyi kapatmak için herhangi bir tuşa basın.

3. Kullanım **Windows Explorer** projenizi içeren dizini bulunamadı. Örneğin: **C:\Users\<your_user_name > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. Bu dizinde açmak **Bin**ve ardından **hata ayıklama**. Testleri çalıştırdığınızda üretilmiş olan metin dosyaları görmeniz gerekir: sentences.txt, counter.txt ve splitter.txt. Her metin dosyasını açın ve verileri inceleyin.

   > [!NOTE]
   > Dize verilerini bu dosyalardaki ondalık değerleri dizisi olarak devam ettirir. Örneğin, \[[97,103,111]] içinde **splitter.txt** dosyasıdır word *ve*.

> [!NOTE]
> Ayarladığınızdan emin olun **proje türü** başa **sınıf kitaplığı** Hdınsight kümesinde bir Storm dağıtmadan önce.

### <a name="log-information"></a>Günlük bilgileri

Kolayca bilgi topoloji bileşenlerinizi kullanarak oturum `Context.Logger`. Örneğin, aşağıdaki bilgilendirici günlük girişi oluşturur:

```csharp
Context.Logger.Info("Component started");
```

Günlüğe kaydedilen bilgileri alanından görüntülenebilir **Hadoop hizmeti günlüğünü**, içinde bulunan **Sunucu Gezgini**. Hdınsight kümesi üzerinde Storm'a giriş'i genişletin ve ardından genişletin **Hadoop hizmeti günlüğünü**. Son olarak, günlük dosyasını görüntülemek için seçin.

> [!NOTE]
> Günlükler, küme tarafından kullanılan Azure depolama hesabında depolanır. Visual Studio'da günlükleri görüntülemek için depolama hesabı sahibi Azure aboneliği ile oturum gerekir.

### <a name="view-error-information"></a>Hata bilgilerini görüntüleme

Çalışan bir topoloji oluşan hataları görüntülemek için aşağıdaki adımları kullanın:

1. Gelen **Sunucu Gezgini**, Hdınsight kümesinde Storm sağ tıklatın ve seçin **görünüm Storm topolojilerini**.

2. İçin **Spout** ve **Cıvatalar**, **son hata** sütun son hata hakkında bilgi içerir.

3. Seçin **Spout kimliği** veya **Cıvata kimliği** listelenen hatalı bileşeni için. Ayrıntıları sayfasında görüntülenen, ek hata bilgileri listede **hataları** sayfanın alt kısmına.

4. Daha fazla bilgi edinmek için seçin bir **bağlantı noktası** gelen **yürütücüler** Storm çalışan günlük için son birkaç dakikada görmek için bu sayfasının bölümünde.

### <a name="errors-submitting-topologies"></a>Topolojileri gönderme hataları

Bir topolojiyi hdınsight'a gönderilirken hatalarla karşılaşırsanız, Hdınsight kümenize topoloji gönderimini işlemek sunucu tarafı bileşenleri için günlükleri bulabilirsiniz. Bu günlükler almak için bir komut satırından aşağıdaki komutu kullanın:

    scp sshuser@clustername-ssh.azurehdinsight.net:/var/log/hdinsight-scpwebapi/hdinsight-scpwebapi.out .

Değiştir __sshuser__ küme için SSH kullanıcı hesabıyla. Değiştir __clustername__ Hdınsight kümesi adı. Kullanma hakkında daha fazla bilgi için `scp` ve `ssh` Hdınsight ile bkz [Hdınsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

Gönderimleri birden çok nedenlerden dolayı başarısız olabilir:

* JDK yüklü değil veya yolu değil.
* Gerekli Java bağımlılıkları gönderisine dahil edilmez.
* Uyumsuz bağımlılıkları.
* Yinelenen topoloji adları.

Varsa `hdinsight-scpwebapi.out` günlük içeren bir `FileNotFoundException`, bunun aşağıdaki koşullardan nedeni olabilir:

* JDK geliştirme ortamı yolunda değil. JDK geliştirme ortamını ve, yüklü olduğunu doğrulayın `%JAVA_HOME%/bin` yolunda olduğundan.
* Java bağımlılığı eksik. Tüm gerekli .jar dosyalar gönderim bir parçası olarak dahil emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Bir örnek veriler işlenirken için Event hubs, bkz: [işlem Hdınsight üzerinde Storm ile Azure Event Hubs olaylarından](hdinsight-storm-develop-csharp-event-hub-topology.md).

Veri akışı birden çok akışlara böler bir C# topolojisi örneği için bkz: [C# Storm örnek](https://github.com/Blackmist/csharp-storm-example).

C# topolojileri oluşturma hakkında daha fazla bilgi bulmak için bkz: [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

Hdınsight ile çalışmak daha fazla yolunu ve Hdınsight örnekleri üzerinde daha fazla Storm için aşağıdaki belgelere bakın:

**Microsoft SCP.NET**

* [SCP Programlama Kılavuzu](hdinsight-storm-scp-programming-guide.md)

**Hdınsight üzerinde Apache Storm**

* [Dağıtma ve hdınsight'ta Apache Storm topolojileri izleme](hdinsight-storm-deploy-monitor-topology.md)
* [HDInsight üzerinde Storm için örnek topolojiler](hdinsight-storm-example-topology.md)

**Hdınsight'ta Apache Hadoop**

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)
* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

**Hdınsight üzerinde Apache HBase**

* [Hdınsight'ta HBase ile çalışmaya başlama](hdinsight-hbase-tutorial-get-started.md)
