---
title: "Azure içerik teslim ağı'nda web içeriğinin kullanım süresini yönetme | Microsoft Docs"
description: "Azure CDN Azure Web Apps/bulut Hizmetleri, ASP.NET ve IIS içeriğinin kullanım süresini yönetme öğrenin."
services: cdn
documentationcenter: .NET
author: zhangmanling
manager: erikre
editor: 
ms.assetid: bef53fcc-bb13-4002-9324-9edee9da8288
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/10/2017
ms.author: mazha
ms.openlocfilehash: dca6ca5f21f4a4f1701af57eb40d92094b6a4754
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="manage-expiration-of-web-content-in-azure-content-delivery-network"></a>Azure içerik teslim ağı'nda web içeriğinin kullanım süresini yönetme
> [!div class="op_single_selector"]
> * [Azure web içeriği](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Depolama](cdn-manage-expiration-of-blob-content.md)
> 

Kendi yaşam süresi (TTL) geçen kadar herhangi bir genel olarak erişilebilir kaynak web sunucusuna dosyalarından Azure içerik teslim ağı (CDN) önbelleğe alınabilir. TTL değeri tarafından belirlenir `Cache-Control` kaynak sunucudan HTTP yanıt üstbilgisi. Bu makalede nasıl ayarlanacağını açıklar `Cache-Control` Microsoft Azure App Service, Azure bulut Hizmetleri, ASP.NET uygulamaları ve bunların tümü benzer şekilde yapılandırılmış, Internet Information Services (IIS) siteleri Web Apps özelliği için üstbilgiler. Ayarlayabileceğiniz `Cache-Control` üstbilgisi yapılandırma dosyalarını kullanarak veya program aracılığıyla. 

Önbellek ayarları Azure portalından ayarlayarak da kontrol edebilirsiniz [kuralları önbelleğe alma CDN](cdn-caching-rules.md). Bir veya daha fazla önbelleğe alma kurallarını ve önbelleğe alma davranışlarını kümesine **geçersiz kılma** veya **atlama önbellek**, bu makalede ele alınan kaynak tarafından sağlanan önbelleğe alma ayarlarını göz ardı edilir. Genel önbelleğe alma kavramları hakkında daha fazla bilgi için bkz: [önbelleğe alma nasıl çalışır](cdn-how-caching-works.md).

> [!TIP]
> Bir dosyada hiçbir TTL ayarlamayı da seçebilirsiniz. Bu durumda, Azure portalında kurallar önbelleğe almayı kurmak ayarlamazsanız Azure CDN varsayılan TTL yedi gün otomatik olarak uygular. Bu varsayılan TTL yalnızca genel web teslim iyileştirmeler için geçerlidir. Büyük dosya en iyi duruma getirme, varsayılan TTL bir gündür ve en iyi duruma getirme akış medya için TTL bir yıl varsayılandır.
> 
> Azure CDN dosyaları ve diğer kaynaklara erişimi hızlandırmak için nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure içerik teslim ağı'ne genel bakış](cdn-overview.md).
> 

## <a name="setting-cache-control-headers-by-using-configuration-files"></a>Cache-Control üst bilgileri yapılandırma dosyalarını kullanarak ayarlama
Görüntüleri ve stil sayfalarını gibi statik içerik güncelleştirme sıklığı değiştirerek denetleyebilirsiniz **applicationHost.config** veya **Web.config** web uygulamanız için yapılandırma dosyaları. `<system.webServer>/<staticContent>/<clientCache>` Ya da dosya kümeleri öğesinde `Cache-Control` içeriğiniz için üstbilgi.

### <a name="using-applicationhostconfig-files"></a>ApplicationHost.config dosyalarını kullanma
**ApplicationHost.config** IIS yapılandırma sistemini kök dosyanın bir dosyadır. Yapılandırma ayarlarında bir **ApplicationHost.config** dosya sitedeki tüm uygulamaları etkiler, ancak herhangi ayarları tarafından geçersiz kılınır **Web.config** mevcut dosyaların bir web uygulaması.

