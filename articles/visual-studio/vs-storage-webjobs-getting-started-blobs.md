---
title: BLOB ile çalışmaya başlama depolama ve Visual Studio bağlı hizmetler (WebJob Proje) | Microsoft Docs
description: Visual Studio kullanarak bir Azure depolama alanına bağlandıktan sonra bir Web işi projesi içinde BLOB Depolama ile çalışmaya başlamak nasıl bağlı hizmetler.
services: storage
author: ghogen
manager: douge
ms.assetid: 324c9376-0225-4092-9825-5d1bd5550058
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 5a7c16e6ac565d1660fee02cb7df178344b195e7
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122933"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Azure Blob ile çalışmaya başlama depolama ve Visual Studio bağlı hizmetler (WebJob Proje)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede, bir Azure blob'u oluşturulduğunda veya güncelleştirildiğinde bir işlemi nasıl tetikleyeceğinizi gösteren C# kod örneği sağlanmıştır. Kod örnekleri kullan [WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) sürüm 1.x. Eklediğinizde, bir depolama hesabı bir WebJob projesi için Visual Studio kullanarak **bağlı hizmet Ekle** iletişim kutusunda, uygun Azure depolama NuGet paketi yüklendi ve uygun .NET başvuruları projeye eklenir ve Depolama hesabı için bağlantı dizelerini App.config dosyasında güncelleştirilir.

## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a>Bir blob oluşturulduğunda veya bir işlev tetiklemek nasıl
Bu bölümde, nasıl kullanılacağını gösterir **BlobTrigger** özniteliği.

 **Not:** WebJobs SDK, yeni veya değiştirilmiş bloblar için izlemek için günlük dosyaları tarar. Bu işlem, doğası gereği yavaş, blob oluşturulduktan sonra bir işleve kadar birkaç dakika veya daha uzun tetiklenir değil.  BLOB'ları hemen işlemek uygulamanız gerekiyorsa, blob oluşturma ve kullanma, bir kuyruk iletisi oluşturmak için önerilen yöntem olduğu **QueueTrigger** özniteliği yerine **BlobTrigger** blob işleyen işlev üzerindeki öznitelik.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Blob adı uzantısı için tek bir yer tutucu
Aşağıdaki kod örneği görünen metin BLOB'ları kopyalar *giriş* kapsayıcıya *çıkış* kapsayıcı:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Öznitelik oluşturucusunda kapsayıcı adı ve blob adı için yer tutucu belirten bir dize parametresi alır. Bu örnekte, bir blob adlı *Blob1.txt* oluşturulur *giriş* kapsayıcı, işlev adlı bir blob oluşturur *Blob1.txt* içinde *çıkış* kapsayıcı.

Aşağıdaki kod örneğinde gösterildiği gibi blob adı yer tutucu ile adı deseni belirtebilirsiniz:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Bu kod, "orijinal-" ile başlayan adları yalnızca BLOB'ları kopyalar. Örneğin, *özgün Blob1.txt* içinde *giriş* kapsayıcı kopyalanır *kopyalama Blob1.txt* içinde *çıkış* kapsayıcı.

Bir küme ayraçlarının içinde ada sahip bir blob adları adı deseni belirtmeniz gerekiyorsa, küme ayracı çift. Örneğin, BLOB'ları bulmak istiyorsanız *görüntüleri* şöyle adlara sahip kapsayıcı:

        {20140101}-soundfile.mp3

Bu desen için kullanın:

        images/{{20140101}}-{name}

Örnekte, *adı* yer tutucu değerini olacak *soundfile.mp3*.

### <a name="separate-blob-name-and-extension-placeholders"></a>Ayrı blob adını ve uzantısını yer tutucuları
Görünen blobları kopyalar aşağıdaki kod örneği dosya uzantısını değiştiren *giriş* kapsayıcıya *çıkış* kapsayıcı. Kod uzantısı günlükleri *giriş* blob ve Dahili hat numarasını ayarlar *çıkış* blob *.txt*.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a name="types-that-you-can-bind-to-blobs"></a>Blob'lara bağlayabilirsiniz türleri
Kullanabileceğiniz **BlobTrigger** aşağıdaki türlerde özniteliği:

* **dize**
* **TextReader**
* **Stream**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Diğer türleri tarafından seri durumdan [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)

De ekleyebilirsiniz Azure depolama hesabı ile doğrudan çalışmak istiyorsanız, bir **CloudStorageAccount** parametresi metodu imzası.

## <a name="getting-text-blob-content-by-binding-to-string"></a>Dize bağlayarak metin blob içeriği alma
Metin blobları bekleniyorsa, **BlobTrigger** uygulanabilir bir **dize** parametresi. Aşağıdaki kod örneği için bir metin blob bağlayan bir **dize** adlı parametre **logMessage**. İşlev bu parametre Web işleri SDK'sı panoya blobun içeriğini yazmak için kullanır.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Blob içeriği ICloudBlobStreamBinder kullanarak seri hale alma
Aşağıdaki kod örneği, uygulayan bir sınıf kullanır **ICloudBlobStreamBinder** etkinleştirmek için **BlobTrigger** bir bloba bağlanacak öznitelik **WebImage** türü.

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK",
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

