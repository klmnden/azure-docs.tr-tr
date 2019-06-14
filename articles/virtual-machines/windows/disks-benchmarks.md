---
title: Azure Disk Depolama - yönetilen diskleri uygulama değerlendirmesi
description: Uygulamanızı azure'da değerlendirmesi işlemi hakkında bilgi edinin.
services: virtual-machines-windows,storage
author: roygara
ms.author: rogarana
ms.date: 01/11/2019
ms.topic: article
ms.service: virtual-machines-windows
ms.tgt_pltfrm: windows
ms.subservice: disks
ms.openlocfilehash: 8db1fb3c9b3ed551cd668cf14105eb8bfb486251
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60679827"
---
# <a name="benchmarking-a-disk"></a>Bir disk değerlendirmesi

Değerlendirmesi, uygulamanızdaki farklı iş yükleri benzetimi ve her iş yükü için uygulama performansını ölçme işlemidir. İçinde açıklanan adımları kullanarak [için yüksek performanslı makale tasarlama](premium-storage-performance.md), toplanan uygulama performansı gereksinimleri. Uygulama barındırma Vm'lerde Kıyaslama Araçlar çalıştırarak uygulamanızı Premium depolama ile ulaşabilir performans düzeylerini belirleyebilirsiniz. Bu makalede, Azure Premium depolama diskleri ile sağlanan standart DS14 VM değerlendirmesi örnekleri sunuyoruz.

Ortak Kıyaslama araçları Iometer ve FIO, Windows ve Linux için sırasıyla kullandık. Bu araçlar, bir üretim iş yükü gibi benzetimi birden çok iş parçacığı oluşturma ve sistem performansına ölçen. Araçları kullanarak, bir uygulama için normalde değiştiremezsiniz blok boyutu ve sıra derinliğini gibi parametreleri de yapılandırabilirsiniz. Bu, farklı uygulama iş yükleri için premium disklere sahip VM'de yüksek ölçekte en yüksek performansı sürücü daha fazla esneklik sağlar. Ziyaret Kıyaslama her araç hakkında daha fazla bilgi edinmek için [Iometer](http://www.iometer.org/) ve [FIO](http://freecode.com/projects/fio).

Aşağıdaki örneklerde takip etmek için standart DS14 VM oluşturma ve 11 Premium depolama diskleri VM'e ekleyin. 11 disklerin "None" önbelleğe alma konakla 10 diskleri yapılandırın ve bunları NoCacheWrites adlı bir birime stripe. "Salt okunur olarak" kalan disk üzerinde önbelleğe alma konak yapılandırın ve bu diskle CacheReads adlı bir birim oluşturun. Bu ayarı kullanarak, bir standart DS14 VM'den en yüksek okuma ve yazma performansı görebilirsiniz. Premium SSD ile DS14 VM oluşturma ile ilgili ayrıntılı adımlar için Git [yüksek performans için tasarlama](premium-storage-performance.md).

[!INCLUDE [virtual-machines-disks-benchmarking](../../../includes/virtual-machines-managed-disks-benchmarking.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bizim tasarım için yüksek performanslı makale ile devam edin. İçinde bir denetim listesi benzer prototip için mevcut uygulamanızı oluşturun. Benchmarking araçlarını kullanarak iş yüklerini benzetimini yapmak ve prototip uygulama performansına ölçen. Bunu yaptığınızda, hangi disk belirleyebilirsiniz teklifi eşleşen veya uygulama performans gereksinimlerinizi aşan. Ardından, bir üretim uygulamanız için aynı yönergeler uygulayabilirsiniz.

> [!div class="nextstepaction"]
> Makaleye bakın [yüksek performans için tasarlama](premium-storage-performance.md) başlayın.