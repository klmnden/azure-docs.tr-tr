---
title: "StorSimple sanal dizi için erişim denetim kayıtlarını yönetme | Microsoft Docs"
description: "StorSimple sanal dizinin bir birimde hangi ana bilgisayarların bağlanabileceği belirlemek için erişim denetim kayıtlarının (ACRs) nasıl yönetildiğini açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: e154bb4f-faab-4d92-a593-900c3ddc9595
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2ce65aa4efba735305208f7a6d761bc2814d1b8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-storsimple-device-manager-to-manage-access-control-records-for-storsimple-virtual-array"></a>StorSimple sanal dizi için erişim denetimi kayıtları yönetmek için StorSimple cihaz Yöneticisi'ni kullanın

## <a name="overview"></a>Genel Bakış

Erişim denetimi kayıtları (ACRs), StorSimple sanal dizinin (olarak da bilinen StorSimple şirket içi sanal aygıtı) bir birimde hangi ana bilgisayarların bağlanabileceği belirtmenize olanak verir. ACRs belirli bir birimi ayarlayın ve ana bilgisayarların iSCSI nitelikli adlar (IQN'ler) içerir. Bir ana bilgisayara bir birime bağlanmaya çalıştığında, cihaz IQN adının bu birimde ilişkili ACR denetler ve bir eşleşme varsa, ardından bağlantı kurulur. **Erişim denetimi kayıtları** içinde dikey **yapılandırma** Aygıt Yöneticisi'ni hizmetinizi bölümünü konak karşılık gelen IQN'ler ile tüm erişim denetimi kayıtları görüntüler.

![Erişim denetimi kayıtlarını yönetme](./media/storsimple-virtual-array-manage-acrs/ova-manage-acrs.png)

Bu öğretici ACR ile ilgili aşağıdaki ortak görevler açıklanmaktadır:

* IQN'sini Al
* Bir erişim denetimi kaydı ekleme
* Bir erişim denetimi kaydı Düzenle
* Bir erişim denetimi kaydını sil

> [!IMPORTANT]
> 
> * Bir ACR bir birime atarken, bu birim bozuk olabilir çünkü birim aynı anda birden fazla kümelenmemiş ana bilgisayar tarafından erişmediğinden dikkatli olun.
> * Bir ACR bir birimden silerken, silme, okuma-yazma kesilme neden olabilir çünkü karşılık gelen ana birim eriştiğini değil emin olun.


## <a name="get-the-iqn"></a>IQN'sini Al

Windows Server 2012 çalıştıran bir Windows konağının IQN'sini almak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Bir ACR ekleme

Kullandığınız **erişim denetimi kayıtları** içinde dikey **yapılandırma** StorSimple Aygıt Yöneticisi'ni hizmetinizin ACRs eklemek için bölüm. Genellikle, bir ACR tek bir birim ile ilişkilendirin.

Bir birim bir ACR ilişkilendirme hakkında daha fazla bilgi için Git [birim Ekle](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).

> [!IMPORTANT]
> Bir ACR bir birime atarken, bu birim bozuk olabilir çünkü birim aynı anda birden fazla kümelenmemiş ana bilgisayar tarafından erişmediğinden dikkatli olun.


Bir ACR eklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-add-an-acr"></a>Bir ACR eklemek için

1. Hizmet giriş sayfasında hizmetinizi seçin, hizmet adına çift tıklayın ve ardından içinde **yapılandırma** 'yi tıklatın **erişim denetimi kayıtları**.
2. İçinde **erişim denetimi kayıtları** dikey penceresinde tıklatın **Ekle**.
3. İçinde **eklemek ACR** dikey penceresinde aşağıdakileri yapın:
   
    1. ACR’nize bir **Ad** verin.
    
    2. Altında **iSCSI başlatıcısı adı**, Windows ana bilgisayarınız IQN adını sağlayın. Windows Server konağının IQN'sini almak için aşağıdakileri yapın:
   
    3. Microsoft iSCSI başlatıcısını Windows konağında başlatın. İSCSI Başlatıcı Özellikleri penceresinde üzerinde **yapılandırma** sekmesinde seçin ve dizeden kopyalama **Başlatıcı adı** alan.
    Bu dizesini yapıştırabilir **IQN** alanındaki **eklemek ACR** dikey.
   
    6. Tıklatın **Ekle** ACR eklemek için.  
   
        ![Erişim denetimi kayıtları ekleyin](./media/storsimple-virtual-array-manage-acrs/ova-add-acrs.png)
4. Sekmeli liste, bu toplama yansıtacak şekilde güncelleştirilir.

## <a name="edit-an-acr"></a>Bir ACR Düzenle

Kullandığınız **erişim denetimi kayıtları** içinde dikey **yapılandırma** Aygıt Yöneticisi'ni hizmetinizi ACRs düzenlemek için Azure portalında bölümü.

> [!NOTE]
> Şu anda kullanımda bir ACR değiştirmemeniz gerekir. Şu anda kullanılmakta olan bir birimi ile ilişkili bir ACR düzenlemek için önce birim çevrimdışı duruma.


Bir ACR düzenlemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-edit-an-acr"></a>Bir ACR düzenlemek için

1. Hizmet giriş sayfasında hizmetinizi seçin, hizmet adına çift tıklayın ve ardından içinde **yapılandırma** bölümünde **erişim denetimi kayıtları**.
2. İçinde **erişim denetimi kayıtları** dikey penceresinde, erişim denetimi kayıtları Tablo listesini değiştirmek istediğiniz ACR çift tıklayın.
3. İçinde **düzenleme erişim denetimi kayıtları** dikey penceresinde aşağıdakileri yapın:
   
    1. IQN için ACR sağlayın.
   
    2. Tıklatın **kaydetmek** değiştirilmiş ACR kaydetmek için dikey pencerenin üstündeki. Aşağıdaki onay iletisini görürsünüz:
   
        ![Erişim denetimi kayıtları düzenleme](./media/storsimple-virtual-array-manage-acrs/ova-edit-acrs.png)

## <a name="delete-an-access-control-record"></a>Bir erişim denetimi kaydını sil

Kullandığınız **yapılandırma** ACRs silmek için Azure portalında sayfası.

> [!NOTE]
> 
> * Şu anda kullanımda bir ACR silmemelisiniz. Şu anda kullanılmakta olan bir birimi ile ilişkili bir ACR silmek için önce birim çevrimdışı duruma.
> * Bir ACR bir birimden silerken, silme, okuma-yazma kesilme neden olabilir çünkü karşılık gelen ana birim eriştiğini değil emin olun.


Bir erişim denetimi kaydını silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-an-access-control-record"></a>Bir erişim denetimi kaydını silmek için

1. Hizmet giriş sayfasında hizmetinizi seçin, hizmet adına çift tıklayın ve ardından içinde **yapılandırma** bölümünde **erişim denetimi kayıtları**.

2. İçinde **erişim denetimi kayıtları** dikey penceresinde, erişim denetimi kayıtları Tablo listesini silmek istediğiniz ACR çift tıklayın.

3. Düzen erişim denetimi kayıtları dikey tıklayın **silmek**.
   
    ![ACRS Sil](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs.png)

4. Onayınız istendiğinde tıklatın **silmek** silme işlemine devam etmek için. Sekmeli liste silme yansıtacak şekilde güncelleştirilir.
   
   ![Uyarı iletisi](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs-warning.png)

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinmek [birimleri ekleme ve ACRs yapılandırma](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).

