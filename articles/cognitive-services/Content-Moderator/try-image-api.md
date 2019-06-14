---
title: API Konsolu - Content Moderator ile orta görüntüleri
titlesuffix: Azure Cognitive Services
description: Görüntü Denetim API'si, Azure Content Moderator görüntü içeriği için tarama ve gözden geçirme denetimi iş akışlarını başlatmak için kullanın.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 1e4efa5e06525194bfdc7d1932fcfec5ec9f8c6b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60607354"
---
# <a name="moderate-images-from-the-api-console"></a>Orta görüntülerden API Konsolu

Kullanım [görüntü denetim API'si](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c) Azure Content Moderator, görüntü içeriği için tarama ve gözden geçirme denetimi iş akışlarını başlatmak için de. Denetimi işi içeriğinizi küfür tarar ve özel ve paylaşılan kara karşı karşılaştırır.

## <a name="use-the-api-console"></a>API Konsolu
Çevrimiçi konsolunda API'yi test sürüşü önce abonelik anahtarınızı gerekir. Bu dosya çubuğunda bulunur **ayarları** sekmesinde **Ocp-Apim-Subscription-Key** kutusu. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

1. Git [görüntü denetim API'si başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c).

   **Görüntü - değerlendirme** görüntü denetimi sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

   ![Görüntü deneyin - sayfası kayıt seçimi değerlendir](images/test-drive-region.png)
  
   **Görüntü - değerlendirme** API konsolu açılır.

3. İçinde **Ocp-Apim-Subscription-Key** kutusuna, abonelik anahtarınızı girin.

   ![Görüntü deneyin - konsol abonelik anahtarı değerlendir](images/try-image-api-1.PNG)

4. İçinde **istek gövdesi** kutusunda, varsayılan örnek görüntüsünü kullanabilir veya taramak için bir görüntü belirtirsiniz. İkili görüntünün kendisi gönderdiğiniz veri bit ya da bir görüntü için genel olarak erişilebilir bir URL belirtin. 

   Bu örnekte, sağlanan yolu kullanmak **istek gövdesi** kutusuna ve ardından **Gönder**. 

   ![Görüntü deneyin - konsol istek gövdesi değerlendirme](images/try-image-api-2.PNG)

   Bu URL'de bir görüntü.

   ![Görüntü deneyin - Konsolu örnek resmi değerlendir](images/sample-image.jpg) 

5. **Gönder**’i seçin.

6. API, her sınıflandırma için bir olasılık puanı döndürür. Ayrıca, görüntünün koşulları karşılayıp bir belirleme döndürür (**true** veya **false**). 

   ![Görüntü deneyin - konsol olasılık puanı değerlendirmek ve koşul belirleme](images/try-image-api-3.PNG)

## <a name="face-detection"></a>Yüz algılama

Görüntü Denetim API'si bir resimdeki yüz bulmak için kullanabilirsiniz. Gizlilik sorunları olan ve belirli bir yüzün platformunuzdaki gönderilen engellemek istiyorsunuz, bu seçenek yararlı olabilir. 

1. İçinde [görüntü denetim API'si başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c), soldaki menüde altında **görüntü**seçin **yüzleri bulun**. 

   **Image - bulma yüzler** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

   ![Görüntü deneyin - yüzleri sayfa bölge seçimi Bul](images/test-drive-region.png)

   **Image - bulma yüzler** API konsolu açılır.

3. Taramak için bir görüntü belirtirsiniz. İkili görüntünün kendisi gönderdiğiniz veri bit ya da bir görüntü için genel olarak erişilebilir bir URL belirtin. Bu örnek bağlantıları görüntüye CNN yazıdaki kullanılır.

   ![Görüntü deneyin - yüzleri örnek görüntüsü bulunamadı](images/try-image-api-face-image.jpg)

   ![Görüntü deneyin - örnek istek yüzleri bulun](images/try-image-api-face-request.png)

4. **Gönder**’i seçin. Bu örnekte, API iki yüzün bulur ve görüntüde onların koordinatlarını döndürür.

   ![Görüntü deneyin - örnek yanıt içerik kutusu yüzleri bulun](images/try-image-api-face-response.png)

## <a name="text-detection-via-ocr-capability"></a>OCR özelliği aracılığıyla metin algılama

Resimlerde metin algılamak için Content Moderator OCR özelliği'ni kullanabilirsiniz.

1. İçinde [görüntü denetim API'si başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c), soldaki menüde altında **görüntü**seçin **OCR**. 

   **Image - OCR** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin. 

   ![Görüntü - OCR sayfası kayıt seçimi](images/test-drive-region.png)

   **Image - OCR** API konsolu açılır.

3. İçinde **Ocp-Apim-Subscription-Key** kutusuna, abonelik anahtarınızı girin.

4. İçinde **istek gövdesi** kutusunda, varsayılan örnek görüntüsünü kullanın. Önceki bölümde kullanılan aynı görüntüsüdür.

5. **Gönder**’i seçin. Ayıklanan metin, JSON biçiminde görüntülenir:

   ![Görüntü - OCR örnek yanıt içerik kutusu](images/try-image-api-ocr.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzda REST API kullanma veya ile başlayan [görüntü denetimi .NET hızlı](image-moderation-quickstart-dotnet.md) uygulamanızla tümleştirmek için.
