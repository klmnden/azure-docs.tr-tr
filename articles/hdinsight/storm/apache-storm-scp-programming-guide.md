---
title: SCP.NET Programlama Kılavuzu | Microsoft Docs
description: SCP.NET oluşturmak için nasıl kullanılacağını öğrenin. Hdınsight üzerinde Storm ile NET tabanlı Storm Topolojileri için kullanın.
services: hdinsight
documentationcenter: ''
author: raviperi
manager: jhubbard
editor: cgronlun
ms.assetid: 34192ed0-b1d1-4cf7-a3d4-5466301cf307
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.date: 05/16/2016
ms.author: raviperi
ms.openlocfilehash: 0f4c021bc209c99e1b3f34b34bf5ba0549eb48f9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31421564"
---
# <a name="scp-programming-guide"></a>SCP Programlama Kılavuzu
SCP gerçek zamanlı, güvenilir ve tutarlı, oluşturmak için platform ve yüksek performanslı veri işleme uygulama ' dir. Üstünde oluşturulmuş [Apache Storm](http://storm.incubator.apache.org/) --bir akış tarafından OSS toplulukları tasarlanmış sistemi işleme. Açık Twitter tarafından kaynaklıdır ve Storm Nathan Marz tarafından tasarlanmıştır. Bunu yararlanır [Apache ZooKeeper](http://zookeeper.apache.org/), yüksek oranda güvenilir etkinleştirmek için başka bir Apache proje dağıtılmış eşgüdümü ve durum yönetimi. 

Yalnızca Windows üzerinde Storm SCP proje bağlantı noktası kurulmuş ancak proje uzantıları ve özelleştirme Windows ekosistemi için de eklenir. Uzantıları .NET geliştirme deneyimi ve kitaplıklarını içerir, Windows tabanlı bir dağıtım özelleştirme içerir. 

Genişletme ve özelleştirme biz OSS projeleri çatallaştırma gerekmez ve Storm üstünde oluşturulmuş türetilmiş ekosistemlerini yararlanan bir şekilde yapılır.

## <a name="processing-model"></a>İşlem modeli
SCP verilerde diziler sürekli akışları olarak modellenir. Genellikle başlıklar bazı sıraya ilk akış sonra toplanmış ve içindeki bir Storm topolojisinin barındırılan iş mantığı tarafından dönüştürülen, son çıktı başka bir SCP sistemine diziler olarak yöneltilen veya dağıtılmış dosya sistemi veya veritabanları gibi depoları için önem SQL Server gibi.

![Bir veri deposu akışları işleme veri besleme bir sıra diyagramı](./media/apache-storm-scp-programming-guide/queue-feeding-data-to-processing-to-data-store.png)

Storm bir uygulama topolojisi hesaplama grafiği tanımlar. Veri akışı düğümler arasındaki bağlantıları gösterir ve her düğüm topolojisinde işleme mantığı içerir. Topoloji giriş verileri eklemesine düğümler olarak adlandırılır _spout'lar_, verileri sıralamak için kullanılabilecek. Giriş verisi dosya günlükleri, işlemsel veritabanı, sistem performans sayacı vb. bulunması. Her iki girdi ve çıktı veri akışları ile düğümler olarak adlandırılır _Cıvatalar_, gerçek verileri filtreleme yapabilir ve seçimleri ve toplama.

SCP en iyi çaba, en az bir kere destekler ve tam olarak-kez veri işleme. Bir dağıtılmış akış işleme uygulamasında, Ağ kaybı, makine hatasını ya da kullanıcı kodu hatası vb. gibi veri işleme sırasında çeşitli hatalar oluşabilir. En az bir kere işleme tüm veriler hata olduğunda otomatik olarak aynı verileri yeniden oynatmak tarafından en az bir kez işlenir sağlar. En az bir kere işleme basit ve güvenilir ve uygun iyi birçok uygulama. Ancak, uygulama tam sayım gerektirdiğinde, aynı veri olası uygulama topolojisinde oynanan bu yana en az bir kere işleme yeterli değil. Bu durumda, tam olarak-bile ne zaman veri yeniden ve birden çok kez işlenen işlem sonucu emin olmak için tasarlanmış bir kez doğru olduğundan.

SCP üzerinde Java sanal makine (JVM) Storm perde arkasında yararlanarak sırasında gerçek zamanlı veri işlem uygulamaları geliştirmek .NET geliştiricilerinin sağlar. .NET ve JVM TCP yerel yuva iletişim kurar. Neredeyse her Spout/Cıvata bir .net/Java işlemi, kullanıcı mantığını .net işlem bir eklenti olarak çalıştığı çiftidir.

SCP en üstünde bir veri işleme uygulaması oluşturmak için birkaç adım gerekir:

