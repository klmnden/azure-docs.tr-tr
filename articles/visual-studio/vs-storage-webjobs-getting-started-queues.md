---
title: Bağlı hizmetler (WebJob Proje) kuyruk depolama ve Visual Studio ile çalışmaya başlama | Microsoft Docs
description: Visual Studio kullanarak bir depolama hesabına bağlandıktan sonra bir Web işi projesi içinde Azure kuyruk depolama ile çalışmaya başlamak nasıl bağlı hizmetler.
services: storage
author: ghogen
manager: douge
ms.assetid: 5c3ef267-2a67-44e9-ab4a-1edd7015034f
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: article
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: f6f1a3a7f0a406e1dbb40f4bfc6a358da7ac68fa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60391232"
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-webjob-projects"></a>Bağlı hizmetler (WebJob Proje) Azure kuyruk depolama ve Visual Studio ile çalışmaya başlama
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede nasıl oluşturduğunuz veya Visual Studio kullanarak bir Azure depolama hesabına başvurulan sonra bir Visual Studio Azure WebJob proje Azure kuyruk depolama kullanmaya başlama **bağlı hizmet Ekle** iletişim kutusu. Eklediğinizde, bir depolama hesabı bir WebJob projesi için Visual Studio kullanarak **bağlı hizmet Ekle** iletişim kutusunda, uygun Azure depolama NuGet paketleri yüklendi, uygun .NET başvuruları projeye eklenir ve Depolama hesabı için bağlantı dizelerini App.config dosyasında güncelleştirilir.  

Bu makalede, Azure Web işleri SDK'sı sürümünü kullanmayı gösteren C# kod örneği sağlanmıştır 1.x ile Azure kuyruk depolama hizmeti.

