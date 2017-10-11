---
title: "StorSimple birim kapsayıcıları yönetme | Microsoft Docs"
description: "Ekleme, değiştirme ve bir birim kapsayıcısı silmek için StorSimple Yöneticisi hizmeti birim kapsayıcıları sayfası nasıl kullanabileceğiniz açıklanır."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 1c64ce75-1fd3-4d3b-9304-d4dc0fc2b069
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: bb55a7a4bff0fd4319de6f6ce958686ad8a4142b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-storsimple-volume-containers"></a>StorSimple birim kapsayıcıları yönetmek için StorSimple Yöneticisi hizmetini kullanma
## <a name="overview"></a>Genel Bakış
Bu öğretici oluşturmak ve StorSimple birim kapsayıcıları yönetmek için StorSimple Yöneticisi hizmetini kullanmayı açıklar.

Microsoft Azure StorSimple aygıtta bir birim kapsayıcısı depolama hesabı, şifreleme ve bant genişliği tüketimi ayarları paylaşan bir veya daha fazla birimleri içerir. Bir aygıt tüm birimlerde için birden çok birim kapsayıcıları olabilir. 

Birim kapsayıcısı aşağıdaki özniteliklere sahiptir:

* **Birimleri** – katmanlı veya birim kapsayıcı içinde bulunan StorSimple birimlerini'yerel olarak sabitlenmiş. Birim kapsayıcısı en fazla 256 StorSimple birimlerini içerebilir.
* **Şifreleme** – her birim kapsayıcısı için tanımlanabilen bir şifreleme anahtarı. Bu anahtar StorSimple Cihazınızı buluta gönderilen verileri şifrelemek için kullanılır. Askeri düzeyde AES 256 bit anahtar kullanıcı tarafından girilen anahtarla kullanılır. Verilerinizin güvenliğini sağlamak için bulut depolama şifreleme her zaman etkinleştirmenizi öneririz.
* **Depolama hesabı** – bulut depolama hizmet sağlayıcınıza bağlı depolama hesabı. Birim kapsayıcısı içinde bulunan tüm birimleri bu depolama hesabını paylaşır. Varolan bir listesinden bir depolama hesabını seçin veya birim kapsayıcısı oluşturduğunuzda ve bu hesaba erişim kimlik bilgilerini belirtin yeni bir hesap oluşturun.
* **Bulut bant genişliği** – verileri CİHAZDAN buluta gönderildiğinde aygıt tarafından kullanılan bant genişliği. Bu kapsayıcı tanımlarken 1 ile 1000 MB/sn arasında bir değer belirterek bir bant genişliği denetimini uygulayabilirsiniz. Tüm kullanılabilir bant genişliği aygıta istiyorsanız, bu alan için sınırsız ayarlayın. Ayrıca, oluşturma ve bant genişliği zamanlamaya göre ayırmak için bant genişliği şablonu uygulayabilirsiniz.

![Birim kapsayıcıları sayfası](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

Bu aşağıdaki yordamları StorSimple nasıl kullanacağınıza ilişkin **birim kapsayıcıları** sayfasını aşağıdaki ortak işlemleri tamamlamak için:

* Birim kapsayıcısı Ekle 
* Birim kapsayıcısı değiştirme 
* Birim kapsayıcısı Sil 

## <a name="add-a-volume-container"></a>Birim kapsayıcısı Ekle
Birim kapsayıcısı eklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]

## <a name="modify-a-volume-container"></a>Birim kapsayıcısı değiştirme
Birim kapsayıcısı değiştirmek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>Birim kapsayıcısı Sil
Birim kapsayıcısı içindeki birimi vardır. Yalnızca kapsadığı tüm birimler ilk silindiğinde silinebilir. Birim kapsayıcısı silmek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [StorSimple birimlerini yönetme](storsimple-manage-volumes.md). 
* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

