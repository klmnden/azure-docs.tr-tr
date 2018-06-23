---
title: Bilişsel hizmetler konuşma SDK sorunlarını giderme | Microsoft Docs
description: Konuşma SDK Hizmetleri Bilişsel sorun giderme
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: 16eaebcf9494ab57521068a9418ccf2ac7f5a8fe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354550"
---
# <a name="troubleshooting-speech-services-sdk"></a>Sorun giderme konuşma Services SDK'sı

Bu makale, konuşma SDK'sı kullanırken karşılaşabileceğiniz sorunları gidermenize yardımcı olacak bilgiler sağlar.

## <a name="error-websocket-upgrade-failed-with-an-authentication-error-403"></a>hata `WebSocket Upgrade failed with an authentication error (403).`

Bölge veya hizmet için yanlış uç noktaya sahip olabilir. Doğru olduğundan emin olmak için URI denetleyin. Bu abonelik anahtarı veya yetkilendirme ile ilgili bir sorun da olabilir gibi bir sonraki bölüm ayrıca bkz: belirteci.

## <a name="error-http-403-forbidden-or-error-http-401-unauthorized"></a>Hata `HTTP 403 Forbidden` veya hata `HTTP 401 Unauthorized`

Bu hata genellikle kimlik doğrulama sorunları kaynaklanır. Geçerli bir olmadan bağlantı isteklerini `Ocp-Apim-Subscription-Key` veya `Authorization` üstbilgi durum 401 veya 403 reddedilir.

* Kimlik doğrulaması için bir abonelik anahtarı kullanıyorsanız, bunun nedeni aşağıdakilerden biri olabilir:

    - Abonelik anahtarı eksik veya geçersiz
    - Aboneliğinizin kullanım kotası aşıldı

* Bir yetkilendirme belirteci kimlik doğrulaması için kullanılıyorsa, nedeni aşağıdakilerden biri olabilir:

    - Yetkilendirme belirteci geçersiz
    - Yetkilendirme belirtecinin süresi doldu

### <a name="validate-your-subscription-key"></a>Abonelik anahtarınızı doğrula

Aşağıdaki komutlardan birini çalıştırarak bir geçerli abonelik anahtara sahip olduğundan emin olmak doğrulayabilirsiniz.

> [!NOTE]
> Değiştir `YOUR_SUBSCRIPTION_KEY` ve `YOUR_REGION` abonelik anahtarı ve ilgili bölge sırasıyla.

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

Kimlik doğrulaması için bir yetki belirteci kullanıyorsanız, yetkilendirme belirtecini hala geçerli olduğunu doğrulamak için aşağıdaki komutlardan birini çalıştırın. Belirteçleri 10 dakika için geçerlidir.

> [!NOTE]
> Değiştir `YOUR_AUDIO_FILE` önceden kaydedilmiş ses dosyanızın yolu ile `YOUR_ACCESS_TOKEN` önceki adımda döndürülen yetkilendirme belirteci ile ve `YOUR_REGION` doğru bölgesiyle.

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

## <a name="error-http-400-bad-request"></a>hata `HTTP 400 Bad Request`

Bu hata genellikle istek gövdesinde geçersiz ses veri içerdiğinde oluşur. Yalnızca `WAV` biçimi desteklenir. Ayrıca uygun belirtme emin olmak için isteğin üstbilgileri kontrol `Content-Type` ve `Content-Length`.

## <a name="error-http-408-request-timeout"></a>hata `HTTP 408 Request Timeout`

Hatanın en olası nedeni hiçbir ses verisi hizmete gönderilen. Bu hata Ayrıca ağ sorunları neden olabilir.

## <a name="the-recognitionstatus-in-the-response-is-initialsilencetimeout"></a>`RecognitionStatus` Yanıt `InitialSilenceTimeout`

Ses verileri, genellikle soruna neden nedenidir. Örneğin:

* Uzun bir Uzat sessizlik ses başındaki yoktur. Hizmet birkaç saniye sonra durdurun ve dönüş `InitialSilenceTimeout`.
* Ses sessizlik kabul edilmesi ses verilerini neden olan bir desteklenmeyen codec biçimi kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Sürüm notları](releasenotes.md)

