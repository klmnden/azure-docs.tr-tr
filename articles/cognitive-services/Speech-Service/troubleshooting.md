---
title: Bilişsel hizmetler konuşma SDK sorunlarını giderme
description: Bilişsel sorun giderme SDK konuşma Hizmetleri
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: ff8aba562cfd2d6d54c708ee7fdc4c6ca7185f29
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284131"
---
# <a name="troubleshooting-speech-services-sdk"></a>Sorun giderme konuşma Hizmetleri SDK'sı

Bu makalede Speech SDK'sı kullanırken karşılaşabileceğiniz sorunları gidermenize yardımcı olacak bilgiler sağlar.

## <a name="error-websocket-upgrade-failed-with-an-authentication-error-403"></a>Hata `WebSocket Upgrade failed with an authentication error (403).`

Bölge veya hizmetiniz için yanlış uç nokta olabilir. Doğru olduğundan emin olmak için URI denetleyin. Bu abonelik anahtarı veya yetkilendirme ile ilgili bir sorun da olabilir bir sonraki bölüm ayrıca görmeniz belirteci.

## <a name="error-http-403-forbidden-or-error-http-401-unauthorized"></a>Hata `HTTP 403 Forbidden` veya hata `HTTP 401 Unauthorized`

Bu hata genellikle tarafından kimlik doğrulama sorunları nedeniyle oluşur. Geçerli bir olmadan bağlantı isteklerini `Ocp-Apim-Subscription-Key` veya `Authorization` üstbilgi durum 401 veya 403 reddedilir.

* Kimlik doğrulaması için bir abonelik anahtarı kullanıyorsanız, bunun nedeni aşağıdakilerden biri olabilir:

    - Abonelik anahtarı eksik veya geçersiz
    - Aboneliğinizin kullanımı kotayı aştınız

* Bir yetkilendirme belirteci kimlik doğrulaması için kullanılıyorsa, neden olabilir:

    - Yetkilendirme belirteci geçersiz.
    - Yetkilendirme belirtecinin süresi doldu

### <a name="validate-your-subscription-key"></a>Abonelik anahtarınızı doğrulayın

Aşağıdaki komutlardan birini çalıştırarak bir geçerli abonelik anahtarı olduğundan emin olmak için doğrulayabilirsiniz.

> [!NOTE]
> Değiştirin `YOUR_SUBSCRIPTION_KEY` ve `YOUR_REGION` abonelik anahtarı ve ilgili bölge, sırasıyla.

* PowerShell

    ```Powershell
    $FetchTokenHeader = @{
      'Content-type'='application/x-www-form-urlencoded'
      'Content-Length'= '0'
      'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
    }
    $OAuthToken = Invoke-RestMethod -Method POST -Uri https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken -Headers $FetchTokenHeader
    $OAuthToken
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0"
    ```

### <a name="validate-an-authorization-token"></a>Bir yetkilendirme belirtecini doğrula

Bir yetkilendirme belirteci kimlik doğrulaması için kullanıyorsanız, yetkilendirme belirtecini hala geçerli olduğunu doğrulamak için aşağıdaki komutlardan birini çalıştırın. Belirteçleri 10 dakika için geçerlidir.

> [!NOTE]
> Değiştirin `YOUR_AUDIO_FILE` önceden kaydedilmiş ses dosyanızın yoluyla `YOUR_ACCESS_TOKEN` önceki adımda döndürülen yetkilendirme belirtecini ve `YOUR_REGION` doğru bölgeyle.

* PowerShell

    ```Powershell
    $SpeechServiceURI =
    'https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US'
    
    # $OAuthToken is the authorization token returned by the token service.
    $RecoRequestHeader = @{
      'Authorization' = 'Bearer '+ $OAuthToken
      'Transfer-Encoding' = 'chunked'
      'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
    }
    
    # Read audio into byte array
    $audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")
    
    $RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes
    
    # Show the result
    $RecoResponse
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Transfer-Encoding: chunked" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
    ```

---

## <a name="error-http-400-bad-request"></a>Hata `HTTP 400 Bad Request`

İstek gövdesi geçersiz ses verilerini içerdiğinde, bu hata genellikle oluşur. Yalnızca `WAV` biçimi destekleniyor. Ayrıca uygun belirttiğinizden emin olmak için istenen üstbilgileri denetleyin `Content-Type` ve `Content-Length`.

## <a name="error-http-408-request-timeout"></a>Hata `HTTP 408 Request Timeout`

Ses veri hizmetine gönderilen nedeni büyük olasılıkla bir hatadır. Bu hata Ayrıca ağ sorunları neden olabilir.

## <a name="the-recognitionstatus-in-the-response-is-initialsilencetimeout"></a>`RecognitionStatus` İçinde yanıt. `InitialSilenceTimeout`

Ses verisi genellikle soruna neden neden olur. Örneğin:

* Uzun bir esnetme ses başına sessizlik yoktur. Hizmet tanıma birkaç saniye sonra durur ve dönüş `InitialSilenceTimeout`.
* Ses sessizlik kabul edilmesi ses verilerini neden olan desteklenmeyen codec biçimi kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Sürüm notları](releasenotes.md)

