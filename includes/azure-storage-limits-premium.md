---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3d100ec08b7b6d70366e605daf9c67edc6ab3bf3
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56331077"
---
Premium depolama hesapları aşağıdaki ölçeklenebilirlik hedefleri vardır:

| Toplam hesabı kapasitesi | Yerel olarak yedekli depolama hesabı için toplam bant genişliği |
| --- | --- | 
| Disk kapasitesi: 35 TB <br>Anlık görüntü kapasitesi: 10 TB | 50 Gigabit / saniye için yukarı gelen<sup>1</sup> + giden<sup>2</sup> |

<sup>1</sup> bir depolama hesabına gönderilen tüm verileri (istekler)

<sup>2</sup> bir depolama hesabından alınan tüm verileri (yanıtlar)

Yönetilmeyen diskler için premium depolama hesapları kullandığınız ve uygulamanızı bir tek bir depolama hesabı ölçeklenebilirlik hedefleri aşarsa, yönetilen disklere geçirmek isteyebilirsiniz. Yönetilen disklere geçirmek istemiyorsanız, birden çok depolama hesaplarını kullanmak için uygulamanızı oluşturun. Ardından, bu depolama hesabı arasında veri bölümleme. Örneğin, birden çok VM arasında 51 TB disk eklemek istiyorsanız, bunları iki depolama hesabı arasında yayılabilir. Tek bir premium depolama hesabı için belirlenen sınırı 35 TB'dir. Tek bir premium depolama hesabı hiçbir zaman sağlanan diskleri 35 TB'den fazla olduğundan emin olun.