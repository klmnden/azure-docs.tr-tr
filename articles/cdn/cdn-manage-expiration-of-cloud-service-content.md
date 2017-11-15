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
ms.openlocfilehash: d58a245923242b3963b188ca869e8290d999c0a2
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="manage-expiration-of-web-content-in-azure-content-delivery-network"></a>Azure içerik teslim ağı'nda web içeriğinin kullanım süresini yönetme
 Azure CDN'de
> [!div class="op_single_selector"]
> * [Azure Web Apps/bulut Hizmetleri, ASP.NET ve IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Depolama](cdn-manage-expiration-of-blob-content.md)
> 

Kendi yaşam süresi (TTL) geçen kadar herhangi bir genel olarak erişilebilir kaynak web sunucusuna dosyalarından Azure içerik teslim ağı (CDN) önbelleğe alınabilir. TTL değeri tarafından belirlenir [ `Cache-Control` üstbilgi](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) kaynak sunucudan HTTP yanıt. Bu makalede nasıl ayarlanacağını açıklar `Cache-Control` Web Apps özelliğini Microsoft Azure App Service, Azure bulut Hizmetleri, ASP.NET uygulamaları ve bunların tümü benzer şekilde yapılandırılmış, Internet Information Services siteleri için üstbilgiler.

> [!TIP]
> Bir dosyada hiçbir TTL ayarlamayı da seçebilirsiniz. Bu durumda, Azure CDN varsayılan TTL yedi gün otomatik olarak uygular.
> 
> Azure CDN dosyaları ve diğer kaynaklara erişimi hızlandırmak için nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure içerik teslim ağı'ne genel bakış](cdn-overview.md).
> 

## <a name="setting-cache-control-headers-in-configuration-files"></a>Cache-Control üst bilgileri yapılandırma dosyalarında ayarlama
Görüntüleri ve stil sayfalarını gibi statik içerik güncelleştirme sıklığı değiştirerek denetleyebilirsiniz **applicationHost.config** veya **web.config** web uygulamanız için dosyaları. **System.webServer\staticContent\clientCache** yapılandırma dosyası kümeleri öğesinde `Cache-Control` içeriğiniz için üstbilgi. İçin **web.config** dosyaları, yapılandırma ayarlarını etkileyen her şeyi klasörü ve alt klasörlerinde ve alt düzeyde kılınmadıkça. Örneğin, üç gün boyunca tüm statik içeriği önbelleğe almak için kök klasöründe bir varsayılan TTL ayar ayarlayın ve yalnızca altı saat içeriğini önbelleğe almak için bir alt klasörü daha değişken içerik ayarlayın. Ancak **applicationHost.config** dosya ayarlarını etkileyen sitedeki tüm uygulamaları, var olan ayarları tarafından geçersiz kılındı **web.config** uygulamalarında dosyaları.

Aşağıdaki XML örneği nasıl ayarlanacağını gösterir **clientCache** üç gün yaş üst sınırını belirtmek için:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Belirtme **UseMaxAge** neden olan bir `Cache-Control: max-age=<nnn>` belirtilen değere dayanarak yanıta eklenecek üstbilgi **CacheControlMaxAge** özniteliği. İçin timespan biçimi **cacheControlMaxAge** özniteliği `<days>.<hours>:<min>:<sec>`. Hakkında daha fazla bilgi için **clientCache** düğümü, bkz: [istemci önbelleği <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Cache-Control üstbilgileri kodda ayarlama
ASP.NET uygulamaları için programlı olarak ayarlayarak önbelleğe alma davranışı CDN denetleyebilirsiniz **HttpResponse.Cache** özelliği. Hakkında daha fazla bilgi için **HttpResponse.Cache** özelliği, bkz: [HttpResponse.Cache özelliği](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) ve [HttpCachePolicy sınıfı](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Program aracılığıyla önbellek uygulama içeriği için ASP.NET, aşağıdaki adımları izleyin:
   1. İçerik ayarlayarak olarak alınabilir işaretli olduğundan emin olun `HttpCacheability` için *ortak*. 
   2. Aşağıdaki yöntemlerden birini çağırarak bir önbellek Doğrulayıcı ayarlayın:
      - Çağrı `SetLastModified` LastModified zaman damgası ayarlamak için.
      - Çağrı `SetETag` ayarlamak için bir `ETag` değeri.
   3. İsteğe bağlı olarak, çağırarak bir önbellek sona erme zamanı belirtin `SetExpires`. Aksi takdirde, bu belgede daha önce açıklanan varsayılan önbellek buluşsal yöntemleri uygulayın.

Örneğin, bir saat için içerik önbellek için aşağıdaki C# kodu ekleyin:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Sonraki Adımlar
* [Ayrıntıları okuyun **clientCache** öğesi](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Belgelerini okuyun **HttpResponse.Cache** özelliği](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Belgelerini okuyun **HttpCachePolicy sınıfı**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

