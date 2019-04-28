---
title: Azure HDInsight içerisindeki Storm için SCP.NET Programlama Kılavuzu
description: SCP.NET oluşturmak için kullanmayı öğrenin. AĞ tabanlı Storm Topolojileri için Azure HDInsight çalışan Storm ile kullanma.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2016
ms.openlocfilehash: c85074a2b26a79dbf5e464972e7f82b5955d15f1
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126894"
---
# <a name="scp-programming-guide"></a>SCP Programlama Kılavuzu
SCP, güvenilir ve tutarlı, gerçek zamanlı oluşturmak için platform ve yüksek performanslı bilgi işlem uygulama ' dir. Üst kısmındaki yerleşik [Apache Storm](https://storm.incubator.apache.org/) --bir akış işleme sistemi OSS topluluklar göre tasarlanmıştır. Storm Nathan Marz tarafından tasarlanmıştır ve açık Twitter tarafından kaynaklanan oluştu. Bunu yararlanır [Apache ZooKeeper](https://zookeeper.apache.org/), işbirliği ve durum yönetimini son derece güvenilir etkinleştirmek için başka bir Apache projesi dağıtılmış. 

Yalnızca Windows üzerinde Storm SCP proje unity'nin ancak proje, uzantıları ve Windows ekosisteminde özelleştirmesi de ekledik. Uzantıları .NET geliştirici deneyimi ve kitaplıkları içerir, Windows tabanlı bir dağıtım özelleştirme içerir. 

Genişletme ve özelleştirme biz OSS projeleri çatalını oluşturmanız gerekmez ve biz Storm üzerinde oluşturulmuş türetilmiş ekosistemlerini yararlanabiliriz şekilde gerçekleştirilir.

## <a name="processing-model"></a>İşlem modeli
SCP içinde veri diziler sürekli akışları olarak modellenir. Genellikle diziler bazı kuyruğuna ilk akış, ardından toplanma ve Storm topolojisini içinde barındırılan iş mantığı tarafından dönüştürülür, son çıktı başka bir SCP sistemine diziler olarak yöneltilen veya dağıtılmış dosya sistemi veya veritabanı gibi depolarına yürütülmesi SQL Server gibi.

![Verileri bir veri deposu akışları işleme besleme sıra diyagramı](./media/apache-storm-scp-programming-guide/queue-feeding-data-to-processing-to-data-store.png)

İçerisindeki Storm, bir uygulama topolojisinin bir hesaplama grafiği tanımlar. Her düğüm topolojisinde işleme mantığı içerir ve veri akışı düğümler arasındaki bağlantıları gösterir. Giriş verileri bir topolojiye eklemesine düğümler olarak adlandırılır _spout'lar_, verileri sıralamak için kullanılabilir. Giriş verilerini dosya günlükleri, işlemsel veritabanı, sistem performans sayacı vb. bulunduğu. Her iki giriş ve çıkış veri akışları ile düğümler olarak adlandırılır _Cıvatalar_, hangi gerçek veri filtreleme yapmak ve seçimleri ve toplama.

SCP en iyi çaba, en az bir kez destekler ve tam olarak-bir kez veri işleme. Bir dağıtılmış akış işleme uygulamasında, ağ kesintisi, makine arızasından ya da kullanıcı kod hatası vb. gibi veri işleme sırasında çeşitli hatalar oluşabilir. En az bir kez işlemeyi, tüm veriler hata oluştuğunda otomatik olarak aynı verileri yürüterek en az bir kez işlenir sağlar. En az bir kez işlemeyi, basit ve güvenilir ve uygun da birçok uygulama. Ancak, uygulamanın tam sayım gerektirdiğinde, aynı verileri olası uygulama topolojisinde yeniden bu yana en az bir kez işleme yeterli değildir. Bu durumda, tam olarak-işlem sonucu emin olmak için tasarlanan sonra bile, veriler yeniden yürütülmesi ve birden çok kez işlenen doğrudur.

SCP perde yararlanarak üzerinde Java sanal makinesi (JVM) Storm ile gerçek zamanlı veri işlem uygulamaları geliştirmek .NET geliştiricilerinin sağlar. .NET ve JVM yerel TCP yuvaları üzerinden iletişim kurar. Temel olarak her Spout/Cıvata bir .NET/Java işlemi, kullanıcı mantığını .NET işlem bir eklenti olarak çalıştığı çiftidir.

SCP üzerine bir veri işleme uygulaması derlemek için birkaç adım gerekir:

* Kuyruğu'ndan veri çekmek için Spout tasarlayıp yeniden açın.
* Tasarım ve girdi verilerini işlemek için Cıvatalar uygulamak ve bir veritabanı gibi harici depolar için verileri kaydedin.
* Topoloji tasarım, gönderme ve topoloji çalıştırın. Topoloji köşeler ve verileri tanımlar köşeler arasında akışlar. SCP topolojisi belirtimi almak ve her köşe bir mantıksal düğüm üzerinde çalıştığı bir Storm kümesinde dağıtın. Yük devretme ve ölçeklendirme Storm Görev Zamanlayıcı tarafından dikkate.

Bu belge birkaç basit örneğe nasıl SCP ile veri işleme uygulaması oluşturmak için kullanır.

