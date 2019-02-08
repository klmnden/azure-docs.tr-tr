---
title: Azure kuyrukları giriş | Microsoft Docs
description: Azure kuyrukları giriş
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/06/2019
ms.author: tamram
ms.subservice: queues
ms.openlocfilehash: 2ae0d3993df54e1c9e5a9bf93619e8f9faa8a917
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873606"
---
# <a name="what-are-azure-queues"></a>Azure kuyrukları nelerdir?

Azure Kuyruk depolama, HTTP veya HTTPS kullanan kimlik doğrulaması yapılmış çağrılar aracılığıyla dünyanın her yerinden erişilebilen çok sayıda iletinin depolanması için bir hizmettir. Tek bir kuyruk iletisinin boyutu 64 KB’ye kadar olabilir ve bir kuyrukta, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti bulunabilir.

## <a name="common-uses"></a>Ortak kullanımlar

Kuyruk depolamanın yaygın kullanımları şunlardır:

* Zaman uyumsuz olarak işlemek için kapsamı oluşturma
* İletileri Azure web rolünden Azure çalışan rolüne geçirme

## <a name="queue-service-concepts"></a>Kuyruk hizmeti kavramları

Kuyruk hizmetinde şu bileşenler bulunur:

![Kuyruk kavramları](./media/storage-queues-introduction/queue1.png)

* **URL biçimi:** Kuyruklar şu URL biçimi kullanılarak adreslenebilir:   
    https://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    Aşağıdaki URL diyagramdaki bir kuyruğun adresini belirtir:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Depolama hesabı:** Tüm Azure depolama erişimi bir depolama hesabı üzerinden yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

* **Kuyruk:** Bir kuyruk, bir dizi ileti içerir. Tüm iletiler bir kuyrukta olmalıdır. Kuyruk adının tamamen küçük harfli olması gerektiğini unutmayın. Kuyrukların adlandırılması hakkında daha fazla bilgi için bkz. [Kuyrukları ve Meta Verileri Adlandırma](https://msdn.microsoft.com/library/azure/dd179349.aspx).

* **İleti:** En fazla 64 KB'ın herhangi bir biçimdeki bir ileti. Bir iletinin kuyrukta kalabileceği en uzun süreyi yedi gündür.

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama hesabı oluşturma](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [.NET ile kuyrukları ile çalışmaya](storage-dotnet-how-to-use-queues.md)
