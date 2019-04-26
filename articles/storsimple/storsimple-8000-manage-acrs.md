---
title: StorSimple, erişim denetimi kayıtları yönetme | Microsoft Docs
description: Erişim denetimi kayıtları (Acr'leri) barındıran bir birime bağlanabilir belirlemek için StorSimple cihazında nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: alkohli
ms.openlocfilehash: ade7da25d2307a382c17e7a3cbb26b601c34ef78
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60321629"
---
# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a>Erişim denetimi kayıtları yönetmek için StorSimple Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış
Erişim denetimi kayıtları (Acr'leri), StorSimple cihazında bir birim hangi ana bilgisayarların bağlanabileceği belirtmenizi sağlar. Acr'leri belirli bir birim için ayarlanır ve konağın iSCSI tam adları (IQN'ler) içerir. Bir konak, bir birime bağlanmaya çalıştığında, cihaz IQN adı söz konusu birimde ACR ilişkili ve bir eşleşme varsa, daha sonra bağlantı kurulur denetler. Erişim denetimi kayıtları içinde **yapılandırma** StorSimple cihaz Yöneticisi hizmet dikey bölümünü karşılık gelen IQN'ler konakları ile tüm erişim denetimi kayıtları görüntüler.

Bu öğreticide, ACR ile ilgili aşağıdaki ortak görevler açıklanmaktadır:

* Erişim denetimi kaydı ekleme
* Bir erişim denetimi kaydını Düzenle
* Bir erişim denetim kaydını silme

> [!IMPORTANT]
> * Bir ACR'yi bir birime atarken, bu birimin bozuk olabilir çünkü birim eşzamanlı olarak birden fazla kümelenmemiş konağıyla erişmediğinden ilgileniriz.
> * Bir ACR'yi bir birimden silerken, silme, okuma-yazma kesinti neden olabileceğinden karşılık gelen konak birimi eriştiğini değil emin olun.

## <a name="get-the-iqn"></a>IQN Al

Windows Server 2012 çalıştıran bir Windows konağının IQN'sini almak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]


## <a name="add-an-access-control-record"></a>Erişim denetimi kaydı ekleme
Kullandığınız **yapılandırma** Acr'leri eklemek için StorSimple cihaz Yöneticisi hizmet dikey bölümde. Genellikle, bir ACR ile tek bir birim ilişkilendireceksiniz.

Bir ACR'yi eklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-add-an-acr"></a>Bir ACR'yi eklemek için

1. StorSimple cihaz Yöneticisi hizmetinize gidin, hizmet adına çift tıklayın ve ardından içinde **yapılandırma** bölümünde **erişim denetimi kayıtları**.
2. İçinde **erişim denetimi kayıtları** dikey penceresinde tıklayın **+ ACR ekleme**.

    ![ACR Ekle seçeneğine tıklayın](./media/storsimple-8000-manage-acrs/createacr1.png)

3. İçinde **ACR ekleme** dikey penceresinde aşağıdaki adımları uygulayın:

    1. Bir ad verin sağlayın.
    
    2. Windows Server konağınızda IQN adı sağlayın **iSCSI başlatıcısı adı (IQN)**.

    3. Tıklayın **Ekle** ACR oluşturma.

        ![ACR Ekle seçeneğine tıklayın](./media/storsimple-8000-manage-acrs/createacr2.png)

4.  Yeni eklenen ACR Acr'leri tablosal listesinde görüntülenir.

    ![ACR Ekle seçeneğine tıklayın](./media/storsimple-8000-manage-acrs/createacr5.png)


## <a name="edit-an-access-control-record"></a>Bir erişim denetimi kaydını Düzenle
Kullandığınız **yapılandırma** Acr'leri düzenlemek için StorSimple cihaz Yöneticisi hizmet dikey bölümde.

> [!NOTE]
> Şu anda kullanımda olmayan Acr'leri değiştirmeniz önerilir. Şu anda kullanılmakta olan bir birimi ile ilişkilendirilmiş bir ACR'yi düzenlemek için önce birimi çevrimdışı duruma getirmeniz gerekir.

Bir ACR'yi düzenlemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-edit-an-access-control-record"></a>Erişim denetimi kaydı düzenlemek için
1.  StorSimple cihaz Yöneticisi hizmetinize gidin, hizmet adına çift tıklayın ve ardından içinde **yapılandırma** bölümünde **erişim denetimi kayıtları**.

    ![Erişim denetimi kayıtları gidin](./media/storsimple-8000-manage-acrs/createacr1.png)

2. Erişim denetimi kayıtları tablosal listesinde,'a tıklayın ve değiştirmek istediğiniz ACR seçin.

    ![Erişim denetimi kayıtları düzenleme](./media/storsimple-8000-manage-acrs/editacr1.png)

3. İçinde **düzenleme erişim denetim kaydını** dikey penceresinde, başka bir konağa karşılık gelen farklı bir IQN sağlayın.

    ![Erişim denetimi kayıtları düzenleme](./media/storsimple-8000-manage-acrs/editacr2.png)

4. **Kaydet**’e tıklayın. Onayınız istendiğinde **Evet**’e tıklayın. 

    ![Erişim denetimi kayıtları düzenleme](./media/storsimple-8000-manage-acrs/editacr3.png)

5. ACR güncelleştirildiğinde size bildirilir. Tablo listesi de değişikliği yansıtacak şekilde güncelleştirir.

   
## <a name="delete-an-access-control-record"></a>Bir erişim denetim kaydını silme
Kullandığınız **yapılandırma** Acr'leri silmek için StorSimple cihaz Yöneticisi hizmet dikey bölümde.

> [!NOTE]
> Şu anda kullanımda olmayan Acr'leri silebilirsiniz. Şu anda kullanılmakta olan bir birimi ile ilişkilendirilmiş bir ACR'yi silmek için önce birimi çevrimdışı duruma getirmeniz gerekir.

Bir erişim denetim kaydını silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-an-access-control-record"></a>Bir erişim denetim kaydını silmek için
1.  StorSimple cihaz Yöneticisi hizmetinize gidin, hizmet adına çift tıklayın ve ardından içinde **yapılandırma** bölümünde **erişim denetimi kayıtları**.

    ![Erişim denetimi kayıtları gidin](./media/storsimple-8000-manage-acrs/createacr1.png)

2. Erişim denetimi kayıtları tablosal listesinde,'a tıklayın ve silmek istediğiniz ACR seçin.

    ![Erişim denetimi kayıtları gidin](./media/storsimple-8000-manage-acrs/deleteacr1.png)

3. Bağlam menüsünü açmak ve seçmek için sağ tıklama **Sil**.

    ![Erişim denetimi kayıtları gidin](./media/storsimple-8000-manage-acrs/deleteacr2.png)

4. Onayınız istendiğinde bilgileri gözden geçirin ve ardından **Sil**.

    ![Erişim denetimi kayıtları gidin](./media/storsimple-8000-manage-acrs/deleteacr3.png)

5. Silme işlemi tamamlandığında size bildirilir. Tablosal silinmesini yansıtacak şekilde güncelleştirilir.

    ![Erişim denetimi kayıtları gidin](./media/storsimple-8000-manage-acrs/deleteacr5.png)

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [StorSimple birimlerini yönetme](storsimple-8000-manage-volumes-u2.md).
* Daha fazla bilgi edinin [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

