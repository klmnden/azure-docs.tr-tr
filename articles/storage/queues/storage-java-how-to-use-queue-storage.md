---
title: Java'dan kuyruk depolama kullanma | Microsoft Docs
description: "Oluşturmak ve Kuyruklar silmek için Azure Queue hizmetini kullanmayı öğrenin ve Ekle, Al ve iletilerini silin. Java'da yazılmış örneklerini içerir."
services: storage
documentationcenter: java
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 68cecc8e-38c9-4a24-99e8-cb722bc63cf9
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 6735e247393e47ed18049c8055eb92b990e8bb02
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-queue-storage-from-java"></a>Java’dan Kuyruk depolama kullanma
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz Azure kuyruk depolama hizmeti kullanılarak yaygın senaryolar gerçekleştirmek nasıl yapacağınızı gösterir. Java ve kullanım örnekleri yazılır [Java için Azure depolama SDK'sı][Azure Storage SDK for Java]. Kapsamdaki senaryolar dahil **ekleme**, **gözatma**, **alma**, ve **silme** kuyruk iletileri yanı  **oluşturma** ve **silme** sıralar. Kuyruklar hakkında daha fazla bilgi için bkz: [sonraki adımlar](#Next-Steps) bölümü.

Not: Bir SDK'sı Android cihazlarda Azure depolama kullanan geliştiriciler için kullanılabilir. Daha fazla bilgi için bkz: [Android için Azure depolama SDK'sı][Azure Storage SDK for Android].

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java uygulaması oluşturma
Bu kılavuzda, bir Java uygulaması içinde yerel olarak veya bir web rolü veya Azure çalışan rolünde çalışan kodu çalıştırılabilir depolama özelliklerini kullanır.

