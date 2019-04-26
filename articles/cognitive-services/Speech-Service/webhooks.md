---
title: Web kancaları - konuşma Hizmetleri
titlesuffix: Azure Cognitive Services
description: Web kancaları HTTP telefonla geri arama çözümünüzü uzun ilgilenirken iyileştirmek için ideal olan çalışan işlemleri içeri aktarmalar, uyarlaması, doğruluk testi veya uzun süre çalışan dosyalarının döküm gibi.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 3ceaed2b1e27a1f5b910865f6e9d0e70ef347b71
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60515386"
---
# <a name="webhooks-for-speech-services"></a>Konuşma Hizmetleri için Web kancaları

Web kancaları, uygulamanızın, kullanılabilir olduğunda konuşma Hizmetleri verileri kabul etmek HTTP geri çağırmalarıdır gibi ' dir. Web kancalarını kullanan, yanıt sürekli yoklama yapma ihtiyacını ortadan kaldırarak REST Apı'lerimizi kullanımını iyileştirebilirsiniz. Sonraki birkaç bölümde konuşma Hizmetleri ile Web kancalarını kullanma öğreneceksiniz.

## <a name="supported-operations"></a>Desteklenen işlemler

Web kancaları, konuşma Hizmetleri tüm uzun süre çalışan işlemler için destekler. Aşağıda listelenen işlemlerin her biri, bir HTTP geri çağırma işlemi tamamlandıktan sonra tetikleyebilirsiniz. 

* DataImportCompletion
* ModelAdaptationCompletion
* AccuracyTestCompletion
* TranscriptionCompletion
* EndpointDeploymentCompletion
* EndpointDataCollectionCompletion

Ardından, bir Web kancası oluşturalım.

## <a name="create-a-webhook"></a>Bir Web kancası oluştur

Çevrimdışı bir döküm için bir Web kancası oluşturalım. Senaryo: bir kullanıcının zaman uyumsuz olarak Batch tanıma API'SİYLE konuşmaların istediğiniz bir uzun süre çalışan ses dosyası. 

Bir web kancası POST https:// oluşturmak için<region>.cris.ai/api/speechtotext/v2.1/transcriptions/hooks

İstek için yapılandırma parametreleri JSON olarak sağlanır:

```json
{
  "configuration": {
    "url": "https://your.callback.url/goes/here",
    "secret": "<my_secret>"
  },
  "events": [
    "TranscriptionCompletion"
  ],
  "active": true,
  "name": "TranscriptionCompletionWebHook",
  "description": "This is a Webhook created to trigger an HTTP POST request when my audio file transcription is completed.",
  "properties": {
      "Active" : "True"
  }

}
```
Batch tanıma API'sine yapılan tüm POST isteklerinden gerektiren bir `name`. `description` Ve `properties` parametreler isteğe bağlıdır.

`Active` Özelliği, geri URL'nizi açıp silin ve Web kancası kaydını yeniden oluşturmak zorunda kalmadan çağırma geçiş yapmak için kullanılır. Yalnızca işlem sahip olduktan sonra geri kez tam çağırmak gerekiyorsa, anahtar ve Web kancası silme `Active` özelliğini false.

Olay türü `TranscriptionCompletion` olayları dizide sağlanır. Bir döküm terminal durumuna ulaştığı zaman geri uç noktanıza çağırır (`Succeeded` veya `Failed`). İstek geri kayıtlı URL'sine çağırırken içerecek bir `X-MicrosoftSpeechServices-Event` kayıtlı olay türlerinden birini içeren üstbilgi. Kayıtlı olay türüne göre bir istek var. 

Abone olunamıyor bir olay türü yoktur. Bu `Ping` olay türü. Bu tür bir istekle URL'ye ping URL (aşağıya bakın) kullanarak bir Web kancası oluşturma tamamlandığında gönderilir.  

Yapılandırma `url` özelliği gereklidir. POST istekleri bu URL'ye gönderilir. `secret` HMAC anahtarı gizli dizi ile yükü SHA256 karmasını oluşturmak için kullanılır. Karma olarak ayarlandığından `X-MicrosoftSpeechServices-Signature` kayıtlı URL'ye geri çağrılırken başlığı. Base64 kodlu başlığıdır.

Bu örnek, bir yük kullanarak doğrulamak verilmektedir C#:

