---
title: "Azure CDN web içeriğinin bitiş tarihini Yönet | Microsoft Docs"
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
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: c207d780857a61d4b1fc0f39e6185cae67abc955
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Azure CDN Azure Web Apps/bulut Hizmetleri, ASP.NET ve IIS içeriğinin kullanım süresini yönetme
> [!div class="op_single_selector"]
> * [Azure Web Apps/bulut Hizmetleri, ASP.NET ve IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure depolama blob hizmeti](cdn-manage-expiration-of-blob-content.md)
> 
> 

Kendi yaşam süresi (TTL) geçen kadar herhangi bir genel olarak erişilebilir kaynak web sunucusuna dosyalarından Azure CDN'de önbelleğe alınabilir.  TTL değeri tarafından belirlenir [ *Cache-Control* üstbilgi](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) kaynak sunucudan HTTP yanıt.  Bu makalede nasıl ayarlanacağını açıklar `Cache-Control` Azure Web uygulamaları, Azure bulut Hizmetleri, ASP.NET uygulamaları ve bunların tümü benzer şekilde yapılandırılmış, Internet Information Services siteleri için üstbilgiler.

> [!TIP]
> Bir dosyada hiçbir TTL ayarlamak tercih edebilirsiniz.  Bu durumda, Azure CDN varsayılan TTL yedi gün otomatik olarak uygular.
> 
> Azure CDN dosyaları ve diğer kaynaklara erişimi hızlandırmak için nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure CDN'ye genel bakış](cdn-overview.md).
> 
> 

## <a name="setting-cache-control-headers-in-configuration"></a>Cache-Control üstbilgileri yapılandırmasında ayarlama
Görüntüleri ve stil sayfalarını gibi statik içerik güncelleştirme sıklığı değiştirerek denetleyebilirsiniz **applicationHost.config** veya **web.config** web uygulamanız için dosyaları.  **System.webServer\staticContent\clientCache** öğesi yapılandırma dosyasında ayarlayacak `Cache-Control` içeriğiniz için üstbilgi. İçin **web.config**, yapılandırma ayarlarını her şeyi klasörü ve tüm alt klasörlerde en alt düzeyinde geçersiz kılınmadığı sürece etkiler.  Örneğin, bir varsayılan yaşam süresi 3 gün boyunca önbelleğe alınmış tüm statik içeriğe sahip kök belirlenmiş ancak 6 saat önbellek ayarı olan daha değişken içeriğe sahip bir alt sahip.  İçin **applicationHost.config**, sitedeki tüm uygulamaları etkiler ancak geçersiz kılınabilir **web.config** uygulamalarında dosyaları.

Aşağıdaki XML ayar örneği gösterilmektedir **clientCache** 3 gün yaş üst sınırını belirtmek için:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Belirtme **UseMaxAge** ekler bir `Cache-Control: max-age=<nnn>` üstbilgisi yanıt için belirtilen değere dayanarak **CacheControlMaxAge** özniteliği. Timespan biçimi içindir **cacheControlMaxAge** özniteliği <days>.<hours>:<min>:<sec>. Daha fazla bilgi için **clientCache** düğümü, bkz: [istemci önbelleği <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Cache-Control üstbilgileri kodda ayarlama
ASP.NET uygulamaları için programlı olarak ayarlayarak önbelleğe alma davranışı CDN ayarlayabilirsiniz **HttpResponse.Cache** özelliği. Daha fazla bilgi için **HttpResponse.Cache** özelliği, bkz: [HttpResponse.Cache özelliği](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) ve [HttpCachePolicy sınıfı](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Program aracılığıyla önbellek uygulama içeriği ASP.NET istiyorsanız, içerik HttpCacheability ayarlayarak olarak alınabilir işaretli olduğundan emin olun *ortak*. Ayrıca, bir önbellek Doğrulayıcı ayarlandığından emin olun. Son değişiklik önbellek Doğrulayıcı olabilir SetLastModified ya da bir etag değeri aranarak zaman damgası ayarlamak SetETag çağırarak. İsteğe bağlı olarak, SetExpires çağırarak bir önbellek süre sonu zamanı belirtebilirsiniz ya da bu belgenin önceki bölümlerinde açıklanan varsayılan önbellek buluşsal yöntemler üzerinde güvenebilirsiniz.  

Örneğin, bir saat için içerik önbelleği için aşağıdakileri ekleyin:  

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