* Veri sırasından çekmesini Spout'lar tasarlayıp yeniden açın.
* Tasarım ve girdi verilerini işlemek için Cıvatalar uygulamak ve bir veritabanı gibi dış depolarına veri kaydedin.
* Topoloji tasarım, gönderme ve topoloji çalıştırın. Topoloji köşeleri ve verileri tanımlayan köşeleri arasında akışları. SCP topoloji belirtimi alın ve her köşe mantıksal bir düğüm üzerinde çalıştığı bir Storm kümede dağıtın. Yük devretme ve Storm Görev Zamanlayıcı'yı dikkate ölçeklendirme sağlar.

Bu belgede bazı basit örnekler SCP ile veri işleme uygulamasının nasıl oluşturulacağını size rehberlik için kullanır.

## <a name="scp-plugin-interface"></a>SCP eklentisi arabirimi
SCP eklentileri (veya uygulamalar) hem de Visual Studio içinde geliştirme aşamasında çalışabilen ve Storm ardışık düzenine üretim dağıtım sonrasında takılı tek başına exe markalarıdır. SCP eklentisi yazma tıpkı diğer standart Windows konsol uygulamaları yazma olarak olur. Spout/Cıvata için bazı arabirimi SCP.NET platform bildirir ve kullanıcı eklenti kodu bu arabirimi uygulamalıdır. Bu tasarım ana amacı, kullanıcının kendi iş logics ve SCP.NET platformu tarafından işlenecek başka şeyler bırakarak odaklanabilirsiniz.

Kullanıcı eklentisi kodu aşağıdakilere arabirimlerinden biri, uygulamalıdır, topoloji işlem ya da işlem dışı olmasına ve bileşen spout veya Cıvata olmasına bağlıdır.

* ISCPSpout
* ISCPBolt
* ISCPTxSpout
* ISCPBatchBolt

### <a name="iscpplugin"></a>ISCPPlugin
ISCPPlugin eklentileri her türlü ortak arabirimidir. Şu anda, onu bir kukla arabirimidir.

    public interface ISCPPlugin 
    {
    }

### <a name="iscpspout"></a>ISCPSpout
ISCPSpout işlemsel olmayan spout arabirimidir.

     public interface ISCPSpout : ISCPPlugin                    
     {
         void NextTuple(Dictionary<string, Object> parms);         
         void Ack(long seqId, Dictionary<string, Object> parms);   
         void Fail(long seqId, Dictionary<string, Object> parms);  
     }

Zaman `NextTuple()` çağrılır, C\# kullanıcı kodu bir veya daha fazla tanımlama grubu yayma. Varsa yayma bir şey yok, bu yöntem herhangi bir şey yayma olmadan döndürmelidir. Dikkat edilmesi gereken, `NextTuple()`, `Ack()`, ve `Fail()` tümü tek bir iş parçacığı c sıkı döngü denir\# işlemi. Hiçbir tanımlama grubu yayma olduğunda, bu nedenle çok fazla CPU boşa harcanmasına değil olarak için kısa bir süre (örneğin, 10 milisaniye) için NextTuple uyku olması courteous.

`Ack()` ve `Fail()` ack mekanizması belirtim dosyasında yalnızca etkin olarak adlandırılır. `seqId` Acked veya başarısız oldu başlığı tanımlamak için kullanılır. Bu nedenle ACK işlemsel olmayan topolojisinde etkinleştirilirse, aşağıdaki emit işlevi içinde Spout kullanılmalıdır:

    public abstract void Emit(string streamId, List<object> values, long seqId); 

ACK işlemsel olmayan topolojisinde desteklenmiyorsa `Ack()` ve `Fail()` boş işlev olarak bırakılabilir.

`parms` Bu işlevler giriş parametresi boş bir sözlük, ileride kullanılmak üzere ayrılmış.

### <a name="iscpbolt"></a>ISCPBolt
ISCPBolt işlemsel olmayan Cıvata arabirimidir.

    public interface ISCPBolt : ISCPPlugin 
    {
    void Execute(SCPTuple tuple);           
    }

Yeni tanımlama grubu mevcut olduğunda `Execute()` işlevi işlemek üzere çağrılır.

### <a name="iscptxspout"></a>ISCPTxSpout
ISCPTxSpout işlem spout arabirimidir.

    public interface ISCPTxSpout : ISCPPlugin
    {
        void NextTx(out long seqId, Dictionary<string, Object> parms);  
        void Ack(long seqId, Dictionary<string, Object> parms);         
        void Fail(long seqId, Dictionary<string, Object> parms);        
    }

İşlem olmayan karşı erişmelerini'olduğu gibi `NextTx()`, `Ack()`, ve `Fail()` tümü tek bir iş parçacığı c sıkı döngü denir\# işlemi. Hiçbir veri yayma olduğunda sahip courteous `NextTx` uyku için kısa bir nedenle çok fazla CPU boşa harcanmasına değil olarak süresi (10 milisaniye).

