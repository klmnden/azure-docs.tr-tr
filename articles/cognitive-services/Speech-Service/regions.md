---
title: Konuşma hizmet bölgeleri
titlesuffix: Azure Cognitive Services
description: Konuşma hizmeti bölgeleri için başvuru.
services: cognitive-services
author: mahilleb-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mahilleb
ms.openlocfilehash: a5fce6f9547a96da3ce482ce388e5ba2093f2af4
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49468135"
---
# <a name="regions-of-the-speech-service"></a>Konuşma hizmeti bölgeleri

Konuşma hizmeti, farklı bölgelerde kullanılabilir.
Bir abonelik oluşturduğunuzda, ihtiyaçlarınıza bağlı olarak kullanılabilir bir bölge seçebilirsiniz.

Aboneliğinizi kullandığınızda, seçtiğiniz bölge için hesabı gerekir.

## <a name="rest-api"></a>REST API

Doğru bölgeye özgü uç noktaları seçmek için REST API'yi kullanın.
Bkz: [REST API'leri](rest-apis.md) Ayrıntılar için.

## <a name="speech-sdk"></a>Konuşma SDK'sı

İçinde [konuşma hizmeti SDK'sı](speech-sdk.md), bölgelerin bir dize olarak belirtilen (örneğin, bir parametre olarak `SpeechConfig.FromSubscription` konuşma SDK for C#).

### <a name="regions-for-speech-recognition-and-translation"></a>Konuşma tanıma ve çeviri için bölgeler

Kullanılabilir bölgeler için aşağıdaki tabloda **konuşma tanıma** ve **çeviri**.

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

Kullanılabilir bölgeler için **niyeti tanıma** Speech SDK'sı listelenir [Language Understanding hizmeti bölge sayfası](/azure/cognitive-services/luis/luis-reference-regions).
Listelenen her yayımlama bölge için uç nokta etki alanı adının ilk bölümü karşılık gelen Speech SDK'sı bölge parametre belirlenir.
Örneğin, `westus` yayımlama Batı ABD bölgesi belirtmek için.
