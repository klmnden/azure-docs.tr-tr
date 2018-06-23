---
title: Sorun giderme | Microsoft Docs
description: Microsoft konuşma hizmetini kullanırken sorunları gidermek nasıl.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 04f3da19939d523d201d357b2b0293db1508431d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352186"
---
# <a name="troubleshooting"></a>Sorun giderme

## <a name="error-http-403-forbidden"></a>hata `HTTP 403 Forbidden`

Konuşma tanıma API'si kullanılırken döndüren bir `HTTP 403 Forbidden` hata.

### <a name="cause"></a>Nedeni

Bu hata genellikle kimlik doğrulama sorunları kaynaklanır. Bağlantı isteklerini olmadan geçerli `Ocp-Apim-Subscription-Key` veya `Authorization` üstbilgi hizmetiyle tarafından reddedilir bir `HTTP 403 Forbidden` yanıt.

Abonelik anahtarı kimlik doğrulaması için kullanılıyorsa, neden olabilir

- Abonelik anahtarı eksik veya geçersiz
- Abonelik anahtarı kullanım kotası aşıldı
- `Ocp-Apim-Subscription-Key` alan REST API çağrıldığında istek üstbilgisinde ayarlanmadı

Yetkilendirme belirteci kimlik doğrulaması için kullanılıyorsa, aşağıdaki nedenlerden hatasına neden olabilir.

- `Authorization` üstbilgisi eksik istekte REST kullanırken
- Yetkilendirme üstbilgisinde belirtilen yetkilendirme belirteci geçersiz
- Yetkilendirme belirtecinin süresi doldu. Erişim belirteci 10 dakikalık bir süre sonu sahip

Kimlik doğrulaması hakkında daha fazla bilgi için bkz: [kimlik doğrulaması](How-to/how-to-authentication.md) sayfası.

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

#### <a name="verify-that-your-subscription-key-is-valid"></a>Abonelik anahtarınızı geçerli olduğunu doğrulayın

Doğrulama için aşağıdaki komutu çalıştırabilirsiniz. Değiştirmek için Not *YOUR_SUBSCRIPTION_KEY* kendi abonelik anahtara sahip. Abonelik anahtarınızı geçerliyse, yanıtta bir JSON Web Token (JWT olarak) yetkilendirme belirtecini alırsınız. Aksi durumda yanıt olarak bir hata alıyorsunuz.

> [!NOTE]
> Değiştir `YOUR_SUBSCRIPTION_KEY` kendi abonelik anahtara sahip.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/Powershell)

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

Örnek curl Linux bash ile kullanır. Platformunuz üzerinde kullanılabilir durumda değilse, curl yüklemeniz gerekebilir. Bu örnek ayrıca Windows, Git Bash, zsh ve diğer Kabukları Cygwin üzerinde çalışması gerekir.

```
curl -v -X POST "https://api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```
---

Yukarıda kullanılan uygulamanızı veya REST isteğindeki aynı abonelik anahtarı kullandığınızdan emin olun.

#### <a name="verify-the-authorization-token"></a>Yetkilendirme belirtecini doğrula

Kimlik doğrulaması için yetkilendirme belirtecini kullanıyorsanız bu adımı yalnızca gereklidir. Yetkilendirme belirtecini hala geçerli olduğunu doğrulamak için aşağıdaki komutu çalıştırın. Komut, hizmete bir POST isteği yapar ve hizmetinden bir yanıt iletisi bekliyor. HTTP almaya devam ediyorsanız `403 Forbidden` hatası, erişim iki kez kontrol belirteci doğru olarak ayarlanmış ve süresi.

> [!IMPORTANT]
> Belirtecin 10 dakikalık bir süre sonu vardır.
> [!NOTE]
> Değiştir `YOUR_AUDIO_FILE` önceden kaydedilmiş ses dosyanızın yolu ile ve `YOUR_ACCESS_TOKEN` yetki belirteciyle önceki adımda döndürülen.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/Powershell)

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

## <a name="error-http-400-bad-request"></a>hata `HTTP 400 Bad Request`

Bu nedenle, genellikle istek gövdesinde geçersiz ses veri içerdiğinden emin olur. Şu anda yalnızca WAV dosyası destekliyoruz.

## <a name="error-http-408-request-timeout"></a>hata `HTTP 408 Request Timeout`

Büyük olasılıkla olduğundan ses veri hizmetine gönderilmez ve hizmet zaman aşımından sonra bu hatayı döndürür hatasıdır. REST API için istek gövdesinde ses verilerini moduna geçirmelisiniz.

## <a name="the-recognitionstatus-in-the-response-is-initialsilencetimeout"></a>`RecognitionStatus` Yanıt `InitialSilenceTimeout`

Ses verileri, genellikle soruna neden nedenidir. Örneğin,

- Ses başında uzun sessizlik süresi vardır. Hizmet tanıma bazı döndürür ve saniye sayısı sonra durdurulacak `InitialSilenceTimeout`.
- Ses sessizlik değerlendirilmesi ses veri yapar desteklenmeyen codec biçimi kullanır.
