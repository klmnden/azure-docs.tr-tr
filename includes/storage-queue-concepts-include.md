---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 8a596293a5c1572b30ea6101dad16328c8db2634
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62108800"
---
## <a name="what-is-queue-storage"></a>Kuyruk Depolama nedir?
Azure Kuyruk depolama, HTTP veya HTTPS kullanan kimlik doğrulaması yapılmış çağrılar aracılığıyla dünyanın her yerinden erişilebilen çok sayıda iletinin depolanması için bir hizmettir. Tek bir kuyruk iletisinin boyutu 64 KB’ye kadar olabilir ve bir kuyrukta, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti bulunabilir.

Kuyruk depolamanın yaygın kullanımları şunlardır:

* Zaman uyumsuz olarak işlemek için kapsamı oluşturma
* İletileri Azure web rolünden Azure çalışan rolüne geçirme

## <a name="queue-service-concepts"></a>Kuyruk Hizmeti Kavramları
Kuyruk hizmetinde şu bileşenler bulunur:

![Kuyruk1](./media/storage-queue-concepts-include/queue1.png)

* **URL biçimi:** Kuyruklar şu URL biçimi kullanılarak adreslenebilir:   
    http://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    Aşağıdaki URL diyagramdaki bir kuyruğun adresini belirtir:  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* **Depolama hesabı:** Tüm Azure depolama erişimi bir depolama hesabı üzerinden yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](../articles/storage/common/storage-scalability-targets.md).
* **Kuyruk:** Bir kuyruk, bir dizi ileti içerir. Tüm iletiler bir kuyrukta olmalıdır. Kuyruk adının tamamen küçük harfli olması gerektiğini unutmayın. Kuyrukların adlandırılması hakkında daha fazla bilgi için bkz. [Kuyrukları ve Meta Verileri Adlandırma](https://msdn.microsoft.com/library/azure/dd179349.aspx).
* **İleti:** En fazla 64 KB'ın herhangi bir biçimdeki bir ileti. Bir iletinin kuyrukta kalabileceği en uzun süre 7 gündür.