## <a name="scp-plugin-interface"></a>SCP eklenti arabirimi
SCP eklentileri (veya uygulamalar) hem de Visual Studio içinde geliştirme aşamasında çalıştırabilirsiniz ve Üretim dağıtımı sonrasında Storm ardışık düzende takılı tek başına exe var. SCP eklenti yazma tıpkı diğer standart Windows konsol uygulamaları yazma olarak olur. Spout/Cıvata için bazı arabirimi SCP.NET platform bildirir ve kullanıcı eklenti kodu Bu arabirimler uygulamalıdır. Bu tasarım ana amacı, kullanıcının kendi iş logics ve SCP.NET platformu tarafından işlenecek başka şeyler bırakarak odaklanabilirsiniz.

Kullanıcı eklentisi kodu aşağıdakilere arabirimlerinden birini uygulamalıdır, topoloji işlem veya işlem olmayan olmasına ve bileşen spout veya Cıvata olmasına bağlıdır.

* ISCPSpout
* ISCPBolt
* ISCPTxSpout
* ISCPBatchBolt

### <a name="iscpplugin"></a>ISCPPlugin
ISCPPlugin eklentileri tüm türleri için ortak arabirimdir. Şu anda, bir işlevsiz arabirimidir.

    public interface ISCPPlugin 
    {
    }

### <a name="iscpspout"></a>ISCPSpout
ISCPSpout işlem olmayan spout arabirimidir.

     public interface ISCPSpout : ISCPPlugin                    
     {
         void NextTuple(Dictionary<string, Object> parms);         
         void Ack(long seqId, Dictionary<string, Object> parms);   
         void Fail(long seqId, Dictionary<string, Object> parms);  
     }

Zaman `NextTuple()` çağrılır, C\# kullanıcı kodu, bir veya daha fazla tanımlama grubu yayabilir. Varsa yaymak için hiçbir şey, bu yöntem herhangi bir şey yayma olmadan döndürmeniz gerekir. Not edilmesi gereken, `NextTuple()`, `Ack()`, ve `Fail()` tüm C tek bir iş parçacığında sıkı bir döngüde adlandırılır\# işlem. Hiçbir tanımlama grubu yayma için olduğunda, çok fazla CPU boşa değil olarak bu nedenle kısa bir süre (örneğin, 10 milisaniye) için NextTuple uyku sahip courteous.

`Ack()` ve `Fail()` ack mekanizması spec dosyasında yalnızca etkin olduğunda çağrılır. `seqId` Onaylanır veya başarısız olan tanımlama grubu tanımlamak için kullanılır. Bu nedenle işlem olmayan topolojisinde ACK etkinleştirilirse, aşağıdaki emit işlevi Spout içinde kullanılması gerekir:

    public abstract void Emit(string streamId, List<object> values, long seqId); 

ACK işlem olmayan topolojisinde desteklenmiyorsa `Ack()` ve `Fail()` işlevi boş bırakılabilir.

`parms` Bu işlevler giriş parametresi boş bir sözlük, gelecekte kullanılmak üzere ayrılmıştır.

### <a name="iscpbolt"></a>ISCPBolt
ISCPBolt işlem olmayan bolt arabirimidir.

    public interface ISCPBolt : ISCPPlugin 
    {
    void Execute(SCPTuple tuple);           
    }

Yeni bir tanımlama grubu mevcut olduğunda `Execute()` işlevi, işlem sırasında çağrılır.

### <a name="iscptxspout"></a>ISCPTxSpout
ISCPTxSpout işlem spout arabirimidir.

    public interface ISCPTxSpout : ISCPPlugin
    {
        void NextTx(out long seqId, Dictionary<string, Object> parms);  
        void Ack(long seqId, Dictionary<string, Object> parms);         
        void Fail(long seqId, Dictionary<string, Object> parms);        
    }

Kendi işlem olmayan karşı Bölümü'olduğu gibi `NextTx()`, `Ack()`, ve `Fail()` tüm C tek bir iş parçacığında sıkı bir döngüde adlandırılır\# işlem. Yaymak için hiçbir veri olduğunda sahip courteous `NextTx` uyku süresi (10 milisaniye) çok fazla CPU boşa değil olarak bu nedenle kısa bir süre.

`NextTx()` out parametresi yeni bir işlem başlatmak için çağrılan `seqId` de kullanılan işlem tanımlamak için kullanılan `Ack()` ve `Fail()`. İçinde `NextTx()`, kullanıcı veri Java tarafına yayabilir. Verileri yeniden yürütme desteklemek için ZooKeeper içinde depolanır. ZooKeeper kapasitesi sınırlı olduğundan, bir kullanıcı yalnızca meta veri yayma, toplu işlem spout verileri değil.

Storm yeniden yürütme bir işlem otomatik olarak başarısız olursa, bunu `Fail()` normal durumda çağrılmamalıdır. Ancak SCP'yi işlem spout'un yayılan meta verileri işaretlerseniz çağırabilirsiniz `Fail()` meta verilerin ne zaman geçersiz.

`parms` Bu işlevler giriş parametresi boş bir sözlük, gelecekte kullanılmak üzere ayrılmıştır.

### <a name="iscpbatchbolt"></a>ISCPBatchBolt
ISCPBatchBolt işlem bolt arabirimidir.

    public interface ISCPBatchBolt : ISCPPlugin           
    {
        void Execute(SCPTuple tuple);
        void FinishBatch(Dictionary<string, Object> parms);  
    }

`Execute()` gelen bolt yeni dizi olduğunda çağrılır. `FinishBatch()` Bu işlem sonlandırıldığında çağırılır. `parms` Giriş parametresi, gelecekte kullanılmak üzere ayrılmıştır.

