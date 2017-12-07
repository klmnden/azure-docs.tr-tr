---
title: "Azure portalını kullanarak içerik anahtarı yetkilendirme ilkesini yapılandırma | Microsoft Docs"
description: "İçerik anahtarının yetkilendirme ilkesini yapılandırma hakkında bilgi edinin."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ee82a3fa-c34b-48f2-a108-8ba321f1691e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 36ec76718d21cac5ae3203f1c6d4b8af2aacb9ed
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="configure-content-key-authorization-policy"></a>İçerik anahtarı yetkilendirme ilkesini yapılandırma
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Genel Bakış
Microsoft Azure Media Services, Gelişmiş Şifreleme Standardı ((128-bit şifreleme anahtarları kullanılarak) AES ile) korumalı MPEG-DASH, kesintisiz akış ve HTTP canlı akış (HLS) akışlar sunmanıza olanak sağlar veya [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS Widevine DRM ile şifrelenmiş DASH akışları teslim etmenizi sağlar. PlayReady ve Widevine, Ortak Şifreleme (ISO/IEC 23001-7 CENC) belirtimi uyarınca şifrelenir.

Medya Hizmetleri de sağlar bir **anahtarı/lisans teslimat hizmeti** istemcilerin AES anahtarları veya şifrelenmiş içeriği yürütmek için PlayReady/Widevine lisansları elde edebilirsiniz.

Bu makalede Azure portal içerik anahtarı yetkilendirme ilkesini yapılandırmak için nasıl kullanılacağı gösterilmektedir. Anahtar, daha sonra dinamik olarak içeriğinizi şifrelemek için de kullanılabilir. Aşağıdaki akış biçimlerine şifrelemek şu anda: HLS, MPEG DASH ve kesintisiz akış. Aşamalı indirme şifrelenemiyor.

Medya Hizmetleri yapılandırılmış anahtar bir oynatıcı dinamik olarak şifrelenmesini şekilde ayarlanmış bir akış istediğinde, dinamik olarak AES veya DRM şifreleme kullanarak içeriğinizi şifrelemek için kullanır. Akış şifresini çözmek için player anahtar anahtar teslim hizmetinden ister. Kullanıcının anahtarını almak için yetkili olup olmadığına karar vermek için anahtar için belirtilen Yetkilendirme İlkeleri hizmet değerlendirir.

Birden çok içerik anahtarı varsa veya belirtmek istiyorsanız planlıyorsanız bir **anahtarı/lisans teslimat hizmeti** Media Services anahtar teslim hizmeti dışındaki URL'yi Media Services .NET SDK veya REST API'leri kullanın.

[Media Services .NET SDK kullanarak içerik anahtarının yetkilendirme ilkesini yapılandırma](media-services-dotnet-configure-content-key-auth-policy.md)

[Media Services REST API kullanarak içerik anahtarının yetkilendirme ilkesini yapılandırma](media-services-rest-configure-content-key-auth-policy.md)

### <a name="some-considerations-apply"></a>Bazı dikkate alınması gereken noktalar vardır:
* AMS hesabınızı oluşturulduğunda bir **varsayılan** akış uç noktası ekleniyor hesabınızda **durduruldu** durumu. İçeriğinizi akış başlatmak ve dinamik paketleme ve dinamik şifreleme yararlanmak için akış uç noktanızı olması sahip **çalıştıran** durumu. 
* Varlığınızı Uyarlamalı bit hızlı MP4s ya da Uyarlamalı bit hızlı kesintisiz akış dosyaları içermelidir. Daha fazla bilgi için bkz: [bir varlık kodla](media-services-encode-asset.md).
* Anahtar teslim hizmeti ContentKeyAuthorizationPolicy ve ilişkili nesnelerini (ilkesi seçenekleri ve kısıtlamaları) 15 dakika için önbelleğe alır.  Bir ContentKeyAuthorizationPolicy oluşturup, bir "Token" kısıtlama kullanılacağını belirtin, test ve ardından ilkesini "Açık" kısıtlama güncelleştirin, ilke ilkesini "Açık" sürümüne geçiş yapmadan önce yaklaşık 15 dakika sürer.
* Akış uç noktası AMS joker karakter olarak denetim öncesi yanıt CORS 'Access-Control-Allow-Origin' üstbilgisinin değerini ayarlar '\*'. Bu da bizim Azure Media Player, Roku ve JW ve diğerleri de dahil olmak üzere çoğu oyuncularla çalışır. "Dahil etmek için" kimlik bilgileri modu ayarlandığında, kullanıcıların dashjs XMLHttpRequest joker izin vermez, ancak dashjs yararlanan bazı oynatıcıları çalışmaya değil "\*" değeri olarak "'Access-Control-Allow-Origin". Tek bir etki alanı istemcinizden barındırıyorsa dashjs içinde bu sınırlamaya geçici Azure Media Services ön yanıt üstbilgisinde bu etki alanı belirtebilirsiniz. Azure portal üzerinden destek bileti açılarak ulaşın.

## <a name="how-to-configure-the-key-authorization-policy"></a>Nasıl yapılır: anahtarı yetkilendirme ilkesini yapılandırın
Anahtar yetkilendirme ilkesi yapılandırmak için seçin **içerik koruma** sayfa.

Media Services, anahtar isteğinde bulunan kullanıcıların kimlik doğrulamasını yapmanın birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesini olabilir **açmak**, **belirteci**, veya **IP** yetkilendirme kısıtlamaları (**IP** REST veya .NET SDK'sı ile yapılandırılabilir).

### <a name="open-restriction"></a>Açık kısıtlama
**Açmak** sınırlama anlamına gelir sistem anahtar istekte herkes için anahtar sağlar. Bu kısıtlama sınama amacıyla yararlı olabilir.

![OpenPolicy][open_policy]

### <a name="token-restriction"></a>Belirteç kısıtlama
Belirteç kısıtlamalı ilkenin seçmek için basın **BELİRTECİ** düğmesi.

**Belirteci** sınırlı İlkesi eşlik, tarafından verilmiş bir belirteç tarafından bir **güvenli belirteç hizmeti** (STS). Media Services belirteçleri destekler **basit Web belirteçleri** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) biçimi ve **JSON Web belirteci** (JWT) biçimi. Bilgi için bkz: [JWT belirteci kimlik doğrulaması](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media Services sağlamaz **güvenli belirteç Hizmetleri**. Özel bir STS oluşturabilir veya Microsoft Azure ACS sorunu belirteçleri yararlanın. STS, belirteç kısıtlamasına yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri imzalı bir belirteç oluşturmak için yapılandırılmalıdır. Media Services anahtar teslim hizmeti şifreleme anahtarını istemci için belirteç geçerliyse ve içerik anahtarı için yapılandırılmış talep belirteci eşleştiğinden döndürür. Daha fazla bilgi için bkz: [sorunu belirteçleri için kullanım Azure ACS](http://mingfeiy.com/acs-with-key-services).

Yapılandırırken **belirteci** kısıtlanmış İlkesi, birincil belirtmelisiniz **doğrulama anahtarı**, **veren** ve **İzleyici** parametreleri. Birincil **doğrulama anahtarı** , belirteci imzalayan anahtarı içeren **veren** belirtecini veren güvenli belirteç hizmetidir. **İzleyici** (bazen adlı **kapsam**) belirteç veya belirteç erişim yetkisi verir kaynak amacı açıklar. Media Services anahtar teslim hizmeti, bu değerleri belirteci şablon değerleri eşleştiğini doğrular.

### <a name="playready"></a>PlayReady
İçeriğinizi ile korurken **PlayReady**, Yetkilendirme ilkesinde belirtmek için gereken şeyleri biri PlayReady lisans şablonu tanımlayan bir XML dizesi. Varsayılan olarak, aşağıdaki İlkesi ayarlanır:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1"><LicenseTemplates> <PlayReadyLicenseTemplate> <AllowTestDevices>true</AllowTestDevices> <ContentKey i:type="ContentEncryptionKeyFromHeader" /> <LicenseType>Nonpersistent</LicenseType> <PlayRight> <AllowPassingVideoContentToUnknownOutput>izin verilen</AllowPassingVideoContentToUnknownOutput> </PlayRight> </PlayReadyLicenseTemplate> </LicenseTemplates></PlayReadyLicenseResponseTemplate>

Tıklayabilirsiniz **ilke XML'yi içe** düğmesini tıklatın ve uygun farklı bir XML tanımlı XML Şeması sağlar [burada](media-services-playready-license-template-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

