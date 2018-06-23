---
title: Özel görme hizmeti - Azure Bilişsel hizmetler kullanarak, sınıflandırıcı artırmak | Microsoft Docs
description: Özel görme hizmet sınıflandırıcı kalitesini öğrenin.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 65b1424f259066c7d5bd6b2b508d2a4052ff0527
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352871"
---
# <a name="how-to-improve-your-classifier"></a>Sınıflandırıcı iyileştirmek nasıl

Özel görme hizmet sınıflandırıcı kalitesini öğrenin. Sınıflandırıcı kalitesini, kendisine verdiğiniz etiketli veri kalitesi bağımlıdır. 

## <a name="train-more-varied-images"></a>Daha fazla değişken görüntüleri eğitme

Etiketli Görüntü farklı açıları, arka plan, nesne boyutu sağlayarak, fotoğraf grupları ve diğer çeşitleri artırır sınıflandırıcı. Fotoğraf bağlamda fotoğraf nötr arka planlar önünde daha iyi. Ne normal kullanım sırasında sınıflandırıcı gönderilir, temsilcisi görüntüleri içerir.

Görüntüler ekleme hakkında daha fazla bilgi için bkz: [sınıflandırıcı yapı](getting-started-build-a-classifier.md) belge.

> [!IMPORTANT]
> Görüntüleri ekledikten sonra sınıflandırıcı eğitmek unutmayın.

## <a name="use-images-submitted-for-prediction"></a>Tahmin için gönderilen görüntüleri kullanın

Özel görme hizmeti tahmin uç noktasına gönderilen resim depolar. Sınıflandırıcı artırmak için bu görüntüleri kullanmak için aşağıdaki adımları kullanın:

1. Sınıflandırıcı gönderilen görüntüleri görmek için [özel görme web sayfası](https://customvision.ai) seçip __tahminleri__ sekmesi.

    ![Tahminleri sekmesini görüntüsü](./media/getting-started-improving-your-classifier/predictions-tab.png)

    > [!TIP]
    > Varsayılan görünüm geçerli yinelemeye görüntülerden gösterir. Kullanabileceğiniz __yineleme__ önceki yineleme sırasında gönderilen resim görüntülemek için alan aşağı açılır.

2. Sınıflandırıcı tarafından tahmin etiketleri görmek için bir resim üzerine gelin.

    > [!TIP]
    > Görüntüleri sıralanır ve böylece çoğu kazançlar sınıflandırıcı çevrimiçine alabilmeniz için görüntüleri en üstünde. Farklı bir sıralama seçmek için kullanın __sıralama__ bölümü.

    Görüntüyü eğitim verilerinizi eklemek için görüntüyü seçin, etiketi seçin ve ardından __Kaydet ve Kapat__. Görüntü kaldırılır __tahminleri__ ve eğitim görüntülerini eklenebilir. Seçerek görüntüleyebilirsiniz __eğitim görüntüleri__ sekmesi.

    ![Etiketleme sayfasının görüntüsü](./media/getting-started-improving-your-classifier/tag-image.png)

3. Kullanım __tren__ sınıflandırıcı yeniden eğitme düğmesi.

## <a name="visually-inspect-predictions"></a>Tahminleri görsel olarak inceleyin

Görüntü tahminleri incelenecek seçin __eğitim görüntüleri__ sekmesini ve ardından __yineleme geçmişi__. Kırmızı kutu ile özetlenen görüntüleri yanlış tahmin.

![Yineleme geçmişi görüntüsü](./media/getting-started-improving-your-classifier/iteration-history.png)

Bazen visual denetleme ek eğitim verilerini ekleyerek sonra düzeltebilirsiniz desenleri tanımlayabilirsiniz. Örneğin, bir sınıflandırıcı güller daises karşılaştırması için tüm beyaz güller daises etiket yanlış. Ekleme ve beyaz güller etiketli görüntülerini içeren eğitim veri sağlayarak bu sorunu düzeltmek mümkün olabilir.

## <a name="unexpected-classification"></a>Beklenmeyen sınıflandırma

Bazen sınıflandırıcı görüntülerinizi ortak olan özellikleri öğrenir. Örneğin, bir sınıflandırıcı güller tulips karşılaştırması için oluşturmak istediğiniz. Görüntüleri tulips alanları ve kırmızı Vazo mavi duvar önünde güller sağlayın. Bu verileri verildiğinde, sınıflandırıcı alanı duvar + tulips karşılaştırması güller yerine Vazo karşılaştırması için eğitmek.

Bu sorunu düzeltmek için daha fazla ile eğitim kılavuzunu kullanın görüntüleri değişmesi: görüntüleri farklı açıları, arka plan, nesne boyutu, grupları ve diğer çeşitleri sağlama.

## <a name="negative-image-handling"></a>Negatif resim işleme

Özel görme hizmeti bazı otomatik negatif resim işleme destekler. Cat köpek sınıflandırıcı karşılaştırması oluşturmakta olduğunuz ve tahmin için ayakkabı görüntüsünü göndermek, sınıflandırıcı kat hem köpek olarak, görüntü %0 yakın puan. 

> [!WARNING]
> Otomatik yaklaşımı açıkça negatif görüntüler için çalışır. İyi negatif görüntüler yalnızca eğitim kullanılan görüntülerin çeşitlemesi olduğu durumlarda çalışmayabilir. 
>
> Örneğin, bir husky corgi sınıflandırıcı karşılaştırması varsa ve bir Pomeranian görüntüdeki akış, onu Pomeranian Husky puan. Negatif görüntülerinizi bu yapısını varsa (örneğin "diğer") yeni bir etiket oluşturmak ve negatif eğitim görüntülere uygulamak.

## <a name="next-steps"></a>Sonraki adımlar

[Tahmin API kullanın](use-prediction-api.md)