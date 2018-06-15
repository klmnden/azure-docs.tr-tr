---
title: İçeriğinizi Azure Media Services ile koruma | Microsoft Docs
description: Bu makaleler, Media Services ile içerik koruma genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 13447fd9193374d80ed5c2e6af8543f11b95e709
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788742"
---
# <a name="content-protection-overview"></a>İçerik koruma genel bakış
 Azure Media Services, depolama, işleme ve teslim üzerinden bilgisayarınıza çıkışında medyanızdan güvenliğini sağlamak için kullanabilirsiniz. Media Services ile dinamik olarak Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sistemlerinden herhangi birini ile şifrelenmiş, canlı ve isteğe bağlı içerik teslim: Microsoft PlayReady, Google Widevine ve Apple FairPlay. Media Services de AES anahtarları ve DRM teslim etmek için bir hizmet sunar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere. 

Aşağıdaki görüntü Media Services içerik koruma iş akışı gösterilmiştir: 

![PlayReady ile koruma](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

Bu makalede, kavramları ve terminolojiyi Media Services ile içerik koruma anlamak için ilgili açıklanmaktadır. Makale ayrıca içeriği korumak nasıl ele makalelerinin bağlantıları sağlar. 

## <a name="dynamic-encryption"></a>Dinamik şifreleme
 Media Services, PlayReady, Widevine veya FairPlay kullanarak AES temizleyin anahtar veya DRM şifreleme ile dinamik olarak şifrelenmiş içeriğinizi teslim etmek için kullanabilirsiniz. Şu anda, HTTP canlı akışı (HLS), MPEG DASH ve kesintisiz akış biçimlerinin şifreleyebilirsiniz. Aşamalı indirme şifreleme desteklenmiyor. Her şifreleme yöntemini aşağıdaki akış protokollerini destekler:

- AES: MPEG-DASH, kesintisiz akış ve HLS
- PlayReady: MPEG-DASH, kesintisiz akış ve HLS
- Widevine: MPEG-DASH
- FairPlay: HLS

Bir varlık şifrelemek için şifreleme içerik anahtarı, varlıkla ilişkilendirin ve aynı zamanda anahtar için bir yetkilendirme ilkesi yapılandırma gerekir. İçerik anahtarı belirtilen veya Media Services tarafından otomatik olarak oluşturulur.

Ayrıca, varlığın teslim ilkesini yapılandırmanız gerekir. Depolama şifrelenmiş bir varlığı akışla aktarmak istiyorsanız, nasıl varlık teslim ilkesini yapılandırarak teslim etmek istediğinizi belirttiğinizden emin olun.

Bir akış player tarafından istendiğinde Media Services belirtilen anahtarı dinamik olarak içeriğinizi AES şifresiz anahtar veya DRM şifreleme kullanarak şifrelemek için kullanır. Akış şifresini çözmek için Media Services anahtar teslim hizmetinden anahtar player ister. Kullanıcının anahtarını almak için yetkili olup olmadığına karar vermek için anahtar için belirtilen Yetkilendirme İlkeleri hizmet değerlendirir.

## <a name="aes-128-clear-key-vs-drm"></a>AES-128 temizleyin anahtar vs. DRM
Müşteriler genellikle bunlar AES şifreleme veya DRM sistemini kullanması gerekip gerekmediğini merak ediyor. İki sistem arasındaki birincil fark, AES şifreleme ile şifresiz bir biçimde ("Sil") istemcinin içerik anahtarı iletilen ' dir. Sonuç olarak, içeriği şifrelemek için kullanılan anahtar düz metin içinde bulunan istemciye bir ağ izlemesi görüntülenebilir. AES-128 temizleyin anahtar şifrelemesi viewer güvenilen taraf (çalışanlar tarafından görüntülenmesi için bir şirket içinde dağıtılmış Örneğin, şifrelenen Kurumsal videolar) olduğu kullanım örnekleri için uygundur.

PlayReady, Widevine ve FairPlay tüm şifreleme karşılaştırıldığında daha yüksek düzeyde AES-128 sağlamak anahtar şifrelemesi temizleyin. İçerik anahtarı şifreli biçimde iletilir. Ayrıca, şifre çözme güvenli bir ortamda kötü niyetli bir kullanıcı saldırmak daha zor olduğu işletim sistemi düzeyinde ele alınır. DRM burada Görüntüleyici güvenilen taraf olmayabilir ve yüksek düzeyde güvenlik gerektiren kullanım durumları için önerilir.

## <a name="storage-encryption"></a>Depolama şifrelemesi
Depolama şifrelemesi Temizle içeriğinizi yerel olarak AES 256 bit şifreleme kullanarak şifrelemek için kullanabilirsiniz. Şifrelenen Azure nerede depolandığı depolama ile sonra yükleyebilirsiniz. Depolama şifrelemesi ile korunan varlıklar otomatik olarak şifrelenmemiş ve kodlamadan önce şifrelenmiş bir dosya sistemine yerleştirilir. Varlıklar geri yeni bir çıkış varlığı yüklenmeden önce isteğe bağlı olarak yeniden şifrelenmiş. Depolama şifrelemesinin birincil kullanım durumu, bekleyen güçlü şifreleme diskte, yüksek kaliteli giriş medya dosyalarınızın güvenliğini sağlamak istediğiniz durumdur.

Böylece, içeriğinizi teslim etmek istediğiniz nasıl Media Services bilir depolama şifrelenmiş bir varlık teslim etmek için varlığın teslim ilkesini yapılandırmanız gerekir. Varlığınızı akışı önce akış sunucusu şifresini çözer ve belirtilen teslim ilkesini (örneğin, AES, ortak şifreleme veya şifreleme) kullanarak içeriğinizi akışlarını.

## <a name="types-of-encryption"></a>Şifreleme türleri
PlayReady ve Widevine, ortak şifreleme (AES CTRL modu) kullanın. FairPlay CBC modunda AES şifreleme kullanır. AES-128 temizleyin anahtar şifrelemesi Zarf şifreleme kullanır.

## <a name="licenses-and-keys-delivery-service"></a>Lisansları ve anahtarları teslimat hizmeti
Media Services (PlayReady, Widevine, FairPlay) DRM lisansları ve AES anahtarları yetkili istemcilere teslim etmek için bir anahtar teslimi hizmet sağlar. Kullanabileceğiniz [Azure portalı](media-services-portal-protect-content.md), REST API veya .NET için Media Services SDK'sı lisansları ve anahtarları için yetkilendirme ve kimlik doğrulama ilkelerini yapılandırmak için.

## <a name="control-content-access"></a>İçerik erişimi denetleme
İçerik anahtarı yetkilendirme ilkesini yapılandırarak içeriğinize olanların kontrol edebilirsiniz. İçerik anahtarı yetkilendirme ilkesini açık veya belirteç kısıtlama destekler.

### <a name="open-authorization"></a>Açık yetkilendirme
Bir açık yetkilendirme ilkesi, içerik anahtarı herhangi bir istemci (sınırsız) gönderilir.

### <a name="token-authorization"></a>Belirteci yetkilendirme
Bir belirteç kısıtlanmış yetkilendirme ilkesi, içerik anahtarı anahtar/lisans isteğinde geçerli JSON Web Token (JWT) veya basit web token (SWT) sunan bir istemci gönderilir. Bu belirteç güvenlik belirteci hizmeti (STS) tarafından verilmiş olması gerekir. Bir STS olarak Azure Active Directory kullanın veya özel bir STS dağıtın. STS, belirteç kısıtlamasına yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri imzalı bir belirteç oluşturmak için yapılandırılmalıdır. Media Services anahtar teslim hizmeti istenen anahtar/lisans istemcisi için belirteç geçerliyse ve anahtar/lisans için yapılandırılmış talep belirteci eşleştiğinden döndürür.

Belirteç kısıtlamalı ilkenin yapılandırdığınızda, birincil doğrulama anahtarı, veren ve İzleyici parametreleri belirtmeniz gerekir. Birincil doğrulama anahtar belirteci ile imzalandığı anahtarı içerir. Verici belirteç veren güvenli bir belirteç hizmetidir. Kaynak belirteci erişimini yetkilendirir veya kapsam, bazen adlı hedef kitle, belirtecin amacı açıklanır. Media Services anahtar teslim hizmeti, bu değerleri belirteci şablon değerleri eşleştiğini doğrular.

## <a name="streaming-urls"></a>Akış URL'leri
Varlığınızı birden çok DRM ile şifrelenmiş bir şifreleme etiketi akış URL'SİNDE kullanın: (biçimi 'm3u8-aapl' = şifreleme = 'xxx').

Aşağıdaki maddeler geçerlidir:

* Birden fazla şifreleme türü belirtilebilir.
* Şifreleme türü bir şifreleme varlık için uygulanan yalnızca URL'de belirtilmesi gerekmez.
* Şifreleme türü büyük küçük harfe duyarlı değil.
* Aşağıdaki şifreleme türlerini belirtilebilir:
  * **cenc**: için PlayReady veya Widevine (ortak şifreleme)
  * **cbcs-aapl**: için FairPlay (AES CBC şifreleme)
  * **CBC**: için AES zarfı şifreleme

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelerde, içerik koruma ile çalışmaya başlamanıza yardımcı olmak için sonraki adımlar açıklanmaktadır:

* [Depolama şifrelemesi ile koruma](media-services-rest-storage-encryption.md)
* [AES şifreleme ile koruma](media-services-protect-with-aes128.md)
* [PlayReady ve/veya Widevine ile koruma](media-services-protect-with-playready-widevine.md)
* [FairPlay ile koruma](media-services-protect-hls-with-FairPlay.md)

## <a name="related-links"></a>İlgili bağlantılar

* [Azure Media Services PlayReady lisans teslimat fiyatlandırma açıklanmıştır](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)
* [Azure Media Services AES şifrelenmiş akış için hata ayıklama](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)
* [JWT belirteci kimlik doğrulaması](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)
* [Azure Media Services OWIN MVC tabanlı uygulama Azure Active Directory ile tümleştirme ve JWT talepleri temelinde içerik anahtar teslim kısıtla](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/)

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
