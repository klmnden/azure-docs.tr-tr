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
ms.openlocfilehash: e7f3f80c886f90a8bc3ae8c38e7d101c506439a6
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35250231"
---
Birim kapsayıcısı silmek için şunları yapmanız gerekir
 - Birim kapsayıcısı birimleri silin. Birim kapsayıcısı birimleri ilişkili varsa bu birimlerin çevrimdışı ilk alın. Adımları [bir birim çevrimdışına](../articles/storsimple/storsimple-8000-manage-volumes-u2.md#take-a-volume-offline). Birimleri çevrimdışı olduktan sonra bunları silebilirsiniz. 
 - ilişkili yedekleme ilkelerini silin ve bulut anlık görüntüler. Birim kapsayıcısı yedekleme ilkeleri ve bulut anlık görüntüleri ilişkili olmadığını denetleyin. Bu durumda, ardından [yedekleme ilkelerini Sil](../articles/storsimple/storsimple-8000-manage-backup-policies-u2.md#delete-a-backup-policy). Bu da bulut anlık görüntüleri siler. 
 
Birim kapsayıcısı yok ilişkili birimler, yedekleme ilkeleri ve bulut anlık görüntüleri olduğunda, silebilirsiniz. Birim kapsayıcısı silmek için aşağıdaki yordamı gerçekleştirin.

#### <a name="to-delete-a-volume-container"></a>Birim kapsayıcısı silmek için
1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Seçin ve aygıtı tıklatın ve ardından gidin **ayarlar > Yönet > birim kapsayıcıları**.

    ![Birim kapsayıcıları dikey penceresi](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

2. Birim kapsayıcıları Tablo listesinden silmek için sağ tıklayın, istediğiniz birim kapsayıcısı seçin **...**  ve ardından **silmek**.

    ![Birim kapsayıcısı Sil](./media/storsimple-8000-delete-volume-container/deletevolumecontainer1.png)

3. Birim kapsayıcısı yok ilişkili birimler, yedekleme ilkeleri ve bulut anlık görüntüleri varsa, silinebilir. Onayınız istendiğinde gözden geçirin ve birim kapsayıcısı silme etkisini belirten onay kutusunu seçin. Tıklatın **silmek** birim kapsayıcısı silmek için.

    ![Silme işlemini onaylama](./media/storsimple-8000-delete-volume-container/deletevolumecontainer2.png)

Birim kapsayıcıları listesi silinen birim kapsayıcısı yansıtacak şekilde güncelleştirilir.

![](./media/storsimple-8000-delete-volume-container/deletevolumecontainer5.png)


