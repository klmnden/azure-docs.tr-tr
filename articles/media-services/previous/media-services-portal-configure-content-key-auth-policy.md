---
title: Azure portalını kullanarak bir içerik anahtarı yetkilendirme ilkesini yapılandırma | Microsoft Docs
description: Bir içerik anahtarı yetkilendirme ilkesini yapılandırmayı öğrenin.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: ee82a3fa-c34b-48f2-a108-8ba321f1691e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: b046ce5a8647abe601a6327667241d98445ce1e4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61130582"
---
# <a name="configure-a-content-key-authorization-policy"></a>Bir içerik anahtarı yetkilendirme ilkesini yapılandırma
[!INCLUDE [media-services-selector-content-key-auth-policy](../../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Genel Bakış
 Azure Media Services, 128 bit şifreleme anahtarları kullanılarak Gelişmiş Şifreleme Standardı (AES ile) korumalı MPEG-DASH, kesintisiz akış ve HTTP canlı akışı (HLS) akışlar sunmanıza olanak kullanabilirsiniz veya [PlayReady dijital hak yönetimi (DRM)](https://www.microsoft.com/playready/overview/). Media Services ile Widevine DRM ile şifrelenmiş DASH akışları teslim edebilirsiniz. PlayReady ve Widevine, ortak şifreleme (ISO/IEC 23001-7 CENC) belirtimi uyarınca şifrelenir.

Media Services, kendisinden şifrelenmiş içeriği yürütmek için PlayReady/Widevine lisansları veya AES anahtarları istemciler elde edebilirsiniz bir anahtar/lisans teslim hizmeti de sağlar.

Bu makalede, içerik anahtarı yetkilendirme ilkesini yapılandırmak için Azure portalını kullanma gösterilmektedir. Anahtar, daha sonra dinamik olarak, içeriği şifrelemek için de kullanılabilir. Şu anda, HLS, MPEG-DASH ve kesintisiz akış biçimlerinde şifreleyebilirsiniz. İlerlemeli indirmeler şifrelenemiyor.

Media Services, yapılandırılmış olan anahtar bir oynatıcı dinamik olarak şifrelenmesini ayarlanmış bir akış istediğinde, dinamik olarak içeriğinizi AES veya DRM şifreleme kullanarak şifrelemek için kullanır. Oynatıcı, akışın şifresini çözmek için anahtar teslim hizmetinden anahtarı ister. Kullanıcı anahtarı almak için yetki verilip verilmediğini belirlemek için anahtar için belirtilen Yetkilendirme İlkeleri hizmet tarafından değerlendirilir.

Birden çok içerik anahtarı veya Media Services anahtar dağıtımı hizmetiyle dışında bir anahtar/lisans teslim hizmeti URL'si belirtin isterseniz planlıyorsanız, Media Services .NET SDK veya REST API'leri kullanın. Daha fazla bilgi için bkz.

* [Media Services .NET SDK'sını kullanarak bir içerik anahtarı yetkilendirme ilkesini yapılandırma](media-services-dotnet-configure-content-key-auth-policy.md)
* [Media Services REST API kullanarak bir içerik anahtarı yetkilendirme ilkesini yapılandırma](media-services-rest-configure-content-key-auth-policy.md)

### <a name="some-considerations-apply"></a>Bazı önemli noktalar geçerlidir
* Media Services hesabınız oluşturulduğunda hesabınıza “Durdurulmuş” durumda bir varsayılan akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için akış uç noktanızı "Çalışıyor" durumunda olması gerekir. 
* Varlığınız bir dizi hızı Uyarlamalı MP4 veya uyarlamalı bit hızlı kesintisiz akış dosyaları içermelidir. Daha fazla bilgi için [bir varlığı kodlama](media-services-encode-asset.md).
* Anahtar dağıtımı hizmetiyle ContentKeyAuthorizationPolicy ve ilgili nesneleri (ilkesi seçenekleri ve kısıtlamaları) 15 dakika için önbelleğe alır. Bir ContentKeyAuthorizationPolicy oluşturabilir ve sonra ilke için açık kısıtlama güncelleştirme belirteç kısıtlamasına kullanın ve test için belirtin. Bu işlem açık bir sürümüne ilke anahtarları önce yaklaşık 15 dakika sürer.
* Bir Media Services akış uç noktası olarak bir joker karakter ön yanıt CORS Access-Control-Allow-Origin üstbilgisinin değerini ayarlar "\*". Bu değer, Azure Media Player, Roku ve JWPlayer ve diğerleri dahil olmak üzere de çoğu oyuncuları ile çalışır. Dash.js kullanan bazı oyuncuların "içerecek şekilde" kimlik bilgilerini modunda, kendi dash.js içinde XMLHttpRequest joker izin vermez çünkü, ancak çalışmıyor "\*" Access-Control-Allow-Origin değeri olarak. Tek bir etki alanı istemcinizden barındırıyorsanız ön yanıt üst bilgisinde dash.js içinde bu sınırlama için bir geçici çözüm olarak, Media Services, etki alanı belirtebilirsiniz. Yardım için Azure portalından bir destek bileti açın.

## <a name="configure-the-key-authorization-policy"></a>Anahtarı yetkilendirme ilkesini yapılandırma
Anahtar yetkilendirme ilkesi yapılandırmak için seçin **CONTENT PROTECTION** sayfası.

Media Services, anahtar isteğinde bulunan kullanıcıların kimliğini doğrulamak için birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesinin açık olabilir. belirteç veya IP yetkilendirme kısıtlaması. (IP REST veya .NET SDK'sı ile yapılandırılabilir.)

### <a name="open-restriction"></a>Açık kısıtlama
Açık kısıtlama Sistem anahtarı bir anahtar istekte herkese sunar anlamına gelir. Bu kısıtlama, sınama amacıyla yararlı olabilir.

![OpenPolicy][open_policy]

### <a name="token-restriction"></a>Belirteç kısıtlama
Belirteç kısıtlamalı ilkenin seçmek için Seç **BELİRTECİ** düğmesi.

Belirteç kısıtlamalı ilkenin beraberinde bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, basit web belirteci belirteçleri destekler ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) ve JSON Web Token (JWT) biçimlendirir. Daha fazla bilgi için [JWT kimlik doğrulaması](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media Services, STS sağlamaz. Belirteçlerini vermek için özel STS oluşturabilirsiniz. STS belirteci kısıtlama yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri ile imzalanmış bir belirteç oluşturmak için yapılandırılmalıdır. Belirteç geçerliyse ve belirteçteki talepler içerik anahtarı için yapılandırılanlarla eşleşen, Media Services anahtar dağıtımı hizmetiyle şifreleme anahtarının istemciye döndürür.

Belirteç kısıtlamalı ilkenin yapılandırdığınızda, birincil doğrulama anahtarı, veren ve İzleyici parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı belirteç birlikte imzalandığı anahtarını içerir. Belirteci veren STS veren olur. Belirtecin amacı (kapsam olarak da adlandırılır) İzleyici açıklayan veya kaynak belirteci erişimini yetkilendirir. Media Services anahtar dağıtımı hizmetiyle belirtecindeki bu değerleri şablon değerleri eşleştiğini doğrular.

### <a name="playready"></a>PlayReady
PlayReady ile içeriğinizi korumanıza, yetkilendirme ilkenizde belirtmeniz gereken şeylerden biri PlayReady lisans şablonu tanımlayan bir XML dizedir. Varsayılan olarak, şu ilkeyi ayarlanır:

    <PlayReadyLicenseResponseTemplate xmlns:i="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
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

Seçebileceğiniz **içeri aktarma İlkesi xml** düğmesine tıklayın ve tanımlanmış XML şemasına uyan farklı bir XML sağlamak [Media Services PlayReady lisans şablonuna genel bakış](media-services-playready-license-template-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

