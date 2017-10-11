---
title: "StorSimple erişim denetimi kayıtları yönetme | Microsoft Docs"
description: "Erişim denetimi kayıtları (ACRs) barındıran bir birime bağlanabilir belirlemek için StorSimple cihazında nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2f1475d8-36a5-4cc4-84b9-adf8a310b60c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: a87624b5706c1d9b8c2b9926e5580996a89ce984
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a>Erişim denetimi kayıtları yönetmek için StorSimple Yöneticisi hizmetini kullanma
## <a name="overview"></a>Genel Bakış
Erişim denetimi kayıtları (ACRs), StorSimple cihaz üzerindeki bir birimi hangi ana bilgisayarların bağlanabileceği belirtmenize olanak verir. ACRs belirli bir birimi ayarlayın ve ana bilgisayarların iSCSI nitelikli adlar (IQN'ler) içerir. Bir ana bilgisayara bir birime bağlanmaya çalıştığında, cihaz IQN adının bu birimde ACR ilişkili ve bir eşleşme varsa, ardından bağlantı kurulur denetler. Erişim denetimi kayıtları bölüm üzerinde **yapılandırma** sayfası konak karşılık gelen IQN'ler ile tüm erişim denetimi kayıtları görüntüler.

Bu öğretici ACR ile ilgili aşağıdaki ortak görevler açıklanmaktadır:

* Bir erişim denetimi kaydı ekleme 
* Bir erişim denetimi kaydı Düzenle 
* Bir erişim denetimi kaydını sil 

> [!IMPORTANT]
> * Bir ACR bir birime atarken, bu birim bozuk olabilir çünkü birim aynı anda birden fazla kümelenmemiş ana bilgisayar tarafından erişmediğinden dikkatli olun. 
> * Bir ACR bir birimden silerken, silme, okuma-yazma kesilme neden olabilir çünkü karşılık gelen ana birim eriştiğini değil emin olun.
> 
> 

## <a name="add-an-access-control-record"></a>Bir erişim denetimi kaydı ekleme
StorSimple Yöneticisi hizmetini kullanma **yapılandırma** ACRs eklemek için sayfa. Genellikle, bir birimle bir ACR ilişkilendireceğiniz.

Bir ACR eklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-add-an-access-control-record"></a>Bir erişim denetimi kaydı eklemek için
1. Hizmet giriş sayfasında hizmetinizi seçin, hizmet adına çift tıklayın ve ardından **yapılandırma** sekmesi.
2. Altında Tablo listesindeki **erişim denetimi kayıtları**, sağlar bir **adı** verin.
3. Windows ana bilgisayarınız IQN adını sağlayın **iSCSI başlatıcısı adı**. Windows Server konağının IQN'sini almak için aşağıdakileri yapın:
   
   * Microsoft iSCSI başlatıcısını Windows konağında başlatın.
   * **iSCSI Başlatıcısı Özellikleri** penceresinin **Yapılandırma** sekmesinde, **Başlatıcı Adı** alanından dizeyi seçip kopyalayın.
   * Bu dizesini yapıştırabilir **iSCSI başlatıcısı adı** Klasik Azure portalı ACRs tablosunda alan.
4. Tıklatın **kaydetmek** yeni oluşturulan ACR kaydetmek için. Sekmeli liste, bu toplama yansıtacak şekilde güncelleştirilir.

## <a name="edit-an-access-control-record"></a>Bir erişim denetimi kaydı Düzenle
Kullandığınız **yapılandırma** ACRs düzenlemek için Klasik Azure portalındaki sayfası. 

> [!NOTE]
> Şu anda kullanımda olmayan ACRs değiştirebilirsiniz. Şu anda kullanılmakta olan bir birimi ile ilişkili bir ACR düzenlemek için önce birim çevrimdışı duruma getirmeniz gerekir.
> 
> 

Bir ACR düzenlemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-edit-an-access-control-record"></a>Bir erişim denetimi kaydını düzenlemek için
1. Hizmet giriş sayfasında hizmetinizi seçin, hizmet adına çift tıklayın ve ardından **yapılandırma** sekmesi.
2. Erişim denetimi kayıtları Tablo listesinde değiştirmek istediğiniz ACR gelin.
3. Yeni bir ad ve/veya IQN için ACR sağlayın.
4. Tıklatın **kaydetmek** değiştirilmiş ACR kaydetmek için. Sekmeli liste, bu değişikliği yansıtacak şekilde güncelleştirilir.

## <a name="delete-an-access-control-record"></a>Bir erişim denetimi kaydını sil
Kullandığınız **yapılandırma** ACRs silmek için Azure Klasik portalında sayfası. 

> [!NOTE]
> Şu anda kullanımda olmayan ACRs silebilirsiniz. Şu anda kullanılmakta olan bir birimi ile ilişkili bir ACR silmek için önce birim çevrimdışı duruma getirmeniz gerekir.
> 
> 

Bir erişim denetimi kaydını silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-an-access-control-record"></a>Bir erişim denetimi kaydını silmek için
1. Hizmet giriş sayfasında hizmetinizi seçin, hizmet adına çift tıklayın ve ardından **yapılandırma** sekmesi.
2. Erişim denetimi kayıtları (ACRs) Tablo listesinde silmek istediğiniz ACR gelin.
3. Sil simgesini (**x**) için seçtiğiniz ACR aşırı sağ sütunda görüntülenir. Tıklatın **x** ACR silmek için simge.
4. Onayınız istendiğinde tıklatın **Evet** silme işlemine devam etmek için. Sekmeli liste silme yansıtacak şekilde güncelleştirilir.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [StorSimple birimlerini yönetme](storsimple-manage-volumes.md).
* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

