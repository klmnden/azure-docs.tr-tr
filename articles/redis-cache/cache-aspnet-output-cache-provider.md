---
title: "Önbelleği ASP.NET çıktı önbelleği sağlayıcısı"
description: "ASP.NET sayfası Azure Redis önbelleği kullanılarak çıktı önbellek öğrenin"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 78469a66-0829-484f-8660-b2598ec60fbf
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/14/2017
ms.author: sdanie
ms.openlocfilehash: 845f25637a0e48460fc76c1ee36060274b3cec38
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>ASP.NET çıktı önbelleği sağlayıcısı Azure Redis önbelleği
Redis çıkış önbelleği sağlayıcısı, çıktı önbelleği veriler için bir işlem dışı depolama mekanizmasıdır. Bu veriler için özellikle tam HTTP yanıtları edinilebilir (sayfa çıkış önbelleğe alma). Sağlayıcı, ASP.NET 4'te tanıtılan yeni çıkış önbelleği sağlayıcısı genişletilebilirlik noktası içine takılan.

Çıktı önbelleği Redis sağlayıcısını kullanmak için ilk önbelleğiniz yapılandırın ve Redis çıkış önbelleği sağlayıcısı NuGet paketi kullanarak ASP.NET uygulamanızı yapılandırmak için. Bu konu, uygulamanızı Redis çıkış önbelleği sağlayıcısı kullanacak şekilde yapılandırma hakkında yönergeler sağlar. Oluşturma ve bir Azure Redis önbelleği örneği yapılandırma hakkında daha fazla bilgi için bkz: [bir önbellek oluşturma](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>ASP.NET sayfası çıktı önbelleğinde depolamak
Redis önbelleği oturum durumu NuGet paketini kullanarak Visual Studio'da bir istemci uygulamasını yapılandırmak için tıklatın **NuGet Paket Yöneticisi**, **Paket Yöneticisi Konsolu** gelen **Araçları** menüsü.

`Package Manager Console` penceresinden aşağıdaki komutu çalıştırın.
    
```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

Redis çıkış önbelleği sağlayıcısı NuGet paketi StackExchange.Redis.StrongName paketi bir bağımlılığa sahiptir. StackExchange.Redis.StrongName paket projenizde mevcut değilse, yüklenir. Redis çıkış önbelleği sağlayıcısı NuGet paketi hakkında daha fazla bilgi için bkz: [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet sayfası.

>[!NOTE]
>Tanımlayıcı adlı StackExchange.Redis.StrongName paket ek olarak, ayrıca olmayan-tanımlayıcı adlı StackExchange.Redis sürümü vardır. Projenizi kaldırmalısınız olmayan-tanımlayıcı adlı StackExchange.Redis sürümünü kullanıyorsanız, aksi takdirde, projenizde adlandırma çakışmaları alın. Bu paketleri hakkında daha fazla bilgi için bkz: [yapılandırma .NET önbellek istemcilerinin](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

NuGet paketi indirir ve gerekli derleme başvurularını ekler ve aşağıdaki bölümü web.config dosyasına ekler. Bu bölüm Redis çıkış önbelleği sağlayıcısı kullanmak, ASP.NET uygulamanız için gerekli yapılandırmayı içerir.

```xml
<caching>
  <outputCachedefault Provider="MyRedisOutputCache">
    <providers>
      <!--
      <add name="MyRedisOutputCache"
        host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "5000" [number]
      />
      -->
      <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
    </providers>
  </outputCache>
</caching>
```

Açıklamalı bölüm öznitelikleri ve örnek ayarlarını her özniteliği için bir örnek sağlar.

Microsoft Azure Portal'da önbellek dikey pencerenizin değerlerle öznitelikleri yapılandırma ve diğer değerleri istediğiniz gibi yapılandırın. Önbellek özelliklerine erişme ile ilgili yönergeler için bkz: [Redis önbelleği ayarlarını yapılandırma](cache-configure.md#configure-redis-cache-settings).

* **ana bilgisayar** – önbellek uç noktasını belirtin.
* **bağlantı noktası** – SSL olmayan bağlantı noktası veya ssl ayarlarına bağlı olarak, SSL bağlantı noktası kullanın.
* **accessKey** – önbelleğiniz için birincil veya ikincil anahtarı kullanın.
* **SSL** – önbelleği/istemci iletişimleri ssl ile güvenli hale getirmek istiyorsanız True, aksi takdirde false. Doğru bağlantı noktasına belirttiğinizden emin olun.
  * SSL olmayan bağlantı noktasın yeni önbellekler için varsayılan olarak devre dışı bırakılmıştır. SSL bağlantı noktası kullanmak üzere bu ayarı TRUE belirtin. SSL olmayan bağlantı noktası etkinleştirme hakkında daha fazla bilgi için bkz: [erişim bağlantı noktaları](cache-configure.md#access-ports) bölümüne [bir önbellek yapılandırma](cache-configure.md) konu.
* **Databaseıd** – belirtilen önbelleğe yönelik kullanmak için hangi veritabanı çıkış verileri. Belirtilmezse, varsayılan değer 0 kullanılır.
* **applicationName** – anahtarları redis depolanır `<AppName>_<SessionId>_Data`. Bu adlandırma şeması aynı anahtarı paylaşmak birden çok uygulama sağlar. Bu parametre isteğe bağlıdır ve onu belirtmezseniz, varsayılan değer kullanılır.
* **connectionTimeoutInMilliseconds** – Bu ayar, StackExchange.Redis istemcisi connectTimeout ayarında geçersiz kılmanıza olanak sağlar. Belirtilmezse, varsayılan connectTimeout ayarını 5000 kullanılır. Daha fazla bilgi için bkz: [StackExchange.Redis yapılandırma modeli](http://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** – Bu ayar, StackExchange.Redis istemcisi syncTimeout ayarında geçersiz kılmanıza olanak sağlar. Belirtilmezse, varsayılan syncTimeout ayarı 1000 kullanılır. Daha fazla bilgi için bkz: [StackExchange.Redis yapılandırma modeli](http://go.microsoft.com/fwlink/?LinkId=398705).

Bir OutputCache yönergesi, çıktıyı önbelleğe istediğiniz her sayfasına ekleyin.

```
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

Önceki örnekte, önbelleğe alınmış bir sayfa verileri kalır önbellekte 60 saniye boyunca ve sayfayı farklı bir sürümünü her parametre birleşimi için önbelleğe alınır. OutputCache yönergesi hakkında daha fazla bilgi için bkz: [ @OutputCache ](http://go.microsoft.com/fwlink/?linkid=320837).

Bu adımları gerçekleştirdikten sonra uygulamanızı Redis çıkış önbelleği sağlayıcısı kullanacak şekilde yapılandırılır.

## <a name="next-steps"></a>Sonraki adımlar
Kullanıma [Azure Redis önbelleği için ASP.NET oturum durumu sağlayıcısı](cache-aspnet-session-state-provider.md).

