---
title: Kuyruk depolama ve Visual Studio ile çalışmaya başlama bağlı Hizmetleri (Web işi projeleri) | Microsoft Docs
description: Visual Studio kullanarak bir depolama hesabı bağlandıktan sonra bir Web işi projesinin Azure kuyruk depolama ile çalışmaya başlamak nasıl Hizmetleri bağlı.
services: storage
author: ghogen
manager: douge
ms.assetid: 5c3ef267-2a67-44e9-ab4a-1edd7015034f
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: article
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 332d682147ba832f631052d8348039f74b46c438
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-webjob-projects"></a>Azure kuyruk depolama ve Visual Studio ile çalışmaya başlama bağlı Hizmetleri (Web işi projeler)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede nasıl oluşturduğunuz veya Visual Studio kullanarak Azure storage hesabı başvurulan sonra Visual Studio Azure WebJob projede Azure kuyruk depolama kullanma başlamak **bağlı Hizmetleri Ekle** iletişim kutusu. Eklediğinizde bir depolama hesabı bir Web işi projesi için Visual Studio kullanarak **bağlı Hizmetleri Ekle** iletişim kutusunda, uygun Azure depolama NuGet paketleri yüklü, uygun .NET başvuruları projeye eklenir ve depolama hesabı için bağlantı dizelerini App.config dosyasında güncelleştirilir.  

Bu makalede Azure WebJobs SDK sürümü kullanmak nasıl gösteren C# kod örnekleri sağlar 1.x Azure kuyruk depolama hizmeti ile.

