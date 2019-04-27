---
title: StorSimple 8000 serisi cihaz, StorSimple birim kapsayıcıları yönetme | Microsoft Docs
description: Ekleme, değiştirme veya birim kapsayıcısını silme için StorSimple cihaz Yöneticisi hizmeti birim kapsayıcıları sayfası nasıl kullanabileceğinizi açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/19/2017
ms.author: alkohli
ms.openlocfilehash: 7e1a5ac2c2b734c77fc3dbe788206f8c75044953
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60724792"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-storsimple-volume-containers"></a>StorSimple birim kapsayıcıları yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış
Bu öğreticide, oluşturmak ve StorSimple birim kapsayıcıları yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanmayı açıklar.

Microsoft Azure StorSimple cihaz birim kapsayıcısı, depolama hesabı, şifreleme ve bant genişliği tüketimi ayarları paylaşan bir veya daha fazla birimlerini içerir. Birden çok birim kapsayıcıları, birimler için bir cihaz olabilir. 

Birim kapsayıcısı, aşağıdaki özniteliklere sahiptir:

* **Birimleri** – katmanlı veya birim kapsayıcı içinde bulunan StorSimple birimlerini'yerel olarak sabitlenmiş. 
* **Şifreleme** – her bir birim kapsayıcısı için tanımlanabilir bir şifreleme anahtarı. Bu anahtar, StorSimple cihazınızın buluta gönderilen verileri şifrelemek için kullanılır. Askeri düzey AES-256 bit anahtar, kullanıcı tarafından girilen anahtar ile kullanılır. Verilerinizin güvenliğini sağlamak için bulut depolama şifrelemesi her zaman etkinleştirmenizi öneririz.
* **Depolama hesabı** – verileri depolamak için kullanılan Azure depolama hesabı. Bu depolama hesabında birim kapsayıcısı içinde bulunan tüm birimler paylaşın. Varolan bir listeden bir depolama hesabı seçin veya yeni bir hesap oluşturun, birim kapsayıcısı oluşturduğunuzda ve ardından bu hesap için erişim kimlik bilgilerini belirtin.
* **Bulut bant genişliğini** – verileri CİHAZDAN buluta gönderilen cihaz tarafından kullanılan bant genişliğini. Bu kapsayıcı oluşturduğunuzda, 1 MB/sn ile 1000 MB/sn arasında bir değer belirterek bir bant genişliği denetimi zorunlu kılabilir. Cihaz, kullanılabilir tüm bant genişliğini kullanmasını istiyorsanız, bu alan kümesine **sınırsız**. Ayrıca, oluşturabilir ve bant genişliği zamanlamaya göre ayırmak için bir bant genişliği şablonu uygulayın.

Aşağıdaki yordamlarda, StorSimple kullanılacak açıklanmaktadır **birim kapsayıcıları** dikey penceresinde, aşağıdaki yaygın işlemleri tamamlamak için:

* Birim kapsayıcısı Ekle
* Birim kapsayıcısını Değiştir
* Birim kapsayıcısını silme

## <a name="add-a-volume-container"></a>Birim kapsayıcısı Ekle
Birim kapsayıcısı eklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-add-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="modify-a-volume-container"></a>Birim kapsayıcısını Değiştir
Birim kapsayıcısı değiştirmek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-modify-volume-container](../../includes/storsimple-8000-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>Birim kapsayıcısını silme
Birim kapsayıcısı içindeki birimleri içerir. Yalnızca kapsadığı tüm birimler ilk silindiğinde silinebilir. Birim kapsayıcısını silmek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-delete-volume-container](../../includes/storsimple-8000-delete-volume-container.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [StorSimple birimlerini yönetme](storsimple-8000-manage-volumes-u2.md). 
* Daha fazla bilgi edinin [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

