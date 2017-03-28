.NET uygulamaları, önbellek istemcisi uygulamalarının yapılandırmasını basitleştiren bir NuGet paketi kullanarak Visual Studio’da yapılandırılabilecek **StackExchange.Redis** önbellek istemcisi kullanabilir. 

> [!NOTE]
> Daha fazla bilgi için, bkz. [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) github sayfası ve [StackExchange.Redis önbelleği istemcisi belgeleri](http://github.com/StackExchange/StackExchange.Redis#documentation).
> 
> 

Visual Studio’da StackExchange.Redis NuGet paketi kullanarak bir istemci uygulamasını yapılandırmak için, **Çözüm Gezgini**’nde sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin. 

![NuGet paketlerini yönetme](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

Arama metin kutusuna **StackExchange.Redis** ya da **StackExchange.Redis.StrongName** yazın, sonuçlardan istediğiniz sürümü seçin ve **Yükle**’ye tıklayın.

> [!NOTE]
> **StackExchange.Redis** istemci kitaplığının kesin adlandırılmış bir sürümünü kullanmak isterseniz, **StackExchange.Redis.StrongName** seçeneğin seçin; istemezseniz **StackExchange.Redis** seçeneğin seçin.
> 
> 

![StackExchange.Redis NuGet paketi](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

NuGet paketi, StackExchange.Redis önbellek istemcisiyle Azure Redis Önbelleğine erişmek üzere istemci uygulamanız için gerekli derleme başvurularını ekler.

> [!NOTE]
> Daha önce projenizi StackExchange.Redis kullanacak şekilde yapılandırdıysanız **NuGet Paket Yöneticisi**’nden paket güncelleştirmelerini denetleyebilirsiniz. StackExchange.Redis NuGet paketinin güncelleştirilmiş sürümlerini denetlemek ve yüklemek için **NuGet Paket Yöneticisi** penceresinde **Güncelleştirmeler**’e tıklayın. StackExchange.Redis NuGet paketin bir güncelleştirme varsa projenizi güncelleştirilmiş sürüme yükseltebilirsiniz.
> 
> 

Ayrıca, **Araçlar** menüsünden **NuGet Paket Yöneticisi**, **Paket Yöneticisi Konsolu**’na tıklayıp `Package Manager Console` penceresinde aşağıdaki komutu çalıştırarak StackExchange.Redis NuGet paketini yükleyebilirsiniz.
    
```
Install-Package StackExchange.Redis
```