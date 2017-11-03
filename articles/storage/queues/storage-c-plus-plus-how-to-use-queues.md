---
title: Kuyruk depolama (C++) kullanma | Microsoft Docs
description: "Azure kuyruk depolama hizmetini kullanmayı öğrenin. C++'ta örnekleri yazılır."
services: storage
documentationcenter: .net
author: cbrooksmsft
manager: jahogg
editor: tysonn
ms.assetid: c8a36365-29f6-404d-8fd1-858a7f33b50a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 05/11/2017
ms.author: cbrooksmsft
ms.openlocfilehash: 5e81d5e0af9871099b7f921f355cf94249e4d30c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-queue-storage-from-c"></a>C++ içinden kuyruk depolama kullanma
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz Azure kuyruk depolama hizmeti kullanılarak yaygın senaryolar gerçekleştirmek nasıl yapacağınızı gösterir. C++ ve kullanım örnekleri yazılır [C++ için Azure Storage istemci Kitaplığı](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Kapsamdaki senaryolar dahil **ekleme**, **gözatma**, **alma**, ve **silme** kuyruk iletileri yanı **oluşturma ve silme**.

> [!NOTE]
> Bu kılavuz, c++ sürümü 1.0.0 ve yukarıda Azure Storage istemci kitaplığı hedefler. Aracılığıyla kullanılabilir olan depolama istemci kitaplığı 2.2.0, önerilen sürümüdür [NuGet](http://www.nuget.org/packages/wastorage) veya [GitHub](http://github.com/Azure/azure-storage-cpp/).
> 
> 

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>C++ uygulaması oluşturma
Bu kılavuzda, C++ uygulamasında çalıştırabileceğiniz depolama özelliklerini kullanır.

Bunu yapmak için Azure Storage istemci kitaplığı C++ için yükleme ve Azure aboneliğinizde bir Azure depolama hesabı oluşturmanız gerekir.

C++ için Azure Storage istemci kitaplığı yüklemek için aşağıdaki yöntemleri kullanabilirsiniz:

* **Linux:** verilen yönergeleri izleyerek [C++ Benioku için Azure Storage istemci Kitaplığı](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sayfası.
* **Windows:** Visual Studio'da sırasıyla **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**. Aşağıdaki komutu yazın [NuGet Paket Yöneticisi Konsolu](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ve basın **ENTER**.

```  
Install-Package wastorage
```

## <a name="configure-your-application-to-access-queue-storage"></a>Kuyruk depolama erişmek için uygulamanızı yapılandırın
Azure depolama API'leri sıraları erişmek için kullanmasını istediğiniz C++ dosyanın en üstüne deyimlerini şunlar ekleyin:  

```cpp
#include <was/storage_account.h>
#include <was/queue.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Bir Azure depolama bağlantı dizesi ayarlama
Bir Azure storage istemci uç noktaları ve Veri Yönetimi Hizmetleri erişmek için kimlik bilgilerini depolamak için bir depolama bağlantı dizesi kullanır. Bir istemci uygulamasında çalıştırırken, depolama hesabı altında listelenen için adını depolama hesabınız ve depolama erişim tuşunu kullanarak depolama bağlantı dizesi şu biçimde sağlamalısınız [Azure Portal](https://portal.azure.com) için *AccountName* ve *AccountKey* değerleri. Depolama hesapları ve erişim anahtarları hakkında daha fazla bilgi için bkz: [Azure Storage hesapları hakkında](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json). Bu örnek, bağlantı dizesi tutmak için statik bir alana nasıl bildirebilir gösterir:  

```cpp
// Define the connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

Yerel Windows bilgisayarınızda uygulamanızı test etmek için Microsoft Azure kullanabilirsiniz [depolama öykünücüsü](../common/storage-use-emulator.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) ile yüklü [Azure SDK'sı](https://azure.microsoft.com/downloads/). Depolama öykünücüsü Blob, kuyruk ve Tablo Hizmetleri, yerel geliştirme makinenizde Azure'da kullanılabilir benzetim yapan bir yardımcı programdır. Aşağıdaki örnek, yerel depolama öykünücüsü için bağlantı dizesi tutmak için statik bir alana nasıl bildirebilir gösterir:  

```cpp
// Define the connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

Azure storage öykünücüsü başlatmak için **Başlat** düğmesini veya tuşuna **Windows** anahtarı. Yazmaya başlayın **Azure Storage öykünücüsü**seçip **Microsoft Azure Storage öykünücüsü** uygulamalar listesinden.

Aşağıdaki örnekler, bu iki yöntemden birini depolama bağlantı dizesini almak için kullanılan olduğunu varsayalım.

## <a name="retrieve-your-connection-string"></a>Bağlantı dizesi alma
Kullanabileceğiniz **cloud_storage_account** depolama hesabı bilgileri temsil eden sınıf. Depolama bağlantı dizesi, depolama hesabı bilgilerini almak için kullanabileceğiniz **ayrıştırma** yöntemi.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="how-to-create-a-queue"></a>Nasıl yapılır: bir sıra oluşturun
A **cloud_queue_client** nesne başvuru nesneleri için kuyrukları almak olanak sağlar. Aşağıdaki kod oluşturur bir **cloud_queue_client** nesnesi.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create a queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();
```

Kullanmak **cloud_queue_client** kullanmak istediğiniz kuyruğuna başvuru nesnesi. Yoksa, kuyruk oluşturabilirsiniz.

```cpp
// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create the queue if it doesn't already exist.
 queue.create_if_not_exists();  
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Nasıl yapılır: bir sıraya bir ileti Ekle
Varolan bir sıraya bir ileti eklemek için ilk olarak yeni bir oluşturma **cloud_queue_message**. Ardından, çağrı **add_message** yöntemi. A **cloud_queue_message** herhangi birinden bir dize oluşturulabilir veya **bayt** dizi. Burada, bir kuyruk oluşturan (eğer yoksa) ve 'Hello, World' iletisini yerleştiren bir kod yer almaktadır:

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create the queue if it doesn't already exist.
queue.create_if_not_exists();

// Create a message and add it to the queue.
azure::storage::cloud_queue_message message1(U("Hello, World"));
queue.add_message(message1);  
```

## <a name="how-to-peek-at-the-next-message"></a>Nasıl yapılır: sonraki iletiye
Kuyruğun önündeki iletiye sıradan çağırarak kaldırmadan iletiye göz atabilirsiniz **peek_message** yöntemi.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Peek at the next message.
azure::storage::cloud_queue_message peeked_message = queue.peek_message();

// Output the message content.
std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Nasıl yapılır: kuyruğa alınan iletinin içeriğini değiştirme
Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. Eğer ileti bir iş görevini temsil ediyorsa, bu özelliği kullanarak iş görevinin durumunu güncelleştirebilirsiniz. Aşağıdaki kod kuyruk iletisini yeni içeriklerle güncelleştirir ve görünürlük zaman aşımını 60 saniye daha uzatır. Bu, ileti ile ilişkili işin durumunu kaydeder ve istemciye ileti üzerinde çalışmaya devam etmesi için bir dakika daha zaman verir. Bir işleme adımı donanım veya yazılım arızasından dolayı başarısız olursa baştan başlamanıza gerek kalmadan kuyruk iletilerindeki çok adımlı iş akışlarını izlemek için bu yöntemi kullanabilirsiniz. Genellikle, bir yeniden deneme sayısı da tutacak ve ileti birden fazla n kez denenirse, silebilirsiniz. Bu, her işlendiğinde bir uygulama hatası tetikleyen bir iletiye karşı koruma sağlar.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get the message from the queue and update the message contents.
// The visibility timeout "0" means make it visible immediately.
// The visibility timeout "60" means the client can get another minute to continue
// working on the message.
azure::storage::cloud_queue_message changed_message = queue.get_message();

changed_message.set_content(U("Changed message"));
queue.update_message(changed_message, std::chrono::seconds(60), true);

// Output the message content.
std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  
```

## <a name="how-to-de-queue-the-next-message"></a>Nasıl yapılır: sonraki iletiyi sıradan çıkarmak
Kodunuz, bir iletiyi bir kuyruktan iki adımda çıkarır. Çağırdığınızda **get_message**, sonraki iletiyi sıraya alın. Döndürülen bir ileti **get_message** iletileri bu sıradan okuyan herhangi bir kod görünmez olur. İletiyi kuyruktan kaldırmayı tamamlamak için de çağırmanız gerekir **delete_message**. Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. Kod çağrılarınızı **delete_message** ileti işlendikten sonra sağ.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get the next message.
azure::storage::cloud_queue_message dequeued_message = queue.get_message();
std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

// Delete the message.
queue.delete_message(dequeued_message);
```

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Nasıl yapılır: yararlanan çıkarılması iletileri için ek seçenekleri
İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz. İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz. Aşağıdaki kod örneğinde **get_messages** tek çağrıda 20 ileti almak için yöntemi. Her bir iletiyi kullanarak işler sonra bir **için** döngü. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır. 5 dakikalık sürenin tüm iletiler için aynı anda Not böylece sonra 5 dakika geçirilen çağrısından sonra **get_messages**, silinmemiş tüm iletiler görünür olacaktır.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
// 5 minutes (300 seconds).
azure::storage::queue_request_options options;
azure::storage::operation_context context;

// Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

for (auto it = messages.cbegin(); it != messages.cend(); ++it)
{
    // Display the contents of the message.
    std::wcout << U("Get: ") << it->content_as_string() << std::endl;
}
```

## <a name="how-to-get-the-queue-length"></a>Nasıl yapılır: kuyruk uzunluğu alma
Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. **Download_attributes** yöntemi kuyruk hizmeti ileti sayısı dahil olmak üzere kuyruk özniteliklerini almasını ister. **Approximate_message_count** yöntemi kuyrukta yaklaşık iletilerin sayısını alır.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Fetch the queue attributes.
queue.download_attributes();

// Retrieve the cached approximate message count.
int cachedMessageCount = queue.approximate_message_count();

// Display number of messages.
std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  
```

## <a name="how-to-delete-a-queue"></a>Nasıl yapılır: bir kuyruk silme
Bir kuyruk ve içerdiği tüm iletileri silmek için arama **delete_queue_if_exists** nesnesinde yöntemi.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// If the queue exists and delete it.
queue.delete_queue_if_exists();  
```

## <a name="next-steps"></a>Sonraki adımlar
Kuyruk depolamanın temellerini öğrendiğinize göre Azure Storage hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* [C++ içinden BLOB Storage kullanma](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Tablo depolama C++ içinden kullanma](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [C++'ta listesi Azure Storage kaynakları](../common/storage-c-plus-plus-enumeration.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [C++ başvurusu için depolama istemci kitaplığı](http://azure.github.io/azure-storage-cpp)
* [Azure Depolama Belgeleri](https://azure.microsoft.com/documentation/services/storage/)