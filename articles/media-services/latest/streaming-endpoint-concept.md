---
title: Akış uç noktaları Azure Media Services | Microsoft Docs
description: Bu makalede, akış uç noktaları nelerdir ve Azure Media Services tarafından nasıl kullanıldıkları bir açıklama sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 09/02/2018
ms.author: juliako
ms.openlocfilehash: c91a538acda44efe55777ec76c5804075a43c322
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669851"
---
# <a name="streaming-endpoints"></a>Akış Uç Noktaları

Microsoft Azure Media Services (AMS) içinde [akış uç noktalarını](https://docs.microsoft.com/rest/api/media/streamingendpoints) varlık içeriği bir istemci Yürütücü uygulamaya doğrudan teslim eden ya da daha fazla için bir içerik teslim ağı (CDN) bir akış hizmetini temsil eder Dağıtım. Bir akış uç noktası hizmetinden giden akış canlı akış ve isteğe bağlı varlığı Media Services hesabınızda olabilir. Bir Media Services hesabı oluşturduğunuzda bir **varsayılan** akış uç noktası, durdurulmuş durumda sizin için oluşturulur. Varsayılan akış uç noktası silinemiyor. Ek akış uç noktaları hesap altında oluşturulabilir. Video akışını başlatmak için akış uç noktası başlatmanız gerekir. 

## <a name="streamingendpoint-types"></a>StreamingEndpoint türleri  

İki **StreamingEndpoint** türleri: **standart** ve **Premium**. Türü ölçek birimi sayısına göre tanımlanır (`scaleUnits`) akış uç noktası için ayırın. 

Tablo türleri açıklanmaktadır:  

|Tür|Ölçek birimleri|Açıklama|
|--------|--------|--------|  
|**Standart akış uç noktası** (önerilir)|0|**Standart** türü neredeyse tüm akış senaryoları ve hedef kitle boyutları için önerilen seçenektir. **Standart** türü giden bant genişliğini otomatik olarak ölçeklendirir. <br/>Media Services ile son derece gereksinimleri yoğunlukta olan çeşitli müşteriler için sunar **Premium** akış uç noktaları kullanılabilir ölçek artırma kapasitesi en büyük internet izleyiciler için. Geniş kitlelere ve eş zamanlı görüntüleyiciler düşünüyorsanız bizimle iletişime geçin amsstreaming@microsoft.com taşımak gereken yönergeler **Premium** türü. |
|**Premium akış uç noktası**|>0|**Premium** akış uç noktaları, adanmış ve ölçeklenebilir bant genişliği kapasitesi sağlar; dolayısıyla gelişmiş iş yükleri için uygundur. Geçmeden bir **Premium** ayarlayarak türü `scaleUnits`. `scaleUnits` 200 MB/sn'lik artışlarla satın alınabilir adanmış çıkış kapasitesi sağlar. Kullanırken **Premium** türü, her etkin birim, uygulamaya ek bant genişliği kapasitesi sağlar. |

## <a name="working-with-cdn"></a>CDN ile çalışma

Çoğu durumda, CDN etkin olması gerekir. En fazla eşzamanlılık 500 görüntüleyiciler düşük kapasitesinden ancak ardından eşzamanlılık ile CDN en iyi ölçeklenen beri CDN devre dışı bırakmak için önerilir.

### <a name="detailed-explanation-of-how-caching-works"></a>Ayrıntılı açıklaması ve önbelleğe alma nasıl işler?

CDN, akış uç noktası için bir CDN gereken bant genişliği miktarını etkin olduğundan ekleme değişir, belirli bir bant genişliği değer yoktur. Çok fazla içerik, ne kadar popüler olduğunu, bit hızlarına dönüştürme ve iletişim kuralları türüne bağlıdır. CDN ne istenen yalnızca önbelleği. Video parça önbelleğe sürece bu popüler içerik doğrudan CDN'den – hizmet anlamına gelir. Canlı içerik tam aynı şeyi izlemek çok kişi genellikle olduğundan önbelleğe olasıdır. Popüler ve bazıları da değil içerikler olabilir çünkü isteğe bağlı içerik biraz zor olabilir. Milyonlarca nerede bunların hiçbiri popüler (yalnızca 1 veya 2 görüntüleyiciler haftada) olan ancak binlerce kişinin yayınlanan tüm farklı videoları sahip video varlığını varsa, CDN daha az etkili olur. Bu önbellek isabetsizliği, akış uç noktasında yük artırın.
 
Ayrıca nasıl Uyarlamalı akış works göz önünde bulundurmanız gerekir. Kendi varlık olduğundan tek tek her video parçayı önbelleğe alınır. Örneğin, belirli bir video izlenen, ilk kez kişi yalnızca birkaç saniye izlemeyi geçici olarak atlarsa vardır ve burada yalnızca kişi izlenen ile ilişkili video parçasının CDN'de önbelleğe. Uyarlamalı akış ile video 5-7 farklı bit hızlarında genellikle sahiptir. Bir kişinin tek bit hızlı izliyor ve başka bir kişiye farklı bir bit hızı izliyor, ardından bunların her ayrı olarak CDN'de önbelleğe alınır. İki kişinin aynı hızı izlerken olsa bile, farklı protokoller üzerinden akış. Her bir protokol (HLS, MPEG-DASH, kesintisiz akış) ayrı olarak önbelleğe alınır. Bu nedenle her bit hızı ve protokol önbelleğe ayrı olarak ve istenen bu video parçasının önbelleğe alınır.
 
## <a name="streamingendpoint-properties"></a>StreamingEndpoint özellikleri 

Bu bölüm, bazı StreamingEndpoint'ın özellikleri hakkında ayrıntılar sağlar. Yeni bir akış uç noktası ve açıklamaları tüm özelliklerinin nasıl oluşturulacağını örnekleri için bkz: [akış uç noktası](https://docs.microsoft.com/rest/api/media/streamingendpoints/create). 

|Özellik|Açıklama|  
|--------------|----------|
|`accessControl`|Bu akış uç noktası için aşağıdaki güvenlik ayarlarını yapılandırmak için kullanılan: Akamai imza üst bilgisi kimlik doğrulaması anahtarları ve IP adresleri, bu uç noktaya bağlanmak için izin verilir.<br />Bu özellik zaman ayarlanabilir `cdnEnabled`"' false olarak ayarlanır.|  
|`cdnEnabled`|Bu akış uç noktası için Azure CDN tümleştirmesi etkin (varsayılan olarak devredışı.) olup olmadığını gösterir<br /><br /> Ayarlarsanız `cdnEnabled` true olarak, aşağıdaki yapılandırmaları devre dışı: `customHostNames` ve `accessControl`.<br /><br />Tüm veri merkezlerini, Azure CDN tümleştirmesini desteklemiyor. Veri merkezinizde Azure CDN sahip olup olmadığını denetlemek için kullanılabilen tümleştirme aşağıdakileri yapın:<br /><br /> -Ayarlamaya çalıştığınızda `cdnEnabled` true.<br /><br /> -Denetlemek için döndürülen sonuç bir `HTTP Error Code 412` (PreconditionFailed), "Akış uç noktası CdnEnabled özellik CDN özelliği geçerli bölgede kullanılamıyor gibi true olarak ayarlanamaz." iletisi ile<br /><br /> Bu hatayı alırsanız veri merkezi bunu desteklemez. Başka bir veri merkezinde çalışmanız gerekir.|  
|`cdnProfile`|Zaman `cdnEnabled` ayarlandığında true de geçirebilirsiniz `cdnProfile` değerleri. `cdnProfile` CDN profil adı, CDN uç noktası oluşturulacağı ' dir. Mevcut bir cdnProfile sağlamak veya yeni bir tane kullanın. Değer NULL ise ve `cdnEnabled` true, "AzureMediaStreamingPlatformCdnProfile" kullanılan varsayılan değer olan. Varsa sağlanan `cdnProfile` zaten varolan bir uç nokta altında oluşturulur. Profil yok, yeni bir profil otomatik olarak oluşturulur.|
|`cdnProvider`|CDN etkinleştirildiğinde de geçirebilirsiniz `cdnProvider` değerleri. `cdnProvider` hangi sağlayıcısı kullanılacak denetler. Şu anda üç değerleri desteklenir: "StandardVerizon", "PremiumVerizon" ve "StandardAkamai". Hiçbir değer sağlanmışsa ve `cdnEnabled` true ise "StandardVerizon" (varsayılan değer olan.) kullanılır|
|`crossSiteAccessPolicies`|Siteler arası erişim ilkeleri için çeşitli istemcilere belirtmek için kullanılır. Daha fazla bilgi için [etki alanları arası ilke dosyası belirtimi](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html) ve [bir hizmet üzerinden etki alanı sınırlarında kullanılabilir hale getirme](http://msdn.microsoft.com/library/cc197955\(v=vs.95\).aspx).|  
|`customHostNames`|Özel ana bilgisayar adına yönlendirilen trafiği kabul etmek için bir akış uç noktasını yapılandırmak için kullanılır. Bu, daha kolay trafik Yönetimi yapılandırması bir genel Traffic Manager (GTM) aracılığıyla ve ayrıca için akış uç noktası adı kullanılacak markalı bir etki alanı adları sağlar.<br /><br /> Etki alanı sahipliğini Azure Media Services tarafından onaylanmalıdır. Azure Media Services gerektirerek etki alanı adı sahipliğini doğrulayan bir `CName` Azure Media Services hesabı kimliği olarak kullanılan etki alanına eklenmesi için bir bileşen içeren kayıt. Örneğin, "bir kayıt için akış uç noktası için bir özel konak adı olarak kullanılacak sports.contoso.com" için `<accountId>.contoso.com` Media Services doğrulama ana bilgisayar adlarından birini işaret edecek şekilde yapılandırılması gerekir. Doğrulama ana bilgisayar adı verifydns oluşur. \<mediaservices dns bölgesi >. Aşağıdaki tabloda, farklı Azure bölgelerine için doğrulama kaydında kullanılacak beklenen DNS bölgelerini içerir.<br /><br /> Kuzey Amerika, Avrupa, Singapur, Hong Kong, Japonya:<br /><br /> -mediaservices.windows.net<br /><br /> -verifydns.mediaservices.windows.net<br /><br /> Çin'de:<br /><br /> -mediaservices.chinacloudapi.cn<br /><br /> -verifydns.mediaservices.chinacloudapi.cn<br /><br /> Örneğin, bir `CName` "945a4c4e-28ea-45 cd-8ccb-a519f6b700ad.contoso.com" için "verifydns.mediaservices.windows.net" eşleyen kaydı, Azure Media Services kimliği 945a4c4e-28ea-45cd-8ccb-a519f6b700ad sahipliğini olduğunu kanıtlar Bu nedenle söz konusu hesap altında bir akış uç noktası için bir özel konak adı olarak kullanılacak contoso.com altındaki herhangi bir ad etkinleştirme, contoso.com etki.<br /><br /> Medya hizmeti kimlik değerini bulmak için Git [Azure portalında](https://portal.azure.com/) ve medya hizmeti hesabınızı seçin. MEDYA hizmeti kimliği PANO sayfasının sağ tarafta görüntülenir.<br /><br /> **Uyarı**: uygun bir doğrulaması olmadan bir özel konak adı ayarlama girişimi varsa `CName` kaydı, DNS yanıtının başarısız olur ve bir süre sonra önbelleğe alınabilir. Uygun bir kayıt yerleştirildikten sonra önbelleğe alınan yanıtın yeniden doğrulanır kadar biraz sürebilir. Özel etki alanı için DNS sağlayıcıya bağlı olarak, herhangi bir yere birkaç dakika veya saat kaydı düzeltin için ele geçirebilir.<br /><br /> Ek olarak `CName` eşleyen `<accountId>.<parent domain>` için `verifydns.<mediaservices-dns-zone>`, başka oluşturmalısınız `CName` özel ana bilgisayar adı eşleyen (örneğin, `sports.contoso.com`), medya Hizmetleri StreamingEndpont'ın ana bilgisayar adı (örneğin, `amstest.streaming.mediaservices.windows.net`).<br /><br /> **Not**: akış uç noktaları aynı veri merkezinde bulunan aynı özel ana bilgisayar adı paylaşamaz.<br /> Bu özellik standart ve premium akış uç noktaları için geçerlidir ve ne zaman ayarlanabilir "cdnEnabled": false<br/><br/> Şu anda AMS SSL ile özel etki alanlarını desteklemiyor.  |  
|`maxCacheAge`|Akış uç noktasında medya parçasının ve isteğe bağlı bildirimlerini tarafından belirlenen varsayılan max-age HTTP önbellek denetim üstbilgisini geçersiz kılar. Saniye cinsinden değeri ayarlanır.|
|`resourceState`|Özellik için değerleri şunlardır:<br /><br /> -Durduruldu. Bir akış uç noktası oluşturulduktan sonra başlangıç durumu.<br /><br /> -Başlatılıyor. Akış uç noktasının çalışır duruma geçiyor.<br /><br /> -Çalışıyor. Akış uç noktasını, istemciler içerik akışı kuramıyor.<br /><br /> -Ölçeklendirilmesi. Ölçek birimleri gönderildiğini artırabilir veya azaltılabilir.<br /><br /> -Durduruluyor. Akış uç noktasını durdurulmuş duruma geçiyor.<br/><br/> -Siliniyor. Akış uç noktası siliniyor.|
|`scaleUnits `|scaleUnits 200 MB/sn'lik artışlarla satın alınabilir adanmış çıkış kapasitesi sağlar. Taşımak gerekiyorsa bir **Premium** yazın, ayarlamak `scaleUnits`. |

## <a name="next-steps"></a>Sonraki adımlar

Örnek [bu depodaki](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) varsayılan akış uç .NET ile başlatmak gösterilmektedir.

