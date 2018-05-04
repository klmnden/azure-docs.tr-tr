---
title: Yönetilen hizmet kimliği kullanarak App Service’ten Azure SQL Veritabanı bağlantısını güvenli hale getirme | Microsoft Docs
description: Yönetilen hizmet kimliği kullanarak veritabanı bağlantısını daha güvenli hale getirme ve ayrıca bunu diğer Azure hizmetlerine uygulama hakkında bilgi edinin.
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: syntaxc4
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 04/17/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 1b51638754287d3359eaea7bd5da3f71bf15cc89
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="tutorial-secure-sql-database-connection-with-managed-service-identity"></a>Öğretici: Yönetilen hizmet kimliği ile SQL Veritabanı bağlantısını güvenli hale getirme

[App Service](app-service-web-overview.md), Azure’da yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Ayrıca, uygulamanız için [Azure SQL Veritabanı](/azure/sql-database/)’na ve diğer Azure hizmetlerine erişimi güvenli hale getirmeye yönelik anahtar teslim bir çözüm olan [yönetilen hizmet kimliği](app-service-managed-service-identity.md) sağlar. App Service içindeki yönetilen hizmet kimlikleri, bağlantı dizelerindeki kimlik bilgileri gibi uygulamanızdaki gizli dizileri ortadan kaldırarak uygulamanızı daha güvenli hale getirir. Bu öğreticide, [Öğretici: Azure’da SQL Veritabanı ile ASP.NET uygulaması derleme](app-service-web-tutorial-dotnet-sqldatabase.md) bölümünde derlediğiniz örnek ASP.NET web uygulamasına yönetilen hizmet kimliği ekleyeceksiniz. İşiniz bittiğinde, örnek uygulamanız kullanıcı adı ve parolaya gerek kalmadan SQL Veritabanıa güvenli bir şekilde bağlanacaktır.

Aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Yönetilen hizmet kimliğini etkinleştirme
> * Hizmet kimliğine SQL Veritabanı erişimi verme
> * Uygulama kodunu Azure Active Directory kimlik doğrulaması kullanarak SQL Veritabanı ile kimlik doğrulaması yapacak şekilde yapılandırma
> * SQL Veritabanında hizmet kimliğine en düşük ayrıcalıkları verme

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu makale, [Öğretici: Azure’da SQL Veritabanı ile ASP.NET uygulaması derleme](app-service-web-tutorial-dotnet-sqldatabase.md) bölümünde bıraktığınız yerden devam eder. Henüz yapmadıysanız önce o öğreticiyi takip edin. Alternatif olarak, adımları SQL Veritabanı ile kendi ASP.NET uygulamanıza uyarlayabilirsiniz.

<!-- ![app running in App Service](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png) -->

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="enable-managed-service-identity"></a>Yönetilen hizmet kimliğini etkinleştirme

Azure uygulamanızda bir hizmet kimliği etkinleştirmek için Cloud Shell’de [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az_webapp_identity_assign) komutunu kullanın. Aşağıdaki komutta *\<app name>* değerini değiştirin.

```azurecli-interactive
az webapp identity assign --resource-group myResourceGroup --name <app name>
```

Kimlik Azure Active Directory’de oluşturulduktan sonra elde edilen çıktının bir örneği aşağıda verilmiştir:

```json
{
  "additionalProperties": {},
  "principalId": "21dfa71c-9e6f-4d17-9e90-1d28801c9735",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "type": "SystemAssigned"
}
```

`principalId` değerini sonraki adımda kullanacaksınız. Azure Active Directory'de yeni kimliğin ayrıntılarını görmek isterseniz aşağıdaki isteğe bağlı komutu `principalId` değeriyle çalıştırın:

