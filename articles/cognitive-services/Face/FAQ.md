---
title: Yüz API'si hizmeti için sık sorulan sorular | Microsoft Docs
description: Burada, yüz API hizmeti hakkında en popüler sorulan soruların yanıtları bulunur.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 01/26/2017
ms.author: sbowles
ms.openlocfilehash: da2f75deef8a8beea3ba23b6a39eb6d2fe104b54
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351557"
---
# <a name="face-api-frequently-asked-questions"></a>Yüz API sık sorulan sorular
### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-face-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Bu SSS sorularınızın yanıtlarını bulamazsanız, üzerinde yüz API topluluk isteyen deneyin [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) veya Yardım ve Destek kişi [UserVoice](https://cognitive.uservoice.com/).

-----
**Soru**: etkenleri yüz API'nin doğruluğu tanıma, doğrulama veya Benzerini Bul için azaltabilir?

**Yanıt**: genellikle birisi dahil olmak üzere; tanımlayan zorluk insanlar sahip olduğu aynı durumda.
* Bir veya iki gözler engelleme engelleri
* , Örneğin ciddi ışık sert Arka Aydınlatma
* Artı stili veya yüz ince değişiklikler
* Değişikliklerinin geçerlilik süresi
* (Örneğin screaming) aşırı yüz ifadeleri

Yüz API genellikle zor durumlarda bu gibi başarılı olur, ancak doğruluğu azaltılabilir. Tanıma daha sağlam hale ve bu sorunları çözmek için kişileriniz seviyelerine açıları ve Aydınlatma dahil fotoğraf ile eğitmek.

-----
**Soru**: ikili resim verileri geçirme ancak bir "geçersiz resmini" hata iletisi.

**Yanıt**: Bu algoritma görüntü ayrıştırma bir sorun olduğunu gösterir. Nedenler şunlardır:
* Desteklenen giriş resim biçimleri, JPEG, PNG, (ilk çerçeve) GIF, BMP içerir.
* Görüntü dosya boyutu 4 MB'den daha büyük olmalıdır
* Algılanabilir yüz boyutu aralığı 36 x 36 için 4096 x 4096 pikseldir. Bu aralık dışında yüzeyleri algılanmaz
* Bazı yüzeyleri teknik zorluklar nedeniyle, örneğin çok büyük yüz açıları (head-poz) algılanamayabilir büyük kapanması. En iyi sonuçlar tamamen çıplak ve tamamen yakın yüzler sahip

-----
