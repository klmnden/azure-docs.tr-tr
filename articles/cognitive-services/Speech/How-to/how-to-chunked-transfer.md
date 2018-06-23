---
title: Öbekli aktarım ses akışı nasıl | Microsoft Docs
description: Öbekli trasfer ses akışı konuşma hizmete göndermek için nasıl kullanılacağını
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 7d02340932dfc547893c4c40cbe08978b7b93756
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352169"
---
# <a name="chunked-transfer-encoding"></a>Öbekli aktarım kodlaması

Metin konuşma transcribe için Microsoft konuşma tanıma API'si bir tam öbek ses göndermeye veya ses küçük parçalara kesme sağlar. Verimli ses akış ve transcription gecikme için kullanmanız önerilir [öbekli aktarım kodlamasını](https://en.wikipedia.org/wiki/Chunked_transfer_encoding) hizmetine ses akışını sağlamak için. Diğer uygulamalardan kullanıcı algılanan daha yüksek gecikme neden olabilir. Daha fazla bilgi için bkz: [ses akışları](../concepts.md#audio-streams) sayfası.

> [!NOTE]
> 10 saniyeden fazla herhangi bir istek ses karşıya değil ve toplam istek süresi 14 saniye aşamaz.
> [!NOTE]
> Öbekli aktarım yalnızca kullanıyorsanız kodlaması belirtmek zorunda [REST API'leri](../GetStarted/GetStartedREST.md) konuşma hizmetini çağırmak için. Kullanan uygulamalar [istemci kitaplıkları](../GetStarted/GetStartedClientLibraries.md) öbekli aktarım kodlamasını yapılandırma gerekmez.

Aşağıdaki kod öbekli aktarım kodlamasını ayarlamak ve 1024 bayt parçalara öbekli bir ses dosyası göndermek için nasıl gösterir.

```cs

HttpWebRequest request = null;
request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.Host = @"speech.platform.bing.com";
request.ContentType = @"audio/wav; codec=audio/pcm; samplerate=16000";
request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR_SUBSCRIPTION_KEY";

using (fs = new FileStream(audioFile, FileMode.Open, FileAccess.Read))
{

    /*
    * Open a request stream and write 1024 byte chunks in the stream one at a time.
    */
    byte[] buffer = null;
    int bytesRead = 0;
    using (Stream requestStream = request.GetRequestStream())
    {
        /*
        * Read 1024 raw bytes from the input audio file.
        */
        buffer = new Byte[checked((uint)Math.Min(1024, (int)fs.Length))];
        while ((bytesRead = fs.Read(buffer, 0, buffer.Length)) != 0)
        {
            requestStream.Write(buffer, 0, bytesRead);
        }

        // Flush
        requestStream.Flush();
    }
}
```