Bunu yapmak için Java Geliştirme Seti (JDK) yükleyin ve Azure aboneliğinizde bir Azure depolama hesabı oluşturmanız gerekir. Bunu yaptıktan sonra geliştirme sisteminizde içinde listelenen bağımlılıkları ve en düşük gereksinimleri karşıladığını doğrulamanız gerekir [Java için Azure depolama SDK'sı] [ Azure Storage SDK for Java] github'daki. Sisteminiz bu gereksinimleri karşılıyorsa, indirme ve bu depodan sisteminizdeki Java için Azure depolama kitaplıkları yükleme yönergelerini izleyebilirsiniz. Bu görevleri tamamladığınızda, bu makaledeki örnekler kullanan bir Java uygulaması oluşturmak mümkün olacaktır.

## <a name="configure-your-application-to-access-queue-storage"></a>Kuyruk depolama erişmek için uygulamanızı yapılandırın
Aşağıdaki içeri aktarma deyimlerini Azure depolama API'leri sıraları erişmek için kullanmasını istediğiniz Java dosyasının üstüne ekleyin:

```java
// Include the following imports to use queue APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.queue.*;
```

## <a name="setup-an-azure-storage-connection-string"></a>Bir Azure depolama bağlantı dizesini ayarlayın
Bir Azure storage istemci uç noktaları ve Veri Yönetimi Hizmetleri erişmek için kimlik bilgilerini depolamak için bir depolama bağlantı dizesi kullanır. Bir istemci uygulamasında çalıştırırken, depolama hesabınızın adını kullanarak depolama bağlantı dizesi şu biçimde sağlamanız gerekir ve depolama hesabı için birincil erişim anahtarını listelenen [Azure Portal](https://portal.azure.com) için *AccountName* ve *AccountKey* değerleri. Bu örnek, bağlantı dizesi tutmak için statik bir alana nasıl bildirebilir gösterir:

```java
// Define the connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

Çalışan bir uygulama içinde Microsoft Azure rolünde içinde bu dize hizmet yapılandırma dosyasında depolanabilir *ServiceConfiguration.cscfg*ve çağrısıyla erişilebilir **RoleEnvironment.getConfigurationSettings** yöntemi. Bağlantı dizesi alma örneği bir **ayarı** adlı öğe *StorageConnectionString* hizmet yapılandırma dosyasında:

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

Aşağıdaki örnekler, bu iki yöntemden birini depolama bağlantı dizesini almak için kullanılan olduğunu varsayalım.

## <a name="how-to-create-a-queue"></a>Nasıl yapılır: bir sıra oluşturun
A **CloudQueueClient** nesne başvuru nesneleri için kuyrukları almak olanak sağlar. Aşağıdaki kod oluşturur bir **CloudQueueClient** nesnesi. (Not: oluşturmak için ek yol vardır **CloudStorageAccount** nesneleri; daha fazla bilgi için bkz: **CloudStorageAccount** içinde [Azure Storage istemci SDK'sı başvurusu].)

Kullanmak **CloudQueueClient** kullanmak istediğiniz kuyruğuna başvuru nesnesi. Yoksa, kuyruk oluşturabilirsiniz.

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

## <a name="how-to-add-a-message-to-a-queue"></a>Nasıl yapılır: bir sıraya bir ileti ekleyin
Varolan bir sıraya bir ileti yerleştirmek için ilk olarak yeni bir **CloudQueueMessage** oluşturun. Ardından, çağrı **addMessage** yöntemi. A **CloudQueueMessage** bir dizeden (UTF-8 biçiminde) veya bir bayt dizisi oluşturulabilir. Burada kodudur (yoksa), bir kuyruk oluşturur ve "Hello, World" iletisini ekler.

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

## <a name="how-to-peek-at-the-next-message"></a>Nasıl yapılır: sonraki iletiye
Kuyruğun önündeki iletiye sıradan çağırarak kaldırmadan iletiye göz atabilirsiniz **peekMessage**.

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

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Nasıl yapılır: kuyruğa alınan iletinin içeriğini değiştirme
Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. Eğer ileti bir iş görevini temsil ediyorsa, bu özelliği kullanarak iş görevinin durumunu güncelleştirebilirsiniz. Aşağıdaki kod kuyruk iletisini yeni içeriklerle güncelleştirir ve görünürlük zaman aşımını 60 saniye daha uzatır. Bu, ileti ile ilişkili işin durumunu kaydeder ve istemciye ileti üzerinde çalışmaya devam etmesi için bir dakika daha zaman verir. Bir işleme adımı donanım veya yazılım arızasından dolayı başarısız olursa baştan başlamanıza gerek kalmadan kuyruk iletilerindeki çok adımlı iş akışlarını izlemek için bu yöntemi kullanabilirsiniz. Genellikle bir yeniden deneme sayacı tutmanı gerekir ve bir ileti *n* seferden daha fazla yeniden denenirse, silebilirsiniz. Bu, her işlendiğinde bir uygulama hatası tetikleyen bir iletiye karşı koruma sağlar.

Aşağıdaki kod örnek aramalar ileti sırası içeriği için "Hello, World" eşleşen sonra içerik ileti değiştirir ve çıkar ilk iletiyi bulur.

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

Alternatif olarak, aşağıdaki kod örneği yalnızca ilk görünür ileti sırasına güncelleştirir.

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

## <a name="how-to-get-the-queue-length"></a>Nasıl yapılır: kuyruk uzunluğu alma
Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. **DownloadAttributes** kaç iletiler bir kuyrukta olan sayısına dahil yöntemi çeşitli geçerli değerler için sıra hizmeti sorar. İletileri eklenen veya sıra hizmeti isteğinize yanıt sonra kaldırıldığı için yalnızca yaklaşık sayısıdır. **GetApproximateMessageCount** yöntem çağrısı tarafından alınan en son değeri döndürür **downloadAttributes**, kuyruk hizmetini çağırmadan olmadan.

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

## <a name="how-to-dequeue-the-next-message"></a>Nasıl yapılır: sonraki iletiyi sıradan çıkarma
Kodunuzun bir iletiyi bir kuyruktan iki adımda dequeues. Çağırdığınızda **retrieveMessage**, sonraki iletiyi sıraya alın. Döndürülen bir ileti **retrieveMessage** iletileri bu sıradan okuyan herhangi bir kod görünmez olur. Varsayılan olarak bu ileti 30 saniye görünmez kalır. İletiyi kuyruktan kaldırmayı tamamlamak için de çağırmanız gerekir **deleteMessage**. Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. Kod çağrılarınızı **deleteMessage** ileti işlendikten sonra sağ.

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

## <a name="additional-options-for-dequeuing-messages"></a>İletilerin kuyruktan alma için ek seçenekleri
İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz. İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz.

Aşağıdaki kod örneğinde **retrieveMessages** tek çağrıda 20 ileti almak için yöntemi. Her bir iletiyi kullanarak işler sonra bir **için** döngü. Ayrıca beş dakika (300 saniye) her ileti için görünmezlik zaman aşımı ayarlar. Beş dakika başlar çağrısından sonra aynı anda tüm iletileri böylece zaman beş dakika geçtikten için Not **retrieveMessages**, silinmemiş tüm iletiler görünür olacaktır.

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

## <a name="how-to-list-the-queues"></a>Nasıl yapılır: sıraları listesi
Geçerli kuyrukların listesini almak için arama **CloudQueueClient.listQueues()** koleksiyonunu döndürür yöntemi **CloudQueue** nesneleri.

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

## <a name="how-to-delete-a-queue"></a>Nasıl yapılır: bir kuyruk silme
Bir kuyruk ve içerdiği tüm iletileri silmek için arama **deleteIfExists** yöntemi **CloudQueue** nesnesi.

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
Kuyruk depolamanın temellerini öğrendiğinize göre daha karmaşık depolama görevleri hakkında bilgi edinmek için aşağıdaki bağlantıları izleyin.

* [Azure depolama için Java SDK'sı][Azure Storage SDK for Java]
* [Azure Storage istemci SDK'sı başvurusu][Azure Storage istemci SDK'sı başvurusu]
* [Azure Storage Hizmetleri REST API'si][Azure Storage Services REST API]
* [Azure depolama ekibi blogu][Azure Storage Team Blog]

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Storage istemci SDK'sı başvurusu]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage Services REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
