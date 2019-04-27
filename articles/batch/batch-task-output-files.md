---
title: Batch hizmeti API'si - Azure Batch ile Azure depolama için iş ve görev çıktılarını kalıcı hale getirme | Microsoft Docs
description: Toplu görev ve iş çıkışı Azure Depolama'da kalıcı hale getirme için Batch hizmeti API'sini kullanmayı öğrenin.
services: batch
author: laurenhughes
manager: jeconnoc
editor: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 03/05/2019
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 1d2d53213af34377d23c9ea140bab15822fc1b2e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60554720"
---
# <a name="persist-task-data-to-azure-storage-with-the-batch-service-api"></a>Batch hizmeti API'si ile Azure depolama için görev verileri kalıcı hale

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Batch hizmeti API'si, görevleri ve havuzları ile sanal makine yapılandırması üzerinde çalışan iş yöneticisi görevleri için çıktı verilerini Azure Depolama'da kalıcı destekler. Bir görev eklediğinizde, Azure Depolama'da kapsayıcı görev çıkışı için hedef olarak belirtebilirsiniz. Görev tamamlandığında, Batch hizmeti herhangi bir çıktı veri ardından kapsayıcıya yazar.

Görev çıktısını kalıcı hale getirmek için Batch hizmeti API'sini kullanmanın bir avantajı, görevin çalıştığı uygulama değiştirmek gerekmez ' dir. Bunun yerine, istemci uygulamanız için bazı değişikliklerle, görev çıktısı görevi oluşturan aynı kodu devam edebilir.

## <a name="when-do-i-use-the-batch-service-api-to-persist-task-output"></a>Batch hizmeti API'si görev çıktısını kalıcı hale getirmek için ne zaman kullanırım?

Azure Batch, görev çıktısını kalıcı hale getirmek için birden fazla yol sağlar. Batch hizmeti API'si kullanılarak en uygun kullanışlı bir yaklaşım için bu senaryolar verilmiştir:

- Görev çıktısı, istemci uygulama, görevin çalıştığı uygulama değiştirmeden kalıcı hale getirmek için kod yazmak istediğiniz.
- Toplu iş görevleri ve iş yöneticisi görevleri sanal makine yapılandırmasıyla oluşturulan havuzlarda çıktısını kalıcı hale getirmek istediğiniz.
- Rastgele bir ada sahip bir Azure depolama kapsayıcısı çıkışı kalıcı hale getirmek istediğiniz.
- Çıkış göre adlı bir Azure depolama kapsayıcısına kalıcı hale getirmek istediğiniz [Batch dosya kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/Batch/Support/FileConventions#conventions). 

Senaryonuz yukarıda listelenenlerden farklıysa, farklı bir yaklaşım dikkate almanız gerekebilir. Örneğin, Görev yürütülürken Batch hizmeti API'si akış çıkışı Azure depolama şu anda desteklemiyor. Çıkış akış için kullanılabilen .NET için Batch dosya kuralları kitaplığı kullanmayı düşünün. Diğer diller için kendi çözümünüzü uygulamak gerekir. Görev çıktısını kalıcı hale getirme için diğer seçenekler hakkında daha fazla bilgi için bkz. [Azure Depolama'ya iş ve görev çıktılarını kalıcı hale getirme](batch-task-output.md).

## <a name="create-a-container-in-azure-storage"></a>Azure Depolama'da kapsayıcı oluşturma

