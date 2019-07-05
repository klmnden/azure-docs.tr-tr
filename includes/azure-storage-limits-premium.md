---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 07/01/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e878ca23b9187fe3175ad0af1b4f27e59e1deef6
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509931"
---
### <a name="premium-performance-block-blob-storage"></a>Premium performans blok blob depolama

Premium performans blok blob depolama hesabı, daha küçük, kilobayt aralığı, nesneleri kullanan uygulamalar için optimize edilmiştir. Yüksek işlem hızları ya da tutarlı düşük gecikme süreli depolama gerektiren uygulamalar için idealdir. Premium performans blok blobu depolama ile uygulamalarınızı ölçeklendirmek için tasarlanmıştır. Yüz binlerce saniye başına istek sayısı veya depolama kapasitesi petabaytlarca gerektiren uygulamaları dağıtmayı planlıyorsanız, Lütfen bir destek isteği göndererek bizimle [Azure portalında](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="premium-performance-filestorage"></a>Premium performans dosya deposundan

[!INCLUDE [azure-storage-limits-filestorage](azure-storage-limits-filestorage.md)]

 Ölçek hedefleri için Premium dosya paylaşmak için bkz: [Premium dosyaları ölçeklendirme hedeflerini](../articles/storage/common/storage-scalability-targets.md#premium-files-scale-targets) bölümü.

### <a name="premium-performance-page-blob-storage"></a>Premium performans sayfa blob depolama

Premium performans, genel amaçlı v1 veya v2 depolama hesaplarının aşağıdaki ölçeklenebilirlik hedefleri vardır:

| Toplam hesabı kapasitesi                            | Yerel olarak yedekli depolama hesabı için toplam bant genişliği                     |
| ------------------------------------------------- | --------------------------------------------------------------------------- |
| Disk kapasitesi: 35 TB <br>Anlık görüntü kapasitesi: 10 TB | 50 Gigabit / saniye için yukarı gelen<sup>1</sup> + giden<sup>2</sup> |

<sup>1</sup> bir depolama hesabına gönderilen tüm verileri (istekler)

<sup>2</sup> bir depolama hesabından alınan tüm verileri (yanıtlar)

Yönetilmeyen diskler için premium performans depolama hesapları kullandığınız ve uygulamanızı bir tek bir depolama hesabı ölçeklenebilirlik hedefleri aşarsa, yönetilen disklere geçirmek isteyebilirsiniz. Yönetilen disklere geçirmek istemiyorsanız, birden çok depolama hesaplarını kullanmak için uygulamanızı oluşturun. Ardından, bu depolama hesabı arasında veri bölümleme. Örneğin, birden çok VM arasında 51 TB disk eklemek istiyorsanız, bunları iki depolama hesabı arasında yayılabilir. Tek bir premium depolama hesabı için belirlenen sınırı 35 TB'dir. Tek premium performans depolama hesabı hiçbir zaman sağlanan diskleri 35 TB'den fazla olduğundan emin olun.