İşlem topolojisi için önemli bir kavramdır – yoktur `StormTxAttempt`. İki alan vardır `TxId` ve `AttemptId`. `TxId` belirli bir işlemi tanımlamak için kullanılır ve belirli bir işlem olabilir birden fazla girişimde işlem başarısız olur ve yeniden yürütülmesi. SCP.NET oluşturur her işlem için yeni bir ISCPBatchBolt nesne `StormTxAttempt`, yalnızca Java dilinde Storm yaptığı gibi. Bu tasarım amacı, paralel işlemleri işleme desteklemektir. Kullanıcı göz önünde işlem girişimi tamamlandıysa, karşılık gelen ISCPBatchBolt nesne yok edilir ve çöp olarak toplanacak tutmanız gerekir.

## <a name="object-model"></a>Nesne modeli
SCP.NET, aynı zamanda anahtar nesneleri ile programlamayı geliştiriciler için basit bir dizi sağlar. Bunlar **bağlam**, **StateStore**, ve **SCPRuntime**. Bunlar, bu bölümün kalan kısmında ele alınmıştır.

### <a name="context"></a>Bağlam
Bağlamı uygulamaya çalışan bir ortamı sağlar. Her ISCPPlugin örneği (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) karşılık gelen bir bağlam örneği vardır. Bağlam tarafından sağlanan işlevselliği, iki bölüme ayrılabilir: (1) tüm C kullanılabilir statik bölümü\# işlemi, yalnızca belirli bir bağlam örneği için kullanılabilir (2) dinamik bölümü.

### <a name="static-part"></a>Statik bölümü
    public static ILogger Logger = null;
    public static SCPPluginType pluginType;                      
    public static Config Config { get; set; }                    
    public static TopologyContext TopologyContext { get; set; }  

`Logger` Günlük amaç için sağlanır.

`pluginType` C eklentisi türünü belirtmek için kullanılan\# işlem. C\# işlem (Java) olmadan yerel test modunda çalıştırın, eklenti türü `SCP_NET_LOCAL`.

    public enum SCPPluginType 
    {
        SCP_NET_LOCAL = 0,       
        SCP_NET_SPOUT = 1,       
        SCP_NET_BOLT = 2,        
        SCP_NET_TX_SPOUT = 3,   
        SCP_NET_BATCH_BOLT = 4  
    }

`Config` Java tarafında yapılandırma parametreleri almak için sağlanır. Parametre Java tarafında geçirilen zaman C\# eklentisi başlatılır. `Config` Parametreleri, iki bölüme ayrılmıştır: `stormConf` ve `pluginConf`.

    public Dictionary<string, Object> stormConf { get; set; }  
    public Dictionary<string, Object> pluginConf { get; set; }  

`stormConf` Storm tarafından tanımlanan parametreler ve `pluginConf` SCP tarafından tanımlanan parametreler. Örneğin:

    public class Constants
    {
        … …

        // constant string for pluginConf
        public static readonly String NONTRANSACTIONAL_ENABLE_ACK = "nontransactional.ack.enabled";  

        // constant string for stormConf
        public static readonly String STORM_ZOOKEEPER_SERVERS = "storm.zookeeper.servers";           
        public static readonly String STORM_ZOOKEEPER_PORT = "storm.zookeeper.port";                 
    }

`TopologyContext` sağlanan topolojisi bağlamı almak için birden fazla paralellik ile bileşenler için ekseriyetle faydalıdır. Örnek aşağıda verilmiştir:

    //demo how to get TopologyContext info
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)                      
    {
        Context.Logger.Info("TopologyContext info:");
        TopologyContext topologyContext = Context.TopologyContext;                    
        Context.Logger.Info("taskId: {0}", topologyContext.GetThisTaskId());          
        taskIndex = topologyContext.GetThisTaskIndex();
        Context.Logger.Info("taskIndex: {0}", taskIndex);
        string componentId = topologyContext.GetThisComponentId();                    
        Context.Logger.Info("componentId: {0}", componentId);
        List<int> componentTasks = topologyContext.GetComponentTasks(componentId);  
        Context.Logger.Info("taskNum: {0}", componentTasks.Count);                    
    }

### <a name="dynamic-part"></a>Dinamik bölümü
Aşağıdaki arabirimlerinden ilgili belirli bir bağlam örneği. Bağlam örneği SCP.NET platformu tarafından oluşturulur ve kullanıcı koduna geçen:

    // Declare the Output and Input Stream Schemas

    public void DeclareComponentSchema(ComponentStreamSchema schema);   

    // Emit tuple to default stream.
    public abstract void Emit(List<object> values);                   

    // Emit tuple to the specific stream.
    public abstract void Emit(string streamId, List<object> values);  

İşlem olmayan spout ACK desteklemek için aşağıdaki yöntemi sağlanır:

    // for non-transactional Spout which supports ack
    public abstract void Emit(string streamId, List<object> values, long seqId);  

İşlem olmayan bolt ACK desteklemek için açıkça olması gerektiği `Ack()` veya `Fail()` aldığı tanımlama grubu. Ve yeni bir tanımlama grubu yayma, ayrıca yer işaretlerini yeni kayıt düzeninin belirtmeniz gerekir. Aşağıdaki yöntemler sağlanır.

    public abstract void Emit(string streamId, IEnumerable<SCPTuple> anchors, List<object> values); 
    public abstract void Ack(SCPTuple tuple);
    public abstract void Fail(SCPTuple tuple);

