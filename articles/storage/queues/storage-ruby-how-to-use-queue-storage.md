---
title: Ruby'den kuyruk depolama kullanma | Microsoft Docs
description: Oluşturmak ve Kuyruklar, silmek için Azure Queue hizmetini kullanmayı öğrenin ve Ekle, Al ve iletilerini silin. Ruby'de yazılan örnekleri.
services: storage
author: tamram
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.subservice: queues
ms.openlocfilehash: 7ebb4326a8ec8a3382a5488ce3b966526bef446a
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62112013"
---
# <a name="how-to-use-queue-storage-from-ruby"></a>Ruby’den Kuyruk depolama kullanma
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz Microsoft Azure kuyruk depolama hizmetini kullanarak, yaygın senaryoları gerçekleştirmek nasıl gösterir. Örnek Ruby Azure API'sini kullanarak yazılır.
Senaryoları ele alınmaktadır **ekleme**, **gözatma**, **alma**, ve **silme** kuyruk iletileri yanı  **oluşturma ve kuyrukları silme**.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Ruby uygulaması oluşturma
Ruby uygulaması oluşturun. Yönergeler için [Linux üzerinde App Service'te Ruby uygulaması oluşturma](https://docs.microsoft.com/azure/app-service/containers/quickstart-ruby).

## <a name="configure-your-application-to-access-storage"></a>Depolama erişim için uygulamanızı yapılandırma
Azure depolama kullanmak için depolama REST Hizmetleri ile iletişim kuran bir dizi kolaylık içeren Ruby azure paketi indirip gerekir.

### <a name="use-rubygems-to-obtain-the-package"></a>Paketi edinmek için RubyGems kullanma
1. **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (Unix) gibi bir komut satırı arabirimi kullanın.
2. Gem ve bağımlılıklarını yüklemek için komut penceresinde "gem yükleme azure" yazın.

### <a name="import-the-package"></a>Paketi içeri aktarma
Metin düzenleyiciyi kullanın, burada depolama kullanmak için istediğinize Ruby dosyasının en üstüne aşağıdakileri ekleyin:

```ruby
require "azure"
```

## <a name="setup-an-azure-storage-connection"></a>Bir Azure depolama bağlantısı kurma
Azure modülünü ortam değişkenlerini okur **AZURE\_depolama\_hesabı** ve **AZURE\_depolama\_ACCESS_KEY** bilgi Azure depolama hesabınıza bağlanmak için gereklidir. Bu ortam değişkenleri ayarlanmamışsa, hesap bilgilerini kullanmadan önce belirtmelisiniz **Azure::QueueService** aşağıdaki kod ile:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your Azure storage access key>"
```

Bu değerleri Azure portalında bir klasik veya Kaynak Yöneticisi depolama hesabından edinmek için:

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Kullanmak istediğiniz depolama hesabına gidin.
3. Sağdaki Ayarlar dikey penceresinde **Erişim Anahtarları**'na tıklayın.
4. Açılan Erişim anahtarları dikey penceresinde, 1. ve 2. erişim anahtarını göreceksiniz. Bunlardan birini kullanabilirsiniz. 
5. Anahtarı panoya kopyalamak için Kopyala simgesine tıklayın. 

## <a name="how-to-create-a-queue"></a>Nasıl Yapılır: Kuyruk oluşturma
Aşağıdaki kod oluşturur bir **Azure::QueueService** kuyrukları ile çalışmanıza olanak sağlayan nesne.

```ruby
azure_queue_service = Azure::QueueService.new
```

Kullanım **create_queue()** belirtilen ada sahip bir kuyruk oluşturmak için yöntemi.

```ruby
begin
  azure_queue_service.create_queue("test-queue")
rescue
  puts $!
end
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Nasıl Yapılır: Kuyruğa bir ileti Ekle
Bir kuyruğa ileti eklemek için kullanın **create_message()** yöntemi yeni bir ileti oluşturun ve kuyruğa ekleyin.

```ruby
azure_queue_service.create_message("test-queue", "test message")
```

## <a name="how-to-peek-at-the-next-message"></a>Nasıl Yapılır: Sonraki iletiye gözatın
Kuyruğun iletiyi kuyruktan kaldırmadan çağırarak peek **gözlem\_messages()** yöntemi. Varsayılan olarak, **gözlem\_messages()** peeks tek bir iletiye. Özet istediğiniz ileti sayısını da belirtebilirsiniz.

```ruby
result = azure_queue_service.peek_messages("test-queue",
  {:number_of_messages => 10})
```

## <a name="how-to-dequeue-the-next-message"></a>Nasıl Yapılır: Sonraki iletiyi sıradan çıkar
Bir iletiyi bir kuyruktan iki adımda kaldırabilirsiniz.

1. Çağırdığınızda **listesi\_messages()**, varsayılan olarak sonraki iletiyi kuyruğa alın. Almak istediğiniz ileti sayısını da belirtebilirsiniz. Döndürülen iletilerin **listesi\_messages()** bu kuyruktan iletileri okuyan herhangi bir kod için görünmez hale gelir. Görünebilirlik zaman aşımı saniye parametresi olarak geçirin.
2. İletiyi kuyruktan kaldırmayı tamamlamak için de çağırmanız gerekir **delete_message()**.

Kodunuzun bir iletiyi donanım veya yazılım hatası nedeniyle başarısız olduğunda, kodunuzun başka bir örneği aynı iletiyi alıp yeniden deneyin, bu iki adımlı işlem, bir iletinin sağlar. Kod çağrılarınızı **Sil\_message()** ileti işlendikten sonra sağ.

```ruby
messages = azure_queue_service.list_messages("test-queue", 30)
azure_queue_service.delete_message("test-queue", 
  messages[0].id, messages[0].pop_receipt)
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Nasıl Yapılır: Kuyruğa Alınan iletinin içeriğini değiştirme
Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. Aşağıdaki kod kullanır **update_message()** bir ileti güncelleştirmek için yöntemi. Yöntem üstten alma girişi kuyruk iletisi ve ileti sırasına görünür olduğunda temsil eden bir UTC tarih saat değeri içeren bir tanımlama grubu döndürür.

```ruby
message = azure_queue_service.list_messages("test-queue", 30)
pop_receipt, time_next_visible = azure_queue_service.update_message(
  "test-queue", message.id, message.pop_receipt, "updated test message", 
  30)
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Nasıl Yapılır: İletileri sıradan çıkarmak için ek seçenekler
İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz.

1. Toplu ileti alabilirsiniz.
2. Uzun veya daha kısa bir görünmezlik zaman aşımı, kodunuzu daha fazla veya daha az zaman her iletiyi tamamen işlemesi için izin verebilirsiniz.

Aşağıdaki kod örneğinde **listesi\_messages()** tek çağrıda 15 iletileri almak için yöntemi. Ardından yazdırır ve her iletiyi siler. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır.

```ruby
azure_queue_service.list_messages("test-queue", 300
  {:number_of_messages => 15}).each do |m|
  puts m.message_text
  azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
end
```

## <a name="how-to-get-the-queue-length"></a>Nasıl Yapılır: Kuyruk uzunluğu alma
Kuyrukta ileti sayısını tahmini elde edebilirsiniz. **Alma\_kuyruk\_metadata()** yöntemi yaklaşık ileti sayısı ve kuyruk hakkındaki meta verileri döndürmek için kuyruk hizmeti sorar.

```ruby
message_count, metadata = azure_queue_service.get_queue_metadata(
  "test-queue")
```

## <a name="how-to-delete-a-queue"></a>Nasıl Yapılır: Bir kuyruk silme
Bir kuyruk ve içerdiği tüm iletileri silmek için çağrı **Sil\_queue()** kuyruk nesnesi üzerinde yöntemi.

```ruby
azure_queue_service.delete_queue("test-queue")
```

## <a name="next-steps"></a>Sonraki Adımlar
Kuyruk depolamanın temellerini öğrendiğinize göre daha karmaşık depolama görevleri hakkında bilgi edinmek için bu bağlantıları izleyin.

* Ziyaret [Azure depolama ekibi blogu](https://blogs.msdn.com/b/windowsazurestorage/)
* Ziyaret [Ruby için Azure SDK'sı](https://github.com/WindowsAzure/azure-sdk-for-ruby) GitHub deposunu

Azure Service Bus kuyrukları açıklandığı ve bu makalede ele alınan Azure kuyruk hizmeti arasında bir karşılaştırma için [nasıl Service Bus kuyruklarını kullanma](https://azure.microsoft.com/develop/ruby/how-to-guides/service-bus-queues/) makale için bkz: [Azure kuyrukları ve Service Bus kuyrukları - karşılaştırıldığında ve Karşıtlıklar](../../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
