---
title: Media Services ve Apple FairPlay lisansı desteği - Azure | Microsoft Docs
description: Bu konu, gereksinimler bir Apple FairPlay lisansı genel bir bakış sağlar ve yapılandırma.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 6d4b7ba842d08723b90a4f2491d9e79e68dd932e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60733581"
---
# <a name="apple-fairplay-license-requirements-and-configuration"></a>Apple FairPlay lisansı gereksinimleri ve yapılandırma 

Azure Media Services ile HLS içeriğinizin şifrelemenizi sağlar **Apple FairPlay** (AES-128 CBC). Media Services için FairPlay lisansları teslim etmek üzere bir hizmet de sağlar. Bir oynatıcı FairPlay ile korunan içeriğinizi oynatma çalıştığında, bir lisans almak için bir istek için lisans teslimat hizmetinin gönderilir. Lisans hizmeti isteği onaylarsa, istemciye gönderilen ve şifresini çözmek ve belirtilen içeriğin yürütmek için kullanılan lisans verir.

Media Services FairPlay lisanslarınızı yapılandırmak için kullanabileceğiniz API'ler de sağlar. Bu konuda FairPlay lisansı gereksinimleri açıklanır ve nasıl yapılandırılacağını gösteren bir **FairPlay** medya hizmetler API'lerini kullanarak lisanslayın. 

## <a name="requirements"></a>Gereksinimler

HLS içeriğinizin ile şifrelemek için Media Services'ı kullanırken, aşağıdaki gerekli **Apple FairPlay** ve Media Services FairPlay lisansları vermek için kullanın:

