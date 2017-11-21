---
title: "Linux üzerinde Azure App Service'te bir .NET Core ve SQL veritabanı web uygulaması oluşturma | Microsoft Docs"
description: "Linux üzerinde Azure App Service'te bir SQL veritabanına bağlantı çalışma .NET Core uygulamayı Al öğrenin."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: syntaxc4
editor: 
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 10/10/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: ef68f64437935f08f76c29ecf15d574279cca7f1
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2017
---
# <a name="build-a-net-core-and-sql-database-web-app-in-azure-app-service-on-linux"></a>Linux üzerinde Azure App Service'te bir .NET Core ve SQL veritabanı web uygulaması oluşturma

[Uygulama hizmeti Linux'ta](app-service-linux-intro.md) düzeyde ölçeklenebilir, otomatik olarak düzeltme eki uygulama web hizmetini kullanarak Linux işletim sistemi barındırma sağlar. Bu öğretici, bir .NET Core web uygulaması oluşturmak ve bir SQL veritabanına bağlanmak gösterilmiştir. İşiniz bittiğinde, App Service'te Linux üzerinde çalışan .NET Core MVC uygulamasına sahip olacaksınız.

![App Service içinde Linux üzerinde çalışan uygulama](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

Ne hakkında bilgi edineceksiniz nasıl yapılır:

> [!div class="checklist"]
> * Azure SQL veritabanı oluşturma
> * Bir .NET connect SQL veritabanına çekirdek uygulama
> * Uygulamayı Azure'a dağıtma
> * Veri modeli güncelleştirme ve uygulamayı yeniden dağıtın
> * Azure Stream tanılama günlükleri
> * Azure portalında uygulama yönetme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

1. [Git'i yükleyin](https://git-scm.com/)
1. [.NET Core SDK 1.1.2 yükleyin](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.1.2-download.md)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-local-net-core-app"></a>Yerel .NET Core uygulaması oluşturma

Bu adımda, yerel .NET Core projesi ayarlayın.

### <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Terminal penceresinde `cd` bir çalışma dizini için.

Örnek depoyu kopyalayın ve kendi kök dizinine değiştirmek için aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/azure-samples/dotnetcore-sqldb-tutorial
cd dotnetcore-sqldb-tutorial
```

Temel CRUD (Oluştur-okunur-güncelleştirme-Sil) uygulamasını kullanarak bir örnek proje içeren [Entity Framework Çekirdek](https://docs.microsoft.com/ef/core/).

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Gereken paketleri yüklemek, veritabanı geçişler çalıştırın ve uygulamayı başlatmak için aşağıdaki komutları çalıştırın.

```bash
dotnet restore
dotnet ef database update
dotnet run
```

Bir tarayıcıda `http://localhost:5000` sayfasına gidin. Seçin **Yeni Oluştur** bağlayın ve birkaç oluşturun _Yapılacaklar_ öğeleri.

![başarılı bir şekilde SQL veritabanına bağlar](./media/tutorial-dotnetcore-sqldb-app/local-app-in-browser.png)

.NET Core herhangi bir zamanda durdurmak için basın `Ctrl+C` Terminal.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-production-sql-database"></a>Üretim SQL veritabanı oluşturma

Bu adımda, Azure SQL veritabanı oluşturun. Uygulamanızın Azure'a dağıtıldığında, bu bulut veritabanını kullanır.

SQL veritabanı için Bu öğretici kullanır [Azure SQL veritabanı](/azure/sql-database/).

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)]

### <a name="create-a-sql-database-logical-server"></a>SQL Database mantıksal sunucusu oluşturma

SQL Database mantıksal sunucusu ile bulut Kabuğu'nda oluşturma [az sql server oluşturun](/cli/azure/sql/server#create) komutu.

Değiştir  *\<sunucu_adı >* yer tutucu içeren benzersiz bir SQL veritabanı adı. Bu ad, SQL veritabanı endpoint parçası olarak kullanılacaktır `<server_name>.database.windows.net`, adının Azure içindeki tüm mantıksal sunucular arasında benzersiz olması gerekir. Ad yalnızca küçük harf, sayı ve tire (-) karakterini içermelidir ve 3 ila 50 karakter uzunluğunda olmalıdır. Ayrıca, değiştirin  *\<db_username >* ve  *\<db_password >* bir kullanıcı adı ve parolayla tercih ettiğiniz. 


```azurecli-interactive
az sql server create --name <server_name> --resource-group myResourceGroup --location "West Europe" --admin-user <db_username> --admin-password <db_password>
```

SQL Database mantıksal sunucusu oluşturulduğunda, Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir:

```json
{
  "administratorLogin": "sqladmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/<server_name>",
  "identity": null,
  "kind": "v12.0",
  "location": "westeurope",
  "name": "<server_name>",
  "resourceGroup": "myResourceGroup",
  "state": "Ready",
  "tags": null,
  "type": "Microsoft.Sql/servers",
  "version": "12.0"
}
```

### <a name="configure-a-server-firewall-rule"></a>Sunucu güvenlik duvarı kurallarını yapılandırma

[az sql server firewall create](/cli/azure/sql/server#create) komutunu kullanarak [sunucu düzeyinde bir Azure SQL Veritabanı güvenlik duvarı kuralı](../../sql-database/sql-database-firewall-configure.md) oluşturun. Başlangıç IP ve bitiş IP 0.0.0.0 olarak ayarladığınızda, Güvenlik Duvarı'nı yalnızca diğer Azure kaynakları için açıldı. 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server <server_name> --name AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

### <a name="create-a-database"></a>Veritabanı oluşturma

[az sql db create](/cli/azure/sql/db#create) komutunu kullanarak [S0 performans düzeyine](../../sql-database/sql-database-service-tiers.md) sahip bir veritabanı oluşturun.

```azurecli-interactive
az sql db create --resource-group myResourceGroup --server <server_name> --name coreDB --service-objective S0
```

### <a name="create-connection-string"></a>Bağlantı dizesi oluştur

Aşağıdaki dizesi ile değiştirin  *\<sunucu_adı >*,  *\<db_username >*, ve  *\<db_password >* , daha önce kullanılır.

```
Server=tcp:<server_name>.database.windows.net,1433;Initial Catalog=coreDB;Persist Security Info=False;User ID=<db_username>;Password=<db_password>;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

Bu, .NET Core uygulamanızı bağlantı dizesi değil. Daha sonra kullanılmak üzere kopyalayın.

## <a name="deploy-app-to-azure"></a>Azure için uygulama dağıtma

Bu adımda, .NET Core SQL veritabanına bağlı uygulamanızı Linux'ta App Service'e dağıtın.

### <a name="configure-local-git-deployment"></a>Yerel git dağıtımını yapılandırma

[!INCLUDE [Configure a deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-dotnetcore-no-h.md)] 

### <a name="configure-an-environment-variable"></a>Bir ortam değişkeni yapılandırın

Azure uygulamanızı bağlantı dizesini ayarlamak için kullanın [az webapp config appsettings güncelleştirme](/cli/azure/webapp/config/appsettings#update) bulut Kabuğu'nda komutu. Aşağıdaki komutta,  *\<uygulama adı >*, yanı sıra  *\<connection_string >* daha önce oluşturduğunuz bağlantı dizesiyle parametresi.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='<connection_string>' --connection-string-type SQLServer
```

Ardından, ayarlayın `ASPNETCORE_ENVIRONMENT` uygulama ayarının _üretim_. Bu ayar, Azure'da çalışan SQLLite yerel geliştirme ortamı ve SQL veritabanı için Azure ortamınız için kullandığınız olduğundan bilmenizi sağlar.

Aşağıdaki örnek yapılandırır bir `ASPNETCORE_ENVIRONMENT` , Azure web uygulaması uygulama ayarı. Değiştir  *\<app_name >* yer tutucu.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings ASPNETCORE_ENVIRONMENT="Production"
```

### <a name="connect-to-sql-database-in-production"></a>Üretimde SQL veritabanına bağlan

Yerel deponuzu haline açın ve aşağıdaki kodu bulun:

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
```

Daha önce yapılandırılmış ortam değişkenleri kullanan aşağıdaki kodla değiştirin.

```csharp
// Use SQL Database if in Azure, otherwise, use SQLLite
if(Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") == "Production")
    services.AddDbContext<DotNetCoreSqlDbContext>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
else
    services.AddDbContext<DotNetCoreSqlDbContext>(options =>
            options.UseSqlite("Data Source=MvcMovie.db"));

// Automatically perform database migration
services.BuildServiceProvider().GetService<DotNetCoreSqlDbContext>().Database.Migrate();
```

Bu kod (Azure ortamı gösteren) üretimde çalışan sonra bağlantı dizesini kullanır algılarsa SQL veritabanına bağlanmak üzere yapılandırılmış.

`Database.Migrate()` Çağrısı yardımcı olur, Azure'da çalıştırdığınızda, otomatik olarak veritabanları, oluşturduğundan, geçiş yapılandırmasına bağlı olarak, .NET Core uygulama gereksinimlerinize. 

Yaptığınız değişiklikleri kaydedin.

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash
Counting objects: 98, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (92/92), done.
Writing objects: 100% (98/98), 524.98 KiB | 5.58 MiB/s, done.
Total 98 (delta 8), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: .
remote: Updating submodules.
remote: Preparing deployment for commit id '0c497633b8'.
remote: Generating deployment script.
remote: Project file path: ./DotNetCoreSqlDb.csproj
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling ASP.NET Core Web Application deployment.
remote: .
remote: .
remote: .
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulaması'na göz atın

Web tarayıcınız üzerinden dağıtılan web uygulamasının göz atın.

```bash
http://<app_name>.azurewebsites.net
```

Birkaç Yapılacaklar öğelerini ekleyin.

![App Service içinde Linux üzerinde çalışan uygulama](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

**Tebrikler!** Linux üzerinde bir veri güdümlü .NET Core uygulaması App Service'te çalıştırdığınız.

## <a name="update-locally-and-redeploy"></a>Yerel olarak güncelleştirin ve yeniden dağıtın

Bu adımda, veritabanı şemanızı değişiklik ve Azure'a yayımlayacaksınız.

### <a name="update-your-data-model"></a>Veri modelinizi güncelleştir

Açık _Models\Todo.cs_ Kod düzenleyicisinde. Aşağıdaki özellik ekleme `ToDo` sınıfı:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Code First Migrations yerel olarak çalıştırma

Yerel veritabanınızı güncelleştirme yapmak için birkaç komutları çalıştırın.

```bash
dotnet ef migrations add AddProperty
```

Yerel veritabanı güncelleştirin:

```bash
dotnet ef database update
```

### <a name="use-the-new-property"></a>Yeni özelliğini kullanın

Kodunuzu kullanmak için bazı değişiklikler yapmak `Done` özelliği. Bu öğreticide kolaylık olması için yalnızca değiştirme oluşturacağız `Index` ve `Create` görünümleri eylem özelliğine bakın.

Açık _Controllers\TodosController.cs_.

Bul `Create()` yöntemi ekleyin `Done` özelliklerinde listesine `Bind` özniteliği. İşiniz bittiğinde, `Create()` yöntemi imza aşağıdaki kod gibi görünür:

```csharp
public async Task<IActionResult> Create([Bind("ID,Description,CreatedDate,Done")] Todo todo)
```

Açık _Views\Todos\Create.cshtml_.

Razor kodunda görmelisiniz bir `<div class="form-group">` öğesi için `Description`ve ardından başka bir `<div class="form-group">` öğesi için `CreatedDate`. Bu iki öğenin hemen ardından, eklemek başka `<div class="form-group">` öğesi için `Done`:

```csharp
<div class="form-group">
    <label asp-for="Done" class="col-md-2 control-label"></label>
    <div class="col-md-10">
        <input asp-for="Done" class="form-control" />
        <span asp-validation-for="Done" class="text-danger"></span>
    </div>
</div>
```

Açık _Views\Todos\Index.cshtml_.

Boş bir Ara `<th></th>` öğesi. Bu öğe yalnızca aşağıdaki Razor kodu ekleyin:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Bul `<td>` içeren öğeyi `asp-action` etiket Yardımcıları. Bu öğe yalnızca aşağıdaki Razor kodu ekleyin:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.CreatedDate)
</td>
```

Tüm değişiklikleri görmek için ihtiyacınız olan `Index` ve `Create` görünümleri.

### <a name="test-your-changes-locally"></a>Yaptığınız değişiklikler yerel olarak test etme

Uygulamayı yerel olarak çalıştırın.

```bash
dotnet run
```

Tarayıcınızda gidin `http://localhost:5000/`. Şimdi Yapılacaklar öğesi ekleyin ve denetleme **Bitti**. Ardından, giriş sayfanız tamamlanmış bir öğe olarak gösterilmesi gerekir. Unutmayın `Edit` değil görünümü göster `Done` alan, değişmedi çünkü `Edit` görünümü.

### <a name="publish-changes-to-azure"></a>Değişiklikler için Azure yayımlama

```bash

git commit -am "added done field"
git push azure master
```

Bir kez `git push` tamamlamak, Azure web uygulamasına gidin ve yeni işlevselliği deneyin.

![Kod ilk geçişten sonra Azure web uygulaması](./media/tutorial-dotnetcore-sqldb-app/this-one-is-done.png)

Tüm mevcut Yapılacaklar öğelerini hala görüntülenir. .NET Core uygulamanızı yayımladığınızda, SQL veritabanında var olan veri kaybı olmadığından. Ayrıca, Entity Framework Çekirdek geçişler yalnızca veri şemasını değiştirir ve varolan verilerinizi dokunmaz.

## <a name="manage-your-azure-web-app"></a>Azure web uygulamanızı yönetme

Git [Azure portal](https://portal.azure.com) oluşturduğunuz web uygulaması görmek için.

Sol menüden **Uygulama Hizmetleri**’ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/tutorial-dotnetcore-sqldb-app/access-portal.png)

Varsayılan olarak, portal, web uygulamanızın gösterir **genel bakış** sayfası. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafında sekmeleri açabilir farklı yapılandırma sayfalarında gösterilir.

![Azure portalında App Service sayfası](./media/tutorial-dotnetcore-sqldb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Sonraki adımlar

Öğrendiklerinizi:

> [!div class="checklist"]
> * Azure SQL veritabanı oluşturma
> * Bir .NET connect SQL veritabanına çekirdek uygulama
> * Uygulamayı Azure'a dağıtma
> * Veri modeli güncelleştirme ve uygulamayı yeniden dağıtın
> * Azure akış günlükleri, terminal
> * Azure portalında uygulama yönetme

Web uygulamanıza özel bir DNS adı eşleme öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](../app-service-web-tutorial-custom-domain.md)