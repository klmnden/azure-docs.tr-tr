---
title: Konuşma hizmet bölgeleri
description: Konuşma hizmeti bölgeleri için başvuru.
services: cognitive-services
author: mahilleb-msft
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 06/28/2018
ms.author: mahilleb
ms.openlocfilehash: 082002b25b02e1e496221f4686d0e636630dd438
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324400"
---
# <a name="regions-of-the-speech-service"></a>Konuşma hizmeti bölgeleri

Konuşma hizmeti, farklı bölgelerde kullanılabilir.
Bir abonelik oluşturduğunuzda, gereksinimlerinize bağlı olarak kullanılabilir bir bölge seçebilirsiniz.

Aboneliğinizi kullandığınızda, seçtiğiniz bölgede hesabı gerekir.

## <a name="rest-api"></a>REST API

REST API kullanarak, bölgeye özgü sağ uç noktalarını seçin.
Bkz: [REST API'leri](rest-apis.md) Ayrıntılar için.

## <a name="speech-sdk"></a>Konuşma SDK'sı

İçinde [Speech SDK'sı](speech-sdk.md), bölgelerin bir dize olarak belirtilen (örneğin, bir parametre olarak [SpeechFactory.FromSubscription](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechfactory.fromsubscription) konuşma SDK for C#).

### <a name="regions-for-speech-recognition-and-translation"></a>Konuşma tanıma ve çeviri için bölgeler

Kullanılabilir bölgeler için aşağıdaki tabloda listelenmiştir **konuşma tanıma** ve **çeviri**:

Bölge| Speech SDK'sı bölge parametresi için değer
-|-
Batı ABD| `westus`
Doğu Asya| `eastasia`
Kuzey Avrupa| `northeurope`

### <a name="regions-for-intent-recognition"></a>Amaç tanıma için bölgeler

Kullanılabilir bölgeler için **niyeti tanıma** Speech SDK'sı listelenen [Language Understanding hizmeti bölge sayfası](/azure/cognitive-services/luis/luis-reference-regions).
Listelenen her yayımlama bölge için uç nokta etki alanı adının ilk bölümü karşılık gelen Speech SDK'sı bölge parametre belirlenir.
Örneğin, `westus` yayımlama Batı ABD bölgesi belirtmek için.
