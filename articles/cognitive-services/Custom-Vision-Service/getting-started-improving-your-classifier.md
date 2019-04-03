---
title: Sınıflandırıcınızı - özel görüntü işleme hizmeti geliştirme
titlesuffix: Azure Cognitive Services
description: Sınıflandırıcınızı kalitesini öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: pafarley
ms.openlocfilehash: 35f83832b0ceb7507b39095e9cc974d82a480c69
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58883082"
---
# <a name="how-to-improve-your-classifier"></a>Nasıl sınıflandırıcınızı geliştirme

Bu kılavuzda Custom Vision Service'e sınıflandırıcınızı kalitesini öğreneceksiniz. Sınıflandırıcınızı kalitesini tutar, kalite ve ve nasıl dengeli genel bir veri kümesidir sağladığınız etiketli veri çeşitli bağlıdır. İyi bir sınıflandırıcı temsilcisi, bir sınıflandırıcı gönderilecek bir dengeli bir eğitim veri kümesi vardır. Böyle bir sınıflandırıcı oluşturma yinelemeli bir işlemdir; beklenen sonuçlara ulaşmak eğitim, birkaç yuvarlar yararlanmak için yaygındır.

Daha doğru bir sınıflandırıcı oluşturmanıza yardımcı olmak için bir genel düzen aşağıdaki gibidir:

1. İlk hepsini eğitim
1. Daha fazla görüntü ve Bakiye veri ekleyin; yeniden eğitme
1. Değişen arka plan, aydınlatma, nesne boyutu, kamera açısını ve stil görüntülerle; ekleyin. yeniden eğitme
1. Tahmin test etmek için yeni yansımaları kullanın
1. Mevcut eğitim verileri tahmin sonuçlarını göre Değiştir

## <a name="prevent-overfitting"></a>Overfitting engelle

Bazı durumlarda, bir sınıflandırıcı görüntülerinizi ortak olan rastgele özelliklerine göre tahminlerde bulunmayı öğreneceksiniz. Örneğin, bir sınıflandırıcı elma citrus karşılaştırması için oluşturduğunuz ve uygulamalı olarak elma ve Limonlu görüntüleri üzerinde beyaz kalıplarını kullandınız, elma citrus karşılaştırması yerine kalıplarını ve uygulamalı aşırı önem sınıflandırıcı verebilir.

![Beklenmeyen sınıflandırma görüntüsü](./media/getting-started-improving-your-classifier/unexpected.png)

Bu sorunu düzeltmek için daha fazla eğitimlerle aşağıdaki yönergeleri kullanın görüntüleri değiştirilen: farklı açıları, arka plan, nesne boyutu, grupları ve diğer Çeşitlemeler ile görüntüleri sağlar.

## <a name="data-quantity"></a>Veri miktarı

Eğitim görüntülerinin sayısını en önemli bir faktördür. Etiket başına en az 50 görüntüleri bir başlangıç noktası olarak kullanmanızı öneririz. Daha az görüntülerle overfitting daha yüksek riski yoktur ve kaliteli performans numaralarınızı önerebilir olsa da modelinizi gerçek verilerle uğraşıyor. 

## <a name="data-balance"></a>Veri bakiyesi

Eğitim verilerinizi göreli miktarlarını göz önünde bulundurmanız önemlidir. Örneğin, bir etiket ve başka bir etiket için 50 görüntüleri için 500 görüntüleri kullanarak bir imbalanced eğitim veri kümesi için yapar. Bu, bir etiket diğerinden tahmin etmeye yönelik daha doğru olacak şekilde modeli neden olur. Çoğu görüntülerle az görüntülerle etiketi ve etiket arasında en az bir 1:2 oranını korumak, daha iyi sonuçlar görmeniz olasıdır. Örneğin, çoğu görüntüleri etiketle 500 görüntü varsa, en az görüntülerle etiket eğitimi için en az 250 görüntüleri olmalıdır.

## <a name="data-variety"></a>Veri çeşitli

