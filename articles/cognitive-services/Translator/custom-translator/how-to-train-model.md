---
title: Bir model - özel Translator eğitin
titleSuffix: Azure Cognitive Services
description: Modeli önemli bir çeviri modeline oluştururken bir adımdır. Eğitim için o eğitimleri seçtiğiniz belgelerde göre gerçekleşir.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.date: 02/21/2019
ms.author: v-rada
ms.topic: conceptual
ms.openlocfilehash: eeecb3e21a8a725932884f13919e58247f85dffb
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729143"
---
# <a name="train-a-model"></a>Modeli eğitme

Bir eğitim olmadan modeli oluşturulamıyor çünkü modeli bir çeviri modeli oluşturmak için önemli bir adımdır. Eğitim için eğitimleri seçtiğiniz belgelere göre gerçekleşir.

Bir modeli eğitmek için:

1.  Bir model oluşturmak üzere istediğiniz projeyi seçin.

2.  Proje için veri sekmesi, proje dili çifti için tüm belgelerin gösterilir. Modelinizi eğitmek için kullanmak istediğiniz belgeleri el ile seçin. Eğitim, ayarlama ve bu ekrandan belgeleri test seçebilirsiniz. Ayrıca, yalnızca Eğitim kümesi seçin ve özel ayarı oluşturun ve kümeleri için test Translator sahip.

    -  Belge adı: Belgenin adı.

    -  Eşleştirme: Bu belgede bir paralel veya tek dilli Kinsoku'ya belgesi ise. Tek dilli Kinsoku'ya belgeler, eğitim için şu anda desteklenmemektedir.

    -  Belge türü: Eğitim, ayarlama, test veya sözlük olabilir.

    -  Dil çifti: Bu kaynak ve hedef dil için proje gösterir.

    -  Kaynak cümleler: Kaynak dosyasından ayıklanan cümleler sayısını gösterir.

    -  Hedef cümleler: Cümle hedef dosyasından ayıklanan sayısını gösterir.

    ![Modeli eğitme](media/how-to/how-to-train-model.png)

3.  Eğit düğmesine tıklayın.

4.  İletişim kutusunda, modeliniz için bir ad belirtin.

5.  Modeli eğitme tıklayın.

    ![Train model iletişim kutusu](media/how-to/how-to-train-model-2.png)

6.  Özel Translator eğitim gönderin ve modelleri sekmede eğitim durumunu gösterir.

    ![Train model sayfası](media/how-to/how-to-train-model-3.png)


## <a name="edit-a-model"></a>Bir modeli Düzenle

Model Ayrıntıları sayfasında düzenleme bağlantısını kullanarak bir model düzenleyebilirsiniz.

1.  Kalem simgesine tıklayın.

    ![Modeli Düzenle](media/how-to/how-to-edit-model.png)

2.  İletişim değişikliği

    1.  Model adı (gerekli): Modelinizi anlamlı bir ad verin.

        ![Daha fazla iletişim Düzenle](media/how-to/how-to-edit-model-dialog.png)

3.  Kaydet’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi [modeli ayrıntılarını görüntülemek nasıl](how-to-view-model-details.md).
