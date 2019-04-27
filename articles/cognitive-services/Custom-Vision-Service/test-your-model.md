---
title: Test edin ve bir modeli - özel görüntü işleme hizmeti yeniden eğitme
titlesuffix: Azure Cognitive Services
description: Bir görüntüyü test etme ve modeli yeniden kullanma hakkında bilgi edinin.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: anroth
ms.openlocfilehash: d516cee81aef66ec58399cb5ff23c89db16bf2ab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60816634"
---
# <a name="test-and-retrain-a-model-with-custom-vision-service"></a>Özel görüntü işleme hizmeti ile bir modeli yeniden eğitme ve test

Modelinizi eğitin sonra hızlıca yerel olarak depolanan bir resmi veya çevrimiçi bir görüntü kullanarak test edebilirsiniz. Test en yakın zamanda eğitilen yineleme kullanır.

## <a name="test-your-model"></a>Modelinizi test etme

1. Gelen [Custom Vision web sayfası](https://customvision.ai), projenizi seçin. Seçin **hızlı Test** sağ üst menü çubuğu. Bu eylem etiketli bir pencere açılır **hızlı Test**.

    ![Hızlı Test Et düğmesine pencerenin sağ üst köşesinde görüntülenir.](./media/test-your-model/quick-test-button.png)

2. İçinde **hızlı Test** penceresinde tıklatın **görüntü gönderme** alan ve test için kullanmak istediğiniz görüntünün URL'sini girin. Bunun yerine yerel olarak depolanan bir resmi kullanmak istiyorsanız, tıklayın **yerel dosyalara Gözat** düğmesine tıklayın ve yerel bir resim dosyası seçin.

    ![Gönder görüntüsü sayfasının görüntüsü](./media/test-your-model/submit-image.png)

Seçtiğiniz görüntüye sayfanın ortasında görünür. Ardından sonuçları, aşağıdaki görüntüde etiketli iki sütunlu bir tablo biçiminde görünür **etiketleri** ve **güvenle**. Sonuçları görüntüledikten sonra kapanabiliyor **hızlı Test** penceresi.

Artık bu test görüntüsü eklersiniz ve sonra da modelinizi yeniden eğitme.

## <a name="use-the-predicted-image-for-training"></a>Eğitim için tahmin edilen görüntü kullanma

Eğitim için daha önce gönderilen görüntüyü kullanmak için aşağıdaki adımları kullanın:

1. Bir sınıflandırıcı gönderilen görüntüleri görmek için [Custom Vision web sayfası](https://customvision.ai) seçip __Öngörüler__ sekmesi.

    ![Öngörüler sekmesinin resmi](./media/test-your-model/predictions-tab.png)

    > [!TIP]
    > Geçerli yineleme görüntülerden varsayılan görünümünü gösterir. Kullanabileceğiniz __yineleme__ alan sırasında önceki yinelemelerin gönderilen görüntüleri görüntülemek için aşağı açılır.

2. Sınıflandırıcı tarafından tahmin etiketlerini görmek için görüntüyü üzerine gelin.

    > [!TIP]
    > Görüntüleri bir sınıflandırıcı çoğu kazanç getiren görüntüleri en üstünde olacak şekilde sıralanır. Farklı bir sıralama seçmek için kullanın __sıralama__ bölümü.

    Eğitim verilerinizi resim eklemek için görüntüyü seçin, etiketi seçin ve ardından __Kaydet ve Kapat__. Görüntü kaldırılır __Öngörüler__ ve eğitim görüntülerin eklenir. Seçerek görüntüleyebilirsiniz __eğitim resmi__ sekmesi.

    ![Etiketleme sayfasının görüntüsü](./media/test-your-model/tag-image.png)

3. Kullanım __eğitme__ sınıflandırıcı yeniden eğitme düğmesini.

## <a name="next-steps"></a>Sonraki adımlar

[Sınıflandırıcınızı geliştirme](getting-started-improving-your-classifier.md)
