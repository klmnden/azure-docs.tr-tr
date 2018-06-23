---
title: Duygu tanıma API'si ile ilgili SSS | Microsoft Docs
description: Bilişsel Hizmetleri'nde duygu tanıma API'si hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 01/26/2017
ms.author: anroth
ms.openlocfilehash: 8532d7c00fd8d7b01d84b5e55cb9bbc60241789c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351575"
---
# <a name="emotion-api-frequently-asked-questions"></a>Duygu tanıma API'si sık sorulan sorular
 
> [!IMPORTANT]
> Video API Önizleme 30 Ekim 2017 sona erer. Yeni deneyin [Video dizin oluşturucu API önizlemesi](https://azure.microsoft.com/services/cognitive-services/video-indexer/) kolayca videoların öngörüleri ayıklamak ve konuşulan sözcüklerin, yüzler, karakterler ve duygular algılayarak arama sonuçları gibi içerik bulma deneyimlerini geliştirmek üzere. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Bu SSS sorularınızın yanıtlarını bulamazsanız, üzerinde duygu tanıma API'si topluluk isteyen deneyin [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) veya Yardım ve Destek kişi [UserVoice](https://cognitive.uservoice.com/).  

-----

**Soru**: *en iyi ne tür görüntüleri alma sonuçları duygu API'SİNDEN?*

**Yanıt**: engellenmemiş olmayan, tam tamamen çıplak yüz görüntüleri en iyi sonuçlar için kullanın. Kısmi tamamen çıplak yazıtipleri ile güvenilirlik azalır ve duygu tanıma API'si duygular yüz Döndürülmüş 45 dereceyi veya daha fazla olduğu görüntülerinde anlamayabilir.

-----

**Soru**: *kaç duygular duygu tanıma API'si belirleyebilir mi?*

**Yanıt**: duygu tanıma API'si sekiz farklı evrensel olarak kabul duygular tanır: 
* Mutluluk
* Üzüntü
* Şaşkınlık
* Öfke
* Korku
* Küçümseme
* İğrenme 
* Nötr 

-----

**Soru**: *API için Canlı bir video akışına geçirmek ve aynı anda sonuç almak için herhangi bir şekilde yok?*

**Yanıt**: resim tabanlı duygu API kullanın ve her çerçevesi çağrısı veya performans çerçeveleri atlayabilirsiniz.  Video kare kare analiz örnekleri kullanılabilir.

-----

**Soru**: *ikili resim verileri geçirme ancak bana verir: "geçersiz yüz görüntü.**

**Yanıt**: Bu algoritma görüntü ayrıştırma bir sorun olduğunu gösterir.  
* Desteklenen giriş resim biçimleri, JPEG, PNG, (ilk çerçeve) GIF, BMP içerir. 
* Görüntü dosya boyutu 4 MB'den daha büyük olmalıdır
* Algılanabilir yüz boyutu aralığı 36 x 36 için 4096 x 4096 pikseldir. Bu aralık dışında yüzeyleri algılanmaz
* Bazı yüzeyleri teknik zorluklar nedeniyle, örneğin çok büyük yüz açıları (head-poz) algılanamayabilir büyük kapanması. En iyi sonuçlar tamamen çıplak ve tamamen yakın yüzler sahip

-----
