---
title: Azure Content Moderator etiketleri kullanarak | Microsoft Docs
description: Content Moderator, varsayılan etiketler içerir ve işletmenize özel içeriği yönetme için özel etiketler oluşturabilirsiniz.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: c462ff2937453f942db7fdd5b751f3356b6fe715
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2018
ms.locfileid: "49310088"
---
# <a name="about-tags"></a>Etiketler hakkında #

İki varsayılan etiketleri yanı sıra **isadult** (**bir**) ve **isracy** (**r**), daha fazla tarama hedeflenen için özel etiketler oluşturabilirsiniz. Bu özel etiketler daha sonra görüntü veya metin atamak İnsan gözden geçirenler için kullanılabilir.

## <a name="create-tags"></a>Etiketleri oluşturma ##

1.  Etiketleri ayarlar sekmesinden seçin.

  ![İçerik denetleme etiketleri](images/tags-1.png)

2.  Etiket için iki harfli kısa bir kod girin.
3.  Etiket için bir ad girin. Kısa ve açıklayıcı adı tutun. Örneğin, **isbullying**.
4.  Bir açıklama girin.
5.  Ekle'ye tıklayın.
6.  Oluşturma etiketleri tamamladığınızda, Kaydet'e tıklayın.

![İçerik denetleme etiketlerini tanımlamaya](images/tags-2-define.png)

## <a name="using-custom-tags"></a>Özel etiketler kullanma ##

Özel etiketler, insan tarafından İnceleme sırasında kullanılır. Bunlar Preview'de görüntüleme ve İnceleme tıklayarak seçer.

![İçerik denetleme etiketleri kullanarak](images/tags-3-use.png)

Farklı incelemeleri, farklı etiket denetimi veya görünür işaretini devre dışı bırakabilirsiniz.
 
![İçerik denetleme etiketleri devre dışı bırakma](images/tags-4-disable.png)

İki varsayılan etiketleri silemezsiniz ancak **isadult** ve **isracy**, tanımladığınız herhangi bir özel etiket silebilirsiniz. Silmek istediğiniz etiketi yanındaki Çöp Kutusu'e tıklayın.

![İçerik denetleme etiketleri silme](images/tags-5-delete.png)

## <a name="next-steps"></a>Sonraki adımlar ##

Görüntü denetimi için etiketleri kullanma konusunda bilgi almak için bkz: [gözden geçirme aracılı görüntüleri](Review-Moderated-Images.md).
