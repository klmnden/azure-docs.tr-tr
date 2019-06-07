---
title: Azure kuyrukları - giriş Azure depolama
description: Azure kuyrukları giriş
services: storage
author: mhopkins-msft
ms.service: storage
ms.topic: overview
ms.date: 06/07/2019
ms.author: mhopkins
ms.reviewer: cbrooks
ms.subservice: queues
ms.openlocfilehash: bc3045e3d3b6977555818fcdb3dcaf3246ebd200
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754814"
---
# <a name="what-are-azure-queues"></a>Azure Kuyrukları nedir?

Azure kuyruk depolama, büyük sayıda iletiyi depolamak için kullanılan bir hizmettir. İletileri her yerden erişim HTTP veya HTTPS kullanılarak kimliği doğrulanmış aramalar yoluyla dünyanın. Bir kuyruk iletisi boyutu 64 KB'ye kadar olabilir. Bir kuyruk iletileri, depolama hesabının toplam kapasite sınırına kadar milyonlarca içerebilir.

## <a name="common-uses"></a>Ortak kullanımlar

Kuyruk depolamanın yaygın kullanımları şunlardır:

* Zaman uyumsuz olarak işlemek için kapsamı oluşturma
* İletileri Azure web rolünden Azure çalışan rolüne geçirme

## <a name="queue-service-concepts"></a>Kuyruk hizmeti kavramları

Kuyruk hizmetinde şu bileşenler bulunur:

![Kuyruk kavramları](./media/storage-queues-introduction/queue1.png)

* **URL biçimi:** Kuyruklar şu URL biçimi kullanılarak adreslenebilir:

    `https://<storage account>.queue.core.windows.net/<queue>`
  
    Aşağıdaki URL diyagramdaki bir kuyruğun adresini belirtir:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Depolama hesabı:** Tüm Azure depolama erişimi bir depolama hesabı üzerinden yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

* **Kuyruk:** Bir kuyruk, bir dizi ileti içerir. Kuyruk adı **gerekir** tamamı küçük harfli olması. Kuyrukların adlandırılması hakkında daha fazla bilgi için bkz. [Kuyrukları ve Meta Verileri Adlandırma](https://msdn.microsoft.com/library/azure/dd179349.aspx).

* **İleti:** En fazla 64 KB'ın herhangi bir biçimdeki bir ileti. Sürüm 2017-07-29 önce izin verilen maksimum yaşam süresi yedi gündür. İçin sürüm 2017-07-29 veya daha sonra yaşam süresi pozitif bir sayı veya -1 ileti sona ermez belirten en yüksek olabilir. Bu parametre atlanırsa, varsayılan yaşam süresi yedi gündür.

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama hesabı oluşturma](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [.NET ile kuyrukları ile çalışmaya](storage-dotnet-how-to-use-queues.md)
