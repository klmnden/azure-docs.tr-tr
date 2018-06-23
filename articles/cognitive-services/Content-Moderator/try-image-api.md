---
title: Azure içerik denetleyici görüntülerle Orta | Microsoft Docs
description: Görüntü yönetimini içerik denetleyici API Konsolu'nda dediğini.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: fec54826c70ae10e56c68406f629c56639985295
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351659"
---
# <a name="moderate-images-from-the-api-console"></a>API konsolundan Orta görüntüleri

Kullanım [Görüntü Yönetimi API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c) görüntü içeriği için tarama ve İnceleme Yönetimi iş akışlarını başlatmak için Azure içeriği denetleyicinin içinde. Denetleme işi içeriğiniz uygunsuz metin için tarar ve özel ve paylaşılan blacklists karşı karşılaştırır.

## <a name="use-the-api-console"></a>API Konsolu
Çevrimiçi konsolunda API dediğini önce abonelik anahtarınızı gerekir. Bu bulunan **ayarları** sekmesinde **Apim abonelik anahtar Ocp** kutusu. Daha fazla bilgi için bkz: [genel bakış](overview.md).

1.  Git [Görüntü Yönetimi API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c).

  **Görüntü - değerlendirmek** görüntü yönetimini sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin. 

  ![Görüntü deneyin - sayfası bölge seçimi değerlendir](images/test-drive-region.png)
  
  **Görüntü - değerlendirmek** API konsolu açılır.

3. İçinde **Apim abonelik anahtar Ocp** kutusuna, abonelik anahtarınızı girin.

  ![Görüntü deneyin - konsol abonelik anahtarı değerlendir](images/try-image-api-1.PNG)

4. İçinde **istek gövdesinde** kutusunda, varsayılan örnek görüntüsünü kullanabilir veya tarama için bir görüntüsünü belirtin. İkili olarak görüntünün kendisini gönderebilirsiniz bit veri ya da bir görüntü için genel olarak erişilebilir bir URL belirtin. 

  Bu örnekte, sağlanan yolu kullanmak **istek gövdesinde** kutusuna ve ardından **Gönder**. 

   ![Görüntü deneyin - konsol istek gövdesi değerlendirme](images/try-image-api-2.PNG)

  Bu URL'de görüntüdür:

  ![Görüntü deneyin - Konsolu örnek resmi değerlendir](images/sample-image.jpg) 

5. **Gönder**’i seçin.

6. API her sınıflandırması için bir olasılık puan döndürür. Ayrıca, görüntü koşullar karşılayıp belirleme döndürür (**true** veya **false**). 

  ![Görüntü deneyin - konsol olasılık puanı değerlendirme ve koşul belirleme](images/try-image-api-3.PNG)

## <a name="face-detection"></a>Yüz algılama

Görüntü Yönetimi API yüzeyleri görüntüdeki bulmak için kullanabilirsiniz. Gizlilik sorunları sahip olduğunuzda ve belirli bir yazıtipi platformunuz üzerinde yayınlanan engellemek istediğinizde bu seçeneği yararlı olabilir. 

1.  İçinde [Görüntü Yönetimi API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c), soldaki menüde altında **görüntü**seçin **Bul bakarken**. 

  **Image - bulma bakarken** sayfası açılır.

2.  İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin. 

  ![Görüntü deneyin - yüzeyleri sayfa bölge seçimi Bul](images/test-drive-region.png)

  **Image - bulma bakarken** API konsolu açılır.

3. Tarama için bir resim belirtin. İkili olarak görüntünün kendisini gönderebilirsiniz bit veri ya da bir görüntü için genel olarak erişilebilir bir URL belirtin. Bu örnek bağlantılar görüntüye CNN yazıdaki kullanılır.

  ![Görüntü deneyin - yüzeyleri örnek görüntüsü bulamadı](images/try-image-api-face-image.jpg)

  ![Görüntü deneyin - yüzeyleri örnek istek Bul](images/try-image-api-face-request.png)

4. **Gönder**’i seçin. Bu örnekte, API iki yüz bulur ve bunların koordinatları görüntüde döndürür.

   ![Görüntü deneyin - yüzeyleri örnek yanıt içerik kutusu Bul](images/try-image-api-face-response.png)

## <a name="text-detection-via-ocr-capability"></a>OCR özelliği yoluyla metin algılama

Metin görüntülerinde algılamak için içerik denetleyici OCR özelliği kullanabilirsiniz.

1. İçinde [Görüntü Yönetimi API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c), soldaki menüde altında **görüntü**seçin **OCR**. 

  **Image - OCR** sayfası açılır.

2. İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin. 

  ![Görüntü - OCR sayfa bölge seçimi](images/test-drive-region.png)

  **Image - OCR** API konsolu açılır.

3. İçinde **Apim abonelik anahtar Ocp** kutusuna, abonelik anahtarınızı girin.

4. İçinde **istek gövdesinde** kutusunda, varsayılan örnek görüntüsünü kullanabilir. Bu önceki bölümde kullanılan aynı görüntüdür.

5. **Gönder**’i seçin. Ayıklanan metin JSON'de görüntülenir:

  ![Görüntü - OCR örnek yanıt içerik kutusu](images/try-image-api-ocr.PNG)

## <a name="next-steps"></a>Sonraki adımlar

REST API kodunuzdaki kullanın veya başlayın [görüntü yönetimi .NET quickstart](image-moderation-quickstart-dotnet.md) uygulamanızla tümleştirmek için.
