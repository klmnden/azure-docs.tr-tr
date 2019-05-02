---
title: Redis için Azure Cache için ASP.NET çıktı önbelleği sağlayıcısı
description: Azure önbelleği için Redis kullanarak ASP.NET sayfası çıkış önbelleğe alma hakkında bilgi edinin
services: cache
documentationcenter: na
author: yegu-ms
manager: jhubbard
editor: tysonn
ms.assetid: 78469a66-0829-484f-8660-b2598ec60fbf
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache
ms.workload: tbd
ms.date: 04/22/2018
ms.author: yegu
ms.openlocfilehash: a93d21b07dc486f743694ee99f60018ed4ef517c
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943865"
---
# <a name="aspnet-output-cache-provider-for-azure-cache-for-redis"></a>Redis için Azure Cache için ASP.NET çıktı önbelleği sağlayıcısı

Redis çıktı önbelleği sağlayıcısı, çıkış verileri önbelleğe almak için bir işlem dışı depolama mekanizmasıdır. Özellikle tam HTTP yanıtları için bu verilerdir (çıkış önbelleğe alma sayfası). ASP.NET 4'te kullanılmaya başlanan yeni çıktı önbelleği sağlayıcısı genişletilebilirlik noktasına sağlayıcı yararlanmanıza imkan sağlar.

