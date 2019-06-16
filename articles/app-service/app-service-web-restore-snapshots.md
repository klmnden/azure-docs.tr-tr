---
title: -Azure App Service yedekleme dosyasından geri yükler
description: Uygulamanızı bir anlık görüntüden geri yüklemeyi öğreneceksiniz.
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
ms.custom: seodec18
ms.openlocfilehash: 8d4290f1411749e2d8d3d27fbd792ceeeea47ef7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60851367"
---
# <a name="restore-an-app-in-azure-from-a-snapshot"></a>Uygulamayı azure'daki bir anlık görüntüden geri yükleme
Bu makalede, uygulamanızı geri yükleme işlemini göstermektedir [Azure App Service](../app-service/overview.md) anlık görüntüden. Uygulamanız, uygulamanızın anlık görüntüleri birini temel alan bir önceki durumuna geri yükleyebilirsiniz. Anlık görüntüleri yedeklemeyi etkinleştirme gerekmez, platform otomatik olarak veri kurtarma amacıyla tüm uygulamalara anlık görüntüsünü kaydeder.

Anlık görüntüleri olan artımlı gölge kopyaları ve bunlar normal kıyasla çeşitli avantajlar sunar [yedeklemeleri](manage-backup.md):
- Dosya kilitleri nedeniyle dosya kopyalama hatası olmadığını.
- Depolama boyutu sınırlaması yoktur.
- Herhangi bir yapılandırma gerekmez.

Anlık görüntülerden geri çalışan uygulamalar için kullanılabilir **Premium** katmanı veya üzeri. Uygulamanızın ölçeğini genişletme hakkında daha fazla bilgi için bkz: [azure'da uygulamanın ölçeğini](web-sites-scale.md).

## <a name="limitations"></a>Sınırlamalar

- Bu özellik şu anda Önizleme aşamasındadır.
- Yalnızca aynı uygulamaya veya bu uygulamaya ait bir yuva geri yükleyebilirsiniz.
- App Service, geri yükleme yaparken, hedef uygulama veya hedef yuva durdurur.
- App Service, üç ay değerinde anlık görüntüleri platform veri kurtarma amacıyla tutar.
- Yalnızca son 30 güne ait anlık görüntü geri yükleyebilirsiniz.
- Bir App Service ortamında çalışan uygulama hizmetleri anlık görüntülerini desteklemez.
 

## <a name="restore-an-app-from-a-snapshot"></a>Uygulamayı bir anlık görüntüden geri yükleme

1. Üzerinde **ayarları** uygulamanızda sayfasının [Azure portalında](https://portal.azure.com), tıklayın **yedeklemeleri** görüntülenecek **yedeklemeleri** sayfası. Ardından **geri** altında **Snapshot(Preview)** bölümü.
   
    ![](./media/app-service-web-restore-snapshots/1.png)

2. İçinde **geri** sayfasında, geri yüklemek için anlık görüntü seçin.
   
    ![](./media/app-service-web-restore-snapshots/2.png)
   
3. Uygulama geri yükleme hedefini belirtin **geri yükleme hedefini**.
   
    ![](./media/app-service-web-restore-snapshots/3.png)
   
   > [!WARNING]
   > Seçerseniz **üzerine yaz**tüm uygulamanızın dosya sistemini mevcut veriler silinir ve üzerine. Tıklamadan önce **Tamam**, yapmak istediğiniz olduğundan emin olun.
   > 
   > 
      
   > [!Note]
   > Geçerli teknik sınırlamaları nedeniyle, aynı ölçek birimi uygulamalarında yalnızca geri yükleyebilirsiniz. Bu sınırlama, gelecekteki bir sürümde kaldırılacak.
   > 
   > 
   
    Seçebileceğiniz **var olan bir uygulamayı** bir yuvaya geri yüklemek için. Bu seçeneği kullanmadan önce bir yuva uygulamanızda oluşturmuş olmanız.

4. Site yapılandırmanıza geri yüklemeyi tercih edebilirsiniz.
   
    ![](./media/app-service-web-restore-snapshots/4.png)

5. **Tamam**'ı tıklatın.