`NextTx()` out parametresi yeni bir işlem başlatmaya adlı `seqId` ayrıca kullanılır işlem tanımlamak için kullanılan `Ack()` ve `Fail()`. İçinde `NextTx()`, kullanıcı veri Java dışarıdan yayma. Verileri yeniden yürütme desteklemek için ZooKeeper içinde depolanır. ZooKeeper kapasitesi sınırlı olduğundan, bir kullanıcı yalnızca meta veri yayma, işlem spout verileri toplu değil.

Storm yeniden yürütme bir işlem otomatik olarak başarısız olursa, bunu `Fail()` normal durumda çağrılmamalıdır. Ancak SCP işlem spout'un yayılan meta verileri işaretlerseniz çağırabilirsiniz `Fail()` zaman meta veriler geçerli değil.

`parms` Bu işlevler giriş parametresi boş bir sözlük, ileride kullanılmak üzere ayrılmış.

### <a name="iscpbatchbolt"></a>ISCPBatchBolt
ISCPBatchBolt işlem Cıvata arabirimidir.

    public interface ISCPBatchBolt : ISCPPlugin           
    {
        void Execute(SCPTuple tuple);
        void FinishBatch(Dictionary<string, Object> parms);  
    }

`Execute()` Yeni kayıt sırasında Cıvata ulaşan olduğunda çağrılır. `FinishBatch()` Bu işlem sona erdikten sonra çağrılır. `parms` Giriş parametresi, gelecekte kullanılmak üzere ayrılmıştır.

İşlem topolojisi için önemli bir kavram – olduğundan `StormTxAttempt`. İki alanı olan `TxId` ve `AttemptId`. `TxId` belirli bir işlemi tanımlamak için kullanılır ve belirli bir işlem için birden çok deneme işlemi başarısız olur ve durumunda olabilir yeniden. SCP.NET her işlemek için yeni bir ISCPBatchBolt nesnesi oluşturur `StormTxAttempt`, yalnızca Storm Java yaptığı gibi. Bu tasarım amacı, paralel işlemleri işleme desteklemektir. Kullanıcı işlem girişimi tamamlandıysa, karşılık gelen ISCPBatchBolt nesnesi yok unutmayın ve toplanacak tutmanız gerekir.

## <a name="object-model"></a>Nesne modeli
SCP.NET anahtar nesneleri ile program geliştiriciler için basit bir dizi de sağlar. Bunlar **bağlamı**, **StateStore**, ve **SCPRuntime**. Bunlar, bu bölümde rest bölümünde ele alınmıştır.