```csharp

private const string EventTypeHeaderName = "X-MicrosoftSpeechServices-Event";
private const string SignatureHeaderName = "X-MicrosoftSpeechServices-Signature";

[HttpPost]
public async Task<IActionResult> PostAsync([FromHeader(Name = EventTypeHeaderName)]WebHookEventType eventTypeHeader, [FromHeader(Name = SignatureHeaderName)]string signature)
{
    string body = string.Empty;
    using (var streamReader = new StreamReader(this.Request.Body))
    {
        body = await streamReader.ReadToEndAsync().ConfigureAwait(false);
        var secretBytes = Encoding.UTF8.GetBytes("my_secret");
        using (var hmacsha256 = new HMACSHA256(secretBytes))
        {
            var contentBytes = Encoding.UTF8.GetBytes(body);
            var contentHash = hmacsha256.ComputeHash(contentBytes);
            var storedHash = Convert.FromBase64String(signature);
            var validated = contentHash.SequenceEqual(storedHash);
        }
    }
 
    switch (eventTypeHeader)
    {
        case WebHookEventType.Ping:
            // Do your ping event related stuff here (or ignore this event)
            break;
        case WebHookEventType.TranscriptionCompletion:
            // Do your subscription related stuff here.
            break;
        default:
            break;
    }
 
    return this.Ok();
}

```
Bu kod parçacığında `secret` çözülmüş ve doğrulandı. Ayrıca, Web kancası olay türü geçirildi görürsünüz. Şu anda tamamlanan döküm başına bir olay yok. Kod, vazgeçmeden önce (ile bir saniyelik bir gecikmenin) her olay için beş kez yeniden dener.

### <a name="other-webhook-operations"></a>Diğer Web kancası işlemleri

Tüm kayıtlı Web kancaları almak için: AL https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks

Belirli bir Web kancası almak için: AL https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id

Belirli bir Web kancası kaldırmak için: DELETE https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id

> [!Note] 
> Yukarıdaki örnekte, 'westus' bölgedir. Bu konuşma Hizmetleri kaynağınızı Azure portalında nerede oluşturduğunuz bölge tarafından değiştirilmelidir.

POST https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id/ping gövdesi: boş

Kayıtlı URL'sine bir POST isteği gönderir. İstek içeren bir `X-MicrosoftSpeechServices-Event` üst bilgi değeri ping ile. Web kancası ile bir gizli dizi kaydettiyseniz, içerdiği bir `X-MicrosoftSpeechServices-Signature` bir SHA256 karma akıştaki HMAC anahtar olarak gizli olan üstbilgiyle. Base64 kodlu karmasıdır. 

POST https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id/test gövdesi: boş

Abone olunan olay türü (döküm) için bir varlık sistemde mevcut olduğundan ve uygun durumda kayıtlı URL'sine bir POST isteği gönderir. Web kancası çağırdığınız son varlık yükü oluşturulur. Varlık mevcutsa POST 204 ile yanıt verecektir. Bir testi isteği yapılabilir, 200 ile yanıt verecektir. Web kancası (örneğin, döküm için) abone GET isteği belirli bir varlığa ait olduğu gibi aynı şeklin istek gövdesidir. İsteğinin `X-MicrosoftSpeechServices-Event` ve `X-MicrosoftSpeechServices-Signature` önce anlatıldığı gibi üst bilgiler.

### <a name="run-a-test"></a>Test çalıştırma

Hızlı bir testi yapılabilir Web sitesini kullanarak https://bin.webhookrelay.com. Burada, çağrı edinebilirsiniz belgenin önceki bölümlerinde açıklanan bir Web kancası oluşturmak için HTTP POST için parametre olarak geçirmek için URL'leri yedekleyin.

'Oluşturma demetinde' tıklayın ve izleyin ekrandaki bir kanca almak için yönergeler. Ardından kanca konuşma hizmeti sayesinde kaydetmek için bu sayfada sağlanan bilgileri kullanın. Geçiş yükü message - transkripsiyon görünüyor tamamlanması için yanıt şu şekilde:

```json
{
    "results": [],
    "recordingsUrls": [
        "my recording URL"
    ],
    "models": [
        {
            "modelKind": "AcousticAndLanguage",
            "datasets": [],
            "id": "a09c8c8b-1090-443c-895c-3b1cf442dec4",
            "createdDateTime": "2019-03-26T12:48:46Z",
            "lastActionDateTime": "2019-03-26T14:04:47Z",
            "status": "Succeeded",
            "locale": "en-US",
            "name": "v4.13 Unified",
            "description": "Unified",
            "properties": {
                "Purpose": "OnlineTranscription,BatchTranscription,LanguageAdaptation",
                "ModelClass": "unified-v4"
            }
        }
    ],
    "statusMessage": "None.",
    "id": "d41615e1-a60e-444b-b063-129649810b3a",
    "createdDateTime": "2019-04-16T09:35:51Z",
    "lastActionDateTime": "2019-04-16T09:38:09Z",
    "status": "Succeeded",
    "locale": "en-US",
    "name": "Simple transcription",
    "description": "Simple transcription description",
    "properties": {
        "PunctuationMode": "DictatedAndAutomatic",
        "ProfanityFilterMode": "Masked",
        "AddWordLevelTimestamps": "True",
        "AddSentiment": "True",
        "Duration": "00:00:02"
    }
}
```
Kayıt URL'si ve modelleri kaydetme özelliği kullanılan ileti içerir.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
