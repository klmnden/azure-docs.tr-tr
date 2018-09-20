---
title: Sık sorulan sorular - duygu tanıma API'si
titlesuffix: Azure Cognitive Services
description: Duygu tanıma API'si hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: anroth
ms.openlocfilehash: 56b3f2899f1e77c1a2b840285e2c69bdb8987ff4
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363097"
---
# <a name="emotion-api-frequently-asked-questions"></a>Duygu tanıma API'si hakkında sık sorulan sorular
 
> [!IMPORTANT]
> Video API'si önizlemesi 30 Ekim 2017 tarihinde sona erecek. Yeni deneyin [Video Indexer API önizlemesi](https://azure.microsoft.com/services/cognitive-services/video-indexer/) kolayca videolardan içgörüleri ayıklayın ve konuşulan sözcükleri, yüzleri, karakterleri ve duyguları algılayarak arama sonuçları gibi içerik keşif deneyimlerini geliştirin. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Bu SSS'de sorularınızın yanıtlarını bulamazsanız, duygu tanıma API'si topluluk üzerinde sorma [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) veya Yardım ve Destek kişi [UserVoice](https://cognitive.uservoice.com/).  

-----

**Soru**: *hangi türde görüntü yararlanın, sunduğu duygu tanıma API'si sonuçlarını?*

**Yanıt**: en iyi sonuçlar için yakınlaştırarak daha rahat, tam tamamen çıplak yüz görüntüleri kullanın. Kısmi tamamen çıplak yüzleri ile güvenilirlik azalır ve duygu tanıma API'si, yüz tanıma 45 derece ya da daha fazla olduğu resimlerde duyguları algılamayabilir.

-----

**Soru**: *kaç duyguları duygu tanıma API'si belirleyebilir mi?*

**Yanıt**: duygu tanıma API'si, sekiz farklı evrensel olarak kabul edilen duyguları tanır: 
* Mutluluk
* Üzüntü
* Şaşkınlık
* Öfke
* Korku
* Küçümseme
* İğrenme 
* Nötr 

-----

**Soru**: *canlı video akışı API'sine geçirin ve sonuç aynı anda almak için herhangi bir yol var mı?*

**Yanıt**: resim tabanlı duygu tanıma API'si kullanın ve her karede arayın veya performans çerçeveleri atlayın.  Video kare kare analizi örneklerini kullanılabilir.

-----

**Soru**: *bana sağlar ancak ikili resim verileri geçirme: "geçersiz yüz resmini.**

**Yanıt**: Bu algoritma görüntü ayrıştırılırken bir sorun olduğu anlamına gelir.  
* Desteklenen giriş resim biçimleri, JPEG, PNG, GIF (ilk çerçeve), BMP içerir. 
* Resim dosyasının boyutu 4 MB'tan büyük olmamalıdır
* Algılanabilir yüz boyut aralığı 36 x 36 için 4096 x 4096 pikseldir. Bu aralık dışında yüzleri algılanmaz
* Teknik güçlükler nedeniyle, örneğin çok büyük yüz açıları (baş-poz) bazı yüzleri algılanamayabilir büyük kapatma. En iyi sonuçları tamamen çıplak ve neredeyse tamamen yüzleri sahip

-----