### <a name="context"></a>Bağlam
Bağlamı uygulamaya çalışan bir ortam sağlar. Her ISCPPlugin örneğinin (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) karşılık gelen bir bağlam örneği vardır. Bağlam tarafından sağlanan işlevleri iki bölüme ayrılabilir: tüm C kullanılabilir (1 statik bölümü\# işlem, yalnızca belirli bağlam örneği için kullanılabilir (2) dinamik bölümü.

### <a name="static-part"></a>Statik bölümü
    public static ILogger Logger = null;
    public static SCPPluginType pluginType;                      
    public static Config Config { get; set; }                    
    public static TopologyContext TopologyContext { get; set; }  

`Logger` Günlük amaçla sağlanır.

`pluginType` C eklentisi türünü belirtmek için kullanılan\# işlemi. Varsa C\# işlemi (olmadan Java) yerel test modda çalıştırılır, eklenti tür `SCP_NET_LOCAL`.

    public enum SCPPluginType 
    {
        SCP_NET_LOCAL = 0,       
        SCP_NET_SPOUT = 1,       
        SCP_NET_BOLT = 2,        
        SCP_NET_TX_SPOUT = 3,   
        SCP_NET_BATCH_BOLT = 4  
    }

`Config` Java taraftan yapılandırma parametreleri almak için sağlanır. Java taraftan geçilen parametreler zaman C\# eklentisi başlatılır. `Config` Parametreleri iki parçalara bölünür: `stormConf` ve `pluginConf`.

    public Dictionary<string, Object> stormConf { get; set; }  
    public Dictionary<string, Object> pluginConf { get; set; }  

`stormConf` Storm tarafından tanımlanan parametre ve `pluginConf` SCP tarafından tanımlanan parametre. Örneğin:

    public class Constants
    {
        … …

        // constant string for pluginConf
        public static readonly String NONTRANSACTIONAL_ENABLE_ACK = "nontransactional.ack.enabled";  

        // constant string for stormConf
        public static readonly String STORM_ZOOKEEPER_SERVERS = "storm.zookeeper.servers";           
        public static readonly String STORM_ZOOKEEPER_PORT = "storm.zookeeper.port";                 
    }

`TopologyContext` sağlanan topoloji içerik almak için birden çok paralellik ile bileşenleri için en yararlı olacaktır. Örnek aşağıda verilmiştir:

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
Aşağıdaki arabirimleri belirli bir bağlam örneğine ilgili. Bağlam örneği SCP.NET platformu tarafından oluşturulur ve kullanıcı koduna geçirilen:

    // Declare the Output and Input Stream Schemas

    public void DeclareComponentSchema(ComponentStreamSchema schema);   

    // Emit tuple to default stream.
    public abstract void Emit(List<object> values);                   

    // Emit tuple to the specific stream.
    public abstract void Emit(string streamId, List<object> values);  

ACK destekleme işlemsel olmayan spout için aşağıdaki yöntemi sağlanır:

    // for non-transactional Spout which supports ack
    public abstract void Emit(string streamId, List<object> values, long seqId);  

ACK destekleme işlemsel olmayan Cıvata için açıkça gerektiği `Ack()` veya `Fail()` aldığı tanımlama grubu. Ve yeni bir tanımlama grubu yayma, ayrıca yeni tuple bağlayıcılarını belirtmeniz gerekir. Aşağıdaki yöntemlerden sağlanır.

    public abstract void Emit(string streamId, IEnumerable<SCPTuple> anchors, List<object> values); 
    public abstract void Ack(SCPTuple tuple);
    public abstract void Fail(SCPTuple tuple);

### <a name="statestore"></a>StateStore
`StateStore` Meta Veri Hizmetleri, monoton sıra oluşturma ve bekleme serbest eşgüdüm sağlar. Üst düzey dağıtılmış eşzamanlılık soyutlamalar oluşturulabilen `StateStore`dağıtılmış kilitleri, dağıtılmış kuyruklar, engelleri ve işlem Hizmetleri dahil olmak üzere.

SCP uygulamalar kullanabilir `State` ZooKeeper, özellikle işlem topolojisi için bazı bilgileri kalıcı hale getirmek için nesne. Böylece işlem spout çöker ve yeniden başlatırsanız, ZooKeeper gerekli bilgileri almak ve ardışık düzeni yeniden yapılıyor.

`StateStore` Nesnenin çoğunlukla bu yöntemleri vardır:

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
    /// <returns>Uncommited States</returns>
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
    public IEnumerable<Registry> Commited();

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

`State` Nesnenin çoğunlukla bu yöntemleri vardır:

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

İçin `Commit()` simpleMode ayarlandığında yöntemi true, ZooKeeper içinde karşılık gelen ZNode siler. Aksi takdirde geçerli ZNode ve kabul edilen içinde yeni bir düğüm ekleme siler\_yolu.

### <a name="scpruntime"></a>SCPRuntime
SCPRuntime aşağıdaki iki yöntem sunar:

    public static void Initialize();

    public static void LaunchPlugin(newSCPPlugin createDelegate);  

`Initialize()` SCP çalışma zamanı ortamı başlatmak için kullanılır. Bu yöntemde, C\# işlem Java yan bağlanır ve yapılandırma parametreleri ve topoloji bağlamını alır.

`LaunchPlugin()` ileti işleme döngüsü kazandırın için kullanılır. Bu döngü, C\# eklentisi aldığı iletileri form Java yan (başlık ve denetim sinyalleri dahil) ve sonra işlem belki de arabirim yöntemini çağırarak iletileri, kullanıcı kodu tarafından sağlayın. Giriş parametresi yöntemi için `LaunchPlugin()` ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt arabirimini uygulayan nesne döndüren bir temsilci.

    public delegate ISCPPlugin newSCPPlugin(Context ctx, Dictionary\<string, Object\> parms); 

ISCPBatchBolt için biz alabilirsiniz `StormTxAttempt` gelen `parms`ve bunu denemedir yeniden yürütülmüş olup olmadığını değerlendirmek için kullanabilirsiniz. Yeniden yürütme girişimi denetle genellikle yürütme Cıvata yapılır ve örneklerde gösterildiği `HelloWorldTx` örnek.

Genel olarak bakıldığında, SCP eklentileri burada iki modda çalışabilir:

1. Yerel Test modu: Bu modda SCP eklentileri (C\# kullanıcı kodu) Visual Studio içinde geliştirme aşamasında çalıştırın. `LocalContext` Bu modda, yerel dosyalara verilmiş başlıklar seri hale getirmek ve bunları geri belleğe okumak için yöntem sağlar kullanılabilir.
   
        public interface ILocalContext
        {
            List\<SCPTuple\> RecvFromMsgQueue();
            void WriteMsgQueueToFile(string filepath, bool append = false);  
            void ReadFromFileToMsgQueue(string filepath);                    
        }
2. Normal modu: Bu modda SCP eklentileri storm java işlemi tarafından başlatılır.
   
    SCP eklentisi başlatmanın örnek aşağıda verilmiştir:
   
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

## <a name="topology-specification-language"></a>Topoloji belirtimi dili
SCP topoloji açıklayan ve SCP topolojileri yapılandırmak için bir etki alanına özgü dil belirtimidir. Storm'ın Clojure DSL üzerinde temel alır (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) ve SCP tarafından genişletilir.

Topoloji belirtimleri yürütmenin storm kümesine doğrudan gönderilebilir ***runspec*** komutu.

SCP.NET işlem topolojileri tanımlamak için aşağıdaki işlevleri ekledi:

| **Yeni işlevleri** | **Parametreler** | **Açıklama** |
| --- | --- | --- |
| **Tx topolopy** |Topoloji adı<br />spout eşleme<br />Cıvata eşleme |Topoloji ada sahip bir işlem topolojisi tanımlayın &nbsp;tanımı harita ve Cıvatalar tanımı Haritası spout'lar |
| **SCP tx spout** |Exec adı<br />bağımsız değişken<br />alanlar |İşlem spout tanımlayın. Uygulama ile çalışan ***exec adı*** kullanarak ***args***.<br /><br />***Alanları*** spout çıktı alanları |
| **tx toplu Cıvata SCP** |Exec adı<br />bağımsız değişken<br />alanlar |Bir işlem toplu Cıvata tanımlayın. Uygulama ile çalışan ***exec adı*** kullanarak ***bağımsız değişken.***<br /><br />Alanlar Cıvata çıktı alanları modudur. |
| **SCP-tx-yürütme-Cıvata** |Exec adı<br />bağımsız değişken<br />alanlar |İşlem yürütme Cıvata tanımlayın. Uygulama ile çalışan ***exec adı*** kullanarak ***args***.<br /><br />***Alanları*** Cıvata çıktı alanları |
| **nontx topolopy** |Topoloji adı<br />spout eşleme<br />Cıvata eşleme |Topoloji ada sahip bir işlem topolojisi tanımlayın&nbsp; tanımı harita ve Cıvatalar tanımı Haritası spout'lar |
| **SCP spout** |Exec adı<br />bağımsız değişken<br />alanlar<br />parametreler |Bir işlem spout tanımlayın. Uygulama ile çalışan ***exec adı*** kullanarak ***args***.<br /><br />***Alanları*** spout çıktı alanları<br /><br />***Parametreleri*** "nontransactional.ack.enabled" gibi bazı parametreler belirtmek için kullanmak isteğe bağlıdır. |
| **SCP Cıvata** |Exec adı<br />bağımsız değişken<br />alanlar<br />parametreler |İşlem dışı Cıvata tanımlayın. Uygulama ile çalışan ***exec adı*** kullanarak ***args***.<br /><br />***Alanları*** Cıvata çıktı alanları<br /><br />***Parametreleri*** "nontransactional.ack.enabled" gibi bazı parametreler belirtmek için kullanmak isteğe bağlıdır. |

SCP.NET tanımlanan aşağıdaki anahtar sözcükleri vardır:

| **Anahtar sözcükler** | **Açıklama** |
| --- | --- |
| **: adı** |Topoloji adı tanımlayın |
| **: topolojisi** |Önceki işlevleri kullanarak topolojisi tanımlayın ve olanları içinde oluşturun. |
| **: p** |Her spout veya Cıvata için paralellik ipucu tanımlayın. |
| **: yapılandırma** |Tanımlamak parametresini yapılandırabilir veya var olanları güncelleştir |
| **: şeması** |Akış şeması tanımlayın. |

Ve sık kullanılan parametreleri:

| **Parametre** | **Açıklama** |
| --- | --- |
| **"plugin.name"** |C# eklentisi exe dosyası adı |
| **"plugin.args"** |Eklenti bağımsız değişken |
| **"output.schema"** |Çıkış şeması |
| **"nontransactional.ack.enabled"** |Ack işlem topolojisi için etkinleştirilip etkinleştirilmediği |

Runspec komutu BITS ile birlikte dağıtılır, kullanım gibidir:

    .\bin\runSpec.cmd
    usage: runSpec [spec-file target-dir [resource-dir] [-cp classpath]]
    ex: runSpec examples\HelloWorld\HelloWorld.spec specs examples\HelloWorld\Target

***Kaynak dir*** parametre isteğe bağlı, bir C takın istediğinizde belirtmek zorunda\# uygulama ve bu dizinde uygulama, bağımlılıklar ve yapılandırmaları içerir.

***Sınıf*** parametredir Ayrıca isteğe bağlı. Java Spout veya Cıvata belirtim dosya içeriyorsa, Java sınıf belirtmek için kullanılır.

## <a name="miscellaneous-features"></a>Çeşitli özellikler
### <a name="input-and-output-schema-declaration"></a>Girdi ve çıktı şema bildirimi
Kullanıcıların c tanımlama grubu yayma\# işlemleri, platform gerekiyor tanımlama grubu seri byte [], Java yan transfer ve Storm bu tanımlama grubu hedeflerini transfer. Aşağı Akış bileşenleri, C, bu sırada\# işlemleri geri java taraftan diziler alır ve platforma göre özgün türlerine dönüştürmek, bu işlemlerin Platform tarafından gizlenir.

Seri hale getirme ve seri durumdan çıkarma desteklemesi, kullanıcı kodu girişleri ve çıkışları şeması bildirmeniz gerekir.

Giriş/Çıkış akış şeması bir sözlük olarak tanımlanır. StreamId anahtardır. Sütun türlerini değerdir. Bileşen bildirilen çok akışları sahip olabilir.

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


Bağlam nesnesinde eklenen aşağıdaki API vardır:

    public void DeclareComponentSchema(ComponentStreamSchema schema)

Geliştiricilerin bu akış için tanımlanan şema yayılan başlıklar uyma, aksi halde sistem çalışma zamanı özel durum atar emin olmalısınız.

### <a name="multi-stream-support"></a>Birden çok akış desteği
SCP yayma veya aynı anda birden çok ayrı akışlardan almak için kullanıcı kodu destekler. Emit yöntemi bir isteğe bağlı Akış ID parametresi yararlanırken destek bağlamı nesnesinde yansıtır.

SCP.NET bağlamı nesnesindeki iki yöntemler eklenmiştir. Tanımlama grubu ya da diziler StreamId belirtmek için yaymak üzere kullanılır. StreamId bir dize ve her iki C'de tutarlı olması gerekiyor\# ve topoloji tanımı belirtimi.

        /* Emit tuple to the specific stream. */
        public abstract void Emit(string streamId, List<object> values);

        /* for non-transactional Spout only */
        public abstract void Emit(string streamId, List<object> values, long seqId);

Var olmayan bir akış yayma çalışma zamanı özel durumları neden olur.

### <a name="fields-grouping"></a>Alanları gruplandırma
Yerleşik Strom alanları gruplandırma SCP.NET düzgün çalışmıyor. Java Proxy tarafında tüm alanlar veri türleri: gerçekte byte [] ve gruplandırma alanları gruplandırma gerçekleştirmek için byte [] nesnesi karma kodu kullanır. Byte [] nesnesi karma kodu, bu nesnenin bellekte adresidir. Bu nedenle gruplandırma aynı içerik ancak aynı adresini paylaşan iki byte [] nesneler için yanlış olur.

Özelleştirilmiş gruplandırma yöntemi SCP.NET ekler ve byte [] içeriğini gruplandırma yapmak için kullanır. İçinde **SPEC** gibi dosya, söz dizimi:

    (bolt-spec
        {
            "spout_test" (scp-field-group :non-tx [0,1])
        }
        …
    )


Burada,

1. "scp alan grup", "SCP tarafından uygulanan özelleştirilmiş alan gruplandırma" anlamına gelir.
2. ": tx"veya": tx olmayan" işlem topoloji olduğu anlamına gelir. Başlangıç dizini tx olmayan topolojileri tx farklı olduğundan bu bilgileri ihtiyacımız var.
3. [0,1] anlamına alan kimlikleri, 0'dan başlayarak karma kümesi.

### <a name="hybrid-topology"></a>Karma topolojisi
Yerel Storm Java yazılır. SCP.Net C etkinleştirmek için Gelişmiş ve\# C yazmak için geliştiricilere\# kendi iş mantığı işlemek için kod. Ancak yalnızca C içeren karma topolojiler de destekler\# spout'lar/Cıvatalar, aynı zamanda Java Spout/Cıvatalar.

### <a name="specify-java-spoutbolt-in-spec-file"></a>Java Spout/Cıvata belirtim dosyasında belirtin
Belirtim dosyasındaki "scp-spout" ve "scp-Cıvata" de Java Spout'lar ve Cıvatalar belirtmek için kullanılabilir, örnek aşağıda verilmiştir:

    (spout-spec 
      (microsoft.scp.example.HybridTopology.Generator.)           
      :p 1)

Burada `microsoft.scp.example.HybridTopology.Generator` Java Spout sınıfı adı.

### <a name="specify-java-classpath-in-runspec-command"></a>Java sınıf runSpec komutunu belirtin
Java Spout'lar veya Cıvatalar içeren topoloji göndermek istiyorsanız, önce Java Spout'lar veya Cıvatalar derlemek ve Jar dosyalarını almak gerekir. Ardından topoloji gönderirken Jar dosyalarını içeren java sınıf belirtmeniz gerekir. Örnek aşağıda verilmiştir:

    bin\runSpec.cmd examples\HybridTopology\HybridTopology.spec specs examples\HybridTopology\net\Target -cp examples\HybridTopology\java\target\*

Burada **örnekler\\HybridTopology\\java\\hedef\\**  Spout/Cıvata Java Jar dosyasını içeren klasör.

### <a name="serialization-and-deserialization-between-java-and-c"></a>Seri hale getirme ve seri durumdan çıkarma Java ve C arasındaki\#
SCP bileşeni içerir Java yan ve C\# yan. Yerel Java Spout'lar/Cıvatalar ile etkileşim kurmak için serileştirme/seri durumdan çıkarma Java yan ve C uygulanması gereken\# , aşağıdaki grafikte gösterildiği gibi tarafı.

![Java bileşenine gönderme SCP bileşen gönderme java bileşeni diyagramı](./media/apache-storm-scp-programming-guide/java-compent-sending-to-scp-component-sending-to-java-component.png)

1. **Seri hale getirme Java yan ve seri durumdan çıkarma c\# yan**
   
   Java taraf serileştirme ve seri durumundan çıkarma c için varsayılan uygulaması ilk sağladığımız\# yan. Java yan serileştirme yönteminde belirtim dosyasında belirtilebilir:
   
       (scp-bolt
           {
               "plugin.name" "HybridTopology.exe"
               "plugin.args" ["displayer"]
               "output.schema" {}
               "customized.java.serializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer"]
           })
   
   Seri durumdan çıkarma yöntemini c\# yan C'de belirtilmelidir\# kullanıcı kodu:
   
       Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
       inputSchema.Add("default", new List<Type>() { typeof(Person) });
       this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
       this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());            
   
   Veri türü çok karmaşık değil sağlanan çoğu durumda bu varsayılan uygulama işlemelidir. Belirli durumlarda, kullanıcı veri türü çok karmaşık olduğu için veya bizim varsayılan uygulama performansını kendi uygulama kullanıcının gereksinimi, kullanıcıların can eklenti karşılamadığından.
   
   Java yan serileştirme arabiriminde olarak tanımlanır:
   
       public interface ICustomizedInteropJavaSerializer {
           public void prepare(String[] args);
           public List<ByteBuffer> serialize(List<Object> objectList);
       }
   
   C deserialize arabiriminde\# yan olarak tanımlanır:
   
   Ortak arabirimi ICustomizedInteropCSharpDeserializer
   
       public interface ICustomizedInteropCSharpDeserializer
       {
           List<Object> Deserialize(List<byte[]> dataList, List<Type> targetTypes);
       }
