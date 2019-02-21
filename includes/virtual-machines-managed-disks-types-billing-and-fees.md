---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0d771f03f9f71151ef25140148d4dd4daf3d46ec
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56443327"
---
**Giden veri aktarımları**: [Giden veri aktarımları](https://azure.microsoft.com/pricing/details/bandwidth/) (Azure veri merkezlerinden çıkan veriler) bant genişliği kullanımı için fatura doğurur.

**İşlem**: Standart yönetilen disk üzerinde gerçekleştirdiğiniz işlem sayısı için faturalandırılırsınız. Azure, 100.000 işlem için standart HDD'ler 0.0036 uygular. İşlemlere Depolama'da yapılan hem okuma hem de yazma işlemleri dahildir.

Standart SSD GÇ birim boyutu 256 KB'ı kullanın. Aktarılan veriler 256 KB'den küçük ise, bir g/ç birim olarak kabul edilir. Büyük g/ç boyutları sayılır olarak birden çok g/ç boyutu 256 KB. Örneğin, bir 1.100 KB g/ç beş g/ç birimleri sayılır.

Premium yönetilen disk işlemleri herhangi bir maliyet yoktur.

Yönetilen diskler için fiyatlandırma hakkında ayrıntılı bilgi için bkz: [yönetilen diskler fiyatlandırma](https://azure.microsoft.com/pricing/details/managed-disks).

### <a name="ultra-ssd-vm-reservation-fee"></a>Ultra yüksek SSD VM ayırma ücreti

Azure sanal makineleri ultra SSD sürücüsü ile uyumlu olup olmadığını belirtmek için özelliğine sahip. Uyumlu sanal makine ayıran bir Ultra yüksek disk adanmış işlem VM örneği ve performansı iyileştirmek ve gecikme süresini azaltmak için blok depolama ölçek birimi arasında bant genişliği kapasitesi. Ultra yüksek bir disk eklemeden VM'de Ultra yüksek disk özelliği etkinse, yalnızca uygulanan rezervasyon ücreti VM sonuçları bu özellik ekleniyor. Ultra yüksek bir disk bağlı olduğu için ultra uyumlu VM disk, bu ücret uygulanmamış. Bu ücretlendirme, VM üzerinde sağlanan vCPU başına yapılır.

Başvurmak [Azure fiyatlandırma sayfasını diskler](https://azure.microsoft.com/pricing/details/managed-disks/) diskler fiyatlandırma ayrıntıları için ultra disk.