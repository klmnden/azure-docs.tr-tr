---
title: "İçeriğinizi Azure Media Services ile koruma | Microsoft Docs"
description: "Bu makaleler, Media Services ile içerik koruma genel bir bakış sağlar."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 64be4ea104bd11b8e191e2c8d4170a2de88acb47
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="protecting-content-overview"></a>İçerik koruma genel bakış
Microsoft Azure Media Services, medyanızı bilgisayarınızdan ayrıldığı andan başlayıp depolama, işleme ve teslim aşamaları boyunca güvenlik altına almanızı sağlar. Media Services dinamik olarak Gelişmiş Şifreleme Standardı ((128-bit şifreleme anahtarları kullanılarak) AES) veya herhangi bir önemli DRMs ile şifrelenmiş, canlı ve isteğe bağlı içerik teslim etmenizi sağlar: Microsoft PlayReady, Google Widevine ve Apple FairPlay. Media Services de AES anahtarları ve DRM teslim etmek için bir hizmet sunar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere. 

Aşağıdaki resimde AMS destekleyen içerik koruma iş akışlarını gösterir. 

![PlayReady ile koruma](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 

Bu konuda açıklanmaktadır [kavramları ve terminolojiyi](media-services-content-protection-overview.md) AMS ile içerik koruma anlama ilgilidir. Konu aynı zamanda içerir [bağlantılar](media-services-content-protection-overview.md#common-scenarios) içerik koruma görevlerin nasıl yerine getirileceğini gösteren konulara. 

## <a name="dynamic-encryption"></a>Dinamik şifreleme
Microsoft Azure Media Services AES temizleyin anahtar veya DRM şifreleme ile dinamik olarak şifrelenmiş içeriğinizi teslim etmenizi sağlar: Microsoft PlayReady, Google Widevine ve Apple FairPlay.

Aşağıdaki akış biçimlerine şifrelemek şu anda: HLS, MPEG DASH ve kesintisiz akış. Aşamalı indirme şifrelenemiyor.

Media Services'ın bir varlık şifrelemek istiyorsanız, bir şifreleme anahtarı (CommonEncryption veya EnvelopeEncryption), varlıkla ilişkilendirin ve anahtarı için yetkilendirme ilkelerini de yapılandırmanız gerekir.

Ayrıca, varlığın teslim ilkesini yapılandırmanız gerekir. Bir depolama şifrelenmiş varlığı akışla aktarmak istiyorsanız, nasıl varlık teslim ilkesini yapılandırarak teslim etmek istediğinizi belirttiğinizden emin olun.

Bir akış player tarafından istendiğinde Media Services belirtilen anahtarı dinamik olarak AES temizleyin anahtar kullanarak içerik veya DRM şifreleme şifrelemek için kullanır. Akış şifresini çözmek için player anahtar teslim hizmetinden anahtarı ister. Kullanıcının anahtarını almak için yetkili olup olmadığına karar vermek için anahtar için belirtilen Yetkilendirme İlkeleri hizmet değerlendirir.


## <a name="storage-encryption"></a>Depolama şifrelemesi
Depolama şifrelemesi Temizle içeriğinizi yerel olarak AES 256 bit şifreleme kullanarak şifrelemek ve Azure nerede depolandığı şifrelenen depolama alanına yüklemek için kullanın. Depolama şifrelemesi ile korunan varlıklar otomatik olarak şifrelenmemiş ve kodlamadan önce şifrelenmiş bir dosya sistemine yerleştirilir ve isteğe bağlı olarak yeni çıkış varlığı şeklinde geri bir yüklemeden önce yeniden şifrelenir. Depolama şifrelemesinin birincil kullanım durumu, güçlü şifreleme REST diskte, yüksek kaliteli giriş medya dosyalarınızın güvenliğini sağlamak istediğiniz durumdur.

Media Services, içeriğinizi teslim etmek istediğiniz nasıl bilmesi için depolama şifrelenmiş varlık teslim etmek için varlığın teslim ilkesini yapılandırmanız gerekir. Varlığınızı akışı önce akış sunucusu depolama şifreleme kaldırır ve belirtilen teslim ilkesini (örneğin, AES, ortak şifreleme veya şifreleme) kullanarak içeriğinizi akışlarını.

## <a name="common-encryption-cenc"></a>Ortak şifreleme (CENC)
Ortak şifreleme içeriğinizi PlayReady ile şifrelerken kullanılan veya / ve Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Cbcs-aapl şifreleme kullanma
Cbcs-aapl içeriğinizi FairPlay ile şifrelerken kullanılır.

## <a name="envelope-encryption"></a>Zarf şifreleme
AES-128 şifresiz anahtarla içeriğinizi korumak istiyorsanız bu seçeneği kullanın. Daha güvenli bir seçenek istiyorsanız, bu konuda listelenen DRMs birini seçin. 

## <a name="licenses-and-keys-delivery-service"></a>Lisansları ve anahtarları teslimat hizmeti
AES anahtarları yetkili istemcilere temizleyin ve Media Services (PlayReady, Widevine, FairPlay) DRM lisansları teslim etmek için bir hizmet sağlar. Kullanabileceğiniz [Azure portalı](media-services-portal-protect-content.md), REST API veya .NET için Media Services SDK'sı lisansları ve anahtarları için yetkilendirme ve kimlik doğrulama ilkelerini yapılandırmak için.

## <a name="token-restriction"></a>Belirteç kısıtlama
İçerik anahtarı yetkilendirme ilkesinin bir veya daha fazla yetkilendirme kısıtlaması olabilir: açık veya belirteç kısıtlaması. Belirteç kısıtlamalı ilkenin beraberinde bir Güvenli Belirteç Hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, basit Web belirteçleri (SWT) biçimini ve JSON Web Token (JWT) biçimlerindeki belirteçleri destekler. Media Services, güvenli belirteç hizmetleri sağlamaz. Özel bir STS oluşturabilir veya Microsoft Azure ACS sorunu belirteçleri yararlanın. STS, belirteç kısıtlamasına yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri imzalı bir belirteç oluşturmak için yapılandırılmalıdır. Media Services anahtar teslim hizmeti istenen anahtarı (veya lisans) istemcinin belirtecin geçerli olup olmadığını ve bu belirteci yapılandırmış anahtarı (veya lisans) için talepleri döndürür.

Belirteç yapılandırma İlkesi sınırlı olduğunda, birincil doğrulama anahtarı, veren ve İzleyici parametreleri belirtmeniz gerekir. Birincil doğrulama anahtar belirteci ile imzalandığı anahtarı içerir, veren belirtecini veren güvenli belirteç hizmeti. Kaynak belirteci erişimini yetkilendirir veya (bazen kapsam denir) İzleyici belirteç amacı açıklanır. Media Services anahtar teslim hizmeti, bu değerleri belirteci şablon değerleri eşleştiğini doğrular.

## <a name="streaming-urls"></a>Akış URL'leri
Varlığınızı birden çok DRM ile şifrelenmiş bir şifreleme etiketi akış URL'SİNDE kullanmalısınız: (biçimi 'm3u8-aapl' = şifreleme = 'xxx').

Aşağıdaki maddeler geçerlidir:

* Yalnızca sıfır veya bir şifreleme türü belirtilebilir.
* Şifreleme türü bir şifreleme varlık için uygulanan yalnızca URL'de belirtilmesi gerekmez.
* Şifreleme türü büyük küçük harfe duyarlı değil.
* Aşağıdaki şifreleme türlerini belirtilebilir:  
  * **cenc**: ortak şifreleme (Playready veya Widevine)
  * **cbcs-aapl**: Fairplay
  * **CBC**: AES zarfı şifreleme.

## <a name="common-scenarios"></a>Genel senaryolar
Aşağıdaki konular depolama alanında içeriği koruma, dinamik olarak şifrelenmiş akan medya teslim etme, AMS anahtarı/lisans teslimat hizmeti kullanmak nasıl gösterir

* [AES ile koruma](media-services-protect-with-aes128.md) 
* [PlayReady ve/veya Widevine ile koruma](media-services-protect-with-drm.md)
* [HLS içerik korumalı Apple FairPlay ve/veya PlayReady ile akışı](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>İlave senaryolar
* [Azure PlayReady lisans hizmeti, kendi Şifreleyici ve akış sunucusuyla tümleştirmek nasıl](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
* [Azure Media Services DRM lisansları teslim için castLabs kullanma](media-services-castlabs-integration.md)

>[!NOTE]
>Bir dış DRM server(technology) ve AMS akıştan kullanmak bir senaryo şu anda desteklenmiyor.


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[PlayReady hizmet ve AES dinamik şifreleme Azure Media Services ile tanışın](http://mingfeiy.com/playready)

[Azure Media Services PlayReady lisans teslimat fiyatlandırma açıklanmıştır](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Azure Media Services AES şifrelenmiş akış için hata ayıklama](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT belirteci kimlik doğrulaması](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Azure Media Services OWIN MVC tabanlı uygulama Azure Active Directory ile tümleştirme ve JWT talepleri temelinde içerik anahtar teslim kısıtlamak](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Azure ACS sorunu belirteçleri kullanmak](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