Temsilcisi, normal kullanım sırasında ne bir sınıflandırıcı gönderilir, görüntüleri kullanmayı unutmayın. Aksi takdirde, sınıflandırıcınızı görüntülerinizi ortak olan rastgele özelliklerine göre tahminlerde bulunmayı öğrenin. Örneğin, bir sınıflandırıcı elma citrus karşılaştırması için oluşturduğunuz ve uygulamalı olarak elma ve Limonlu görüntüleri üzerinde beyaz kalıplarını kullandınız, elma citrus karşılaştırması yerine kalıplarını ve uygulamalı aşırı önem sınıflandırıcı verebilir.

![Beklenmeyen sınıflandırma görüntüsü](./media/getting-started-improving-your-classifier/unexpected.png)

Bu sorunu düzeltmek için resimler, sınıflandırıcınızı iyi genelleştirebilirsiniz emin olmak için çeşitli yer almaktadır. Aşağıda, eğitim yapabileceğiniz bazı açılardan daha çeşitli ayarlanır:

* __Arka planı:__ Nesnenizin önünde farklı arka plan görüntüleri sağlar. Fotoğraf doğal bağlamlarda daha fazla bilgi için sınıflandırıcı sağladıkları gibi bağımsız arka planlar önüne fotoğraflar'den daha iyidir.

    ![Arka plan örnekleri görüntüsü](./media/getting-started-improving-your-classifier/background.png)

* __Aydınlatma:__ Değiştirilen aydınlatma ile (flash, yüksek ifşasını alınma) ve benzeri görüntüleri sağlar, özellikle tahmin için kullanılan görüntülerin farklı ışık varsa. Değişen doygunluğu, hue ve parlaklık görüntüleri kullanmak yararlıdır.

    ![Aydınlatma örnekleri görüntüsü](./media/getting-started-improving-your-classifier/lighting.png)

* __Nesne boyutu:__ Hangi nesnelerin farklı boyut ve sayısının görüntüleri sağlayın (örneğin, fotoğrafını bananas ve tek Muz sağında bunches). Farklı boyutlandırma daha iyi generalize sınıflandırıcı yardımcı olur.

    ![Görüntü boyutu örnekleri](./media/getting-started-improving-your-classifier/size.png)

* __Kamera Açısı:__ Görüntüleri farklı Kamera Açısı ile yapılan sağlayın. Tüm fotoğraflarınızı, sabit kamera (örneğin, ekleme işlemlerini kaydedecek) ile atılmalıdır alternatif olarak, farklı bir etiket overfitting önlemek için her düzenli aralıklarla gerçekleşen nesneye atamak mutlaka&mdash;ilgisiz nesne (örneğin, lampposts) yorumlama anahtar özellik.

    ![Açı örnekleri görüntüsü](./media/getting-started-improving-your-classifier/angle.png)

* __Stili:__ Aynı sınıfın (örneğin, farklı çeşitleri aynı Meyve) farklı stillerde görüntülerini sağlar. Ancak, önemli ölçüde farklı stilleri (örneğin, gerçek zamanlı konuşmaların fare karşılaştırması Mickey fare) nesneler varsa, bunların farklı özellikleri daha iyi ayrı sınıfları temsil etiket öneririz.

    ![Stil örnekleri görüntüsü](./media/getting-started-improving-your-classifier/style.png)

## <a name="negative-images"></a>Negatif görüntüler

Projenizdeki belirli bir noktada eklemeniz gerekebilir _negatif örnekleri_ sınıflandırıcınızı daha doğru hale getirilmesine yardımcı olacak. Negatif örnekleri diğer etiketlerden herhangi birini eşleşmeyen olanlardır. Bu görüntüleri karşıya yüklediğinizde, özel uygulama **negatif** için bunları etiketleyin.

