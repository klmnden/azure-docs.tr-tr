---
title: İçeriğinizi Azure Media Services ile koruma | Microsoft Docs
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
ms.openlocfilehash: 28c10d7c478ea7a9b1d1bfa91da79452eba3a4b6
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133108"
---
# <a name="content-protection-overview"></a>İçerik koruma genel bakış

Azure Media Services, depolama, işleme ve teslim üzerinden bilgisayarınıza çıkışında medyanızdan güvenliğini sağlamak için kullanabilirsiniz. Media Services ile dinamik olarak Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sistemlerinden herhangi birini ile şifrelenmiş, canlı ve isteğe bağlı içerik teslim: Microsoft PlayReady, Google Widevine ve Apple FairPlay. Media Services de AES anahtarları ve DRM teslim etmek için bir hizmet sunar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere. 

Aşağıdaki görüntü Media Services içerik koruma iş akışı gösterilmiştir: 

![İçerik koruma](./media/content-protection/content-protection.png)

&#42;*dinamik şifreleme, AES-128 "şifresiz anahtar", CBCS ve CENC destekler. Ayrıntılar için destek matrisi bkz [burada](#streaming-protocols-and-encryption-types).*

Bu makalede, kavramları ve terminolojiyi Media Services ile içerik koruma anlamak için ilgili açıklanmaktadır. Makale ayrıca içeriği korumak nasıl ele makalelerinin bağlantıları sağlar. 

## <a name="main-components-of-the-content-protection-system"></a>İçerik koruma sisteminin ana bileşenleri

"İçerik koruma" Sistem/Uygulama tasarımınız başarıyla tamamlamak için çaba kapsamını tam olarak anlamak için gerekir. Aşağıdaki listede, uygulamanız gereken üç bölümden genel bir bakış sağlar. 

