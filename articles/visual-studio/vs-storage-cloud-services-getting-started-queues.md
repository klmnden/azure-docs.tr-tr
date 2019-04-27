---
title: Kuyruk depolama ve Visual Studio ile çalışmaya başlama (bulut Hizmetleri) Hizmetleri bağlı | Microsoft Docs
description: Visual Studio kullanarak bir depolama hesabına bağlandıktan sonra bir bulut hizmeti projesini Visual Studio'da Azure kuyruk depolama ile çalışmaya başlamak nasıl bağlı hizmetler
services: storage
author: ghogen
manager: douge
ms.assetid: da587aac-5e64-4e9a-8405-44cc1924881d
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 28a7de1b43d793641237197aea841022996b07e3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60505638"
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Azure kuyruk depolama ve Visual Studio ile çalışmaya başlama (bulut) bağlı Hizmetleri projeleri
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede oluşturduğunuz veya Visual Studio kullanarak bir Azure depolama hesabı bulut Hizmetleri projesinde başvurulan sonra Visual Studio'da Azure kuyruk depolama kullanmaya başlama işlemini açıklamaktadır **bağlı hizmet Ekle** iletişim.

Kodda bir kuyruk oluşturulacağını göstereceğiz. Ayrıca, ekleme, değiştirme, okuma ve kuyruk iletilerine kaldırma gibi temel kuyruk işlemlerini gerçekleştirmek nasıl göstereceğiz. C# kodu ve kullanım örnekleri yazılır [.NET için Microsoft Azure depolama istemci Kitaplığı](https://msdn.microsoft.com/library/azure/dn261237.aspx).

**Bağlı hizmet Ekle** işlemi projenizde Azure depolamaya erişmek için uygun NuGet paketlerini yükler ve proje yapılandırma dosyalarınızı depolama hesabı için bağlantı dizesi ekler.

* Bkz: [.NET kullanarak Azure kuyruk depolama ile çalışmaya başlama](../storage/queues/storage-dotnet-how-to-use-queues.md) kuyrukları kod düzenleme hakkında daha fazla bilgi.
* Bkz: [depolama belgeleri](https://azure.microsoft.com/documentation/services/storage/) Azure depolama hakkında genel bilgiler.
* Bkz: [bulut Hizmetleri belgeleri](https://azure.microsoft.com/documentation/services/cloud-services/) Azure bulut hizmetleri hakkında genel bilgiler.
* Bkz: [ASP.NET](https://www.asp.net) ASP.NET uygulamalarını programlama hakkında daha fazla bilgi.

Azure Kuyruk depolama, HTTP veya HTTPS kullanan kimlik doğrulaması yapılmış çağrılar aracılığıyla dünyanın her yerinden erişilebilen çok sayıda iletinin depolanması için bir hizmettir. Tek bir kuyruk iletisinin boyutu 64 KB’ye kadar olabilir ve bir kuyrukta, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti bulunabilir.

## <a name="access-queues-in-code"></a>Kod erişim kuyruklar
Visual Studio Cloud Services projelerde kuyrukları erişmek için Azure kuyruk depolamaya erişen tüm C# kaynak dosyası aşağıdaki öğelere eklemeniz gerekir.

1. Bu ad alanı bildirimi C# dosyası üst kısmındaki eklediğinizden emin olun **kullanarak** deyimleri.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
2. Alma bir **CloudStorageAccount** depolama hesap bilgilerini temsil eden nesne. Almak için aşağıdaki kodu kullanın. Azure hizmet yapılandırma depolama hesabı bilgilerini ve depolama bağlantı dizesi.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. Alma bir **CloudQueueClient** depolama hesabınızda sıra nesneleri başvurmak için nesne.  
   
        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. Alma bir **CloudQueue** belirli bir kuyruğa başvurmak için nesne.
   
        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

**NOT:** Tüm kod önünde Yukarıdaki kod, aşağıdaki örnekleri kullanın.

## <a name="create-a-queue-in-code"></a>Kodda bir kuyruk oluşturun
Sıra kodu oluşturmak için yalnızca bir çağrı ekleyin **Createıfnotexists**.

    // Create the CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Kuyruğa bir ileti ekleyin
Varolan bir kuyruğa ileti eklemek için yeni bir oluşturma **CloudQueueMessage** nesnesi ve ardından arama **AddMessage** yöntemi.

A **CloudQueueMessage** nesne oluşturulabilir, bir dizeden (UTF-8 biçiminde) veya bayt dizisi.

'Hello, World' iletisini ekleyen bir örnek aşağıda verilmiştir.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Kuyruktaki bir iletiyi okuyun
**PeekMessage** yöntemini çağırarak iletiyi kuyruktan kaldırmadan kuyruğun önündeki iletiye göz atabilirsiniz.

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Okuma ve bir kuyrukta bir ileti Kaldır
Kodunuzu kaldırabilirsiniz (kuyruktan) bir iletiyi bir kuyruktan iki adımda.

1. Çağrı **GetMessage** sonraki iletiyi kuyruğa alınacak. **GetMessage**’dan dönen bir ileti bu kuyruktaki kod okuyan iletilere karşı görünmez olur. Varsayılan olarak bu ileti 30 saniye görünmez kalır.
2. İletiyi kuyruktan kaldırmayı tamamlamak için çağrı **DeleteMessage**.

Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. Aşağıdaki kod çağrıları **DeleteMessage** ileti işlendikten sonra sağ.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-to-process-and-remove-queue-messages"></a>İşlem ve kuyruk iletilerine kaldırmak için ek seçenekleri kullanın
İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz.

* Toplu iletiler (en fazla 32) elde edebilirsiniz.
* Uzun veya daha kısa bir görünmezlik zaman aşımı, kodunuzu daha fazla veya daha az zaman her iletiyi tamamen işlemesi için izin verebilirsiniz. Aşağıdaki kod örneğinde tek çağrıda 20 ileti almak için **GetMessages** yöntemi kullanılmıştır. Ardından her ileti bir **foreach** döngüsü ile işlenir. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır. 5 dakikalık sürenin tüm iletiler için aynı zamanda başladığını unutmayın, bu nedenle **GetMessages** çağrısından itibaren 5 dakika geçtikten sonra silinmeyen tüm iletiler görünür olacaktır.

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

## <a name="use-the-async-await-pattern-with-common-azure-queue-apis"></a>Yaygın Azure kuyruk API'leri ile zaman uyumsuz-bekleme yöntemi kullanın
Bu örnek, ortak Azure kuyruk API'leri ile zaman uyumsuz-bekleme yöntemi kullanmayı gösterir. Örnek belirtilen yöntemlerin her biri zaman uyumsuz sürümü çağırır, bu tarafından görülebilir **zaman uyumsuz** her yöntemin sonrası düzeltme. Zaman uyumsuz bir yöntem kullanıldığında async-await deseni çağrı tamamlanana kadar yerel çalıştırmayı askıya alır. Bu davranış, performans sorunlarını önlemeye yardımcı olur ve uygulamanızın genel yanıt hızını artıran başka işleri yapmak geçerli iş parçacığı sağlar. .NET’te Zaman Uyumsuz-Bekleme yönteminin kullanılması ile ilgili daha fazla ayrıntı için bkz. [Zaman Uyumsuz ve Bekleme (C# ve Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

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

