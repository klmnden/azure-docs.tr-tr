---
title: Azure Media Services ile içeriğinizi korumanıza | Microsoft Docs
description: Bu makaleler, Media Services ile içerik koruma genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2018
ms.author: juliako
ms.openlocfilehash: 1568ea3431f18b7a7a020d34d803f883904e18b4
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39115239"
---
# <a name="content-protection-overview"></a>Content protection genel bakış

Azure Media Services, depolama, işleme ve teslim üzerinden bilgisayarınıza çıkışında medyanızdaki güvenliğini sağlamak için kullanabilirsiniz. Media Services sayesinde, Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sistemlerinden ile dinamik olarak şifrelenmiş canlı ve isteğe bağlı içerik teslim edebilirsiniz: Microsoft PlayReady ve Google Widevine Apple FairPlay. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere. 

Aşağıdaki resimde Media Services content protection iş akışı gösterilmektedir: 

![İçerik koruma](./media/content-protection/content-protection.png)

&#42;*dinamik şifreleme, AES-128 "şifresiz anahtar" ve CBCS CENC destekler. Ayrıntılar için destek matrisi bkz [burada](#streaming-protocols-and-encryption-types).*

Bu makalede, kavramlar ve terminoloji content protection ile Media Services anlamak için ilgili açıklanmaktadır. Makalede ayrıca içeriği korumak nasıl açıklayan makalelerin bağlantıları sağlanır. 

## <a name="main-components-of-the-content-protection-system"></a>İçerik koruma sisteminin ana bileşenleri

"Content protection" sistem/uygulama tasarımınızı başarıyla tamamlamak için kapsamı çalışmasının tam olarak anlamak için gerekir. Aşağıdaki liste, üç bölümden uygulamanız gereken genel bir bakış sağlar. 