2. **Seri hale getirme c\# yan ve Java taraf seri durumdan çıkarma**
   
   C serileştirme yönteminde\# yan C'de belirtilmelidir\# kullanıcı kodu:
   
       this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer()); 
   
   Java taraf seri durumdan çıkarma yöntemini belirtim dosyasında belirtilen:
   
     (scp-spout
   
       {
         "plugin.name" "HybridTopology.exe"
         "plugin.args" ["generator"]
         "output.schema" {"default" ["person"]}
         "customized.java.deserializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" "microsoft.scp.example.HybridTopology.Person"]
       })
   
   Burada "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" seri durumdan Çıkarıcının adını, ve "microsoft.scp.example.HybridTopology.Person" için hedef sınıf veri serisi.
   
   Kullanıcı ayrıca C kendi uygulamasında takın\# seri hale getirici ve Java seri durumdan Çıkarıcının. Bu kod C arabirimidir\# seri hale getirici:
   
       public interface ICustomizedInteropCSharpSerializer
       {
           List<byte[]> Serialize(List<object> dataList);
       }
   
   Bu kod Java kaldırıcı arabirimidir:
   
       public interface ICustomizedInteropJavaDeserializer {
           public void prepare(String[] targetClassNames);
           public List<Object> Deserialize(List<ByteBuffer> dataList);
       }

