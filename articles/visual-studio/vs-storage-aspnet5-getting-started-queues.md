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
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 394344c0e126472b97c2e8f721c8c8d6514a17dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a>Kuyruk depolama ve Visual Studio ile çalışmaya başlama bağlı Hizmetleri (ASP.NET çekirdek)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede nasıl oluşturduğunuz veya Visual Studio kullanarak bir ASP.NET Core projesini bir Azure depolama hesabında başvurulan sonra Visual Studio'da Azure kuyruk depolama kullanarak başlayacağınızı **bağlı Hizmetleri Ekle** iletişim. **Bağlı Hizmetleri Ekle** işlemi Azure depolama projenize erişmek için uygun NuGet paketlerini yükler ve proje yapılandırma dosyalarınızı depolama hesabı için bağlantı dizesi ekler.

Azure kuyruk depolama, çok sayıda herhangi bir yere HTTP veya HTTPS kullanarak kimlik doğrulaması yapılmış çağrılar aracılığıyla erişilebilen iletileri depolamak için bir hizmettir. Tek bir kuyruk iletisinin 64 kilobayt (KB) boyutunda olabilir ve bir kuyruk iletileri, bir depolama hesabının toplam kapasite sınırına kadar milyonlarca içerebilir.

Başlamak için ilk Azure kuyruk depolama hesabınızdaki oluşturmanız gerekir. Kodda bir kuyruk oluşturulacağını göstereceğiz. Ayrıca, ekleme, değiştirme, okuma ve iletileri kuyruğa kaldırma gibi temel kuyruk işlemlerini gerçekleştirmek nasıl göstereceğiz. Örnekler C yazılır\# kod ve .NET için Azure Storage istemci kitaplığı kullanın. ASP.NET hakkında daha fazla bilgi için bkz: [ASP.NET](http://www.asp.net).

**Not:** bazı Azure depolama çağrıları ASP.NET Core gerçekleştirdiğinizde API'leri zaman uyumsuzdur. Bkz: [uyumsuz ve bekleme ile zaman uyumsuz programlama](http://msdn.microsoft.com/library/hh191443.aspx) daha fazla bilgi için. Aşağıdaki kodu, zaman uyumsuz programlama yöntemleri kullanıldığı varsayılmaktadır.

