---
title: Kayıtları toplu halde Azure bildirim hub'ları içeri ve dışarı | Microsoft Docs
description: Bildirim hub'ları toplu desteği, çok sayıda bildirim hub'ı üzerinde işlemler gerçekleştirmek için veya tüm kayıtlar dışarı aktarmak için kullanmayı öğrenin.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 03/18/2019
ms.author: jowargo
ms.openlocfilehash: c24fcd5f007b641bb594bb07348491f70c03ea41
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60623995"
---
# <a name="export-and-import-azure-notification-hubs-registrations-in-bulk"></a>Azure Notification hubs'ı kayıtları toplu halde alma ve verme
Hangi oluşturmak veya bir bildirim hub'ı kayıtları çok sayıda değiştirmek için gereklidir senaryo vardır. Bu senaryolardan bazıları batch hesaplamaları şu veya bildirim hub'ları kullanmak için mevcut bir anında iletme uygulamasına geçirirken etiketi güncelleştirmelerdir.

Bu makalede, çok sayıda bildirim hub'ı üzerinde işlemler gerçekleştirmek için veya tüm kayıtları toplu halde dışa aktarmak için nasıl açıklar.

## <a name="high-level-flow"></a>Üst düzey akış
Batch desteği, uzun süre çalışan işler milyonlarca kaydı içeren desteklemek için tasarlanmıştır. Bu ölçeği elde etmek için iş ayrıntılarını ve çıktıyı depolamak için Azure depolama batch desteği kullanır. Toplu güncelleştirme işlemleri için kullanıcı kayıt güncelleme işlemleri listesini içeriğe sahip olan bir blob kapsayıcısında bir dosya oluşturmak için gereklidir. Kullanıcı, işi başlatılırken bir çıktı dizini (aynı zamanda bir blob kapsayıcısında) için bir URL ile birlikte giriş blob için bir URL sağlar. İş başlatıldıktan sonra kullanıcının işini başlatma sırasında sağlanan bir URL konumu sorgulayarak durumu kontrol edebilirsiniz. Belirli bir işin yalnızca belirli bir tür işlemler gerçekleştirebilir (oluşturur, güncelleştirir ve siler). Dışa aktarma işlemleri öğesine gerçekleştirilir.

## <a name="import"></a>İçeri Aktarma

### <a name="set-up"></a>Kurulum
Bu bölümde, aşağıdaki varlıkların olduğunu varsayar:

