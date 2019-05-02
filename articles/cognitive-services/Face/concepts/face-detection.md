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
ms.openlocfilehash: 3293067190386738efbfeb433bd38453c9e5699f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2019
ms.locfileid: "64572543"
---
# <a name="face-detection-and-attributes"></a>Yüz algılama ve öznitelikler

Bu makalede, yüz algılama ve yüz özniteliği veri kavramlarını açıklar. Yüz algılama, bir resimdeki İnsan yüzlerini bulma ve isteğe bağlı olarak çeşitli yüz tanıma ile ilgili veriler döndüren eylemdir.

Kullandığınız [yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) işlemin görüntüdeki yüzleri algılayın. En azından her yüz için karşılık gelen algıladı. bir **faceRectangle** yanıt kodu. (Sol, üst düzey, genişlik, yükseklik) piksel koordinatları kümesini budur bulunduğu yüz işaretleme. Bu koordinatları kullanarak boyutunun yanı sıra yüz konumu elde edebilirsiniz. API yanıtında sırayla boyutu büyükten küçüğe yüzleri listelenir.

## <a name="face-id"></a>Yüz kimliği

Yüz tanıma, yalnızca bir benzersiz tanımlayıcı dizesi görüntüdeki algılanan her yüz için kimliğidir. Bir yüz kimliği isteyebilir, [yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API çağrısı.

## <a name="face-landmarks"></a>Yüz tanıma yer işaretleri

Yüz tanıma yer işareti göz bebeklerinin gibi bir yüz noktalarında Bul kolay bir dizi veya burun ucunu ' dir. Varsayılan olarak önceden tanımlanmış 27 yer işareti noktası vardır. Tüm 27 noktaları aşağıdaki şekilde gösterilmiştir:

![Etiketli tüm 27 yer işareti ile bir yüz diyagramı](../Images/landmarks.1.jpg)

Noktalarının koordinatları piksel birimlerinde döndürülür.

## <a name="attributes"></a>Öznitelikler

Öznitelikleri tarafından isteğe bağlı olarak algılanabilir ek yüz özellikleri kümesidir [yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API. Algılanabilir öznitelikleri şunlardır:

* **Yaş** belirli yüz bir yıl cinsinden tahmini yaş.
* **Bulanıklaştırma** resimdeki yüz, bulanıklık. Sıfır ve tek yanı sıra arasında resmi olmayan bir derecelendirme ("Düşük", "Orta", "Yüksek") bir değer döndürür.
* **Etkilenme** resimdeki yüz açığa. Sıfır ve tek yanı sıra arasında bir resmi olmayan bir derecelendirme ("parazite", "goodExposure", "aşırı") bir değer döndürür.
* **Duygu tanıma** duyguları belirli yüz için kendi algılama güvenle listesi. Güven puanları normalleştirilmiş: puanları tüm duyguları arasında en fazla bir ekleyeceksiniz. Döndürülen duyguları olan mutluluk, üzüntü, nötr, kızgınlık, küçümseme, iğrenme, şaşkınlık ve Korkmayın.
* **Bıyığın** tahmini bıyığın varlığı ve belirli yüz tanıma, uzunluğu.
* **Cinsiyet** belirli yüz tahmini dinleyicilerinin. Olası değerler şunlardır: "erkek", "kadın" ve "genderless".
* **Gözlük** belirli yüz gözlük sahip olup olmadığını belirler. Olası değerler şunlardır: "NoGlasses", "ReadingGlasses", "Gözlüğü" ve "Yüzme Goggles".
* **Saç** yüzü artı stili. Bu, artı görünür olup, baldness olup olmadığı algılandığında ve hangi artı renkleri algılandı gösterir.
* **HEAD konusunda sizi uyarmayı** 3B alanda yüzünün yönü. Bu aralık, toplama ve yaw açıların derece cinsinden açıklanmaktadır. Değer aralıkları [-90, 90] olan [-180, 180] ve [-90, 90] derece sırasıyla. Açı eşlemeleri için aşağıdaki çizime bakın:

    ![Head aralık ile alma ve etiketli eksenleri yaw](../Images/headpose.1.jpg)
* **Düzenini** yüzü düzenini sahip olup olmadığını belirler. Bu, "eyeMakeup" ve "lipMakeup" için bir Boole değeri döndürür.
* **Gürültü** visual gürültü içinde için yüz resmini algıladı. Sıfır ve tek yanı sıra arasında resmi olmayan bir derecelendirme ("Düşük", "Orta", "Yüksek") bir değer döndürür.
* **Kapatma** yüzü bölümlerini engelleme nesneleri olup olmadığı. Bu, "eyeOccluded", "foreheadOccluded" ve "mouthOccluded" için bir Boole değeri döndürür.
* **Gülümseyin** belirli yüz gülümseme ifadesi. Bu, sıfır (hiçbir gülümseme Gönder) ve diğeri (NET gülümseme Gönder) arasında bir değerdir.

> [!IMPORTANT]
> Yüz öznitelikleri istatistiksel algoritmalar kullanılarak öngörülen ve her zaman doğru olmayabilir. Öznitelik verilere dayalı kararları verirken dikkatli olun.

## <a name="input-data"></a>Giriş verileri

En doğru algılama sonuçları giriş görüntülerinizi vermek emin olmak için aşağıdaki ipuçlarını kullanın:

* Desteklenen giriş görüntü biçimleri, JPEG, PNG, GIF (ilk çerçeve), BMP olacaktır.
* Resim dosyasının boyutu 4 MB'tan büyük olmamalıdır.
* Algılanabilir yüz boyut aralığı 36 x 36 için 4096 x 4096 pikseldir. Bu aralığın dışında yüzleri algılanmaz.
* Teknik güçlükler nedeniyle bazı yüzleri algılanamayabilir. Algılama, aşırı yüz açıları (baş poz) veya yüz kapatma (nesneler gözlüğü veya bire bir yüzü parçası engelleme gibi) etkileyebilir. Tamamen çıplak ve neredeyse tamamen yüzleri en iyi sonuçlar verir.

## <a name="next-steps"></a>Sonraki adımlar

Yüz algılama kavramlarına alışık olduğunuz, yüzleri algılar belirli bir görüntüde basit bir komut dosyası yazmayı öğrenin.

* [Nasıl bir görüntüde yüz algılama](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)