### <a name="using-webconfig-files"></a>Web.config dosyalarını kullanma
İle bir **Web.config** dosya, tüm web uygulamanızı veya belirli bir dizin, web uygulamanızın üzerinde davranışını özelleştirebilirsiniz. Genellikle, en az bir tane **Web.config** web uygulamanızın kök klasörü içinde dosya. Her **Web.config** dosya bunlar en alt düzeyinde bir başkası tarafından geçersiz kılınır sürece belirli bir klasörde yapılandırma ayarları her şey bu klasöre ve alt klasörlerinde etkileyen **Web.config**dosya. Örneğin, ayarlayabileceğiniz bir `<clientCache>` öğesinde bir **Web.config** web uygulamanız için üç gün boyunca tüm statik içeriği önbelleğe almak için web uygulamanızın kök klasörü içinde dosya. De ekleyebilirsiniz bir **Web.config** dosyasını daha değişken içeriği olan bir alt klasör (örneğin, `\frequent`) ve kendi `<clientCache>` öğesi için altı saat alt klasörün içeriği önbelleğe. Tüm bu içeriğe sonucunda olduğu web sitesi önbelleğe alınır içerik dışında üç gün `\frequent` yalnızca altı saat önbelleğe dizin.  

Aşağıdaki XML örneği nasıl ayarlanacağını gösterir `<clientCache>` öğesi bir yapılandırma dosyasına üç gün yaş üst sınırını belirtin:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Kullanılacak **cacheControlMaxAge** öznitelik değerini ayarlamalısınız **cacheControlMode** özniteliğini `UseMaxAge`. Bu ayar HTTP üstbilgisi ve yönerge neden `Cache-Control: max-age=<nnn>`, yanıta eklenecek. Timespan değeri biçimi **cacheControlMaxAge** özniteliği `<days>.<hours>:<min>:<sec>`. Değeri saniye dönüştürülür ve değeri olarak kullanılan `Cache-Control` `max-age` yönergesi. Hakkında daha fazla bilgi için `<clientCache>` öğesi, bkz: [istemci önbelleği <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-programmatically"></a>Cache-Control üst bilgileri programlı olarak ayarlama
ASP.NET uygulamaları için programlı olarak ayarlayarak önbelleğe alma davranışı CDN kontrol **HttpResponse.Cache** .NET API özelliği. Hakkında bilgi için **HttpResponse.Cache** özelliği, bkz: [HttpResponse.Cache özelliği](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) ve [HttpCachePolicy sınıfı](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Program aracılığıyla önbellek uygulama içeriği için ASP.NET, aşağıdaki adımları izleyin:
   1. İçerik ayarlayarak olarak alınabilir işaretli olduğundan emin olun `HttpCacheability` için `Public`. 
   2. Aşağıdakilerden birini çağırarak bir önbellek Doğrulayıcı ayarlamak `HttpCachePolicy` yöntemleri:
      - Çağrı `SetLastModified` bir zaman damgası değerini ayarlamak için `Last-Modified` üstbilgi.
      - Çağrı `SetETag` için bir değer ayarlamak için `ETag` üstbilgi.
   3. İsteğe bağlı olarak, çağırarak bir önbellek sona erme zamanı belirtin `SetExpires` için bir değer ayarlamak için `Expires` üstbilgi. Aksi takdirde, bu belgede daha önce açıklanan varsayılan önbellek buluşsal yöntemleri uygulayın.

Örneğin, bir saat için içerik önbellek için aşağıdaki C# kodu ekleyin:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="testing-the-cache-control-header"></a>Cache-Control üstbilgisinin test etme
Web içeriğinize TTL ayarlarını kolayca doğrulayabilirsiniz. Tarayıcınızın ile [Geliştirici Araçları](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), web içeriğinize içeren test `Cache-Control` yanıtı üstbilgisi. Bir aracı gibi kullanabilir **wget**, [Postman](https://www.getpostman.com/), veya [Fiddler](http://www.telerik.com/fiddler) yanıt üstbilgileri incelemek için.

## <a name="next-steps"></a>Sonraki Adımlar
* [Ayrıntıları okuyun **clientCache** öğesi](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Belgelerini okuyun **HttpResponse.Cache** özelliği](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Belgelerini okuyun **HttpCachePolicy sınıfı**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx)  
* [Kavramları önbelleğe alma hakkında bilgi edinin](cdn-how-caching-works.md)
