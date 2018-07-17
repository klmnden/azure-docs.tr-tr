---
title: Konuşma hizmet bölgeleri | Microsoft Docs
description: Konuşma hizmeti bölgeleri için başvuru.
services: cognitive-services
author: mahilleb-msft
manager: wolmfa61
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 06/28/2018
ms.author: mahilleb
ms.openlocfilehash: 11360d163fdba057d373d091d46903cde7789a8b
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071428"
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

Kullanılabilir bölgeler için aşağıdaki tabloda listelenmiştir **konuşma tanıma** ve **çeviri**:

Bölge| Speech SDK'sı bölge parametresi için değer
-|-
Batı ABD| `westus`
Doğu Asya| `eastasia`
Kuzey Avrupa| `northeurope`

Kullanılabilir bölgeler için **niyeti tanıma** Speech SDK'sı listelenen [Language Understanding hizmeti bölge sayfası](/azure/cognitive-services/luis/luis-reference-regions).
Listelenen her yayımlama bölge için uç nokta etki alanı adının ilk bölümü karşılık gelen Speech SDK'sı bölge parametre belirlenir.
Örneğin, `westus` yayımlama Batı ABD bölgesi belirtmek için.
