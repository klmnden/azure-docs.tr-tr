---
title: Sık sorulan sorular - yüz tanıma API'si
titlesuffix: Azure Cognitive Services
description: Yüz tanıma API'si hizmeti ile ilgili en yaygın soruların yanıtları aşağıdadır.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: sbowles
ms.openlocfilehash: b4b2c09ef608da7c52d415d5f1f2215ddc31c41a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55223332"
---
# <a name="face-api-frequently-asked-questions"></a>Yüz tanıma API'si hakkında sık sorulan sorular

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-face-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Bu SSS'de sorularınızın yanıtlarını bulamazsanız, yüz tanıma API'si topluluk üzerinde sorma [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) veya Yardım ve Destek kişi [UserVoice](https://cognitive.uservoice.com/).

-----
**Soru**: Etkenleri tanıma, doğrulama veya benzer bulmak için yüz tanıma API'SİNİN doğruluğu azaltabilir miyim?

**Yanıt**: Genel olarak, insanlar birisi dahil olmak üzere tanımlayan zorluk sahip olduğu aynı durumda;
* Engelleri birini veya ikisini gözler engelleme
* Fon ışığı, örneğin ciddi ışık olanaklı kılma
* Saç stil veya yanı sıra yüz ince değişiklikler
* Geçerlilik süresi nedeniyle değişiklikler
* Extreme yüz ifadelerini (örneğin screaming)

Yüz tanıma API'si genellikle bunlar gibi zorlu durumlarda başarılı oluyor ancak doğruluğu azalabilir. Tanıma daha sağlam hale ve bu sorunları gidermek için kişileriniz açıları ve Aydınlatma bir çeşitliliğini içeren fotoğraflarla eğitin.

-----
**Soru**:  İkili resim verileri geçirme, ancak "geçersiz yüz resmini" hatasını alıyorum.

**Yanıt**:  Bu algoritma görüntü ayrıştırılırken bir sorun olduğu anlamına gelir. Nedenler şunlardır:
* Desteklenen giriş resim biçimleri, JPEG, PNG, GIF (ilk çerçeve), BMP içerir.
* Resim dosyasının boyutu 4 MB'tan büyük olmamalıdır
* Algılanabilir yüz boyut aralığı 36 x 36 için 4096 x 4096 pikseldir. Bu aralık dışında yüzleri algılanmaz
* Teknik güçlükler nedeniyle, örneğin çok büyük yüz açıları (baş-poz) bazı yüzleri algılanamayabilir büyük kapatma. En iyi sonuçları tamamen çıplak ve neredeyse tamamen yüzleri sahip

-----
