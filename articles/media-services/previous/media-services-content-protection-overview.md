---
title: Azure Media Services ile içeriğinizi korumanıza | Microsoft Docs
description: Bu makaleler, Media Services ile içerik koruma genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: 8259b58c7f30b63084e970bd9aed99642a43226f
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58805746"
---
# <a name="content-protection-overview"></a>Content protection genel bakış 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)

Azure Media Services, depolama, işleme ve teslim üzerinden bilgisayarınıza çıkışında medyanızdaki güvenliğini sağlamak için kullanabilirsiniz. Media Services sayesinde, Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sistemlerinden ile dinamik olarak şifrelenmiş canlı ve isteğe bağlı içerik teslim edebilirsiniz: Microsoft PlayReady, Google Widevine ve FairPlay Apple. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere. 

Aşağıdaki resimde Media Services content protection iş akışı gösterilmektedir: 

![PlayReady ile koruma](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

Bu makalede, kavramlar ve terminoloji content protection ile Media Services anlamak için ilgili açıklanmaktadır. Makalede ayrıca içeriği korumak nasıl açıklayan makalelerin bağlantıları sağlanır. 

## <a name="dynamic-encryption"></a>Dinamik şifreleme
 Media Services PlayReady, Widevine ve FairPlay kullanarak AES şifresiz anahtarını veya DRM şifreleme ile dinamik olarak şifrelenmiş içeriğinizi teslim etmek için kullanabilirsiniz. Şu anda, HTTP canlı akışı (HLS), MPEG DASH ve kesintisiz akış biçimlerinde şifreleyebilirsiniz. İlerlemeli indirmeler şifreleme desteklenmiyor. Her bir şifreleme yöntemi, aşağıdaki akış protokollerini destekler:

- AES: MPEG-DASH, kesintisiz akış ve HLS
- PlayReady: MPEG-DASH, kesintisiz akış ve HLS
- Widevine: MPEG-DASH
- FairPlay: HLS

Bir varlık şifrelemek için şifreleme içerik anahtarı, varlıkla ilişkilendirme ve bir yetkilendirme ilkesi anahtarı için de yapılandırmanız gerekir. İçerik anahtarı, belirtilen veya Media Services tarafından otomatik olarak oluşturulur.

Varlık teslim ilkesini yapılandırmanız gerekir. Depolama ile şifrelenmiş bir varlığı akışla aktarmak istiyorsanız, nasıl varlık teslim ilkesini yapılandırarak sunmak istediğiniz belirttiğinizden emin olun.

Bir akışa bir oynatıcı tarafından istendiğinde Media Services dinamik olarak içeriğinizi AES şifresiz anahtar veya DRM şifreleme kullanarak şifrelemek için belirtilen anahtar kullanır. Akış şifresini çözmek için Media Services anahtar teslim hizmetinden anahtar player ister. Kullanıcı anahtarı almak için yetkili olup olmadığına karar vermek için anahtar için belirtilen Yetkilendirme İlkeleri hizmet tarafından değerlendirilir.

## <a name="aes-128-clear-key-vs-drm"></a>AES-128 şifresiz anahtarını vs. DRM
Müşteriler genellikle bunlar AES şifrelemesi veya DRM sistem kullanması gerekip gerekmediğini merak ediyor. İki sistem arasındaki başlıca fark, AES şifreleme ile içerik anahtarını şifresiz bir biçimde ("clear") istemciye iletilir ' dir. Sonuç olarak, içeriği şifrelemek için kullanılan anahtar düz metin içinde bulunan istemciye bir ağ izleme görüntülenebilir. AES-128 şifresiz anahtar şifrelemesi Görüntüleyicisi (çalışanlar tarafından görüntülenmek üzere şirket içinde dağıtılmış gibi şifreleme şirket videolarınızı) güvenilen taraf olduğu kullanım örnekleri için uygundur.

PlayReady, Widevine ve FairPlay tüm şifreleme kıyasla daha yüksek bir düzeyde AES-128 şifresiz anahtar şifrelemesiyle koruyun. İçerik anahtarı şifrelenmiş biçimde iletilir. Ayrıca, şifre çözme, kötü niyetli bir kullanıcı saldırı daha zor olduğu işletim sistemi düzeyinde güvenli bir ortamda ele alınır. DRM burada Görüntüleyicisi güvenilen taraf olmayabilir ve yüksek düzeyde güvenlik gerektiren kullanım durumları için önerilir.

## <a name="storage-encryption"></a>Depolama şifrelemesi
Depolama şifrelemesi Temizle içeriğinizi yerel olarak AES 256 bit şifreleme kullanarak şifrelemek için kullanabilirsiniz. Bekleme sırasında şifrelenmiş Azure depolandığı depolama ile daha sonra yükleyebilirsiniz. Depolama şifrelemesi ile korunan varlıklar otomatik olarak şifrelenmemiş ve kodlama önce şifrelenmiş bir dosya sistemine yerleştirilir. Yeni çıktı varlığı şeklinde geri bir yüklenmeden önce isteğe bağlı olarak yeniden şifrelenmiş varlıklardır. Depolama şifrelemesinin birincil kullanım durumu, yüksek kaliteli girdi medya dosyalarınızı bekleyen güçlü şifrelemeyle diskte güvenliğini sağlamak istediğiniz durumdur.

Depolama ile şifrelenmiş bir varlık sunmak için Media Services, içeriğinizi teslim etmek istediğiniz şekli bilebilmesi varlık teslim ilkesini yapılandırmanız gerekir. Varlığınızı akışla önce akış sunucusu şifresini çözer ve belirtilen teslim ilkesini (örneğin, AES, ortak şifreleme veya şifreleme) kullanarak içeriğinizi akışları.

## <a name="types-of-encryption"></a>Şifreleme türleri
PlayReady ve Widevine, ortak şifreleme (AES CTRL modu) kullanın. FairPlay CBC modunda AES şifreleme kullanır. AES-128 şifresiz anahtar şifrelemesi Zarf şifreleme kullanır.

## <a name="licenses-and-keys-delivery-service"></a>Lisanslar ve anahtarları teslim hizmeti
Media Services DRM (PlayReady, Widevine, FairPlay) lisansları ve AES anahtarları yetkili istemcilere sunmak için bir anahtar dağıtımı hizmetiyle sağlar. Kullanabileceğiniz [Azure portalında](media-services-portal-protect-content.md), REST API'si veya .NET için Media Services SDK'sını lisanslar ve anahtarları için yetkilendirme ve kimlik doğrulama ilkelerini yapılandırma.

## <a name="control-content-access"></a>İçerik erişimi denetleme
İçeriğinizi içerik anahtarı yetkilendirme ilkesini yapılandırarak kimlerin erişebileceğini kontrol edebilirsiniz. İçerik anahtarı yetkilendirme ilkesinin açık veya belirteç kısıtlaması destekler.

### <a name="open-authorization"></a>Açık yetkilendirme
Bir açık yetkilendirme ilkesi ile içerik anahtarı herhangi bir istemciye (sınırsız) gönderilir.

### <a name="token-authorization"></a>Yetkilendirme belirteci
Belirteç kısıtlamalı yetkilendirme ilkesi ile anahtar/lisans istekte geçerli JSON Web Token (JWT) veya basit web belirteci (SWT) sunan bir istemci içerik anahtarı gönderilir. Bu belirteci bir güvenlik belirteci hizmeti (STS) tarafından verilmiş olması gerekir. Azure Active Directory STS kullanın veya özel STS dağıtın. STS belirteci kısıtlama yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri ile imzalanmış bir belirteç oluşturmak için yapılandırılmalıdır. Media Services anahtar dağıtımı hizmetiyle belirteç geçerliyse ve belirteçteki talepler için bir anahtar/lisans yapılandırılanlar eşleşen istemci için istenen anahtar/lisans döndürür.

Belirteç kısıtlamalı ilkenin yapılandırdığınızda, birincil doğrulama anahtarı, veren ve İzleyici parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı belirteç birlikte imzalandığı anahtarını içerir. Verici belirteci veren güvenli belirteç hizmetidir. Belirtecin amacı kapsam olarak da adlandırılan, hedef kitle açıklayan veya kaynak belirteci erişimini yetkilendirir. Media Services anahtar dağıtımı hizmetiyle belirtecindeki bu değerleri şablon değerleri eşleştiğini doğrular.

## <a name="streaming-urls"></a>Akış URL'leri
Varlığınız birden çok DRM ile şifrelenmiş, şifreleme etiketi akış URL'SİNDE kullanın: (format = 'm3u8-aapl' şifreleme = 'xxx').

Aşağıdaki maddeler geçerlidir:

* Birden fazla şifreleme türü belirtilebilir.
* Şifreleme türü yalnızca varlık için bir şifreleme uygulandı URL'nin belirtilmesi gerekmez.
* Şifreleme türü büyük/küçük harfe duyarlıdır.
* Aşağıdaki şifreleme türlerini belirtilebilir:
  * **cenc**: PlayReady veya Widevine (ortak şifreleme)
  * **cbcs-aapl**: HLS için FairPlay (AES-CBC şifreleme)
  * **CBC**: AES zarfı şifreleme

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelerde, content protection ile çalışmaya başlamanıza yardımcı olmak için sonraki adımlar açıklanmaktadır:

* [Depolama şifrelemesi ile koruma](media-services-rest-storage-encryption.md)
* [AES şifrelemesi ile koruma](media-services-protect-with-aes128.md)
* [PlayReady ve/veya Widevine ile koruma](media-services-protect-with-playready-widevine.md)
* [HLS ile FairPlay koruyun](media-services-protect-hls-with-FairPlay.md)

## <a name="related-links"></a>İlgili bağlantılar

* [JWT belirteci kimlik doğrulaması](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)
* [Azure Media Services OWIN MVC tabanlı bir uygulamayı Azure Active Directory ile tümleştirin ve içerik anahtar teslim JWT taleplere göre kısıtlama](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/)

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
