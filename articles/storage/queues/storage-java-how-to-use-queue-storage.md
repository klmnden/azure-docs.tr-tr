---
title: Java'dan kuyruk depolama kullanma | Microsoft Docs
description: Oluşturmak ve Kuyruklar, silmek için Azure Queue hizmetini kullanmayı öğrenin ve Ekle, Al ve iletilerini silin. Java dilinde yazılmış örnekleri.
services: storage
author: roygara
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/08/2016
ms.author: rogarana
ms.subservice: queues
ms.openlocfilehash: 7c2fbc168c663cc4dc5981071d40540cfc4797ca
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648495"
---
# <a name="how-to-use-queue-storage-from-java"></a>Java’dan Kuyruk depolama kullanma
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuzda Azure kuyruk depolama hizmetini kullanarak, yaygın senaryoları gerçekleştirmek nasıl gösterir. Örnekler Java dilinde yazılır ve [Java için Azure Depolama SDK’sı][Azure Storage SDK for Java] kullanır. Senaryoları ele alınmaktadır **ekleme**, **gözatma**, **alma**, ve **silme** kuyruk iletileri yanı  **oluşturma** ve **silme** sıralar. Kuyruklar hakkında daha fazla bilgi için bkz. [sonraki adımlar](#next-steps) bölümü.

Not: Android cihazlarda Azure Depolama kullanan geliştiriciler için bir SDK mevcuttur. Daha fazla bilgi için bkz. [Android için Azure Depolama SDK’sı][Azure Storage SDK for Android].

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java uygulaması oluşturma
Bu kılavuzda, bir Java uygulama içinde yerel olarak veya bir web rolü veya azure'da çalışan rolü içinde çalışan kodu çalıştırılabilir depolama özelliklerini kullanır.

Bunu yapmak için Java Development Kit (JDK) yükleme ve Azure aboneliğinizde bir Azure depolama hesabı oluşturmanız gerekir. Bunu yaptıktan sonra geliştirme sisteminizde içinde listelenen bağımlılıkları ve en düşük gereksinimleri karşıladığını doğrulamanız gerekir [Java için Azure depolama SDK'sı] [ Azure Storage SDK for Java] github deposu. Sisteminiz bu gereksinimleri karşılıyorsa, indirme ve bu depodan sisteminizde Java için Azure depolama kitaplıkları yükleme yönergelerini takip edebilirsiniz. Bu görevleri tamamladıktan sonra bu makaledeki örneklerde kullanan bir Java uygulaması oluşturmak mümkün olacaktır.

## <a name="configure-your-application-to-access-queue-storage"></a>Kuyruk depolamaya erişmek için uygulamanızı yapılandırma
İçeri aktarma deyimlerini kuyruklarına erişmek için Azure depolama API'leri kullanmak istediğiniz Java dosyasının en üstüne ekleyin:

```java
// Include the following imports to use queue APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.queue.*;
```

## <a name="setup-an-azure-storage-connection-string"></a>Bir Azure depolama bağlantı dizesi ayarlama
Azure depolama istemcisi, veri yönetimi hizmetlerine erişmek üzere uç noktaları ve kimlik bilgilerini depolamak için bir depolama bağlantı dizesi kullanır. Bir istemci uygulamasında çalışırken, depolama hesabınızın adını kullanarak depolama bağlantı dizesi şu biçimde sağlamanız gerekir ve depolama hesabı için birincil erişim anahtarını listelenen [Azure portalı](https://portal.azure.com) için *AccountName* ve *AccountKey* değerleri. Bu örnekte bağlantı dizesini tutmak için nasıl statik bir alan bildirebileceğiniz gösterilmektedir:

```java
// Define the connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

Çalışan bir uygulama içinde Microsoft azure'da bir roldeki, bu dizenin hizmet yapılandırma dosyasında depolanabilir *ServiceConfiguration.cscfg*, çağrısı ile erişilebilir  **RoleEnvironment.getConfigurationSettings** yöntemi. Yapılandırma dosyasındaki *StorageConnectionString* adlı bir **Ayar** öğesinden bağlantı dizesini alma örneği aşağıda verilmiştir:

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

Aşağıdaki örnekler, depolama bağlantı dizesini almak için bu iki yöntemden birini kullandığınızı varsayar.

## <a name="how-to-create-a-queue"></a>Nasıl yapılır: Kuyruk oluştur
A **CloudQueueClient** nesne kuyruklar için başvuru nesneleri almanıza olanak tanır. Aşağıdaki kod oluşturur bir **CloudQueueClient** nesne. (Not: Oluşturmak için ek yolla **CloudStorageAccount** nesnelerini; daha fazla bilgi için bkz **CloudStorageAccount** içinde [Azure Depolama İstemcisi SDK Başvurusu].)

Kullanma **CloudQueueClient** sıranın kullanmak istediğiniz bir başvuru almak için nesne. Yoksa, kuyruk oluşturabilirsiniz.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

   // Create the queue client.
   CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

   // Retrieve a reference to a queue.
   CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Create the queue if it doesn't already exist.
   queue.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-a-message-to-a-queue"></a>Nasıl yapılır: Kuyruğa bir ileti ekleyin
Varolan bir sıraya bir ileti yerleştirmek için ilk olarak yeni bir **CloudQueueMessage** oluşturun. Ardından, arama **addMessage** yöntemi. A **CloudQueueMessage** bir dizeden (UTF-8 biçiminde) veya bayt dizisi oluşturulabilir. İşte (yoksa), bir kuyruk oluşturan kodu ve "Hello, World" iletisini ekler.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.createIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.addMessage(message);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-peek-at-the-next-message"></a>Nasıl yapılır: Sonraki iletiye gözatın
Kuyruğun iletiyi kuyruktan kaldırmadan çağırarak peek **peekMessage**.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Peek at the next message.
    CloudQueueMessage peekedMessage = queue.peekMessage();

    // Output the message value.
    if (peekedMessage != null)
    {
      System.out.println(peekedMessage.getMessageContentAsString());
   }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Nasıl yapılır: Kuyruğa alınan iletinin içeriğini değiştirme
Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. Eğer ileti bir iş görevini temsil ediyorsa, bu özelliği kullanarak iş görevinin durumunu güncelleştirebilirsiniz. Aşağıdaki kod kuyruk iletisini yeni içeriklerle güncelleştirir ve görünürlük zaman aşımını 60 saniye daha uzatır. Bu, ileti ile ilişkili işin durumunu kaydeder ve istemciye ileti üzerinde çalışmaya devam etmesi için bir dakika daha zaman verir. Bir işleme adımı donanım veya yazılım arızasından dolayı başarısız olursa baştan başlamanıza gerek kalmadan kuyruk iletilerindeki çok adımlı iş akışlarını izlemek için bu yöntemi kullanabilirsiniz. Genellikle bir yeniden deneme sayacı tutmanı gerekir ve bir ileti *n* seferden daha fazla yeniden denenirse, silebilirsiniz. Bu, her işlendiğinde bir uygulama hatası tetikleyen bir iletiye karşı koruma sağlar.

Aşağıdaki kod örnek aramaları ileti kuyruğu üzerinden içeriğini, "Hello, World" eşleşen sonra içerik ileti değiştirir ve çıkar ilk iletiyi bulur.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // The maximum number of messages that can be retrieved is 32.
    final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

    // Loop through the messages in the queue.
    for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
    {
        // Check for a specific string.
        if (message.getMessageContentAsString().equals("Hello, World"))
        {
            // Modify the content of the first matching message.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 30 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 30, updateFields, null, null);
            break;
        }
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

Alternatif olarak, aşağıdaki kod örneği, yalnızca ilk görünür ileti sıraya güncelleştirir.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage message = queue.retrieveMessage();

    if (message != null)
    {
        // Modify the message content.
        message.setMessageContent("Updated contents.");
        // Set it to be visible in 60 seconds.
        EnumSet<MessageUpdateFields> updateFields =
            EnumSet.of(MessageUpdateFields.CONTENT,
            MessageUpdateFields.VISIBILITY);
        // Update the message.
        queue.updateMessage(message, 60, updateFields, null, null);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-get-the-queue-length"></a>Nasıl yapılır: Kuyruk uzunluğu alma
Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. **DownloadAttributes** kaç iletiler bir kuyrukta olan sayısına dahil yöntemi çeşitli geçerli değerler için kuyruk hizmeti sorar. İletileri eklenebilir veya kuyruk hizmeti, isteğine yanıt vermeden sonra kaldırılan sayısı yalnızca yaklaşık olmasıdır. **GetApproximateMessageCount** yöntem çağrısı tarafından alınan en son değeri döndürür **downloadAttributes**, kuyruk hizmetini çağırmadan olmadan.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Download the approximate message count from the server.
    queue.downloadAttributes();

    // Retrieve the newly cached approximate message count.
    long cachedMessageCount = queue.getApproximateMessageCount();

    // Display the queue length.
    System.out.println(String.format("Queue length: %d", cachedMessageCount));
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-dequeue-the-next-message"></a>Nasıl yapılır: Sonraki iletiyi sıradan çıkar
Kodunuzun bir iletiyi bir kuyruktan iki adımda dequeues. Çağırdığınızda **retrieveMessage**, sonraki iletiyi bir kuyruğa alın. Öğesinden döndürülen bir ileti **retrieveMessage** bu kuyruktan iletileri okuyan herhangi bir kod için görünmez hale gelir. Varsayılan olarak bu ileti 30 saniye görünmez kalır. İletiyi kuyruktan kaldırmayı tamamlamak için de çağırmanız gerekir **deleteMessage**. Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. Kod çağrılarınızı **deleteMessage** ileti işlendikten sonra sağ.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage retrievedMessage = queue.retrieveMessage();

    if (retrievedMessage != null)
    {
        // Process the message in less than 30 seconds, and then delete the message.
        queue.deleteMessage(retrievedMessage);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="additional-options-for-dequeuing-messages"></a>İletileri sıradan çıkarmak için ek seçenekler
İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz. İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz.

Aşağıdaki kod örneğinde **retrieveMessages** tek çağrıda 20 ileti almak için yöntemi. Her bir iletiyi kullanarak işler sonra bir **için** döngü. Beş dakika (300 saniye) her ileti için görünmezlik zaman aşımı da ayarlar. Beş dakikada başlatılır çağrısından sonra tüm iletiler aynı zamanda, bu nedenle zaman beş dakika geçtikten için Not **retrieveMessages**, silinmeyen tüm iletiler yeniden görünür hale gelir.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
        // Do processing for all messages in less than 5 minutes,
        // deleting each message after processing.
        queue.deleteMessage(message);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-the-queues"></a>Nasıl yapılır: Kuyrukların listesi
Geçerli kuyruk listesini almak için çağrı **CloudQueueClient.listQueues()** koleksiyonunu döndürür yöntemi **CloudQueue** nesneleri.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient =
        storageAccount.createCloudQueueClient();

    // Loop through the collection of queues.
    for (CloudQueue queue : queueClient.listQueues())
    {
        // Output each queue name.
        System.out.println(queue.getName());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-queue"></a>Nasıl yapılır: Bir kuyruk silme
Bir kuyruk ve içerdiği tüm iletileri silmek için çağrı **deleteIfExists** metodunda **CloudQueue** nesne.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Delete the queue if it exists.
    queue.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a>Sonraki adımlar
Kuyruk depolamanın temellerini öğrendiğinize göre daha karmaşık depolama görevleri hakkında bilgi edinmek için bu bağlantıları izleyin.

* [Java için Azure Depolama SDK'sı][Azure Storage SDK for Java]
* [Azure Depolama İstemcisi SDK Başvurusu][Azure Depolama İstemcisi SDK Başvurusu]
* [Azure depolama hizmetleri REST API'si][Azure Storage Services REST API]
* [Azure Depolama Ekibi Blogu][Azure Storage Team Blog]

[Azure SDK for Java]: https://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Depolama İstemcisi SDK Başvurusu]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage Services REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: https://blogs.msdn.com/b/windowsazurestorage/
