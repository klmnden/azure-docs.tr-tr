---
title: Azure içerik denetleyici İnsan incelemeler kullanarak içerik Orta | Microsoft Docs
description: İçerik denetleyici API konsolunda İnsan incelemeler oluşturmayı öğrenin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: e9faf595e65ba4475a743e4cb45919fd30fbd6e8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351580"
---
# <a name="create-reviews-from-the-api-console"></a>Gözden geçirmeler API Konsolu oluşturma

Gözden geçirme API'nin kullanmak [işlemleri gözden](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4) görüntü veya metin incelemeler İnsan denetleme için oluşturmak için. İnsan araburucu kullanmak içeriği gözden geçirmek için İnceleme aracı. Bu işlem, sonrası yönetimini iş mantığına göre kullanın. İçerik denetleyici görüntü veya metin API'leri ya da diğer kavrama Hizmetleri API'lerini kullanarak içeriğinizi taradıktan sonra kullanın. 

İnsan denetleyici otomatik atanan etiketleri ve öngörü verileri gözden geçirir ve son denetleme karar gönderdikten sonra gözden geçirme API, API uç tüm bilgileri gönderir.

## <a name="use-the-api-console"></a>API Konsolu
Çevrimiçi konsolunu kullanarak API dediğini konsola girmek için birkaç değerleri gerekir:

- **teamName**: gözden geçirme aracı hesabınızı oluşturduğunuz ekip adı. 
- **ContentID**: Bu dize API için geçirilen ve geri döndürüldü. ContentID iç tanımlayıcıları veya meta veri yönetimini işin sonuçlarını ilişkilendirmek için yararlıdır.
- **Meta veri**: özel anahtar-değer çiftleri API uç noktasına geri çağırma sırasındaki döndürdü. Gözden geçirme aracında tanımlanan kısa bir kod anahtar etiket olarak görüntülenir.
- **Ocp Apim abonelik anahtar**: bulunan **ayarları** sekmesi. Daha fazla bilgi için bkz: [genel bakış](overview.md).

Bir sınama konsoluna erişmek için en basit yolu **kimlik bilgileri** penceresi.

1.  İçinde **kimlik bilgileri** penceresinde, seçin [gözden geçirme API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4).

  **Gözden - oluşturma** sayfası açılır.

2.  İçin **açık API sınama Konsolu**, en yakın konumunuzu açıklar bölgeyi seçin.

  ![Gözden geçirme - sayfası bölge seçimi oluşturma](images/test-drive-region.png)

  **Gözden - oluşturma** API konsolu açılır.
  
3.  Gerekli sorgu parametreleri, içerik türü ve abonelik anahtarınızı değerlerini girin. İçinde **istek gövdesinde** kutusunda, içerik (örneğin, görüntü konumu), meta verileri ve içerikle ilişkili diğer bilgileri belirtin.

  ![Gözden geçirme - konsol sorgu parametreleri, üstbilgiler ve istek gövdesi kutusu oluşturma](images/test-drive-review-1.PNG)
  
4.  **Gönder**’i seçin. Bir gözden geçirme kimliği oluşturulur. Aşağıdaki adımlarda kullanmak için bu kimliği kopyalayın.

  ![Gözden geçirme - Konsolu yanıt içerik İnceleme Kimliği kutusu görüntüler oluşturma](images/test-drive-review-2.PNG)
  
5.  Seçin **almak**ve ardından API bölgenizi eşleşen düğmesini seçerek açın. Sonuçta elde edilen sayfasında için değerleri girin **teamName**, **ReviewID**, ve **abonelik anahtarı**. Seçin **Gönder** sayfasında düğmesini. 

  ![Gözden geçirme - Get sonuçları Konsolu oluşturma](images/test-drive-review-3.PNG)
  
6.  Tarama sonuçlarını görürsünüz.

  ![Gözden geçirme - konsol yanıt içerik kutusu oluşturma](images/test-drive-review-4.PNG)
  
7.  İçerik denetleyici Panoda seçin **gözden geçirme** > **görüntü**. Taranan görüntü görünür, İnsan gözden geçirme için hazır.

  ![Bir futbol topu aracı görüntüsünü gözden geçirin](images/test-drive-review-5.PNG)

## <a name="next-steps"></a>Sonraki adımlar

REST API kodunuzdaki kullanın veya başlayın [incelemeler .NET quickstart](moderation-reviews-quickstart-dotnet.md) uygulamanızla tümleştirmek için.
