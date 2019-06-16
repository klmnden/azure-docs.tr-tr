---
title: Projeniz için 3.0 geçirme API
titlesuffix: Azure Cognitive Services
description: Custom Vision projeleri API önceki sürümünden 3. 0'geçirmeyi öğrenin API.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: areddish
ms.openlocfilehash: 9dd473aadd7123cafc27209f5c34322fdbcffb71
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60816465"
---
# <a name="migrate-to-the-30-api"></a>3\. 0'geçirme API

Özel görüntü işleme artık genel kullanım sınırına ulaştı ve bir API'yi güncelleştirme gerçekleştirdi.
Bu güncelleştirme birkaç yeni özellik ve daha önemlisi, birkaç önemli değişiklikler içerir:

* Tahmin API artık içinde iki proje türüne göre bölünür.
* Belirli bir şekilde bir proje oluşturmak için işleme yapay ZEKA Geliştirme Seti (VAIDK) dışarı aktarma seçeneği gerekir.
* Varsayılan yineleme yerine Yayımla kaldırılmış / adlandırılmış bir yineleme Yayımdan Kaldır.

Bu kılavuz, projelerinize yeni API sürümüyle çalışacak şekilde güncelleştirme konusunda gösterilmektedir. Bkz: [sürüm notları](release-notes.md) değişikliklerin tam listesi için.

## <a name="use-the-updated-prediction-api"></a>Güncelleştirilmiş tahmin API'sini kullanma

API 2.x aynı tahmin çağrısı resim sınıflandırıcıları ve nesne algılayıcısı projeleri için kullanılır. Her iki proje türleri için kabul edilebilir **PredictImage** ve **PredictImageUrl** çağırır. Çağrı proje türüyle eşleşmesi gerekir böylece 3.0 ile başlayarak, biz bu API böldüyseniz:

* Kullanım **[ClassifyImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c15)** ve **[ClassifyImageUrl](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c14)** görüntü sınıflandırma projeleri için Öngörüler edinmek için.
* Kullanım **[DetectImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c19)** ve **[DetectImageUrl](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c18)** nesne algılama projeleri için Öngörüler edinmek için.

## <a name="use-the-new-iteration-publishing-workflow"></a>Yeni yineleme yayımlama iş akışını kullanın

API 2.x tahmin için kullanılacak yinelemeyi seçmek için varsayılan yineleme veya belirtilen yineleme kimliği kullanılır. 3\. 0'dan başlayarak ilk yapabildiği eğitim API'sinden belirtilen bir adla yineleme yayımlama yayımlama akış benimsemiştir. Daha sonra kullanmak için hangi yineleme belirtmek için tahmin yöntemleri adı geçirin.

> [!IMPORTANT]
> 3\.0 API'leri varsayılan yineleme özelliğini kullanmayın. Biz daha eski API'lar kullanımdan kadar API 2.x varsayılan olarak bir yineleme geçiş yapmak için kullanmaya devam edebilirsiniz. Bu API'ler, bir süre için korunur ve çağırabilirsiniz **[UpdateIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c771cdcbf6a2b18a0c3b818)** yineleme varsayılan olarak işaretlemek için yöntemi.

### <a name="publish-an-iteration"></a>Bir yineleme yayımlama

Bir yineleme eğitildi sonra bunu kullanarak tahmin için kullanılabilir duruma getirebilirsiniz **[PublishIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c82db28bf6a2b11a8247bbc)** yöntemi. Bir yineleme yayımlama CustomVision Web sitesinin Ayarları sayfasında kullanılabilir tahmin kaynak kimliği gerekir.

![Özel görüntü işleme Web Sitesi Ayarları sayfası özetlenen tahmin kaynak Kimliğine sahip.](./media/update-application-to-3.0-sdk/prediction-id.png)

> [!TIP]
> Bu bilgi de edinebilirsiniz [Azure portalı](https://portal.azure.com) Custom Vision tahmin kaynağa ve seçerek **özellikleri**.

Yineleme yayımlandıktan sonra uygulamaları bu tahmin için kendi tahmin API çağrısı içinde adı belirterek kullanabilirsiniz. Bir yineleme tahmin çağrısı için kullanılabilir yapmak için **[UnpublishIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c771cdcbf6a2b18a0c3b81a)** API.

## <a name="additional-export-options"></a>Ek dışarı aktarma seçenekleri

3\.0 ile biz gösterme iki ek API'ler dışarı hedefleri: ARM mimarisi ve işleme yapay ZEKA Geliştirme Seti.

* ARM kullanılacak sıkıştırılmış bir etki alanı seçin ve DockerFile'ı seçin ve ardından dışarı aktarma seçenekleri ARM yeterlidir.
* İşleme yapay ZEKA Geliştirme Seti için proje ile oluşturulmalıdır __genel (CD)__ etki alanı hem de hedef VAIDK belirterek platformlar bağımsız değişken dışarı aktarma.

## <a name="next-steps"></a>Sonraki adımlar

* [Eğitim API başvuru belgeleri (REST)](https://go.microsoft.com/fwlink/?linkid=865446)
* [Tahmin API başvuru belgeleri (REST)](https://go.microsoft.com/fwlink/?linkid=865445)