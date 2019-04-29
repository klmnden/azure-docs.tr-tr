---
title: Python'dan kuyruk depolama kullanma | Microsoft Docs
description: Python'dan Azure kuyruk hizmeti oluşturmak ve Kuyruklar, silmek için kullanmayı öğrenin ve Ekle, Al ve iletilerini silin.
services: storage
author: tamram
ms.service: storage
ms.devlang: python
ms.topic: article
ms.date: 12/14/2018
ms.author: tamram
ms.subservice: queues
ms.openlocfilehash: afa0b6d4f87c4325f116463242c15df9d9b6e7c4
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62108813"
---
# <a name="how-to-use-queue-storage-from-python"></a>Python’dan Kuyruk depolama kullanma
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz Azure kuyruk depolama hizmetini kullanarak, yaygın senaryoları gerçekleştirmek nasıl gösterir. Python ve kullanım örnekleri yazılır [Microsoft Azure depolama için Python SDK'sı]. Senaryoları ele alınmaktadır **ekleme**, **gözatma**, **alma**, ve **silme** kuyruk iletileri yanı  **oluşturma ve kuyrukları silme**. Kuyruklar hakkında daha fazla bilgi için [sonraki adımlar] bölümüne bakın.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="download-and-install-azure-storage-sdk-for-python"></a>İndirin ve Python için Azure depolama SDK'sını yükleyin

[Python için Azure depolama SDK'sı](https://github.com/azure/azure-storage-python) Python 2.7, 3.3, 3.4, 3.5 ve 3.6 gerektirir.
 
### <a name="install-via-pypi"></a>Pypı yüklemek

Python paket dizinini (Pypı) yüklemek için şunu yazın:

```bash
pip install azure-storage-queue
```

> [!NOTE]
> Python 0.36 veya önceki bir sürümü için Azure depolama SDK'sından yükseltme yapıyorsanız, eski SDK'sını kullanarak kaldırma `pip uninstall azure-storage` en yeni paketi yüklemeden önce.

Diğer yükleme yöntemleri için bkz [Python için Azure depolama SDK'sı](https://github.com/Azure/azure-storage-python/).

## <a name="view-the-sample-application"></a>Örnek uygulamayı görüntüleme

Görüntülemek ve Python ile Azure kuyrukları kullanma gösteren bir örnek uygulama çalıştırmak için bkz: [Azure Depolama: Python'da Azure kuyrukları ile çalışmaya başlama](https://github.com/Azure-Samples/storage-queue-python-getting-started). 

Örnek uygulamayı çalıştırmak için her ikisi de yüklediğinizden emin olun `azure-storage-queue` ve `azure-storage-common` paketleri.

## <a name="how-to-create-a-queue"></a>Nasıl Yapılır: Kuyruk oluşturma

**QueueService** nesne kuyrukları ile çalışmanıza olanak tanır. Aşağıdaki kod oluşturur bir **QueueService** nesne. Aşağıdaki program aracılığıyla Azure depolamaya erişmek istediğiniz herhangi bir Python dosyası üstüne yakın bir yere ekleyin:

```python
from azure.storage.queue import QueueService
```

Aşağıdaki kod oluşturur bir **QueueService** depolama hesabı adını ve hesap anahtarını kullanarak nesne. 'Myaccount' ve 'mykey' ile hesap adınız ve anahtarınızla değiştirin.

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Nasıl Yapılır: Kuyruğa bir ileti Ekle
Bir kuyruğa ileti eklemek için kullanın **put\_ileti** yöntemi yeni bir ileti oluşturun ve kuyruğa ekleyin.

```python
queue_service.put_message('taskqueue', u'Hello World')
```

## <a name="how-to-peek-at-the-next-message"></a>Nasıl Yapılır: Sonraki iletiye gözatın
Kuyruğun iletiyi kuyruktan kaldırmadan çağırarak peek **gözlem\_iletileri** yöntemi. Varsayılan olarak, **gözlem\_iletileri** peeks tek bir iletiye.

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="how-to-dequeue-messages"></a>Nasıl Yapılır: İletileri sıradan çıkarma
Kodunuzun bir iletiyi bir kuyruktan iki adımda kaldırır. Çağırdığınızda **alma\_iletileri**, varsayılan olarak sonraki iletiyi kuyruğa alın. Öğesinden döndürülen bir ileti **alma\_iletileri** bu kuyruktan iletileri okuyan herhangi bir kod için görünmez hale gelir. Varsayılan olarak bu ileti 30 saniye görünmez kalır. İletiyi kuyruktan kaldırmayı tamamlamak için de çağırmanız gerekir **Sil\_ileti**. Kodunuzun bir iletiyi donanım veya yazılım hatası nedeniyle başarısız olduğunda, kodunuzun başka bir örneği aynı iletiyi alıp yeniden deneyin, bu iki adımlı işlem, bir iletinin sağlar. Kod çağrılarınızı **Sil\_ileti** ileti işlendikten sonra sağ.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz.
İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz. Aşağıdaki kod örneğinde **alma\_iletileri** tek çağrıda 16 ileti almak için yöntemi. Her bir iletiyi kullanarak işler sonra bir for döngüsü. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır.

```python
messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)        
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Nasıl Yapılır: Kuyruğa Alınan iletinin içeriğini değiştirme
Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. Eğer ileti bir iş görevini temsil ediyorsa, bu özelliği kullanarak iş görevinin durumunu güncelleştirebilirsiniz. Kullanan aşağıdaki kodu **güncelleştirme\_ileti** bir ileti güncelleştirmek için yöntemi. Görünebilirlik zaman aşımı iletisi hemen gösterilir ve içerik güncelleştirildi 0 olarak ayarlanır.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="how-to-get-the-queue-length"></a>Nasıl Yapılır: Kuyruk uzunluğu alma
Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. **Alma\_kuyruk\_meta verileri** yöntemi kuyruk hakkındaki meta verileri döndürmek için kuyruk hizmeti sorar ve **approximate_message_count**. İletileri veya kuyruk hizmeti, isteğine yanıt vermeden sonra kaldırılamaz çünkü yalnızca yaklaşık sonucudur.

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="how-to-delete-a-queue"></a>Nasıl Yapılır: Bir kuyruk silme
Bir kuyruk ve içerdiği tüm iletileri silmek için çağrı **Sil\_kuyruk** yöntemi.

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a>Sonraki Adımlar
Kuyruk depolamanın temellerini öğrendiğinize göre daha fazla bilgi için bu bağlantıları izleyin.

* [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/)
* [Azure Depolama Hizmetleri REST API'si](https://msdn.microsoft.com/library/azure/dd179355)

[Azure Storage Team Blog]: https://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure depolama için Python SDK'sı]: https://github.com/Azure/azure-storage-python
