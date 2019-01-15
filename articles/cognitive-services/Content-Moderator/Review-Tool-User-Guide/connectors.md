---
title: İçerik - Content Moderator'ı yönetme çalışırken diğer hizmetlere bağlanma
titlesuffix: Azure Cognitive Services
description: Bağlayıcıları kullanarak diğer API'lere, Content Moderator iş akışları için öğrenin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 99d8b3603278a9c6c432ca32a1d85e9abe34e1da
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54265592"
---
# <a name="connect-to-other-cognitive-services"></a>Bilişsel diğer hizmetlere bağlanma

Azure Content Moderator iş akışları, Content Moderator API'sine yanı sıra diğer API'leri kullanabilirsiniz. Diğer API'ler, bağlayıcı Content Moderator ile erişin. Bağlayıcı bir API için bir bağlantı sağlar.

Content Moderator, bu varsayılan bağlayıcıları içerir:

* Duygu Tanıma API'si
* Yüz Tanıma API'si
* PhotoDNA bulut hizmeti

![Content Moderator kullanılabilir bağlayıcılar](images/connectors-1.png)

## <a name="verify-your-credentials"></a>Kimlik bilgilerinizi doğrulayın 

Bir iş akışı tanımlamadan önce kullanmak istediğiniz API Bağlayıcısı için geçerli kimlik bilgilerine sahip olduğunuzdan emin olun:

1.  Gözden geçirme aracı üzerinde Pano seçin **ayarları** > **Bağlayıcılar**.

  ![Content Moderator select bağlayıcılar](images/connectors-2.png)

2.  Seçin **Düzenle** kimlik bilgilerini doğrulamak istediğiniz bağlayıcıyı yanındaki simge.

  ![Content Moderator düzenleme simgesini seçin](images/connectors-3.png)

3.  Abonelik anahtarı görüntülenir. Tüm düzenlemeleri yapın, seçin **Kaydet** işiniz bittiğinde.

  ![Content Moderator Düzenle bağlayıcılar sayfasında](images/connectors-4-1.png)
 
## <a name="add-a-connector"></a>Bağlayıcı Ekle

1.  Bir bağlayıcı eklemeden önce bir abonelik anahtarı gerekir. Gözden geçirme aracı üzerinde Pano seçin **ayarları** > **kimlik bilgilerini**. Seçin ve içinde değerini kopyalayın **Ocp-Admin-Subscription-Key** kutusu.

2.  Seçin **Bağlayıcılar**. İnceleme aracını Panosu üzerinde görüntülenen bağlayıcıları birini seçin. Ardından, **Connect**. 

  ![Content Moderator Ekle bağlayıcı sayfası](images/connectors-5.png)

3.  İçinde **Ocp-Admin-Subscription-Key** kutusunda, kopyaladığınız anahtarını yapıştırın. Ardından **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Bağlayıcılar için kullanmayı öğrenin [özel iş akışlarınızı tanımlamanızı](workflows.md).