### <a name="statestore"></a>StateStore
`StateStore` Meta Veri Hizmetleri, monoton bir sıra oluşturma ve bekleme sorunsuz koordinasyon sağlar. Üst düzey dağıtılmış eşzamanlılık soyutlamalar derlenebilir `StateStore`dağıtılmış kilitleri, dağıtılmış kuyrukları, önündeki engelleri ve işlem Hizmetleri dahil olmak üzere.

SCP uygulamaları kullanabilir `State` bazı bilgileri kalıcı hale getirmek için nesne [Apache ZooKeeper](https://zookeeper.apache.org/), özellikle işlem topolojisi için. Böylece işlem spout Kilitlenmeler ve yeniden başlatırsanız, ZooKeeper gerekli bilgileri almak ve işlem hattını yeniden yapılıyor.

`StateStore` Nesne çoğunlukla bu yöntemleri vardır:

    /// <summary>
    /// Static method to retrieve a state store of the given path and connStr 
    /// </summary>
    /// <param name="storePath">StateStore Path</param>
    /// <param name="connStr">StateStore Address</param>
    /// <returns>Instance of StateStore</returns>
    public static StateStore Get(string storePath, string connStr);

    /// <summary>
    /// Create a new state object in this state store instance
    /// </summary>
    /// <returns>State from StateStore</returns>
    public State Create();

    /// <summary>
    /// Retrieve all states that were previously uncommitted, excluding all aborted states 
    /// </summary>
    /// <returns>Uncommitted States</returns>
    public IEnumerable<State> GetUnCommitted();

    /// <summary>
    /// Get all the States in the StateStore
    /// </summary>
    /// <returns>All the States</returns>
    public IEnumerable<State> States();

    /// <summary>
    /// Get state or registry object
    /// </summary>
    /// <param name="info">Registry Name(Registry only)</param>
    /// <typeparam name="T">Type, Registry or State</typeparam>
    /// <returns>Return Registry or State</returns>
    public T Get<T>(string info = null);

    /// <summary>
    /// List all the committed states
    /// </summary>
    /// <returns>Registries contain the Committed State </returns> 
    public IEnumerable<Registry> Committed();

    /// <summary>
    /// List all the Aborted State in the StateStore
    /// </summary>
    /// <returns>Registries contain the Aborted State</returns>
    public IEnumerable<Registry> Aborted();

    /// <summary>
    /// Retrieve an existing state object from this state store instance 
    /// </summary>
    /// <returns>State from StateStore</returns>
    /// <typeparam name="T">stateId, id of the State</typeparam>
    public State GetState(long stateId)

`State` Nesne çoğunlukla bu yöntemleri vardır:

    /// <summary>
    /// Set the status of the state object to commit 
    /// </summary>
    public void Commit(bool simpleMode = true); 

    /// <summary>
    /// Set the status of the state object to abort 
    /// </summary>
    public void Abort();

    /// <summary>
    /// Put an attribute value under the give key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <param name="attribute">State Attribute</param> 
    public void PutAttribute<T>(string key, T attribute); 

    /// <summary>
    /// Get the attribute value associated with the given key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <returns>State Attribute</returns>               
    public T GetAttribute<T>(string key);                    

İçin `Commit()` simpleMode ayarlandığında yöntemi true, ZooKeeper, karşılık gelen ZNode siler. Aksi takdirde, geçerli ZNode ve yeni bir düğüm kabul edilen ekleme siler\_yolu.

### <a name="scpruntime"></a>SCPRuntime
Aşağıdaki iki yöntemden SCPRuntime sağlar:

    public static void Initialize();

    public static void LaunchPlugin(newSCPPlugin createDelegate);  

`Initialize()` SCP çalışma zamanı ortamı başlatmak için kullanılır. Bu yöntemde, C\# işlemi için Java tarafı bağlanır ve yapılandırma parametreleri ve topoloji bağlamını alır.

`LaunchPlugin()` devre dışı ileti işleme döngüsü başlatmak üzere kullanılır. Bu döngüye C\# eklentisi iletileri form Java yan (diziler ve denetim sinyalleri dahil) alır ve ardından işlem belki de arabirim yöntemini çağırarak iletileri, kullanıcı kodu tarafından sağlayın. Yöntemi giriş parametresi `LaunchPlugin()` ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt arabirimini uygulayan bir nesne döndüren bir temsilci.

    public delegate ISCPPlugin newSCPPlugin(Context ctx, Dictionary\<string, Object\> parms); 

ISCPBatchBolt için aldığımız `StormTxAttempt` gelen `parms`ve onu yeniden yürütülmüş bir girişimi olup olmadığını değerlendirmek için kullanabilirsiniz. Yeniden yürütme girişimi için onay işleme bolt sık gerçekleştirilir ve örneklerde gösterildiği `HelloWorldTx` örnek.

Genel olarak bakıldığında, SCP eklentileri, burada iki modda çalışabilir:

1. Yerel Test modu: Bu modda SCP eklentileri (C\# kullanıcı kodu) geliştirme aşamasında Visual Studio içinde çalıştırın. `LocalContext` Bu modda, yerel dosyalara yayılan diziler seri hale getirmek ve bunları geri bellek okumak için yöntem sağlar kullanılabilir.
   
        public interface ILocalContext
        {
            List\<SCPTuple\> RecvFromMsgQueue();
            void WriteMsgQueueToFile(string filepath, bool append = false);  
            void ReadFromFileToMsgQueue(string filepath);                    
        }
2. Normal mod: Bu modda SCP eklentileri storm java işlemi tarafından başlatılır.
   
    SCP eklentisi başlatılırken bir örnek aşağıda verilmiştir:
   
        namespace Scp.App.HelloWorld
        {
        public class Generator : ISCPSpout
        {
            … …
            public static Generator Get(Context ctx, Dictionary<string, Object> parms)
            {
            return new Generator(ctx);
            }
        }
   
        class HelloWorld
        {
            static void Main(string[] args)
            {
            /* Setting the environment variable here can change the log file name */
            System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "HelloWorld");
   
            SCPRuntime.Initialize();
            SCPRuntime.LaunchPlugin(new newSCPPlugin(Generator.Get));
            }
        }
        }

## <a name="topology-specification-language"></a>Topoloji belirtim dili
SCP topolojisi açıklayan ve SCP topolojisini yapılandırmak için bir etki alanına özgü dil özelliğidir. Storm'ın Clojure DSL üzerinde bağlıdır (<https://storm.incubator.apache.org/documentation/Clojure-DSL.html>) ve SCP tarafından genişletilir.

Topoloji belirtimleri aracılığıyla yürütme için storm kümesine doğrudan gönderilebilir ***runspec*** komutu.

SCP.NET işlem topolojileri tanımlamak için aşağıdaki işlevler eklemiştir:

| **Yeni işlevleri** | **Parametreler** | **Açıklama** |
| --- | --- | --- |
| **Tx topolopy** |Topoloji adı<br />spout eşleme<br />bolt eşleme |Topoloji adı ile bir işlem topolojisi tanımlayın &nbsp;tanımı Haritası ve bolt'lar tanımı harita spout'lar |
| **SCP tx spout** |Exec-name<br />args<br />Alanları |Bir işlem spout tanımlayın. Uygulama ile çalıştığı ***exec-name*** kullanarak ***args***.<br /><br />***Alanları*** spout için çıkış alanlar |
| **tx batch bolt SCP** |Exec-name<br />args<br />Alanları |Bir işlem toplu Bolt tanımlayın. Uygulama ile çalıştığı ***exec-name*** kullanarak ***args.***<br /><br />Bolt çıktı alanlarını alanların olur. |
| **tx işleme bolt SCP** |Exec-name<br />args<br />Alanları |Bir işlem tabanlı işleme bolt tanımlayın. Uygulama ile çalıştığı ***exec-name*** kullanarak ***args***.<br /><br />***Alanları*** bolt için çıkış alanlar |
| **nontx topolopy** |Topoloji adı<br />spout eşleme<br />bolt eşleme |Topoloji adı ile bir işlem topolojisi tanımlayın&nbsp; tanımı Haritası ve bolt'lar tanımı harita spout'lar |
| **scp-spout** |Exec-name<br />args<br />Alanları<br />parametreler |Bir işlem spout tanımlayın. Uygulama ile çalıştığı ***exec-name*** kullanarak ***args***.<br /><br />***Alanları*** spout için çıkış alanlar<br /><br />***Parametreleri*** "nontransactional.ack.enabled" gibi bazı parametreler belirtmek için kullanarak, isteğe bağlıdır. |
| **SCP bolt** |Exec-name<br />args<br />Alanları<br />parametreler |İşleme uygun olmayan Bolt tanımlayın. Uygulama ile çalıştığı ***exec-name*** kullanarak ***args***.<br /><br />***Alanları*** bolt için çıkış alanlar<br /><br />***Parametreleri*** "nontransactional.ack.enabled" gibi bazı parametreler belirtmek için kullanarak, isteğe bağlıdır. |

SCP.NET tanımlanan aşağıdaki anahtar sözcükler vardır:

| **anahtar sözcükler** | **Açıklama** |
| --- | --- |
| **: ad** |Topoloji adı tanımlayın |
| **: topolojisi** |Önceki işlevlerini kullanarak topolojisi tanımlayın ve olanları içinde oluşturun. |
| **: p** |Her spout veya Cıvata için paralellik ipucu tanımlayın. |
| **:config** |Tanımlama parametresini yapılandırabilir veya var olanları güncelleştir |
| **:schema** |Stream şemasını tanımlar. |

Ve sık kullanılan parametreler:

| **Parametre** | **Açıklama** |
| --- | --- |
| **"plugin.name"** |C# eklentisi exe dosyası adı |
| **"plugin.args"** |Eklenti bağımsız değişkenleri |
| **"output.schema"** |Çıkış şeması |
| **"nontransactional.ack.enabled"** |Ack işlem topolojisi için etkinleştirilip etkinleştirilmediğini gösterir |

BITS ile birlikte dağıtılan runspec komutu, kullanım gibidir:

    .\bin\runSpec.cmd
    usage: runSpec [spec-file target-dir [resource-dir] [-cp classpath]]
    ex: runSpec examples\HelloWorld\HelloWorld.spec specs examples\HelloWorld\Target

***Kaynak dizini*** parametresi isteğe bağlı ise, bir C takın istediğinizde bunu belirtmek gereken\# uygulama ve bu dizin uygulama, bağımlılıklar ve yapılandırmaları içerir.

***Sınıf*** parametresi isteğe bağlıdır, ayrıca. Java Spout veya Cıvata belirtim dosyasını içeriyorsa, Java sınıf yolu belirtmek için kullanılır.

## <a name="miscellaneous-features"></a>Çeşitli özellikler
### <a name="input-and-output-schema-declaration"></a>Giriş ve çıkış şema bildirimi
Kullanıcılar C'de tanımlama gruplarına yayabilir\# işlemleri, platform gerekiyor tanımlama grubu, byte [] ile seri hale getirmek Java tarafa aktarmak ve Storm bu demet transfer edeceğini hedeflerini. Bu arada, aşağı akış bileşenleri C içindeki\# işlemleri java taraftan dizilerini alır ve platforma göre özgün türlerine dönüştürme, tüm bu işlemler, Platform tarafından gizlenir.

Serileştirme ve seri durumundan çıkarma desteklemek için girdileri ve çıktıları şemasını bildirmek kullanıcı kodu gerekir.

Giriş/Çıkış akış şeması bir sözlük olarak tanımlanır. Streamıd anahtardır. Sütun türlerini değerdir. Bileşen bildirilen çok akışları sahip olabilir.

    public class ComponentStreamSchema
    {
        public Dictionary<string, List<Type>> InputStreamSchema { get; set; }
        public Dictionary<string, List<Type>> OutputStreamSchema { get; set; }
        public ComponentStreamSchema(Dictionary<string, List<Type>> input, Dictionary<string, List<Type>> output)
        {
            InputStreamSchema = input;
            OutputStreamSchema = output;
        }
    }


Bağlam nesnesi içinde eklenen aşağıdaki API'yi sunuyoruz:

    public void DeclareComponentSchema(ComponentStreamSchema schema)

Geliştiriciler bu akış için tanımlanan şema yayılan tanımlama grubu uyma, aksi takdirde, sistemin bir çalışma zamanı özel durum oluşturur emin olmalısınız.

### <a name="multi-stream-support"></a>Birden çok akış desteği
SCP yayma veya aynı anda birden çok farklı akışlardan almak üzere kullanıcı kodunun destekler. Emit yöntemi bir isteğe bağlı Akış kimliği parametresi yararlanırken bağlam nesnesinde destek yansıtır.

SCP.NET bağlam nesnesi içindeki iki yöntem sürümüne eklenmiştir. Bunlar, tanımlama grubu veya diziler Streamıd belirtmek için yaymak için kullanılır. Streamıd bir dizedir ve hem C'de tutarlı olması gereken\# ve topoloji tanımı belirtimi.

        /* Emit tuple to the specific stream. */
        public abstract void Emit(string streamId, List<object> values);

        /* for non-transactional Spout only */
        public abstract void Emit(string streamId, List<object> values, long seqId);

Var olmayan bir akışa yayma, çalışma zamanı özel durumları neden olur.

### <a name="fields-grouping"></a>Alan gruplandırma
Storm içindeki yerleşik alanların gruplandırma SCP.NET düzgün şekilde çalışmıyor. Java Ara sunucu tarafında tüm alanların veri türleri: gerçekten byte [] ve gruplandırma alanları gruplandırma gerçekleştirmek için bayt [] nesnenin karma kodunu kullanır. Bayt [] nesnesi karma kodu bu nesneyi bellek içinde adresidir. Bu nedenle gruplandırma aynı adresi değil, ancak aynı içerik paylaşmak için iki bayt [] nesneleri yanlış olur.

SCP.NET özelleştirilmiş gruplandırma yöntemini ekler ve bayt [] içeriğini gruplandırma yapmak için kullanır. İçinde **SPEC** dosyası sözdizimi benzer:

    (bolt-spec
        {
            "spout_test" (scp-field-group :non-tx [0,1])
        }
        …
    )


Burada,

1. "scp-alanı-group", "SCP tarafından uygulanan özelleştirilmiş alan gruplandırma" anlamına gelir.
2. ": tx"veya": tx olmayan" işlem topolojisi olduğu anlamına gelir. Başlangıç dizini tx olmayan topolojileri tx farklı olduğundan bu bilgiye ihtiyacımız var.
3. [0,1] alan kimlikleri, 0'dan başlayan bir karma kümesini belirtir.

### <a name="hybrid-topology"></a>Karma bir topolojide
Yerel bir Storm Java dilinde yazılır. SCP.NET C etkinleştirmek için Gelişmiş ve\# geliştiricilerin C yazma\# kendi iş mantığı işlemek için kod. Ancak yalnızca C içeren karma topolojiler de destekler\# spout/Cıvatalar, aynı zamanda Java Spout/Cıvatalar.

### <a name="specify-java-spoutbolt-in-spec-file"></a>Java Spout/Cıvata belirtim dosyasını belirtin.
Belirtim dosyasındaki "spout scp" ve "bolt scp" de Java Spout'lar ve Bolt'lar belirtmek için kullanılabilir, bir örnek aşağıda verilmiştir:

    (spout-spec 
      (microsoft.scp.example.HybridTopology.Generator.)           
      :p 1)

Burada `microsoft.scp.example.HybridTopology.Generator` Java Spout sınıf adıdır.

### <a name="specify-java-classpath-in-runspec-command"></a>Komut runSpec içinde Java sınıf yolu belirtin
Java Spout'lar veya Cıvatalar içeren topolojisi göndermek istiyorsanız, ilk Java Spout'lar veya Cıvatalar derleyin ve Jar dosyaları almak gerekir. Topoloji gönderirken Jar dosyalarını içeren java sınıf yolu belirtmeniz gerekir. Örnek aşağıda verilmiştir:

    bin\runSpec.cmd examples\HybridTopology\HybridTopology.spec specs examples\HybridTopology\net\Target -cp examples\HybridTopology\java\target\*

Burada **örnekler\\HybridTopology\\java\\hedef\\**  Spout/Cıvata Java Jar dosyasını içeren klasördür.

### <a name="serialization-and-deserialization-between-java-and-c"></a>Serileştirme ve seri durumundan çıkarma Java ve C arasındaki\#
SCP bileşeni içerir tarafı Java ve C\# yan. Yerel Java Spout/Bolt ile etkileşim kurmak için serileştirme/seri durumundan çıkarma tarafı Java ve C gerçekleştirilmesi gerekir\# yan aşağıdaki grafikte gösterildiği gibi.

![Java bileşenine göndermeden SCP bileşenine göndermeden java bileşen diyagramı](./media/apache-storm-scp-programming-guide/java-compent-sending-to-scp-component-sending-to-java-component.png)

1. **Java yan ve seri durumundan çıkarma C'de serileştirme\# yan**
   
   İlk Java tarafındaki serileştirme ve seri durumundan çıkarma c için varsayılan uygulama sağladığımız\# yan. Java tarafındaki seri duruma getirme yöntemi, özel dosyasında belirtilebilir:
   
       (scp-bolt
           {
               "plugin.name" "HybridTopology.exe"
               "plugin.args" ["displayer"]
               "output.schema" {}
               "customized.java.serializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer"]
           })
   
   C seri durumdan çıkarma yöntemini\# yan C'de belirtilmelidir\# kullanıcı kodu:
   
       Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
       inputSchema.Add("default", new List<Type>() { typeof(Person) });
       this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
       this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());            
   
   Veri türü karmaşık değil sağlanan çoğu durumda bu varsayılan uygulama işlemelidir. Bazı durumlarda, kullanıcı veri türünün çok karmaşık olduğu için veya varsayılan kararlılığımızın performansını, kendi uygulama kullanıcıların can eklenti kullanıcının gereksinimi karşılamadığından.
   
   Java yan serileştirme arabiriminde olarak tanımlanır:
   
       public interface ICustomizedInteropJavaSerializer {
           public void prepare(String[] args);
           public List<ByteBuffer> serialize(List<Object> objectList);
       }
   
   C deserialize arabiriminde\# yan olarak tanımlanır:
   
   Ortak arabirim ICustomizedInteropCSharpDeserializer
   
       public interface ICustomizedInteropCSharpDeserializer
       {
           List<Object> Deserialize(List<byte[]> dataList, List<Type> targetTypes);
       }
