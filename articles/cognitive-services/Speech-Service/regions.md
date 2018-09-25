---
title: Konuşma hizmet bölgeleri
description: Konuşma hizmeti bölgeleri için başvuru.
services: cognitive-services
author: mahilleb-msft
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 09/24/2018
ms.author: mahilleb
ms.openlocfilehash: 8485caeff3a7c96ed8f7403befac0026fae16e90
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987569"
---
# <a name="regions-of-the-speech-service"></a>Konuşma hizmeti bölgeleri

Konuşma hizmeti, farklı bölgelerde kullanılabilir.
Bir abonelik oluşturduğunuzda, ihtiyaçlarınıza bağlı olarak kullanılabilir bir bölge seçebilirsiniz.

Aboneliğinizi kullandığınızda, seçtiğiniz bölge için hesabı gerekir.

## <a name="rest-api"></a>REST API

Doğru bölgeye özgü uç noktaları seçmek için REST API'yi kullanın.
Bkz: [REST API'leri](rest-apis.md) Ayrıntılar için.

## <a name="speech-sdk"></a>Konuşma SDK'sı

İçinde [Speech SDK'sı](speech-sdk.md), bölgelerin bir dize olarak belirtilen (örneğin, bir parametre olarak `SpeechConfig.FromSubscription` konuşma SDK for C#).

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
