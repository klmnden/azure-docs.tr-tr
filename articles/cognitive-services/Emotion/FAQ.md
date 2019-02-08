---
title: Sık sorulan sorular - duygu tanıma API'si
titlesuffix: Azure Cognitive Services
description: Duygu tanıma API'si hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: d4e074a789a23d18349a32d65f7a4e9a6f41f6e8
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55860040"
---
# <a name="emotion-api-frequently-asked-questions"></a>Duygu tanıma API'si hakkında sık sorulan sorular

> [!IMPORTANT]
> Duygu Tanıma API'si 15 Şubat 2019 tarihinde kullanım dışı bırakılacaktır. Duygu tanıma özelliği [Yüz Tanıma API'sinin](https://docs.microsoft.com/azure/cognitive-services/face/) bir parçası olarak genel kullanıma sunulmuştur.

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Bu SSS'de sorularınızın yanıtlarını bulamazsanız, duygu tanıma API'si topluluk üzerinde sorma [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) veya Yardım ve Destek kişi [UserVoice](https://cognitive.uservoice.com/).  

-----

**Soru**: *Hangi türde görüntü sunduğu duygu tanıma API'si en iyi sonuçları elde?*

**Yanıt**: Kılavuzundaki, tam tamamen çıplak yüz görüntüleri en iyi sonuçlar için kullanın. Kısmi tamamen çıplak yüzleri ile güvenilirlik azalır ve duygu tanıma API'si, yüz tanıma 45 dereceden fazla olduğu Döndürülmüş resimlerde duyguları algılamayabilir.

-----

**Soru**: *Duygu tanıma API'si kaç duyguları belirleyebilir misiniz?*

**Yanıt**: Duygu tanıma API'si, sekiz farklı evrensel olarak kabul edilen duyguları tanır:
* Mutluluk
* Üzüntü
* Şaşkınlık
* Öfke
* Korku
* Küçümseme
* İğrenme
* Nötr

-----

**Soru**: *Canlı video akışı API'sine geçirin ve sonuç aynı anda almak için herhangi bir yolu var mı?*

**Yanıt**: Kullanım görüntü tabanlı duygu tanıma API'si ve her karede çağrı veya çerçeveleri için performans atlayabilirsiniz.  Video kare kare analizi örneklerini kullanılabilir.

-----

**Soru**: *İkili resim verileri geçirme ancak bana sağlar: "Geçersiz yüz resmini.**

**Yanıt**: Bu ileti, algoritma görüntü ayrıştırılırken bir sorun olduğu anlamına gelir.  
* Desteklenen giriş resim biçimleri, JPEG, PNG, GIF (ilk çerçeve), BMP içerir
* Resim dosyasının boyutu 4 MB'tan büyük olmamalıdır
* Algılanabilir yüz boyut aralığı 36 x 36 için 4096 x 4096 pikseldir. Bu aralık dışında yüzleri algılanmaz
* Teknik güçlükler nedeniyle, örneğin, büyük yüz açıları (poz head), büyük kapatma bazı yüzleri algılanamayabilir. En iyi sonuçları tamamen çıplak ve neredeyse tamamen yüzleri sahip

-----