1. Azure Media Services kod
  
  * PlayReady, Widevine ve/veya FairPlay lisansı şablonları. Şablonları hakları ve izinleri her kullanılan benzeri DRM yapılandırmanıza olanak sağlar
  * Lisans teslim yetkilendirme, JWT talepleri temel yetkilendirme denetiminin mantık belirtme
  * İçerik anahtarı, akış protokolleri ve uygulanan, karşılık gelen benzeri DRM tanımlama DRM şifreleme

  > [!NOTE]
  > Her varlık birden fazla şifreleme türü (AES-128, PlayReady, Widevine, FairPlay) ile şifreleyebilirsiniz. Bkz: [akış protokolleri ve şifreleme türlerini](#streaming-protocols-and-encryption-types)ne birleştirmek mantıklıdır görmek için.
  
  Aşağıdaki makaleler adımları şifreleme için AES ve/veya DRM ile içerik göster: 
  
  * [AES şifrelemesi ile koruma](protect-with-aes128.md)
  * [DRM ile koruma](protect-with-drm.md)

2. AES veya DRM istemci oynatıcı. Bir oynatıcı SDK (yerel veya tarayıcı tabanlı) tabanlı bir video oynatıcı uygulaması aşağıdaki gereksinimleri karşılaması gerekir:
  * Oyuncu SDK gerekli DRM istemcileri destekler
  * Oyuncu SDK gerekli akış protokollerini destekler: kesintisiz/DASH veya HLS
  * Oyuncu SDK lisansı edinme istekte bir JWT belirteci geçirme işleyebilir olması gerekir
  
    Bir oynatıcı kullanarak oluşturabileceğiniz [Azure Media Player API'sine](http://amp.azure.net/libs/amp/latest/docs/). Kullanma [Azure Media Player ProtectionInfo API'sine](http://amp.azure.net/libs/amp/latest/docs/) farklı DRM platformlarda kullanılacak DRM teknolojileri belirtmek için.

    Test AES veya şifrelenmiş CENC (Widevine ve/veya PlayReady) için içerik, kullanabileceğiniz [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html). "Gelişmiş Seçenekler" seçeneğini tıklatın ve denetimi, şifreleme seçenekleri emin olun.

    FairPlay şifreli içeriği test etmek istediğiniz kullanırsanız [bu test yürütücünün](http://aka.ms/amtest). Player, Widevine, PlayReady, destekler ve benzeri FairPlay DRM ve bunun yanı sıra AES-128 şifresiz anahtar şifrelemesiyle koruyun. Farklı benzeri DRM test etmek için doğru tarayıcı seçmeniz gerekebilir: Chrome/Opera/Firefox'ta Widevine, MS Edge/ıe11 PlayReady için Safari macOS FairPlay için.

3. Belirteç Hizmeti (JSON Web Token (JWT) olarak arka uç kaynağına erişim için erişim belirteci verir STS), güvenli hale getirin. AMS lisans teslim hizmetleri arka uç kaynağı olarak kullanabilirsiniz. STS sahip aşağıdaki tanımlamak:

  * Veren ve İzleyici (veya kapsam)
  * İçerik koruma, iş gereksinimleri bağımlı olan talepleri
  * İmza doğrulaması için simetrik ya da asimetrik doğrulama
  * Anahtar geçişi desteği (gerekirse)

    Kullanabileceğiniz [bu STS araç](https://openidconnectweb.azurewebsites.net/DRMTool/Jwt) doğrulama anahtarı tüm 3 türlerini destekleyen STS, test için: simetrik, asimetrik veya AAD anahtar geçişi ile. 

> [!NOTE]
> Odaklanmanıza ve her parça (yukarıda açıklanmıştır) tam olarak test sonraki adımına geçmeden önce önerilir. "Content protection" sisteminizi test etmek için yukarıdaki listede belirtilen araçları kullanın.  

## <a name="streaming-protocols-and-encryption-types"></a>Akış protokolleri ve şifreleme türleri

Media Services PlayReady, Widevine ve FairPlay kullanarak AES şifresiz anahtarını veya DRM şifreleme ile dinamik olarak şifrelenmiş içeriğinizi teslim etmek için kullanabilirsiniz. Şu anda, HTTP canlı akışı (HLS), MPEG DASH ve kesintisiz akış biçimlerinde şifreleyebilirsiniz. Her protokolü, aşağıdaki şifreleme yöntemlerini destekler:

|Protokol|Kapsayıcı biçimi|Şifreleme şeması|
|---|---|---|---|
|MPEG-DASH|Tümü|AES|
||CSF(fmp4) |CENC (Widevine + PlayReady) |
||CMAF(fmp4)|CENC (Widevine + PlayReady)|
|HLS|Tümü|AES|
||TS MPG2 |CBCS (Fairplay) |
||TS MPG2 |CENC (PlayReady) |
||CMAF(fmp4) |CENC (PlayReady) |
|Kesintisiz Akış|fMP4|AES|
||fMP4 | CENC (PlayReady) |

## <a name="dynamic-encryption"></a>Dinamik şifreleme

Media Services v3 sürümünde bir içerik anahtarı StreamingLocator ile ilişkilendirilir (bkz [Bu örnek](protect-with-aes128.md)). Media Services anahtar teslim hizmeti kullanıyorsanız, otomatik içerik anahtarı oluşturun. İçerik anahtarı kendiniz, kendi anahtar dağıtımı hizmetiyle kullanıyorsanız veya, iki veri merkezlerinde aynı içerik anahtarı olması gereken bir yüksek kullanılabilirlik senaryonun işlenmesi gerekiyorsa oluşturmanız gerekir.

Bir akışa bir oynatıcı tarafından istendiğinde Media Services dinamik olarak içeriğinizi AES şifresiz anahtar veya DRM şifreleme kullanarak şifrelemek için belirtilen anahtar kullanır. Akış şifresini çözmek için Media Services anahtar dağıtımı hizmetiyle veya belirtilen anahtar dağıtımı hizmetiyle anahtar player ister. Kullanıcı anahtarı almak için yetkili olup olmadığına karar vermek için anahtar için belirtilen içerik anahtarı ilkesi hizmet tarafından değerlendirilir.

## <a name="aes-128-clear-key-vs-drm"></a>AES-128 şifresiz anahtarını vs. DRM

Müşteriler genellikle bunlar AES şifrelemesi veya DRM sistem kullanması gerekip gerekmediğini merak ediyor. İki sistem arasındaki başlıca fark, AES şifreleme ile içerik anahtarını şifresiz bir biçimde ("clear") istemciye iletilir ' dir. Sonuç olarak, içeriği şifrelemek için kullanılan anahtar düz metin içinde bulunan istemciye bir ağ izleme görüntülenebilir. AES-128 şifresiz anahtar şifrelemesi Görüntüleyicisi (çalışanlar tarafından görüntülenmek üzere şirket içinde dağıtılmış gibi şifreleme şirket videolarınızı) güvenilen taraf olduğu kullanım örnekleri için uygundur.

PlayReady, Widevine ve FairPlay tüm şifreleme kıyasla daha yüksek bir düzeyde AES-128 şifresiz anahtar şifrelemesiyle koruyun. İçerik anahtarı şifrelenmiş biçimde iletilir. Ayrıca, şifre çözme, kötü niyetli bir kullanıcı saldırı daha zor olduğu işletim sistemi düzeyinde güvenli bir ortamda ele alınır. DRM burada Görüntüleyicisi güvenilen taraf olmayabilir ve yüksek düzeyde güvenlik gerektiren kullanım durumları için önerilir.

## <a name="storage-side-encryption"></a>Depolama tarafında şifreleme

Bekleyen veri varlıklarınızı korumanın varlıklar tarafından depolama tarafı şifrelemesi şifrelenmelidir. Aşağıdaki tabloda, depolama tarafı şifrelemesi Media Services v3 sürümünde nasıl çalıştığı gösterilmektedir:

|Şifreleme seçeneği|Açıklama|Media Services v3|
|---|---|---|---|
|Media Services'ı depolama şifrelemesi| Media Services tarafından yönetilen AES-256 şifreleme anahtarı|Desteklenmeyen<sup>(1)</sup>|
|[Bekleyen veriler için depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Sunucu tarafı şifrelemesi, Azure Depolama tarafından sunulan anahtarı Azure tarafından veya müşteri tarafından yönetilen|Desteklenen|
|[Depolama istemci tarafı şifreleme](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Azure depolama, anahtar Kasası'nda müşteri tarafından yönetilen bir kiracı anahtarı tarafından sunulan istemci tarafı şifreleme|Desteklenmiyor|

<sup>1</sup> , Media Services v3 (AES-256 şifreleme) depolama şifrelemesi, yalnızca varlıklarınızı Media Services v2 ile oluşturulduğunda için geriye dönük uyumluluk desteklenir. Var olan depolama ile v3 çalışır anlamı varlıklar şifreli ancak yenilerini oluşturulmasına izin vermez.

## <a name="licenses-and-keys-delivery-service"></a>Lisanslar ve anahtarları teslim hizmeti

Media Services DRM (PlayReady, Widevine, FairPlay) lisansları ve AES anahtarları yetkili istemcilere sunmak için bir anahtar dağıtımı hizmetiyle sağlar. Lisanslar ve anahtarları için yetkilendirme ve kimlik doğrulama ilkelerini yapılandırmak için REST API veya bir Media Services istemci Kitaplığı'nı kullanabilirsiniz.

## <a name="control-content-access"></a>İçerik erişimi denetleme

İçeriğinizi içerik anahtarı ilkesi yapılandırarak kimlerin erişebileceğini kontrol edebilirsiniz. Media Services, anahtar isteğinde bulunan kullanıcıların kimlik doğrulamasını yapmanın birden çok yöntemini destekler. İçerik anahtarı İlkesi yapılandırmanız gerekir. Anahtarın istemciye teslim edilebilmesi için istemci (oynatıcı) ilkeyi karşılaması gerekir. İçerik anahtarı ilkeniz olabilir **açın** veya **belirteci** kısıtlama. 

Bir belirteç kısıtlamalı içerik anahtar ilkesiyle, içerik anahtarı, anahtar/lisans istekte geçerli JSON Web Token (JWT) veya basit web belirteci (SWT) sunan bir istemciye gönderilir. Bu belirteci bir güvenlik belirteci hizmeti (STS) tarafından verilmiş olması gerekir. Azure Active Directory STS kullanın veya özel STS dağıtın. STS belirteci kısıtlama yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri ile imzalanmış bir belirteç oluşturmak için yapılandırılmalıdır. Media Services anahtar dağıtımı hizmetiyle belirteç geçerliyse ve belirteçteki talepler için bir anahtar/lisans yapılandırılanlar eşleşen istemci için istenen anahtar/lisans döndürür.

Belirteç kısıtlamalı ilkenin yapılandırdığınızda, birincil doğrulama anahtarı, veren ve İzleyici parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı belirteç birlikte imzalandığı anahtarını içerir. Verici belirteci veren güvenli belirteç hizmetidir. Belirtecin amacı kapsam olarak da adlandırılan, hedef kitle açıklayan veya kaynak belirteci erişimini yetkilendirir. Media Services anahtar dağıtımı hizmetiyle belirtecindeki bu değerleri şablon değerleri eşleştiğini doğrular.


## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelere göz atın:

  * [AES şifrelemesi ile koruma](protect-with-aes128.md)
  * [DRM ile koruma](protect-with-drm.md)

Ek bilgiler bulunabilir [DRM tasarımı ve uygulama başvurusu](../previous/media-services-cenc-with-multidrm-access-control.md)


