---
title: Python'dan kuyruk depolama kullanma | Microsoft Docs
description: "Oluşturma ve silme sıraları, Python Azure kuyruk hizmetinden kullanmayı öğrenin ve Ekle, Al ve iletilerini silin."
services: storage
documentationcenter: python
author: tamram
manager: timlt
editor: tysonn
ms.assetid: cc0d2da2-379a-4b58-a234-8852b4e3d99d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: c7976c01436b1c30880bfd4c57cb97f72a4f48b0
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-use-queue-storage-from-python"></a>Python’dan Kuyruk depolama kullanma
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz Azure kuyruk depolama hizmeti kullanılarak yaygın senaryolar gerçekleştirme gösterir. Python ve kullanım örnekleri yazılır [Python için Microsoft Azure depolama SDK]. Kapsamdaki senaryolar dahil **ekleme**, **gözatma**, **alma**, ve **silme** kuyruk iletileri yanı **oluşturma ve silme**. Kuyruklar hakkında daha fazla bilgi için [sonraki adımlar] bölümüne bakın.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="download-and-install-azure-storage-sdk-for-python"></a>İndirme ve Python için Azure depolama SDK yükleme

Python için Azure depolama SDK Python 2.7, 3.3, 3.4, 3.5 veya 3.6 gerektirir ve 4 farklı paketlerde gelir: `azure-storage-blob`, `azure-storage-file`, `azure-storage-table` ve `azure-storage-queue`. Bu öğreticide biz kullanacaksanız `azure-storage-queue` paket.
 
### <a name="install-via-pypi"></a>Pypı yüklemek

Python paket dizinini (Pypı) yüklemek için şunu yazın:

```bash
pip install azure-storage-queue
```


> [!NOTE]
> Azure depolama SDK Python 0.36 veya önceki bir sürümü için yükseltme yapıyorsanız, önce kullanarak kaldırmak gerekir `pip uninstall azure-storage` artık depolama SDK'sı Python için tek bir paket sunacağımız gibi.
> 
> 

Alternatif yükleme yöntemleri için ziyaret [github'da Python için Azure depolama SDK'sı](https://github.com/Azure/azure-storage-python/).

## <a name="how-to-create-a-queue"></a>Nasıl yapılır: bir sıra oluşturun
**QueueService** nesne kuyruklarla çalışmanıza olanak sağlar. Aşağıdaki kod oluşturur bir **QueueService** nesnesi. İlk Azure Storage programlı olarak erişmek istediğiniz herhangi bir Python dosyasının aşağıdakileri ekleyin:

```python
from azure.storage.queue import QueueService
```

Aşağıdaki kod oluşturur bir **QueueService** depolama hesabı adı ve hesap anahtarını kullanarak nesne. 'Myaccount' ve 'mykey' hesap adı ve anahtarı ile değiştirin.

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Nasıl yapılır: bir sıraya bir ileti Ekle
Kuyruğa bir ileti eklemek için kullanın **put\_ileti** yeni bir ileti oluşturun ve sıraya eklemek için yöntem.

```python
queue_service.put_message('taskqueue', u'Hello World')
```

## <a name="how-to-peek-at-the-next-message"></a>Nasıl yapılır: sonraki iletiye
Kuyruğun önündeki iletiye sıradan çağırarak kaldırmadan iletiye göz atabilirsiniz **gözlem\_iletileri** yöntemi. Varsayılan olarak, **gözlem\_iletileri** tek bir iletiye peeks.

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="how-to-dequeue-messages"></a>Nasıl yapılır: İletiler Dequeue
Kodunuzun bir iletiyi bir kuyruktan iki adımda kaldırır. Çağırdığınızda **almak\_iletileri**, varsayılan olarak bir sırada sonraki iletiyi alın. Döndürülen bir ileti **almak\_iletileri** iletileri bu sıradan okuyan herhangi bir kod görünmez olur. Varsayılan olarak bu ileti 30 saniye görünmez kalır. İletiyi kuyruktan kaldırmayı tamamlamak için de çağırmanız gerekir **silmek\_ileti**. Bir ileti kaldırmanın bu iki adımlı işlem donanım veya yazılım hatası nedeniyle bir ileti işlemek kodunuzu başarısız olduğunda, kodunuzu başka bir örneği aynı ileti alma ve yeniden deneyin sağlar. Kod çağrılarınızı **silmek\_ileti** ileti işlendikten sonra sağ.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz.
İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz. Aşağıdaki kod örneğinde **almak\_iletileri** tek çağrıda 16 iletileri almak için yöntemi. Her bir iletiyi kullanarak işler sonra bir döngü için. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır.

```python
messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)        
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Nasıl yapılır: kuyruğa alınan iletinin içeriğini değiştirme
Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. Eğer ileti bir iş görevini temsil ediyorsa, bu özelliği kullanarak iş görevinin durumunu güncelleştirebilirsiniz. Aşağıdaki kod kullanır **güncelleştirme\_ileti** bir ileti güncelleştirmek için yöntemi. Görünürlük zaman aşımını ileti hemen görüntülenir ve içeriği güncellenir yani 0 olarak ayarlanır.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="how-to-get-the-queue-length"></a>Nasıl yapılır: kuyruk uzunluğu alma
Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. **Almak\_sıra\_meta veri** yöntemi kuyruk hakkındaki meta verileri döndürmek üzere kuyruk hizmetinden ister ve **approximate_message_count**. İletileri eklenen veya sıra hizmeti isteğinize yanıt sonra kaldırıldığı için yalnızca yaklaşık sonucudur.

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="how-to-delete-a-queue"></a>Nasıl yapılır: bir kuyruk silme
Bir kuyruk ve içerdiği tüm iletileri silmek için arama **silmek\_sıra** yöntemi.

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a>Sonraki Adımlar
Kuyruk depolamanın temellerini öğrendiğinize göre daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* [Python Geliştirici Merkezi](/develop/python/)
* [Azure Depolama Hizmetleri REST API'si](http://msdn.microsoft.com/library/azure/dd179355)
* [Azure Depolama Ekibi Blog’u]
* [Python için Microsoft Azure depolama SDK]

[Azure Depolama Ekibi Blog’u]: http://blogs.msdn.com/b/windowsazurestorage/
[Python için Microsoft Azure depolama SDK]: https://github.com/Azure/azure-storage-python