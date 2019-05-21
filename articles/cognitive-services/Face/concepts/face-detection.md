---
title: Yüz algılama ve öznitelikleri kavramları
titleSuffix: Azure Cognitive Services
description: Kavramları yüz algılama ve yüz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: PatrickFarley
manager: nitime
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: pafarley
ms.openlocfilehash: e61048eeab9d7061c18f3237db22fc87ca52f526
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65891157"
---
# <a name="face-detection-and-attributes"></a>Yüz algılama ve öznitelikler

Bu makalede, yüz algılama ve yüz özniteliği veri kavramlarını açıklar. Yüz algılama, bir resimdeki İnsan yüzlerini bulma ve isteğe bağlı olarak farklı türlerde yüz tanıma ile ilgili veri döndüren eylemdir.

Kullandığınız [yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) işlemin görüntüdeki yüzleri algılayın. En azından, algılanan her yüz yanıt faceRectangle alanına karşılık gelir. Şu sol, üst, genişlik ve yükseklik piksel koordinat kümesini bulunduğu yüz işaretleyin. Bu koordinatları kullanarak, yüz tanıma ve boyutunu konumunu alabilirsiniz. API yanıtında sırayla boyutu büyükten küçüğe yüzleri listelenir.

## <a name="face-id"></a>Yüz kimliği

Face ID görüntüdeki algılanan her yüz için benzersiz tanımlayıcı dizedir. Bir yüz kimliği isteyebilir, [yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API çağrısı.

## <a name="face-landmarks"></a>Yüz tanıma yer işaretleri

Yüz tanıma yer işareti bir kolayca bulunabilen noktalarında göz bebeklerinin veya burun ucunu gibi bir yüz tanıma, kümesidir. Varsayılan olarak önceden tanımlanmış 27 yer işareti noktası vardır. Tüm 27 noktaları aşağıdaki şekilde gösterilmiştir:

![Etiketli tüm 27 yer işareti ile bir yüz diyagramı](../Images/landmarks.1.jpg)

Noktalarının koordinatları piksel birimlerinde döndürülür.

## <a name="attributes"></a>Öznitelikler

Öznitelikleri tarafından isteğe bağlı olarak algılanabilir özellikler kümesidir [yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API. Aşağıdaki öznitelikler algılanabilir:

* **Yaş**. Belirli yüz yıl cinsinden tahmini yaşını.
* **Bulanıklaştırma**. Resimdeki yüz, bulanıklık. Bu öznitelik bir değer arasında sıfır ve tek ve resmi olmayan bir derecelendirme, düşük, Orta veya yüksek döndürür.
* **Duygu tanıma**. Belirtilen yüz için kendi algılama güvenle duyguları listesi. Güven puanları normalleştirilir ve puanları tüm duyguları arasında en fazla bir tane ekleyin. Döndürülen duyguları olan mutluluk, üzüntü, nötr, kızgınlık, küçümseme, iğrenme, şaşkınlık ve Korkmayın.
* **Etkilenme**. Resimdeki yüz açığa. Bu öznitelik sıfır ve tek ile resmi olmayan bir derecelendirme parazite, goodExposure veya aşırı arasında bir değer döndürür.
* **Bıyığın**. Tahmini bıyığın varlığı ve uzunluğu belirtilen yüz için.
* **Cinsiyet**. Belirtilen yüz tahmini cinsiyeti. Olası değerler şunlardır: erkek, kadın ve genderless.
* **Gözlük**. Belirtilen yüz gözlük olup olmadığı. Olası değerler şunlardır: NoGlasses ReadingGlasses, Güneş gözlüğü ve yüzme Goggles.
* **Saç**. Yüz tanıma artı türü. Bu öznitelik, artı görünür olup, baldness olup olmadığı algılandığında ve hangi artı renkleri algılanan gösterir.
* **HEAD konusunda sizi uyarmayı**. 3B alanda yüzünün yönü. Bu öznitelik, aralık, toplama ve yaw açıların derece cinsinden açıklanmıştır. Değer aralıklarına-90 derece 90 derece, - 180 derece 180 derece ve -90 derece 90 derece, sırasıyla. Açı eşlemeleri için aşağıdaki çizime bakın:

    ![Head aralık ile alma ve etiketli eksenleri yaw](../Images/headpose.1.jpg)
* **Düzenini**. Yüz tanıma düzenini olup olmadığı. Bu öznitelik eyeMakeup ve lipMakeup için bir Boole değeri döndürür.
* **Gürültü**. Görsel gürültü içinde için yüz resmini algıladı. Bu öznitelik bir değer arasında sıfır ve tek ve resmi olmayan bir derecelendirme, düşük, Orta veya yüksek döndürür.
* **Kapatma**. Yüz tanıma engelleme bölümlerini olup olmadığını nesneleri. Bu öznitelik eyeOccluded foreheadOccluded ve mouthOccluded için bir Boole değeri döndürür.
* **Gülümseyin**. Belirtilen yüz gülümseme ifadesi. Bu değer sıfır hiçbir gülümseme Gönder, diğeri de Temizle gülümseme arasındadır.

> [!IMPORTANT]
> Yüz öznitelikleri istatistiksel algoritmalar kullanılarak tahmin. Bunlar her zaman doğru olmayabilir. Öznitelik verilere dayalı kararları verirken dikkatli olun.

## <a name="input-data"></a>Giriş verileri

Giriş görüntülerinizi en doğru algılama sonuçları tanıdığınızdan emin olmak için aşağıdaki ipuçlarını kullanın:

* Desteklenen giriş resim biçimleri, JPEG, PNG, GIF ilk çerçeve ve BMP içindir.
* Resim dosyasının boyutu 4 MB'tan büyük olmamalıdır.
* Algılanabilir yüz boyut aralığı 36 x 36 için 4096 x 4096 pikseldir. Bu aralığın dışında yüzleri algılanmaz.
* Teknik güçlükler nedeniyle bazı yüzleri algılanmayabilen. Algılama, aşırı yüz açıları (baş poz) veya yüz kapatma (nesneler gözlüğü veya yüz parçası block uygulamalı gibi) etkileyebilir. Tamamen çıplak ve neredeyse tamamen yüzleri en iyi sonuçlar verir.

Akış bir video yüz algılama, belirli ayarları, video kamera ayarlayarak performansını doğrulayabilirsiniz:

* **Düzgünleştirme**: Birçok video kamera, yumuşatma bir efekt uygulayın. Bulanıklaştırma kareler arasındaki oluşturur ve açıklık azaltır çünkü mümkünse bunu kapatmalısınız.
* **Perde hızını**: Daha hızlı perde hızı kareleri arasında hareket azaltır ve her karede daha anlaşılır hale getirir. Perde hızları 1 60 saniye veya daha hızlı öneririz.
* **Perde açı**: Bazı kameralar perde açı perde hızı yerine belirtin. Mümkünse, daha düşük bir perde açısı kullanmalısınız. Bu, NET video çerçevelerde neden olur.

    >[!NOTE]
    > Görüntü koyu olacak bir kamera daha düşük bir perde açısı ile daha az ışık her karede alırsınız. Kullanılacak doğru düzeyini belirlemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Yüz algılama kavramlarına alışık olduğunuz, yüzleri algılar belirli bir görüntüyü bir komut dosyası yazmayı öğrenin.

* [Görüntüdeki yüzleri algılayın](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)