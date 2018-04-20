---
title: Azure Storage Azure Batch hizmeti API'si ile iş ve görev çıktısına kalıcı | Microsoft Docs
description: Toplu işlem görevi ve iş çıktısı Azure depolama için kalıcı hale getirmek için Batch hizmeti API'si kullanmayı öğrenin.
services: batch
author: dlepow
manager: jeconnoc
editor: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: danlep
ms.openlocfilehash: ee8622525adcc698bf920b0c3379cc3065798a19
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="persist-task-data-to-azure-storage-with-the-batch-service-api"></a>Görev verileri Azure Batch hizmeti API'si ile Sürdür

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Batch hizmeti API'si 2017-05-01 sürümünden başlayarak, görevler ve sanal makine yapılandırması havuzları çalışan iş yöneticisi görevleri için kalıcı çıktı verilerini Azure Storage'a destekler. Bir görev eklediğinizde, görevin çıkış için hedef olarak Azure Storage'da kapsayıcı belirtebilirsiniz. Görev tamamlandığında, Batch hizmeti kapsayıcıya sonra herhangi bir çıktı veri yazar.

Görev çıktısı kalıcı hale getirmek için Batch hizmeti API'si kullanmanın bir avantajı, görevin çalıştığı uygulama değişiklik gerekmez ' dir. Bunun yerine, istemci uygulamanız için birkaç basit değişiklikler ile görev çıktısı görev oluşturan kodu devam edebilir.   

## <a name="when-do-i-use-the-batch-service-api-to-persist-task-output"></a>Ne zaman Batch hizmeti API'si görev çıktısını kalıcı hale getirmek için kullanabilirim?

Azure Batch görev çıktısını kalıcı hale getirmek için birden fazla yol sağlar. En iyi uyan kullanışlı bir yaklaşım bu senaryoları için Batch hizmeti API'si kullanmaktır:

- Görev çıktısı, istemci uygulaması, görevin çalıştığı uygulama değiştirmeden kalıcı hale getirmek için kod yazmak istiyorum.
- Toplu iş görevleri ve iş yöneticisi görevleri sanal makine yapılandırmasıyla oluşturulan havuzlarında çıktısını kalıcı hale getirmek istediğiniz.
- Rastgele bir ada sahip bir Azure Storage kapsayıcısına çıkış kalıcı hale getirmek istediğiniz.
- Çıktı göre adlı bir Azure Storage kapsayıcısına kalıcı hale getirmek istediğiniz [toplu iş dosyası kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). 

Senaryonuz Yukarıda listelenenler farklıysa, farklı bir yaklaşım dikkate almanız gerekebilir. Örneğin, Görev yürütülürken Batch hizmeti API'si Azure Storage akış çıkışı şu anda desteklemiyor. Çıkış akışı, .NET için kullanılabilir toplu iş dosyası kuralları kitaplığı kullanmayı düşünün. Diğer diller için kendi çözümü uygulamak gerekir. Kalıcı görev çıktısı için diğer seçenekleri hakkında daha fazla bilgi için bkz: [iş ve görev çıktı Azure depolama için kalıcı](batch-task-output.md). 

## <a name="create-a-container-in-azure-storage"></a>Azure Storage'da kapsayıcı oluşturma

Görev çıktısını Azure depolama için kalıcı hale getirmek için çıkış dosyaları için hedef olarak hizmet veren bir kapsayıcı oluşturmanız gerekir. İşinizi göndermeden önce göreviniz, tercihen çalıştırmadan önce kapsayıcı oluşturun. Kapsayıcı oluşturmak için uygun Azure Storage istemci kitaplığı veya SDK kullanın. Azure depolama API'leri hakkında daha fazla bilgi için bkz: [Azure Storage belgeleri](https://docs.microsoft.com/azure/storage/).

Örneğin, uygulamanızın C# dilinde yazıyorsanız, kullanın [.NET için Azure Storage istemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/). Aşağıdaki örnek, bir kapsayıcı oluşturulacağını gösterir:

```csharp
CloudBlobContainer container = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
await conainer.CreateIfNotExists();
```

## <a name="get-a-shared-access-signature-for-the-container"></a>İçin kapsayıcı paylaşılan erişim imzası Al

