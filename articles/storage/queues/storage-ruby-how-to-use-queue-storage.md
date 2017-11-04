---
title: Ruby'den kuyruk depolama kullanma | Microsoft Docs
description: "Oluşturmak ve Kuyruklar silmek için Azure Queue hizmetini kullanmayı öğrenin ve Ekle, Al ve iletilerini silin. Ruby içinde yazılmış örneklerini içerir."
services: storage
documentationcenter: ruby
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 59c2d81b-db9c-46ee-ade2-2f0caae6b1e6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 8fe080aabe3079f571f5979245adfc453dfcd459
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-queue-storage-from-ruby"></a>Ruby’den Kuyruk depolama kullanma
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz Microsoft Azure kuyruk depolama hizmeti kullanılarak yaygın senaryolar gerçekleştirme gösterir. Örnekler, Ruby Azure API'sini kullanarak yazılır.
Kapsamdaki senaryolar dahil **ekleme**, **gözatma**, **alma**, ve **silme** kuyruk iletileri yanı **oluşturma ve silme**.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Ruby uygulaması oluşturma
Bir Ruby uygulaması oluşturun. Yönergeler için bkz: [Azure VM'de rayları Web uygulaması üzerinde Söyleniş](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-access-storage"></a>Uygulamanızı erişimli depolama için yapılandırın
Azure depolama kullanmak için indirme ve storage REST Hizmetleri ile iletişim kuran bir dizi kolaylık içerir Söyleniş azure paketini kullanmak gerekir.

### <a name="use-rubygems-to-obtain-the-package"></a>Paket elde etmek için RubyGems kullanın
1. Bir komut satırı arabirimi gibi kullandığınız **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (UNIX).
2. Gem ve bağımlılıklarını yüklemek için komut penceresinde "gem yükleme azure" yazın.

### <a name="import-the-package"></a>Paket alma
Sık kullandığınız metin düzenleyiciyi kullanın, burada depolama kullanmayı düşündüğünüz Söyleniş dosyasının üstüne aşağıdakileri ekleyin:

```ruby
require "azure"
```

## <a name="setup-an-azure-storage-connection"></a>Bir Azure depolama bağlantı Kur
Azure modülü ortam değişkenleri okur **AZURE\_depolama\_hesap** ve **AZURE\_depolama\_ACCESS_KEY** Azure depolama hesabınıza bağlanmak için gerekli bilgileri için. Bu ortam değişkenleri ayarlanmamışsa, hesap bilgilerini kullanmadan önce belirtmelisiniz **Azure::QueueService** aşağıdaki kod ile:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your Azure storage access key>"
```

Klasik veya Resource Manager depolama hesabı Azure portalında bu değerleri almak için:

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Kullanmak istediğiniz depolama hesabınıza gidin.
3. Sağ taraftaki ayarları dikey penceresinde tıklayın **erişim tuşları**.
4. Görüntülenen erişim anahtarları dikey penceresinde, erişim tuşu 1 ve 2 erişim anahtarı görürsünüz. Bunlardan kullanabilirsiniz. 
5. Anahtarı panoya kopyalamak için Kopyala simgesine tıklayın. 

## <a name="how-to-create-a-queue"></a>Nasıl yapılır: bir sıra oluşturun
Aşağıdaki kod oluşturur bir **Azure::QueueService** kuyruklarla çalışmanıza olanak tanır nesnesi.

```ruby
azure_queue_service = Azure::QueueService.new
```

Kullanım **create_queue()** yöntemi belirtilen ada sahip bir kuyruk oluşturun.

```ruby
begin
  azure_queue_service.create_queue("test-queue")
rescue
  puts $!
end
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Nasıl yapılır: bir sıraya bir ileti Ekle
Kuyruğa bir ileti eklemek için kullanın **create_message()** yeni bir ileti oluşturun ve sıraya eklemek için yöntem.

```ruby
azure_queue_service.create_message("test-queue", "test message")
```

## <a name="how-to-peek-at-the-next-message"></a>Nasıl yapılır: sonraki iletiye
Kuyruğun önündeki iletiye sıradan çağırarak kaldırmadan iletiye göz atabilirsiniz **gözlem\_messages()** yöntemi. Varsayılan olarak, **gözlem\_messages()** tek bir iletiye peeks. Gözatmak istediğiniz kaç iletileri de belirtebilirsiniz.

```ruby
result = azure_queue_service.peek_messages("test-queue",
  {:number_of_messages => 10})
```

## <a name="how-to-dequeue-the-next-message"></a>Nasıl yapılır: sonraki iletiyi sıradan çıkarma
Bir iletiyi bir kuyruktan iki adımda kaldırabilirsiniz.

1. Çağırdığınızda **listesi\_messages()**, varsayılan olarak bir sırada sonraki iletiyi alın. Kaç tane iletileri almak istediğiniz de belirtebilirsiniz. Döndürülen iletileri **listesi\_messages()** iletileri bu sıradan okuyan herhangi bir kod görünmez olur. Görünürlük zaman aşımı saniye olarak bir parametre olarak geçirin.
2. İletiyi kuyruktan kaldırmayı tamamlamak için de çağırmanız gerekir **delete_message()**.

Bir ileti kaldırmanın bu iki adımlı işlem donanım veya yazılım hatası nedeniyle bir ileti işlemek kodunuzu başarısız olduğunda, kodunuzu başka bir örneği aynı ileti alma ve yeniden deneyin sağlar. Kod çağrılarınızı **silmek\_message()** ileti işlendikten sonra sağ.

```ruby
messages = azure_queue_service.list_messages("test-queue", 30)
azure_queue_service.delete_message("test-queue", 
  messages[0].id, messages[0].pop_receipt)
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Nasıl yapılır: kuyruğa alınan iletinin içeriğini değiştirme
Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. Aşağıdaki kod kullanır **update_message()** bir ileti güncelleştirmek için yöntem. Yöntem pop alınmasını kuyruk iletisi ve ileti sırası görünür olacak zaman temsil eden bir UTC tarih saat değeri içeren bir tanımlama grubu döndürür.

```ruby
message = azure_queue_service.list_messages("test-queue", 30)
pop_receipt, time_next_visible = azure_queue_service.update_message(
  "test-queue", message.id, message.pop_receipt, "updated test message", 
  30)
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Nasıl yapılır: kuyruktan alma için ek seçenekler iletileri
İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz.

1. İleti toplu elde edebilirsiniz.
2. Uzun veya daha kısa bir görünmezlik zaman aşımı kodunuzun her iletiyi tamamen işlemesi için zaman daha az veya daha fazla izin verebilirsiniz.

Aşağıdaki kod örneğinde **listesi\_messages()** tek çağrıda 15 iletileri almak için yöntemi. Ardından yazdırır ve her iletiyi siler. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır.

```ruby
azure_queue_service.list_messages("test-queue", 300
  {:number_of_messages => 15}).each do |m|
  puts m.message_text
  azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
end
```

## <a name="how-to-get-the-queue-length"></a>Nasıl yapılır: kuyruk uzunluğu alma
Sırasındaki ileti sayısını tahmin alabilirsiniz. **Almak\_sıra\_metadata()** yöntemi yaklaşık ileti sayısı ve kuyruk hakkındaki meta verileri döndürmek için sıra hizmeti sorar.

```ruby
message_count, metadata = azure_queue_service.get_queue_metadata(
  "test-queue")
```

## <a name="how-to-delete-a-queue"></a>Nasıl yapılır: bir kuyruk silme
Bir kuyruk ve içerdiği tüm iletileri silmek için arama **silmek\_queue()** nesnesinde yöntemi.

```ruby
azure_queue_service.delete_queue("test-queue")
```

## <a name="next-steps"></a>Sonraki Adımlar
Kuyruk depolamanın temellerini öğrendiğinize göre daha karmaşık depolama görevleri hakkında bilgi edinmek için aşağıdaki bağlantıları izleyin.

* Ziyaret [Azure depolama ekibi blogu](http://blogs.msdn.com/b/windowsazurestorage/)
* Ziyaret [Ruby için Azure SDK](https://github.com/WindowsAzure/azure-sdk-for-ruby) github'daki

Bu makalede ele alınan Azure sıra hizmeti ve Azure hizmet veri yolu kuyrukları ele arasında bir karşılaştırma için [Service Bus kuyruklarını kullanma](/develop/ruby/how-to-guides/service-bus-queues/) makale için bkz: [Azure kuyruklar ve hizmet veri yolu kuyrukları karşılaştırılan ve Contrasted -](../../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)