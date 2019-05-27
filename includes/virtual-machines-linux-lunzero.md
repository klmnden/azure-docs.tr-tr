---
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 87dd3680aae3e87f78ab2dbe70c44b2008706747
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66172002"
---
Bir Linux VM'ye veri diskleri ekleme, bir diskin LUN 0 yoksa hatalarla karşılaşabilirsiniz. El ile kullanarak bir diski ekliyorsanız `azure vm disk attach-new` komutunu bir LUN belirtin (`--lun`) uygun LUN belirlemek üzere Azure platformunun izin vermek yerine, dikkatli bir disk zaten mevcut olduğundan / 0 LUN sunulacaktır. 

Bir kod parçacığı çıktısı gösteren aşağıdaki örneği göz önünde bulundurun `lsscsi`:

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

İki veri diski LUN 0 ve LUN 1 mevcut (ilk sütunda `lsscsi` çıkış ayrıntıları `[host:channel:target:lun]`). Her iki diskin VM içinden erişilebilir olması gerekir. Birinci diskin LUN 1 eklenecek ve ikinci diskin LUN 2 sırasında el ile belirtilmemişti ise VM'NİZİN içinde diskleri doğru göremeyebilirsiniz.

> [!NOTE]
> Azure `host` değer 5'te aşağıdaki örneklerde, ancak bu, seçtiğiniz depolama türüne bağlı olarak değişiklik gösterebilir.
> 
> 

Bu disk davranışı bir Azure sorunundan ancak Linux çekirdeğinin SCSI belirtimleri takip eden yolu değil. Linux çekirdeğinin SCSI veri yolu eklenen cihazları tarar, bir cihaz için ek cihazları taramaya devam etmek için LUN 0 sistem sırada bulunması gerekir. Bu nedenle:

* Çıkışı gözden `lsscsi` LUN 0 bir diski olduğunu doğrulamak için bir veri diski ekledikten sonra.
* Diskinizin doğru VM'NİZİN içinde gösterilmez, LUN 0 bir disk zaten doğrulayın.

