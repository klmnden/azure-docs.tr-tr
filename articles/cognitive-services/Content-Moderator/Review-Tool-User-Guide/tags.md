---
title: İçerik denetleme - Content Moderator özel etiketler kullanma
titlesuffix: Azure Cognitive Services
description: Content Moderator, varsayılan etiketler içerir ve işletmenize özel içeriği yönetme için özel etiketler oluşturabilirsiniz.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 942b30ab9949a28c42949913d90e7448c22b658f
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55206291"
---
# <a name="create-and-use-moderation-tags"></a>Oluşturma ve denetleme etiketleri kullanma

İki varsayılan etiketleri yanı sıra **isadult** (**bir**) ve **isracy** (**r**), daha fazla tarama hedeflenen için özel etiketler oluşturabilirsiniz. Bu özel etiketler daha sonra görüntü veya metin atamak İnsan gözden geçirenler için kullanılabilir.

## <a name="create-tags"></a>Etiket oluşturma

1.  Etiketleri ayarlar sekmesinden seçin.

  ![İçerik denetleme etiketleri](images/tags-1.png)

2.  Etiket için iki harfli kısa bir kod girin.
3.  Etiket için bir ad girin. Kısa ve açıklayıcı adı tutun. Örneğin, **isbullying**.
4.  Bir açıklama girin.
5.  Ekle'ye tıklayın.
6.  Oluşturma etiketleri tamamladığınızda, Kaydet'e tıklayın.

![İçerik denetleme etiketlerini tanımlamaya](images/tags-2-define.png)

## <a name="using-custom-tags"></a>Özel etiketler kullanma

Özel etiketler, insan tarafından İnceleme sırasında kullanılır. Bunlar Preview'de görüntüleme ve İnceleme tıklayarak seçer.

![İçerik denetleme etiketleri kullanarak](images/tags-3-use.png)

Farklı incelemeleri, farklı etiket denetimi veya görünür işaretini devre dışı bırakabilirsiniz.
 
![İçerik denetleme etiketleri devre dışı bırakma](images/tags-4-disable.png)

İki varsayılan etiketleri silemezsiniz ancak **isadult** ve **isracy**, tanımladığınız herhangi bir özel etiket silebilirsiniz. Silmek istediğiniz etiketi yanındaki Çöp Kutusu'e tıklayın.

![İçerik denetleme etiketleri silme](images/tags-5-delete.png)

## <a name="next-steps"></a>Sonraki adımlar

Görüntü denetimi için etiketleri kullanma konusunda bilgi almak için bkz: [gözden geçirme aracılı görüntüleri](Review-Moderated-Images.md).
