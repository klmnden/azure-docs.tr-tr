---
title: "Azure Media Services için kimlik doğrulama belirteçleri geçirmek | Microsoft Docs"
description: "Kimlik doğrulama belirteçleri istemciden Azure Media Services anahtar teslim hizmete göndermek öğrenin"
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
ms.openlocfilehash: 7d143242231444b8557a303d1b504d5311693f1a
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="learn-how-clients-pass-tokens-to-the-azure-media-services-key-delivery-service"></a>İstemcileri belirteçleri Azure Media Services anahtar teslim hizmete nasıl geçirmek öğrenin
Müşteriler genellikle player anahtarı edinebilirsiniz şekilde nasıl bir oynatıcı belirteçleri doğrulama için Azure Media Services anahtar teslim hizmeti geçirebilirsiniz isteyin. JSON Web Token (JWT) biçimleri ve Media Services basit web token (SWT) destekler. Belirteç kimlik doğrulama anahtarı, ortak şifreleme veya Gelişmiş Şifreleme Standardı (AES) Zarf şifreleme sistemde kullansanız bağımsız olarak her türlü uygulanır.

 Player ve hedef platformu bağlı olarak, aşağıdaki yollarla player ile belirteç geçirebilirsiniz:

- HTTP Authorization Üstbilgisi.
    > [!NOTE]
    > "Bearer" öneki OAuth 2.0 belirtimlerin beklenir. Belirteç yapılandırma ile bir örnek oynatıcı üzerinde Azure Media Player barındırılan [gösteri sayfası](http://ampdemo.azureedge.net/). Video kaynağı ayarlamak için seçin **AES (JWT belirteci)** veya **AES (SWT belirteci)**. Belirteç Authorization Üstbilgisi geçirilir.

- Bir URL eklenmesi sorgu parametresi ile "token tokenvalue =."  
    > [!NOTE]
    > "Bearer" öneki beklenen değil. Belirtecin bir URL yoluyla gönderildiğinden, belirteç dizesini koruma sağlamak gerekir. Nasıl yapılacağını gösteren bir C# örnek kod aşağıda verilmiştir:

    ```csharp
    string armoredAuthToken = System.Web.HttpUtility.UrlEncode(authToken);
    string uriWithTokenParameter = string.Format("{0}&token={1}", keyDeliveryServiceUri.AbsoluteUri, armoredAuthToken);
    Uri keyDeliveryUrlWithTokenParameter = new Uri(uriWithTokenParameter);
    ```

- CustomData alan.
Bu seçenek yalnızca, PlayReady lisans edinme PlayReady lisans edinme sınama CustomData alan kullanılır. Bu durumda, belirteç içinde XML belgesi burada açıklandığı gibi olmalıdır:

    ```xml
    <?xml version="1.0"?>
    <CustomData xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyCustomData/v1"> 
        <Token></Token> 
    </CustomData>
    ```
    Kimlik doğrulama belirteci belirteci öğesinde yerleştirin.

- Alternatif bir HTTP canlı akışı (HLS) çalma. AES + HLS belirteci kimlik doğrulamasını yapılandırmak gereken kayıttan yürütme iOS/üzerinde Safari, belirteçte doğrudan gönderebileceğiniz bir yolu yoktur. Bu senaryoyu etkinleştirmek için çalma alternatif hakkında daha fazla bilgi için bkz [blog gönderisi](http://azure.microsoft.com/blog/2015/03/06/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]