## <a name="scp-host-mode"></a>SCP ana mod
Bu modda, kullanıcı kodlarına dll derleyin ve SCP tarafından sağlanan SCPHost.exe topoloji göndermek için kullanın. Özel dosyayı bu kod gibi görünür:

    (scp-spout
      {
        "plugin.name" "SCPHost.exe"
        "plugin.args" ["HelloWorld.dll" "Scp.App.HelloWorld.Generator" "Get"]
        "output.schema" {"default" ["sentence"]}
      })

Burada, `plugin.name` olarak belirtilen `SCPHost.exe` SCP SDK'sı tarafından sağlanan. SCPHost.exe üç parametre kabul eder:

1. İlk olan DLL adıdır `"HelloWorld.dll"` Bu örnekte.
2. İkincisi olan sınıf adı olduğu `"Scp.App.HelloWorld.Generator"` Bu örnekte.
3. Üçüncü bir ISCPPlugin örneğini almak üzere çağrılabilen genel bir statik yöntem adıdır.

Ana mod, kullanıcı kodu DLL olarak derlenmiş ve SCP platformu tarafından çağrılır. Bu nedenle SCP platform tüm işlem mantığının tam denetim elde edebilirsiniz. Bu nedenle geliştirme deneyimi kolaylaştırmak ve bize de sonraki sürüm için daha fazla esneklik ve daha iyi geriye dönük uyumluluk Getir SCP ana mod topolojisinde göndermek için müşterilerimizin öneririz.