- Sağlanan bildirim hub.
- Bir Azure depolama blob kapsayıcısı.
- Başvurular [Azure depolama NuGet paketini](https://www.nuget.org/packages/windowsazure.storage/) ve [Notification Hubs NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.9).

### <a name="create-input-file-and-store-it-in-a-blob"></a>Giriş dosyası oluşturma ve blob depolama
Giriş dosyası kayıtları satır başına bir XML seri hale getirilmiş bir listesini içerir. Aşağıdaki kod örneği, Azure SDK'sını kullanarak, kayıtları seri hale getirmek ve bunları blob kapsayıcısına yüklemek nasıl gösterir.

```csharp
private static void SerializeToBlob(CloudBlobContainer container, RegistrationDescription[] descriptions)
{
    StringBuilder builder = new StringBuilder();
    foreach (var registrationDescription in descriptions)
    {
        builder.AppendLine(RegistrationDescription.Serialize());
    }

    var inputBlob = container.GetBlockBlobReference(INPUT_FILE_NAME);
    using (MemoryStream stream = new MemoryStream(Encoding.UTF8.GetBytes(builder.ToString())))
    {
        inputBlob.UploadFromStream(stream);
    }
}
```

> [!IMPORTANT]
> Yukarıdaki kod, bellek kayıtları serileştirir ve ardından tüm stream blob yükler. Birden fazla yalnızca birkaç megabayt bir dosyası yüklediyseniz, Azure blob Kılavuzu adımları gerçekleştirmek nasıl bakın; Örneğin, [blok blobları](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).

### <a name="create-url-tokens"></a>URL belirteçleri oluşturun
Giriş dosyanız karşıya yüklendikten sonra giriş dosyası ve çıkış dizini için bildirim hub'ınıza sağlamak için URL'lerin oluşturur. Giriş ve çıkış için iki farklı blob kapsayıcıları kullanabilirsiniz.

```csharp
static Uri GetOutputDirectoryUrl(CloudBlobContainer container)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
    {
        SharedAccessExpiryTime = DateTime.UtcNow.AddDays(1),
        Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
    };

    string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);
    return new Uri(container.Uri + sasContainerToken);
}

static Uri GetInputFileUrl(CloudBlobContainer container, string filePath)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
    {
        SharedAccessExpiryTime = DateTime.UtcNow.AddDays(1),
        Permissions = SharedAccessBlobPermissions.Read
    };
    string sasToken = container.GetBlockBlobReference(filePath).GetSharedAccessSignature(sasConstraints);
    return new Uri(container.Uri + "/" + filePath + sasToken);
}
```

### <a name="submit-the-job"></a>İşi gönderme
İki giriş ve çıkış URL'lerle, batch işi şimdi başlayabilirsiniz.

```csharp
NotificationHubClient client = NotificationHubClient.CreateClientFromConnectionString(CONNECTION_STRING, HUB_NAME);
var createTask = client.SubmitNotificationHubJobAsync(
new NotificationHubJob {
        JobType = NotificationHubJobType.ImportCreateRegistrations,
        OutputContainerUri = outputContainerSasUri,
        ImportFileUri = inputFileSasUri
    }
);
createTask.Wait();

var job = createTask.Result;
long i = 10;
while (i > 0 && job.Status != NotificationHubJobStatus.Completed)
{
    var getJobTask = client.GetNotificationHubJobAsync(job.JobId);
    getJobTask.Wait();
    job = getJobTask.Result;
    Thread.Sleep(1000);
    i--;
}
```

Giriş ve çıkış URL'leri ek olarak, bu örnek, oluşturur bir `NotificationHubJob` içeren nesne bir `JobType` nesnesi şu türlerden biri olabilir:

- `ImportCreateRegistrations`
- `ImportUpdateRegistrations`
- `ImportDeleteRegistrations`

Arama tamamlandı, bildirim hub'ı iş devam ettirildiğinde ve çağrısıyla durumunun kontrol edilebileceğini sonra [GetNotificationHubJobAsync](/dotnet/api/microsoft.azure.notificationhubs.notificationhubclient.getnotificationhubjobasync?view=azure-dotnet).

İş tamamlandığında, çıkış dizinindeki aşağıdaki dosyaları bakarak sonuçları inceleyebilirsiniz:

- `/<hub>/<jobid>/Failed.txt`
- `/<hub>/<jobid>/Output.txt`

Bu dosyalar, batch gelen başarılı ve başarısız işlemlerin listesini içerir. Dosya biçimi `.cvs`, her satırın ilk giriş dosyasının satır numarası ve işlemin (genellikle oluşturduğunuz veya güncelleştirdiğiniz kaydı açıklaması) çıktısını hiç.

### <a name="full-sample-code"></a>Tam örnek kod
Aşağıdaki örnek kod, bir bildirim hub'ına kayıtları alır.

```csharp
using Microsoft.Azure.NotificationHubs;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using System.Runtime.Serialization;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using System.Xml;

namespace ConsoleApplication1
{
    class Program
    {
        private static string CONNECTION_STRING = "---";
        private static string HUB_NAME = "---";
        private static string INPUT_FILE_NAME = "CreateFile.txt";
        private static string STORAGE_ACCOUNT = "---";
        private static string STORAGE_PASSWORD = "---";
        private static StorageUri STORAGE_ENDPOINT = new StorageUri(new Uri("---"));

        static void Main(string[] args)
        {
            var descriptions = new[]
            {
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMkUxREQFBlVTTkMwMQ"),
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMjUxREQFBlVTTkMwMQ"),
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMhUxREQFBlVTTkMwMQ"),
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMdUxREQFBlVTTkMwMQ"),
            };

            //write to blob store to create an input file
            var blobClient = new CloudBlobClient(STORAGE_ENDPOINT, new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(STORAGE_ACCOUNT, STORAGE_PASSWORD));
            var container = blobClient.GetContainerReference("testjobs");
            container.CreateIfNotExists();

            SerializeToBlob(container, descriptions);

            // TODO then create Sas
            var outputContainerSasUri = GetOutputDirectoryUrl(container);
            var inputFileSasUri = GetInputFileUrl(container, INPUT_FILE_NAME);


            //Lets import this file
            NotificationHubClient client = NotificationHubClient.CreateClientFromConnectionString(CONNECTION_STRING, HUB_NAME);
            var createTask = client.SubmitNotificationHubJobAsync(
                new NotificationHubJob {
                    JobType = NotificationHubJobType.ImportCreateRegistrations,
                    OutputContainerUri = outputContainerSasUri,
                    ImportFileUri = inputFileSasUri
                }
            );
            createTask.Wait();

            var job = createTask.Result;
            long i = 10;
            while (i > 0 && job.Status != NotificationHubJobStatus.Completed)
            {
                var getJobTask = client.GetNotificationHubJobAsync(job.JobId);
                getJobTask.Wait();
                job = getJobTask.Result;
                Thread.Sleep(1000);
                i--;
            }
        }

        private static void SerializeToBlob(CloudBlobContainer container, RegistrationDescription[] descriptions)
        {
            StringBuilder builder = new StringBuilder();
            foreach (var registrationDescription in descriptions)
            {
                builder.AppendLine(RegistrationDescription.Serialize());
            }

            var inputBlob = container.GetBlockBlobReference(INPUT_FILE_NAME);
            using (MemoryStream stream = new MemoryStream(Encoding.UTF8.GetBytes(builder.ToString())))
            {
                inputBlob.UploadFromStream(stream);
            }
        }

        static Uri GetOutputDirectoryUrl(CloudBlobContainer container)
        {
            //Set the expiry time and permissions for the container.
            //In this case no start time is specified, so the shared access signature becomes valid immediately.
            SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
            {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(4),
                Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
            };

            //Generate the shared access signature on the container, setting the constraints directly on the signature.
            string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

            //Return the URI string for the container, including the SAS token.
            return new Uri(container.Uri + sasContainerToken);
        }

        static Uri GetInputFileUrl(CloudBlobContainer container, string filePath)
        {
            //Set the expiry time and permissions for the container.
            //In this case no start time is specified, so the shared access signature becomes valid immediately.
            SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
            {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(4),
                Permissions = SharedAccessBlobPermissions.Read
            };

            //Generate the shared access signature on the container, setting the constraints directly on the signature.
            string sasToken = container.GetBlockBlobReference(filePath).GetSharedAccessSignature(sasConstraints);

            //Return the URI string for the container, including the SAS token.
            return new Uri(container.Uri + "/" + filePath + sasToken);
        }

        static string GetJobPath(string namespaceName, string notificationHubPath, string jobId)
        {
            return string.Format(CultureInfo.InvariantCulture, @"{0}//{1}/{2}/", namespaceName, notificationHubPath, jobId);
        }
    }
}
```

## <a name="export"></a>Dışarı Aktarma
Kayıt dışarı aktarma, aşağıdaki farklarla birlikte içeri aktarmak için benzer:

- Yalnızca çıktı URL gerekir.
- Bir tür ExportRegistrations NotificationHubJob oluşturursunuz.

### <a name="sample-code-snippet"></a>Örnek kod parçacığı
Java'da kayıtlarını dışarı aktarma için bir örnek kod parçacığı aşağıda verilmiştir:

```java
// submit an export job
NotificationHubJob job = new NotificationHubJob();
job.setJobType(NotificationHubJobType.ExportRegistrations);
job.setOutputContainerUri("container uri with SAS signature");
job = hub.submitNotificationHubJob(job);

// wait until the job is done
while(true){
    Thread.sleep(1000);
    job = hub.getNotificationHubJob(job.getJobId());
    if(job.getJobStatus() == NotificationHubJobStatus.Completed)
        break;
}

```

## <a name="next-steps"></a>Sonraki adımlar
Kayıtları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Kayıt Yönetimi](notification-hubs-push-notification-registration-management.md)
- [Kayıtlar için etiketler](notification-hubs-tags-segment-push-message.md)
- [Şablon kayıtları](notification-hubs-templates-cross-platform-push-messages.md)
