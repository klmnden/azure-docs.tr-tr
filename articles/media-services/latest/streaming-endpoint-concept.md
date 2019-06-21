---
title: Akış uç noktaları (kaynak) Azure Media Services | Microsoft Docs
description: Azure Media Services'de akış uç noktası (kaynak) dinamik paketleme ve içeriği doğrudan bir istemci Yürütücü uygulamasına veya daha fazla dağıtım bir içerik teslim ağı'için (CDN) teslim eden akış hizmetini temsil eder.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/27/2019
ms.author: juliako
ms.openlocfilehash: ab74b778757aefc22f66e8b52d1f1d922526f14a
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296141"
---
# <a name="streaming-endpoints"></a>Akış Uç Noktaları 

Microsoft Azure Media Services, bir [akış uç noktası](https://docs.microsoft.com/rest/api/media/streamingendpoints) birini kullanarak doğrudan bir istemci oynatıcı uygulaması için canlı ve isteğe bağlı içerik teslim eden bir dinamik (tam zamanında) paketleme ve kaynak hizmetini temsil eder yaygın akış medya protokolleri (HLS veya DASH). Ayrıca, **akış uç noktası** sektör lideri benzeri DRM dinamik (tam zamanında) şifreleme sağlar.

Bir Media Services hesabı oluşturduğunuzda bir **varsayılan** akış uç noktası, durdurulmuş durumda sizin için oluşturulur. Nelze odstranit **varsayılan** akış uç noktası. Ek akış uç noktaları hesap altında oluşturulabilir (bkz [kotaları ve sınırlamaları](limits-quotas-constraints.md)). 

> [!NOTE]
> Video akışını başlatmak için başlatmanız **akış uç noktası** video akışı yapmak istediğiniz. 
>  
> Akış uç noktanızı çalışır durumda olduğunda yalnızca faturalandırılırsınız.

## <a name="naming-convention"></a>Adlandırma kuralı

Varsayılan uç nokta için: `{AccountName}-{DatacenterAbbreviation}.streaming.media.azure.net`

Tüm ek uç noktalar için: `{EndpointName}-{AccountName}-{DatacenterAbbreviation}.streaming.media.azure.net`

## <a name="types"></a>Türleri  

İki **Akış Uç Noktası** türü vardır: **Standart** (Önizleme) ve **Premium**. Türü ölçek birimi sayısına göre tanımlanır (`scaleUnits`) akış uç noktası için ayırın. 

Türler aşağıdaki tabloda açıklanmıştır:  

|Tür|Ölçek birimleri|Açıklama|
|--------|--------|--------|  
|**Standart**|0|Varsayılan akış uç noktası olan bir **standart** yazın, Premium türüne ayarlayarak değiştirilebilir `scaleUnits`.|
|**Premium**|>0|**Premium** akış uç noktalarını Gelişmiş iş yükleri için adanmış ve ölçeklenebilir bant genişliği kapasitesi sağlar. Geçmeden bir **Premium** ayarlayarak türü `scaleUnits` (akış birimi). `scaleUnits` 200 MB/sn'lik artışlarla satın alınabilir adanmış çıkış kapasitesi sağlar. **Premium** türü kullandığınızda etkinleştirilen her birim, uygulamaya ek bant genişliği kapasitesi sağlar. |

> [!NOTE]
> İnternet geniş kitlelere içerik sağlamak isteyen müşteriler için akış uç noktasında CDN'yi etkinleştirmenizi öneririz.

SLA bilgileri için bkz. [fiyatlandırma ve SLA](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="comparing-streaming-types"></a>Akış türlerini karşılaştırma

Özellik|Standart|Premium
---|---|---
İlk 15 gün ücretsiz <sup>1</sup>| Evet |Hayır
Aktarım hızı |600 MB/sn için ve CDN kullanıldığında bir çok daha yüksek maliyetli performans sağlayabilir.|Akış birimi (SU) başına 200 MB/sn. CDN kullanıldığında bir çok daha yüksek maliyetli performans sağlayabilir.
CDN|Azure CDN, üçüncü taraf CDN veya hiçbir CDN.|Azure CDN, üçüncü taraf CDN veya hiçbir CDN.
Faturalama saatlere eşit olarak dağıtılır| Günlük|Günlük
Dinamik şifreleme|Evet|Evet
Dinamik paketleme|Evet|Evet
Ölçek|Otomatik yönelik hedeflenen aktarım hızını ölçeklendirir.|Ek SUs
IP filtrelemeyi/G20/özel konak <sup>2</sup>|Evet|Evet
Aşamalı indirme|Evet|Evet
Önerilen kullanım |Büyük bir çoğunluğu senaryoları akış önerilir.|Profesyonel kullanımı.

<sup>1</sup> ücretsiz deneme sürümünü yalnızca yeni oluşturulan media services hesapları ve varsayılan akış uç noktası için geçerlidir.<br/>
<sup>2</sup> CDN uç noktasında etkinleştirilmediğinde yalnızca doğrudan akış uç kullanılır.<br/>

## <a name="properties"></a>Özellikler 

Bu bölüm, akış uç noktasının özelliklerini bazıları hakkında ayrıntılar sağlar. Yeni bir akış uç noktası ve açıklamaları tüm özelliklerinin nasıl oluşturulacağını örnekleri için bkz: [akış uç noktası](https://docs.microsoft.com/rest/api/media/streamingendpoints/create). 

- `accessControl` -Bu akış uç noktası için aşağıdaki güvenlik ayarlarını yapılandırmak için kullanılır: Akamai imza üst bilgisi kimlik doğrulaması anahtarları ve bu uç noktaya bağlanmak için izin verilen IP adresleri.<br />Bu özellik yalnızca zaman ayarlanabilir `cdnEnabled` false olarak ayarlanır.
- `cdnEnabled` -Bu akış uç noktası için Azure CDN tümleştirmesi etkin (varsayılan olarak devredışı) olup olmadığını gösterir. Ayarlarsanız `cdnEnabled` true olarak, aşağıdaki yapılandırmaları devre dışı: `customHostNames` ve `accessControl`.
  
    Tüm veri merkezlerini, Azure CDN tümleştirmesini desteklemiyor. Veri merkezinizde Azure CDN tümleştirmesi kullanılabilir sahip olup olmadığını denetlemek için aşağıdakileri yapın:
 
  - Ayarlamaya `cdnEnabled` true.
  - Denetlemek için döndürülen sonuç bir `HTTP Error Code 412` (PreconditionFailed), "Akış uç noktası CdnEnabled özellik CDN özelliği geçerli bölgede kullanılamıyor gibi true olarak ayarlanamaz." iletisi ile 

    Bu hatayı alırsanız veri merkezi bunu desteklemez. Başka bir veri merkezinde çalışmanız gerekir.
- `cdnProfile` - `cdnEnabled` Ayarlandığında true de geçirebilirsiniz `cdnProfile` değerleri. `cdnProfile` CDN profil adı, CDN uç noktası oluşturulacağı ' dir. Mevcut bir cdnProfile sağlamak veya yeni bir tane kullanın. Değer NULL ise ve `cdnEnabled` true, "AzureMediaStreamingPlatformCdnProfile" kullanılan varsayılan değer olan. Varsa sağlanan `cdnProfile` zaten varolan bir uç nokta altında oluşturulur. Profil mevcut değilse yeni bir profil otomatik olarak oluşturulur.
- `cdnProvider` -CDN etkinleştirildiğinde de geçirebilirsiniz `cdnProvider` değerleri. `cdnProvider` hangi sağlayıcısı kullanılacak denetler. Şu anda üç değerleri desteklenir: "StandardVerizon", "PremiumVerizon" ve "StandardAkamai". Hiçbir değer sağlanmışsa ve `cdnEnabled` true ise "StandardVerizon" (varsayılan değer olan) kullanılır.
- `crossSiteAccessPolicies` -Çeşitli istemciler için erişim ilkeleri siteler arası belirtmek için kullanılır. Daha fazla bilgi için [etki alanları arası ilke dosyası belirtimi](https://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html) ve [bir hizmet üzerinden etki alanı sınırlarında kullanılabilir hale getirme](https://msdn.microsoft.com/library/cc197955\(v=vs.95\).aspx).<br/>Ayarlar, yalnızca kesintisiz akış için geçerlidir.
- `customHostNames` -Bir akış için bir özel konak adı yönlendirilmiş trafiğini kabul edecek şekilde uç noktası yapılandırmak için kullanılır.  Bu özellik standart ve Premium akış uç noktaları için geçerlidir ve ne zaman ayarlanabilir `cdnEnabled`: false.
    
    Etki alanı sahipliğini, Media Services tarafından onaylanmalıdır. Media Services gerektirerek etki alanı adı sahipliğini doğrulayan bir `CName` Media Services hesabı kimliği olarak kullanılan etki alanına eklenmesi için bir bileşen içeren kayıt. Örneğin, "bir kayıt için akış uç noktası için bir özel konak adı olarak kullanılacak sports.contoso.com" için `<accountId>.contoso.com` Media Services doğrulama ana bilgisayar adlarından birini işaret edecek şekilde yapılandırılması gerekir. Doğrulama ana bilgisayar adı verifydns oluşur. \<mediaservices dns bölgesi >. 

    Farklı Azure bölgelerine için doğrulama kaydında kullanılacak beklenen DNS bölgeleri şunlardır:
  
  - Kuzey Amerika, Avrupa, Singapur, Hong Kong ÖİB, Japonya:
      
    - `media.azure.net`
    - `verifydns.media.azure.net`
      
  - Çin'de:
        
    - `mediaservices.chinacloudapi.cn`
    - `verifydns.mediaservices.chinacloudapi.cn`
        
    Örneğin, bir `CName` "945a4c4e-28ea-45 cd-8ccb-a519f6b700ad.contoso.com" için "verifydns.media.azure.net" eşleyen kaydı kanıtlar medya Hizmetleri kimliği 945a4c4e-28ea-45cd-8ccb-a519f6b700ad bu nedenle contoso.com etki alanının sahipliğini sahip Bu hesap altında bir akış uç noktası için bir özel konak adı olarak kullanılacak contoso.com altındaki herhangi bir ad etkinleştiriliyor. Medya hizmeti kimlik değerini bulmak için Git [Azure portalında](https://portal.azure.com/) ve medya hizmeti hesabınızı seçin. **Hesap kimliği** üstte görünür sayfanın sağ.
        
    Uygun bir doğrulaması olmadan bir özel konak adı ayarlama girişimi varsa `CName` kaydı, DNS yanıtının başarısız olur ve bir süre sonra önbelleğe alınabilir. Uygun bir kayıt yerleştirildikten sonra önbelleğe alınan yanıtın yeniden doğrulanır kadar biraz sürebilir. Özel etki alanı için DNS sağlayıcıya bağlı olarak, herhangi bir yere birkaç dakika veya saat kaydı düzeltin için ele geçirebilir.
        
    Ek olarak `CName` eşleyen `<accountId>.<parent domain>` için `verifydns.<mediaservices-dns-zone>`, başka oluşturmalısınız `CName` özel ana bilgisayar adı eşleyen (örneğin, `sports.contoso.com`), medya Hizmetleri akış uç noktanın ana bilgisayar adı (örneğin, `amstest-usea.streaming.media.azure.net`).
 
    > [!NOTE]
    > Akış uç noktaları aynı veri merkezinde bulunan paylaşamaz aynı özel ana bilgisayar adı.

    Şu anda, Media Services, SSL ile özel etki alanlarını desteklemiyor. 
    
- `maxCacheAge` -Geçersiz kılmalar varsayılan max-age HTTP önbellek denetimi akış uç noktasında medya parçasının ve isteğe bağlı bildirimlerini tarafından ayarlanan başlığı. Saniye cinsinden değeri ayarlanır.
- `resourceState` -

    - Durduruldu - akış uç noktası oluşturulduktan sonra başlangıç durumu
    - Çalışır duruma başlangıç - na geçiyor
    - -Çalışan istemciler için içerik akışı için
    - Ölçeklendirme - birimleri güncellenmekte ölçek artırabilir veya azaltılabilir
    - Durdurma - durdurulmuş duruma geçiş
    - Silme - siliniyor
    
- `scaleUnits` -200 MB/sn'lik artışlarla satın alınabilir adanmış çıkış kapasitesi sağlar. Taşımak gerekiyorsa bir **Premium** yazın, ayarlamak `scaleUnits`.

## <a name="working-with-cdn"></a>CDN ile çalışma

Çoğu durumda CDN'yi etkinleştirmeniz gerekir. Ancak eşzamanlı görüntüleyici sayısının 500'ün altında olmasını bekliyorsanız CDN en iyi ölçeklendirme performansını eşzamanlılık ile birlikte sunduğundan CDN'yi devre dışı bırakmanız önerilir.

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Akış uç noktası `hostname` ve CDN'yi etkinleştirme olup olmadığını akış URL'si aynı kalır.
* İçeriğinizi ile veya olmadan CDN test etme olanağı gerekiyorsa, başka bir akış CDN etkin olmayan uç noktası oluşturabilirsiniz.

### <a name="detailed-explanation-of-how-caching-works"></a>Ayrıntılı açıklaması ve önbelleğe alma nasıl işler?

CDN, akış uç noktası için bir CDN gereken bant genişliği miktarını etkin olduğundan ekleme değişir, belirli bir bant genişliği değer yoktur. Çok fazla içerik, ne kadar popüler olduğunu, bit hızlarına dönüştürme ve iletişim kuralları türüne bağlıdır. CDN ne istenen yalnızca önbelleği. Video parça önbelleğe sürece bu popüler içerik doğrudan CDN'den – hizmet anlamına gelir. Canlı içerik tam aynı şeyi izlemek çok kişi genellikle olduğundan önbelleğe olasıdır. Popüler ve bazıları da değil içerikler olabilir çünkü isteğe bağlı içerik biraz zor olabilir. Milyonlarca nerede bunların hiçbiri popüler (yalnızca 1 veya 2 görüntüleyiciler haftada) olan ancak binlerce kişinin yayınlanan tüm farklı videoları sahip video varlığını varsa, CDN daha az etkili olur. Bu önbellek isabetsizliği, akış uç noktasında yük artırın.
 
Ayrıca nasıl Uyarlamalı akış works göz önünde bulundurmanız gerekir. Kendi varlık olduğundan tek tek her video parçayı önbelleğe alınır. Örneğin, belirli bir video izlenen, ilk kez kişi yalnızca birkaç saniye izlemeyi geçici olarak atlarsa vardır ve burada yalnızca kişi izlenen ile ilişkili video parçasının CDN'de önbelleğe. Uyarlamalı akış ile video 5-7 farklı bit hızlarında genellikle sahiptir. Bir kişinin tek bit hızlı izliyor ve başka bir kişiye farklı bir bit hızı izliyor, ardından bunların her ayrı olarak CDN'de önbelleğe alınır. İki kişinin aynı hızı izlerken olsa bile, farklı protokoller üzerinden akış. Her bir protokol (HLS, MPEG-DASH, kesintisiz akış) ayrı olarak önbelleğe alınır. Bu nedenle her bit hızı ve protokol önbelleğe ayrı olarak ve istenen bu video parçasının önbelleğe alınır.

### <a name="enable-azure-cdn-integration"></a>Azure CDN tümleştirmesini etkinleştirme

Bir akış uç noktası ile sağlandıktan sonra akış uç noktası CDN uç noktasına eşleme DNS güncelleştirme yapılmadan önce etkinleştirilirse CDN Media Services'da tanımlanmış bekleme süresini olduğu.

Daha sonra devre dışı bırak / CDN kullanılabilir hale getirmek isterseniz, akış uç noktanızı olmalıdır **durduruldu** durumu. Bu iki saate kadar etkin Azure CDN tümleştirmesi ve değişikliklerin tüm CDN POP'larına arasında etkin olması ele geçirebilir. Ancak, kullanabilirsiniz akış uç noktası ve kesintileri olmadan akışı akış uç noktasından Başlat ve tümleştirme tamamlandıktan sonra akışı CDN'den sağlanır. Akış uç noktanızı olacaktır sağlama süresi boyunca **başlangıç** durumu ve performans gözlemleyin.

Standart akış uç noktası oluşturulduğunda, varsayılan olarak standart Verizon ile yapılandırılır. REST API'lerini kullanarak Premium Verizon veya Akamai Standard sağlayıcılarını yapılandırabilirsiniz. 

CDN tümleştirmesi, Çin ve Federal devlet bölgeler dışındaki tüm Azure veri merkezlerinde etkinleştirilir.

> [!IMPORTANT]
> Azure CDN ile Azure Media Services tümleştirmesi uygulandığını **verizon'dan Azure CDN** standart akış uç noktaları. Premium akış uç noktaları kullanarak tüm yapılandırılabilir **Azure CDN fiyatlandırma katmanları ve sağlayıcıları**. Azure CDN özellikleri hakkında daha fazla bilgi için bkz: [CDN'ye genel bakış](../../cdn/cdn-overview.md).

### <a name="determine-if-dns-change-has-been-made"></a>DNS değişiklik yapılıp yapılmadığını belirlemek

Bir akış (trafiği yönlendirildiğinde Azure CDN) uç noktasında DNS değişiklik yapılıp yapılmadığını kullanarak belirleyebilirsiniz https://www.digwebinterface.com. Sonuçları varsa azureedge.net etki alanı adları sonuçlarda, trafiği artık CDN işaret edilip.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, görüşlerinizi, güncelleştirmeleri alın

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını görmek için makaleyi.

## <a name="next-steps"></a>Sonraki adımlar

Örnek [bu depodaki](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) varsayılan akış uç .NET ile başlatmak gösterilmektedir.

