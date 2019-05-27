---
title: include dosyası
description: include dosyası
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 06/08/2018
ms.author: alkohli
ms.custom: include file
ms.openlocfilehash: e683d17422321b780a1c01b3011292f2e2c631cb
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66156104"
---
Birim kapsayıcısını silme için şunları yapmanız gerekir
 - Birim kapsayıcısı birimleri silin. Birim kapsayıcısı ilişkili birimlere sahip ise bu birimleri çevrimdışı önce yapın. Bağlantısındaki [bir birimi çevrimdışı duruma](../articles/storsimple/storsimple-8000-manage-volumes-u2.md#take-a-volume-offline). Birimlerin çevrimdışı olduğundan sonra bunları silebilirsiniz. 
 - ilişkili yedekleme ilkeleri silin ve bulut anlık görüntüleri. Yedekleme ilkeleri ve bulut anlık görüntüleri Birim kapsayıcısı ilişkili olmadığını kontrol edin. Bu durumda, ardından [yedekleme ilkelerini silin](../articles/storsimple/storsimple-8000-manage-backup-policies-u2.md#delete-a-backup-policy). Bu, bulut anlık görüntüleri de siler. 
 
Birim kapsayıcısı yok ilişkili birimleri, yedekleme ilkeleri ve bulut anlık görüntüleri varsa, bunu silebilirsiniz. Birim kapsayıcısını silmek için aşağıdaki yordamı gerçekleştirin.

#### <a name="to-delete-a-volume-container"></a>Bir birim kapsayıcısı silinemiyor
1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Seçin ve cihaza tıklayın ve ardından Git **ayarlar > Yönet > birim kapsayıcıları**.

    ![Birim kapsayıcıları dikey penceresi](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

2. Birim kapsayıcıları tablosal listeden silmek için sağ tıklayın, istediğiniz birim kapsayıcısı seçin **...**  seçip **Sil**.

    ![Birim kapsayıcısını silme](./media/storsimple-8000-delete-volume-container/deletevolumecontainer1.png)

3. Birim kapsayıcısı yok ilişkili birimleri, yedekleme ilkeleri ve bulut anlık görüntüleri ise silinebilir. Onayınız istendiğinde gözden geçirin ve birim kapsayıcısını silmenin etkilerini belirten onay kutusunu işaretleyin. Tıklayın **Sil** sonra birim kapsayıcısı silinemiyor.

    ![Silmeyi onayla](./media/storsimple-8000-delete-volume-container/deletevolumecontainer2.png)

Birim kapsayıcıları listesi, silinen bir birim kapsayıcısı yansıtacak şekilde güncelleştirilir.

![](./media/storsimple-8000-delete-volume-container/deletevolumecontainer5.png)


