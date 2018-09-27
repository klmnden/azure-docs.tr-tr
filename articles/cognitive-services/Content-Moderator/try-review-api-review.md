---
title: API Konsolu - Content Moderator incelemelere kullanarak içeriği Orta
titlesuffix: Azure Cognitive Services
description: Content Moderator API'si konsolda incelemelere oluşturmayı öğrenin.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: bb95341a09f09ce8020f34476e720270fd401909
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219762"
---
# <a name="create-reviews-from-the-api-console"></a>API Konsolu incelemeleri oluşturma

Gözden geçirme API'nin kullanın [işlemleri gözden](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4) görüntü veya metin incelemeleri insan tarafından denetim için oluşturulacak. İnsan Moderatörler gözden geçirme aracı içeriği gözden geçirmek için kullanın. Bu işlem sonrası denetimi iş mantığınıza tabanlı kullanın. Content Moderator resim, metin API'leri veya diğer Bilişsel hizmetler API'lerini kullanarak içeriğinizi taradıktan sonra bunu kullanın. 

İnsan moderatör otomatik olarak atanan etiketleri ve tahmin verilerini gözden geçirmeleri ve son denetimi karar gönderdikten sonra gözden geçirme API, API uç noktanıza tüm bilgileri gönderir.

## <a name="use-the-api-console"></a>API Konsolu
API çevrimiçi Konsolu aracılığıyla dediğini konsoluna girmek için bazı değerler gerekir:

- **teamName**: gözden geçirme aracı hesabınızı oluşturduğunuz takım adı. 
- **ContentID**: Bu dize API için geçirilen ve geri döndürdü. ContentID iç tanımlayıcılar veya meta veri denetimi iş sonuçları ile ilişkilendirmek için kullanışlıdır.
- **Meta veri**: geri çağırma sırasındaki özel anahtar-değer çiftleri döndürülen API uç noktanıza. Anahtar gözden geçirme Aracı'nda tanımlanan kısa bir kod varsa, bir etiket olarak görüntülenir.
- **Ocp-Apim-Subscription-Key**: bulunan **ayarları** sekmesi. Daha fazla bilgi için [genel bakış](overview.md).

Bir test konsoluna erişmek için en basit yolu **kimlik bilgilerini** penceresi.

1.  İçinde **kimlik bilgilerini** penceresinde [gözden geçirme API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4).

  **Gözden geçirin - oluşturma** sayfası açılır.

2.  İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklayan bölgeyi seçin.

  ![Gözden geçirme - kayıt seçimi sayfası oluşturma](images/test-drive-region.png)

  **Gözden geçirin - oluşturma** API konsolu açılır.
  
3.  Gerekli sorgu parametreleri, içerik türü ve abonelik anahtarınız için değerleri girin. İçinde **istek gövdesi** kutusunda, (örneğin, görüntü konumu) içerik, meta verileri ve içerikle ilişkili diğer bilgileri belirtin.

  ![Gözden geçirme - konsol sorgu parametreleri, üst bilgileri ve istek gövdesi kutusu oluşturma](images/test-drive-review-1.PNG)
  
4.  **Gönder**’i seçin. Gözden geçirme kimliği oluşturulur. Aşağıdaki adımlarda kullanmak için bu kimliği kopyalayın.

  ![Gözden geçirme - Konsolu yanıt içerik kutusu İnceleme Kimliğini görüntüler oluşturma](images/test-drive-review-2.PNG)
  
5.  Seçin **alma**ve API bölgenizi eşleşen düğmesini seçerek açın. Sonuçta elde edilen sayfasında değerlerini girin **teamName**, **ReviewID**, ve **abonelik anahtarı**. Seçin **Gönder** sayfasında düğme. 

  ![Gözden geçirme - Get sonuçları konsol oluşturun](images/test-drive-review-3.PNG)
  
6.  Tarama sonuçlarını görürsünüz.

  ![Gözden geçirme - konsol yanıt içerik kutusu oluşturma](images/test-drive-review-4.PNG)
  
7.  Content Moderator Panoda seçin **gözden geçirme** > **görüntü**. Taranan görüntü görünür, insan tarafından İnceleme için hazır.

  ![Gözden geçirme aracı bir futbol Top görüntüsü](images/test-drive-review-5.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzda REST API kullanma veya ile başlayan [incelemeleri .NET Hızlı Başlangıç](moderation-reviews-quickstart-dotnet.md) uygulamanızla tümleştirmek için.
