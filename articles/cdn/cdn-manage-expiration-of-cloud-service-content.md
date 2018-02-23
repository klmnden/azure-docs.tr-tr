---
title: "Azure içerik teslim ağı'nda web içeriğinin kullanım süresini yönetme | Microsoft Docs"
description: "Azure CDN Azure Web Apps/bulut Hizmetleri, ASP.NET ve IIS içeriğinin kullanım süresini yönetme öğrenin."
services: cdn
documentationcenter: .NET
author: dksimpson
manager: akucer
editor: 
ms.assetid: bef53fcc-bb13-4002-9324-9edee9da8288
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/15/2018
ms.author: mazha
ms.openlocfilehash: db7b5053cb926d2ec86c7feea4ac411acbeb1ae2
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="manage-expiration-of-web-content-in-azure-content-delivery-network"></a>Azure içerik teslim ağı'nda web içeriğinin kullanım süresini yönetme
> [!div class="op_single_selector"]
> * [Azure web içeriği](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Depolama](cdn-manage-expiration-of-blob-content.md)
> 

Kendi yaşam süresi (TTL) geçen kadar genel olarak erişilebilir özgün web sunucularından dosyaları Azure içerik teslim ağı (CDN) önbelleğe alınabilir. TTL değeri tarafından belirlenir `Cache-Control` kaynak sunucudan HTTP yanıt üstbilgisi. Bu makalede nasıl ayarlanacağını açıklar `Cache-Control` Microsoft Azure App Service, Azure bulut Hizmetleri, ASP.NET uygulamaları ve bunların tümü benzer şekilde yapılandırılmış, Internet Information Services (IIS) siteleri Web Apps özelliği için üstbilgiler. Ayarlayabileceğiniz `Cache-Control` üstbilgisi yapılandırma dosyalarını kullanarak veya program aracılığıyla. 

Önbellek ayarları Azure portalından ayarlayarak da kontrol edebilirsiniz [kuralları önbelleğe alma CDN](cdn-caching-rules.md). Oluşturmanız veya daha fazla önbelleğe alma kurallarını ve önbelleğe alma davranışlarını kümesine **geçersiz kılma** veya **atlama önbellek**, bu makalede ele alınan kaynak tarafından sağlanan önbelleğe alma ayarlarını göz ardı edilir. Genel önbelleğe alma kavramları hakkında daha fazla bilgi için bkz: [önbelleğe alma nasıl çalışır](cdn-how-caching-works.md).

> [!TIP]
> Bir dosyada hiçbir TTL ayarlamayı da seçebilirsiniz. Bu durumda, Azure portalında kurallar önbelleğe almayı kurmak ayarladınız sürece Azure CDN varsayılan TTL yedi gün otomatik olarak uygular. Bu varsayılan TTL yalnızca genel web teslim iyileştirmeler için geçerlidir. Büyük dosya en iyi duruma getirme, varsayılan TTL bir gündür ve en iyi duruma getirme akış medya için TTL bir yıl varsayılandır.
> 
> Azure CDN dosyaları ve diğer kaynaklara erişimi hızlandırmak için nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure içerik teslim ağı'ne genel bakış](cdn-overview.md).
> 

## <a name="setting-cache-control-headers-by-using-cdn-caching-rules"></a>Cache-Control üstbilgileri kuralları önbelleğe alma CDN kullanarak ayarlama
Web sunucunuzun ayar için tercih edilen yöntem `Cache-Control` başlığıdır Azure portalında önbelleğe alma kurallarını kullanmak için. CDN kuralları önbelleğe alma hakkında daha fazla bilgi için bkz: [denetim Azure CDN kuralları önbelleğe alma ile önbelleğe alma davranışı](cdn-caching-rules.md).

> [!NOTE] 
> Önbelleğe alma kuralları yalnızca kullanılabilir **Azure CDN Verizon standardı** ve **Azure CDN Akamai standardı** profilleri. İçin **Azure CDN Verizon Premium'a** profilleri kullanmalıdır [Azure CDN kurallar altyapısı](cdn-rules-engine.md) içinde **Yönet** benzer işlevselliği için portal.

**CDN önbelleğe alma Kurallar sayfasına gitmek için**:

1. Azure portalında bir CDN profili seçin, sonra web sunucusu için uç nokta seçin.

2. Ayarları altındaki sol bölmede seçin **kuralları önbelleğe alma**.

   ![CDN önbelleğe alma kurallarını düğmesi](./media/cdn-manage-expiration-of-cloud-service-content/cdn-caching-rules-btn.png)

   **Kuralları önbelleğe alma** sayfası görüntülenir.

   ![CDN önbelleğe alma sayfası](./media/cdn-manage-expiration-of-cloud-service-content/cdn-caching-page.png)


**Genel önbelleğe alma kurallarını kullanarak bir web sunucusunun Cache-Control üstbilgileri ayarlamak için:**