Azure Kuyruk depolama, HTTP veya HTTPS kullanan kimlik doğrulaması yapılmış çağrılar aracılığıyla dünyanın her yerinden erişilebilen çok sayıda iletinin depolanması için bir hizmettir. Tek bir kuyruk iletisinin boyutu 64 KB’ye kadar olabilir ve bir kuyrukta, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti bulunabilir. Bkz: [.NET kullanarak Azure kuyruk depolama ile çalışmaya başlama](../storage/queues/storage-dotnet-how-to-use-queues.md) daha fazla bilgi için. ASP.NET hakkında daha fazla bilgi için bkz: [ASP.NET](http://www.asp.net).

## <a name="how-to-trigger-a-function-when-a-queue-message-is-received"></a>Bir kuyruk iletisi alındığında bir işlev tetikleme
Bir kuyruk iletisi alındığında WebJobs SDK çağıran bir işlev yazmak için **QueueTrigger** özniteliği. Öznitelik oluşturucunun yoklamak için sıra adını belirten bir dize parametresi alan. Kuyruk adı dinamik olarak belirlemek nasıl görmek için kullanıma [yapılandırma seçeneklerinin nasıl ayarlanacağını](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>Dize iletileri
Bir dize ileti sırası aşağıdaki örnekte, bu nedenle içerir **QueueTrigger** adlı bir dize parametresi uygulanan **logMessage** kuyruk iletisini içeriğini içerir. İşlev [Pano için bir günlük iletisi Yazar](#how-to-write-logs).

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

Yanında **dize**, parametresi bir bayt dizisi olabilir bir **CloudQueueMessage** nesne ya da tanımladığınız bir POCO.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(düz eski CLR nesnesi](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) kuyruk iletileri
Aşağıdaki örnekte, JSON için kuyruk iletisini içeren bir **BlobInformation** içeren nesne bir **BlobName** özelliği. SDK'yı otomatik olarak nesne seri durumdan çıkarır.

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

SDK'sı [Newtonsoft.Json NuGet paketi](http://www.nuget.org/packages/Newtonsoft.Json) seri hale getirmek ve seri durumdan iletileri. WebJobs SDK kullanmayan bir programda iletileri kuyruğa oluşturursanız, SDK ayrıştıramıyor bir POCO kuyruk iletisi oluşturmak için aşağıdaki örneğe benzer kod yazabilirsiniz.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a>Zaman uyumsuz işlevleri
Aşağıdaki zaman uyumsuz işlev [bir günlüğü panoya Yazar](#how-to-write-logs).

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Zaman uyumsuz işlevleri sürebilir bir [iptal belirteci](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken)blob kopyalar aşağıdaki örnekte gösterildiği gibi. (Bir açıklaması için **queueTrigger** yer tutucu, bkz: [BLOB'lar](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) bölümüne.)

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

## <a name="types-the-queuetrigger-attribute-works-with"></a>Türleri QueueTrigger öznitelik birlikte çalışır.
Kullanabileceğiniz **QueueTrigger** şu türden:

* **Dize**
* JSON olarak serileştirilen bir POCO türü
* **Byte]**
* **CloudQueueMessage**

## <a name="polling-algorithm"></a>Yoklama algoritması
SDK boşta kuyruk depolama işlem maliyetleri yoklama etkisini azaltmak için bir rastgele üstel geri alma algoritması uygular.  Bir ileti bulunduğunda, SDK iki saniye bekler ve ardından başka bir ileti için denetler; bir ileti bulunduğunda yeniden denemeden önce yaklaşık dört saniye bekler. Bir kuyruk iletisi almak için sonraki başarısız girişimden sonra bekleme süresini bir dakika olarak varsayılan olarak en fazla bekleme süresi ulaşana kadar artmaya devam eder. [En fazla bekleme süresi yapılandırılabilir](#how-to-set-configuration-options).

## <a name="multiple-instances"></a>Birden çok örneği
Web uygulamanız birden çok örneği üzerinde çalışıyorsa, sürekli Webjob'lar her makinede çalışır ve her makine için Tetikleyicileri bekleyin ve işlevleri çalıştırmayı deneyin. Bu aynı verileri iki kez işlemeyi bazı işlevler neden bazı senaryolarda, böylece işlevleri (böylece bunları sürekli olarak aynı girdi verileriyle çağırma yinelenen sonuçları oluşturmuyor yazılmış) ıdempotent gerekir.  

## <a name="parallel-execution"></a>Paralel yürütme
Birden çok işlevler farklı sıralarda dinleme varsa, iletileri aynı anda alındığında SDK bunları paralel olarak çağırın.

Tek bir kuyruk için birden fazla ileti alındığında aynı durum geçerlidir. Varsayılan olarak, SDK 16 iletileri kuyruğa toplu bir zaman alır ve paralel olarak işler işlevi yürütür. [Toplu iş boyutu yapılandırılabilir](#how-to-set-configuration-options). İşlenmekte olan numarası yarısı toplu iş boyutu aldığında, SDK başka bir toplu iş alır ve bu iletileri işleme başlatır. Bu nedenle en fazla eş zamanlı ileti işlevi işlenen sayısı bir ve yarı kez bir toplu iş boyutu dur. Bu sınırı olan her işlev ayrı olarak geçerli bir **QueueTrigger** özniteliği. Paralel yürütme üzerinde bir Sıraya alınan iletileri istemiyorsanız, yığın boyutu 1 olarak ayarlayın.

## <a name="get-queue-or-queue-message-metadata"></a>Sıra veya sıra ileti meta verileri alma
Aşağıdaki ileti özellikleri yöntemi imza parametreleri ekleyerek alabilirsiniz:

* **DateTimeOffset** expirationTime
* **DateTimeOffset** insertionTime
* **DateTimeOffset** nextVisibleTime
* **dize** queueTrigger (ileti metni içerir)
* **dize** kimliği
* **dize** popReceipt
* **int** dequeueCount

Azure depolama alanıyla doğrudan API çalışmak isterseniz, ayrıca ekleyebileceğiniz bir **CloudStorageAccount** parametresi.

Aşağıdaki örnekte tüm bu meta veri bilgileri uygulama günlüğüne yazar. Örnekte, kuyruk iletisini içeriğini logMessage ve queueTrigger içerir.

        public static void WriteLog([QueueTrigger("logqueue")] string logMessage,
            DateTimeOffset expirationTime,
            DateTimeOffset insertionTime,
            DateTimeOffset nextVisibleTime,
            string id,
            string popReceipt,
            int dequeueCount,
            string queueTrigger,
            CloudStorageAccount cloudStorageAccount,
            TextWriter logger)
        {
            logger.WriteLine(
                "logMessage={0}\n" +
            "expirationTime={1}\ninsertionTime={2}\n" +
                "nextVisibleTime={3}\n" +
                "id={4}\npopReceipt={5}\ndequeueCount={6}\n" +
                "queue endpoint={7} queueTrigger={8}",
                logMessage, expirationTime,
                insertionTime,
                nextVisibleTime, id,
                popReceipt, dequeueCount,
                cloudStorageAccount.QueueEndpoint,
                queueTrigger);
        }

Örnek kodu ile yazılmış bir örnek günlük şöyledir:

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

## <a name="graceful-shutdown"></a>Kapama
Sürekli bir WebJob içinde çalışan bir işlevinin kabul edebileceği bir **CancellationToken** işlevi hakkında sonlandırılacak WebJob olduğunu bildirmek işletim sistemi sağlayan parametre. Bu bildirim, beklenmedik bir şekilde veri tutarsız bir durumda bırakır şekilde sonlandırma işlevi değil emin olmak için kullanabilirsiniz.

Aşağıdaki örnek, bir işlev yaklaşan WebJob sonlandırma denetlemek gösterilmiştir.

    public static void GracefulShutdownDemo(
                [QueueTrigger("inputqueue")] string inputText,
                TextWriter logger,
                CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(1000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }

**Not:** durumunu ve çıkışını kapatılmışsa işlevlerin Pano doğru gösterilmeyebilir.

Daha fazla bilgi için bkz: [Web işleri normal şekilde kapatılmasını](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).   

## <a name="how-to-create-a-queue-message-while-processing-a-queue-message"></a>Bir kuyruk iletisi işlenirken bir kuyruk iletisi oluşturma
Yeni bir kuyruk iletisi oluşturan bir işlev yazmak için **sıra** özniteliği. Gibi **QueueTrigger**, kuyruk adı bir dize olarak geçirin veya yapabilecekleriniz [sıra adı dinamik olarak ayarlamak](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>Dize iletileri
Aşağıdaki zaman uyumsuz olmayan kod örneği sıranın "inputqueue" adlı sıraya alınan kuyruk iletisini aynı içerikle "outputqueue" adlı yeni bir kuyruk iletisi oluşturur. (İşlevler için async kullanma **IAsyncCollector<T>**  daha sonra bu bölümde gösterilen.)

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(düz eski CLR nesnesi](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) kuyruk iletileri
Bir dize yerine bir POCO içeren bir kuyruk iletisi oluşturmak için bir çıktı parametresi olarak POCO türü geçirmek **sıra** özniteliği Oluşturucusu.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

SDK'yı otomatik olarak JSON nesneyi serileştirir. Nesne boş olsa bile bir kuyruk iletisi her zaman oluşturulur.

### <a name="create-multiple-messages-or-in-async-functions"></a>Birden çok iletileri oluşturmak veya zaman uyumsuz işlevleri
Birden çok iletileri oluşturmak için çıkış sırası için parametre türü olun **ICollector<T>**  veya **IAsyncCollector<T>**, aşağıdaki örnekte gösterildiği gibi.

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Her kuyruk iletisi hemen oluşturulan zaman **Ekle** yöntemi çağrılır.

### <a name="types-that-the-queue-attribute-works-with"></a>Sıra özniteliği çalışır türleri
Kullanabileceğiniz **sıra** özniteliği aşağıdaki parametre türleri:

* **dize çıkışı** (parametre değeri null olmayan ise işlevi sona erdiğinde kuyruk iletisi oluşturur)
* **byte [] çıkışı** (gibi çalışır **dize**)
* **CloudQueueMessage çıkışı** (gibi çalışır **dize**)
* **POCO çıkışı** (serializable bir tür oluşturduğu bir ileti null bir nesne ile işlevi sona erdiğinde parametre null ise)
* **ICollector**
* **IAsyncCollector**
* **CloudQueue** (için el ile Azure Storage API'sini kullanarak doğrudan iletileri oluşturma)

### <a name="use-webjobs-sdk-attributes-in-the-body-of-a-function"></a>Web işleri SDK'si öznitelikleri bir işlev gövdesine kullanın
Web işleri SDK'si öznitelik gibi kullanmadan önce işlevinizi bazı iş yapmanız gerekirse **sıra**, **Blob**, veya **tablo**, kullanabilirsiniz **IBinder** arabirimi.

Aşağıdaki örnek, bir giriş sırası iletisi alır ve bir çıkış sırası aynı içeriği ile yeni bir ileti oluşturur. Çıkış sırası adı işlevinin gövdesini kodda tarafından ayarlanır.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

**IBinder** arabirimi de kullanılabilir olan **tablo** ve **Blob** öznitelikleri.

## <a name="how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message"></a>Nasıl okunacağını ve yazma BLOB'ları ve bir sıraya ileti işlenirken tabloları
**Blob** ve **tablo** öznitelikleri BLOB'ları ve tabloları okuma ve yazma olanak tanır. Bu bölümdeki örnekler BLOB'lar için geçerlidir. BLOB'ları oluşturulduğunda veya güncelleştirilmiş işlemleri tetiklemek nasıl gösteren kod örnekleri için bkz: [WebJobs SDK ile Azure blob storage kullanma](https://github.com/Azure/azure-webjobs-sdk/wiki).
<!-- , and for code samples that read and write tables, see [How to use Azure table storage with the WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md). -->

### <a name="string-queue-messages-triggering-blob-operations"></a>Dize iletileri kuyruğa blobu işlemleri tetikleme
Bir dizeyi içeren bir kuyruk iletisi için **queueTrigger** olarak kullanabileceğiniz bir yer tutucudur **Blob** özniteliğin **blobPath** iletinin içeriğini içeren bir parametre.

Aşağıdaki örnek kullanır **akış** nesneleri okumak ve BLOB'ları yazmak için. Kuyruk iletisini textblobs kapsayıcıda bulunan bir blob adıdır. Blob ile birlikte bir kopyasını "-Yeni" eklenecek ad aynı kapsayıcıda oluşturulur.

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

**Blob** özniteliği Oluşturucusu alır bir **blobPath** kapsayıcı ve blob adını belirten parametre. Bu yer tutucu hakkında daha fazla bilgi için bkz: [WebJobs SDK ile Azure blob storage kullanma](https://github.com/Azure/azure-webjobs-sdk/wiki).

Ne zaman öznitelik süsler bir **akış** nesnesi, başka bir oluşturucu parametresini belirtir **FileAccess** modu okuma, yazma veya okuma/yazma olarak.

Aşağıdaki örnek kullanan bir **CloudBlockBlob** bir blobu silmek için nesne. Kuyruk iletisini blob adıdır.

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(düz eski CLR nesnesi](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) kuyruk iletileri
Kuyruk iletisini JSON olarak depolanan bir POCO için nesnenin özelliklerini adı yer tutucuları kullanabilirsiniz **sıra** özniteliğin **blobPath** parametresi. Bu gibi durumlarda, sıra meta veri özellik adları da yer tutucu olarak kullanabilirsiniz. Bkz: [sıra veya sıra ileti meta verileri alma](#get-queue-or-queue-message-metadata).

Aşağıdaki örnek yeni bir blob farklı bir uzantıya sahip bir blob kopyalar. Kuyruk iletisi bir **BlobInformation** içeren nesnesinin **BlobName** ve **BlobNameWithoutExtension** özellikleri. Blob yolu için yer tutucu olarak kullanılan özellik adları **Blob** öznitelikleri.

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

SDK'sı [Newtonsoft.Json NuGet paketi](http://www.nuget.org/packages/Newtonsoft.Json) seri hale getirmek ve seri durumdan iletileri. WebJobs SDK kullanmayan bir programda iletileri kuyruğa oluşturursanız, SDK ayrıştıramıyor bir POCO kuyruk iletisi oluşturmak için aşağıdaki örneğe benzer kod yazabilirsiniz.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

Bir blob için bir nesne bağlama önce işlevinizi bazı iş gerçekleştirmeniz gerekiyorsa, gösterildiği gibi işlevinin gövdesini özniteliğinde kullanabilirsiniz [kullanmak Web işleri SDK'si öznitelikleri bir işlev gövdesine](#use-webjobs-sdk-attributes-in-the-body-of-a-function).

### <a name="types-you-can-use-the-blob-attribute-with"></a>Blob özniteliğiyle kullanabileceğiniz türü
**Blob** özniteliği şu türleriyle kullanılabilir:

* **Akış** (okuma veya yazma, FileAccess Oluşturucu parametresi kullanılarak belirtilen)
* **TextReader**
* **TextWriter**
* **dize** (okuma)
* **dize çıkışı** (yazma; işlevi döndüğünde yalnızca dize parametresi null olmayan ise bir blob oluşturur)
* POCO (okuma)
* POCO out (yazma; her zaman bir blob oluşturur, işlevi döndüğünde POCO parametre null ise null nesnesi olarak oluşturur)
* **CloudBlobStream** (yazma)
* **ICloudBlob** (okuma veya yazma)
* **CloudBlockBlob** (okuma veya yazma)
* **CloudPageBlob** (okuma veya yazma)

## <a name="how-to-handle-poison-messages"></a>Zehirli ileti işleme
İçerikleri bir işlev başarısız olmasına neden olan iletileri çağrılır *zehirli ileti*. İşlev başarısız olduğunda, kuyruk iletisini silinmez ve sonunda tekrar tekrar için döngü neden kayıt. SDK'yı otomatik olarak sınırlı sayıda yineleme sonra döngüsü engelleyebilecek veya el ile yapabilirsiniz.

### <a name="automatic-poison-message-handling"></a>Otomatik zehirli ileti işleme
SDK bir kuyruk iletisi işleyemedi 5 kata işlevi çağırır. Beşinci deneme başarısız olursa, ileti zararlı kuyruğuna taşınır. İçinde yeniden deneme sayısı üst sınırını yapılandırmak nasıl görebilirsiniz [yapılandırma seçeneklerinin nasıl ayarlanacağını](#how-to-set-configuration-options).

Adlı zararlı sırası *{originalqueuename}*-zararlı. Günlüğe yazma veya el ile ilgili dikkat bir bildirim göndererek zararlı sırasından iletilerini işlemek için bir işlev gerekli yazabilirsiniz.

Aşağıdaki örnekte **CopyBlob** bir kuyruk iletisi mevcut olmayan bir blob adını içerdiğinde işlevi başarısız olur. Bu durum oluştuğunda ileti copyblobqueue poison kuyruğuna copyblobqueue sıradan taşınır. **ProcessPoisonMessage** zehir iletisi günlüğe kaydeder.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

        public static void ProcessPoisonMessage(
            [QueueTrigger("copyblobqueue-poison")] string blobName, TextWriter logger)
        {
            logger.WriteLine("Failed to copy blob, name=" + blobName);
        }

Zararlı bir ileti işlenirken bu işlevler konsol çıktısı aşağıda gösterilmektedir.

![Zehirli ileti işleme için konsol çıkışı](./media/vs-storage-webjobs-getting-started-queues/poison.png)

### <a name="manual-poison-message-handling"></a>El ile zehirli ileti işleme
Ekleyerek sayısı bir ileti toplanma işleme alabilirsiniz bir **int** adlı parametre **dequeueCount** , işlevi. Ardından, işlev kodu dequeue sayıma denetleyin ve sayısı bir eşiği aştığında kendi zehir iletisi aşağıdaki örnekte gösterildiği gibi işleme gerçekleştirin.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed to copy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <a name="how-to-set-configuration-options"></a>Yapılandırma seçeneklerini ayarlama
Kullanabileceğiniz **JobHostConfiguration** türü aşağıdaki yapılandırma seçeneklerini ayarlamak için:

* Kod içinde SDK bağlantı dizelerini ayarlayın.
* Yapılandırma **QueueTrigger** maksimum gibi ayarları dequeue sayısı.
* Sıra adları yapılandırmasından alın.

### <a name="set-sdk-connection-strings-in-code"></a>Kod içinde SDK bağlantı dizelerini ayarlayın
Kodda SDK bağlantı dizelerini ayarlama, kendi bağlantı dizesi adlarında yapılandırma dosyalarının veya ortam değişkenlerini kullanmak aşağıdaki örnekte gösterildiği gibi sağlar.

        static void Main(string[] args)
        {
            var _storageConn = ConfigurationManager
                .ConnectionStrings["MyStorageConnection"].ConnectionString;

            var _dashboardConn = ConfigurationManager
                .ConnectionStrings["MyDashboardConnection"].ConnectionString;

            var _serviceBusConn = ConfigurationManager
                .ConnectionStrings["MyServiceBusConnection"].ConnectionString;

            JobHostConfiguration config = new JobHostConfiguration();
            config.StorageConnectionString = _storageConn;
            config.DashboardConnectionString = _dashboardConn;
            config.ServiceBusConnectionString = _serviceBusConn;
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="configure-queuetrigger--settings"></a>QueueTrigger ayarlarını yapılandırma
Sıra ileti işleme için uygulama aşağıdaki ayarları yapılandırabilirsiniz:

* En fazla eşzamanlı olarak paralel olarak yürütülecek toplanmış sıra ileti sayısı (varsayılan olarak 16).
* Bir kuyruk iletisi zararlı bir sıraya gönderilmeden önce yeniden deneme sayısı (varsayılan olarak 5).
* En fazla bekleme süresi bir sıra boş olduğunda yeniden yoklama önce (varsayılan değer 1 dakika).

Aşağıdaki örnek, bu ayarların nasıl yapılandırılacağını gösterir:

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="set-values-for-webjobs-sdk-constructor-parameters-in-code"></a>Değerleri için Web işleri SDK'si Oluşturucu parametreleri kodda ayarlama
Kuyruk adı, bir blob adı veya kapsayıcı belirtmek istediğiniz bazen veya bir tablo adı kod sabit kodlu yerine onu. Örneğin, kuyruk adı belirtmek isteyebilirsiniz **QueueTrigger** bir yapılandırma dosyası veya ortam değişkeninde.

Bunu geçirerek yapabilirsiniz bir **NameResolver** nesnesini **JobHostConfiguration** türü. Web işleri SDK'si özniteliği Oluşturucusu parametrelerinde yüzde (%) işareti tarafından çevrelenen özel yer tutucular içerir ve **NameResolver** kod bu yer tutucular yerine kullanılacak gerçek değerler belirtir.

Örneğin, test ortamında logqueuetest ve üretimde bir adlandırılmış logqueueprod adlı bir sıra kullanmak istediğinizi varsayalım. Bir giriş adını belirtmek istediğiniz sabit kodlanmış kuyruk adı yerine **appSettings** gerçek sıra adı olurdu koleksiyonu. Varsa **appSettings** anahtar logqueue, işlevinizi aşağıdaki gibi görünebilir.

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

**NameResolver** sınıfını sıra adından sonra almak **appSettings** aşağıdaki örnekte gösterildiği gibi:

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

Geçirdiğiniz **NameResolver** için sınıfını **JobHost** nesne aşağıdaki örnekte gösterildiği gibi.

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

**Not:** kuyruk, tablo ve blob adları çözümlenmiş her zaman bir işlev çağrılır, ancak blob kapsayıcı adları yalnızca uygulama başladığında çözümlenir. İş çalışırken blob kapsayıcı adı değiştirilemiyor.

## <a name="how-to-trigger-a-function-manually"></a>Bir işlev el ile tetikleme
Bir işlev el ile tetiklemek için kullanabileceğiniz **çağrısı** veya **CallAsync** yöntemi **JobHost** nesne ve **NoAutomaticTrigger** aşağıdaki örnekte gösterildiği gibi işlev, öznitelik.

        public class Program
        {
            static void Main(string[] args)
            {
                JobHost host = new JobHost();
                host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
            }

            [NoAutomaticTrigger]
            public static void CreateQueueMessage(
                TextWriter logger,
                string value,
                [Queue("outputqueue")] out string message)
            {
                message = value;
                logger.WriteLine("Creating queue message: ", message);
            }
        }

## <a name="how-to-write-logs"></a>Günlükleri yazma
Pano günlükleri iki yerde gösterir: Web işi için sayfası ve sayfanın belli bir Web işi başlatma.

![Web işi sayfasındaki günlükleri](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

![İşlev çağırma sayfasındaki günlükleri](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

Konsol yöntemler, bir işlev çağrısı veya buna çıktısını **Main()** yöntemi görünür Web işi için Pano sayfası, sayfa belirli yöntem çağırma için içinde değil. Çıktı yöntemi imzanız bir parametresinden alma TextWriter nesneden bir yöntem çağırma için Pano sayfası görüntülenir.

Konsol birçok iş işlevlerinin aynı anda çalışabilir tek iş parçacıklı, olduğu için konsol çıktısı bir belirli yöntem çağrısının bağlanamaz. İşte bu nedenle SDK, her işlev çağrısını kendi benzersiz günlük yazıcı nesnesi ile sağlar.

Yazılacak [uygulama izleme günlükleri](../app-service/web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), kullanın **Console.Out** (bilgisi olarak işaretlenmiş günlükleri oluşturur) ve **Console.Error** (hata olarak işaretlenmiş günlükleri oluşturur). Alternatif kullanmaktır [izleme veya TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), uyarı, ayrıntı sağlar ve kritik bilgileri ve hata yanı sıra düzeyleri. Azure web uygulamanızı nasıl yapılandırdığınıza bağlı olarak Azure BLOB'veya uygulama izleme günlükleri web app günlük dosyalarında, Azure tabloları, görünür. Tüm konsol çıktısı doğru olduğundan, en son 100 uygulama günlüklerini sayfada değil bir işlev çağrısını için Web işi için Pano sayfası da görüntülenir.

Programı yerel olarak çalışmıyorsa program bir Azure WebJob içinde yalnızca çalışıyorsa, Pano veya başka bir ortamında konsol çıktısı görüntülenir.

Günlüğe kaydetme, Pano bağlantı dizesi null değerine ayarlayarak devre dışı bırakabilirsiniz. Daha fazla bilgi için bkz: [yapılandırma seçeneklerinin nasıl ayarlanacağını](#how-to-set-configuration-options).

Aşağıdaki örnek günlüklerini yazma için çeşitli yollar gösterir:

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

WebJobs SDK panosunda, çıkışı **TextWriter** zaman, belirli bir sayfaya gitmek yukarı gösterir işlev çağırma ve seçin nesnesi **geçiş çıktı**:

![Çağırma bağlantı](./media/vs-storage-webjobs-getting-started-queues/dashboardinvocations.png)

![İşlev çağırma sayfasındaki günlükleri](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

Web işi (için işlev çağrısını) için sayfaya gidin ve seçin WebJobs SDK panosunda en son 100 satır konsolunun Göster yukarı çıktı **geçiş çıktı**.

![Çıkışı Aç/Kapat](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

Bir sürekli Webjob'un uygulama günlüklerini/data/işleri/sürekli/içinde gösterilmesi *{webjobname}* web uygulama dosya sisteminde /job_log.txt.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

Bir Azure uygulama günlükleri görünümlü bu blob: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Merhaba Dünya!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Merhaba Dünya!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Merhaba Dünya!,

Ve bir Azure tablosu **Console.Out** ve **Console.Error** günlükleri şuna benzeyebilir:

![Tablo bilgi günlüğüne](./media/vs-storage-webjobs-getting-started-queues/tableinfo.png)

![Hata günlüğü tablosundaki](./media/vs-storage-webjobs-getting-started-queues/tableerror.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Azure kuyruklarla çalışmaya yönelik yaygın senaryolar nasıl ele alınacağını gösteren kod örnekleri sağlamıştır. Azure Web işleri ve WebJobs SDK nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Azure Web işleri belge kaynakları](http://go.microsoft.com/fwlink/?linkid=390226).

