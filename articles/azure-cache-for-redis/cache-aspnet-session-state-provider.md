---
title: Cache ASP.NET oturum durumu sağlayıcısı | Microsoft Docs
description: Azure önbelleği için Redis kullanarak ASP.NET oturumu durumu depolama hakkında bilgi edinin
services: cache
documentationcenter: na
author: yegu-ms
manager: jhubbard
editor: tysonn
ms.assetid: 192f384c-836a-479a-bb65-8c3e6d6522bb
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache
ms.workload: tbd
ms.date: 05/01/2017
ms.author: yegu
ms.openlocfilehash: 4a51040ecdbf22af03ce1e6edaaa0ff577bbc076
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60233314"
---
# <a name="aspnet-session-state-provider-for-azure-cache-for-redis"></a>Redis Azure Cache için ASP.NET oturum durumu sağlayıcısı

Azure önbelleği için Redis, oturum durumu ile bellek içi Azure önbelleği için Redis yerine SQL Server veritabanını depolamak için kullanabileceğiniz bir oturum durumu sağlayıcısı sağlar. Önbelleğe alma oturum durumu sağlayıcısı kullanmak için önbelleğinizi önce yapılandırın ve ardından ASP.NET uygulamanız Azure önbelleği için Redis oturum durumu NuGet paketi kullanarak bir önbellek için yapılandırın.

