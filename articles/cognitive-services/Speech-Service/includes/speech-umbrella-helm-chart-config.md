---
title: Konuşma kapsayıcılara yükleme
titleSuffix: Azure Cognitive Services
description: Konuşma terimdir helm grafiği yapılandırma seçeneklerinin ayrıntılı olarak açıklanmaktadır.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: ed64412ccf9d192506fafe546b1ccee7941aa43a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711818"
---
### <a name="speech-umbrella-chart"></a>Konuşma (terimdir grafik)

Üst düzey "Genel" grafikteki değerleri, karşılık gelen alt grafik değerlerini geçersiz kılar. Bu nedenle, tüm şirket özelleştirilmiş değerleri burada eklenmelidir.

|Parametre|Açıklama|Varsayılan|
| -- | -- | -- | -- |
| `speechToText.enabled` | Olmadığını **konuşma metin** etkinleştirildi. | `true` |
| `speechToText.verification.enabled` | Olmadığını `helm test` yeteneği **konuşma metin** etkinleştirildi. | `true` |
| `speechToText.verification.image.registry` | Docker görüntü deposuna, `helm test` test etmek için kullandığı **konuşma metin** hizmeti. Helm, test etmek için küme içinde ayrı pod oluşturur ve çeker *test kullanımı* bu kayıt defterinden görüntü. | `docker.io` |
| `speechToText.verification.image.repository` | Docker görüntü deposuna, `helm test` test etmek için kullandığı **konuşma metin** hizmeti. Helm test pod çekmek için bu depoyu kullanır *test kullanımı* görüntü. | `antsu/on-prem-client` |
| `speechToText.verification.image.tag` | Docker görüntü etiketi ile kullanılan `helm test` için **konuşma metin** hizmeti. Helm test pod çekmek için bu etiket kullanır *test kullanımı* görüntü. | `latest` |
| `speechToText.verification.image.pullByHash` | Olmadığını *test kullanımı* docker görüntüsü karma tarafından çekilir. Varsa `true`, `speechToText.verification.image.hash` ile geçerli görüntü karma değeri eklenmelidir. | `false` |
| `speechToText.verification.image.arguments` | Yürütmek için kullanılan bağımsız değişkenleri *test kullanımı* docker görüntüsü. Helm test pod çalıştırırken bu bu bağımsız değişkenler kapsayıcıya geçirir `helm test`. | `"./speech-to-text-client"`<br/> `"./audio/whatstheweatherlike.wav"` <br/> `"--expect=What's the weather like"`<br/>`"--host=$(SPEECH_TO_TEXT_HOST)"`<br/>`"--port=$(SPEECH_TO_TEXT_PORT)"` |
| `textToSpeech.enabled` | Olmadığını **metin okuma** etkinleştirildi. | `true` |
| `textToSpeech.verification.enabled` | Olmadığını `helm test` yeteneği **konuşma metin** etkinleştirildi. | `true` |
| `textToSpeech.verification.image.registry` | Docker görüntü deposuna, `helm test` test etmek için kullandığı **konuşma metin** hizmeti. Helm, test etmek için küme içinde ayrı pod oluşturur ve çeker *test kullanımı* bu kayıt defterinden görüntü. | `docker.io` |
| `textToSpeech.verification.image.repository` | Docker görüntü deposuna, `helm test` test etmek için kullandığı **konuşma metin** hizmeti. Helm test pod çekmek için bu depoyu kullanır *test kullanımı* görüntü. | `antsu/on-prem-client` |
| `textToSpeech.verification.image.tag` | Docker görüntü etiketi ile kullanılan `helm test` için **konuşma metin** hizmeti. Helm test pod çekmek için bu etiket kullanır *test kullanımı* görüntü. | `latest` |
| `textToSpeech.verification.image.pullByHash` | Olmadığını *test kullanımı* docker görüntüsü karma tarafından çekilir. Varsa `true`, `textToSpeech.verification.image.hash` ile geçerli görüntü karma değeri eklenmelidir. | `false` |
| `textToSpeech.verification.image.arguments` | Bağımsız değişkenler ile yürütülecek *test kullanımı* docker görüntüsü. Bu bağımsız değişkenler için kapsayıcı çalıştırırken helm test pod geçirir `helm test`. | `"./text-to-speech-client"`<br/> `"--input='What's the weather like'"` <br/> `"--host=$(TEXT_TO_SPEECH_HOST)"`<br/>`"--port=$(TEXT_TO_SPEECH_PORT)"` |