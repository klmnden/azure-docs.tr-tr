---
title: Öbekli aktarım ses Stream nasıl | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Bing konuşma hizmeti için ses akışı göndermek için öbekli trasfer kullanma
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: d9796cf60e2695c21d781131c935d24891401efa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60514997"
---
# <a name="chunked-transfer-encoding"></a>Öbekli aktarım kodlaması

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Konuşmayı metne dönüştürme özelliği için Microsoft konuşma tanıma API'si, ses bir tam öbek göndermek veya ses küçük öbeklere kesme için sağlar. Verimli bir ses akışı ve döküm gecikme süresini azaltmak için kullanmanız önerilir [öbekli aktarım kodlamasını](https://en.wikipedia.org/wiki/Chunked_transfer_encoding) hizmetine ses akışı. Diğer uygulamalar, daha yüksek gecikme kullanıcı algılanan neden olabilir. Daha fazla bilgi için [ses akışları](../concepts.md#audio-streams) sayfası.

> [!NOTE]
> 10 saniyeden fazla herhangi bir istek ses karşıya değil ve toplam istek süresi 14 saniyeden uzun olamaz.
> [!NOTE]
> Yalnızca kullanırsanız öbekli aktarım kodlamasını belirtmeniz gereken [REST API'leri](../GetStarted/GetStartedREST.md) konuşma hizmeti çağırmak için. Kullanan uygulamalar [istemci kitaplıkları](../GetStarted/GetStartedClientLibraries.md) öbekli aktarım kodlamasını yapılandırma gerekmez.

Aşağıdaki kod, öbekli aktarım kodlamasını ayarlayın ve 1024 bayt öbeklere öbekli bir ses dosyası göndermek için nasıl gösterir.

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
