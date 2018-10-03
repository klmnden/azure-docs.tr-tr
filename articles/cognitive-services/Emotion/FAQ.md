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
ROBOTS: NOINDEX
ms.openlocfilehash: 6c1c4b8e5c2701f3c419a58bc3fdc33f7e629bbd
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48238552"
---
# <a name="emotion-api-frequently-asked-questions"></a>Duygu tanıma API'si hakkında sık sorulan sorular

> [!IMPORTANT]
> Duygu tanıma API'si, 15 Şubat 2019 üzerinde kullanımdan kaldırılacaktır. Duygu tanıma özelliği artık genel olarak kullanılabilir parçası [yüz tanıma API'si](https://docs.microsoft.com/azure/cognitive-services/face/).

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Bu SSS'de sorularınızın yanıtlarını bulamazsanız, duygu tanıma API'si topluluk üzerinde sorma [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) veya Yardım ve Destek kişi [UserVoice](https://cognitive.uservoice.com/).  

-----

**Soru**: *hangi türde görüntü yararlanın, sunduğu duygu tanıma API'si sonuçlarını?*

**Yanıt**: en iyi sonuçlar için yakınlaştırarak daha rahat, tam tamamen çıplak yüz görüntüleri kullanın. Kısmi tamamen çıplak yüzleri ile güvenilirlik azalır ve duygu tanıma API'si, yüz tanıma 45 dereceden fazla olduğu Döndürülmüş resimlerde duyguları algılamayabilir.

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

**Yanıt**: Bu ileti algoritma görüntü ayrıştırılırken bir sorun olduğu anlamına gelir.  
* Desteklenen giriş resim biçimleri, JPEG, PNG, GIF (ilk çerçeve), BMP içerir
* Resim dosyasının boyutu 4 MB'tan büyük olmamalıdır
* Algılanabilir yüz boyut aralığı 36 x 36 için 4096 x 4096 pikseldir. Bu aralık dışında yüzleri algılanmaz
* Teknik güçlükler nedeniyle, örneğin, büyük yüz açıları (poz head), büyük kapatma bazı yüzleri algılanamayabilir. En iyi sonuçları tamamen çıplak ve neredeyse tamamen yüzleri sahip

-----
