---
title: StorSimple 8000 serisi cihaz üzerinde kasayı değiştirme | Microsoft Docs
description: StorSimple, birincil kasası veya EBOD muhafazası kasayı değiştirme ve kaldırma işlemini açıklamaktadır.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 7dfc39f4d08c8a49d1564a0a5bd7e3ef4156e3fe
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61312454"
---
# <a name="replace-the-chassis-on-your-storsimple-device"></a>StorSimple Cihazınızda kasayı değiştirme
## <a name="overview"></a>Genel Bakış
Bu öğreticide, bir StorSimple 8000 serisi cihaz bir kasayı değiştirme ve kaldırma açıklanmaktadır. 8600 çift muhafaza cihaz (iki kasa) iken StorSimple 8100 tek kutu cihaz (bir kasa) modelidir. 8600 model için bir cihazı devredebildiği olası iki kasa vardır: birincil kasa veya kasanın EBOD muhafazası için gövde.

Her iki durumda da Microsoft tarafından sevk edilen değiştirme kasa boştur. Hiçbir güç ve soğutma modülleri (PCMs) denetleyicisi modüller, katı hal disk sürücüleri (SSD'ler), sabit disk sürücülerinin (HDD'ler) veya modülleri EBOD dahil edilir.

> [!IMPORTANT]
> Kasayı değiştirme ve kaldırma gözden önce güvenlik bilgileri [StorSimple donanım bileşeni değişimi](storsimple-8000-hardware-component-replacement.md).


## <a name="remove-the-chassis"></a>Kasa Kaldır
StorSimple Cihazınızda kasa kaldırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-remove-a-chassis"></a>Bir kasayı kaldırmak için
1. StorSimple cihazı kapatmak ve güç kaynaklardan gelen bağlantısı kesildi emin olun.
2. Tüm ağ ve SAS kabloları varsa kaldırın.
3. Birim rafa kaldırın.
4. Her bir sürücü kaldırın ve, bunlar kaldırılır yuvaları dikkat edin. Daha fazla bilgi için [disk sürücüsünü Kaldır](storsimple-8000-disk-drive-replacement.md#remove-the-disk-drive).
5. EBOD muhafazası (Bu başarısız kasa ise) üzerinde EBOD denetleyicisi modülleri kaldırmak. Daha fazla bilgi için [EBOD denetleyicisini kaldırma](storsimple-8000-ebod-controller-replacement.md#remove-an-ebod-controller).
   
    (Bu başarısız kasa ise) birincil kasada denetleyicilerini kaldırın ve bunlar öğeleri kaldırıldı yuvaları not edin. Daha fazla bilgi için [bir denetleyicisini kaldırma](storsimple-8000-controller-replacement.md#remove-a-controller).

## <a name="install-the-chassis"></a>Kasa yükleyin
StorSimple Cihazınızda kasaya yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-a-chassis"></a>Bir kasaya yüklemek için
1. Kasa, raf bağlayın. Daha fazla bilgi için [rafa monte, StorSimple 8100 cihaz](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) veya [rafa monte, StorSimple 8600 cihaz](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).
2. Kasa rafa bağlı sonra bunlar daha önce yüklü aynı konumlarda denetleyicisi modüllerini yükleyin.
3. Aynı konumları ve bunlar, daha önce yüklenen yuvaları sürücüleri yükleyin.
   
   > [!NOTE]
   > SSD'ler yuvalarda yüklemeniz ve ardından HDD'ler yükleyin öneririz.
  
4. Raf ve bileşenlerin yüklü bağlı cihaz ile Cihazınızı bağlamak için uygun güç kaynakları ve cihazı açın. Ayrıntılar için bkz [StorSimple 8100 cihazınızın kablolarını bağlama](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) veya [StorSimple 8600 cihazınızın kablolarını bağlama](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [StorSimple donanım bileşeni değişimi](storsimple-8000-hardware-component-replacement.md).

