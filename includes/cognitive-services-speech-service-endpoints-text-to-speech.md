---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 05/06/2019
ms.author: wolfma
ms.openlocfilehash: d5a4b3a07854c2664de7ec60f3677b666798a9bd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66145396"
---
### <a name="standard-and-neural-voices"></a>Standart ve sinir sesleri

Bölge/uç noktası tarafından standart ve sinir sesleri kullanılabilirliğini belirlemek için bu tabloyu kullanın:

| Bölge | Uç Nokta | Standart sesler | Sinir sesleri |
|--------|----------|-----------------|---------------|
| Avustralya Doğu | `https://australiaeast.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Orta Kanada | `https://canadacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Orta ABD | `https://centralus.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Doğu Asya | `https://eastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Doğu ABD | `https://eastus.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Doğu ABD 2 | `https://eastus2.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Fransa Orta | `https://francecentral.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Hindistan Orta | `https://centralindia.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Japonya Doğu | `https://japaneast.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Kore Orta | `https://koreacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Orta Kuzey ABD | `https://northcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Kuzey Avrupa | `https://northeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Orta Güney ABD | `https://southcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Güneydoğu Asya | `https://southeastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Birleşik Krallık Güney | `https://uksouth.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Batı Avrupa | `https://westeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |
| Batı ABD | `https://westus.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Hayır |
| Batı ABD 2 | `https://westus2.tts.speech.microsoft.com/cognitiveservices/v1` | Evet | Evet |

### <a name="custom-voices"></a>Özel ses

Özel ses tipi oluşturduysanız, oluşturduğunuz uç noktasını kullanın. Aşağıda listelenen uç noktaları değiştirerek kullanabilirsiniz `{deploymentId}` ses modeliniz için dağıtım Kimliğine sahip.

| Bölge | Uç Nokta |
|--------|----------|
| Avustralya Doğu | `https://australiaeast.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Orta Kanada | `https://canadacentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Orta ABD | `https://centralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Doğu Asya | `https://eastasia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Doğu ABD | `https://eastus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Doğu ABD 2 | `https://eastus2.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Fransa Orta | `https://francecentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Hindistan Orta | `https://centralindia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Japonya Doğu | `https://japaneast.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Kore Orta | `https://koreacentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Orta Kuzey ABD | `https://northcentralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Kuzey Avrupa | `https://northeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Orta Güney ABD | `https://southcentralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Güneydoğu Asya | `https://southeastasia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Birleşik Krallık Güney | `https://uksouth.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Batı Avrupa | `https://westeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Batı ABD | `https://westus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Batı ABD 2 | `https://westus2.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