## <a name="scp-programming-examples"></a>SCP programlama örnekleri
### <a name="helloworld"></a>HelloWorld
**HelloWorld** SCP.Net bakış göstermek için basit bir örnektir. Adlı bir spout ile bir işlemsel olmayan topolojisi kullanır **Oluşturucu**ve adlı iki Cıvatalar **Bölümlendirici** ve **sayaç**. Spout **Oluşturucu** rastgele cümleleri oluşturur ve bu cümleleri yayma **Bölümlendirici**. Cıvata **Bölümlendirici** sözcükler cümlelere ayırır ve bu sözcükleri yayma **sayaç** Cıvata. Her sözcüğün oluşum sayısı kaydetmek için bir sözlük "sayacı" Cıvata kullanır.

İki belirtim dosya **HelloWorld.spec** ve **HelloWorld\_EnableAck.spec** Bu örnek için. C\# kodu, onu bulabilir ack Java taraftan pluginConf alarak etkinleştirilip etkinleştirilmediği çıkışı.

    /* demo how to get pluginConf info */
    if (Context.Config.pluginConf.ContainsKey(Constants.NONTRANSACTIONAL_ENABLE_ACK))
    {
        enableAck = (bool)(Context.Config.pluginConf[Constants.NONTRANSACTIONAL_ENABLE_ACK]);
    }
    Context.Logger.Info("enableAck: {0}", enableAck);

