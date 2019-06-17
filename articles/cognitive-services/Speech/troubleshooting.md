---
title: Bing konuşma sorunlarını giderme | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Bing konuşma kullanırken sorunları gidermek nasıl.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: e70e7b79be7dd4ea55c56898eaf8007d25732366
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60513968"
---
# <a name="troubleshooting-bing-speech"></a>Bing konuşma sorunlarını giderme

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

## <a name="error-http-403-forbidden"></a>Hata `HTTP 403 Forbidden`

Konuşma tanıma API'si kullanılırken döndürür bir `HTTP 403 Forbidden` hata.

### <a name="cause"></a>Nedeni

Bu hata genellikle tarafından kimlik doğrulama sorunları nedeniyle oluşur. Bağlantı isteklerini olmadan geçerli `Ocp-Apim-Subscription-Key` veya `Authorization` üstbilgi hizmetiyle tarafından reddedilir bir `HTTP 403 Forbidden` yanıt.

Abonelik anahtarı kimlik doğrulaması için kullanılıyorsa, neden olabilir

- Abonelik anahtarı eksik veya geçersiz
- Abonelik anahtarı kullanım kotası aşıldı
- `Ocp-Apim-Subscription-Key` REST API çağrıldığında alanı istek üst bilgisinde ayarlanmadı

Yetkilendirme belirteci kimlik doğrulaması için kullanılıyorsa, aşağıdaki nedenlerden hatasına neden olabilir.

- `Authorization` üstbilgisi eksik istekte REST kullanırken
- Yetkilendirme üst bilgisinde belirtilen yetkilendirme belirteci geçersiz.
- Yetkilendirme belirtecinin süresi doldu. Erişim belirtecine sahip, 10 dakikalık bir süre sonu

Kimlik doğrulaması hakkında daha fazla bilgi için bkz. [kimlik doğrulaması](How-to/how-to-authentication.md) sayfası.

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

#### <a name="verify-that-your-subscription-key-is-valid"></a>Abonelik anahtarınızı geçerli olduğunu doğrulayın

Doğrulama için aşağıdaki komutu çalıştırabilirsiniz. Değiştirilecek Not *YOUR_SUBSCRIPTION_KEY* kendi abonelik anahtarınızla. Abonelik anahtarınızı geçerliyse, yanıtta bir JSON Web Token (JWT olarak) yetkilendirme belirtecini alır. Aksi takdirde yanıtında hata alırsınız.

> [!NOTE]
> Değiştirin `YOUR_SUBSCRIPTION_KEY` kendi abonelik anahtarınızla.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```Powershell
$FetchTokenHeader = @{
  'Content-type'='application/x-www-form-urlencoded';
  'Content-Length'= '0';
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
}

$OAuthToken = Invoke-RestMethod -Method POST -Uri https://api.cognitive.microsoft.com/sts/v1.0/issueToken -Headers $FetchTokenHeader

# show the token received
$OAuthToken

```

# <a name="curltabcurl"></a>[Curl](#tab/curl)

Bu örnek, Linux üzerinde bash ile curl kullanır. Platformunuzda bulunan kullanılabilir durumda değilse, curl yüklemeniz gerekebilir. Örnek ayrıca, Windows, Git Bash, zsh ve diğer Kabukları Cygwin üzerinde çalışmalıdır.

```
curl -v -X POST "https://api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```
---

Yukarıda kullanılan uygulamanızda veya REST isteği aynı abonelik anahtarını kullandığınızdan emin olun.

#### <a name="verify-the-authorization-token"></a>Yetkilendirme belirteci doğrulayın

Yetkilendirme belirteci kimlik doğrulaması için kullanıyorsanız bu adım yalnızca gereklidir. Yetkilendirme belirteci hala geçerli olduğunu doğrulamak için aşağıdaki komutu çalıştırın. Bu komut, hizmete bir POST isteği yapar ve hizmetinden bir yanıt iletisi bekliyor. HTTP almaya devam ederseniz `403 Forbidden` hata erişim sağlayamazsanız belirteci doğru şekilde ayarlanması ve süresi dolmuş.

> [!IMPORTANT]
> Belirteç, 10 dakikalık bir süre sonu sahiptir.
> [!NOTE]
> Değiştirin `YOUR_AUDIO_FILE` önceden kaydedilmiş ses dosyanızın yoluyla ve `YOUR_ACCESS_TOKEN` yetkilendirme belirteciyle önceki adımda döndürdü.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```Powershell

$SpeechServiceURI =
'https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed'

# $OAuthToken is the authrization token returned by the token service.
$RecoRequestHeader = @{
  'Authorization' = 'Bearer '+ $OAuthToken;
  'Transfer-Encoding' = 'chunked'
  'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
}

# Read audio into byte array
$audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")

$RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes

# Show the result
$RecoResponse

```

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -v -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
```

---

## <a name="error-http-400-bad-request"></a>Hata `HTTP 400 Bad Request`

Bu nedenle, genellikle istek gövdesi geçersiz ses veri içerdiğinden emin olur. Şu anda yalnızca WAV dosyası destekliyoruz.

## <a name="error-http-408-request-timeout"></a>Hata `HTTP 408 Request Timeout`

Hata büyük olasılıkla ses veri hizmetine gönderilmez ve hizmet zaman aşımından sonra bu hatayı verir çünkü. REST API için istek gövdesinde ses verilerini koymanız gerekir.

## <a name="the-recognitionstatus-in-the-response-is-initialsilencetimeout"></a>`RecognitionStatus` İçinde yanıt. `InitialSilenceTimeout`

Ses verisi genellikle soruna neden neden olur. Örneğin,

- Ses başında bir uzun sessizlik zamanına sahip. Hizmet tanıma bazı döndürür ve saniye sayısı sonra durdurulacak `InitialSilenceTimeout`.
- Ses sessizlik işlem görecek ses verileri getiren desteklenmeyen codec biçimi kullanır.
