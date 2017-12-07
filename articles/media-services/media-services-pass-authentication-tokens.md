---
title: "Azure Media Services için kimlik doğrulama belirtecin geçip | Microsoft Docs"
description: "Kimlik doğrulama belirteçleri istemciden Azure Media Services anahtar teslim hizmetine göndermek öğrenin"
services: media-services
keywords: "İçerik koruma, DRM, belirteç kimlik doğrulama"
documentationcenter: 
author: dbgeorge
manager: jasonsue
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: dwgeo
ms.openlocfilehash: 0e56726266898e5738dd797a8a019987d457170e
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="how-clients-pass-tokens-to-azure-media-services-key-delivery-service"></a>İstemcileri belirteçleri için Azure Media Services anahtar teslim hizmeti nasıl geçirin
Biz sürekli bir oynatıcı doğrulandı bizim anahtar teslim hizmetleri için nasıl belirteci geçirebilirdiniz geçici soruları almak ve player anahtarı edinir. Basit Web Token (SWT) ve JSON Web Token (JWT) bu iki belirteç biçimleri destekliyoruz. Belirteç kimlik doğrulama anahtarı herhangi bir türde uygulanabilir – bakılmaksızın sistemde ortak şifreleme veya AES zarfı şifrelemesi yapma.

Bu belirteç player ile geçirebilirdiniz aşağıdaki yöntemlerle, player ve platformu, hedeflediğiniz bağlıdır:
- HTTP Authorization Üstbilgisi.
> [!NOTE]
> "Bearer" öneki OAuth 2.0 belirtimlerin beklenir unutmayın. Azure Media Player barındırılan belirteci yapılandırmasına sahip bir örnek oynatıcı yoktur [gösteri sayfası](http://ampdemo.azureedge.net/). Lütfen AES (JWT belirteci) veya AES (SWT belirteç) video kaynağı ayarlamak için seçin. Belirteç Authorization Üstbilgisi geçirilir.

- Url sorgu parametresi ile ekleyerek aracılığıyla "token tokenvalue =".  
> [!NOTE]
> "Bearer" önek beklenir unutmayın. Simge bir URL aracılığıyla gönderilir olduğundan, belirteç dizesini koruma sağlamak gerekir. Bunun nasıl yapılacağı C# örnek kod şöyledir:

```csharp
    string armoredAuthToken = System.Web.HttpUtility.UrlEncode(authToken);
    string uriWithTokenParameter = string.Format("{0}&token={1}", keyDeliveryServiceUri.AbsoluteUri, armoredAuthToken);
    Uri keyDeliveryUrlWithTokenParameter = new Uri(uriWithTokenParameter);
```

- CustomData alanı.
İçin PlayReady lisans edinme yalnızca, PlayReady lisans edinme sınama CustomData alan. Bu durumda, belirteç aşağıda açıklanan xml belgesi içinde olması gerekir.

```xml
    <?xml version="1.0"?>
    <CustomData xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyCustomData/v1"> 
        <Token></Token> 
    </CustomData>
```
Kimlik doğrulama belirteci içine <Token> öğesi.

- Bir alternatif HLS çalma listesi. AES + HLS için belirteci kimlik doğrulamasını yapılandırmak gereken kayıttan yürütme iOS/Safari üzerinde doğrudan gönderdiğiniz belirteçte bir yolu yoktur. Lütfen bu bakın [blog gönderisi](http://azure.microsoft.com/blog/2015/03/06/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/) bu senaryoyu etkinleştirmek için çalma alternatif konusunda.

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]