* Bkz: [.NET kullanarak Azure kuyruk depolamaya başlayın](../storage/queues/storage-dotnet-how-to-use-queues.md) program aracılığıyla Kuyrukları düzenleme hakkında daha fazla bilgi.
* Bkz: [Storage belgeleri](https://azure.microsoft.com/documentation/services/storage/) Azure Storage hakkında genel bilgiler.
* Bkz: [bulut Hizmetleri belgelerinde](https://azure.microsoft.com/documentation/services/cloud-services/) Azure bulut hizmetleri hakkında genel bilgi için.
* Bkz: [ASP.NET](http://www.asp.net) ASP.NET uygulamalarını programlama hakkında daha fazla bilgi.

## <a name="access-queues-in-code"></a>Kod erişim kuyruklar
ASP.NET Core projeleri kuyruklarda erişmek için aşağıdaki öğeleri Azure kuyruk depolama erişen tüm C# kaynak dosyasına eklemeniz gerekir.

1. Ad alanı bildirimlerini dosyanın üst kısmındaki C# bu eklediğinizden emin olun **kullanarak** deyimleri.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Alma bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Almak için aşağıdaki kodu kullanın depolama bağlantı dizesini ve Azure hizmet yapılandırma depolama hesabı bilgileri.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. Alma bir **CloudQueueClient** depolama hesabınızda sıra nesneleri başvurmak için.  
   
        // Create the CloudQueueClient object for the storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. Alma bir **CloudQueue** belirli bir kuyruğa başvurmak için.
   
        // Get a reference to the CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

**Not:** tüm kod önünde Yukarıdaki kod aşağıdaki örneklerde kullanın.

### <a name="create-a-queue-in-code"></a>Kodda bir sıra oluşturun
Kodda Azure kuyruk oluşturmak için yalnızca bir çağrı ekleyin **CreateIfNotExistsAsync**.

    // Create the CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

## <a name="add-a-message-to-a-queue"></a>Kuyruğa bir ileti Ekle
Varolan bir sıraya bir ileti eklemek için yeni bir oluşturma **CloudQueueMessage** nesne sonra çağırın **AddMessageAsync** yöntemi.

A **CloudQueueMessage** bir dizeden (UTF-8 biçiminde) veya bir bayt dizisi nesne oluşturulabilir.

Burada, 'Hello, World' iletisini ekleyen bir örnek verilmiştir.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

## <a name="read-a-message-in-a-queue"></a>Bir kuyruktaki ileti okuma
Kuyruğun önündeki iletiye sıradan çağırarak kaldırmadan iletiye göz atabilirsiniz **PeekMessageAsync** yöntemi.

    // Peek the next message in the queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


## <a name="read-and-remove-a-message-in-a-queue"></a>Okuma ve bir sıraya bir ileti Kaldır
Kodunuzu kaldırabilirsiniz (dequeue) bir iletiyi bir kuyruktan iki adımda.

1. Çağrı **GetMessageAsync** sonraki iletiyi sıraya alınamadı. Döndürülen bir ileti **GetMessageAsync** iletileri bu sıradan okuyan herhangi bir kod görünmez olur. Varsayılan olarak bu ileti 30 saniye görünmez kalır.
2. İletiyi kuyruktan kaldırmayı tamamlamak için arama **DeleteMessageAsync**.

Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. Aşağıdaki kod çağrıları **DeleteMessageAsync** ileti işlendikten sonra sağ.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process the message in less than 30 seconds.

    // Then delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a>İletilerin kuyruktan alma için ek seçenekler yararlanın
İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz.
İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz. Aşağıdaki kod örneğinde tek çağrıda 20 ileti almak için **GetMessages** yöntemi kullanılmıştır. Ardından her ileti bir **foreach** döngüsü ile işlenir. Ayrıca her ileti için 5 dakika için görünmezlik zaman aşımı ayarlar. Tüm iletiler için aynı anda 5 dakika başlayan Not böylece sonra 5 dakika geçirilen çağrısından sonra **GetMessages**, silinmemiş herhangi ileti yeniden görünür hale gelmiştir.

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Kuyruk uzunluğu alma
Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. **FetchAttributes** yöntemi kuyruk hizmeti ileti sayısı dahil olmak üzere kuyruk özniteliklerini almasını ister. **ApproximateMethodCount** özelliği tarafından alınan en son değeri döndürür **FetchAttributes** kuyruk hizmetini çağırmadan olmadan yöntemi.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display the number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>Ortak sıra API'leri ile zaman uyumsuz-bekleme yöntemini kullanın
Bu örnek nasıl ortak sırası API'leri ile zaman uyumsuz-bekleme yönteminin kullanılacağını gösterir. Örnek belirtilen yöntemlerin her biri zaman uyumsuz sürümü çağırır. Bu zaman uyumsuz sonrası düzeltme her yöntemi tarafından görülebilir. Async yöntemi kullanıldığında, zaman uyumsuz-bekleme yöntemi çağrı tamamlanana kadar yerel yürütme askıya alır. Bu davranış geçerli iş parçacığının performans sorunlarını engellemeye yardımcı olmak ve uygulamanızın genel yanıt hızını artırır başka işler yapmasını sağlar. . NET'te zaman uyumsuz-bekleme yönteminin kullanılması ile ilgili daha fazla ayrıntı için bkz: [zaman uyumsuz ve bekleme (C# ve Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

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
## <a name="delete-a-queue"></a>Bir kuyruk silme
Bir kuyruğu ve içinde yer alan tüm iletileri silmek için kuyruk nesnesindeki **Sil** yöntemini çağırın.

    // Delete the queue.
    messageQueue.Delete();


## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