2. **C'de serileştirme\# yan ve Java tarafındaki seri durumundan çıkarma**
   
   C'de seri duruma getirme yöntemi\# yan C'de belirtilmelidir\# kullanıcı kodu:
   
       this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer()); 
   
   Java yan seri durumundan çıkarma yöntemi SPEC dosyasında belirtilmesi:
   
     (scp spout
   
       {
         "plugin.name" "HybridTopology.exe"
         "plugin.args" ["generator"]
         "output.schema" {"default" ["person"]}
         "customized.java.deserializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" "microsoft.scp.example.HybridTopology.Person"]
       })
   
   Burada "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" seri durumdan Çıkarıcının adını ve "microsoft.scp.example.HybridTopology.Person" hedef sınıf verileri seri durumdan.
   
   Kullanıcı aynı zamanda kendi C uygulamasında takın\# seri hale getirici ve Java seri durumdan Çıkarıcının. Bu kod C arabirimidir\# seri hale getirici:
   
       public interface ICustomizedInteropCSharpSerializer
       {
           List<byte[]> Serialize(List<object> dataList);
       }
   
   Bu kod Java seri durumdan Çıkarıcının arabirimidir:
   
       public interface ICustomizedInteropJavaDeserializer {
           public void prepare(String[] targetClassNames);
           public List<Object> Deserialize(List<ByteBuffer> dataList);
       }