```azurecli-interactive
az ad sp show --id <principalid>`
```

## <a name="grant-database-access-to-identity"></a>Kimliğe veritabanı erişimi verme

Ardından, Cloud Shell’de [`az sql server ad-admin create`](/cli/azure/sql/server/ad-admin?view=azure-cli-latest#az_sql_server_ad-admin_create) komutunu kullanarak uygulamanızın hizmet kimliğine veritabanı erişimi verin. Aşağıdaki komutta *\<server_name>* ve <principalid_from_last_step> değerlerini değiştirin. *\<admin_user>* için bir yönetici adı yazın.

```azurecli-interactive
az sql server ad-admin create --resource-group myResourceGroup --server-name <server_name> --display-name <admin_user> --object-id <principalid_from_last_step>
```

Yönetilen hizmet kimliği artık Azure SQL Veritabanı sunucunuza erişebilir.

## <a name="modify-connection-string"></a>Bağlantı dizesini değiştirme

Cloud Shell’de [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) komutunu kullanarak uygulamanız için daha önce ayarladığınız bağlantıyı değiştirin. Aşağıdaki komutta *\<app name>* değerini uygulamanızın adıyla, *\<server_name>* ile *\<db_name>* değerlerini ise SQL Veritabanınızın adlarıyla değiştirin.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='Server=tcp:<server_name>.database.windows.net,1433;Database=<db_name>;' --connection-string-type SQLAzure
```

## <a name="modify-aspnet-code"></a>ASP.NET kodunu değiştirme

Visual Studio’daki **DotNetAppSqlDb** projenizde _packages.config_ dosyasını açın ve paket listesine aşağıdaki satırı ekleyin.

```xml
<package id="Microsoft.Azure.Services.AppAuthentication" version="1.1.0-preview" targetFramework="net461" />
```

_Models\MyDatabaseContext.cs_ dosyasını açın ve aşağıdaki `using` deyimlerini dosyanın üstüne ekleyin:

```csharp
using System.Data.SqlClient;
using Microsoft.Azure.Services.AppAuthentication;
using System.Web.Configuration;
```

`MyDatabaseContext` sınıfında aşağıdaki oluşturucuyu ekleyin:

```csharp
public MyDatabaseContext(SqlConnection conn) : base(conn, true)
{
    conn.ConnectionString = WebConfigurationManager.ConnectionStrings["MyDbConnection"].ConnectionString;
    // DataSource != LocalDB means app is running in Azure with the SQLDB connection string you configured
    if(conn.DataSource != "(localdb)\\MSSQLLocalDB")
        conn.AccessToken = (new AzureServiceTokenProvider()).GetAccessTokenAsync("https://database.windows.net/").Result;

    Database.SetInitializer<MyDatabaseContext>(null);
}
```