**WebImage** bağlama kodunu sağlanır bir **WebImageBinder** türetilen sınıf **ICloudBlobStreamBinder**.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input,
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="how-to-handle-poison-blobs"></a>Zehirli BLOB'ları işlemek nasıl
Olduğunda bir **BlobTrigger** işlev başarısız olursa, SDK'sı çağırır, yeniden durumda tarafından geçici bir hata nedeniyle bir hata oluştu. Blob işlemeye çalıştığında her zaman blobun içeriğini tarafından hataya neden oluyorsa, işlev başarısız olur. Varsayılan olarak, SDK'sı bir işlev en fazla 5 kez belirli bir blob için çağırır. Beşinci başarısız çalışırsanız, SDK'sı adlı bir kuyruğa bir ileti ekler *webjobs blobtrigger poison*.

Yeniden deneme sayısı yapılandırılabilir. Aynı **MaxDequeueCount** ayarı işleme zehirli blob ve kuyruk zehirli ileti işleme için kullanılır.

Kuyruk iletisi zehirli bloblar için aşağıdaki özellikleri içeren bir JSON nesnesidir:

* FunctionId (biçimde *{WebJob adı}*. İşlevler. *{İşlev adı}*, örneğin: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" veya "PageBlob")
* ContainerName
* BlobName
* ETag (örneğin, bir blob sürüm tanımlayıcısı: "0x8D1DC6E70A277EF")

Aşağıdaki kod örneğinde, **CopyBlob** işlev her çağrıldığında başarısız olmasına neden koduna sahip. Sonra en fazla yeniden deneme sayısı için çağırdığı SDK'sı, zehirli blob kuyrukta bir ileti oluşturulur ve bu iletiyi tarafından işlenen **LogPoisonBlob** işlevi.

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }

        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

SDK otomatik olarak JSON iletisi seri durumdan çıkarır. İşte **PoisonBlobMessage** sınıfı:

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>BLOB yoklama algoritması
Web işleri SDK'sı tarafından belirtilen tüm kapsayıcıları tarar **BlobTrigger** uygulama başlangıcında öznitelikleri. Yeni BLOB'ları bulunan önce biraz kalmış olabilir bir büyük depolama hesabında Bu tarama biraz zaman alabilir ve **BlobTrigger** işlevleri yürütülür.

SDK'sı, uygulama başladıktan sonra yeni veya değiştirilmiş blobları algılamak için blob depolama günlüklerinden düzenli olarak okur. Blob günlükleri arabelleğe alınmış ve yalnızca fiziksel olarak 10 dakikada yazılan veya bu nedenle, bu nedenle önemli gecikme olabilir bir blob oluşturulur veya karşılık gelen önce güncelleştirilir sonra **BlobTrigger** işlevi yürütür.

Kullanarak oluşturduğunuz BLOB'ları için bir özel durum **Blob** özniteliği. WebJobs SDK, yeni bir blob oluşturduğunda, yeni blob hemen eşleşen tüm arabimini **BlobTrigger** işlevleri. Blob giriş ve çıkışları zinciri varsa, bu nedenle SDK verimli bir şekilde işleyebilir. Oluşturulan veya başka yollarla güncelleştirilmiş bloblar için işleme işlevleri blobunuza çalışan düşük gecikme süresi istiyorsanız kullanmanızı öneririz, ancak **QueueTrigger** yerine **BlobTrigger**.

### <a name="blob-receipts"></a>BLOB giriş
Xenapp'i WebJobs SDK hiçbir **BlobTrigger** işlevi aynı yeni veya güncelleştirilmiş blob için birden çok kez çağrıldığından. Bunu tutarak yapar *blob giriş* belirtilen blob sürüm işlenen belirlemek için.

BLOB giriş adlı bir kapsayıcıda depolanan *azure webjobs konakları* AzureWebJobsStorage bağlantı dizesi tarafından belirtilen Azure depolama hesabında. Bir blob giriş bölümünde aşağıdaki bilgiler bulunur:

* Blob için çağrılan işlev ("*{WebJob adı}*. İşlevler. *{İşlev adı}*", örneğin: "WebJob1.Functions.CopyBlob")
* Kapsayıcı adı
* Blob türü ("BlockBlob" veya "PageBlob")
* Blob adı
* ETag (örneğin, bir blob sürüm tanımlayıcısı: "0x8D1DC6E70A277EF")

Bir blobu yeniden işlemeyerek zorlamak istiyorsanız, o blobu için blob giriş el ile silebilirsiniz *azure webjobs konakları* kapsayıcı.

## <a name="related-topics-covered-by-the-queues-article"></a>Kuyruklar makalenin konusu ilgili konular
İşleme, blob değil belirli senaryoları WebJobs SDK veya bir kuyruk iletisi tarafından tetiklenen blob işlem hakkında bilgi için bkz [WebJobs SDK ile Azure kuyruk depolama kullanma](https://github.com/Azure/azure-webjobs-sdk/wiki).

Bu makalede ele alınan ilgili konular şunlardır:

* Zaman uyumsuz işlevleri
* Birden çok örnek
* Normal şekilde kapatılmasını
* WebJobs SDK öznitelikleri bir işlevin gövdesinde kullanın
* SDK bağlantı dizeleri kod içinde ayarlayabilirsiniz.
* Değerleri Oluşturucu parametresi için Web işleri SDK'sı, kod içinde ayarlayabilirsiniz.
* Yapılandırma **MaxDequeueCount** zehirli blob işleme.
* Bir işlev el ile tetikleme
* Günlükler Yazar

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure BLOB'ları ile çalışmak için ortak senaryolar nasıl ele alınacağını gösteren kod örnekleri sağlamıştır. Azure WebJobs ve WebJobs SDK'sı kullanma hakkında daha fazla bilgi için bkz. [Azure WebJobs belgeleri kaynakları](https://go.microsoft.com/fwlink/?linkid=390226).

