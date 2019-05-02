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
ms.openlocfilehash: 42ab8be45d4086589f0793531003700e7552a440
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64744656"
---
**Giden veri aktarımları**: [Giden veri aktarımları](https://azure.microsoft.com/pricing/details/bandwidth/) (Azure veri merkezlerinden çıkan veriler) bant genişliği kullanımı için fatura doğurur.

**İşlem**: Standart yönetilen disk üzerinde gerçekleştirdiğiniz işlem sayısı için faturalandırılırsınız. Standart SSD için g/ç birim boyutu 256 KiB işlem sayısı hesap için kullanılır. Daha büyük g/ç boyutları olarak birden çok g/ç boyutu 256 sayılır KiB. Standart HDD için her bir GÇ işlemi g/ç boyutundan bağımsız olarak tek bir işlem olarak kabul edilir.

Yönetilen işlem maliyetleri dahil diskler için fiyatlandırma hakkında ayrıntılı bilgi için bkz. [yönetilen diskler fiyatlandırma](https://azure.microsoft.com/pricing/details/managed-disks).

### <a name="ultra-ssd-vm-reservation-fee"></a>Ultra yüksek SSD VM ayırma ücreti

Azure sanal makineleri ultra SSD sürücüsü ile uyumlu olup olmadığını belirtmek için özelliğine sahip. Uyumlu sanal makine ayıran bir Ultra yüksek disk adanmış işlem VM örneği ve performansı iyileştirmek ve gecikme süresini azaltmak için blok depolama ölçek birimi arasında bant genişliği kapasitesi. Ultra yüksek bir disk eklemeden VM'de Ultra yüksek disk özelliği etkinse, yalnızca uygulanan rezervasyon ücreti VM sonuçları bu özellik ekleniyor. Ultra yüksek bir disk bağlı olduğu için ultra uyumlu VM disk, bu ücret uygulanmamış. Bu ücretlendirme, VM üzerinde sağlanan vCPU başına yapılır.

Başvurmak [Azure fiyatlandırma sayfasını diskler](https://azure.microsoft.com/pricing/details/managed-disks/) diskler fiyatlandırma ayrıntıları için ultra disk.