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
ms.date: 06/21/2019
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 31535642526c608ad0ae29e5c0e3c93368e184ad
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67481007"
---
# <a name="tutorial-secure-azure-sql-database-connection-from-app-service-using-a-managed-identity"></a>Öğretici: Yönetilen kimlik kullanarak App Service'ten Azure SQL veritabanı bağlantısını güvenli hale getirme

[App Service](overview.md), Azure’da yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Ayrıca, uygulamanız için [Azure SQL Veritabanı](/azure/sql-database/)’na ve diğer Azure hizmetlerine erişimi güvenli hale getirmeye yönelik anahtar teslim bir çözüm olan [yönetilen kimliği](overview-managed-identity.md) sağlar. App Service içindeki yönetilen kimlikler, bağlantı dizelerindeki kimlik bilgileri gibi uygulamanızdaki gizli dizileri ortadan kaldırarak uygulamanızı daha güvenli hale getirir. Bu öğreticide, bölümünde derlediğiniz örnek ASP.NET web uygulamasına yönetilen kimlik ekleyeceksiniz [Öğreticisi: Azure'da SQL veritabanı ile ASP.NET uygulaması derleme](app-service-web-tutorial-dotnet-sqldatabase.md). İşiniz bittiğinde, örnek uygulamanız kullanıcı adı ve parolaya gerek kalmadan SQL Veritabanıa güvenli bir şekilde bağlanacaktır.

> [!NOTE]
> Bu senaryo, şu anda ve üstü 4.7.2 .NET Framework tarafından desteklenir. [.NET Core 2.2](https://www.microsoft.com/net/download/dotnet-core/2.2) senaryoyu destekler ancak henüz App Service varsayılan görüntülerine dahil edilmemiştir. 
>

Aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Yönetilen kimlikleri etkinleştirme
> * Yönetilen kimliğe SQL Veritabanı erişimi verme
> * Entity Framework, SQL veritabanı ile Azure AD kimlik doğrulamasını kullanmak için yapılandırma
> * Visual Studio'dan Azure AD kimlik doğrulamasını kullanarak SQL veritabanı'na bağlanma

> [!NOTE]
>Azure AD kimlik doğrulaması _farklı_ gelen [tümleşik Windows kimlik doğrulaması](/previous-versions/windows/it-pro/windows-server-2003/cc758557(v=ws.10)) içinde şirket Active Directory (AD DS) içinde. AD DS ve Azure AD tamamen farklı kimlik doğrulama protokolleri kullanır. Daha fazla bilgi için [Azure AD Domain Services belgeleri](https://docs.microsoft.com/azure/active-directory-domain-services/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, kaldığınız yerden devam [Öğreticisi: Azure'da SQL veritabanı ile ASP.NET uygulaması derleme](app-service-web-tutorial-dotnet-sqldatabase.md). Henüz yapmadıysanız önce o öğreticiyi takip edin. Alternatif olarak, adımları SQL Veritabanı ile kendi ASP.NET uygulamanıza uyarlayabilirsiniz.

SQL veritabanı arka uç kullanarak uygulamanızda hata ayıklama için emin olun [bilgisayarınızdan istemci bağlantısına izin](app-service-web-tutorial-dotnet-sqldatabase.md#allow-client-connection-from-your-computer).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="grant-azure-ad-user-access-to-database"></a>GRANT Azure AD kullanıcı veritabanına erişim

SQL veritabanı sunucusu Active Directory Yöneticisi olarak bir Azure AD kullanıcısının atayarak önce SQL veritabanı için Azure AD kimlik doğrulamasını etkinleştirin. Bu kullanıcı, Azure aboneliğiniz için kaydolmak için kullandığınız Microsoft hesabı farklıdır. Bu, oluşturulan, alınan, eşitlenen veya Azure AD ile davet bir kullanıcı olmalıdır. Azure AD kullanıcıları, izin verilen hakkında daha fazla bilgi görmek için [Azure AD özellikleri ve sınırlamaları SQL veritabanı'nda](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations). 

Azure AD kullanarak kullanıcı nesnesi Kimliğini bulmak [ `az ad user list` ](/cli/azure/ad/user?view=azure-cli-latest#az-ad-user-list) değiştirin  *\<kullanıcı asıl adı >* . Sonucu bir değişkene kaydedilir.

```azurecli-interactive
azureaduser=$(az ad user list --filter "userPrincipalName eq '<user-principal-name>'" --query [].objectId --output tsv)
```
> [!TIP]
> Azure AD'deki tüm kullanıcı asıl adlarının listesini görmek için şunu çalıştırın `az ad user list --query [].userPrincipalName`.
>

Bu Azure AD kullanıcısı bir Active Directory kullanarak yönetici olarak Ekle [ `az sql server ad-admin create` ](/cli/azure/sql/server/ad-admin?view=azure-cli-latest#az-sql-server-ad-admin-create) Cloud shell'de komutu. Aşağıdaki komutta  *\<sunucu-adı >* .

```azurecli-interactive
az sql server ad-admin create --resource-group myResourceGroup --server-name <server-name> --display-name ADMIN --object-id $azureaduser
```

Bir Active Directory Yöneticisi ekleme hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı sunucunuz için bir Azure Active Directory Yöneticisi sağlama](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)

## <a name="set-up-visual-studio"></a>Visual Studio'yu ayarlama

Geliştirme ve Visual Studio'da hata ayıklamayı etkinleştirmek için Azure AD Kullanıcınızı Visual Studio'da seçerek ekleyin **dosya** > **hesap ayarları** menüsüne ve ardından gelen **Ekle bir Hesap**.

Azure hizmeti kimlik doğrulaması için Azure AD kullanıcı ayarlamak için seçin **Araçları** > **seçenekleri** ardından menüden **Azure hizmeti kimlik doğrulaması**  >  **Hesap seçimi**. Eklediğiniz Azure AD kullanıcı seçin ve tıklayın **Tamam**.

Artık Azure AD kimlik doğrulamasını kullanarak geliştirme ve arka uç SQL veritabanı ile uygulamanızı hata ayıklama hazırsınız.

## <a name="modify-aspnet-project"></a>ASP.NET projesi değiştirme

Visual Studio'da Paket Yöneticisi konsolunu açın ve NuGet paketi ekleme [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication):

```powershell
Install-Package Microsoft.Azure.Services.AppAuthentication -Version 1.2.0
```

İçinde *Web.config*, dosyanın üst kısmından çalışma ve aşağıdaki değişiklikleri yapın:

- İçinde `<configSections>`, aşağıdaki bölümde bildirimi ekleyebilirsiniz:

    ```xml
    <section name="SqlAuthenticationProviders" type="System.Data.SqlClient.SqlAuthenticationProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
    ```

- Kapanış aşağıda `</configSections>` etiketinde, aşağıdaki XML kodunu ekleyin `<SqlAuthenticationProviders>`.

    ```xml
    <SqlAuthenticationProviders>
      <providers>
        <add name="Active Directory Interactive" type="Microsoft.Azure.Services.AppAuthentication.SqlAppAuthenticationProvider, Microsoft.Azure.Services.AppAuthentication" />
      </providers>
    </SqlAuthenticationProviders>
    ```    

- Adlı bağlantı dizesi bulma `MyDbConnection` değiştirin, `connectionString` değerini `"server=tcp:<server-name>.database.windows.net;database=<db-name>;UID=AnyString;Authentication=Active Directory Interactive"`. Değiştirin  _\<sunucu-adı >_ ve  _\<db-adı >_ sunucu adını ve veritabanı adı.

Tür `Ctrl+F5` uygulamayı yeniden çalıştırmak için. Aynı CRUD uygulamasını tarayıcınızda artık Azure SQL veritabanı'na doğrudan Azure AD kimlik doğrulamasını kullanarak bağlanıyor. Bu kurulum, veritabanı geçişlerini çalıştırarak sağlar. App Service, yönetilen uygulamanın kimlik ile aynı ayarları iş değişikliklerinizi dağıttığınızda sonraki.

## <a name="use-managed-identity-connectivity"></a>Yönetilen kimlik bağlantısı kullanın

Ardından, sistem tarafından atanan bir yönetilen kimlikle SQL veritabanına bağlanmak için App Service uygulamanızı yapılandırın.

### <a name="enable-managed-identity-on-app"></a>Yönetilen uygulama kimliğini etkinleştirme

Azure uygulamanızda bir yönetilen kimlik etkinleştirmek için Cloud Shell’de [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) komutunu kullanın. Aşağıdaki komutta  *\<-adı >* .

```azurecli-interactive
az webapp identity assign --resource-group myResourceGroup --name <app-name>
```

Çıktının bir örneği aşağıda verilmiştir:

```json
{
  "additionalProperties": {},
  "principalId": "21dfa71c-9e6f-4d17-9e90-1d28801c9735",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "type": "SystemAssigned"
}
```

### <a name="add-managed-identity-to-an-azure-ad-group"></a>Yönetilen kimlik bir Azure AD grubuna Ekle

SQL veritabanınız için bu kimlik erişimi vermek için eklemek gereken bir [Azure AD grubu](../active-directory/fundamentals/active-directory-manage-groups.md). İsteğe bağlı olarak Cloud Shell'de adlı yeni bir grup eklemek _myAzureSQLDBAccessGroup_aşağıdaki kodda gösterilmiştir:

```azurecli-interactive
groupid=$(az ad group create --display-name myAzureSQLDBAccessGroup --mail-nickname myAzureSQLDBAccessGroup --query objectId --output tsv)
msiobjectid=$(az webapp identity show --resource-group myResourceGroup --name <app-name> --query principalId --output tsv)
az ad group member add --group $groupid --member-id $msiobjectid
az ad group member list -g $groupid
```

Her komut için tam JSON çıktısını görmek istiyorsanız, `--query objectId --output tsv` parametrelerini bırakın.

### <a name="grant-permissions-to-azure-ad-group"></a>Azure AD grubu için izin ver

Cloud Shell’de SQLCMD komutunu kullanarak SQL Veritabanı oturumunu açın. Değiştirin  _\<sunucu-adı >_ SQL veritabanı sunucu adınız ile  _\<db-adı >_ ile veritabanı adı, uygulama kullanır ve  _\< aad kullanıcı adı >_ ve  _\<aad-password >_ Azure AD kullanıcınızın kimlik bilgileriyle.

```azurecli-interactive
sqlcmd -S <server-name>.database.windows.net -d <db-name> -U <aad-user-name> -P "<aad-password>" -G -l 30
```

İstediğiniz veritabanının SQL isteminde Azure AD eklemek için aşağıdaki komutları grup ve uygulama izinleri çalıştırması gerekir. Örneğin, 

```sql
CREATE USER [myAzureSQLDBAccessGroup] FROM EXTERNAL PROVIDER;
ALTER ROLE db_datareader ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_datawriter ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_ddladmin ADD MEMBER [myAzureSQLDBAccessGroup];
GO
```

Cloud Shell istemine geri dönmek için `EXIT` yazın.

### <a name="modify-connection-string"></a>Bağlantı dizesini değiştirme

Aynı yapılan değişiklikler, unutmayın `Web.config` hangi Visual Studio yönetilen kimlik ile çalışır, varolan bağlantı dizeni uygulamanızda kaldırmak için yapmanız gereken tek şey, bu nedenle oluşturulan uygulamanızı ilk kez dağıtma. Aşağıdaki komutu kullanın, ancak değiştirin  *\<-adı >* uygulamanızın adına sahip.

```azurecli-interactive
az webapp config connection-string delete --resource-group myResourceGroup --name <app-name> --setting-names MyDbConnection
```

## <a name="publish-your-changes"></a>Değişikliklerinizi yayımlama

Bundan sonra tek yapmanız gereken, değişikliklerinizi Azure'da yayımlamaktır.

**Çözüm Gezgini**’nde **DotNetAppSqlDb** projenize sağ tıklayıp **Yayımla**’yı seçin.

![Çözüm Gezgini'nden yayımlama](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Yayımlama sayfasında **Yayımla**'ya tıklayın. Yeni web sayfası yapılacaklar listenizi gösterdiğinde uygulamanızın yönetilen kimliğini kullanarak veritabanına bağlanmakta olduğu anlamına gelir.

![Code First Migration'dan sonra Azure uygulaması](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Bundan sonra yapılacaklar listesini daha önce olduğu gibi düzenleyebilirsiniz.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Öğrendikleriniz:

> [!div class="checklist"]
> * Yönetilen kimlikleri etkinleştirme
> * Yönetilen kimliğe SQL Veritabanı erişimi verme
> * Entity Framework, SQL veritabanı ile Azure AD kimlik doğrulamasını kullanmak için yapılandırma
> * Visual Studio'dan Azure AD kimlik doğrulamasını kullanarak SQL veritabanı'na bağlanma

Web uygulamanıza özel bir DNS adı eşlemeyle ilgili bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure App Service'e eşlemek](app-service-web-tutorial-custom-domain.md)
