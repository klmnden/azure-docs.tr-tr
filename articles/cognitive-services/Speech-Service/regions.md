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
ms.openlocfilehash: d651a0a9e006e996db44b82eeaf0a1242a4114e0
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46296099"
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

  Bölge | Konuşma SDK parametresi | Portal
 ------|-------|--------
 Batı ABD | `westus` | https://westus.cris.ai
 Batı ABD 2 | `westus2` | https://westus2.cris.ai 
 Doğu ABD | `eastus` | https://eastus.cris.ai
 Doğu ABD 2 | `eastus2` | https://eastus2.cris.ai
 Doğu Asya | `eastasia` | https://eastasia.cris.ai
 Güneydoğu Asya | `southeastasia` | https://southeastasia.cris.ai
 Kuzey Avrupa | `northeurope` | https://northeurope.cris.ai
 Batı Avrupa | `westeurope` | https://westeurope.cris.ai


### <a name="regions-for-intent-recognition"></a>Amaç tanıma için bölgeler

Kullanılabilir bölgeler için **niyeti tanıma** Speech SDK'sı listelenen [Language Understanding hizmeti bölge sayfası](/azure/cognitive-services/luis/luis-reference-regions).
Listelenen her yayımlama bölge için uç nokta etki alanı adının ilk bölümü karşılık gelen Speech SDK'sı bölge parametre belirlenir.
Örneğin, `westus` yayımlama Batı ABD bölgesi belirtmek için.
