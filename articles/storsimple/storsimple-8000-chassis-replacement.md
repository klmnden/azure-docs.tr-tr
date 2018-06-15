---
title: StorSimple 8000 serisi aygıtta kasa Değiştir | Microsoft Docs
description: StorSimple birincil muhafaza veya EBOD muhafazası için kasa kaldırdığınızda ve açıklar.
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
ms.openlocfilehash: 073fcf0064f1d1482f4683d733f00cf918ff2f38
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23874804"
---
# <a name="replace-the-chassis-on-your-storsimple-device"></a>Kasa, StorSimple Cihazınızda değiştirin
## <a name="overview"></a>Genel Bakış
Bu öğretici, kaldırmak ve StorSimple 8000 serisi aygıtta bir kasa Değiştir açıklanmaktadır. 8600 çift muhafaza aygıt (iki kasa) iken StorSimple 8100 model bir tek kasası (bir kasa), aygıttır. 8600 model için büyük olasılıkla aygıtı başarısız iki kasa vardır: birincil muhafaza veya EBOD muhafazası kasa için Kasa.

Her iki durumda da, Microsoft tarafından sevk değiştirme kasa boştur. Hiçbir güç ve soğutma modülleri (PCMs) denetleyicisi modülleri, katı hal disk sürücüleri (SSD), sabit disk sürücülerinin (HDD'ler) ya da EBOD modülleri dahil edilir.

> [!IMPORTANT]
> Kasa değiştirme ve kaldırma gözden önce güvenlik bilgileri [StorSimple donanım bileşeni değiştirme](storsimple-8000-hardware-component-replacement.md).


## <a name="remove-the-chassis"></a>Kasa Kaldır
Kasa, StorSimple Cihazınızda kaldırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-remove-a-chassis"></a>Bir kasa kaldırmak için
1. StorSimple cihazı kapatmak ve güç kaynaklardan gelen bağlantısı kesilmiş emin olun.
2. Tüm ağ ve SAS kabloları varsa kaldırın.
3. Birim raf kaldırın.
4. Her sürücü kaldırın ve, bunlar kaldırılır yuvaları unutmayın. Daha fazla bilgi için bkz: [disk sürücüsünü Kaldır](storsimple-8000-disk-drive-replacement.md#remove-the-disk-drive).
5. (Bu başarısız kasa ise) EBOD muhafazası üzerinde EBOD denetleyicisi modülleri kaldırın. Daha fazla bilgi için bkz: [EBOD denetleyicisi kaldırmak](storsimple-8000-ebod-controller-replacement.md#remove-an-ebod-controller).
   
    (Bu başarısız kasa ise) birincil kasada denetleyicilerini kaldırın ve bunlar kaldırılan yuvaları not edin. Daha fazla bilgi için bkz: [bir denetleyici kaldırmak](storsimple-8000-controller-replacement.md#remove-a-controller).

## <a name="install-the-chassis"></a>Kasa yükleyin
StorSimple Cihazınızda kasaya yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-a-chassis"></a>Bir kasaya yüklemek için
1. Raf kasaya bağlayın. Daha fazla bilgi için bkz: [rafa monte StorSimple 8100 aygıt](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) veya [rafa monte StorSimple 8600 aygıt](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).
2. Kasa rafa monte sonra bunlar daha önce yüklü aynı konumlarda denetleyicisi modüllerini yükleyin.
3. Aynı konum ve bunlar daha önce yüklü yuvaları sürücüleri yükleyin.
   
   > [!NOTE]
   > SSD yuvalarında ilk yükleyin ve HDD yükleyin öneririz.
  
4. Raf ve yüklenen bileşenlerle aygıtla Cihazınızı bağlamak için uygun güç kaynakları ve aygıtı açın. Ayrıntılar için bkz [StorSimple 8100 cihazınız kablo](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) veya [StorSimple 8600 model Cihazınızı kablo](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [StorSimple donanım bileşeni değiştirme](storsimple-8000-hardware-component-replacement.md).

