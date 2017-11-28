---
title: "Kuyruk depolama ve Visual Studio ile çalışmaya başlama bağlı Hizmetleri (ASP.NET Core) | Microsoft Docs"
description: "Visual Studio'da ASP.NET Core projesinde Azure kuyruk depolama kullanarak nereden başlayacaksınız"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: kraigb
ms.openlocfilehash: d83ad26e4f96e1a7670c5212b7e9ca8182b7cec4
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a>Kuyruk depolama ve Visual Studio ile çalışmaya başlama bağlı Hizmetleri (ASP.NET çekirdek)

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

Bu makalede nasıl oluşturduğunuz veya Visual Studio kullanarak bir ASP.NET Core projesini bir Azure depolama hesabında başvurulan sonra Visual Studio'da Azure kuyruk depolama kullanarak başlayacağınızı **bağlantılı Hizmetler** özelliği. **Bağlantılı Hizmetler** işlemi Azure depolama projenize erişmek için uygun NuGet paketlerini yükler ve proje yapılandırma dosyalarınızı depolama hesabı için bağlantı dizesi ekler. (Bkz [Storage belgeleri](https://azure.microsoft.com/documentation/services/storage/) Azure Storage hakkında genel bilgi için.)

Azure kuyruk depolama, çok sayıda herhangi bir yere HTTP veya HTTPS kullanarak kimlik doğrulaması yapılmış çağrılar aracılığıyla erişilebilen iletileri depolamak için bir hizmettir. Tek bir kuyruk iletisinin 64 kilobayt (KB) boyutunda olabilir ve bir kuyruk iletileri, bir depolama hesabının toplam kapasite sınırına kadar milyonlarca içerebilir. Ayrıca bkz. [.NET kullanarak Azure kuyruk depolamaya başlayın](../storage/queues/storage-dotnet-how-to-use-queues.md) program aracılığıyla sıraları düzenleme ile ilgili ayrıntılar için.

Başlamak için ilk Azure kuyruk depolama hesabınızı oluşturun. Bu makalede daha sonra bir sıra C# ' ta oluşturma ve ekleme, değiştirme, okuma ve iletileri kuyruğa kaldırma gibi temel kuyruk işlemlerini gerçekleştirmek nasıl gösterir.  Kod, .NET için Azure Storage istemci kitaplığı kullanır. ASP.NET hakkında daha fazla bilgi için bkz: [ASP.NET](http://www.asp.net).

Bazı Azure depolama API'leri zaman uyumsuz ve zaman uyumsuz yöntemleri kullanıldığı bu makaledeki kod varsayar. Bkz: [zaman uyumsuz programlama](https://docs.microsoft.com/dotnet/csharp/async) daha fazla bilgi için.

## <a name="access-queues-in-code"></a>Kod erişim kuyruklar

ASP.NET Core projeleri kuyruklarda erişmek için Azure kuyruk depolama erişen tüm C# kaynak dosyasında aşağıdaki öğeleri içerir. Bu kod kodu önünde tümünün izleyen bölümlerde kullanın.

1. Gerekli eklemek `using` deyimleri:
    ```cs
    using Microsoft.Framework.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Framework.Logging.LogLevel;
    ```

1. Alma bir `CloudStorageAccount` depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın:

    ```cs
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Alma bir `CloudQueueClient` depolama hesabınızda sıra nesneleri başvurmak için:

    ```cs
    // Create the CloudQueueClient object for the storage account.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. Alma bir `CloudQueue` belirli bir kuyruğa başvurmak için:

    ```cs
    // Get a reference to the CloudQueue named "messagequeue"
    CloudQueue messageQueue = queueClient.GetQueueReference("messagequeue");
    ```

### <a name="create-a-queue-in-code"></a>Kodda bir sıra oluşturun

Kodda Azure kuyruk oluşturmak için arama '' CreateIfNotExistsAsync':

```cs
// Create the CloudQueue if it does not exist.
await messageQueue.CreateIfNotExistsAsync();
```

## <a name="add-a-message-to-a-queue"></a>Kuyruğa bir ileti Ekle

