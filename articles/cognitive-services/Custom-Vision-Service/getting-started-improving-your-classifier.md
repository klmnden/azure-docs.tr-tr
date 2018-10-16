---
title: Sınıflandırıcınızı - özel görüntü işleme hizmeti geliştirme
titlesuffix: Azure Cognitive Services
description: Sınıflandırıcınızı kalitesini öğrenin.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 07/05/2018
ms.author: pafarley
ms.openlocfilehash: 2bee7f0af98bf03a13e376dea9dbf083b3f61815
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49340299"
---
# <a name="how-to-improve-your-classifier"></a>Nasıl sınıflandırıcınızı geliştirme

Custom Vision Service'e sınıflandırıcınızı kalitesini öğrenin. Sınıflandırıcınızı kalitesini tutarı, kalite ve daha ve nasıl dengeli veri kümesi için sağladığınız etiketli veri çeşitli bağlıdır. İyi bir sınıflandırıcı normalde bir sınıflandırıcı gönderilir, temsilcisi bir dengeli bir eğitim veri kümesi vardır. Böyle bir sınıflandırıcı oluşturma işlemi genellikle yinelemelidir. Beklenen sonuçlara ulaşmak eğitim, birkaç yuvarlar yararlanmak için yaygındır.

Sınıflandırıcı geliştirmek için genel adımlar aşağıdadır. Bu adımları, sabit ve hızlı kuralları, ancak daha iyi bir sınıflandırıcı oluşturmanıza yardımcı olacak bir buluşsal yöntem değildir.

1. İlk hepsini eğitim
1. Daha fazla görüntü eklemek ve veri Bakiye
1. Yeniden eğitme
1. Değişen arka plan, aydınlatma, nesne boyutu, kamera açısını ve stil ile görüntü ekleme
1. Yeniden eğitme & görüntüde tahmin için akış
1. Tahmin sonuçlarını İnceleme
1. Mevcut eğitim verileri değiştirme

## <a name="data-quantity-and-data-balance"></a>Veri miktarı ve verileri bakiyesi

Sınıflandırıcı eğitmek için yeterli görüntüleri karşıya yüklemek için gereken en önemli şey var. Etiket için Eğitim kümesi başına en az 50 görüntüleri bir başlangıç noktası olarak kullanılması önerilir. Daha az görüntülerle overfitting bir güçlü riski yoktur. Kaliteli performans numaralarınızı önerebilir, ancak gerçek dünya verileri karşı uğraşıyor. Daha fazla görüntü ile bir sınıflandırıcı eğitim genellikle tahmin sonuçlarının doğruluğu artırır.

Verilerinizi dengelenir emin olmalısınız başka bir husustur. Örneği için bir etiket için 500 görüntüler ve başka bir etiket için 50 görüntüleri sahip bir etiket diğerinden tahmin etmeye yönelik daha doğru olacak şekilde modeli neden bir imbalanced eğitim veri kümesi oluşturur. Çoğu görüntülerle az görüntülerle etiketi ve etiket arasında en az bir 1:2 oranını korumak, daha iyi sonuçlar görmeniz olasıdır. Görüntüleri en yüksek sayıda etiketle 500 görüntü varsa, örneğin, en az görüntülerle etiket eğitimi için en az 250 görüntüleri olmalıdır.

## <a name="train-more-diverse-images"></a>Daha fazla farklı görüntüleri eğitin

Temsilcisi, normal kullanım sırasında ne bir sınıflandırıcı gönderilir, görüntüleri sağlar. Örneğin, "apple" sınıflandırıcı eğitim, sınıflandırıcınızı kalıplarını ancak elma ağaçları üzerinde fotoğraf üzerinde yapma tahminler elma fotoğraflarını yalnızca eğitimle doğru olmayabilir. Çeşitli görüntüleri dahil olmak üzere sınıflandırıcınızı ağırlıklı olmayan ve iyi genelleştirebilirsiniz sağlayacaktır. Aşağıda, eğitim yapabileceğiniz bazı açılardan daha çeşitli ayarlanır:

__Arka planı:__ farklı arka plan (diğer bir deyişle, Market Torba içinde Meyve karşı kalıbı üzerinde Meyve) önündeki nesne görüntüleri sağlar. Fotoğraf bağlamında daha fazla bilgi için sınıflandırıcı sağladıkları gibi bağımsız arka planlar önüne fotoğraflar'den daha iyidir.

![Arka plan örnekleri görüntüsü](./media/getting-started-improving-your-classifier/background.png)

__Aydınlatma:__ çeşitli aydınlatma ile görüntüleri sağlar (diğer bir deyişle, flash, yüksek ifşasını gerçekleştirilen, vb.), özellikle tahmin için kullanılan görüntülerin farklı ışık varsa. Değiştirilen Doygunluk ve hue parlaklık ile görüntüleri dahil etmek yararlıdır.

![Aydınlatma örnekleri görüntüsü](./media/getting-started-improving-your-classifier/lighting.png)

__Nesne boyutu:__ sağlama görüntüleri nesneler olduğu, değiştirilen nesnesinin farklı bölümlerini yakalama boyutlandırma. Örneğin, fotoğrafını bananas ve tek Muz sağında bunches. Farklı boyutlandırma daha iyi generalize sınıflandırıcı yardımcı olur.

![Görüntü boyutu örnekleri](./media/getting-started-improving-your-classifier/size.png)

__Kamera Açısı:__ görüntüleri farklı Kamera Açısı ile yapılan sağlayın. Tüm fotoğraflarınızı, (örneğin, ekleme işlemlerini kaydedecek) sabit kameralar kümesiyle alınır, bunlar overfitting - ilgisiz nesne (örneğin, lampposts) bir anahtar özellik olarak modelleme önlemek için aynı nesnelerini yakalama olsa bile her kamera için farklı bir etiket atamak emin olun.

