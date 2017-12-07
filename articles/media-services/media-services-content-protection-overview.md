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
ms.openlocfilehash: 6475a865b9d1b263bd7cc68c99acdb5f6959531e
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="protecting-content-overview"></a>İçerik koruma genel bakış
Microsoft Azure Media Services, medyanızı bilgisayarınızdan ayrıldığı andan başlayıp depolama, işleme ve teslim aşamaları boyunca güvenlik altına almanızı sağlar. Media Services dinamik olarak Gelişmiş Şifreleme Standardı (AES-128) veya üç ana DRM sistemlerinden herhangi birini ile şifrelenmiş, canlı ve isteğe bağlı içerik teslim etmenizi sağlar: Microsoft PlayReady, Google Widevine ve Apple FairPlay. Media Services de AES anahtarları ve DRM teslim etmek için bir hizmet sunar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere. 

Aşağıdaki resim Azure medya Hizmetleri içerik koruma iş akışı gösterilmektedir. 

![PlayReady ile koruma](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

Bu makalede, kavramları ve terminolojiyi AMS ile içerik koruma anlamak için ilgili açıklanmaktadır. Makale ayrıca içeriği korumak nasıl ele makalelerinin bağlantıları sağlar. 

## <a name="dynamic-encryption"></a>Dinamik şifreleme
Azure Media Services AES temizleyin anahtar veya DRM şifreleme ile dinamik olarak şifrelenmiş içeriğinizi teslim etmenizi sağlar: Microsoft PlayReady, Google Widevine ve Apple FairPlay. Aşağıdaki akış biçimlerine şifrelemek şu anda: HLS, MPEG DASH ve kesintisiz akış. Aşamalı indirme şifreleme desteklenmiyor. Her şifreleme yöntemini aşağıdaki akış protokollerini destekler:
- AES: MPEG-DASH, kesintisiz akış ve HLS
- PlayReady: MPEG-DASH, kesintisiz akış ve HLS
- Widevine: MPEG-DASH
- FairPlay: HLS

Bir varlık şifrelemek için şifreleme içerik anahtarı, varlıkla ilişkilendirin ve aynı zamanda anahtar için bir yetkilendirme ilkesi yapılandırma gerekir. İçerik anahtarı belirtilen veya Media Services tarafından otomatik olarak oluşturulur.

Ayrıca, varlığın teslim ilkesini yapılandırmanız gerekir. Bir depolama şifrelenmiş varlığı akışla aktarmak istiyorsanız, nasıl varlık teslim ilkesini yapılandırarak teslim etmek istediğinizi belirttiğinizden emin olun.

Bir akış player tarafından istendiğinde Media Services belirtilen anahtarı dinamik olarak AES temizleyin anahtar kullanarak içerik veya DRM şifreleme şifrelemek için kullanır. Akış şifresini çözmek için player anahtar AMS anahtar teslim hizmetinden ister. Kullanıcının anahtarını almak için yetkili olup olmadığına karar vermek için anahtar için belirtilen Yetkilendirme İlkeleri hizmet değerlendirir.

## <a name="aes-128-clear-key-vs-drm"></a>AES-128 şifresiz anahtar vs DRM
Müşteriler genellikle bunlar AES şifreleme veya DRM sistemini kullanması gerekip gerekmediğini merak ediyor. AES şifreleme ve DRM sistemleri kullanarak arasındaki birincil fark, AES şifreleme ile şifresiz bir biçimde ("Sil") istemcinin içerik anahtarı iletilen ' dir. Sonuç olarak, içeriği şifrelemek için kullanılan anahtar düz metin içinde bulunan istemciye bir ağ izlemesi görüntülenebilir. AES-128 şifresiz anahtar Görüntüleyici güvenilen taraf olduğu kullanım durumları için uygun (örn: çalışanlar tarafından görüntülenmesi için bir şirket içinde dağıtılmış şirket videolar şifreleme).

PlayReady, Widevine ve FairPlay tüm şifreleme karşılaştırıldığında daha yüksek düzeyde AES-128 sağlamak anahtar şifrelemesi temizleyin. İçerik anahtarı şifreli biçimde iletilir. Ayrıca, şifre çözme güvenli bir ortamda işletim sistemi düzeyinde işleyicisidir burada kötü niyetli bir kullanıcı saldırmak önemli ölçüde daha zordur. DRM burada Görüntüleyici güvenilen taraf olmayabilir ve yüksek düzeyde güvenlik gerektiren kullanım durumları için önerilir.

## <a name="storage-encryption"></a>Depolama şifrelemesi
Depolama şifrelemesi Temizle içeriğinizi yerel olarak AES 256 bit şifreleme kullanarak şifrelemek için kullanın ve Azure nerede depolandığı şifrelenen depolama birimine yükleyin. Depolama şifrelemesi ile korunan varlıklar otomatik olarak şifrelenmemiş ve kodlamadan önce şifrelenmiş bir dosya sistemine yerleştirilir ve isteğe bağlı olarak yeni çıkış varlığı şeklinde geri bir yüklemeden önce yeniden şifrelenir. Depolama şifrelemesinin birincil kullanım durumu, bekleyen güçlü şifreleme diskte, yüksek kaliteli giriş medya dosyalarınızın güvenliğini sağlamak istediğiniz durumdur.

Media Services, içeriğinizi teslim etmek istediğiniz nasıl bilmesi için depolama şifrelenmiş varlık teslim etmek için varlığın teslim ilkesini yapılandırmanız gerekir. Varlığınızı akışı önce akış sunucusu şifresini çözer ve belirtilen teslim ilkesini (örneğin, AES, ortak şifreleme veya şifreleme) kullanarak içeriğinizi akışlarını.

## <a name="types-of-encryption"></a>Şifreleme türleri
PlayReady ve Widevine, ortak şifreleme (AES CTRL modu) kullanın. FairPlay AES CBC modunda şifreleme kullanır. AES-128 temizleyin anahtar şifrelemesi Zarf şifreleme kullanır.

## <a name="licenses-and-keys-delivery-service"></a>Lisansları ve anahtarları teslimat hizmeti
Media Services (PlayReady, Widevine, FairPlay) DRM lisansları ve AES anahtarları yetkili istemcilere teslim etmek için bir anahtar teslimi hizmet sağlar. Kullanabileceğiniz [Azure portalı](media-services-portal-protect-content.md), REST API veya .NET için Media Services SDK'sı lisansları ve anahtarları için yetkilendirme ve kimlik doğrulama ilkelerini yapılandırmak için.

## <a name="control-content-access"></a>İçerik erişimi denetleme
İçerik anahtarı yetkilendirme ilkesini yapılandırarak içeriğinize olanların kontrol edebilirsiniz. İçerik anahtarı yetkilendirme ilkesini açık veya belirteç kısıtlama destekler.

### <a name="open-authorization"></a>Açık yetkilendirme
Bir açık yetkilendirme ilkesi, içerik anahtarı herhangi bir istemci (sınırsız) gönderilir.

### <a name="token-authorization"></a>Belirteci yetkilendirme
Bir belirteç kısıtlanmış yetkilendirme ilkesi, içerik anahtarı yalnızca gönderme bir istemciye geçerli JSON Web Token (JWT) veya basit Web Token (SWT) anahtar/lisans istek sayısını gösterir. Bu belirteç bir güvenli belirteç hizmeti (STS) tarafından verilmiş olması gerekir. Microsoft Active Directory bir STS olarak kullanın veya özel bir STS dağıtın. STS, belirteç kısıtlamasına yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri imzalı bir belirteç oluşturmak için yapılandırılmalıdır. Media Services anahtar teslim hizmeti istenen anahtar/lisans istemci için belirteç geçerliyse ve anahtar/lisans için yapılandırılmış talep belirteci eşleştiğinden döndürür.

Belirteç yapılandırma İlkesi sınırlı olduğunda, birincil doğrulama anahtarı, veren ve İzleyici parametreleri belirtmeniz gerekir. Birincil doğrulama anahtar belirteci ile imzalandığı anahtarı içerir, veren belirtecini veren güvenli belirteç hizmeti. Kaynak belirteci erişimini yetkilendirir veya kapsam, bazen adlı hedef kitle, belirtecin amacı açıklanır. Media Services anahtar teslim hizmeti, bu değerleri belirteci şablon değerleri eşleştiğini doğrular.

## <a name="streaming-urls"></a>Akış URL'leri
Varlığınızı birden çok DRM ile şifrelenmiş bir şifreleme etiketi akış URL'SİNDE kullanmalısınız: (biçimi 'm3u8-aapl' = şifreleme = 'xxx').

Aşağıdaki maddeler geçerlidir:
* Birden fazla şifreleme türü belirtilebilir.
* Şifreleme türü bir şifreleme varlık için uygulanan yalnızca URL'de belirtilmesi gerekmez.
* Şifreleme türü büyük küçük harfe duyarlı değil.
* Aşağıdaki şifreleme türlerini belirtilebilir:  
  * **cenc**: PlayReady veya Widevine (ortak şifreleme)
  * **cbcs-aapl**: FairPlay (AES CBC şifreleme) için
  * **CBC**: AES zarfı şifreleme (Zarf şifreleme)

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelerde, içerik koruma ile çalışmaya başlamak için sonraki adımlar açıklanmaktadır:
* [Depolama şifrelemesi ile koruma](media-services-rest-storage-encryption.md)
* [AES şifreleme ile koruma](media-services-protect-with-aes128.md)
* [PlayReady ve/veya Widevine ile koruma](media-services-protect-with-playready-widevine.md)
* [FairPlay ile koruma](media-services-protect-hls-with-FairPlay.md)

## <a name="related-links"></a>İlgili Bağlantılar
[Azure Media Services PlayReady lisans teslimat fiyatlandırma açıklanmıştır](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Azure Media Services AES şifrelenmiş akış için hata ayıklama](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT belirteci kimlik doğrulaması](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Azure Media Services OWIN MVC tabanlı uygulama Azure Active Directory ile tümleştirme ve JWT talepleri temelinde içerik anahtar teslim kısıtlamak](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