Kapsayıcıyı oluşturduktan sonra kapsayıcıya yazma erişimi olan bir paylaşılan erişim imzası (SAS) alın. Bir SAS kapsayıcı yetkilendirilmiş erişim sağlar. SAS izinleri ve belirli bir zaman aralığı içinde belirtilen bir kümesiyle erişim verir. Batch hizmeti için kapsayıcı görevi çıkışını yazmak için yazma izinlerine sahip bir SAS gerekir. SAS hakkında daha fazla bilgi için bkz: [kullanarak paylaşılan erişim imzaları \(SAS\) Azure storage'da](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

Azure depolama API'leri kullanılarak bir SAS aldığınızda, API bir SAS belirteci dize döndürür. Bu belirteç dizesini izinleri ve SAS geçerli aralığa dahil olmak üzere, SAS tüm parametreleri içerir. SAS Azure storage'da kapsayıcı erişmek üzere kullanmak için kaynak URI'si SAS belirteci dizesi eklemek gerekir. Eklenen SAS belirteci ile birlikte kaynak URI'si, Azure depolama birimine kimliği doğrulanmış erişim sağlar.

Aşağıdaki örnek salt yazılır SAS belirteci dize için kapsayıcı alma gösterir, ardından SAS URI'si kapsayıcıya ekler:

```csharp
string containerSasToken = container.GetSharedAccessSignature(new SharedAccessBlobPolicy()
{
    SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddDays(1),
    Permissions = SharedAccessBlobPermissions.Write
});

string containerSasUrl = container.Uri.AbsoluteUri + containerSasToken; 
```

## <a name="specify-output-files-for-task-output"></a>Görev çıktısı için çıktı dosyaları belirtin

Bir görev için çıktı dosyaları belirtmek için bir koleksiyonunu oluşturmak [ÇıktıDosyası](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile) nesneleri ve atayın [CloudTask.OutputFiles](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudtask.outputfiles#Microsoft_Azure_Batch_CloudTask_OutputFiles) görev oluşturduğunuzda özelliği. 

Rastgele sayılar adlı bir dosyaya yazar bir görev aşağıdaki .NET kod örneği oluşturur `output.txt`. Örnek bir çıktı dosyası oluşturur `output.txt` kapsayıcıya yazılacak. Bu örnek ayrıca dosya desenle eşleşen tüm günlük dosyalarını için çıktı dosyaları oluşturur `std*.txt` (_örneğin_, `stdout.txt` ve `stderr.txt`). Kapsayıcı URL'si oluşturulduğu SAS önceden kapsayıcısı gerektirir. Batch hizmeti, kapsayıcıya erişimi kimlik doğrulaması yapmak için SAS kullanır: 

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

### <a name="specify-a-file-pattern-for-matching"></a>Eşleşen bir dosya düzeni belirtme

Bir çıkış dosyası belirttiğinizde, kullanabileceğiniz [OutputFile.FilePattern](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile.filepattern#Microsoft_Azure_Batch_OutputFile_FilePattern) özelliği eşleşen bir dosya düzeni belirtin. Dosya düzeni eşleşen sıfır dosyaları, tek bir dosya veya bir dizi görevi tarafından oluşturulan dosyalar.

**FilePattern** özelliğini destekleyen standart dosya sistemi joker karakterler gibi `*` (özyinelemeli olmayan eşleşen için) ve `**` (özyinelemeli eşleşen için). Örneğin, yukarıdaki kod örneği eşleştirilecek dosya deseni belirtir `std*.txt` yinelemeli olmayan: 

`filePattern: @"..\std*.txt"`

Tek bir dosyayı karşıya yüklemek için joker ile bir dosya düzeni belirtin. Örneğin, yukarıdaki kod örneği eşleştirilecek dosya deseni belirtir `output.txt`:

`filePattern: @"output.txt"`

### <a name="specify-an-upload-condition"></a>Bir karşıya yükleme koşulu belirtin

[OutputFileUploadOptions.UploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileuploadoptions.uploadcondition#Microsoft_Azure_Batch_OutputFileUploadOptions_UploadCondition) özelliği verir koşullu çıktı dosyaları karşıya yükleme. Başarısız olursa bir dizi görev başarılı olursa ve farklı bir dosya kümesini karşıya ortak bir senaryodur. Örneğin, yalnızca görev başarısız olur ve sıfır olmayan çıkış kodu ile çıkar ayrıntılı günlük dosyalarını karşıya yüklemek isteyebilirsiniz. Benzer şekilde, yalnızca görev başarılı olursa, bu dosyaları ya da görev başarısız olursa eksik olabileceğinden sonuç dosyalarını karşıya yüklemek isteyebilirsiniz.

Ayarlar Yukarıdaki kod örneğini **UploadCondition** özelliğine **Net_offline_option**. Bu ayar, dosyanın görevler tamamlandığında, çıkış kodu değerini bakılmaksızın karşıya olduğunu belirtir. 

`uploadCondition: OutputFileUploadCondition.TaskCompletion`

Diğer ayarlar için bkz: [OutputFileUploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.common.outputfileuploadcondition) enum.

### <a name="disambiguate-files-with-the-same-name"></a>Aynı ada sahip dosyaları ayırt etmek

İşteki görevler aynı ada sahip dosyaları oluşturabilir. Örneğin, `stdout.txt` ve `stderr.txt` bir işlemde çalışan her görev için oluşturulur. Her görev kendi içeriğinde çalıştığından, bu dosyalar düğümün dosya sisteminde çakışmadığından. Bununla birlikte, paylaşılan bir kapsayıcıya birden fazla görevden dosyaları karşıya yüklediğinizde, aynı ada sahip dosyaları belirsizliğini ortadan kaldırmak gerekir.

[OutputFileBlobContainerDestination.Path](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination.path#Microsoft_Azure_Batch_OutputFileBlobContainerDestination_Path) özelliği, hedef blob veya sanal dizinde çıktı dosyalarını belirtir. Kullanabileceğiniz **yolu** özelliği blob veya çıktı dosyaları aynı ada sahip Azure depolama alanında benzersiz olarak adlandırıldığından şekilde sanal dizin adı. Görev Kimliği yolu kullanarak benzersiz adlar sağlamak ve dosyaları kolayca tanımlamak için iyi bir yoludur.

Varsa **FilePattern** özelliği bir joker karakter ifadesini ayarlayın, sonra tarafından belirtilen sanal dizin için bir desenle eşleşen tüm dosyaları karşıya **yolu** özelliği. Örneğin, kapsayıcı ise `mycontainer`, kimliği görev `mytask`, ve dosya deseni `..\std*.txt`, Azure storage'da çıkış dosyaları için mutlak URI şuna benzeyecektir sonra:

```
https://myaccount.blob.core.windows.net/mycontainer/mytask/stderr.txt
https://myaccount.blob.core.windows.net/mycontainer/mytask/stdout.txt
```

Varsa **FilePattern** özelliği içermediğinden herhangi bir joker karakteri, sonra değeri, yani tek bir dosya adı, eşleşecek şekilde ayarlanmış **yolu** özelliği, tam blob adı belirtir. Adlandırma çakışmaları birden fazla görevden tek bir dosya ile öngörüyorsanız, bu dosyaları belirsizliğini ortadan kaldırmak için dosya adı bir parçası olarak sanal dizinin adını içerir. Örneğin, **yolu** özelliği görev kimliği, sınırlayıcı karakter (genellikle bir eğik çizgi) ve dosya adı eklemek için:

`path: taskId + @"/output.txt"`

Bir dizi görevi için çıktı dosyaları için mutlak URI şuna benzeyecektir:

```
https://myaccount.blob.core.windows.net/mycontainer/task1/output.txt
https://myaccount.blob.core.windows.net/mycontainer/task2/output.txt
```

Azure depolama alanında sanal dizinleri hakkında daha fazla bilgi için bkz: [BLOB'ları bir kapsayıcıda listelemek](../storage/blobs/storage-quickstart-blobs-dotnet.md#list-the-blobs-in-a-container).


## <a name="diagnose-file-upload-errors"></a>Dosya karşıya yükleme hatalarını tanılama

Çıkış dosyaları Azure depolama alanına yükleme başarısız olduysa görev taşır **tamamlandı** durumu ve [TaskExecutionInformation.FailureInformation](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskexecutioninformation.failureinformation#Microsoft_Azure_Batch_TaskExecutionInformation_FailureInformation) özelliği ayarlanmış. İncelemek **FailureInformation** hangi hatanın oluştuğunu belirlemek için özellik. Örneğin, kapsayıcı bulunamazsa dosya karşıya yükleme sırasında oluşan bir hata şöyledir: 

```
Category: UserError
Code: FileUploadContainerNotFound
Message: One of the specified Azure container(s) was not found while attempting to upload an output file
```

Her dosya karşıya yükleme toplu işlem düğümü için iki günlük dosyalarını Yazar `fileuploadout.txt` ve `fileuploaderr.txt`. Belirli hata hakkında daha fazla bilgi edinmek için bu günlük dosyaları inceleyebilirsiniz. Görev çalıştırılamadı çünkü burada dosya karşıya yükleme hiçbir zaman, örneğin denendi durumlarda, sonra bu günlük dosyaları mevcut olmaz.

## <a name="diagnose-file-upload-performance"></a>Dosya karşıya yükleme performansı tanılama

`fileuploadout.txt` Dosya karşıya yükleme ilerleme durumu günlüğe kaydeder. Dosya karşıya yüklemeler hakkında ne kadar sürüyor daha fazla bilgi için bu dosyayı inceleyebilirsiniz. Kaç tane düğümleri stora karşıya yüklediğiniz Batch havuzu ile aynı bölgede hedef kapsayıcısı olup olmadığını düğümü, düğümdeki diğer etkinliklerin karşıya yükleme zaman boyutu da dahil olmak üzere performansı, karşıya yüklemek için birçok faktörlere vardır göz önünde bulundurun aynı anda vb. ge hesabı.

## <a name="use-the-batch-service-api-with-the-batch-file-conventions-standard"></a>Toplu iş dosyası kuralları standart toplu işlem hizmeti API'si kullanın

Görev çıktısı Batch hizmeti API'si ile kalıcı, hedef kapsayıcı adı verebilirsiniz ve istediğiniz ancak blobları. Ayrıca bunları göre seçebilirsiniz [toplu iş dosyası kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). Dosya kuralları standart Hedef kapsayıcıyı ve Azure storage'da bir blob iş ve görev adlarına göre verilen çıktı dosyası adını belirler. Dosya kuralları standart çıktı dosyalarını adlandırma için kullandığınız sonra çıktı dosyaları görüntülemek için kullanılabilir olan [Azure portal](https://portal.azure.com).

C# ' ta geliştiriyorsanız yerleşik yöntemlerini kullanabilirsiniz [.NET için toplu iş dosyası kuralları Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files). Bu kitaplık, blob yolları ve düzgün bir şekilde adlandırılmış kapsayıcıları sizin için oluşturur. Örneğin, iş adını temel alarak bu kapsayıcı için doğru bir adı almak için API çağırabilirsiniz:

```csharp
string containerName = job.OutputStorageContainerName();
```

Kullanabileceğiniz [CloudJobExtensions.GetOutputStorageContainerUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files.cloudjobextensions.getoutputstoragecontainerurl) kapsayıcıya yazmak için kullanılan bir paylaşılan erişim imzası (SAS) dönüş URL'ine yöntemi. Ardından bu SAS geçirebilirsiniz [OutputFileBlobContainerDestination](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination) Oluşturucusu.

C# dışındaki bir dilde geliştiriyorsanız dosyası kuralları standart kendiniz uygulamak gerekir.

## <a name="code-sample"></a>Kod örneği

[PersistOutputs] [ github_persistoutputs] örnek proje biridir [Azure Batch kod örnekleri] [ github_samples] github'da. Bu Visual Studio çözümü .NET için toplu istemci Kitaplığı görev çıktısını dayanıklı depolama için kalıcı hale getirmek için nasıl kullanılacağını gösterir. Örneği çalıştırmak için aşağıdaki adımları izleyin:

1. Projeyi **Visual Studio 2015 veya daha yeni**.
2. Batch ve Storage eklemek **hesabının kimlik bilgilerini** için **AccountSettings.settings** öğesini kullanıma alın projesinde.
3. **Yapı** (ancak çalışmaz) çözümü. İstenirse herhangi bir NuGet paketinin geri yükleyin.
4. Karşıya yüklemek için Azure portal'ı kullanmanızı bir [uygulama paketi](batch-application-packages.md) için **PersistOutputsTask**. Dahil `PersistOutputsTask.exe` ve uygulama kimliği "PersistOutputsTask" ve "1.0" uygulama paketi sürümüne bağımlı derlemeleri .zip paketindeki ayarlayın.
5. **Başlat** (Çalıştır) **PersistOutputs** projesi.
6. Örnek çalıştırmak için kullanmak için girin Kalıcılık teknolojisi seçmek için istendiğinde **2** görev çıktısını kalıcı hale getirmek için Batch hizmeti API'si kullanılarak örneği çalıştırmak için.
7. İsterseniz, girme örneği yeniden çalıştırmak **3** çıktı Batch hizmeti API'si ile kalıcı hale getirmek için ve ayrıca dosyası kuralları standart göre hedef kapsayıcı ve blob yol adı.

## <a name="next-steps"></a>Sonraki adımlar

- .NET için kalıcı Görev çıkış dosyası kuralları Kitaplığı hakkında daha fazla bilgi için bkz: [iş ve görev verileri Azure depolama kalıcı hale getirmek .NET için toplu iş dosyası kuralları kitaplığıyla kalıcı ](batch-task-output-file-conventions.md).
- Azure Batch kalıcı çıktı verileri için diğer yaklaşımlar hakkında daha fazla bilgi için bkz: [iş ve görev çıktı Azure depolama için kalıcı](batch-task-output.md).

[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