Azure Kuyruk depolama, HTTP veya HTTPS kullanan kimlik doğrulaması yapılmış çağrılar aracılığıyla dünyanın her yerinden erişilebilen çok sayıda iletinin depolanması için bir hizmettir. Tek bir kuyruk iletisinin boyutu 64 KB’ye kadar olabilir ve bir kuyrukta, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti bulunabilir. Bkz: [.NET kullanarak Azure kuyruk depolama ile çalışmaya başlama](../storage/queues/storage-dotnet-how-to-use-queues.md) daha fazla bilgi için. ASP.NET hakkında daha fazla bilgi için bkz: [ASP.NET](https://www.asp.net).

## <a name="how-to-trigger-a-function-when-a-queue-message-is-received"></a>Bir kuyruk iletisi alındığında bir işlev tetiklemek nasıl
Bir kuyruk iletisi alındığında Web işleri SDK'sı çağıran bir işlev yazmak için kullanın **QueueTrigger** özniteliği. Öznitelik oluşturucusunda yoklamak için kuyruk adını belirten bir dize parametresi alır. Kuyruk adı dinamik olarak ayarlama görmek için kullanıma [yapılandırma seçeneklerini ayarlama](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>Dize kuyruk iletileri
Bir dize iletisi sırası aşağıdaki örnekte, bu nedenle içerir **QueueTrigger** adlı bir dize parametresine uygulanan **logMessage** kuyruk iletisinin içeriği içerir. İşlev [Panoya bir günlük iletisi Yazar](#how-to-write-logs).

```csharp
public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
{
    logger.WriteLine(logMessage);
}
```

Yanında **dize**, parametre bir bayt dizisi olabilir bir **CloudQueueMessage** nesne ya da tanımladığınız bir POCO.

### <a name="poco-plain-old-clr-objecthttpsenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(düz eski CLR nesne](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) kuyruk iletileri
Aşağıdaki örnekte, kuyruk iletisi için JSON içeriyor. bir **BlobInformation** içeren bir nesne bir **BlobName** özelliği. SDK otomatik olarak nesne seri durumdan çıkarır.

```csharp
public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
{
    logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
}
```

SDK'sı kullanır [Newtonsoft.Json NuGet paketini](https://www.nuget.org/packages/Newtonsoft.Json) seri hale getirmek ve seri durumdan iletileri. WebJobs SDK kullanmayan bir programda kuyruk iletileri oluşturursanız, SDK'sı ayrıştırabilen POCO kuyruk iletisi oluşturmak için aşağıdaki örnekte olduğu gibi kod yazabilirsiniz.

```csharp
BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
logQueue.AddMessage(queueMessage);
```

### <a name="async-functions"></a>Zaman uyumsuz işlevleri
Aşağıdaki zaman uyumsuz işlev [günlüğü panoya Yazar](#how-to-write-logs).

```csharp
public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
{
    await logger.WriteLineAsync(logMessage);
}
```

Zaman uyumsuz işlevleri uzun bir [iptal belirteci](https://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken)bir blobu kopyalar aşağıdaki örnekte gösterildiği gibi. (Bir açıklaması için **queueTrigger** yer tutucu bkz [Blobları](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) bölümüne.)

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
    [QueueTrigger("blobcopyqueue")] string blobName,
    [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
    [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
    CancellationToken token)
{
    await blobInput.CopyToAsync(blobOutput, 4096, token);
}
```

## <a name="types-the-queuetrigger-attribute-works-with"></a>Türleri QueueTrigger özniteliği ile çalışır.
Kullanabileceğiniz **QueueTrigger** şu türden:

* **dize**
* JSON olarak serileştirilen bir POCO türü
* **bayt]**
* **CloudQueueMessage**

## <a name="polling-algorithm"></a>Yoklama algoritması
SDK'sı boş kuyruk üzerinde depolama işlem maliyetleri yoklama etkisini azaltmak için bir rastgele üstel geri alma algoritması uygular.  Bir ileti bulunduğunda, SDK'sı iki saniye bekler ve ardından başka bir ileti için denetler; ileti bulunduğu zaman yeniden denemeden önce yaklaşık dört saniye bekler. Bir kuyruk iletisi almak için sonraki başarısız girişimden sonra bekleme süresini bir dakika için varsayılan olarak en fazla bekleme zamanı ulaşıncaya kadar artmaya devam eder. [En fazla bekleme zamanı yapılandırılabilirdir](#how-to-set-configuration-options).

## <a name="multiple-instances"></a>Birden çok örnek
Web uygulamanız birden çok örnek üzerinde çalışıyorsa, sürekli WebJobs her makinede çalışır ve her makine için Tetikleyiciler bekleyin ve işlevleri çalıştırmayı deneyin. Bazı senaryolarda bu bazı işlevler aynı verileri iki kez işlemesine yol açabilir, bu nedenle işlevleri (böylece yinelenen sonuçlar bunları tekrar tekrar aynı girdi verileriyle çağırma üretemez yazılmış) bir kez etkili olmalıdır.  

## <a name="parallel-execution"></a>Paralel yürütme
Farklı kuyrukları dinleyen birden çok işlevi varsa, iletileri eşzamanlı olarak alındığında SDK bunları paralel olarak çağırır.

Tek bir kuyruk için birden çok ileti aldığında, aynı durum geçerlidir. Varsayılan olarak, SDK 16 kuyruk iletileri toplu bir zaman alır ve bunları paralel olarak işleyen işlevi yürütür. [Toplu iş boyutu yapılandırılabilir](#how-to-set-configuration-options). İşlenen numarası toplu iş boyutu yarısını aldığında, SDK başka bir toplu iş alır ve bu iletileri işlemeye başlıyor. Bu nedenle en fazla eş zamanlı iletileri işlev işlenen bir ve yarı kez bir toplu iş boyutu sayısıdır. Bu sınır olan her işlev için ayrı ayrı uygulanır. bir **QueueTrigger** özniteliği. Paralel yürütme bir kuyruğa alınan iletileri istemiyorsanız, toplu iş boyutu 1 olarak ayarlayın.

## <a name="get-queue-or-queue-message-metadata"></a>Kuyruğu veya kuyruk iletisi meta verilerini al
Yöntem imzası için parametreleri ekleyerek, aşağıdaki ileti özellikleri alabilirsiniz:

* **DateTimeOffset** expirationTime
* **DateTimeOffset** insertionTime
* **DateTimeOffset** nextVisibleTime
* **dize** queueTrigger (ileti metni içerir)
* **dize** kimliği
* **dize** popReceipt
* **int** dequeueCount

De ekleyebilirsiniz Azure depolama ile doğrudan API çalışmak istiyorsanız, bir **CloudStorageAccount** parametresi.

Aşağıdaki örnek, tüm bu meta veri bilgileri uygulama günlüğüne yazar. Bu örnekte, kuyruk iletisinin içeriği logMessage hem queueTrigger içerir.

```csharp
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
```

Örnek kodu ile yazılmış bir örnek günlük şu şekildedir:

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

## <a name="graceful-shutdown"></a>Normal şekilde kapatılmasını
Sürekli bir WebJob içinde çalışan bir işlev kabul edebilen bir **CancellationToken** WebJob sona erdirilecek olduğunda işlevi bildirmek işletim sistemi sağlayan parametresi. Bu bildirim, işlev beklenmedik bir şekilde verileri tutarsız bir durumda bırakır şekilde sonlandırmaz emin olmak için kullanabilirsiniz.

Aşağıdaki örnek, bir işlevde yaklaşan WebJob sonlandırma olup olmadığını denetlemek gösterilmektedir.

```csharp
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
```

**Not:** Pano, durumunu ve kapatıldığından işlevler çıkışını doğru şekilde göstermeyebilir.

Daha fazla bilgi için [WebJobs kapatılmasını](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).   

## <a name="how-to-create-a-queue-message-while-processing-a-queue-message"></a>Nasıl bir kuyruk iletisi işlenirken bir kuyruk iletisi oluşturmak için
Yeni bir kuyruk iletisi oluşturan bir işlev yazmak için kullanın **kuyruk** özniteliği. Gibi **QueueTrigger**, kuyruk adı bir dize olarak geçirin veya yapabilecekleriniz [kuyruk adı dinamik olarak ayarlama](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>Dize kuyruk iletileri
Aşağıdaki zaman uyumsuz olmayan kod örneği, sırasındaki "inputqueue" adlı sıraya alınan kuyruk iletisi olarak aynı içeriğe sahip "outputqueue" adlı yeni bir kuyruk iletisi oluşturur. (İçin zaman uyumsuz işlevleri kullanmak **IAsyncCollector<T>**  daha sonra bu bölümde gösterilen.)

```csharp
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] string queueMessage,
    [Queue("outputqueue")] out string outputQueueMessage )
{
    outputQueueMessage = queueMessage;
}
```

### <a name="poco-plain-old-clr-objecthttpsenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(düz eski CLR nesne](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) kuyruk iletileri
Bir dize yerine bir POCO içeren bir kuyruk iletisi oluşturmak için bir çıktı parametresi olarak POCO türü geçirin **kuyruk** öznitelik Oluşturucusu.

```csharp
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
    [Queue("outputqueue")] out BlobInformation blobInfoOutput )
{
    blobInfoOutput = blobInfoInput;
}
```

SDK otomatik olarak JSON nesnesini serileştirir. Nesne null olsa bile bir kuyruk iletisi her zaman oluşturulur.

### <a name="create-multiple-messages-or-in-async-functions"></a>Birden çok ileti oluşturmak veya zaman uyumsuz işlevleri
Birden çok ileti oluşturmak için çıkış kuyruğuna için parametre türü olun **ICollector<T>**  veya **IAsyncCollector<T>** aşağıdaki örnekte gösterildiği gibi.

```csharp
public static void CreateQueueMessages(
    [QueueTrigger("inputqueue")] string queueMessage,
    [Queue("outputqueue")] ICollector<string> outputQueueMessage,
    TextWriter logger)
{
    logger.WriteLine("Creating 2 messages in outputqueue");
    outputQueueMessage.Add(queueMessage + "1");
    outputQueueMessage.Add(queueMessage + "2");
}
```

Her kuyruk iletisi hemen oluşturulur, **Ekle** yöntemi çağrılır.

### <a name="types-that-the-queue-attribute-works-with"></a>Kuyruk öznitelik çalışır türleri
Kullanabileceğiniz **kuyruk** özniteliği aşağıdaki parametre türleri:

* **Çıkış dizesi** (parametre değeri null olmayan ise işlev sona erdiğinde kuyruk iletisi oluşturur)
* **bayt [] kullanıma** (gibi çalışır **dize**)
* **CloudQueueMessage kullanıma** (gibi çalışır **dize**)
* **POCO kullanıma** (serializable bir tür oluşturduğu bir ileti ile bir null Nesne işlev sona erdiğinde parametre null ise)
* **ICollector**
* **IAsyncCollector**
* **CloudQueue** (için el ile Azure depolama API kullanarak doğrudan ileti oluşturma)

### <a name="use-webjobs-sdk-attributes-in-the-body-of-a-function"></a>WebJobs SDK öznitelikleri bir işlevin gövdesinde kullanın
WebJobs SDK öznitelik gibi kullanmadan önce bazı çalışma işlevinizde yapmanız gerekiyorsa **kuyruk**, **Blob**, veya **tablo**, kullanabileceğiniz **IBinder**arabirimi.

Aşağıdaki örnek, bir giriş sırası iletiyi alır ve bir çıkış sırasına aynı içeriğe sahip yeni bir ileti oluşturur. Çıkış kuyruğu adı işlevinin gövdesindeki kod tarafından ayarlanır.

```csharp
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] string queueMessage,
    IBinder binder)
{
    string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
    QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
    CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
    outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
}
```

**IBinder** arabirimi de kullanılabilir olan **tablo** ve **Blob** öznitelikleri.

## <a name="how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message"></a>Okuma ve yazma BLOB ve kuyruk iletisi işlenirken tablolara
**Blob** ve **tablo** öznitelikleri BLOB'lar ve tablolar okumasına ve yazmasına olanak tanır. Bu bölümdeki örnekler, BLOB'ları için geçerlidir. BLOB'ları oluşturulduğunda veya güncelleştirildiğinde, işlemleri tetiklemek nasıl gösteren kod örnekleri için bkz. [WebJobs SDK ile Azure blob depolama kullanma](https://github.com/Azure/azure-webjobs-sdk/wiki).
<!-- , and for code samples that read and write tables, see [How to use Azure table storage with the WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md). -->

### <a name="string-queue-messages-triggering-blob-operations"></a>Dize kuyruk iletileri BLOB işlemleri tetikleme
Bir dize içeren bir kuyruk iletisi için **queueTrigger** kullanabileceğiniz bir yer tutucudur **Blob** özniteliğin **blobPath** içeriğini içeren bir parametre İleti.

Aşağıdaki örnekte **Stream** okuma ve yazma blobları nesneleri. Kuyruk iletisi textblobs kapsayıcıda bulunan blobların addır. Blob kopyası "-Yeni" eklenecek ad aynı kapsayıcıda oluşturulur.

```csharp
public static void ProcessQueueMessage(
    [QueueTrigger("blobcopyqueue")] string blobName,
    [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
    [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
{
    blobInput.CopyTo(blobOutput, 4096);
}
```

**Blob** özniteliği Oluşturucu alır bir **blobPath** parametresi kapsayıcı ve blob adını belirtir. Bu yer tutucu hakkında daha fazla bilgi için bkz: [WebJobs SDK ile Azure blob depolama kullanma](https://github.com/Azure/azure-webjobs-sdk/wiki).

Ne zaman öznitelik düzenler bir **Stream** nesnesinin, başka bir oluşturucu parametresi belirtir **FileAccess** modu okuma, yazma veya okuma/yazma olarak.

Aşağıdaki örnekte bir **CloudBlockBlob** bir blobun silinmesi için nesne. Kuyruk iletisi blob adıdır.

```csharp
public static void DeleteBlob(
    [QueueTrigger("deleteblobqueue")] string blobName,
    [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
{
    blobToDelete.Delete();
}
```

### <a name="poco-plain-old-clr-objecthttpsenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(düz eski CLR nesne](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) kuyruk iletileri
Kuyruk iletisinde JSON olarak depolanır ve bir POCO için özellikleri nesnesinin adı yer tutucuları kullanabilirsiniz **kuyruk** özniteliğin **blobPath** parametresi. Kuyruk meta veri özellik adlarının yer tutucu olarak kullanabilirsiniz. Bkz: [kuyruğu veya kuyruk iletisi meta verileri alma](#get-queue-or-queue-message-metadata).

Aşağıdaki örnek, farklı bir uzantıya sahip yeni bir blob için bir blobu kopyalar. Kuyruk iletisi bir **BlobInformation** içeren nesne **BlobName** ve **BlobNameWithoutExtension** özellikleri. Blob yolu için yer tutucu olarak kullanılan özellik adları **Blob** öznitelikleri.

```csharp
public static void CopyBlobPOCO(
    [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
    [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
    [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
{
    blobInput.CopyTo(blobOutput, 4096);
}
```

SDK'sı kullanır [Newtonsoft.Json NuGet paketini](https://www.nuget.org/packages/Newtonsoft.Json) seri hale getirmek ve seri durumdan iletileri. WebJobs SDK kullanmayan bir programda kuyruk iletileri oluşturursanız, SDK'sı ayrıştırabilen POCO kuyruk iletisi oluşturmak için aşağıdaki örnekte olduğu gibi kod yazabilirsiniz.

```csharp
BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
logQueue.AddMessage(queueMessage);
```

Bir blob için bir nesne bağlama önce bazı çalışma işlevinizde yapmanız gerekiyorsa, gösterildiği gibi işlev gövdesinde özniteliği kullanabilirsiniz [kullanan Web işleri SDK'sı öznitelikleri bir işlevin gövdesinde](#use-webjobs-sdk-attributes-in-the-body-of-a-function).

### <a name="types-you-can-use-the-blob-attribute-with"></a>Blob özniteliği ile kullanabileceğiniz türleri
**Blob** özniteliği aşağıdaki türleriyle kullanılabilir:

* **Stream** (okuma veya yazma FileAccess Oluşturucu parametresi kullanılarak belirtilen)
* **TextReader**
* **TextWriter**
* **dize** (okuma)
* **Çıkış dizesi** (yazma; yalnızca dize parametresi null olmayan ise işlevi döndüğünde, bir blob oluşturur)
* POCO (okuma)
* POCO out (yazma; her zaman bir blob oluşturur, işlevi döndüğünde POCO parametre null ise null Nesne olarak oluşturur.)
* **CloudBlobStream** (yazma)
* **ICloudBlob** (okuma veya yazma)
* **CloudBlockBlob** (okuma veya yazma)
* **CloudPageBlob** (okuma veya yazma)

## <a name="how-to-handle-poison-messages"></a>Zehirli iletilerin nasıl ele alınacağını
İçeriğe sahip bir işlev başarısız olmasına neden olan iletileri çağrılır *zehirli iletiler*. İşlev başarısız olursa, kuyruk iletisi silinmez ve sonunda yeniden yinelenmesi için döngüyü neden seçilir. SDK otomatik olarak sınırlı sayıda yinelemeden sonra döngü engelleyebilecek veya el ile yapabilirsiniz.

### <a name="automatic-poison-message-handling"></a>Otomatik zehirli ileti işleme
SDK'sı bir kuyruk iletisi işlemek için 5 kata kadar bir işlevi çağırır. Beşinci deneme başarısız olursa, zehirli bir kuyruğa ileti taşınır. En fazla yeniden deneme sayısı yapılandırma gördüğünüz [yapılandırma seçeneklerini ayarlama](#how-to-set-configuration-options).

Zehirli sıranın adlı *{originalqueuename}*-zehirli. İşlem iletileri bir işleve zehirli kuyruktan günlüğe yazma ya da bir bildirim göndererek el ile ilgili dikkat edilmesi gereken yazabilirsiniz.

Aşağıdaki örnekte **CopyBlob** bir kuyruk iletisi mevcut olmayan bir blobun adını içerdiğinde işlevi başarısız olur. Bu durum oluştuğunda yapılacak copyblobqueue poison kuyruğa copyblobqueue kuyruktan taşınır. **ProcessPoisonMessage** ardından zehirli ileti günlüğe kaydeder.

```csharp
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
```

Zehirli ileti işlendikten sonra aşağıdaki resimde bu işlevler konsol çıktısı gösterir.

![Zehirli ileti işleme için konsol çıktısı](./media/vs-storage-webjobs-getting-started-queues/poison.png)

### <a name="manual-poison-message-handling"></a>El ile zehirli ileti işleme
Bir ileti toplanmış kaç kez işlenmek ekleyerek alabilirsiniz bir **int** adlı parametre **dequeueCount** işlevinize. Ardından, işlev kodunu sıradan çıkarma sayısı denetleyin ve sayısı bir eşiği aştığında, kendi zehirli ileti aşağıdaki örnekte gösterildiği gibi işleme gerçekleştirin.

```csharp
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
```

## <a name="how-to-set-configuration-options"></a>Yapılandırma seçeneklerini ayarlama
Kullanabileceğiniz **JobHostConfiguration** türü aşağıdaki yapılandırma seçeneklerini ayarlamak için:

* SDK bağlantı dizeleri kod içinde ayarlayabilirsiniz.
* Yapılandırma **QueueTrigger** maksimum gibi ayarları sıradan çıkarma sayısı.
* Kuyruk adları yapılandırmasından edinin.

### <a name="set-sdk-connection-strings-in-code"></a>Kodda SDK bağlantı dizelerini ayarlama
Kodda SDK bağlantı dizelerini ayarlama, yapılandırma dosyalarının veya ortam değişkenlerini kendi bağlantı dizesi adları kullanmak aşağıdaki örnekte gösterildiği gibi sağlar.

```csharp
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
```

### <a name="configure-queuetrigger--settings"></a>QueueTrigger ayarlarını yapılandırma
Kuyruk ileti işleme için geçerli olan aşağıdaki ayarları yapılandırabilirsiniz:

* Aynı anda paralel olarak yürütülecek alınacağı kuyruk iletileri sayısı (varsayılan değer 16).
* Bir kuyruk iletisi zehirli kuyruğa gönderilmeden önce yeniden deneme sayısı (varsayılan değer 5).
* Maksimum bekleme süresi bir kuyruğu boş olduğunda tekrar yoklama önce (varsayılan değer 1 dakika).

Aşağıdaki örnek, bu ayarların nasıl yapılandırılacağı gösterilmektedir:

```csharp
static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.Queues.BatchSize = 8;
    config.Queues.MaxDequeueCount = 4;
    config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="set-values-for-webjobs-sdk-constructor-parameters-in-code"></a>Değerleri Oluşturucu parametresi için Web işleri SDK'sı, kod içinde ayarlayabilirsiniz.
Bazı durumlarda bir kuyruk adı, bir blob adı veya kapsayıcı belirtmek istediğiniz veya bir tablo adı: sabit kodlamak yerine kod bu. Örneğin, kuyruk adı belirtmek isteyebilirsiniz **QueueTrigger** bir yapılandırma dosyası veya ortam değişkeni içinde.

Geçirerek bunu yapabilirsiniz bir **NameResolver** nesnesini **JobHostConfiguration** türü. Yüzde (%) tarafından çevrilmiş özel yer tutucu karakterleri içeren WebJobs SDK özniteliği Oluşturucu parametrelerinde imzalar ve **NameResolver** kodu, bu yer tutucular yerine kullanılacak gerçek değerleri belirtir.

Örneğin, test ortamında logqueuetest ve üretimde bir adlandırılmış logqueueprod adlı bir sıra kullanmak istediğiniz varsayalım. Bir giriş adını belirtmek istediğiniz bir sabit kodlanmış kuyruk adı yerine **appSettings** gerçek kuyruk adı olması gereken bir koleksiyon. Varsa **appSettings** anahtar logqueue, işlevinizi aşağıdaki örnekteki gibi görünebilir.

```csharp
public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
{
    Console.WriteLine(logMessage);
}
```

**NameResolver** sınıfı kuyruk adından sonra alabilir **appSettings** aşağıdaki örnekte gösterildiği gibi:

```csharp
public class QueueNameResolver : INameResolver
{
    public string Resolve(string name)
    {
        return ConfigurationManager.AppSettings[name].ToString();
    }
}
```

Geçirdiğiniz **NameResolver** için sınıfını **JobHost** nesne aşağıdaki örnekte gösterildiği gibi.

```csharp
static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.NameResolver = new QueueNameResolver();
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

**Not:** Kuyruk, tablo ve blob adları, her bir işlev çağrılır, ancak yalnızca uygulama başladığında blob kapsayıcı adları çözümlenir çözümlenir. İş çalışırken blob kapsayıcı adı değiştirilemiyor.

## <a name="how-to-trigger-a-function-manually"></a>Bir işlev el ile tetikleme
Bir işlev el ile tetiklemek için kullanmak **çağrı** veya **CallAsync** metodunda **JobHost** nesne ve **NoAutomaticTrigger** Aşağıdaki örnekte gösterildiği gibi işlev özniteliği.

```csharp
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
```

## <a name="how-to-write-logs"></a>Günlükleri yazma
Günlükleri iki yerde gösteren panoyu: sayfa WebJob için ve belirli bir WebJob çağrısına sayfası.

![WebJob sayfasında günlükleri](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

![İşlev çağırma sayfasında günlükleri](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

Konsol yöntemleri, bir işlev çağrısında veya buna çıktısını **Main()** yöntemi, WebJob için Pano sayfası, sayfayı belirli yöntem çağırma için değil, görünür. Çıkış bir parametresi, Yöntem imzasında alma TextWriter nesnedeki bir yöntem çağırma için Pano sayfası görüntülenir.

Aynı anda birçok iş işlevlerini çalışabilir ancak tek iş parçacıklı, konsolu olduğu için konsol çıktısı bir belirli yöntem çağırma için bağlanamaz. İşte bu nedenle kendi benzersiz günlük yazıcı nesnesi ile her bir işlevi çağırmayı SDK sağlar.

Yazılacak [uygulama izleme günlükleri](../app-service/troubleshoot-dotnet-visual-studio.md#logsoverview), kullanın **Console.Out** (bilgisi olarak işaretlenmiş günlükleri oluşturur) ve **Console.Error dosyası** (hata olarak işaretlenmiş günlükleri oluşturur). Kullanmaya alternatiftir [izleme veya TraceSource](https://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), uyarı, ayrıntı, sağlar ve kritik bilgileri ve hata yanı sıra düzeyleri. Uygulama izleme günlükleri web uygulaması günlük dosyalarında, Azure tabloları, görünür veya Azure web uygulamanıza nasıl yapılandırdığınıza bağlı olarak Azure blobları. Tüm konsol çıkışını true olduğu gibi en son 100 uygulama günlüklerini sayfa işlevi çağırma için değil WebJob için Pano sayfasında da görünür.

Konsol çıktısı, programın yerel olarak çalışmıyorsa yalnızca program bir Azure WebJob içinde çalışıyorsa bu Panoda veya başka bir ortama görünür.

Pano bağlantı dizesi null olarak ayarlayarak günlüğü devre dışı bırakabilirsiniz. Daha fazla bilgi için [yapılandırma seçeneklerini ayarlama](#how-to-set-configuration-options).

Aşağıdaki örnek, günlükleri yazmak için birçok yol gösterir:

```csharp
public static void WriteLog(
    [QueueTrigger("logqueue")] string logMessage,
    TextWriter logger)
{
    Console.WriteLine("Console.Write - " + logMessage);
    Console.Out.WriteLine("Console.Out - " + logMessage);
    Console.Error.WriteLine("Console.Error - " + logMessage);
    logger.WriteLine("TextWriter - " + logMessage);
}
```

WebJobs SDK panosunda çıktısı **TextWriter** ne zaman, belirli bir sayfaya gidin yukarı gösterir işlev çağırma ve seçin nesnesi **çıkışı Aç/Kapat**:

![Çağırma bağlantısı](./media/vs-storage-webjobs-getting-started-queues/dashboardinvocations.png)

![İşlev çağırma sayfasında günlükleri](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

(İçin işlev çağrısını) WebJob için sayfasına gidin ve seçin Web işleri SDK'sı panosunda Konsolu en son 100 satırlarını göster yukarı çıkış **çıkışı Aç/Kapat**.

![Çıkışı Aç/Kapat](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

Sürekli bir WebJob uygulama günlükleri/data/iş/sürekli/içinde gösterilmesi *{webjobname}* web uygulaması dosya sistemindeki /job_log.txt.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

Uygulama günlükleri görünümü şunun gibi bir Azure blob: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Merhaba Dünya!, 2014-09-26T21 : 01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Merhaba Dünya!,

Bir Azure tablosu ve **Console.Out** ve **Console.Error dosyası** günlükleri şuna benzeyebilir:

![Tablo bilgisi günlüğünde](./media/vs-storage-webjobs-getting-started-queues/tableinfo.png)

![Tabloda hata günlüğü](./media/vs-storage-webjobs-getting-started-queues/tableerror.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure kuyrukları ile çalışmaya yönelik yaygın senaryolar nasıl ele alınacağını gösteren kod örnekleri sağlamıştır. Azure WebJobs ve WebJobs SDK'sı kullanma hakkında daha fazla bilgi için bkz. [Azure WebJobs belgeleri kaynakları](https://go.microsoft.com/fwlink/?linkid=390226).