1. Altında **genel kurallar önbelleğe alma**ayarlayın **sorgu dizesini önbelleğe alma davranışı** için **sorgu dizelerini yoksayabilir** ve **önbelleğe alma davranışı** için **Geçersiz kılma**.
      
2. İçin **önbelleğe sona erme süresi**, 3600 içinde girin **saniye** kutusu veya 1'de **saatleri** kutusu. 

   ![CDN genel önbelleğe alma kuralları örneği](./media/cdn-manage-expiration-of-cloud-service-content/cdn-global-caching-rules-example.png)

   Bu genel önbellek kuralını önbellek süresi bir saat ayarlar ve uç nokta için tüm istekleri etkiler. Tüm geçersiz kılmaları `Cache-Control` veya `Expires` bitiş noktası tarafından belirtilen kaynak sunucu tarafından gönderilen HTTP üstbilgileri.   

3. **Kaydet**’i seçin.

**Özel önbelleğe alma kurallarını kullanarak bir web sunucusu dosyanın Cache-Control üstbilgileri ayarlamak için:**

1. Altında **özel kurallar önbelleğe alma**, iki eşleşme koşullar oluşturun:

     a. İlk eşleşme koşulu için **eşleşen koşulu** için **yolu** ve girin `/webfolder1/*` için **eşleşen değeri**. Ayarlama **önbelleğe alma davranışı** için **geçersiz kılma** ve 4'te girin **saatleri** kutusu.

     b. İkinci eşleşme koşulu için **eşleşen koşulu** için **yolu** ve girin `/webfolder1/file1.txt` için **eşleşen değeri**. Ayarlama **önbelleğe alma davranışı** için **geçersiz kılma** ve 2'de girin **saatleri** kutusu.

    ![CDN özel önbelleğe alma kuralları örneği](./media/cdn-manage-expiration-of-cloud-service-content/cdn-custom-caching-rules-example.png)

    İlk özel önbellek kuralını dört saat dosyalar için önbellek süresini ayarlar `/webfolder1` , bitiş noktası tarafından belirtilen kaynak sunucuda klasör. İlk kural için ikinci kuralı geçersiz kılar `file1.txt` yalnızca dosya ve onun için iki saatlik bir önbellek süresi ayarlar.

2. **Kaydet**’i seçin.


## <a name="setting-cache-control-headers-by-using-configuration-files"></a>Cache-Control üst bilgileri yapılandırma dosyalarını kullanarak ayarlama
Görüntüleri ve stil sayfalarını gibi statik içerik güncelleştirme sıklığı değiştirerek denetleyebilirsiniz **applicationHost.config** veya **Web.config** web uygulamanız için yapılandırma dosyaları. Ayarlamak için `Cache-Control` kullanım içeriğiniz için üstbilgi `<system.webServer>/<staticContent>/<clientCache>` ya da dosyadaki öğe.

### <a name="using-applicationhostconfig-files"></a>ApplicationHost.config dosyalarını kullanma
**ApplicationHost.config** IIS yapılandırma sistemini kök dosyanın bir dosyadır. Yapılandırma ayarlarında bir **ApplicationHost.config** dosya sitedeki tüm uygulamaları etkiler, ancak herhangi ayarları tarafından geçersiz kılınır **Web.config** mevcut dosyaların bir web uygulaması.

### <a name="using-webconfig-files"></a>Web.config dosyalarını kullanma
İle bir **Web.config** dosya, tüm web uygulamanızı veya belirli bir dizin, web uygulamanızın üzerinde davranışını özelleştirebilirsiniz. Genellikle, en az bir tane **Web.config** web uygulamanızın kök klasörü içinde dosya. Her **Web.config** dosya bunlar en alt düzeyinde bir başkası tarafından geçersiz kılınır sürece belirli bir klasörde yapılandırma ayarları her şey bu klasöre ve alt klasörlerinde etkileyen **Web.config** dosya. 

Örneğin, ayarlayabileceğiniz bir `<clientCache>` öğesinde bir **Web.config** web uygulamanız için üç gün boyunca tüm statik içeriği önbelleğe almak için web uygulamanızın kök klasörü içinde dosya. De ekleyebilirsiniz bir **Web.config** dosyasını daha değişken içeriği olan bir alt klasör (örneğin, `\frequent`) ve kendi `<clientCache>` öğesi için altı saat alt klasörün içeriği önbelleğe. Tüm bu içeriğe sonucunda olduğu web sitesi için herhangi bir içerik dışında üç gün boyunca önbellekte `\frequent` yalnızca altı saat önbelleğe dizin.  

Aşağıdaki XML yapılandırma dosyası örneği nasıl ayarlanacağını gösterir `<clientCache>` öğesi üç gün yaş üst sınırını belirtin:  

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