Bir kullanıcı oturumu için durum çeşit depolanmasını önlemek için bir gerçek hayatta kullanılan bulut uygulaması pratik değildir, ancak aşağıdaki yaklaşımlardan diğerlerinden daha fazla performans ve ölçeklenebilirlik etkisi. Durumunu depolamak varsa, durum miktarını küçük tutun ve tanımlama bilgilerini depolamak için en iyi çözüm olur. Bu uygun değilse, dağıtılmış, bellek içi önbelleği için ASP.NET oturum durumu sağlayıcısı ile kullanılacak sonraki en iyi çözüm olduğundan. En kötü bir performans ve ölçeklenebilirlik açısından bir veritabanını kullanmak için oturum durumu sağlayıcısı desteklenen çözümdür. Bu konu, Redis için Azure Cache için ASP.NET oturum durumu sağlayıcısını kullanarak rehberlik sağlar. Diğer oturum durumu seçenekleri hakkında daha fazla bilgi için bkz: [ASP.NET oturum durumu seçenekleri](#aspnet-session-state-options).

## <a name="store-aspnet-session-state-in-the-cache"></a>Önbellekte ASP.NET oturumu durumu depolama

Visual Studio'da Azure önbelleği için Redis oturum durumu NuGet paketi kullanarak bir istemci uygulamasını yapılandırmak için tıklayın **NuGet Paket Yöneticisi**, **Paket Yöneticisi Konsolu** gelen **araçları**  menüsü.

`Package Manager Console` penceresinden aşağıdaki komutu çalıştırın.
    

```powershell
Install-Package Microsoft.Web.RedisSessionStateProvider
```

> [!IMPORTANT]
> Premium katmandan kümeleme özelliğini kullanıyorsanız, kullanmalısınız [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 veya daha yüksek veya farklı bir özel durum oluşturulur. 2.0.1 veya üzeri taşıma bölünmesi farklıdır; Daha fazla bilgi için [v2.0.0 bozucu değişiklik ayrıntıları](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details). Bu makalede güncelleştirme zaman bu paketin geçerli sürümü 2.2.3 ' dir.
> 
> 

Redis oturum durumu sağlayıcısı NuGet paketini StackExchange.Redis.StrongName paketinde bağımlılık vardır. StackExchange.Redis.StrongName paket, projenizde mevcut değilse yüklenir.

>[!NOTE]
>Tanımlayıcı adlı StackExchange.Redis.StrongName paket yanı sıra de olmayan-tanımlayıcı adlı StackExchange.Redis sürümü mevcuttur. Projenizi kaldırmalısınız olmayan-tanımlayıcı adlı StackExchange.Redis sürümü kullanıyorsanız, aksi takdirde, projenizde adlandırma çakışmaları alın. Bu paketler hakkında daha fazla bilgi için bkz. [önbellek istemcilerini yapılandırma .NET](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

NuGet paketi indirir ve gerekli derleme başvurularını ekler ve aşağıdaki bölümde, web.config dosyasına ekler. Bu bölümde, Azure önbelleği için Redis oturum durumu sağlayıcısı kullanmak ASP.NET uygulamanız için gerekli yapılandırmayı içerir.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
  <providers>
    <!-- Either use 'connectionString' OR 'settingsClassName' and 'settingsMethodName' OR use 'host','port','accessKey','ssl','connectionTimeoutInMilliseconds' and 'operationTimeoutInMilliseconds'. -->
    <!-- 'throwOnError','retryTimeoutInMilliseconds','databaseId' and 'applicationName' can be used with both options. -->
    <!--
      <add name="MySessionStateStore" 
        host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        throwOnError = "true" [true|false]
        retryTimeoutInMilliseconds = "5000" [number]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "1000" [number]
        connectionString = "<Valid StackExchange.Redis connection string>" [String]
        settingsClassName = "<Assembly qualified class name that contains settings method specified below. Which basically return 'connectionString' value>" [String]
        settingsMethodName = "<Settings method should be defined in settingsClass. It should be public, static, does not take any parameters and should have a return type of 'String', which is basically 'connectionString' value.>" [String]
        loggingClassName = "<Assembly qualified class name that contains logging method specified below>" [String]
        loggingMethodName = "<Logging method should be defined in loggingClass. It should be public, static, does not take any parameters and should have a return type of System.IO.TextWriter.>" [String]
        redisSerializerType = "<Assembly qualified class name that implements Microsoft.Web.Redis.ISerializer>" [String]
      />
    -->
    <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider"
         host=""
         accessKey=""
         ssl="true" />
</sessionState>
```

Açıklamalı bölüm öznitelikleri ve her bir öznitelik için örnek ayarlarını örneği sağlar.

Microsoft Azure Portal'da önbellek dikey pencerenize değerlerle öznitelikleri yapılandırma ve diğer değerleri istediğiniz gibi yapılandırın. Önbellek özelliklerinizi erişme ile ilgili yönergeler için bkz: [yapılandırma Azure önbelleği için Redis ayarları](cache-configure.md#configure-azure-cache-for-redis-settings).

* **konak** –, önbellek uç noktasını belirtin.
* **bağlantı noktası** – SSL olmayan bağlantı noktası ya da ssl ayarlarına bağlı olarak, SSL bağlantı noktasını kullanın.
* **accessKey** – önbelleğiniz için birincil veya ikincil anahtarı kullanın.
* **SSL** – önbellek/istemci iletişimleri ssl ile güvenli hale getirmek istiyorsanız true; Aksi takdirde false. Doğru bağlantı noktasını belirttiğinizden emin olun.
  * SSL olmayan bağlantı noktasın yeni önbellekler için varsayılan olarak devre dışı bırakılmıştır. SSL bağlantı noktası kullanmak üzere bu ayarı TRUE belirtin. SSL olmayan bağlantı noktası etkinleştirme hakkında daha fazla bilgi için bkz: [erişim bağlantı noktaları](cache-configure.md#access-ports) konusundaki [bir önbellek yapılandırma](cache-configure.md) konu.
* **throwOnError** – bir özel varsa bir hata veya yanlış işlemini sessizce başarısız isterseniz durum istiyorsanız true. Statik Microsoft.Web.Redis.RedisSessionStateProvider.LastException özelliği kontrol ederek bir hata için kontrol edebilirsiniz. Varsayılan değer True'dur.
* **retryTimeoutInMilliseconds** – başarısız işlemleri yeniden denenir. milisaniye cinsinden belirtilen bu aralık sırasında. İlk yeniden deneme 20 milisaniye sonra gerçekleşir ve saniyede retryTimeoutInMilliseconds aralığı süresi dolana kadar yeniden denemeler meydana gelir. Bu aralık hemen sonra işlemi son bir kez yeniden denendi. İşlem yine başarısız olursa, özel durum throwOnError ayara bağlı olarak, çağırana geri oluşturulur. Varsayılan değer, yeniden deneme yok anlamına gelen 0 ' dır.
* **Databaseıd** – önbelleği için çıktı verilerini kullanmak için hangi veritabanını belirtir. Belirtilmezse, varsayılan değer olan 0 kullanılır.
* **applicationName** – anahtarları redis depolanır `{<Application Name>_<Session ID>}_Data`. Bu adlandırma şeması aynı Redis örneği paylaşmak birden çok uygulama sağlar. Bu parametre isteğe bağlıdır ve onu belirtmezseniz varsayılan değer kullanılır.
* **connectionTimeoutInMilliseconds** – Bu ayar, StackExchange.Redis istemcisi connectTimeout ayarında geçersiz kılmanıza olanak sağlar. Belirtilmezse, varsayılan connectTimeout ayar 5000 kullanılır. Daha fazla bilgi için [StackExchange.Redis yapılandırma modeli](https://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** – Bu ayar, StackExchange.Redis istemcisi syncTimeout ayarında geçersiz kılmanıza olanak sağlar. Belirtilmezse varsayılan syncTimeout olarak 1000 kullanılır. Daha fazla bilgi için [StackExchange.Redis yapılandırma modeli](https://go.microsoft.com/fwlink/?LinkId=398705).
* **redisSerializerType** -Bu ayar özel serileştirme Redis'e gönderilen oturum içeriğinin belirtmenizi sağlar. Belirtilen tür uygulamalıdır `Microsoft.Web.Redis.ISerializer` ve genel parametresiz oluşturucusu bildirmeniz gerekir. Varsayılan olarak `System.Runtime.Serialization.Formatters.Binary.BinaryFormatter` kullanılır.

Bu özellikler hakkında daha fazla bilgi için özgün blog gönderisi duyurusuna bakın [Redis için ASP.NET oturum durumu sağlayıcısı ile tanışın](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).

Standart InProc oturum durumu sağlayıcısını web.config dosyasındaki bölümünü, açıklama unutmayın.

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

Bu adımların sonra uygulamanızı Azure önbelleği için Redis oturum durumu sağlayıcısı kullanmak üzere yapılandırılır. Uygulamanızda oturum durumu kullandığınızda, bir Azure önbelleği için Redis örneği depolanır.

> [!IMPORTANT]
> Önbelleğinde depolanan verileri seri hale getirilebilir olması gerekir, varsayılan olarak depolanan verileri aksine bellek içi ASP.NET oturum durumunu sağlayıcısı. Redis için oturum durumu Sağlayıcısı kullanıldığında, oturum durumu içinde depolanan veri türleri seri hale getirilebilir olduğundan emin olun.
> 
> 

## <a name="aspnet-session-state-options"></a>ASP.NET oturum durumu seçenekleri

* Bellek oturum durumu sağlayıcısı - bu sağlayıcı oturum durumu bellekte depolar. Bu sağlayıcı kullanmanın faydası, basit ve hızlı ' dir. Ancak bu yana olmayan dağıtılmış bellek sağlayıcı kullanıyorsanız, Web uygulamalarınızı ölçeklendirilemez.
* SQL Server oturum durumu sağlayıcısı - bu sağlayıcı Sql Server'da oturum durumunu depolar. Oturum durumu kalıcı depolama alanında depolamak istiyorsanız bu sağlayıcısını kullanın. Web uygulamanızı ölçeklendirmek ancak oturumu için Sql Server'ı kullanarak Web uygulamanızı bir performans etkisi olur. Bu sağlayıcı ile de kullanabileceğiniz bir [bellek içi OLTP'yi yapılandırma](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/11/28/asp-net-session-state-with-sql-server-in-memory-oltp/) performansını geliştirmeye yardımcı olmak için.
* Dağıtılmış, bellek oturum durumu sağlayıcısını Azure önbelleği için Redis oturum durumu sağlayıcısı - bu sağlayıcının gibi en iyi şekilde yararlanmanızı sağlar. Web uygulamanızı basit, hızlı ve ölçeklenebilir bir oturum durumu sağlayıcısı olabilir. Bu sağlayıcı oturum durumu bir önbellekte depolar için göz önünde bulundurarak dağıtılmış, bellek önbelleğini, geçici ağ hataları gibi konuşurken ilişkili tüm özelliklerini almak uygulamanızı sahiptir. Önbellek kullanarak en iyi uygulamalar için bkz: [önbelleğe alma Kılavuzu](../best-practices-caching.md) Microsoft Patterns & yöntemler [Azure bulut uygulama tasarımı ve Uygulama Kılavuzu](https://github.com/mspnp/azure-guidance).

Oturum durumu ve diğer en iyi yöntemler hakkında daha fazla bilgi için bkz. [Web geliştirme en iyi yöntemler (gerçek hayatta kullanılan bulut uygulamaları Azure ile oluşturma)](https://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices).

## <a name="next-steps"></a>Sonraki adımlar

Kullanıma [Redis için Azure Cache için ASP.NET çıktı önbelleği sağlayıcısı](cache-aspnet-output-cache-provider.md).