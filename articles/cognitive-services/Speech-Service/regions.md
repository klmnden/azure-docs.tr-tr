---
title: Konuşma hizmet bölgeler | Microsoft Docs
description: Konuşma hizmetinin bölgeler için başvuru.
services: cognitive-services
author: mahilleb-msft
manager: wolmfa61
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 06/28/2018
ms.author: mahilleb
ms.openlocfilehash: 1eb3768f5a5c5a27a45dde3f62f862f36fa3e8ac
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098507"
---
# <a name="regions-of-the-speech-service"></a>Konuşma hizmetinin bölgeleri

Konuşma hizmeti farklı bölgelerde kullanılabilir.
Bir abonelik oluşturduğunuzda, gereksinimlerinize bağlı olarak uygun bir bölgeyi seçebilirsiniz.

Aboneliğinizi kullandığınızda, seçtiğiniz bölge için hesabı gerekir.

## <a name="rest-api"></a>REST API

REST API kullanarak, bölgeye özgü sağ uç noktaları seçin.
Bkz: [REST API'leri](rest-apis.md) Ayrıntılar için.



## <a name="speech-sdk"></a>Konuşma SDK'sı

İçinde [konuşma SDK](speech-sdk.md), bölgeleri, dize olarak belirtilir (örneğin, bir parametre olarak [SpeechFactory.FromSubscription](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechfactory.fromsubscription) konuşma SDK için C#).

Konuşma tanıma ve çeviri için kullanılabilir bölgeleri aşağıdaki tabloda listelenmiştir:

Bölge| Konuşma SDK'sı bölge parametresinin değeri
-|-
Batı ABD| `westus`
Doğu Asya| `eastasia`
Kuzey Avrupa| `northeurope`

Konuşma SDK'sı üzerinden hedefi tanıma için kullanılabilir bölgeleri içinde listelenen [dil anlama hizmeti bölge sayfası](/azure/cognitive-services/luis/luis-reference-regions).
Listelenen her yayımlama bölge için karşılık gelen konuşma SDK bölge parametresi uç nokta etki alanı adı ilk parçası olarak belirlenir.
Örneğin, `westus` Batı ABD yayımlama bölgesi belirtmek için.