* Kaydolmak için [Apple geliştirme programı](https://developer.apple.com/).
* Apple gerektirir edinmek üzere içerik sahibi [dağıtım paketi](https://developer.apple.com/contact/fps/). Media Services ile anahtar güvenlik modülü (KSM) zaten uygulanmış ve son FPS paket isteyen durumu. Sertifika oluşturma ve uygulama gizli anahtarı (ASK) elde etmek amacıyla son FPS pakete yönergeler de vardır. ASK FairPlay yapılandırmak için kullanın.
* Media Services anahtar/lisans teslim tarafında aşağıdakiler ayarlanmalıdır:

    * **App Cert (AC)**: Bu özel anahtarı içeren .pfx dosyasıdır. Bu dosya oluşturur ve bir parola ile şifrelemek. .Pfx dosyasını Base64 biçiminde olmalıdır.

        Aşağıdaki adımları HLS için FairPlay bir .pfx sertifika dosyası oluşturmayı açıklar:

        1. OpenSSL yüklemek https://slproweb.com/products/Win32OpenSSL.html.

            FairPlay sertifika ve Apple tarafından sunulan diğer dosyaları olduğu klasöre gidin.
        2. Komut satırından aşağıdaki komutu çalıştırın. Bu, bir .pem dosyasına .cer dosyasını dönüştürür.

            "C:\OpenSSL-Win32\bin\openssl.exe" x509-der bildirmek-FairPlay.cer içinde-FairPlay out.pem çıkış
        3. Komut satırından aşağıdaki komutu çalıştırın. Bu .pem dosyası özel anahtara sahip bir .pfx dosyasına dönüştürür. .Pfx dosyası için parolayı ardından OpenSSL tarafından istenir.

            "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-dışarı aktarma - FairPlay out.pfx-inkey privatekey.pem-FairPlay out.pem - passin file:privatekey-pem-pass.txt içinde
            
    * **App Cert parola**: .Pfx dosyasını oluşturmak için parola.
    * **SORUN**: Apple Geliştirici Portalı'nı kullanarak sertifika oluşturduğunuzda, bu anahtarı alınır. Her geliştirme ekibi bir benzersiz ASK alır. ASK bir kopyasını kaydedin ve güvenli bir yerde saklayın. Media Services ile FairPlayAsk olarak ASK yapılandırmanız gerekir.
    
* FPS istemci tarafından aşağıdakiler ayarlanmalıdır:

  * **App Cert (AC)**: Bu işletim sisteminin bazı yükü şifrelemek için kullandığı ortak anahtar içeren bir.cer/.der dosyasıdır. Media Services player tarafından gerekli olduğu için bu konuda bilmesi gerekir. Anahtar dağıtımı hizmetiyle ilişkili özel anahtarı kullanarak şifresini çözer.

* FairPlay şifrelenmiş stream kayıttan yürütme için gerçek ASK ilk alın ve ardından gerçek bir sertifika oluşturun. Bu işlem, tüm üç bölümü oluşturur:

  * .DER dosya
  * .pfx dosyası
  * .pfx için parolayı

## <a name="fairplay-and-player-apps"></a>FairPlay ve oynatıcı uygulamaları

Ne zaman içeriğinizi bilgilerle şifreli **Apple FairPlay**, tek tek video ve ses örnekleri kullanılarak şifrelenmiş **AES-128 CBC** modu. **FairPlay Streaming** (FPS), iOS ve Apple TV yerel desteğiyle, cihaz işletim sistemlerinin bütünleştirilmiştir. OS X üzerinde Safari şifreli medya Uzantıları (EME) arabirimi desteğini kullanarak FPS sağlar.

Azure Media Player ayrıca FairPlay kayıttan yürütme destekler. Daha fazla bilgi için [Azure Media Player belgeleri](https://amp.azure.net/libs/amp/latest/docs/index.html).

İOS SDK'sını kullanarak kendi oynatıcı uygulamaları geliştirebilirsiniz. FairPlay içeriğini oynatmak lisans exchange protokolünü uygulayan gerekir. Bu protokol, Apple tarafından belirtilmedi. Bu anahtar teslim istekleri göndermek nasıl her uygulamanın tercihine bağlı olur. Medya Hizmetleri FairPlay anahtar dağıtımı hizmetiyle aşağıdaki biçimde bir www-form-url kodlanmış posta iletisi gelmesini SPC bekliyor:

```
spc=<Base64 encoded SPC>
```

## <a name="fairplay-configuration-net-example"></a>FairPlay yapılandırması .NET örneği

Media Services API'sine FairPlay lisansı yapılandırmak için kullanabilirsiniz. Oyuncu FairPlay ile korunan içeriğinizi oynatma çalıştığında, bir istek için lisans teslimat hizmetinin lisansı edinmek için gönderilir. Lisans hizmeti isteği onaylarsa, hizmet lisansı verir. Bu, istemciye gönderilen ve şifresini çözmek ve belirtilen içeriğin yürütmek için kullanılır.

> [!NOTE]
> Genellikle, yalnızca bir sertifika ve bir dizi sahip olacağından FairPlay ilkesi seçenekleri yalnızca bir kez yapılandırmak istiyorsunuz.

Aşağıdaki örnekte [Media Services .NET SDK'sı](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models?view=azure-dotnet) lisans yapılandırmak için.

```csharp
private static ContentKeyPolicyFairPlayConfiguration ConfigureFairPlayPolicyOptions()
{

    string askHex = "";
    string FairPlayPfxPassword = "";

    var appCert = new X509Certificate2("FairPlayPfxPath", FairPlayPfxPassword, X509KeyStorageFlags.Exportable);

    byte[] askBytes = Enumerable
        .Range(0, askHex.Length)
        .Where(x => x % 2 == 0)
        .Select(x => Convert.ToByte(askHex.Substring(x, 2), 16))
        .ToArray();

    ContentKeyPolicyFairPlayConfiguration fairPlayConfiguration =
    new ContentKeyPolicyFairPlayConfiguration
    {
        Ask = askBytes,
        FairPlayPfx =
                Convert.ToBase64String(appCert.Export(X509ContentType.Pfx, FairPlayPfxPassword)),
        FairPlayPfxPassword = FairPlayPfxPassword,
        RentalAndLeaseKeyType =
                ContentKeyPolicyFairPlayRentalAndLeaseKeyType
                .PersistentUnlimited,
        RentalDuration = 2249
    };

    return fairPlayConfiguration;
}
```

## <a name="next-steps"></a>Sonraki adımlar

İşlemlerini nasıl gerçekleştirebileceğinizi [DRM ile koruma](protect-with-drm.md)