## <a name="scp-host-mode"></a>SCP konak modu
Bu modda, kullanıcı dll kodlarını derlemek ve SCP tarafından sağlanan SCPHost.exe topolojisi göndermek için kullanın. Belirtim dosyası şu kod gibi görünür:

    (scp-spout
      {
        "plugin.name" "SCPHost.exe"
        "plugin.args" ["HelloWorld.dll" "Scp.App.HelloWorld.Generator" "Get"]
        "output.schema" {"default" ["sentence"]}
      })

Burada, `plugin.name` olarak belirtilen `SCPHost.exe` SCP SDK'sı tarafından sağlanan. SCPHost.exe üç parametre kabul eder:

1. DLL adı ilk sağlayıcıdır `"HelloWorld.dll"` Bu örnekte.
2. İkinci bir sınıf adı olduğunu `"Scp.App.HelloWorld.Generator"` Bu örnekte.
3. Üçüncü ISCPPlugin örneğini almak için çağrılabilir ortak statik metodun adıdır.

Konak modunda, kullanıcı kodu DLL olarak derlenir ve SCP platformu tarafından çağrılır. Bu nedenle SCP platform, tüm işleme mantığı üzerinde tam denetim elde edebilirsiniz. Bu nedenle müşterilerimizin geliştirme deneyimini basitleştirmek ve bize daha fazla esneklik ve daha iyi geriye dönük uyumluluk de sonraki sürüm Getir SCP konak modunda topoloji Gönder öneririz.