1. Azure Media Services kod
  
  * PlayReady, Widevine ve/veya FairPlay için lisans şablonları. Şablonlar her kullanılan DRMs için haklar ve izinler yapılandırmanıza olanak sağlar
  * Lisans teslimat yetkilendirme, JWT Taleplerde dayalı yetkilendirme onay mantığı belirtme
  * İçerik anahtarı, akış protokolleri ve uygulanan, karşılık gelen DRMs tanımlama DRM şifreleme

  > [!NOTE]
  > Her varlık türleriyle birden çok şifreleme (AES-128, PlayReady, Widevine, FairPlay) şifreleyebilirsiniz. Bkz: [akış protokolleri ve şifreleme türleri](#streaming-protocols-and-encryption-types), ne birleştirmek mantıklıdır görmek için.
  
  Aşağıdaki makalede şifrelemek için adımları AES ile içerik göster: [AES şifreleme ile koru](protect-with-aes128.md)
 
2. Player AES veya DRM istemci ile. Bir oyuncu SDK (yerel veya tarayıcı tabanlı) dayalı bir video oynatıcı uygulaması aşağıdaki gereksinimleri karşılaması gerekir:
  * SDK player gerekli DRM istemcilerini destekler
  * SDK player gerekli akış protokollerini destekler: kesintisiz, DASH ve/veya HLS
  * Lisans edinme istekte bir JWT belirteci geçirme işlemek SDK player gerekir
  
    Kullanarak bir oynatıcı oluşturabilirsiniz [Azure Media Player API](http://amp.azure.net/libs/amp/latest/docs/). Kullanmak [Azure Media Player ProtectionInfo API](http://amp.azure.net/libs/amp/latest/docs/) hangi DRM teknolojinin farklı DRM platformlarda kullanılacağını belirtmek için.

    Sınama AES veya şifrelenmiş CENC (Widevine + PlayReady) için içerik, kullanabileceğiniz [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html). "Gelişmiş Seçenekleri"'i tıklatın ve AES denetleyin ve belirteç sağlayın emin olun.

    FairPlay şifrelenmiş içerik test etmek isterseniz kullanmak [bu test player](http://aka.ms/amtest). Widevine, PlayReady player destekler ve AES-128 yanı sıra FairPlay DRMs anahtar şifrelemesi temizleyin. Farklı DRMs test etmek için doğru tarayıcı seçmeniz gerekiyor: Opera/Chrome/Firefox Widevine, MS kenar/IE11 PlayReady için FairPlay maOS üzerinde Safari için.

3. Belirteç Hizmeti (hangi arka uç kaynak erişimi için erişim belirteci olarak JSON Web Token (JWT) sorunları STS), güvenli hale getirin. AMS lisans teslimat hizmetlerini arka uç kaynağı olarak kullanabilirsiniz. Bir STS olan aşağıdaki tanımlamak:

  * Veren ve İzleyici (veya kapsam)
  * İçerik koruma iş gereksinimlerini bağımlı talepleri
  * İmza doğrulaması için simetrik ya da asimetrik doğrulama
  * Anahtar geçişi desteği (gerekirse)

    Kullanabileceğiniz [bu STS aracı](https://openidconnectweb.azurewebsites.net/DRMTool/Jwt) doğrulama anahtarı tüm 3 türlerini destekler STS, test: simetrik, asimetrik veya anahtar geçişi AAD'ye. 

> [!NOTE]
> Odak ve tam olarak (yukarıda) her bir bölümünü test etmek için sonraki bölümü taşımadan önce önerilir. "İçerik koruma" sisteminizi sınamak için yukarıdaki listede belirtilen araçları kullanın.  

## <a name="streaming-protocols-and-encryption-types"></a>Akış protokolleri ve şifreleme türleri

Media Services, PlayReady, Widevine veya FairPlay kullanarak AES temizleyin anahtar veya DRM şifreleme ile dinamik olarak şifrelenmiş içeriğinizi teslim etmek için kullanabilirsiniz. Şu anda, HTTP canlı akışı (HLS), MPEG DASH ve kesintisiz akış biçimlerinin şifreleyebilirsiniz. Her protokolü, aşağıdaki şifreleme yöntemlerini destekler:

|Protokol|Kapsayıcı biçimi|Şifreleme şeması|
|---|---|---|---|
|MPEG-DASH|Tümü|AES|
||CSF(fmp4) |CENC (Widevine + PlayReady) |
||CMAF(fmp4)|CENC (Widevine + PlayReady)|
|HLS|Tümü|AES|
||MPG2 TS |CBCS (Fairplay) |
||MPG2 TS |CENC (PlayReady) |
||CMAF(fmp4) |CENC (PlayReady) |
|Kesintisiz Akış|fMP4|AES|
||fMP4 | CENC (PlayReady) |

## <a name="dynamic-encryption"></a>Dinamik şifreleme

Media Services v3 bir içerik anahtarı StreamingLocator ile ilişkilendirilen (bkz [Bu örnek](protect-with-aes128.md)). Media Services anahtar teslim hizmeti kullanıyorsanız, otomatik içerik anahtarı oluşturun. İçerik anahtarı kendiniz, kendi anahtar teslim hizmeti kullanıyorsanız veya yüksek kullanılabilirlik senaryo iki veri merkezlerinde aynı içerik anahtarı olmasına gerek burada işlemek gerekirse oluşturmanız gerekir.

Bir akış player tarafından istendiğinde Media Services belirtilen anahtarı dinamik olarak içeriğinizi AES şifresiz anahtar veya DRM şifreleme kullanarak şifrelemek için kullanır. Akış şifresini çözmek için Media Services anahtar teslim hizmeti veya belirtilen anahtar teslim hizmeti anahtarı player ister. Kullanıcının anahtarını almak için yetkili olup olmadığına karar vermek için anahtar için belirtilen Yetkilendirme İlkeleri hizmet değerlendirir.

## <a name="aes-128-clear-key-vs-drm"></a>AES-128 temizleyin anahtar vs. DRM

Müşteriler genellikle bunlar AES şifreleme veya DRM sistemini kullanması gerekip gerekmediğini merak ediyor. İki sistem arasındaki birincil fark, AES şifreleme ile şifresiz bir biçimde ("Sil") istemcinin içerik anahtarı iletilen ' dir. Sonuç olarak, içeriği şifrelemek için kullanılan anahtar düz metin içinde bulunan istemciye bir ağ izlemesi görüntülenebilir. AES-128 temizleyin anahtar şifrelemesi viewer güvenilen taraf (çalışanlar tarafından görüntülenmesi için bir şirket içinde dağıtılmış Örneğin, şifrelenen Kurumsal videolar) olduğu kullanım örnekleri için uygundur.

PlayReady, Widevine ve FairPlay tüm şifreleme karşılaştırıldığında daha yüksek düzeyde AES-128 sağlamak anahtar şifrelemesi temizleyin. İçerik anahtarı şifreli biçimde iletilir. Ayrıca, şifre çözme güvenli bir ortamda kötü niyetli bir kullanıcı saldırmak daha zor olduğu işletim sistemi düzeyinde ele alınır. DRM burada Görüntüleyici güvenilen taraf olmayabilir ve yüksek düzeyde güvenlik gerektiren kullanım durumları için önerilir.

## <a name="storage-side-encryption"></a>Depolama tarafı şifreleme

Varlıklarınızı REST korumak için depolama Tarafı Şifrelemesi tarafından varlıklar şifrelenmelidir. Aşağıdaki tabloda, depolama tarafı şifreleme Media Services v3 nasıl çalıştığı gösterilmektedir:

|Şifreleme seçeneği|Açıklama|Media Services v3|
|---|---|---|---|
|Depolama şifrelemesi medya Hizmetleri| Medya Hizmetleri tarafından yönetilen AES-256 şifrelemesi, anahtar|Desteklenmeyen<sup>(1)</sup>|
|[Rest verileri için depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Sunucu tarafı şifreleme Azure Storage tarafından sunulan anahtar Azure veya müşteri tarafından yönetilen|Desteklenen|
|[İstemci tarafı şifreleme depolama](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Azure depolama, anahtar kasası müşteri tarafından yönetilen anahtar tarafından sunulan istemci tarafı şifreleme|Desteklenmiyor|

<sup>1</sup> Media Services'ın v3 depolama şifreleme (AES 256 şifreleme), yalnızca Media Services v2 ile varlıklarınızı oluşturulduğunda için geriye dönük uyumluluk desteklenir. Var olan depolama ile v3 çalışır anlamı varlıklar şifrelenmiş ancak yenilerini oluşturulmasına izin vermez.

## <a name="licenses-and-keys-delivery-service"></a>Lisansları ve anahtarları teslimat hizmeti

Media Services (PlayReady, Widevine, FairPlay) DRM lisansları ve AES anahtarları yetkili istemcilere teslim etmek için bir anahtar teslimi hizmet sağlar. Lisansları ve anahtarları için yetkilendirme ve kimlik doğrulama ilkelerini yapılandırmak için REST API veya bir Media Services istemci Kitaplığı'nı kullanabilirsiniz.

## <a name="control-content-access"></a>İçerik erişimi denetleme

İçerik anahtarı ilkesi yapılandırarak içeriğinize olanların kontrol edebilirsiniz. Media Services, anahtar isteğinde bulunan kullanıcıların kimlik doğrulamasını yapmanın birden çok yöntemini destekler. İçerik anahtarı İlkesi yapılandırmanız gerekir. Anahtarın istemciye teslim edilebilir önce istemci (oynatıcı) ilkesi karşılaması gerekir. İçerik anahtarı ilkesi olabilir **açmak** veya **belirteci** kısıtlama. 

Bir belirteç sınırlı içerik anahtar ilkesi, içerik anahtarı anahtar/lisans isteğinde geçerli JSON Web Token (JWT) veya basit web token (SWT) sunan bir istemci gönderilir. Bu belirteç güvenlik belirteci hizmeti (STS) tarafından verilmiş olması gerekir. Bir STS olarak Azure Active Directory kullanın veya özel bir STS dağıtın. STS, belirteç kısıtlamasına yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri imzalı bir belirteç oluşturmak için yapılandırılmalıdır. Media Services anahtar teslim hizmeti istenen anahtar/lisans istemcisi için belirteç geçerliyse ve anahtar/lisans için yapılandırılmış talep belirteci eşleştiğinden döndürür.

Belirteç kısıtlamalı ilkenin yapılandırdığınızda, birincil doğrulama anahtarı, veren ve İzleyici parametreleri belirtmeniz gerekir. Birincil doğrulama anahtar belirteci ile imzalandığı anahtarı içerir. Verici belirteç veren güvenli bir belirteç hizmetidir. Kaynak belirteci erişimini yetkilendirir veya kapsam, bazen adlı hedef kitle, belirtecin amacı açıklanır. Media Services anahtar teslim hizmeti, bu değerleri belirteci şablon değerleri eşleştiğini doğrular.

## <a name="streaming-urls"></a>Akış URL'leri

Varlığınızı birden çok DRM ile şifrelenmiş bir şifreleme etiketi akış URL'SİNDE kullanın: (biçimi 'm3u8-aapl' = şifreleme = 'xxx').

Aşağıdaki maddeler geçerlidir:

* Şifreleme türü bir şifreleme varlık için uygulanan yalnızca URL'de belirtilmesi gerekmez.
* Şifreleme türü büyük küçük harfe duyarlı değil.
* Aşağıdaki şifreleme türlerini belirtilebilir:
  * **cenc**: için PlayReady veya Widevine (ortak şifreleme)
  * **cbcs-aapl**: için FairPlay (AES CBC şifreleme)
  * **CBC**: için AES zarfı şifreleme

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları

Uygulama sorunları ile ilgili Yardım için aşağıdaki sorun giderme bilgileri kullanın.

* URL ile bitmelidir veren "/". Hedef kitleyi player uygulama istemci kimliği olmalıdır Ayrıca, Ekle "/" veren URL sonunda.

  ```
  <add key="ida:audience" value="[Application Client ID GUID]" />
  <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />
  ```

* Azure AD uygulama izinleri eklemek **yapılandırma** uygulama sekmesinde. Her uygulama için hem yerel hem de dağıtılan sürümleri izinleri gereklidir.
* Dinamik CENC korumayı ayarlamadan doğru veren kullanın.

  ```
  <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>
  ```

  Aşağıdakiler çalışmaz:

  ```
  <add key="ida:issuer" value="https://username.onmicrosoft.com/" />
  ```

  GUID Azure AD Kiracı kimliğidir. GUID bulunabilir **uç noktaları** Azure portalında açılır menü.

* GRANT grup üyeliği ayrıcalıkları talepleri. Azure AD uygulama bildirim dosyasında aşağıdaki olduğundan emin olun: 

    "groupMembershipClaims": "Tümü" (varsayılan değeri null)

## <a name="next-steps"></a>Sonraki adımlar

[AES şifreleme ile koruma](protect-with-aes128.md)
