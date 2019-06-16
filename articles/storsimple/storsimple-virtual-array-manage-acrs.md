---
title: StorSimple Virtual Array için erişim denetimi kayıtları yönetme | Microsoft Docs
description: StorSimple sanal dizisi bir birimde hangi ana bilgisayarların bağlanabileceği belirlemek için erişim denetimi kayıtları (Acr'leri) yönetme işlemi açıklanır.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: e154bb4f-faab-4d92-a593-900c3ddc9595
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1dfc1b0e0576402624bfe62de0e206d9bd7cd1b0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64724422"
---
# <a name="use-storsimple-device-manager-to-manage-access-control-records-for-storsimple-virtual-array"></a>StorSimple Virtual Array için erişim denetimi kayıtları yönetmek için StorSimple cihaz Yöneticisi'ni kullanın

## <a name="overview"></a>Genel Bakış

Erişim denetimi kayıtları (Acr'leri), StorSimple sanal dizisi (diğer adıyla StorSimple şirket içi sanal cihazı) bir birimde hangi ana bilgisayarların bağlanabileceği belirtmenizi sağlar. Acr'leri belirli bir birim için ayarlanır ve konağın iSCSI tam adları (IQN'ler) içerir. Bir konak, bir birime bağlanmaya çalıştığında, cihaz söz konusu birimde IQN adı ile ilişkili ACR denetler ve bir eşleşme varsa, daha sonra bağlantı kurulur. **Erişim denetimi kayıtları** dikey pencerede **yapılandırma** bölümü cihaz Yöneticisi hizmetinizin ana karşılık gelen IQN'ler ile tüm erişim denetimi kayıtları görüntüler.

![Erişim denetimi kayıtları Yönet](./media/storsimple-virtual-array-manage-acrs/ova-manage-acrs.png)

Bu öğreticide, ACR ile ilgili aşağıdaki ortak görevler açıklanmaktadır:

* IQN Al
* Erişim denetimi kaydı ekleme
* Bir erişim denetimi kaydını Düzenle
* Bir erişim denetim kaydını silme

> [!IMPORTANT]
> 
> * Bir ACR'yi bir birime atarken, bu birimin bozuk olabilir çünkü birim eşzamanlı olarak birden fazla kümelenmemiş konağıyla erişmediğinden ilgileniriz.
> * Bir ACR'yi bir birimden silerken, silme, okuma-yazma kesinti neden olabileceğinden karşılık gelen konak birimi eriştiğini değil emin olun.


## <a name="get-the-iqn"></a>IQN Al

Windows Server 2012 çalıştıran bir Windows konağının IQN'sini almak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Bir ACR ekleme

Kullandığınız **erişim denetimi kayıtları** dikey pencerede **yapılandırma** Acr'leri eklemek için StorSimple cihaz Yöneticisi hizmetinizin bölümü. Genellikle, bir ACR ile tek bir birim ilişkilendirin.

Bir birim bir ACR ilişkilendirme hakkında daha fazla bilgi için Git [birim Ekle](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).

> [!IMPORTANT]
> Bir ACR'yi bir birime atarken, bu birimin bozuk olabilir çünkü birim eşzamanlı olarak birden fazla kümelenmemiş konağıyla erişmediğinden ilgileniriz.


Bir ACR'yi eklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-add-an-acr"></a>Bir ACR'yi eklemek için

1. Hizmet giriş sayfasında hizmetinizi seçin, hizmet adına çift tıklayın ve ardından içinde **yapılandırma** bölümünde **erişim denetimi kayıtları**.
2. İçinde **erişim denetimi kayıtları** dikey penceresinde tıklayın **Ekle**.
3. İçinde **ACR ekleme** dikey penceresinde aşağıdakileri yapın:
   
    1. ACR’nize bir **Ad** verin.
    
    2. Altında **iSCSI başlatıcısı adı**, Windows ana bilgisayarınız IQN adı sağlayın. Windows Server konağının IQN'sini almak için aşağıdakileri yapın:
   
    3. Microsoft iSCSI başlatıcısını Windows konağında başlatın. İSCSI başlatıcısı Özellikleri penceresinde üzerinde **yapılandırma** sekmesinde seçin ve dizeden kopyalama **Başlatıcı adı** alan.
    Bu dizesini yapıştırabilir **IQN** alanındaki **ACR ekleme** dikey penceresi.
   
    6. Tıklayın **Ekle** ACR eklemek için.  
   
        ![Erişim denetimi kayıtları ekleyin](./media/storsimple-virtual-array-manage-acrs/ova-add-acrs.png)
4. Tablosal bu ekleme yansıtacak şekilde güncelleştirilir.

## <a name="edit-an-acr"></a>Bir ACR'yi Düzenle

Kullandığınız **erişim denetimi kayıtları** dikey pencerede **yapılandırma** Acr'leri düzenlemek için Azure portalında cihaz Yöneticisi hizmetinizin bölümü.

> [!NOTE]
> Şu anda kullanılmakta olan bir ACR'yi değiştirmeniz gerekir. Şu anda kullanılmakta olan bir birimi ile ilişkilendirilmiş bir ACR'yi düzenlemek için önce çevrimdışı duruma.


Bir ACR'yi düzenlemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-edit-an-acr"></a>Bir ACR'yi düzenlemek için

1. Hizmet giriş sayfasında hizmetinizi seçin, hizmet adına çift tıklayın ve ardından içinde **yapılandırma** bölümünde **erişim denetimi kayıtları**.
2. İçinde **erişim denetimi kayıtları** erişim denetimi kayıtları tablosal cihaz listesinden dikey penceresinde değiştirmek istediğiniz ACR çift tıklayın.
3. İçinde **düzenleme erişim denetimi kayıtları** dikey penceresinde aşağıdakileri yapın:
   
    1. ACR için IQN sağlayın.
   
    2. Tıklayın **Kaydet** değiştirilmiş ACR kaydetmek için dikey pencerenin üst kısmındaki. Şu onaylama iletisini görürsünüz:
   
        ![Erişim denetimi kayıtları düzenleme](./media/storsimple-virtual-array-manage-acrs/ova-edit-acrs.png)

## <a name="delete-an-access-control-record"></a>Bir erişim denetim kaydını silme

Kullandığınız **yapılandırma** Acr'leri silmek için Azure portalında sayfası.

> [!NOTE]
> 
> * Şu anda kullanılmakta olan bir ACR'yi silmemelisiniz. Şu anda kullanılmakta olan bir birimi ile ilişkilendirilmiş bir ACR'yi silmek için önce çevrimdışı duruma.
> * Bir ACR'yi bir birimden silerken, silme, okuma-yazma kesinti neden olabileceğinden karşılık gelen konak birimi eriştiğini değil emin olun.


Bir erişim denetim kaydını silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-an-access-control-record"></a>Bir erişim denetim kaydını silmek için

1. Hizmet giriş sayfasında hizmetinizi seçin, hizmet adına çift tıklayın ve ardından içinde **yapılandırma** bölümünde **erişim denetimi kayıtları**.

2. İçinde **erişim denetimi kayıtları** erişim denetimi kayıtları tablosal cihaz listesinden dikey penceresinde silmek istediğiniz ACR çift tıklayın.

3. Düzenleme erişim denetimi kayıtları dikey penceresinde tıklayın **Sil**.
   
    ![Delete ACRS](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs.png)

4. Onayınız istendiğinde tıklayın **Sil** silme işlemine devam etmek için. Tablosal silinmesini yansıtacak şekilde güncelleştirilir.
   
   ![Uyarı iletisi](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs-warning.png)

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [birimleri ekleme ve yapılandırma Acr'leri](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).