![Açı örnekleri görüntüsü](./media/getting-started-improving-your-classifier/angle.png)

__Stil:__ aynı sınıfı (diğer bir deyişle, farklı türde citrus) farklı türlerde görüntülerini sağlayın. Ancak, önemli ölçüde farklı stilleri (diğer bir deyişle, gerçek zamanlı konuşmaların rat karşı Mickey fare) nesneleri görüntüler varsa, bunların farklı özellikleri bunları etiketlemek iyi temsil etmek için ayrı sınıflar olarak önerilir.

![Stil örnekleri görüntüsü](./media/getting-started-improving-your-classifier/style.png)

## <a name="use-images-submitted-for-prediction"></a>Tahmin için gönderilen görüntüleri kullanın

Custom Vision Service'e tahmin uç noktasına gönderilen görüntülerini depolar. Sınıflandırıcı geliştirmek için bu görüntüleri kullanmak için aşağıdaki adımları kullanın:

1. Bir sınıflandırıcı gönderilen görüntüleri görmek için [Custom Vision web sayfası](https://customvision.ai), projenize gidin ve seçin __Öngörüler__ sekmesi. Geçerli yineleme görüntülerden varsayılan görünümünü gösterir. Kullanabileceğiniz __yineleme__ alan sırasında önceki yinelemelerin gönderilen görüntüleri görüntülemek için aşağı açılır.

    ![Öngörüler sekmesinin resmi](./media/getting-started-improving-your-classifier/predictions.png)

2. Sınıflandırıcı tarafından tahmin etiketlerini görmek için görüntüyü üzerine gelin. Görüntüleri bir sınıflandırıcı çoğu kazanç getiren görüntüleri en üstünde olacak şekilde sıralanır. Farklı bir sıralama seçmek için kullanın __sıralama__ bölümü. Mevcut eğitim verilerinizi bir görüntü eklemek için görüntüyü seçin, doğru etiketi seçin ve tıklayın __Kaydet ve Kapat__. Görüntü kaldırılacak __Öngörüler__ ve eğitim görüntülerin eklenir. Seçerek görüntüleyebilirsiniz __eğitim resmi__ sekmesi.

    ![Etiketleme sayfasının görüntüsü](./media/getting-started-improving-your-classifier/tag.png)

3. Kullanım __eğitme__ sınıflandırıcı yeniden eğitme düğmesini.

## <a name="visually-inspect-predictions"></a>Öngörüler görsel olarak inceleyin

Görüntü Öngörüler incelemek için seçin __eğitim resmi__ sekmesini seçip __yineleme geçmişi__. Kırmızı kutu ile ana hatlarıyla özetlenen görüntüleri yanlış tahmin.

![Yineleme geçmişi görüntüsü](./media/getting-started-improving-your-classifier/iteration.png)

Bazen görsel denetim ardından ek bir eğitim veri eklemek veya mevcut eğitim verilerini değiştirerek düzeltebilirsiniz desenleri tanımlayabilirsiniz. Örneğin, bir sınıflandırıcı apple Küf karşılaştırması için tüm yeşil elma limes etiket yanlış. Ekleme ve yeşil elma etiketlenmiş resimleri içeren bir eğitim veri sağlayarak bu sorunu çözmek mümkün olabilir.

## <a name="unexpected-classification"></a>Beklenmeyen sınıflandırma

Bazen sınıflandırıcı yanlış görüntülerinizi ortak olan özellikleri öğrenir. Örneğin, elma citrus karşılaştırması için sınıflandırıcı oluşturma ve uygulamalı olarak elma ve Limonlu beyaz kalıplarını içinde sağlanan görüntüleri, sınıflandırıcı uygulamalı citrus karşılaştırması elma yerine beyaz kalıplarını karşılaştırması için eğitim.

![Beklenmeyen sınıflandırma görüntüsü](./media/getting-started-improving-your-classifier/unexpected.png)

Bu sorunu düzeltmek için yukarıdaki yönergeleri ile daha fazla eğitim kullanın görüntüleri değiştirilen: farklı açıları, arka plan, nesne boyutu, grupları ve diğer çeşitleri ile görüntüleri sağlar.

## <a name="negative-image-handling"></a>Negatif bir görüntü işleme

Custom Vision Service'e bazı otomatik negatif görüntü işlemeyi destekler. Burada, bir üzüm Muz sınıflandırıcı karşılaştırması oluşturmaya ve tahmin için bir ayakkabı görüntüsü göndermek durumunda Sınıflandırıcısı olarak bu görüntüyü %0 yakın üzüm ve Muz için puan.

Öte yandan, negatif görüntüler yalnızca eğitim kullanılan görüntülerin çeşitlemesi olduğu durumlarda, model negatif görüntüleri harika benzerlikler nedeniyle etiketli bir sınıf olarak sınıflandırır olasılığı yüksektir. Örneğin, turuncu grapefruit sınıflandırıcı karşılaştırması sahip ve bir clementine görüntüdeki akışı, onu clementine turuncu puan. Clementine özelliklerinin çoğu bu yana oluşabilir (renk, şekil, doku, habitat'ın doğal, vb.), portakallar benzer.  Negatif görüntülerinizi bu doğasını ise oluşturmak için önerilen veya daha fazla etiket ("diğer") ayırın ve bu sınıflar arasında daha iyi ayırt etmek model izin vermek eğitim sırasında negatif görüntüler bu etikete sahip etiket.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl görüntüleri programlı olarak tahmin API'ye göndererek test edebilirsiniz öğrenin.

> [!div class="nextstepaction"]
[Tahmin API'sini kullanma](use-prediction-api.md)