Azure depolama için görev çıktısını kalıcı hale getirmek için çıkış dosyaları için hedef görevi gören bir kapsayıcı oluşturmanız gerekir. İşinizi göndermeden önce göreviniz, tercihen çalıştırmadan önce bir kapsayıcı oluşturun. Kapsayıcı oluşturmak için uygun Azure depolama istemci kitaplığı veya SDK'sını kullanın. Azure depolama API'leri hakkında daha fazla bilgi için bkz. [Azure depolama belgeleri](https://docs.microsoft.com/azure/storage/).

Örneğin, uygulamanızın C# ' ta yazıyorsanız, kullanın [.NET için Azure depolama istemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/). Aşağıdaki örnek, bir kapsayıcı oluşturma işlemi gösterilmektedir:

```csharp
CloudBlobContainer container = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
await container.CreateIfNotExists();
```

## <a name="get-a-shared-access-signature-for-the-container"></a>İçin kapsayıcı paylaşılan erişim imzası Al

Kapsayıcıyı oluşturduktan sonra kapsayıcıya yazma erişimi olan bir paylaşılan erişim imzası (SAS) alın. Bir SAS kapsayıcısına temsilci erişimi sağlar. SAS izinleri ve belirtilen zaman aralığı boyunca belirtilen bir kümesiyle erişim verir. Batch hizmeti görev çıkışını kapsayıcıya yazmak için yazma izinlerine sahip bir SAS gerekir. SAS hakkında daha fazla bilgi için bkz: [paylaşılan erişim imzaları kullanma \(SAS\) Azure depolamadaki](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

Azure Depolama API'lerini kullanarak bir SAS aldığınızda, API, bir SAS belirteci dizesi döndürür. SAS izinleri ve SAS geçerli aralığına dahil olmak üzere, tüm parametrelerinin bu belirteç dizesini içerir. Azure depolamadaki bir kapsayıcıda erişmek için SAS'ı kullanmak için Kaynak URI için SAS belirteci dizesi eklemek gerekir. ' % S'kaynak URI'si, eklenen bir SAS belirteci ile birlikte, Azure Depolama'ya kimliği doğrulanmış erişim sağlar.

Aşağıdaki örnek nasıl salt yazılır SAS belirteci dizesi için kapsayıcı alınacağını gösterir, ardından SAS URI'si kapsayıcıya ekler:

```csharp
string containerSasToken = container.GetSharedAccessSignature(new SharedAccessBlobPolicy()
{
    SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddDays(1),
    Permissions = SharedAccessBlobPermissions.Write
});

string containerSasUrl = container.Uri.AbsoluteUri + containerSasToken;
```

## <a name="specify-output-files-for-task-output"></a>Görev çıktısı için çıktı dosyaları belirtin

Bir görevin çıkış dosyaları belirtmek için bir koleksiyonunu oluşturmak [OutputFile](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile) nesneleri ve atamanıza [CloudTask.OutputFiles](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudtask.outputfiles#Microsoft_Azure_Batch_CloudTask_OutputFiles) görev oluşturduğunuzda özelliği.

Aşağıdaki C# kod örneği, rastgele sayılar adlı bir dosyaya yazan bir görev oluşturduğunda `output.txt`. Bu örnek için bir çıktı dosyası oluşturur `output.txt` kapsayıcıya yazılacak. Örnek ayrıca dosya deseniyle eşleşen tüm günlük dosyaları için Çıkış dosyalarını oluşturur `std*.txt` (_örn_, `stdout.txt` ve `stderr.txt`). Oluşturulan SAS, kapsayıcı URL'si için kapsayıcı önceden gerektirir. Batch hizmeti, kapsayıcı kimlik doğrulaması yapmak için SAS kullanır:

```csharp
new CloudTask(taskId, "cmd /v:ON /c \"echo off && set && (FOR /L %i IN (1,1,100000) DO (ECHO !RANDOM!)) > output.txt\"")
{
    OutputFiles = new List<OutputFile>
    {
        new OutputFile(
            filePattern: @"..\std*.txt",
            destination: new OutputFileDestination(
         new OutputFileBlobContainerDestination(
                    containerUrl: containerSasUrl,
                    path: taskId)),
            uploadOptions: new OutputFileUploadOptions(
            uploadCondition: OutputFileUploadCondition.TaskCompletion)),
        new OutputFile(
            filePattern: @"output.txt",
            destination: 
         new OutputFileDestination(new OutputFileBlobContainerDestination(
                    containerUrl: containerSasUrl,
                    path: taskId + @"\output.txt")),
            uploadOptions: new OutputFileUploadOptions(
            uploadCondition: OutputFileUploadCondition.TaskCompletion)),
}
```

### <a name="specify-a-file-pattern-for-matching"></a>Eşleşen bir dosya deseni belirtin

Bir çıkış dosyası belirttiğinizde, kullanabileceğiniz [OutputFile.FilePattern](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile.filepattern#Microsoft_Azure_Batch_OutputFile_FilePattern) özelliğini eşleştirmek için bir dosya deseni belirtin. Dosya deseni sıfır dosyaları, tek bir dosyayı veya bir dizi görev tarafından oluşturulan dosyaları eşleşmiyor olabilir.

**FilePattern** özelliğini destekleyen standart dosya sistemi joker karakterler gibi `*` (yinelemesiz eşleşen için) ve `**` (yinelemeli eşleşen için). Örneğin, yukarıdaki kod örneğinde eşleştirilecek dosya desenini belirtir. `std*.txt` öz yinelemeli olmayan:

`filePattern: @"..\std*.txt"`

Tek bir dosyayı karşıya yüklemek için joker karakterleri bir dosya deseni belirtin. Örneğin, yukarıdaki kod örneğinde eşleştirilecek dosya desenini belirtir. `output.txt`:

`filePattern: @"output.txt"`

### <a name="specify-an-upload-condition"></a>Karşıya yükleme koşulu belirtin

[OutputFileUploadOptions.UploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileuploadoptions.uploadcondition#Microsoft_Azure_Batch_OutputFileUploadOptions_UploadCondition) özelliği verir koşullu'nın çıktı dosyalarını karşıya yükleniyor. Başarısız olursa, görev başarılı olursa dosya bir kümesini ve farklı bir dosya kümesini karşıya yaygın bir senaryodur. Örneğin, sadece görev başarısız olur ve sıfır olmayan çıkış kodu ile çıkar ayrıntılı günlük dosyalarını karşıya yüklemek isteyebilirsiniz. Benzer şekilde, yalnızca görev başarılı olursa, bu dosyaları ya da görev başarısız olursa eksik gibi sonuç dosyalarını karşıya yüklemek isteyebilirsiniz.

Kod örneği kümeleri üstündeki **UploadCondition** özelliğini **Net_offline_option**. Bu ayar, görevler tamamlandıktan sonra çıkış kodu değerini bağımsız olarak karşıya yüklenecek dosyanın olduğunu belirtir.

`uploadCondition: OutputFileUploadCondition.TaskCompletion`

Diğer ayarlar için bkz: [OutputFileUploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.common.outputfileuploadcondition) sabit listesi.

### <a name="disambiguate-files-with-the-same-name"></a>Aynı ada sahip dosyaları ayırt etmek

Bir işteki görevleri aynı ada sahip dosyaları oluşturabilir. Örneğin, `stdout.txt` ve `stderr.txt` bir işlemde çalışan her görev için oluşturulur. Her görev kendi bağlamında çalıştığından, bu dosyalar düğümün dosya sisteminde çakışmadığını. Bununla birlikte, paylaşılan bir kapsayıcıya birden fazla görevden dosyaları karşıya yüklediğinizde, aynı ada sahip dosyaları ayırt etmek gerekir.

[OutputFileBlobContainerDestination.Path](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination.path#Microsoft_Azure_Batch_OutputFileBlobContainerDestination_Path) özelliği, hedef blob veya sanal dizinde Çıkış dosyalarını belirtir. Kullanabileceğiniz **yolu** özelliği blob veya çıkış dosyalarını aynı ada sahip Azure Depolama'da benzersiz olarak adlandırıldığından biçimindeki sanal dizin adı. Görev Kimliği yolu kullanarak benzersiz adlar sağlamak ve dosyaları kolayca tanımlamak için iyi bir yoludur.

Varsa **FilePattern** özelliği için bir joker karakter ifadesini ayarlayın ve ardından sanal dizini tarafından belirtilen desenle eşleşen tüm dosyaları karşıya **yolu** özelliği. Örneğin, kapsayıcı ise `mycontainer`, görev kimliği `mytask`, ve dosya deseni `..\std*.txt`, mutlak bir URI'leri Azure depolamadaki çıkış dosyaları benzer olabilir:

```
https://myaccount.blob.core.windows.net/mycontainer/mytask/stderr.txt
https://myaccount.blob.core.windows.net/mycontainer/mytask/stdout.txt
```

Varsa **FilePattern** özelliği içermediğinden herhangi bir joker karakter, ardından değeri, yani bir tek dosya adıyla eşleşecek şekilde ayarlanır **yolu** özelliği tam olarak nitelenmiş blob adını belirtir. Adlandırma çakışmaları ile birden çok görevi tek bir dosyadan öngörüyorsanız, ardından bu dosyaları ayırt etmek için dosya adı bir parçası olarak sanal dizin adını içerir. Örneğin, **yolu** özelliği, görev kimliği, sınırlayıcı karakteri (genellikle ileri eğik çizgi) ve dosya adı eklemek için:

`path: taskId + @"/output.txt"`

' % S'mutlak URI için bir dizi görevin çıkış dosyaları şuna benzer olacaktır:

```
https://myaccount.blob.core.windows.net/mycontainer/task1/output.txt
https://myaccount.blob.core.windows.net/mycontainer/task2/output.txt
```

Azure depolama alanında sanal dizinleri hakkında daha fazla bilgi için bkz. [bir kapsayıcıdaki blobları listelemek](../storage/blobs/storage-quickstart-blobs-dotnet.md#list-the-blobs-in-a-container).

## <a name="diagnose-file-upload-errors"></a>Dosya karşıya yükleme hatalarını tanılama

Çıktı dosyaları Azure Storage'a yüklemeden başarısız durumunda görev taşır **tamamlandı** durumu ve [TaskExecutionInformation.FailureInformation](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskexecutioninformation.failureinformation#Microsoft_Azure_Batch_TaskExecutionInformation_FailureInformation) özelliği ayarlanmış. İnceleme **FailureInformation** hangi hatanın oluştuğunu belirlemek için özellik. Örneğin, kapsayıcı bulunamazsa dosya karşıya yükleme sırasında oluşan bir hatayı şu şekildedir:

```
Category: UserError
Code: FileUploadContainerNotFound
Message: One of the specified Azure container(s) was not found while attempting to upload an output file
```

Her dosyayı karşıya yükleme Batch işlem düğümü için iki günlük dosyalarını Yazar `fileuploadout.txt` ve `fileuploaderr.txt`. Belirli bir kültür hakkında daha fazla bilgi için bu günlük dosyaları inceleyebilirsiniz. Görev çalıştırılamadı çünkü burada dosya karşıya yükleme hiçbir zaman, örneğin girişiminde bulunuldu durumlarda, daha sonra bu günlük dosyaları mevcut olmaz.

## <a name="diagnose-file-upload-performance"></a>Dosya karşıya yükleme performansını tanılamanıza

`fileuploadout.txt` Dosya karşıya yükleme durumu günlüğe kaydeder. Dosya yüklemeleriniz hakkında ne kadar sürüyor daha fazla bilgi için bu dosyayı inceleyebilirsiniz. Hedef kapsayıcı kaç düğümleri stora için karşıya yükleme, bir Batch havuzu ile aynı bölgede olup düğümü, düğümdeki başka bir etkinliği karşıya yükleme zamanında boyutu dahil olmak üzere performans, karşıya yüklemek için çok sayıda katkıda bulunan etken vardır unutmayın Ge hesap aynı zamanda ve benzeri.

## <a name="use-the-batch-service-api-with-the-batch-file-conventions-standard"></a>Batch hizmeti API'si ile Batch dosya kuralları standart kullanın

Batch hizmeti API'si ile görev çıktılarını kalıcı hale getirme, hedef kapsayıcı adı verebilirsiniz ve istediğiniz gibi blobları. Ayrıca bunları göre seçebilirsiniz [Batch dosya kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/Batch/Support/FileConventions#conventions). Dosya kuralları standart Azure Depolama'daki bir blob adları iş ve görev tabanlı bir belirtilen çıkış dosyası için ve hedef kapsayıcı adını belirler. Dosya kuralları standart çıkış dosyalarını adlandırma için kullandığınız sonra çıktı dosyalarınızı görüntülenmek üzere kullanılabilir [Azure portalında](https://portal.azure.com).

C# dilinde geliştiriyorsanız yerleşik yöntemlerini kullanabilirsiniz [.NET için Batch dosya kuralları Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files). Bu kitaplık, düzgün bir şekilde adlandırılmış kapsayıcı ve blob yollarının sizin için oluşturur. Örneğin, adın doğru proje adına göre kapsayıcı almak için API çağırabilirsiniz:

```csharp
string containerName = job.OutputStorageContainerName();
```

Kullanabileceğiniz [CloudJobExtensions.GetOutputStorageContainerUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files.cloudjobextensions.getoutputstoragecontainerurl) kapsayıcıya yazmak için kullanılan bir paylaşılan erişim imzası (SAS) URL döndürmek için yöntemi. Ardından bu SAS'ye geçirebilirsiniz [OutputFileBlobContainerDestination](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination) Oluşturucusu.

C# dışındaki bir dilde geliştiriyorsanız, dosya kuralları standart kendiniz uygulamanız gerekir.

## <a name="code-sample"></a>Kod örneği

[PersistOutputs] [ github_persistoutputs] örnek proje, biridir [Azure Batch kod örnekleri] [ github_samples] GitHub üzerinde. Bu Visual Studio çözüm görev çıktısını kalıcı depolama devam ettirmek için .NET için Batch istemci kitaplığını kullanma işlemini gösterir. Örneği çalıştırmak için aşağıdaki adımları izleyin:

1. Projeyi **Visual Studio 2017**.
2. Batch ve Storage ekleme **hesap kimlik bilgileri** için **AccountSettings.settings** Microsoft.Azure.Batch.Samples.Common projedeki.
3. **Derleme** (ancak çalıştırılmadı) çözümü. Herhangi bir NuGet paketinin istenirse geri yükleyin.
4. Karşıya yüklemek için Azure portalını kullanın. bir [uygulama paketini](batch-application-packages.md) için **PersistOutputsTask**. Dahil `PersistOutputsTask.exe` ve bunların bağımlı derlemeleri .zip paketindeki "PersistOutputsTask" için uygulama kimliği ve "1.0" için uygulama paketi sürümünü ayarlama.
5. **Başlangıç** (Çalıştır) **PersistOutputs** proje.
6. Örneği çalıştırmak için kullanmak için girin Kalıcılık teknoloji seçmeniz istendiğinde **2** görev çıktısını kalıcı hale getirmek için Batch hizmeti API'sini kullanarak örneği çalıştırmak için.
7. İsterseniz, örnek yeniden girerek çalıştırın **3** Batch hizmeti API'si ile çıkışı kalıcı hale getirmek için ve ayrıca dosya kuralları standardına göre hedef kapsayıcı ve blob yolunu adlandırmak için.

## <a name="next-steps"></a>Sonraki adımlar

- .NET için dosya kuralları kitaplığı ile kalıcı hale getirme görev çıktısını hakkında daha fazla bilgi için bkz. [.NET için Azure Depolama'ya iş ve görev veri Batch dosya kuralları kitaplığı ile kalıcı](batch-task-output-file-conventions.md).
- Azure batch'te kalıcı çıktı verileri için diğer yaklaşımlar hakkında daha fazla bilgi için bkz: [Azure Depolama'ya iş ve görev çıktılarını kalıcı hale getirme](batch-task-output.md).

[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
