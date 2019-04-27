---
title: Bilgi edinmek için hızlı başlangıç Cache nasıl kullanılır Azure Redis için .NET uygulamaları ile | Microsoft Docs
description: Bu hızlı başlangıçta, Azure Cache, Redis için .NET uygulamalarınızdan erişmeyi öğrenin
services: cache,app-service
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: c502f74c-44de-4087-8303-1b1f43da12d5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 05/18/2018
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 487a51b6f644cca88011383b8035515d437857e6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60553407"
---
# <a name="quickstart-use-azure-cache-for-redis-with-a-net-application"></a>Hızlı Başlangıç: Azure Cache, Redis için bir .NET uygulamasını kullanın.



Bu hızlı başlangıçta, Microsoft Azure önbelleği için Redis ile .NET kullanmaya başlama işlemini göstermektedir. Microsoft Azure önbelleği için Redis, Redis için popüler açık kaynaklı Azure önbelleğini temel alır. Güvenli ve adanmış bir Azure önbelleği için Redis, Microsoft tarafından yönetilen için erişmenizi sağlar. Azure önbelleği için Redis kullanılarak oluşturulan bir önbelleğe, Microsoft Azure içindeki herhangi bir uygulamadan erişilebilir.

Bu hızlı başlangıçta, bir konsol uygulamasında C\# kodu ile [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) istemcisini kullanacaksınız. Bir önbellek oluşturacak ve .NET istemci uygulamasını yapılandıracaksınız. Daha sonra, önbelleğe nesneler ekleyecek ve önbellekteki nesneleri güncelleştireceksiniz. 

![Konsol uygulaması tamamlandı](./media/cache-dotnet-how-to-use-azure-redis-cache/cache-console-app-complete.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio](https://www.visualstudio.com/downloads/)
* StackExchange.Redis istemcisi [.NET Framework 4 veya üst sürümünü](https://www.microsoft.com/net/download/dotnet-framework-runtime) gerektirir.

## <a name="create-a-cache"></a>Bir önbellek oluşturma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-access-keys](../../includes/redis-cache-access-keys.md)]

Bilgisayarınızda *CacheSecrets.config* adlı bir dosya oluşturun ve örnek uygulamanızın kaynak kodu ile denetlenmeyecek bir konuma yerleştirin. Bu hızlı başlangıç için *CacheSecrets.config* dosyası şu konumda bulunur: *C:\AppSecrets\CacheSecrets.config*.

*CacheSecrets.config* dosyasını düzenleyin ve aşağıdaki içerikleri ekleyin:

```xml
<appSettings>
    <add key="CacheConnection" value="<cache-name>.redis.cache.windows.net,abortConnect=false,ssl=true,password=<access-key>"/>
</appSettings>
```

`<cache-name>` adını, önbellek ana bilgisayar adınızla değiştirin.

`<access-key>` adını, önbelleğinizin birincil anahtarıyla değiştirin.


## <a name="create-a-console-app"></a>Konsol uygulaması oluşturma

Visual Studio’da, **Dosya** > **Yeni** > **Proje**’ye tıklayın.

**Visual C#** altında, **Windows Klasik Masaüstü**’ne tıklayın ve daha sonra **Konsol Uygulaması**’na ve **Tamam**’a tıklayarak yeni bir konsol uygulaması oluşturun.


<a name="configure-the-cache-clients"></a>

## <a name="configure-the-cache-client"></a>Önbellek istemcisini yapılandırma

Bu bölümde, .NET için [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) istemcisini kullanmak üzere konsol uygulamasını yapılandıracaksınız.

Visual Studio’da, **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**’na tıklayın ve Paket Yöneticisi Konsolu penceresinden aşağıdaki komutu çalıştırın.

```powershell
Install-Package StackExchange.Redis
```

Yükleme tamamlandıktan sonra *StackExchange.Redis* önbellek istemcisi, projenizle kullanılabilir olur.


## <a name="connect-to-the-cache"></a>Önbelleğe bağlanma

Visual Studio’da, *App.config* dosyanızı açın ve *CacheSecrets.config* dosyasına başvuran bir `appSettings` `file` özniteliği içerecek şekilde güncelleştirin.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.1" />
    </startup>

    <appSettings file="C:\AppSecrets\CacheSecrets.config"></appSettings>  

</configuration>
```

Çözüm Gezgini’nde, **Başvurular**’a sağ tıklayın ve **Başvuru ekle**’ye tıklayın. **System.Configuration** bütünleştirilmiş koduna bir başvuru ekleyin.

Aşağıdaki `using` deyimlerini *Program.cs* dosyasına ekleyin:

```csharp
using StackExchange.Redis;
using System.Configuration;
```

Azure önbelleği için Redis bağlantı tarafından yönetilen `ConnectionMultiplexer` sınıfı. Bu sınıf, istemci uygulamanız genelinde paylaşılmalı ve yeniden kullanılmalıdır. Her işlem için yeni bir bağlantı oluşturmayın. 

Kimlik bilgilerini asla kaynak kodunda depolamayın. Bu örneği basit tutmak için, yalnızca bir dış gizli diziler yapılandırma dosyası kullanıyorum. Daha iyi bir yaklaşım [Sertifikalar ile Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/certificate-scenarios) kullanmaktır.

*Program.cs* dosyasında, konsol uygulamanızın `Program` sınıfına aşağıdaki üyeleri ekleyin:

```csharp
        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
            return ConnectionMultiplexer.Connect(cacheConnection);
        });

        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }
```


Uygulamanızda bir `ConnectionMultiplexer` örneğini paylaşmaya ilişkin bu yaklaşım, bağlı bir örnek döndüren bir statik özelliği kullanır. Kod yalnızca tek bir bağlı `ConnectionMultiplexer` örneği başlatmak için iş parçacığı güvenli bir yol sağlar. `abortConnect` Azure Redis önbelleğine bağlantı kurulmasa bile çağrının başarılı olacağıdır anlamına gelir. false olarak ayarlanır. `ConnectionMultiplexer` temel özelliklerinden biri ağ sorunu ya da diğer nedenler çözümlendiğinde önbellek bağlantısını otomatik olarak geri yüklemesidir.

*CacheConnection* appSetting değeri parola parametresi olarak Azure portalından önbellek bağlantısı dizesine başvurmak için kullanılır.

## <a name="executing-cache-commands"></a>Önbellek komutlarını yürütme

Konsol uygulamanıza yönelik `Program` sınıfının `Main` yordamı için aşağıdaki kodu ekleyin:

```csharp
        static void Main(string[] args)
        {
            // Connection refers to a property that returns a ConnectionMultiplexer
            // as shown in the previous example.
            IDatabase cache = lazyConnection.Value.GetDatabase();

            // Perform cache operations using the cache object...

            // Simple PING command
            string cacheCommand = "PING";
            Console.WriteLine("\nCache command  : " + cacheCommand);
            Console.WriteLine("Cache response : " + cache.Execute(cacheCommand).ToString());

            // Simple get and put of integral data types into the cache
            cacheCommand = "GET Message";
            Console.WriteLine("\nCache command  : " + cacheCommand + " or StringGet()");
            Console.WriteLine("Cache response : " + cache.StringGet("Message").ToString());

            cacheCommand = "SET Message \"Hello! The cache is working from a .NET console app!\"";
            Console.WriteLine("\nCache command  : " + cacheCommand + " or StringSet()");
            Console.WriteLine("Cache response : " + cache.StringSet("Message", "Hello! The cache is working from a .NET console app!").ToString());

            // Demonstrate "SET Message" executed as expected...
            cacheCommand = "GET Message";
            Console.WriteLine("\nCache command  : " + cacheCommand + " or StringGet()");
            Console.WriteLine("Cache response : " + cache.StringGet("Message").ToString());

            // Get the client list, useful to see if connection list is growing...
            cacheCommand = "CLIENT LIST";
            Console.WriteLine("\nCache command  : " + cacheCommand);
            Console.WriteLine("Cache response : \n" + cache.Execute("CLIENT", "LIST").ToString().Replace("id=", "id="));

            lazyConnection.Value.Dispose();
        }
