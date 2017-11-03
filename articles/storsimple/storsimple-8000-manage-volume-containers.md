---
title: "StorSimple 8000 serisi cihazda, StorSimple birim kapsayıcıları yönetme | Microsoft Docs"
description: "Ekleme, değiştirme ve bir birim kapsayıcısı silmek için StorSimple cihaz Yöneticisi hizmeti birim kapsayıcıları sayfası nasıl kullanabileceğiniz açıklanır."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/19/2017
ms.author: alkohli
ms.openlocfilehash: 0f8e00d6d07224f56625482f339e612e68914be2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-storsimple-volume-containers"></a>StorSimple birim kapsayıcıları yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış
Bu öğretici StorSimple cihaz Yöneticisi hizmeti oluşturmak ve StorSimple birim kapsayıcıları yönetmek için nasıl kullanılacağını açıklar.

Microsoft Azure StorSimple aygıtta bir birim kapsayıcısı depolama hesabı, şifreleme ve bant genişliği tüketimi ayarları paylaşan bir veya daha fazla birimleri içerir. Bir aygıt tüm birimlerde için birden çok birim kapsayıcıları olabilir. 

Birim kapsayıcısı aşağıdaki özniteliklere sahiptir:

* **Birimleri** – katmanlı veya birim kapsayıcı içinde bulunan StorSimple birimlerini'yerel olarak sabitlenmiş. 
* **Şifreleme** – her birim kapsayıcısı için tanımlanabilen bir şifreleme anahtarı. Bu anahtar StorSimple Cihazınızı buluta gönderilen verileri şifrelemek için kullanılır. Askeri düzeyde AES 256 bit anahtar kullanıcı tarafından girilen anahtarla kullanılır. Verilerinizin güvenliğini sağlamak için bulut depolama şifreleme her zaman etkinleştirmenizi öneririz.
* **Depolama hesabı** – verilerini depolamak için kullanılan Azure depolama hesabı. Birim kapsayıcısı içinde bulunan tüm birimleri bu depolama hesabını paylaşır. Varolan bir listesinden bir depolama hesabını seçin veya birim kapsayıcısı oluşturduğunuzda ve bu hesaba erişim kimlik bilgilerini belirtin yeni bir hesap oluşturun.
* **Bulut bant genişliği** – verileri CİHAZDAN buluta gönderildiğinde aygıt tarafından kullanılan bant genişliği. Bu kapsayıcı oluşturduğunuzda, 1 MB/sn ile 1000 MB/sn arasında bir değer belirterek bant genişliği denetimini uygulayabilirsiniz. Kullanılabilir tüm bant genişliği aygıta istiyorsanız, bu alan kümesi'ne **sınırsız**. Ayrıca, oluşturma ve bant genişliği zamanlamaya göre ayırmak için bant genişliği şablonu uygulayabilirsiniz.

Aşağıdaki yordamlar StorSimple nasıl kullanacağınıza ilişkin **birim kapsayıcıları** dikey aşağıdaki ortak işlemleri tamamlamak için:

* Birim kapsayıcısı Ekle
* Birim kapsayıcısı değiştirme
* Birim kapsayıcısı Sil

## <a name="add-a-volume-container"></a>Birim kapsayıcısı Ekle
Birim kapsayıcısı eklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-add-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="modify-a-volume-container"></a>Birim kapsayıcısı değiştirme
Birim kapsayıcısı değiştirmek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-modify-volume-container](../../includes/storsimple-8000-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>Birim kapsayıcısı Sil
Birim kapsayıcısı içindeki birimi vardır. Yalnızca kapsadığı tüm birimler ilk silindiğinde silinebilir. Birim kapsayıcısı silmek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-delete-volume-container](../../includes/storsimple-8000-delete-volume-container.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [StorSimple birimlerini yönetme](storsimple-8000-manage-volumes-u2.md). 
* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