ACK etkinleştirilirse, spout içinde bir sözlük acked edilmemiş başlıklar önbelleğe almak için kullanılır. Fail() çağrılırsa, başarısız tanımlama grubu tekrarlanır:

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
**HelloWorldTx** örnek nasıl işlem topoloji uygulanacağını gösterir. Adlı bir spout sahip **Oluşturucu**, toplu Cıvata adlı **kısmi-count**, ve bir yürütme Cıvata adlı **sayısı toplam**. Ayrıca üç önceden oluşturulmuş txt dosyaları vardır: **DataSource0.txt**, **DataSource1.txt**, ve **DataSource2.txt**.

Spout her işlemde **Oluşturucu** rastgele iki dosya önceden oluşturulmuş üç dosyalarından seçer ve iki dosya adlarına yayma **kısmi-count** Cıvata. Cıvata **kısmi-count** dosya adı alınan tanımlama grubundan sonra dosyasını açın ve bu dosyadaki sözcük sayısını ve son olarak word numarasına yayma alır **sayısı toplam** Cıvata. **Sayısı toplam** Cıvata özetler toplam sayısı.

Elde etmek için **tam olarak bir kez** semantiği, yürütme Cıvata **sayısı toplam** yeniden yürütülmüş bir işlem olup olmadığını değerlendirmek için gerekir. Bu örnekte, bir statik üye değişkeni vardır:

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

Zaman `FinishBatch()` olarak adlandırılır, `lastCommittedTxId` yeniden yürütülmüş bir işlem değilse güncelleştirilir.

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
Bu topoloji Spout Java ve C içeren\# Cıvata. SCP platform tarafından sağlanan varsayılan seri hale getirme ve seri durumdan çıkarma uygulaması kullanır. Bkz: **HybridTopology.spec** içinde **örnekler\\HybridTopology** belirtim dosya ayrıntıları için klasör ve **SubmitTopology.bat** nasıl Java belirtmek sınıf.

### <a name="scphostdemo"></a>SCPHostDemo
Bu örnek HelloWorld temelde aynıdır. Tek fark, kullanıcı kodu DLL olarak derlenir ve topoloji SCPHost.exe kullanılarak gönderilen olmasıdır. Daha ayrıntılı açıklaması için "SCP ana modu" bölümüne bakın.

## <a name="next-steps"></a>Sonraki Adımlar
Storm topolojilerini SCP kullanılarak oluşturulan bir örnekleri için aşağıdaki belgelere bakın:

* [Visual Studio kullanarak Hdınsight üzerinde Apache Storm için C# topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md)
* [Hdınsight üzerinde Storm ile Azure Event hubs'tan işlem olayları](apache-storm-develop-csharp-event-hub-topology.md)
* [Event Hubs Hdınsight üzerinde Storm kullanmaya aracın algılayıcı verilerini işlemek](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/IotExample)
* [Ayıklama, dönüştürme ve HBase için Azure olay hub'larından (ETL) yükleme](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample)
