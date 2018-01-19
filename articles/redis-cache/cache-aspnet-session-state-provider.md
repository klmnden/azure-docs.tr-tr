---
title: "Önbellek ASP.NET oturum durumu sağlayıcısı | Microsoft Docs"
description: "Azure Redis önbelleği kullanılarak ASP.NET oturum durumu depolamayı öğrenin"
services: redis-cache
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: tysonn
ms.assetid: 192f384c-836a-479a-bb65-8c3e6d6522bb
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/01/2017
ms.author: wesmc
ms.openlocfilehash: 485375f2f2ffb83b7d0fdeef8daab5880a8bbc27
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-session-state-provider-for-azure-redis-cache"></a>Azure Redis Önbelleği için ASP.NET Oturum Durumu Sağlayıcısı
Azure Redis önbelleği bellek içi yerine bir önbellek veya SQL Server veritabanında, oturum durumunu depolamak için kullanabileceğiniz bir oturum durumu sağlayıcısı sağlar. Önbelleğe alma oturum durumu sağlayıcısı kullanmak için önbelleğinizi önce yapılandırın ve ardından Redis önbelleği oturum durumu NuGet paketi kullanarak önbelleği için ASP.NET uygulamanızı yapılandırma.

Bunu önlemek için bir kullanıcı oturum durumu çeşit depolamak için bir gerçek bulut uygulamasında pratik çoğunlukla değildir, ancak bazı yaklaşımlar diğerlerinden daha fazla performans ve ölçeklenebilirliği etkileyen. Durumunu depolamak varsa, durum miktarını küçük tutun ve tanımlama bilgilerini depolamak için en iyi çözüm olur. Uygun değilse, sonraki en iyi çözüm ASP.NET oturum durumu sahip bir sağlayıcı için dağıtılmış, bellek içi önbellek kullanmaktır. Kötü bir performans ve ölçeklenebilirlik açısından bir veritabanını kullanmak için oturum durumu sağlayıcısı yedeklenen çözümüdür. Bu konuda, Azure Redis önbelleği için ASP.NET oturum durumu sağlayıcısı kullanma hakkında yönergeler sunmaktadır. Diğer oturum durumu seçenekleri hakkında daha fazla bilgi için bkz: [ASP.NET oturum durumu seçenekleri](#aspnet-session-state-options).

## <a name="store-aspnet-session-state-in-the-cache"></a>Önbellekte ASP.NET oturumu durumu depolama
Redis önbelleği oturum durumu NuGet paketini kullanarak Visual Studio'da bir istemci uygulamasını yapılandırmak için tıklatın **NuGet Paket Yöneticisi**, **Paket Yöneticisi Konsolu** gelen **Araçları** menüsü.

`Package Manager Console` penceresinden aşağıdaki komutu çalıştırın.
    
```
Install-Package Microsoft.Web.RedisSessionStateProvider
```

> [!IMPORTANT]
> Premium katmanından Kümelemesi özelliğini kullanıyorsanız, kullanmalısınız [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 ya da daha yüksek veya farklı bir özel durum oluşturulur. 2.0.1 veya üzeri taşıma önemli bir değişiklik olduğunu; Daha fazla bilgi için bkz: [v2.0.0 sonu değişiklik ayrıntıları](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details). Bu makalede güncelleştirme zaman bu paketinin geçerli sürümü 2.2.3 ' dir.
> 
> 

Redis oturum durumu sağlayıcısı NuGet paketi StackExchange.Redis.StrongName paketi bir bağımlılığa sahiptir. StackExchange.Redis.StrongName paket projenizde mevcut değilse, yüklenir.

>[!NOTE]
>Tanımlayıcı adlı StackExchange.Redis.StrongName paket ek olarak, ayrıca olmayan-tanımlayıcı adlı StackExchange.Redis sürümü vardır. Projenizi kaldırmalısınız olmayan-tanımlayıcı adlı StackExchange.Redis sürümünü kullanıyorsanız, aksi takdirde, projenizde adlandırma çakışmaları alın. Bu paketleri hakkında daha fazla bilgi için bkz: [yapılandırma .NET önbellek istemcilerinin](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

NuGet paketi indirir ve gerekli derleme başvurularını ekler ve aşağıdaki bölümü web.config dosyasına ekler. Bu bölüm, Redis önbelleği oturum durumu sağlayıcısı kullanmak, ASP.NET uygulamanız için gerekli yapılandırmayı içerir.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
    <!--
    <add name="MySessionStateStore"
           host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        throwOnError = "true" [true|false]
        retryTimeoutInMilliseconds = "0" [number]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "5000" [number]
    />
    -->
    <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false"/>
    </providers>
</sessionState>
```

Açıklamalı bölüm öznitelikleri ve örnek ayarlarını her özniteliği için bir örnek sağlar.

Microsoft Azure Portal'da önbellek dikey pencerenizin değerlerle öznitelikleri yapılandırma ve diğer değerleri istediğiniz gibi yapılandırın. Önbellek özelliklerine erişme ile ilgili yönergeler için bkz: [Redis önbelleği ayarlarını yapılandırma](cache-configure.md#configure-redis-cache-settings).

* **ana bilgisayar** – önbellek uç noktasını belirtin.
* **bağlantı noktası** – SSL olmayan bağlantı noktası veya ssl ayarlarına bağlı olarak, SSL bağlantı noktası kullanın.
* **accessKey** – önbelleğiniz için birincil veya ikincil anahtarı kullanın.
* **SSL** – önbelleği/istemci iletişimleri ssl ile güvenli hale getirmek istiyorsanız True, aksi takdirde false. Doğru bağlantı noktasına belirttiğinizden emin olun.
  * SSL olmayan bağlantı noktasın yeni önbellekler için varsayılan olarak devre dışı bırakılmıştır. SSL bağlantı noktası kullanmak üzere bu ayarı TRUE belirtin. SSL olmayan bağlantı noktası etkinleştirme hakkında daha fazla bilgi için bkz: [erişim bağlantı noktaları](cache-configure.md#access-ports) bölümüne [bir önbellek yapılandırma](cache-configure.md) konu.
* **throwOnError** – varsa bir hata ya da false işlemi sessizce başarısız isterseniz durum için bir özel durum istiyorsanız true. Statik Microsoft.Web.Redis.RedisSessionStateProvider.LastException özelliğini kontrol ederek için bir hata kontrol edebilirsiniz. Varsayılan değer true şeklindedir.
* **retryTimeoutInMilliseconds** – başarısız olan işlemleri milisaniye cinsinden belirtilen bu aralığında denenecek. İlk yeniden deneme 20 milisaniye sonra gerçekleşir ve saniyede retryTimeoutInMilliseconds aralığı sona erene kadar yeniden denemeler meydana gelir. Bu aralık hemen sonra işlemi bir son kez yeniden denenir. İşlem yine başarısız olursa, özel durum geri çağırana throwOnError ayarı bağlı olarak atılır. Varsayılan değer yeniden deneme yok yani 0 ' dır.
* **Databaseıd** – hangi veritabanı önbellek çıktı verileri için kullanılacağını belirtir. Belirtilmezse, varsayılan değer 0 kullanılır.
* **applicationName** – anahtarları redis depolanır `{<Application Name>_<Session ID>}_Data`. Bu adlandırma şeması aynı Redis örneğini paylaşmak birden çok uygulama sağlar. Bu parametre isteğe bağlıdır ve onu belirtmezseniz, varsayılan değer kullanılır.
* **connectionTimeoutInMilliseconds** – Bu ayar, StackExchange.Redis istemcisi connectTimeout ayarında geçersiz kılmanıza olanak sağlar. Belirtilmezse, varsayılan connectTimeout ayarını 5000 kullanılır. Daha fazla bilgi için bkz: [StackExchange.Redis yapılandırma modeli](http://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** – Bu ayar, StackExchange.Redis istemcisi syncTimeout ayarında geçersiz kılmanıza olanak sağlar. Belirtilmezse, varsayılan syncTimeout ayarı 1000 kullanılır. Daha fazla bilgi için bkz: [StackExchange.Redis yapılandırma modeli](http://go.microsoft.com/fwlink/?LinkId=398705).

Bu özellikler hakkında daha fazla bilgi için özgün blog yayını duyurusuna bakın [Redis için ASP.NET oturum durumu sağlayıcısı Duyurusu](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).

Standart InProc oturum durumu sağlayıcısı bölümünde web.config çıkışı açıklama unutmayın.

```xml
<!-- <sessionState mode="InProc"
     customProvider="DefaultSessionProvider">
     <providers>
        <add name="DefaultSessionProvider"
              type="System.Web.Providers.DefaultSessionStateProvider,
                    System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                    PublicKeyToken=31bf3856ad364e35"
              connectionStringName="DefaultConnection" />
      </providers>
</sessionState> -->
```

Bu adımları gerçekleştirdikten sonra uygulamanızı Redis önbelleği oturum durumu sağlayıcısı kullanacak şekilde yapılandırılır. Uygulamanızda oturum durumu kullandığınızda, bir Azure Redis önbelleği örneği depolanır.

> [!IMPORTANT]
> Önbellekte depolanan veriler Serileştirilebilir olmalıdır, bellek içi ASP.NET oturum durumunu sağlayıcısı varsayılan olarak depolanan veriler farklı. Oturum durumu sağlayıcısı Redis için kullanıldığında, oturum durumu içinde depolanan veri türleri seri hale getirilebilir olduğundan emin olun.
> 
> 

## <a name="aspnet-session-state-options"></a>ASP.NET oturum durumu seçenekleri
* Bellek oturum durumu sağlayıcısı - bu sağlayıcı oturum durumu bellekte depolar. Bu sağlayıcı kullanmanın faydası, basit ve hızlı ' dir. Ancak değil dağıtılmış beri bellek sağlayıcısında kullanıyorsanız, Web uygulamalarınızı ölçeklendirme olamaz.
* SQL Server oturum durumu sağlayıcısı - bu sağlayıcısı, Sql Server'da oturum durumu depolar. Oturum durumu kalıcı depolama alanına saklamak isterseniz bu sağlayıcıyı kullanın. Web uygulamanızı ölçeklendirebilirsiniz ancak oturum açmak için Sql Server kullanan Web uygulamanıza bir performans etkisi olur.
* Dağıtılmış bellek oturum durumu sağlayıcısı Redis, önbelleği oturum durumu sağlayıcısı - bu sağlayıcının gibi en iyi iki açıdan da avantaj sağlar. Web uygulamanızı basit, hızlı ve ölçeklenebilir bir oturum durumu sağlayıcısı olabilir. Bu sağlayıcı oturum durumu verilerini bir önbellekte depolar olduğundan, uygulamanızı bir dağıtılmış, önbellek, geçici ağ hataları gibi konuşurken ilişkili tüm özelliklerini dikkate almanız gerekir. Önbellek kullanımı en iyi uygulamalar için bkz: [Kılavuzu önbelleğe alma](../best-practices-caching.md) Microsoft Patterns & yöntemler [Azure bulut uygulama tasarımı ve Uygulama Kılavuzu](https://github.com/mspnp/azure-guidance).

Oturum durumu ve diğer en iyi uygulamalar hakkında daha fazla bilgi için bkz: [Web geliştirme en iyi yöntemler (yapı gerçek bulut uygulamaları Azure ile)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices).

## <a name="next-steps"></a>Sonraki adımlar
Kullanıma [Azure Redis önbelleği için ASP.NET çıktı önbelleği sağlayıcısı](cache-aspnet-output-cache-provider.md).

