---
title: Azure Media Services için kimlik doğrulama belirteçlerini geçirme | Microsoft Docs
description: Kimlik doğrulama belirteçleri için Azure Media Services anahtar dağıtımı hizmetiyle istemciden göndereceğinizi öğrenin
services: media-services
keywords: İçerik koruma, DRM, belirteç kimlik doğrulaması
documentationcenter: ''
author: dbgeorge
manager: jasonsue
editor: ''
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: dwgeo
ms.openlocfilehash: 71925a1ee67956df45901950b2a59fa4c1b458a7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61463234"
---
# <a name="learn-how-clients-pass-tokens-to-the-azure-media-services-key-delivery-service"></a>İstemciler için Azure Media Services anahtar dağıtımı hizmetiyle belirteçlerini nasıl geçirme öğrenin
Müşteriler genellikle player anahtarı edinebilirsiniz için nasıl bir oynatıcı belirteçleri doğrulama için Azure Media Services anahtar dağıtımı hizmetiyle geçirebilirsiniz isteyin. Media Services basit web belirteci (SWT) destekleyen ve JSON Web Token (JWT) biçimlendirir. Belirteç kimlik doğrulama anahtarı, ortak şifreleme veya Gelişmiş Şifreleme Standardı (AES) Zarf şifreleme sistemde kullanmadığınıza bakılmaksızın herhangi bir türde uygulanır.

 Player ve hedef platforma bağlı olarak, aşağıdaki yollarla, player ile belirteç geçirebilirsiniz:

- HTTP yetkilendirme üst bilgisi ile.
    > [!NOTE]
    > OAuth 2.0 özellikleri "Bearer" öneki bekleniyor. Bir örnek oynatıcı belirteç yapılandırma ile Azure Media Player barındırılan [tanıtım sayfasını](https://ampdemo.azureedge.net/). Video kaynağı koymak için **AES (JWT belirteci)** veya **AES (SWT belirteci)**. Belirteci yetkilendirme üst bilgisi geçirilir.

- Bir URL ek sorgu parametresi ile "belirteci tokenvalue =."  
    > [!NOTE]
    > "Bearer" öneki beklenen değil. Belirteç bir URL üzerinden gönderildiğinden, belirteç dizesini armor gerekir. İşte bir C# nasıl yapılacağını gösteren bir kod örneği:

    ```csharp
    string armoredAuthToken = System.Web.HttpUtility.UrlEncode(authToken);
    string uriWithTokenParameter = string.Format("{0}&token={1}", keyDeliveryServiceUri.AbsoluteUri, armoredAuthToken);
    Uri keyDeliveryUrlWithTokenParameter = new Uri(uriWithTokenParameter);
    ```

- CustomData alan.
Bu seçenek yalnızca, PlayReady lisans edinme CustomData alanın PlayReady lisans edinme sınama kullanılır. Bu durumda, belirteç içinde XML belgesi burada açıklandığı gibi olmalıdır:

    ```xml
    <?xml version="1.0"?>
    <CustomData xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyCustomData/v1"> 
        <Token></Token> 
    </CustomData>
    ```
    Kimlik doğrulama belirtecinizi belirteci öğesinde yerleştirin.

- Alternatif bir HTTP canlı akışı (HLS) çalma. AES + HLS için belirteci kimlik doğrulamasını yapılandırmak ihtiyacınız varsa kayıttan yürütme iOS/Safari, belirteçte doğrudan gönderebilirsiniz bir yolu yoktur. Bu senaryoyu etkinleştirmek için çalma listesi alternatif konusunda daha fazla bilgi için bkz. Bu [blog gönderisi](https://azure.microsoft.com/blog/2015/03/06/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]