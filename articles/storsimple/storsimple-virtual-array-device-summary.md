---
title: StorSimple sanal dizisi cihaz özeti dikey | Microsoft Docs
description: Özet dikey penceresinde cihaz için StorSimple cihaz Yöneticisi ve bunu StorSimple Virtual Array'iniz durumunu izlemek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: a13c1ea7-6428-4234-84a6-0ebf51670a85
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: manuaery
ms.openlocfilehash: 9edc0b552f5c2f38e646bc4b44dd8df5c16b0457
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61408515"
---
# <a name="use-the-device-summary-blade-for-storsimple-device-manager-connected-to-storsimple-virtual-array"></a>Kullanımı Özet dikey penceresinde cihaz için StorSimple cihaz Yöneticisi, StorSimple Virtual Array için bağlı

## <a name="overview"></a>Genel Bakış

StorSimple cihaz Yöneticisi cihaz dikey penceresinde, bir sistem yöneticisinin dikkat etmeniz gereken bu cihaz sorunları vurgulayan dizinin bir StorSimple sanal ile belirli bir StorSimple cihaz Yöneticisi, kayıtlı Özet görünümünü sağlar. Bu öğretici, cihaz Özet dikey penceresinde sunar, içerik ve işlevi açıklanmaktadır ve bu dikey pencereden gerçekleştirebileceğiniz görevler açıklanmaktadır.

Cihaz Özet dikey penceresinde aşağıdaki bilgileri görüntüler:

![Cihaz panosu](./media/storsimple-virtual-array-device-summary/device-blade.png)



## <a name="management"></a>Yönetim

StorSimple cihaz dikey penceresinde, StorSimple Cihazınızı yönetmek için seçeneklere bakın. Dikey pencerenin ve sol taraftaki üstteki yönetimi komutları bakın. Paylaşımları veya birimler eklemek, güncelleştirmek veya sanal diziniz başarısız için bu seçenekleri kullanın.

Temel alan durumu, model, yazılım sürümü hem de bir bağlantı gibi önemli özelliklerin bazılarını yakalar **Web kullanıcı arabirimini** dizi. Bir iç ağda olup, doğrudan başlatabilirsiniz [yerel web kullanıcı Arabirimi](storsimple-ova-web-ui-admin.md) sanal dizinizi yönetmek için.

![Cihaz temelleri](./media/storsimple-virtual-array-device-summary/device-essentials.png)

## <a name="storsimple-device-summary"></a>StorSimple cihaz özeti

* **Uyarılar** kutucuğu uyarı önem derecesine göre gruplanmış sanal diziniz için tüm etkin uyarıları anlık görüntüsünü sağlar. Açmak için kutucuğa tıklayın **uyarılar** dikey penceresinde ve ardından Eylemler dahil, bu uyarı hakkında ek ayrıntıları görüntülemek için bir bireysel uyarı önerilir. Uyarı, sorunu çözerseniz de temizleyebilirsiniz.

* **Kapasite** kutucuk kullanılabilir toplam depolama alanı göreli sanal cihaz için aynı üzerinden sağlanan ve kalan birincil depolama alanı görüntüler. **Sağlanan** hazır ve kullanılmak için ayrılan depolama alanı miktarının başvurduğu **kalan** bu cihaz sağlanabilir kalan kapasiteyi ifade eder. **Kalan katmanlı** bulut dahil olmak üzere sağlanabilir kullanılabilir kapasiteyi kapasitesidir sırada **kalan yerel** olduğundan bu sanal diziye bağlı diskler kalan kapasitesi.

* İçinde **kullanım** grafik, son 7 gün içindeki, varsayılan süre kullanılan bulut depolama alanına yanı sıra sanal dizinizin kullanılan birincil depolama görüntüleyebilirsiniz. Kullanım **Düzenle** farklı zaman ölçeği seçmek için sağ üst köşedeki grafiğin seçeneği.

* **Paylaşımları** veya **birimleri** kutucuğu paylaşımları veya birimler durumlarına göre gruplandırılmış cihazınızdaki sayısının bir özeti sağlar. Açmak için kutucuğa tıklayın **paylaşımları** veya **birimleri** listesi dikey penceresinde ve bir tek paylaşım veya birim, özelliklerini görüntülemek veya değiştirmek için'a tıklayın. Daha fazla bilgi için bkz. nasıl [paylaşımlarını](storsimple-virtual-array-manage-shares.md) veya [birimleri yönetme](storsimple-virtual-array-manage-volumes.md).

## <a name="next-steps"></a>Sonraki adımlar
Şunları nasıl yapacağınızı öğrenin:
- [StorSimple sanal dizisi paylaşımları yönetme](storsimple-virtual-array-manage-shares.md)
    
- [StorSimple sanal dizisi üzerinde birimleri yönetme](storsimple-virtual-array-manage-volumes.md)

