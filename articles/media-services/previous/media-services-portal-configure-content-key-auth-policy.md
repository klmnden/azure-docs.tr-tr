---
title: Azure portalını kullanarak bir içerik anahtarı yetkilendirme ilkesini yapılandırma | Microsoft Docs
description: İçerik anahtarının yetkilendirme ilkesini yapılandırma hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.assetid: ee82a3fa-c34b-48f2-a108-8ba321f1691e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 33b958b97a5883d585bbfda167db35107c0c5997
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="configure-a-content-key-authorization-policy"></a>Bir içerik anahtarı yetkilendirme ilkesini yapılandırma
[!INCLUDE [media-services-selector-content-key-auth-policy](../../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Genel Bakış
 Gelişmiş Şifreleme Standardı (AES ile) 128-bit şifreleme anahtarları kullanılarak korumalı MPEG-DASH, kesintisiz akış ve HTTP canlı akışı (HLS) akışlar sunmanıza olanak Azure Media Services kullanabilirsiniz veya [PlayReady dijital hak yönetimi (DRM)](https://www.microsoft.com/playready/overview/). Media Services ile Widevine DRM ile şifrelenmiş DASH akışları sunabilir. PlayReady ve Widevine, ortak şifreleme (ISO/IEC 23001-7 CENC) belirtimi uyarınca şifrelenir.

Media Services kendisinden şifrelenmiş içerik yürütmek için PlayReady/Widevine lisansları veya AES anahtarları istemcileri elde edebilirsiniz bir anahtar/lisans teslimat hizmeti de sağlar.

Bu makalede Azure portal içerik anahtarı yetkilendirme ilkesini yapılandırmak için nasıl kullanılacağı gösterilmektedir. Anahtar, daha sonra dinamik olarak içeriğinizi şifrelemek için de kullanılabilir. Şu anda, HLS, MPEG DASH ve kesintisiz akış biçimleri şifreleyebilirsiniz. Aşamalı indirme şifrelenemiyor.

Bir oyuncu dinamik olarak şifrelenmesini şekilde ayarlanmış bir akış istediğinde, Media Services yapılandırılmış anahtar dinamik olarak içeriğinizi AES veya DRM şifreleme kullanarak şifrelemek için kullanır. Akış şifresini çözmek için player anahtar anahtar teslim hizmetinden ister. Kullanıcı anahtarı alınamadı yetkilendirilip yetkilendirilmediğini belirlemek için hizmet anahtar için belirtilen yetkilendirme ilkelerini değerlendirir.

Birden çok içerik anahtarı varsa veya bir anahtar/lisans teslimat hizmeti URL'si Media Services anahtar teslim hizmeti dışında belirtmek istiyorsanız planlıyorsanız, Media Services .NET SDK veya REST API'lerini kullanın. Daha fazla bilgi için bkz.

* [Media Services .NET SDK kullanarak bir içerik anahtarı yetkilendirme ilkesini yapılandırma](media-services-dotnet-configure-content-key-auth-policy.md)
* [Media Services REST API kullanarak bir içerik anahtarı yetkilendirme ilkesini yapılandırma](media-services-rest-configure-content-key-auth-policy.md)

### <a name="some-considerations-apply"></a>Bazı önemli noktalar geçerlidir
* Media Services hesabınız oluşturulduğunda hesabınıza “Durdurulmuş” durumda bir varsayılan akış uç noktası eklenir. İçeriğinizi akış başlatmak ve dinamik paketleme ve dinamik şifreleme yararlanmak için akış uç noktanızı "Çalışır" durumda olması gerekir. 
* Varlığınızı Uyarlamalı bit hızlı MP4s ya da Uyarlamalı bit hızlı kesintisiz akış dosyaları içermelidir. Daha fazla bilgi için bkz: [bir varlık kodla](media-services-encode-asset.md).
* Anahtar teslim hizmeti ContentKeyAuthorizationPolicy ve ilişkili nesnelerini (ilkesi seçenekleri ve kısıtlamaları) 15 dakika için önbelleğe alır. Bir ContentKeyAuthorizationPolicy oluşturun ve bir belirteç kısıtlamasına kullanmak için test ve ardından ilkeyi açık kısıtlama güncelleştirmek için belirtin. Bu işlem açık bir sürümüne ilke anahtarları önce yaklaşık 15 dakika sürer.
* Akış uç noktası bir Media Services, joker karakter olarak denetim öncesi yanıt CORS Access-Control-Allow-Origin üstbilgisinin değerini ayarlar "\*". Bu değer Azure Media Player, Roku ve JWPlayer ve diğerleri de dahil olmak üzere de çoğu oyuncularla, çalışır. Ancak, "içerecek şekilde" kimlik bilgileri moduyla, kendi dash.js XMLHttpRequest joker izin vermediği için dash.js kullanan bazı oynatıcıları çalışmıyor "\*" Access-Control-Allow-Origin değeri olarak. Tek bir etki alanı istemcinizden barındırıyorsanız dash.js içinde bu sınırlamaya geçici Media Services ön yanıt üstbilgisinde bu etki alanı belirtebilirsiniz. Yardım için destek bileti Azure portalı üzerinden açın.

## <a name="configure-the-key-authorization-policy"></a>Anahtarı yetkilendirme ilkesini yapılandırın
Anahtar yetkilendirme ilkesi yapılandırmak için seçin **içerik koruma** sayfa.

Media Services, anahtar isteğinde kullanıcıların kimliklerini doğrulamak için birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesini açık olabilir belirteci ya da IP yetkilendirme kısıtlamaları. (IP REST veya .NET SDK'sı ile yapılandırılabilir.)

### <a name="open-restriction"></a>Açık kısıtlama
Açık sınırlama Sistem anahtarı anahtar istekte herkes sunar anlamına gelir. Bu kısıtlama sınama amacıyla yararlı olabilir.

![OpenPolicy][open_policy]

### <a name="token-restriction"></a>Belirteç kısıtlama
Belirteç kısıtlamalı ilkenin seçmek için Seç **BELİRTECİ** düğmesi.

Belirteç kısıtlamalı ilkenin, bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç tarafından eklenmelidir. Media Services basit bir web belirteç belirteçleri destekler ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) ve JSON Web Token (JWT) biçimlendirir. Daha fazla bilgi için bkz: [JWT kimlik doğrulama](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media Services STS sağlamaz. Özel bir STS oluşturun veya Azure erişim denetimi hizmeti sorunu belirteçleri kullanın. STS, belirteç kısıtlamasına yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri imzalı bir belirteç oluşturmak için yapılandırılmalıdır. Belirtecin geçerli olduğu ve içerik anahtarı için yapılandırılmış talep belirteci eşleştiğinden, Media Services anahtar teslim hizmeti istemcisi için şifreleme anahtarını döndürür. Daha fazla bilgi için bkz: [kullanım Azure erişim denetimi hizmeti sorunu belirteçleri için](http://mingfeiy.com/acs-with-key-services).

Belirteç kısıtlanmış İlkesi yapılandırdığınızda, birincil doğrulama anahtarı, veren ve İzleyici parametreleri belirtmeniz gerekir. Birincil doğrulama anahtar belirteci ile imzalandığı anahtarı içerir. Verici belirtecini veren STS ' dir. Kaynak belirteci erişimini yetkilendirir veya (bazen kapsam denir) İzleyici belirteç amacı açıklanır. Media Services anahtar teslim hizmeti, bu değerleri belirteci şablon değerleri eşleştiğini doğrular.

### <a name="playready"></a>PlayReady
İçeriğinizi PlayReady ile koruduğunuzda, Yetkilendirme ilkesinde belirtmek için gereken şeyleri PlayReady lisans şablonu tanımlayan bir XML dizesi biridir. Varsayılan olarak, aşağıdaki İlkesi ayarlanır:

    <PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
          <LicenseTemplates>
            <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices>
              <ContentKey i:type="ContentEncryptionKeyFromHeader" />
              <LicenseType>Nonpersistent</LicenseType>
              <PlayRight>
                <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput>
              </PlayRight>
            </PlayReadyLicenseTemplate>
          </LicenseTemplates>
        </PlayReadyLicenseResponseTemplate>

Seçebileceğiniz **ilke XML'yi içe** düğmesini tıklatın ve içinde tanımlanan XML Şeması uyan farklı bir XML sağlamak [Media Services PlayReady lisans şablonu genel bakış](media-services-playready-license-template-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

