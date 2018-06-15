---
title: Kuyruk depolama ve Visual Studio ile çalışmaya başlama bağlı Hizmetleri (bulut Hizmetleri) | Microsoft Docs
description: Visual Studio kullanarak bir depolama hesabı bağlandıktan sonra bir bulut hizmeti projesini Visual Studio kullanarak Azure kuyruk depolamaya başlama Hizmetleri bağlı
services: storage
author: ghogen
manager: douge
ms.assetid: da587aac-5e64-4e9a-8405-44cc1924881d
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: c856bfb691c8d1b43822718bbfb86ff2122f4988
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31795611"
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Azure kuyruk depolama ve Visual Studio ile çalışmaya başlama (Projeler bulut Hizmetleri) Hizmetleri bağlı
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede nasıl oluşturduğunuz veya Visual Studio kullanarak bir Azure depolama hesabı bulut Hizmetleri projesinde başvurulan sonra Visual Studio'da Azure kuyruk depolama kullanarak başlayacağınızı **bağlı Hizmetleri Ekle** iletişim.

Kodda bir kuyruk oluşturulacağını göstereceğiz. Ayrıca, ekleme, değiştirme, okuma ve iletileri kuyruğa kaldırma gibi temel kuyruk işlemlerini gerçekleştirmek nasıl göstereceğiz. Örnekler C# kodu ve kullanım yazılır [.NET için Microsoft Azure Storage istemci Kitaplığı](https://msdn.microsoft.com/library/azure/dn261237.aspx).

**Bağlı Hizmetleri Ekle** işlemi Azure depolama projenize erişmek için uygun NuGet paketlerini yükler ve proje yapılandırma dosyalarınızı depolama hesabı için bağlantı dizesi ekler.