Redis çıktı önbelleği sağlayıcısı kullanmak için önbelleğinizi önce yapılandırın ve ardından Redis çıktı önbelleği sağlayıcısı NuGet paketini kullanarak ASP.NET uygulamanızı yapılandırın. Bu konu, uygulamanızı Redis çıktı önbelleği sağlayıcısı kullanacak şekilde yapılandırma hakkında rehberlik sağlar. Oluşturma ve bir Azure önbelleği için Redis örneği yapılandırma hakkında daha fazla bilgi için bkz. [bir önbellek oluşturma](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>Önbellekte ASP.NET sayfa çıktısını Store

Visual Studio'da Azure önbelleği için Redis oturum durumu NuGet paketi kullanarak bir istemci uygulamasını yapılandırmak için tıklayın **NuGet Paket Yöneticisi**, **Paket Yöneticisi Konsolu** gelen **araçları**  menüsü.

`Package Manager Console` penceresinden aşağıdaki komutu çalıştırın.

```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

Redis çıktı önbelleği sağlayıcısı NuGet paketini StackExchange.Redis.StrongName paketinde bağımlılık vardır. StackExchange.Redis.StrongName paket, projenizde mevcut değilse yüklenir. Redis çıktı önbelleği sağlayıcısı NuGet paketi hakkında daha fazla bilgi için bkz. [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet sayfası.

>[!NOTE]
>Tanımlayıcı adlı StackExchange.Redis.StrongName paket yanı sıra de olmayan-tanımlayıcı adlı StackExchange.Redis sürümü mevcuttur. Projenizi StackExchange.Redis olmayan-tanımlayıcı adlı sürümü kullanıyorsanız, kaldırmanız gerekir; Aksi takdirde, projenizde adlandırma çakışmaları karşılaşırsınız. Bu paketler hakkında daha fazla bilgi için bkz. [önbellek istemcilerini yapılandırma .NET](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

NuGet paketi indirir ve gerekli derleme başvurularını ekler ve aşağıdaki bölümde, web.config dosyasına ekler. Bu bölümde Redis çıktı önbelleği sağlayıcısı kullanmak ASP.NET uygulamanız için gerekli yapılandırmayı içerir.

```xml
<caching>
  <outputCache defaultProvider="MyRedisOutputCache">
    <providers>
      <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider"
           host=""
           accessKey=""
           ssl="true" />
    </providers>
  </outputCache>
</caching>
```

Microsoft Azure Portal'da önbellek dikey pencerenize değerlerle öznitelikleri yapılandırma ve diğer değerleri istediğiniz gibi yapılandırın. Önbellek özelliklerinizi erişme ile ilgili yönergeler için bkz: [yapılandırma Azure önbelleği için Redis ayarları](cache-configure.md#configure-azure-cache-for-redis-settings).

| Öznitelik | Tür | Varsayılan | Açıklama |
| --------- | ---- | ------- | ----------- |
| *Ana bilgisayar* | string | "localhost" | Redis sunucu IP adresi veya ana bilgisayar adı |
| *Bağlantı noktası* | pozitif bir tamsayı | 6379 (SSL'siz)<br/>6380 (SSL) | Redis sunucu bağlantı noktası |
| *accessKey* | string | "" | Sunucu parolası redis, Redis yetkilendirme etkinleştirildiğinde. Oturum durumu sağlayıcısı herhangi bir parola kullanmaz, Redis sunucusuna bağlanırken anlamına gelir. varsayılan olarak boş bir dize değeridir. **Redis sunucunuza gibi Azure Redis Cache genel olarak erişilebilen bir ağ ise, güvenliğini artırmak Redis yetkilendirme etkinleştirdiğinizden emin olun ve güvenli bir parola belirtin.** |
| *ssl* | boole | **False** | SSL aracılığıyla Redis sunucusuna bağlanmak belirtir. Bu değer **false** varsayılan olarak SSL Redis desteklemediğinden kullanıma hazır. **İlk günden destekleyen SSL için bunu True ayarladığınızdan emin olan Azure Redis Cache kullanıyorsanız güvenliğini artırın.**<br/><br/>SSL olmayan bağlantı noktasın yeni önbellekler için varsayılan olarak devre dışı bırakılmıştır. Belirtin **true** SSL bağlantı noktası kullanmak üzere bu ayarı için. SSL olmayan bağlantı noktası etkinleştirme hakkında daha fazla bilgi için bkz: [erişim bağlantı noktaları](cache-configure.md#access-ports) konusundaki [bir önbellek yapılandırma](cache-configure.md) konu. |
| *databaseIdNumber* | pozitif bir tamsayı | 0 | *Bu öznitelik, yalnızca web.config veya AppSettings aracılığıyla belirtilebilir.*<br/><br/>Hangi Redis veritabanının kullanılacağını belirtin. |
| *connectionTimeoutInMilliseconds* | pozitif bir tamsayı | StackExchange.Redis tarafından sağlanan | Ayarlamak için kullanılan *ConnectTimeout* StackExchange.Redis.ConnectionMultiplexer oluştururken. |
| *operationTimeoutInMilliseconds* | pozitif bir tamsayı | StackExchange.Redis tarafından sağlanan | Ayarlamak için kullanılan *SyncTimeout* StackExchange.Redis.ConnectionMultiplexer oluştururken. |
| *connectionString* (bağlantı dizesi geçerli StackExchange.Redis) | string | *yok* | Parametre başvurusu AppSettings veya web.config, aksi takdirde geçerli StackExchange.Redis bağlantı dizesi. Bu öznitelik için değerleri sağlayabileceğinizi *konak*, *bağlantı noktası*, *accessKey*, *ssl*ve diğer StackExchange.Redis öznitelikleri. İçin bir daha yakın bakın *connectionString*, bkz: [connectionString ayarlama](#setting-connectionstring) içinde [özniteliği notları](#attribute-notes) bölümü. |
| *settingsClassName*<br/>*settingsMethodName* | string<br/>string | *yok* | *Bu öznitelikler, yalnızca web.config veya AppSettings aracılığıyla belirtilebilir.*<br/><br/>Bu öznitelikler, bir bağlantı dizesi sağlamak için kullanın. *settingsClassName* tarafından belirtilen metot içeren bir derleme tam sınıf adı olmalıdır *settingsMethodName*.<br/><br/>Tarafından belirtilen metot *settingsMethodName* genel, statik ve void olmalıdır (herhangi bir parametre katılmak değil), dönüş türüne sahip **dize**. Bu yöntem gerçek bağlantı dizesini döndürür. |
| *loggingClassName*<br/>*loggingMethodName* | string<br/>string | *yok* | *Bu öznitelikler, yalnızca web.config veya AppSettings aracılığıyla belirtilebilir.*<br/><br/>Bu öznitelikler, oturum durumu/çıktı önbelleği günlüklerinden günlükleri ile birlikte StackExchange.Redis sağlayarak uygulamanızda hata ayıklamak için kullanın. *loggingClassName* tarafından belirtilen metot içeren bir derleme tam sınıf adı olmalıdır *loggingMethodName*.<br/><br/>Tarafından belirtilen metot *loggingMethodName* genel, statik ve void olmalıdır (herhangi bir parametre katılmak değil), dönüş türüne sahip **System.IO.TextWriter**. |
| *ApplicationName* | string | Geçerli işlemin modül adı veya "/" | *Yalnızca SessionStateProvider*<br/>*Bu öznitelik, yalnızca web.config veya AppSettings aracılığıyla belirtilebilir.*<br/><br/>Uygulama adı Redis önbelleğinde kullanılacak önek. Müşteri aynı Redis önbelleği farklı amaçlar için kullanabilirsiniz. Oturum anahtarları değil birbiriyle çakışır olmasını sağlamak için uygulama adı ile belirtilebilir. |
| *throwOnError* | boole | true | *Yalnızca SessionStateProvider*<br/>*Bu öznitelik, yalnızca web.config veya AppSettings aracılığıyla belirtilebilir.*<br/><br/>Bir hata oluştuğunda bir özel durum verilmeyeceğini belirtir.<br/><br/>Hakkında daha fazla bilgi için *throwOnError*, bkz: [hakkında notlar *throwOnError* ](#notes-on-throwonerror) içinde [özniteliği notları](#attribute-notes) bölümü. |>*Microsoft.Web.Redis.RedisSessionStateProvider.LastException*. |
| *retryTimeoutInMilliseconds* | pozitif bir tamsayı | 5000 | *Yalnızca SessionStateProvider*<br/>*Bu öznitelik, yalnızca web.config veya AppSettings aracılığıyla belirtilebilir.*<br/><br/>Ne kadar bir işlemi başarısız olduğunda yeniden deneyin. Bu değer ise kısa *operationTimeoutInMilliseconds*, sağlayıcıyı yeniden.<br/><br/>Hakkında daha fazla bilgi için *retryTimeoutInMilliseconds*, bkz: [hakkında notlar *retryTimeoutInMilliseconds* ](#notes-on-retrytimeoutinmilliseconds) içinde [özniteliği notları](#attribute-notes) bölümü. |
| *redisSerializerType* | string | *yok* | Microsoft.Web.Redis uygulayan bir sınıf derleme nitelikli tür adı belirtir. İSerializer ve bu değerleri seri hale getrime ve özel mantığı içerir. Daha fazla bilgi için [hakkında *redisSerializerType* ](#about-redisserializertype) içinde [özniteliği notları](#attribute-notes) bölümü. |
|

## <a name="attribute-notes"></a>Öznitelik notları

### <a name="setting-connectionstring"></a>Ayar *connectionString*

Değerini *connectionString* AppSettings ayarında böyle bir dize zaten varsa anahtarı olarak AppSettings gerçek bağlantı dizesini almak için kullanılır. Bulunamazsa değerini AppSettings içinde *connectionString* gerçek bağlantı dizesini web.config dosyasından getirmek için anahtar olarak kullanılacak **ConnectionString** bölümünde bu bölümü zaten varsa. Bağlantı dizesi yoksa AppSettings veya web.config var **ConnectionString** bölümü değişmez değeri *connectionString* bağlantı dizesi oluştururken kullanılacak StackExchange.Redis.ConnectionMultiplexer.

Aşağıdaki örnekler gösterir nasıl *connectionString* kullanılır.

#### <a name="example-1"></a>Örnek 1

```xml
<connectionStrings>
    <add name="MyRedisConnectionString" connectionString="mycache.redis.cache.windows.net:6380,password=actual access key,ssl=True,abortConnect=False" />
</connectionStrings>
```

İçinde `web.config`, gerçek değer yerine parametre değeri olarak anahtarı kullanın.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvide"
             name = "MySessionStateStore"
             connectionString = "MyRedisConnectionString"/>
    </providers>
</sessionState>
```

#### <a name="example-2"></a>Örnek 2

```xml
<appSettings>
    <add key="MyRedisConnectionString" value="mycache.redis.cache.windows.net:6380,password=actual access key,ssl=True,abortConnect=False" />
</appSettings>
```

İçinde `web.config`, gerçek değer yerine parametre değeri olarak anahtarı kullanın.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvide"
             name = "MySessionStateStore"
             connectionString = "MyRedisConnectionString"/>
    </providers>
</sessionState>
```

#### <a name="example-3"></a>Örnek 3

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvide"
             name = "MySessionStateStore"
             connectionString = "mycache.redis.cache.windows.net:6380,password=actual access key,ssl=True,abortConnect=False"/>
    </providers>
</sessionState>
```

### <a name="notes-on-throwonerror"></a>Şirket Notları *throwOnError*

Şu anda bir oturum işlemi sırasında bir hata meydana gelirse oturum durumu sağlayıcısı bir özel durum oluşturur. Bu uygulamayı kapatır.

Bu davranış, isterseniz, özel durumları davranma özelliği de sağlarken mevcut ASP.NET oturum durumu sağlayıcısı kullanıcı beklentileri destekler şekilde değiştirildi. Varsayılan davranış, yine de, diğer ASP.NET oturum durumu sağlayıcıları ile tutarlı hatada bir özel durum oluşturur; var olan kod, önceki ile aynı çalışmalıdır.

Ayarlarsanız *throwOnError* için **false**, hata oluştuğunda bir özel durum yerine, bunu sessizce başarısız olur. Bir hata olup olmadığını görmek ve bu durumda, özel durumu neydi bulmak için statik özelliği kontrol *Microsoft.Web.Redis.RedisSessionStateProvider.LastException*.

### <a name="notes-on-retrytimeoutinmilliseconds"></a>Şirket Notları *retryTimeoutInMilliseconds*

Bu, burada bazı oturum işlemi hatası nedeniyle ağ sorun gibi şeyler de yeniden deneme zaman aşımı denetlemek veya tamamen yeniden denemeyi iyileştirilmiş olanak tanıyan yeniden denemeniz gerekir çalışması basitleştirmek için bazı yeniden deneme mantığı sağlar.

Ayarlarsanız *retryTimeoutInMilliseconds* oturum işlemi başarısız olduğunda, bir sayı, örneğin 2000, ardından 2000 milisaniye hata olarak değerlendirmesini önce yeniden denenecek. Bu nedenle bu yeniden deneme mantığı uygulamak için oturum durumu sağlayıcısı için yalnızca zaman aşımı süresi yapılandırın. İlk yeniden deneme bir sorun meydana geldiğinde çoğu durumda yeterli olduğu 20 milisaniye sonra gerçekleşir. Zaman aşımına uğrayana kadar her saniye sonra yeniden denenecek. Zaman aşımı hemen sonra (en fazla) bir saniye zaman aşımı devre dışı kesin olmaz emin emin olmak için bir kez daha yeniden denenecek.

(Örneğin, Redis sunucusu, uygulama ile aynı makinede çalıştırırken) yeniden deneyin veya kendiniz yeniden deneme mantığı işlemek istiyorsanız gerekir düşünmüyorsanız *retryTimeoutInMilliseconds* 0.

### <a name="about-redisserializertype"></a>Hakkında *redisSerializerType*

Varsayılan olarak, Redis değerleri depolamak için seri hale getirme tarafından sağlanan bir ikili biçimi gerçekleştirilir **BinaryFormatter** sınıfı. Kullanım *redisSerializerType* uygulayan bir sınıf derleme nitelikli tür adı belirtmek için **Microsoft.Web.Redis.ISerializer** ve değerleri seri hale getrime ve özel mantığı vardır. Örneğin, JSON.NET kullanarak bir Json seri hale getirici sınıfı şöyledir:

```cs
namespace MyCompany.Redis
{
    public class JsonSerializer : ISerializer
    {
        private static JsonSerializerSettings _settings = new JsonSerializerSettings() { TypeNameHandling = TypeNameHandling.All };

        public byte[] Serialize(object data)
        {
            return Encoding.UTF8.GetBytes(JsonConvert.SerializeObject(data, _settings));
        }

        public object Deserialize(byte[] data)
        {
            if (data == null)6t6
            {
                return null;
            }
            return JsonConvert.DeserializeObject(Encoding.UTF8.GetString(data), _settings);
        }
    }
}
```

Bu sınıf varsayılarak ada sahip bir derleme içinde tanımlandığından **MyCompanyDll**, parametre ayarlayabilirsiniz *redisSerializerType* kullanmak için:

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvider"
             name = "MySessionStateStore"
             redisSerializerType = "MyCompany.Redis.JsonSerializer,MyCompanyDll"
             ... />
    </providers>
</sessionState>
```

## <a name="output-cache-directive"></a>Çıktı önbellek yönergesi

OutputCache yönergesi çıkışı önbelleğe almak istediğiniz her sayfaya ekleyin.

```xml
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

Önceki örnekte, önbelleğe alınan sayfa verileri kalır önbellekte 60 saniye ve sayfayı farklı bir sürümünü her parametre bileşimi için önbelleğe alınır. OutputCache yönergesi hakkında daha fazla bilgi için bkz: [ @OutputCache ](https://go.microsoft.com/fwlink/?linkid=320837).

Bu adımların sonra uygulamanızı Redis çıktı önbelleği sağlayıcısı kullanmak üzere yapılandırılmış.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıma [Redis için Azure Cache için ASP.NET oturum durumu sağlayıcısı](cache-aspnet-session-state-provider.md).