Varolan bir sıraya bir ileti eklemek için yeni bir oluşturma `CloudQueueMessage` nesne sonra çağırın `AddMessageAsync` yöntemi. A `CloudQueueMessage` nesne bir dizeden (UTF-8 biçiminde) veya bir bayt dizisi oluşturulabilir.

```cs
// Create a message and add it to the queue.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
await messageQueue.AddMessageAsync(message);
```

## <a name="read-a-message-in-a-queue"></a>Bir kuyruktaki ileti okuma

Kuyruğun önündeki iletiye sıradan çağırarak kaldırmadan iletiye göz atabilirsiniz `PeekMessageAsync` yöntemi:

```cs
// Peek the next message in the queue.
CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();
```

## <a name="read-and-remove-a-message-in-a-queue"></a>Okuma ve bir sıraya bir ileti Kaldır

Kodunuzu kaldırabilirsiniz (dequeue) bir iletiyi bir kuyruktan iki adımda.

1. Çağrı `GetMessageAsync` sonraki iletiyi sıraya alınamadı. Döndürülen bir ileti `GetMessageAsync` iletileri bu sıradan okuyan herhangi bir kod görünmez olur. Varsayılan olarak bu ileti 30 saniye görünmez kalır.
1. İletiyi kuyruktan kaldırmayı tamamlamak için arama `DeleteMessageAsync`.

Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. Aşağıdaki kod çağrıları `DeleteMessageAsync` ileti işlendikten sonra sağ:

```cs
// Get the next message in the queue.
CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

// Process the message in less than 30 seconds.

// Then delete the message.
await messageQueue.DeleteMessageAsync(retrievedMessage);
```

## <a name="additional-options-for-dequeuing-messages"></a>İletilerin kuyruktan alma için ek seçenekleri

Bir sıradan ileti alma özelleştirmek için iki yolu vardır. İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz. Aşağıdaki kod örneğinde `GetMessages` tek çağrıda 20 ileti almak için yöntemi. Her bir iletiyi kullanarak işler sonra bir `foreach` döngü. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır. Beş süreölçer başlatır dakikayı tüm iletiler için aynı zamanda, bu nedenle sonra beş dakika geçtikten, silinmemiş herhangi bir iletisi yeniden görünür hale gelmiştir olduğunu unutmayın.

```cs
// Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
//   delete each message after processing.

foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
{
    // Process all messages in less than 5 minutes, deleting each message after processing.
    queue.DeleteMessage(message);
}
```

## <a name="get-the-queue-length"></a>Kuyruk uzunluğu alma

Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. `FetchAttributes` Yöntemi kuyruk hizmeti ileti sayısı dahil olmak üzere kuyruk özniteliklerini almasını ister. `ApproximateMethodCount` Özelliği tarafından alınan en son değeri döndürür `FetchAttributes` kuyruk hizmetini çağırmadan olmadan yöntemi.

```cs
// Fetch the queue attributes.
messageQueue.FetchAttributes();

// Retrieve the cached approximate message count.
int? cachedMessageCount = messageQueue.ApproximateMessageCount;

// Display the number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>Ortak sıra API'leri ile zaman uyumsuz-bekleme yöntemini kullanın

Bu örnek async kullanmayı gösterir-ortak desenle await ile biten API'leri sıraya `Async`. Async yöntemi kullanıldığında, zaman uyumsuz-bekleme düzeni çağrı tamamlanana kadar yerel çalıştırmayı askıya alır. Bu davranış, performans sorunlarını engellemeye yardımcı olur ve uygulamanızın genel yanıt hızını artırır diğer iş yapmak geçerli iş parçacığının sağlar.

```cs
// Create a message to add to the queue.
CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

// Async enqueue the message.
await messageQueue.AddMessageAsync(cloudQueueMessage);
Console.WriteLine("Message added");

// Async dequeue the message.
CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

// Async delete the message.
await messageQueue.DeleteMessageAsync(retrievedMessage);
Console.WriteLine("Deleted message");
```

## <a name="delete-a-queue"></a>Bir kuyruk silme

Bir kuyruk ve içerdiği tüm iletileri silmek için arama `Delete` nesnesinde yöntemi:

```cs
// Delete the queue.
messageQueue.Delete();
```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
