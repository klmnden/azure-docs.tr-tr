---
title: Kuyruk depolama ve Visual Studio ile çalışmaya başlama bağlı hizmetler (ASP.NET Core) | Microsoft Docs
description: Nasıl bir ASP.NET Core projesinde Visual Studio'da Azure kuyruk depolama kullanmaya başlama
services: storage
author: ghogen
manager: douge
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: article
ms.date: 11/14/2017
ms.author: ghogen
ms.openlocfilehash: 8a91614e7dfb804e6a902967ce60f898ed0e54ad
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60508482"
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a>Kuyruk depolama ve Visual Studio ile çalışmaya başlama bağlı hizmetler (ASP.NET Core)

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

Bu makalede oluşturduğunuz veya Visual Studio kullanarak bir ASP.NET Core projesi bir Azure depolama hesabında başvurulan sonra Visual Studio'da Azure kuyruk depolama kullanmaya başlama işlemini açıklamaktadır **bağlı hizmetler** özelliği. **Bağlı hizmetler** işlemi projenizde Azure depolamaya erişmek için uygun NuGet paketlerini yükler ve proje yapılandırma dosyalarınızı depolama hesabı için bağlantı dizesi ekler. (Bkz [depolama belgeleri](https://azure.microsoft.com/documentation/services/storage/) Azure depolama hakkında genel bilgiler.)

Azure kuyruk depolama, çok sayıda herhangi bir HTTP veya HTTPS kullanılarak kimliği doğrulanmış aramalar yoluyla dünyanın erişilebilen iletileri depolamak için kullanılan bir hizmettir. Tek bir kuyruk iletisinin 64 kilobayt (KB) boyutu olabilir ve bir kuyruk iletileri, depolama hesabının toplam kapasite sınırına kadar milyonlarca içerebilir. Ayrıca bkz: [.NET kullanarak Azure kuyruk depolama ile çalışmaya başlama](../storage/queues/storage-dotnet-how-to-use-queues.md) program aracılığıyla kuyruklar düzenleme hakkında bilgi.

Kullanmaya başlamak için öncelikle bir Azure kuyruk depolama hesabınızdaki oluşturun. Bu makalede daha sonra bir sıra C# dilinde oluşturma ve ekleme, değiştirme, okuma ve kuyruk iletilerine kaldırma gibi temel kuyruk işlemlerini nasıl gerçekleştireceğinizi gösterir.  Kod, .NET için Azure depolama istemci kitaplığı kullanır. ASP.NET hakkında daha fazla bilgi için bkz: [ASP.NET](https://www.asp.net).

Bazı Azure depolama API'leri uyumsuzdur ve bu makalede kod zaman uyumsuz yöntemler kullanıldığını varsayar. Bkz: [zaman uyumsuz programlama](https://docs.microsoft.com/dotnet/csharp/async) daha fazla bilgi için.

## <a name="access-queues-in-code"></a>Kod erişim kuyruklar

ASP.NET Core projelerinde kuyruklarına erişmek için Azure kuyruk depolama erişen tüm C# kaynak dosyası aşağıdaki öğeleri ekleyin. Tüm kod önüne bu kod, aşağıdaki bölümlerde kullanın.

1. Gerekli Ekle `using` ifadeleri:
    ```cs
    using Microsoft.Framework.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Framework.Logging.LogLevel;
    ```

1. Alma bir `CloudStorageAccount` depolama hesap bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından bir depolama bağlantı dizesi ve depolama hesabı bilgileri almak için aşağıdaki kodu kullanın:

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

### <a name="create-a-queue-in-code"></a>Kodda bir kuyruk oluşturun

Azure kuyruk kodu oluşturmak için arama `CreateIfNotExistsAsync`:

```cs
// Create the CloudQueue if it does not exist.
await messageQueue.CreateIfNotExistsAsync();
```

## <a name="add-a-message-to-a-queue"></a>Kuyruğa bir ileti ekleyin

Varolan bir kuyruğa ileti eklemek için yeni bir oluşturma `CloudQueueMessage` nesnesi ve ardından arama `AddMessageAsync` yöntemi. A `CloudQueueMessage` nesne oluşturulabilir, bir dizeden (UTF-8 biçiminde) veya bayt dizisi.

```cs
// Create a message and add it to the queue.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
await messageQueue.AddMessageAsync(message);
```

## <a name="read-a-message-in-a-queue"></a>Kuyruktaki bir iletiyi okuyun

Kuyruğun iletiyi kuyruktan kaldırmadan çağırarak peek `PeekMessageAsync` yöntemi:

```cs
// Peek the next message in the queue.
CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();
```

## <a name="read-and-remove-a-message-in-a-queue"></a>Okuma ve bir kuyrukta bir ileti Kaldır

Kodunuzu kaldırabilirsiniz (sıradan çıkarma) bir iletiyi bir kuyruktan iki adımda.

1. Çağrı `GetMessageAsync` sonraki iletiyi kuyruğa alınacak. Öğesinden döndürülen bir ileti `GetMessageAsync` bu kuyruktan iletileri okuyan herhangi bir kod için görünmez hale gelir. Varsayılan olarak bu ileti 30 saniye görünmez kalır.
1. İletiyi kuyruktan kaldırmayı tamamlamak için çağrı `DeleteMessageAsync`.

Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. Aşağıdaki kod çağrıları `DeleteMessageAsync` ileti işlendikten sonra sağ:

```cs
// Get the next message in the queue.
CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

// Process the message in less than 30 seconds.

// Then delete the message.
await messageQueue.DeleteMessageAsync(retrievedMessage);
```

## <a name="additional-options-for-dequeuing-messages"></a>İletileri sıradan çıkarmak için ek seçenekler

Bir kuyruktan ileti alma özelleştirmek için iki yolu vardır. İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz. Aşağıdaki kod örneğinde `GetMessages` tek çağrıda 20 ileti almak için yöntemi. Her bir iletiyi kullanarak işler sonra bir `foreach` döngü. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır. Beş süreölçer başlatır dakika tüm iletiler için aynı zamanda, böylece sonra beş dakika geçtikten, silinmemiş iletiler yeniden görünür hale geleceğini unutmayın.

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

Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. `FetchAttributes` Yöntemi kuyruk hizmetinin ileti sayısı dahil olmak üzere kuyruk özniteliklerini almasını ister. `ApproximateMethodCount` Özelliği tarafından alınan en son değeri döndürür `FetchAttributes` kuyruk hizmetini çağırmadan olmadan yöntemi.

```cs
// Fetch the queue attributes.
messageQueue.FetchAttributes();

// Retrieve the cached approximate message count.
int? cachedMessageCount = messageQueue.ApproximateMessageCount;

// Display the number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>Ortak kuyruk API'leri ile zaman uyumsuz-bekleme yöntemi kullanın

Bu örnek, zaman uyumsuz işlemi gösterilir-genel bir desenle await API'leri ile biten sıra `Async`. Zaman uyumsuz bir yöntem kullanıldığında, async-await deseni çağrı tamamlanana kadar yerel çalıştırmayı askıya alır. Bu davranış geçerli iş parçacığı performans sorunlarını önlemeye yardımcı olur ve uygulamanızın genel yanıt hızını artıran başka işler yapmasını sağlar.

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

Bir kuyruk ve içerdiği tüm iletileri silmek için çağrı `Delete` kuyruk nesnesi üzerinde yöntemi:

```cs
// Delete the queue.
messageQueue.Delete();
```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