## <a name="scp-programming-examples"></a>SCP programlama örnekleri
### <a name="helloworld"></a>HelloWorld
**HelloWorld** SCP.NET denemek göstermek için basit bir örnektir. Adlı bir spout ile bir işlem olmayan topolojisi kullanan **Oluşturucu**ve adlı iki Cıvatalar **Bölümlendirici** ve **sayaç**. Spout **Oluşturucu** rastgele cümleler oluşturur ve bu cümlelere yayma **Bölümlendirici**. Bolt **Bölümlendirici** cümlelere sözcükleri böler ve bu sözcükler için yayma **sayacı** bolt. Bolt "sayaç", her bir sözcüğün geçtiği sayısını kaydetmek için bir sözlük kullanır.

İki özel dosyalar **HelloWorld.spec** ve **HelloWorld\_EnableAck.spec** bu örneğin. C'de\# kod, etkin olup olmadığını ack Java taraftan pluginConf alarak kullanıma bulabildiği.

    /* demo how to get pluginConf info */
    if (Context.Config.pluginConf.ContainsKey(Constants.NONTRANSACTIONAL_ENABLE_ACK))
    {
        enableAck = (bool)(Context.Config.pluginConf[Constants.NONTRANSACTIONAL_ENABLE_ACK]);
    }
    Context.Logger.Info("enableAck: {0}", enableAck);