Bu oluşturucu, App Service’ten Azure SQL Veritabanı için bir erişim belirteci kullanmak üzere özel bir SqlConnection nesnesi yapılandırır. App Service uygulamanız erişim belirtecini kullanarak yönetilen hizmet kimliği ile Azure SQL Veritabanında kimlik doğrulaması yapar. Daha fazla bilgi için bkz. [Azure kaynakları için belirteç edinme](app-service-managed-service-identity.md#obtaining-tokens-for-azure-resources). `if` deyimi, uygulamanızı LocalDB ile yerel olarak test etmeye devam etmenizi sağlar.

> [!NOTE]
> `SqlConnection.AccessToken` şu anda yalnızca .NET Framework 4.6 ve üzerinde desteklenir ve [.NET Core](https://www.microsoft.com/net/learn/get-started/windows)’da desteklenmez.
>

Bu yeni oluşturucuyu kullanmak için `Controllers\TodosController.cs` dosyasını açın ve `private MyDatabaseContext db = new MyDatabaseContext();` satırını bulun. Var olan kod, [siz değiştirmeden](#modify-connection-string) önce düz metin biçiminde kullanıcı adı ve parolası olan standart bağlantı dizesini kullanarak bir veritabanı oluşturmak için `MyDatabaseContext` denetleyicisini kullanır.

Tüm satırı aşağıdaki kod ile değiştirin:

```csharp
private MyDatabaseContext db = new MyDatabaseContext(new SqlConnection());
```

### <a name="publish-your-changes"></a>Değişikliklerinizi yayımlama

Bundan sonra tek yapmanız gereken, değişikliklerinizi Azure'da yayımlamaktır.

**Çözüm Gezgini**’nde **DotNetAppSqlDb** projenize sağ tıklayıp **Yayımla**’yı seçin.

![Çözüm Gezgini'nden yayımlama](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Yayımlama sayfasında **Yayımla**'ya tıklayın. Yeni web sayfası yapılacaklar listenizi gösterdiğinde uygulamanızın yönetilen hizmet kimliğini kullanarak veritabanına bağlanmakta olduğu anlamına gelir.

![Code First Migration’dan sonra Azure web uygulaması](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Bundan sonra yapılacaklar listesini daha önce olduğu gibi düzenleyebilirsiniz.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="grant-minimal-privileges-to-identity"></a>Kimliğe en düşük ayrıcalıkları verme

Önceki adımlarda büyük olasılıkla yönetilen hizmet kimliğinin SQL Server’a Azure AD yöneticisi olarak bağlandığını fark etmişsinizdir. Yönetilen hizmet kimliğinize en düşük ayrıcalıkları vermek için, Azure SQL Veritabanı sunucusunda Azure AD yöneticisi olarak oturum açmanız ve sonra hizmet kimliğini içeren bir Azure Active Directory grubu eklemeniz gerekir. 

### <a name="add-managed-service-identity-to-an-azure-active-directory-group"></a>Yönetilen hizmet kimliğini bir Azure Active Directory grubuna ekleme

Cloud Shell’de uygulamanızın yönetilen hizmet kimliğini aşağıdaki dizede gösterildiği gibi _myAzureSQLDBAccessGroup_ adlı yeni bir Azure Active Directory grubuna ekleyin:

```azurecli-interactive
groupid=$(az ad group create --display-name myAzureSQLDBAccessGroup --mail-nickname myAzureSQLDBAccessGroup --query objectId --output tsv)
msiobjectid=$(az webapp identity show --resource-group <group_name> --name <app_name> --query principalId --output tsv)
az ad group member add --group $groupid --member-id $msiid
az ad group member list -g $groupid
```

Her komut için tam JSON çıktısını görmek istiyorsanız, `--query objectId --output tsv` parametrelerini bırakın.

### <a name="reconfigure-azure-ad-administrator"></a>Azure AD yöneticisini yeniden yapılandırma

Daha önce, yönetilen hizmet kimliğini SQL Veritabanınızın Azure AD yöneticisi olarak atamıştınız. Bu kimliği etkileşimli oturum açma için (veritabanı kullanıcıları eklemek amacıyla) kullanamayacağınızdan gerçek Azure AD kullanıcınızı kullanmanız gerekir. Azure AD kullanıcınızı eklemek için [Azure SQL Veritabanı Sunucunuz için bir Azure Active Directory yöneticisi sağlama](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) bölümündeki adımları izleyin. 

### <a name="grant-permissions-to-azure-active-directory-group"></a>Azure Active Directory grubuna izinler verme

Cloud Shell’de SQLCMD komutunu kullanarak SQL Veritabanı oturumunu açın. _\<servername>_ değerini SQL Veritabanı sunucunuzun adıyla, _\<AADusername>_ ile _\<AADpassword>_ değerini ise Azure AD kullanıcınızın kimlik bilgileriyle değiştirin.

```azurecli-interactive
sqlcmd -S <server_name>.database.windows.net -U <AADuser_name> -P "<AADpassword>" -G -l 30
```

SQL isteminde, daha önce kullanıcı olarak oluşturduğunuz Azure Active Directory grubunu ekleyen aşağıdaki komutları çalıştırın.

```sql
CREATE USER [myAzureSQLDBAccessGroup] FROM EXTERNAL PROVIDER;
GO
```

Cloud Shell istemine geri dönmek için `EXIT` yazın. Ardından SQLCMD komutunu tekrar çalıştırın ancak bu kez _\<dbname>_ içinde veritabanı adını belirtin.

```azurecli-interactive
sqlcmd -S <server_name>.database.windows.net -d <db_name> -U <AADuser_name> -P "<AADpassword>" -G -l 30
```

İstediğiniz veritabanının SQL isteminde Azure Active Directory grubuna okuma ve yazma izinleri vermek için aşağıdaki komutları çalıştırın.

```sql
ALTER ROLE db_datareader ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_datawriter ADD MEMBER [myAzureSQLDBAccessGroup];
GO
```

## <a name="next-steps"></a>Sonraki adımlar

Öğrendikleriniz:

> [!div class="checklist"]
> * Yönetilen hizmet kimliğini etkinleştirme
> * Hizmet kimliğine SQL Veritabanı erişimi verme
> * Uygulama kodunu Azure Active Directory kimlik doğrulaması kullanarak SQL Veritabanı ile kimlik doğrulaması yapacak şekilde yapılandırma
> * SQL Veritabanında hizmet kimliğine en düşük ayrıcalıkları verme

Web uygulamanıza özel bir DNS adı eşlemeyle ilgili bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](app-service-web-tutorial-custom-domain.md)