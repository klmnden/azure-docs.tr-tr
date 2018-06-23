---
title: Diğer API'lara erişmek için Azure içerik Aracı alanında bağlayıcıları kullanın | Microsoft Docs
description: Bağlayıcıları kullanarak içerik aracı iş akışları için diğer API'lere erişim öğrenin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/22/2017
ms.author: sajagtap
ms.openlocfilehash: d8114457e7079ca8772cab830bd011dcddf372f5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351604"
---
# <a name="connectors"></a>Bağlayıcılar

Azure içerik denetleyici iş akışlarını içerik denetleyici API'leri yanı sıra diğer API'leri kullanabilirsiniz. Diğer API'lara içerik Aracı alanında bir bağlayıcı kullanarak erişin. Bağlayıcı diğer API'leri için bir bağlantı sağlar.

İçerik denetleyici bu varsayılan bağlayıcıları içerir:

* Duygu Tanıma API'si
* Yüz Tanıma API'si
* PhotoDNA bulut hizmeti

![İçerik denetleyici kullanılabilir bağlayıcılar](images/connectors-1.png)

## <a name="verify-your-credentials"></a>Kimlik bilgilerinizi doğrulayın 

Bir iş akışı tanımlamadan önce kullanmak istediğiniz API Bağlayıcısı için geçerli kimlik bilgileri olduğundan emin olun:

1.  Gözden geçirme aracı üzerinde panonun seçin **ayarları** > **Bağlayıcılar**.

  ![İçerik denetleyici select bağlayıcılar](images/connectors-2.png)

2.  Seçin **Düzenle** kimlik bilgilerini doğrulamak istediğiniz bağlayıcıyı yanındaki simge.

  ![İçerik yöneticiyi Düzenle simgesi seçin](images/connectors-3.png)

3.  Abonelik anahtarı görüntülenir. Tüm düzenlemeleri yapın, seçin **kaydetmek** işiniz bittiğinde.

  ![İçerik yöneticiyi Düzenle bağlayıcılar sayfası](images/connectors-4-1.png)
 
## <a name="add-a-connector"></a>Bir bağlayıcı ekleme

1.  Bir bağlayıcı eklemeden önce bir abonelik anahtarı gerekir. Gözden geçirme aracı üzerinde panonun seçin **ayarları** > **kimlik bilgileri**. Seçme ve alanında değer kopyalama **Ocp-Admin-Subscription-Key** kutusu.

2.  Seçin **Bağlayıcılar**. Gözden geçirme aracı Pano görüntülenen kullanılabilir bağlayıcılar birini seçin. Ardından, seçin **Bağlan**. 

  ![İçerik denetleyici Ekle bağlayıcı sayfası](images/connectors-5.png)

3.  İçinde **Ocp-Admin-Subscription-Key** kutusunda, kopyaladığınız anahtarını yapıştırın. Ardından **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Bağlayıcılar kullanmayı öğrenin [özel iş akışları tanımlamak](workflows.md).