* Bkz: [.NET kullanarak Azure kuyruk depolamaya başlayın](../storage/queues/storage-dotnet-how-to-use-queues.md) kod kuyruklarda düzenleme hakkında daha fazla bilgi.
* Bkz: [Storage belgeleri](https://azure.microsoft.com/documentation/services/storage/) Azure Storage hakkında genel bilgiler.
* Bkz: [bulut Hizmetleri belgelerinde](https://azure.microsoft.com/documentation/services/cloud-services/) Azure bulut hizmetleri hakkında genel bilgi için.
* Bkz: [ASP.NET](http://www.asp.net) ASP.NET uygulamalarını programlama hakkında daha fazla bilgi.

Azure Kuyruk depolama, HTTP veya HTTPS kullanan kimlik doğrulaması yapılmış çağrılar aracılığıyla dünyanın her yerinden erişilebilen çok sayıda iletinin depolanması için bir hizmettir. Tek bir kuyruk iletisinin boyutu 64 KB’ye kadar olabilir ve bir kuyrukta, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti bulunabilir.

## <a name="access-queues-in-code"></a>Kod erişim kuyruklar
Visual Studio bulut Hizmetleri projeleri kuyruklarda erişmek için Azure kuyruk depolama erişim aşağıdaki öğeler herhangi C# kaynak dosyaya eklemeniz gerekir.

1. Ad alanı bildirimlerini dosyanın üst kısmındaki C# bu eklediğinizden emin olun **kullanarak** deyimleri.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
2. Alma bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Almak için aşağıdaki kodu kullanın depolama bağlantı dizesini ve Azure hizmet yapılandırma depolama hesabı bilgileri.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. Alma bir **CloudQueueClient** depolama hesabınızda sıra nesneleri başvurmak için.  
   
        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. Alma bir **CloudQueue** belirli bir kuyruğa başvurmak için.
   
        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

**Not:** tüm kod önünde Yukarıdaki kod aşağıdaki örneklerde kullanın.

## <a name="create-a-queue-in-code"></a>Kodda bir sıra oluşturun
Kodda sırayı oluşturmak için yalnızca bir çağrı ekleyin **CreateIfNotExists**.

    // Create the CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Kuyruğa bir ileti Ekle
Varolan bir sıraya bir ileti eklemek için yeni bir oluşturma **CloudQueueMessage** nesne sonra çağırın **AddMessage** yöntemi.

A **CloudQueueMessage** bir dizeden (UTF-8 biçiminde) veya bir bayt dizisi nesne oluşturulabilir.

Burada, 'Hello, World' iletisini ekleyen bir örnek verilmiştir.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Bir kuyruktaki ileti okuma
**PeekMessage** yöntemini çağırarak iletiyi kuyruktan kaldırmadan kuyruğun önündeki iletiye göz atabilirsiniz.

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Okuma ve bir sıraya bir ileti Kaldır
Kodunuzu kaldırabilirsiniz (kuyruktan) bir iletiyi bir kuyruktan iki adımda.

1. Çağrı **GetMessage** sonraki iletiyi sıraya alınamadı. **GetMessage**’dan dönen bir ileti bu kuyruktaki kod okuyan iletilere karşı görünmez olur. Varsayılan olarak bu ileti 30 saniye görünmez kalır.
2. İletiyi kuyruktan kaldırmayı tamamlamak için arama **DeleteMessage**.

Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. Aşağıdaki kod çağrıları **DeleteMessage** ileti işlendikten sonra sağ.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-to-process-and-remove-queue-messages"></a>İşlem ve iletileri kuyruğa kaldırmak için ek seçenekleri kullanın
İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz.

* Toplu (en fazla 32) iletiler alabilirsiniz.
* Uzun veya daha kısa bir görünmezlik zaman aşımı kodunuzun her iletiyi tamamen işlemesi için zaman daha az veya daha fazla izin verebilirsiniz. Aşağıdaki kod örneğinde tek çağrıda 20 ileti almak için **GetMessages** yöntemi kullanılmıştır. Ardından her ileti bir **foreach** döngüsü ile işlenir. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır. 5 dakikalık sürenin tüm iletiler için aynı zamanda başladığını unutmayın, bu nedenle **GetMessages** çağrısından itibaren 5 dakika geçtikten sonra silinmeyen tüm iletiler görünür olacaktır.

Bir örneği aşağıda verilmiştir:

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete the message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-the-queue-length"></a>Kuyruk uzunluğu alma
Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. **FetchAttributes** yöntemi, ileti sayısı dahil olmak üzere Kuyruk hizmetinden kuyruk özniteliklerini almasını ister. **ApproximateMethodCount** özelliği tarafından alınan en son değeri döndürür **FetchAttributes** kuyruk hizmetini çağırmadan olmadan yöntemi.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-azure-queue-apis"></a>Ortak Azure sıra API'leri ile zaman uyumsuz-bekleme yöntemini kullanma
Bu örnek nasıl ortak Azure sıra API'leri ile zaman uyumsuz-bekleme yönteminin kullanılacağını gösterir. Verilen yöntemlerin her biri zaman uyumsuz sürümü örnek çağırır, bu tarafından görülebilir **zaman uyumsuz** yönteminin her sonrası düzeltme. Zaman uyumsuz yöntem kullanıldığında zaman uyumsuz-bekleme düzeni çağrı tamamlanana kadar yerel çalıştırmayı askıya alır. Bu davranış geçerli iş parçacığının performans sorunlarını engellemeye yardımcı olmak ve uygulamanızın genel yanıt hızını artırır başka işler yapmasını sağlar. .NET’te Zaman Uyumsuz-Bekleme yönteminin kullanılması ile ilgili daha fazla ayrıntı için bkz. [Zaman Uyumsuz ve Bekleme (C# ve Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add the message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete the message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Bir kuyruk silme
Bir kuyruğu ve içinde yer alan tüm iletileri silmek için kuyruk nesnesindeki **Sil** yöntemini çağırın.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

