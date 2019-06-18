---
title: Yönetilen kimlik - Azure App Service ile SQL veritabanı bağlantısını güvenli hale getirme | Microsoft Docs
description: Yönetilen kimlik kullanarak veritabanı bağlantısını daha güvenli hale getirme ve ayrıca bunu diğer Azure hizmetlerine uygulama hakkında bilgi edinin.
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
ms.date: 11/30/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 548cd3de6d2eff9f2077ca66b66d5c60aa84f7e2
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67154207"
---
# <a name="tutorial-secure-azure-sql-database-connection-from-app-service-using-a-managed-identity"></a>Öğretici: Yönetilen kimlik kullanarak App Service'ten Azure SQL veritabanı bağlantısını güvenli hale getirme

[App Service](overview.md), Azure’da yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Ayrıca, uygulamanız için [Azure SQL Veritabanı](/azure/sql-database/)’na ve diğer Azure hizmetlerine erişimi güvenli hale getirmeye yönelik anahtar teslim bir çözüm olan [yönetilen kimliği](overview-managed-identity.md) sağlar. App Service içindeki yönetilen kimlikler, bağlantı dizelerindeki kimlik bilgileri gibi uygulamanızdaki gizli dizileri ortadan kaldırarak uygulamanızı daha güvenli hale getirir. Bu öğreticide, bölümünde derlediğiniz örnek ASP.NET web uygulamasına yönetilen kimlik ekleyeceksiniz [Öğreticisi: Azure'da SQL veritabanı ile ASP.NET uygulaması derleme](app-service-web-tutorial-dotnet-sqldatabase.md). İşiniz bittiğinde, örnek uygulamanız kullanıcı adı ve parolaya gerek kalmadan SQL Veritabanıa güvenli bir şekilde bağlanacaktır.

> [!NOTE]
> Bu senaryo şu an için .NET Framework 4.6 ve üzeri tarafından desteklenmektedir ancak [.NET Core 2.1](https://www.microsoft.com/net/learn/get-started/windows) ile kullanılamaz. [.NET Core 2.2](https://www.microsoft.com/net/download/dotnet-core/2.2) senaryoyu destekler ancak henüz App Service varsayılan görüntülerine dahil edilmemiştir. 
>

Aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Yönetilen kimlikleri etkinleştirme
> * Yönetilen kimliğe SQL Veritabanı erişimi verme
> * Uygulama kodunu Azure Active Directory kimlik doğrulaması kullanarak SQL Veritabanı ile kimlik doğrulaması yapacak şekilde yapılandırma
> * SQL Veritabanında yönetilen kimliğe en düşük ayrıcalıkları verme

> [!NOTE]
>Azure Active Directory kimlik doğrulaması, şirket içi Active Directory’deki (AD DS) [Tümleşik Windows kimlik doğrulamasından](/previous-versions/windows/it-pro/windows-server-2003/cc758557(v=ws.10)) _farklıdır_. AD DS ve Azure Active Directory tamamen farklı kimlik doğrulama protokolleri kullanır. Daha fazla bilgi için [Azure AD Domain Services belgeleri](https://docs.microsoft.com/azure/active-directory-domain-services/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, kaldığınız yerden devam [Öğreticisi: Azure'da SQL veritabanı ile ASP.NET uygulaması derleme](app-service-web-tutorial-dotnet-sqldatabase.md). Henüz yapmadıysanız önce o öğreticiyi takip edin. Alternatif olarak, adımları SQL Veritabanı ile kendi ASP.NET uygulamanıza uyarlayabilirsiniz.

<!-- ![app running in App Service](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png) -->

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="enable-managed-identities"></a>Yönetilen kimlikleri etkinleştirme

Azure uygulamanızda bir yönetilen kimlik etkinleştirmek için Cloud Shell’de [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) komutunu kullanın. Aşağıdaki komutta *\<app name>* değerini değiştirin.

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
az ad sp show --id <principalid>
```

## <a name="grant-database-access-to-identity"></a>Kimliğe veritabanı erişimi verme

Ardından, Cloud Shell’de [`az sql server ad-admin create`](/cli/azure/sql/server/ad-admin?view=azure-cli-latest) komutunu kullanarak uygulamanızın yönetilen kimliğe veritabanı erişimi verin. Aşağıdaki komutta *\<server_name>* ve <principalid_from_last_step> değerlerini değiştirin. *\<admin_user>* için bir yönetici adı yazın.

```azurecli-interactive
az sql server ad-admin create --resource-group myResourceGroup --server-name <server_name> --display-name <admin_user> --object-id <principalid_from_last_step>
```

Yönetilen kimlik artık Azure SQL Veritabanı sunucunuza erişebilir.

> [!IMPORTANT]
> Kolaylık olması için bu adım, yönetilen Azure AD kimlik SQL veritabanı yönetici olarak yapılandırır. Yöntemi aşağıdaki sınırlamalara sahiptir:
>
> - Uygulamanın yönetim erişimi, en iyi güvenlik uygulamalarını izleyin değil.
> - Yönetilen kimlik belirli bir uygulama olduğundan, başka bir uygulamadan SQL veritabanına bağlanmak için aynı yönetilen kimlik kullanamazsınız.
> - Ek uygulamalar yönetilen kimliklerini erişim vermek mümkün olacak şekilde yönetilen kimlik SQL veritabanına etkileşimli olarak oturum açamaz. 
>
> Güvenliği artırmak ve SQL veritabanı'nda Azure AD hesapları yönetmek için adımları izleyin [kimliğe en düşük ayrıcalıkları verme](#grant-minimal-privileges-to-identity).

## <a name="modify-connection-string"></a>Bağlantı dizesini değiştirme

Cloud Shell’de [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) komutunu kullanarak uygulamanız için daha önce ayarladığınız bağlantıyı değiştirin. Aşağıdaki komutta *\<app name>* değerini uygulamanızın adıyla, *\<server_name>* ile *\<db_name>* değerlerini ise SQL Veritabanınızın adlarıyla değiştirin.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='Server=tcp:<server_name>.database.windows.net,1433;Database=<db_name>;' --connection-string-type SQLAzure
```

## <a name="modify-aspnet-code"></a>ASP.NET kodunu değiştirme

Visual Studio'da Paket Yöneticisi konsolunu açın ve NuGet paketi ekleme [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication):

```powershell
Install-Package Microsoft.Azure.Services.AppAuthentication -Version 1.1.0-preview
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

Bu oluşturucu, App Service’ten Azure SQL Veritabanı için bir erişim belirteci kullanmak üzere özel bir SqlConnection nesnesi yapılandırır. App Service uygulamanız erişim belirtecini kullanarak yönetilen kimlik ile Azure SQL Veritabanında kimlik doğrulaması yapar. Daha fazla bilgi için bkz. [Azure kaynakları için belirteç edinme](overview-managed-identity.md#obtaining-tokens-for-azure-resources). `if` deyimi, uygulamanızı LocalDB ile yerel olarak test etmeye devam etmenizi sağlar.

> [!NOTE]
> `SqlConnection.AccessToken` şu anda yalnızca .NET Framework 4.6 ve üzerinde desteklenir, [.NET Core 2.2](https://www.microsoft.com/net/download/dotnet-core/2.2) için de desteklenir ancak [.NET Core 2.1](https://www.microsoft.com/net/learn/get-started/windows) için destek sunulmaz.
>

Bu yeni oluşturucuyu kullanmak için `Controllers\TodosController.cs` dosyasını açın ve `private MyDatabaseContext db = new MyDatabaseContext();` satırını bulun. Var olan kod, [siz değiştirmeden](#modify-connection-string) önce düz metin biçiminde kullanıcı adı ve parolası olan standart bağlantı dizesini kullanarak bir veritabanı oluşturmak için `MyDatabaseContext` denetleyicisini kullanır.

Tüm satırı aşağıdaki kod ile değiştirin:

```csharp
private MyDatabaseContext db = new MyDatabaseContext(new System.Data.SqlClient.SqlConnection());
```

### <a name="publish-your-changes"></a>Değişikliklerinizi yayımlama

Bundan sonra tek yapmanız gereken, değişikliklerinizi Azure'da yayımlamaktır.

**Çözüm Gezgini**’nde **DotNetAppSqlDb** projenize sağ tıklayıp **Yayımla**’yı seçin.

![Çözüm Gezgini'nden yayımlama](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Yayımlama sayfasında **Yayımla**'ya tıklayın. Yeni web sayfası yapılacaklar listenizi gösterdiğinde uygulamanızın yönetilen kimliğini kullanarak veritabanına bağlanmakta olduğu anlamına gelir.

![Code First Migration'dan sonra Azure uygulaması](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Bundan sonra yapılacaklar listesini daha önce olduğu gibi düzenleyebilirsiniz.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="grant-minimal-privileges-to-identity"></a>Kimliğe en düşük ayrıcalıkları verme

Önceki adımlarda büyük olasılıkla yönetilen kimliğin SQL Server’a Azure AD yöneticisi olarak bağlandığını fark etmişsinizdir. Yönetilen kimliğinize en düşük ayrıcalıkları vermek için, Azure SQL Veritabanı sunucusunda Azure AD yöneticisi olarak oturum açmanız ve sonra yönetilen kimliği içeren bir Azure Active Directory grubu eklemeniz gerekir. 

### <a name="add-managed-identity-to-an-azure-active-directory-group"></a>Yönetilen kimliği bir Azure Active Directory grubuna ekleme

Cloud Shell’de uygulamanızın yönetilen kimliğini aşağıdaki dizede gösterildiği gibi _myAzureSQLDBAccessGroup_ adlı yeni bir Azure Active Directory grubuna ekleyin:

```azurecli-interactive
groupid=$(az ad group create --display-name myAzureSQLDBAccessGroup --mail-nickname myAzureSQLDBAccessGroup --query objectId --output tsv)
msiobjectid=$(az webapp identity show --resource-group <group_name> --name <app_name> --query principalId --output tsv)
az ad group member add --group $groupid --member-id $msiobjectid
az ad group member list -g $groupid
```

Her komut için tam JSON çıktısını görmek istiyorsanız, `--query objectId --output tsv` parametrelerini bırakın.

### <a name="reconfigure-azure-ad-administrator"></a>Azure AD yöneticisini yeniden yapılandırma

Daha önce, yönetilen kimliği SQL Veritabanınızın Azure AD yöneticisi olarak atamıştınız. Bu kimliği etkileşimli oturum açma için (veritabanı kullanıcıları eklemek amacıyla) kullanamayacağınızdan gerçek Azure AD kullanıcınızı kullanmanız gerekir. Azure AD kullanıcınızı eklemek için [Azure SQL Veritabanı Sunucunuz için bir Azure Active Directory yöneticisi sağlama](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) bölümündeki adımları izleyin. 

> [!IMPORTANT]
> Azure AD erişim için SQL veritabanı tamamen (tüm Azure AD hesaplarından) devre dışı bırakmak istemediğiniz sürece eklendikten sonra bu Azure AD Yöneticisi, SQL veritabanı için kaldırmayın.
> 

### <a name="grant-permissions-to-azure-active-directory-group"></a>Azure Active Directory grubuna izinler verme

Cloud Shell’de SQLCMD komutunu kullanarak SQL Veritabanı oturumunu açın. _\<server\_name>_ değerini SQL Veritabanı sunucunuzun adıyla, _\<db\_name>_ değerini uygulamanızın kullandığı veritabanının adıyla, _\<AADuser\_name>_ ve _\<AADpassword>_ değerini ise Azure AD kullanıcınızın kimlik bilgileriyle değiştirin.

```azurecli-interactive
sqlcmd -S <server_name>.database.windows.net -d <db_name> -U <AADuser_name> -P "<AADpassword>" -G -l 30
```

Önceden oluşturduğunuz Azure Active Directory grubunu eklemek ve uygulamanızın ihtiyaç duyduğu izinleri vermek için istediğiniz veritabanının SQL isteminde aşağıdaki komutları çalıştırın. Örneğin, 

```sql
CREATE USER [myAzureSQLDBAccessGroup] FROM EXTERNAL PROVIDER;
ALTER ROLE db_datareader ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_datawriter ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_ddladmin ADD MEMBER [myAzureSQLDBAccessGroup];
GO
```

Cloud Shell istemine geri dönmek için `EXIT` yazın. 

## <a name="next-steps"></a>Sonraki adımlar

Öğrendikleriniz:

> [!div class="checklist"]
> * Yönetilen kimlikleri etkinleştirme
> * Yönetilen kimliğe SQL Veritabanı erişimi verme
> * Uygulama kodunu Azure Active Directory kimlik doğrulaması kullanarak SQL Veritabanı ile kimlik doğrulaması yapacak şekilde yapılandırma
> * SQL Veritabanında yönetilen kimliğe en düşük ayrıcalıkları verme

Web uygulamanıza özel bir DNS adı eşlemeyle ilgili bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure App Service'e eşlemek](app-service-web-tutorial-custom-domain.md)
