---
title: Azure kuyruk depolamaya giriş | Microsoft Docs
description: Azure kuyruk Depolama'ya giriş
services: storage
author: tamram
ms.service: storage
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/07/2017
ms.author: tamram
ms.component: queues
ms.openlocfilehash: d2d4a31097c4050ba9193fc9d6fa076fe9c6e27f
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39524840"
---
# <a name="introduction-to-queues"></a>Kuyruklara Giriş

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

* **Depolama hesabı:** tüm Azure depolama erişimi bir depolama hesabı üzerinden yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

* **Kuyruk:** Kuyrukta bir dizi ileti vardır. Tüm iletiler bir kuyrukta olmalıdır. Kuyruk adının tamamen küçük harfli olması gerektiğini unutmayın. Kuyrukların adlandırılması hakkında daha fazla bilgi için bkz. [Kuyrukları ve Meta Verileri Adlandırma](https://msdn.microsoft.com/library/azure/dd179349.aspx).

* **İleti:** İleti, biçimi ne olursa olsun en çok 64 KB büyüklüktedir. Bir iletinin kuyrukta kalabileceği en uzun süreyi yedi gündür.

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama hesabı oluşturma](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [.NET ile kuyrukları ile çalışmaya](storage-dotnet-how-to-use-queues.md)
