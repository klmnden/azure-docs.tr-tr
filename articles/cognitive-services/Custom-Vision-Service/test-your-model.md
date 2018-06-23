---
title: Test ve model - özel görme Service - Azure Bilişsel hizmetler yeniden eğitme | Microsoft Docs
description: Görüntüyü test ve model yeniden eğitme için kullanmak hakkında bilgi edinin.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 1933b1a45844ac99308baebe59b49687a957abfa
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353279"
---
# <a name="test-and-retrain-a-model-with-custom-vision-service"></a>Test ve özel görme hizmeti ile bir model yeniden eğitme

Modelinizi eğitmek sonra hızlı bir şekilde çevrimiçi görüntü veya yerel olarak saklanan bir görüntüsünü kullanarak test edebilirsiniz. Test en son eğitilen yineleme kullanır.

## <a name="test-your-model"></a>Modelinizi test etme

1. Gelen [özel görme web sayfası](https://customvision.ai), projenizi seçin. Seçin **hızlı Test** üst menü çubuğunun sağında. Bu eylem etiketli bir pencere açılır **hızlı Test**.

    ![Hızlı sına düğmesine pencerenin sağ üst köşesinde görüntülenir.](./media/test-your-model/quick-test-button.png)

2. İçinde **hızlı Test** penceresinde tıklatın **gönderme görüntü** alan ve test için kullanmak istediğiniz görüntü URL'sini girin. Bunun yerine yerel olarak saklanan bir görüntüsünü kullanmak istiyorsanız, tıklatın **göz atın, yerel dosya** düğmesini tıklatın ve bir yerel görüntü dosyası seçin.

    ![Gönderme görüntü sayfasının görüntüsü](./media/test-your-model/submit-image.png)

Seçtiğiniz görüntü sayfasının ortasında görünür. Etiketli iki sütun içeren bir tablo biçiminde görüntüsünün altındaki sonuçlar görüntülenerek sonra **etiketleri** ve **güvenirlik**. Sonuçları görüntüledikten sonra kapatılabilir **hızlı Test** penceresi.

Şimdi bu test görüntüsü modelinize eklemek ve modelinizi yeniden eğitme.

## <a name="use-the-predicted-image-for-training"></a>Tahmin edilen görüntüyü eğitim için kullanın.

Eğitim için daha önce gönderdiğiniz görüntüsü kullanmak için aşağıdaki adımları kullanın:

1. Sınıflandırıcı gönderilen görüntüleri görmek için [özel görme web sayfası](https://customvision.ai) seçip __tahminleri__ sekmesi.

    ![Tahminleri sekmesini görüntüsü](./media/test-your-model/predictions-tab.png)

    > [!TIP]
    > Varsayılan görünüm geçerli yinelemeye görüntülerden gösterir. Kullanabileceğiniz __yineleme__ önceki yineleme sırasında gönderilen resim görüntülemek için alan aşağı açılır.

2. Sınıflandırıcı tarafından tahmin etiketleri görmek için bir resim üzerine gelin.

    > [!TIP]
    > Görüntüleri sıralanır ve böylece çoğu kazançlar sınıflandırıcı çevrimiçine alabilmeniz için görüntüleri en üstünde. Farklı bir sıralama seçmek için kullanın __sıralama__ bölümü.

    Görüntüyü eğitim verilerinizi eklemek için görüntüyü seçin, etiketi seçin ve ardından __Kaydet ve Kapat__. Görüntü kaldırılır __tahminleri__ ve eğitim görüntülerini eklenebilir. Seçerek görüntüleyebilirsiniz __eğitim görüntüleri__ sekmesi.

    ![Etiketleme sayfasının görüntüsü](./media/test-your-model/tag-image.png)

3. Kullanım __tren__ sınıflandırıcı yeniden eğitme düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

[Sınıflandırıcı geliştirmek](getting-started-improving-your-classifier.md)