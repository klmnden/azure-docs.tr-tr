---
title: Sık sorulan sorular - yüz tanıma API'si
titlesuffix: Azure Cognitive Services
description: Yüz tanıma API'si hizmeti ile ilgili en yaygın soruların yanıtları aşağıdadır.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: sbowles
ms.openlocfilehash: 47ce80e1b0cefc01752d2445b751ebe1c2d65d08
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351346"
---
# <a name="face-api-frequently-asked-questions"></a>Yüz tanıma API'si hakkında sık sorulan sorular

> [!TIP]
> Bu SSS'de sorularınızın yanıtlarını bulamazsanız, yüz tanıma API'si topluluk üzerinde sorma [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) veya Yardım ve Destek kişi [UserVoice](https://cognitive.uservoice.com/).

-----
**Soru**: Etkenleri tanıma, doğrulama veya benzer bulmak için yüz tanıma API'SİNİN doğruluğu azaltabilir miyim?

**Yanıt**: Genellikle aynı durumlarda insanlar birisi dahil olmak üzere tanımlayan zorluk sahip olduğu şöyledir:
* Engelleri birini veya ikisini gözler engelleme
* Sert aydınlatma (örneğin, ciddi Arka Aydınlatma)
* Saç stil veya yanı sıra yüz ince değişiklikler
* Geçerlilik süresi nedeniyle değişiklikler
* Extreme yüz ifadelerini (örneğin, screaming)

Genellikle yüz tanıma API'si yukarıdaki gibi zorlu durumlarda başarılı ancak doğruluğu azalabilir. Tanıma daha sağlam hale ve bu sorunları gidermek için kişileriniz açıları ve Aydınlatma bir çeşitliliğini içeren fotoğraflarla eğitin.

-----
**Soru**:  İkili resim verileri geçirme, ancak "geçersiz yüz resmini" hatasını alıyorum.

**Yanıt**:  Bu hata, algoritma görüntü ayrıştırılırken bir sorun olduğunu gösterir. Nedenler şunlardır:
* Desteklenen giriş resim biçimleri, JPEG, PNG, GIF (ilk çerçeve), BMP içerir.
* Resim dosyasının boyutu 4 MB'tan büyük olmamalıdır
* Algılanabilir yüz boyut aralığı 36 x 36 için 4096 x 4096 pikseldir. Bu aralık dışında yüzleri algılanmaz
* Teknik güçlükler nedeniyle, örneğin büyük yüz açıları (poz head), büyük kapatma bazı yüzleri algılanamayabilir. En iyi sonuçları tamamen çıplak ve neredeyse tamamen yüzleri sahip

-----