```

Azure önbelleği için Redis yapılandırılabilir sayıda verileri bir Azure önbelleği için Redis mantıksal olarak ayırmak için kullanılan veritabanları (varsayılan değer 16) sahiptir. Kod, DB 0 adlı varsayılan veritabanına bağlanır. Daha fazla bilgi için bkz. [Redis veritabanı nedir?](cache-faq.md#what-are-redis-databases) ve [Varsayılan Redis sunucu yapılandırması](cache-configure.md#default-redis-server-configuration).

`StringSet` ve `StringGet` yöntemleri kullanılarak önbellek öğeleri depolanabilir ve alınabilir.

Redis, Redis dizeleri kadar veri depolar, ancak bu dizeler önbellekte .NET nesneleri depolarken kullanılabilecek seri hale getirilmiş ikili veriler dahil, birçok veri türünü içerebilir.

Konsol uygulamasını derleyip çalıştırmak için **Ctrl+F5**'e basın.

Aşağıdaki örnekte, `Message` anahtarının Azure portaldaki Redis Konsolu kullanılarak ayarlanan, önceden önbelleğe alınmış bir değer içerdiğini görebilirsiniz. Uygulama, önbelleğe alınan bu değeri güncelleştirdi. Ayrıca uygulama, `PING` ve `CLIENT LIST` komutlarını da yürüttü.

![Kısmi konsol uygulaması](./media/cache-dotnet-how-to-use-azure-redis-cache/cache-console-app-partial.png)


## <a name="work-with-net-objects-in-the-cache"></a>Önbellekte .NET nesneleriyle çalışma

Azure önbelleği için Redis hem .NET nesneleri hem de temel veri türlerinin önbelleğe alabilir, ancak bir .NET nesnesini önbelleğe alabilmek için seri hale getirilmesi gerekir. Bu .NET nesne serileştirmesi uygulama geliştiricisinin sorumluluğundadır ve geliştiriciye seri hale getirici tercihinde esneklik sağlar.

Nesneleri seri hale getirmenin basit bir yolu, [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)’da `JsonConvert` seri hale getirme yöntemlerini kullanmak ve JSON’a ve JSON’dan seri hale getirmektir. Bu bölümde, önbelleğe bir .NET nesnesi ekleyeceksiniz.

Visual Studio’da, **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**’na tıklayın ve Paket Yöneticisi Konsolu penceresinden aşağıdaki komutu çalıştırın.

```powershell
Install-Package Newtonsoft.Json
```

Aşağıdaki `using` deyimini *Program.cs* dosyasının üst kısmına ekleyin:

```csharp
using Newtonsoft.Json;
```

Aşağıdaki `Employee` sınıf tanımını *Program.cs* dosyasına ekleyin:

```csharp
        class Employee
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public int Age { get; set; }

            public Employee(string EmployeeId, string Name, int Age)
            {
                this.Id = EmployeeId;
                this.Name = Name;
                this.Age = Age;
            }
        }
```

*Program.cs* dosyasındaki `Main()` yordamının alt kısmına ve `Dispose()` için çağrı yapılmadan önce aşağıdaki kod satırlarını önbelleğe ekleyin ve seri hale getirilmiş bir .NET nesnesi alın:

```csharp
            // Store .NET object to cache
            Employee e007 = new Employee("007", "Davide Columbo", 100);
            Console.WriteLine("Cache response from storing Employee .NET object : " + 
                cache.StringSet("e007", JsonConvert.SerializeObject(e007)));

            // Retrieve .NET object from cache
            Employee e007FromCache = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e007"));
            Console.WriteLine("Deserialized Employee .NET object :\n");
            Console.WriteLine("\tEmployee.Name : " + e007FromCache.Name);
            Console.WriteLine("\tEmployee.Id   : " + e007FromCache.Id);
            Console.WriteLine("\tEmployee.Age  : " + e007FromCache.Age + "\n");
```

.NET nesnelerinin serileştirilmesini test etmek üzere konsol uygulamasını oluşturmak ve çalıştırmak için **Ctrl+F5** tuşlarına basın. 

![Konsol uygulaması tamamlandı](./media/cache-dotnet-how-to-use-azure-redis-cache/cache-console-app-complete.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki öğreticiyle devam edecekseniz, bu hızlı başlangıçta oluşturulan kaynakları tutabilir ve sonraki öğreticide yeniden kullanabilirsiniz.

Aksi takdirde, hızlı başlangıç örnek uygulamasını tamamladıysanız ücret yansıtılmaması için bu hızlı başlangıçta oluşturulan Azure kaynaklarını silebilirsiniz. 

> [!IMPORTANT]
> Bir kaynak grubunu silme işlemi geri alınamaz ve kaynak grubunun ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. Bu örneği, tutmak istediğiniz kaynakları içeren mevcut bir kaynak grubunda barındırmak için kaynaklar oluşturduysanız, kaynak grubunu silmek yerine her kaynağı kendi ilgili dikey penceresinden tek tek silebilirsiniz.
>

[Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak grupları**’na tıklayın.

**Ada göre filtrele...** metin kutusuna kaynak grubunuzun adını girin. Bu makaledeki yönergelerde *TestResources* adlı bir kaynak grubu kullanılmıştır. Sonuç listesindeki kaynak grubunuzda **...** ve sonra **Kaynak grubunu sil**’e tıklayın.

![Sil](./media/cache-dotnet-how-to-use-azure-redis-cache/cache-delete-resource-group.png)

Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını yazın ve **Sil**’e tıklayın.

Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.



<a name="next-steps"></a>

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure önbelleği için Redis bir .NET uygulamasından kullanma öğrendiniz. Azure önbelleği için Redis ile ASP.NET web uygulaması kullanmak sonraki hızlı başlangıca devam edin.

> [!div class="nextstepaction"]
> [Bir Azure önbelleği için Redis kullanan ASP.NET web uygulaması oluşturun.](./cache-web-app-howto.md)