> [!NOTE]
> Custom Vision Service'e bazı otomatik negatif görüntü işlemeyi destekler. Bir üzüm Muz sınıflandırıcı karşılaştırması oluşturmaya ve tahmin için bir ayakkabı görüntüsü göndermek, örneğin, sınıflandırıcı %0 yakın olarak bu görüntüyü üzüm ve Muz için Puanlama.
> 
> Öte yandan, negatif görüntüler yalnızca eğitim kullanılan görüntülerin çeşitlemesi olduğu durumlarda, model negatif görüntüleri harika benzerlikler nedeniyle etiketli bir sınıf olarak sınıflandırır olasılığı yüksektir. Clementine özelliklerinin çoğu bu portakallar benzer çünkü gibi turuncu grapefruit sınıflandırıcı karşılaştırması sahip ve bir clementine görüntüdeki akışı, onu clementine turuncu puan. Negatif görüntülerinizi bu yapısı varsa, bir veya daha fazla ek etiketler oluşturma öneririz (gibi **diğer**) ve bu sınıflar arasında daha iyi ayırt etmek model izin vermek eğitim sırasında negatif görüntüler bu etikete sahip etiket .

## <a name="use-prediction-images-for-further-training"></a>Eğitim için daha fazla öngörü görüntülerini kullanma

Kullandığınızda veya görüntü sınıflandırıcı tahmin uç noktasına görüntüleri göndererek test özel görüntü işleme hizmeti, bu görüntüleri depolar. Daha sonra bunları modeli geliştirmek için kullanabilirsiniz.

1. Bir sınıflandırıcı gönderilen görüntüleri görmek için [Custom Vision web sayfası](https://customvision.ai), projenize gidin ve seçin __Öngörüler__ sekmesi. Geçerli yineleme görüntülerden varsayılan görünümünü gösterir. Kullanabileceğiniz __yineleme__ menü sırasında önceki yinelemelerin gönderilen görüntüleri görüntülemek için aşağı açılır.

    ![tahminlerin ekran görüntüsü sekmesi ile görüntüleri görüntüleme](./media/getting-started-improving-your-classifier/predictions.png)

2. Sınıflandırıcı tarafından tahmin etiketlerini görmek için görüntüyü üzerine gelin. Görüntüleri, böylece en sınıflandırıcı geliştirmeleri getirebilirsiniz olanları listelenen sıralanır üst. Farklı bir sıralama yöntemi kullanmak için bir seçim yapın. __sıralama__ bölümü. 

    Mevcut eğitim verilerinizi bir görüntü eklemek için görüntüyü seçin, doğru etiket ayarlama ve tıklayın __Kaydet ve Kapat__. Görüntü kaldırılacak __Öngörüler__ ve eğitim resmi kümesine eklenir. Seçerek görüntüleyebilirsiniz __eğitim resmi__ sekmesi.

    ![Etiketleme sayfasının görüntüsü](./media/getting-started-improving-your-classifier/tag.png)

3. Ardından __eğitme__ sınıflandırıcı yeniden eğitme düğmesini.

## <a name="visually-inspect-predictions"></a>Öngörüler görsel olarak inceleyin

Görüntü Öngörüler incelemek için Git __eğitim resmi__ sekmesinde, önceki eğitim yinelemede seçin **yineleme** açılan menüsünde ve bir veya daha fazla etiket altında **etiketleri** bölümü. Görünüm modeli belirtilen etiketi doğru şekilde tahmin başarısız görüntülerin her çevresinde artık görüntülemelidir.

![Yineleme geçmişi görüntüsü](./media/getting-started-improving-your-classifier/iteration.png)

Bazen görsel denetim sonra daha fazla eğitim verilerini ekleyerek veya mevcut eğitim verilerini değiştirerek düzeltebilirsiniz desenleri tanımlayabilirsiniz. Örneğin, bir sınıflandırıcı elma limes karşılaştırması için tüm yeşil elma limes etiket yanlış. Ardından, ekleme ve yeşil elma etiketlenmiş resimleri içeren bir eğitim veri sağlayarak bu sorunu düzeltebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, özel görüntü sınıflandırma modelini daha doğru hale getirmek amacıyla çeşitli teknikler hakkında bilgi edindiniz. Ardından, programlı olarak tahmin API'ye göndererek test görüntülerimizden öğrenin.

> [!div class="nextstepaction"]
> [Tahmin API’sini kullanma](use-prediction-api.md)
