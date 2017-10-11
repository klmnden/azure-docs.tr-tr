---
title: "StorSimple erişim denetimi kayıtları yönetme | Microsoft Docs"
description: "Erişim denetimi kayıtları (ACRs) barındıran bir birime bağlanabilir belirlemek için StorSimple cihazında nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: alkohli
ms.openlocfilehash: 9173e34f889ce1c082b20bb382cb6ca9a03dd797
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a>Erişim denetimi kayıtları yönetmek için StorSimple Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış
Erişim denetimi kayıtları (ACRs), StorSimple cihaz üzerindeki bir birimi hangi ana bilgisayarların bağlanabileceği belirtmenize olanak verir. ACRs belirli bir birimi ayarlayın ve ana bilgisayarların iSCSI nitelikli adlar (IQN'ler) içerir. Bir ana bilgisayara bir birime bağlanmaya çalıştığında, cihaz IQN adının bu birimde ACR ilişkili ve bir eşleşme varsa, ardından bağlantı kurulur denetler. Erişim denetimi kayıtları **yapılandırma** StorSimple cihaz Yöneticisi hizmeti dikey pencerenize bölümünü konak karşılık gelen IQN'ler ile tüm erişim denetimi kayıtları görüntüler.

Bu öğretici ACR ile ilgili aşağıdaki ortak görevler açıklanmaktadır:

* Bir erişim denetimi kaydı ekleme
* Bir erişim denetimi kaydı Düzenle
* Bir erişim denetimi kaydını sil

> [!IMPORTANT]
> * Bir ACR bir birime atarken, bu birim bozuk olabilir çünkü birim aynı anda birden fazla kümelenmemiş ana bilgisayar tarafından erişmediğinden dikkatli olun.
> * Bir ACR bir birimden silerken, silme, okuma-yazma kesilme neden olabilir çünkü karşılık gelen ana birim eriştiğini değil emin olun.

## <a name="get-the-iqn"></a>IQN'sini Al

Windows Server 2012 çalıştıran bir Windows konağının IQN'sini almak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]


## <a name="add-an-access-control-record"></a>Bir erişim denetimi kaydı ekleme
Kullandığınız **yapılandırma** ACRs eklemek için StorSimple cihaz Yöneticisi hizmeti dikey bölümde. Genellikle, bir birimle bir ACR ilişkilendireceğiniz.

Bir ACR eklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-add-an-acr"></a>Bir ACR eklemek için

1. StorSimple cihaz Yöneticisi hizmetinize gidin, hizmet adına çift tıklayın ve ardından içinde **yapılandırma** 'yi tıklatın **erişim denetimi kayıtları**.
2. İçinde **erişim denetimi kayıtları** dikey penceresinde tıklatın **+ ACR eklemek**.

    ![ACR Ekle'yi tıklatın](./media/storsimple-8000-manage-acrs/createacr1.png)

3. İçinde **eklemek ACR** dikey penceresinde, aşağıdaki adımları uygulayın:

    1. Bir ad verin sağlayın.
    
    2. Windows Server konağınızda altında IQN adını sağlayın **iSCSI başlatıcısı adı (IQN)**.

    3. Tıklatın **Ekle** ACR oluşturmak için.

        ![ACR Ekle'yi tıklatın](./media/storsimple-8000-manage-acrs/createacr2.png)

4.  Yeni eklenen ACR ACRs Tablo listesinde görüntülenir.

    ![ACR Ekle'yi tıklatın](./media/storsimple-8000-manage-acrs/createacr5.png)


## <a name="edit-an-access-control-record"></a>Bir erişim denetimi kaydı Düzenle
Kullandığınız **yapılandırma** ACRs düzenlemek için StorSimple cihaz Yöneticisi hizmeti dikey bölümde.

> [!NOTE]
> Şu anda kullanımda olmayan ACRs değiştirmeniz önerilir. Şu anda kullanılmakta olan bir birimi ile ilişkili bir ACR düzenlemek için önce birim çevrimdışı duruma getirmeniz gerekir.

Bir ACR düzenlemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-edit-an-access-control-record"></a>Bir erişim denetimi kaydını düzenlemek için
1.  StorSimple cihaz Yöneticisi hizmetinize gidin, hizmet adına çift tıklayın ve ardından içinde **yapılandırma** 'yi tıklatın **erişim denetimi kayıtları**.

    ![Erişim denetimi kayıtları gidin](./media/storsimple-8000-manage-acrs/createacr1.png)

2. Erişim denetimi kayıtları Tablo listesi, tıklayın ve değiştirmek istediğiniz ACR seçin.

    ![Erişim denetimi kayıtları düzenleme](./media/storsimple-8000-manage-acrs/editacr1.png)

3. İçinde **düzenleme erişim denetimi kaydı** dikey penceresinde, başka bir ana bilgisayara karşılık gelen farklı bir IQN sağlayın.

    ![Erişim denetimi kayıtları düzenleme](./media/storsimple-8000-manage-acrs/editacr2.png)

4. **Kaydet** düğmesine tıklayın. Onayınız istendiğinde **Evet**’e tıklayın. 

    ![Erişim denetimi kayıtları düzenleme](./media/storsimple-8000-manage-acrs/editacr3.png)

5. ACR güncelleştirildiğinde size bildirilir. Tablo listeleme ayrıca değişikliği yansıtacak şekilde güncelleştirir.

   
## <a name="delete-an-access-control-record"></a>Bir erişim denetimi kaydını sil
Kullandığınız **yapılandırma** ACRs silmek için StorSimple cihaz Yöneticisi hizmeti dikey bölümde.

> [!NOTE]
> Şu anda kullanımda olmayan ACRs silebilirsiniz. Şu anda kullanılmakta olan bir birimi ile ilişkili bir ACR silmek için önce birim çevrimdışı duruma getirmeniz gerekir.

Bir erişim denetimi kaydını silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-an-access-control-record"></a>Bir erişim denetimi kaydını silmek için
1.  StorSimple cihaz Yöneticisi hizmetinize gidin, hizmet adına çift tıklayın ve ardından içinde **yapılandırma** 'yi tıklatın **erişim denetimi kayıtları**.

    ![Erişim denetimi kayıtları gidin](./media/storsimple-8000-manage-acrs/createacr1.png)

2. Erişim denetimi kayıtları Tablo listesi, tıklayın ve silmek istediğiniz ACR seçin.

    ![Erişim denetimi kayıtları gidin](./media/storsimple-8000-manage-acrs/deleteacr1.png)

3. Bağlam menüsü çağırma ve seçmek için sağ **silmek**.

    ![Erişim denetimi kayıtları gidin](./media/storsimple-8000-manage-acrs/deleteacr2.png)

4. Onaylamanız istendiğinde, bilgileri gözden geçirin ve ardından **silmek**.

    ![Erişim denetimi kayıtları gidin](./media/storsimple-8000-manage-acrs/deleteacr3.png)

5. Silme işlemi tamamlandığında size bildirilir. Sekmeli liste silme yansıtacak şekilde güncelleştirilir.

    ![Erişim denetimi kayıtları gidin](./media/storsimple-8000-manage-acrs/deleteacr5.png)

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [StorSimple birimlerini yönetme](storsimple-8000-manage-volumes-u2.md).
* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

