---
title: Uygulamanızı Azure’a geri yükleme
description: Uygulamanızı bir anlık görüntüden geri yükleme hakkında bilgi edinin.
services: app-service
documentationcenter: ''
author: ahmedelnably
manager: cfowler
editor: ''
ms.assetid: 4164f9b5-f735-41c6-a2bb-71f15cdda417
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.topic: article
ms.date: 04/04/2018
ms.author: aelnably;nicking
ms.openlocfilehash: e1ae8fcc30323c865aa96937f43054515f293394
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33767304"
---
# <a name="restore-an-app-in-azure-from-a-snapshot"></a>Bir Azure uygulamasında bir anlık görüntüden geri yükleme
Bu makalede, bir uygulamada geri yükleme gösterilmektedir [Azure App Service](../app-service/app-service-web-overview.md) bir anlık. Uygulamanız, uygulamanızın anlık görüntüleri birini temel alan bir önceki durumuna geri yükleyebilirsiniz. Anlık görüntüler yedeklemeyi etkinleştirme gerekmez, platform anlık görüntü veri kurtarma amacıyla tüm uygulamaların otomatik olarak kaydeder.

Anlık görüntüleri olan artımlı gölge kopyaları ve normal birkaç avantaj sunar [yedeklemeleri](web-sites-backup.md):
- Hiçbir dosya kopyalama hatası nedeniyle dosya kilitler.
- Depolama boyutu sınırlama.
- Herhangi bir yapılandırma gerekmez.

Anlık görüntülerin geri yüklenmesi, çalışan uygulamalar için kullanılabilir **Premium** katmanı ya da daha yüksek. Uygulamanızı ölçeklendirme hakkında daha fazla bilgi için bkz: [Azure bir uygulamada ölçeklendirin](web-sites-scale.md).

## <a name="limitations"></a>Sınırlamalar

- Bu özellik şu anda önizlemede değil.
- Yalnızca aynı uygulamayı veya bu uygulamaya ait bir yuva geri yükleyebilirsiniz.
- Uygulama hizmeti, geri yükleme yaparken hedef uygulaması ya da hedef yuva durdurur.
- Uygulama hizmeti, üç ay değerinde anlık görüntüleri platform veri kurtarma amacıyla tutar.
- Yalnızca son 30 gün için anlık görüntü geri yükleyebilirsiniz.
 

## <a name="restore-an-app-from-a-snapshot"></a>Bir uygulama bir anlık görüntüden geri yükleme

1. Üzerinde **ayarları** uygulamanızda sayfasının [Azure portal](https://portal.azure.com), tıklatın **yedeklemeleri** görüntülemek için **yedeklemeleri** sayfası. Ardından **geri** altında **Snapshot(Preview)** bölümü.
   
    ![](./media/app-service-web-restore-snapshots/1.png)

2. İçinde **geri** sayfasında, geri yüklemek için anlık görüntü seçin.
   
    ![](./media/app-service-web-restore-snapshots/2.png)
   
3. Uygulama geri yükleme hedefini belirtin **geri yükleme hedefini**.
   
    ![](./media/app-service-web-restore-snapshots/3.png)
   
   > [!WARNING]
   > Seçerseniz **üzerine yaz**, tüm uygulamanızın geçerli dosya sistemindeki varolan verileri silinir ve üzerine. Tıklamadan önce **Tamam**, ne yapmak istiyorsunuz olduğundan emin olun.
   > 
   > 
      
   > [!Note]
   > Geçerli teknik sınırlamaları nedeniyle, aynı ölçek birimi uygulamalarında yalnızca geri yükleyebilirsiniz. Bu sınırlama, bir sonraki sürümde kaldırılacak.
   > 
   > 
   
    Seçebileceğiniz **var olan bir uygulamayı** bir yuvaya geri yüklemek için. Bu seçenek kullanmadan önce bir yuva uygulamanızı oluşturmuş.

4. Site yapılandırmanızı geri yüklemek seçebilirsiniz.
   
    ![](./media/app-service-web-restore-snapshots/4.png)

5. **Tamam**’a tıklayın.