ACK etkinleştirilirse, spout içinde onaylanmadıkları diziler önbelleğe almak için bir sözlük kullanılır. Fail() çağrılırsa, başarısız demet tekrarlanır:

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        Context.Logger.Info("Fail, seqId: {0}", seqId);
        if (cachedTuples.ContainsKey(seqId))
        {
            /* get the cached tuple */
            string sentence = cachedTuples[seqId];

            /* replay the failed tuple */
            Context.Logger.Info("Re-Emit: {0}, seqId: {1}", sentence, seqId);
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), seqId);
        }
        else
        {
            Context.Logger.Warn("Fail(), can't find cached tuple for seqId {0}!", seqId);
        }
    }

### <a name="helloworldtx"></a>HelloWorldTx
**HelloWorldTx** örnek işlem topolojiyi uygulamak nasıl gösterir. Adlı bir spout sahip **Oluşturucu**, batch bolt adlı **kısmi-count**, ve işleme bolt adlı **sayısı toplam**. Üç önceden oluşturulmuş txt dosyaları vardır: **DataSource0.txt**, **DataSource1.txt**, ve **DataSource2.txt**.

Spout her işlemde **Oluşturucu** rastgele önceden oluşturulmuş üç dosyalarından iki dosya seçer ve iki dosya adlarını yayma **kısmi-count** bolt. Bolt **kısmi-count** dosya ad alınan tanımlama grubundan sonra dosyasını açın ve bu dosyadaki sözcük sayısını ve sözcük sayıya son yayma alır **sayısı toplam** bolt. **Sayısı toplam** bolt toplam sayısını özetler.

Elde etmek için **tam bir kez** semantiği, işleme bolt **sayısı toplam** yeniden yürütülmüş bir işlem olup olmadığını değerlendirmek için gerekir. Bu örnekte, bir statik üye değişkeni sahiptir:

    public static long lastCommittedTxId = -1; 

ISCPBatchBolt örneği oluşturulduğunda alır `txAttempt` Giriş parametrelerinden:

    public static CountSum Get(Context ctx, Dictionary<string, Object> parms)
    {
        /* for transactional topology, we can get txAttempt from the input parms */
        if (parms.ContainsKey(Constants.STORM_TX_ATTEMPT))
        {
            StormTxAttempt txAttempt = (StormTxAttempt)parms[Constants.STORM_TX_ATTEMPT];
            return new CountSum(ctx, txAttempt);
        }
        else
        {
            throw new Exception("null txAttempt");
        }
    }

Zaman `FinishBatch()` çağrıldığında `lastCommittedTxId` yeniden yürütülmüş bir işlem değilse güncelleştirilir.

    public void FinishBatch(Dictionary<string, Object> parms)
    {
        /* judge whether it is a replayed transaction? */
        bool replay = (this.txAttempt.TxId <= lastCommittedTxId);

        if (!replay)
        {
            /* If it is not replayed, update the totalCount and lastCommittedTxId value */
            totalCount = totalCount + this.count;
            lastCommittedTxId = this.txAttempt.TxId;
        }
        … …
    }


### <a name="hybridtopology"></a>HybridTopology
Bu topoloji, bir Java Spout ve bir C içeren\# Bolt. SCP platform tarafından sağlanan varsayılan seri hale getirme ve seri durumundan çıkarma uygulamasını kullanır. Bkz **HybridTopology.spec** içinde **örnekler\\HybridTopology** belirtim dosyası Ayrıntılar için klasör ve **SubmitTopology.bat** nasıl Java belirtmek için sınıf.

### <a name="scphostdemo"></a>SCPHostDemo
Bu örnekte HelloWorld temelde aynıdır. Tek fark, kullanıcı kodu DLL olarak derlenir ve SCPHost.exe kullanarak topoloji gönderilen ' dir. Daha ayrıntılı açıklama "SCP konak modu" bölümüne bakın.

## <a name="next-steps"></a>Sonraki Adımlar
Apache Storm topolojilerini SCP kullanılarak oluşturulan örnekleri için aşağıdaki belgelere bakın:

* [Visual Studio kullanarak HDInsight üzerinde Apache Storm için C# topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md)
* [HDInsight üzerinde Apache Storm ile Azure Event hubs'dan olayları işleme](apache-storm-develop-csharp-event-hub-topology.md)
* [HDInsight üzerinde Apache Storm kullanarak Event hubs'tan vehicle sensör verisi işleme](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/IotExample)
* [Ayıklama, dönüştürme ve Apache HBase için Azure Event Hubs'dan yükleme (ETL)](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample)
