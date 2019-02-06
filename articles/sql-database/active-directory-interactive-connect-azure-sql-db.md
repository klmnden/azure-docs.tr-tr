---
title: ActiveDirectoryInteractive bağlayan SQL | Microsoft Docs
description: Açıklamalar, SqlAuthenticationMethod.ActiveDirectoryInteractive modunu kullanarak Azure SQL veritabanı'na bağlanma ile C# kod örneği.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: active directory
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: MirekS
ms.reviewer: GeneMi
ms.date: 01/25/2019
manager: craigg
ms.openlocfilehash: def50aecbcf9186af9d0b9c781c3141ad2dcee59
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55753682"
---
# <a name="connect-to-azure-sql-database-with-active-directory-mfa"></a>Active Directory MFA ile Azure SQL Database'e bağlanma

Bu makalede sağlayan bir C# program olan Microsoft Azure SQL veritabanınıza bağlanır. Program destekleyen etkileşimli mod kimlik doğrulaması kullanan [Azure Active Directory (AD) çok faktörlü kimlik doğrulaması (MFA)](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks).

SQL araçları için MFA desteği hakkında daha fazla bilgi için bkz. [Azure Active Directory desteği SQL Server veri Araçları (SSDT)](https://docs.microsoft.com/sql/ssdt/azure-active-directory).

## <a name="multi-factor-authentication-for-azure-sql-database"></a>Azure SQL veritabanı için çok faktörlü kimlik doğrulaması

.NET Framework sürüm 4.7.2, enum başlangıç [ `SqlAuthenticationMethod` ](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlauthenticationmethod) - yeni bir değere sahip `ActiveDirectoryInteractive`. Bir istemci C# programı, sabit listesi değeri yönlendirir sistemin bir Azure SQL veritabanı'na bağlanmak için mfa'yı destekleyen Azure AD'ye etkileşimli modu kullanın. Programı çalıştıran kullanıcının aşağıdaki iletişim kutusu görür:

1. Bir Azure AD kullanıcı adını görüntüler ve kullanıcının parolası soran bir iletişim kutusu.

   Kullanıcının etki alanını Azure AD ile birleştirildiyse parola gerektiğinde bu iletişim kutusunda görünmez.

   Ardından Azure AD İlkesi, kullanıcı MFA uygular, sonraki iki iletişim kutusu görüntülenir.

2. İlk kez bir kullanıcı mfa'yı giden sistem metin ileti göndermek bir cep telefonu numarası için soran bir iletişim kutusu görüntüler. Her ileti sağlar *doğrulama kodu* , kullanıcının sonraki iletişim kutusunda girmeniz gerekir.

3. Sistem cep telefonuna gönderilen bir MFA doğrulama kodu için soran bir iletişim kutusu.

Azure AD MFA gerektirecek şekilde yapılandırma hakkında daha fazla bilgi için bkz: [bulutta Azure multi Factor Authentication kullanmaya başlama](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-cloud).

Bu iletişim kutularının ekran görüntüleri için bkz. [SQL Server Management Studio ve Azure AD için çok faktörlü kimlik doğrulamasını yapılandırma](sql-database-ssms-mfa-authentication-configure.md).

> [!TIP]
> .NET Framework API'ları ile arayabilirsiniz bizim **.NET API Browser** aracı sayfası:
>
> [https://docs.microsoft.com/dotnet/api/](https://docs.microsoft.com/dotnet/api/)
>
> Ayrıca doğrudan isteğe bağlı arama yapabilirsiniz **? terimi =&lt;arama değeri&gt;**  parametresi:
>
> [https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod](https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod)

## <a name="configure-your-c-application-in-the-azure-portal"></a>Yapılandırma, C# Azure portalında uygulama

Başlamadan önce olmalıdır bir [Azure SQL veritabanı sunucusu](sql-database-get-started-portal.md) oluşturulan ve kullanılabilir.

### <a name="register-your-app-and-set-permissions"></a>Uygulamanızı kaydetmenizi ve izinleri ayarlama


Azure AD kimlik doğrulamasını kullanmak için C# programı olan bir AD uygulaması kaydedilecek. Bir uygulamayı kaydetme için bir AD yöneticisi olmanız gerekir veya bir kullanıcı AD atanan *uygulama geliştiricisi* rol. Rol atama hakkında daha fazla bilgi için bkz. [yönetici ve yönetici olmayan rollerin Azure Active Directory ile kullanıcılara atama.](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal)

 Bir uygulama kaydı tamamlama oluşturur ve görüntüler bir **uygulama kimliği**. Bağlanmak için bu kimliği eklemek, programınızı sahiptir.


Kaydolun ve uygulamanız için gerekli izinleri ayarlamak için:

1. Azure portalı > **Azure Active Directory** > **uygulama kayıtları** > **yeni uygulama kaydı**

    ![Uygulama kaydı](media/active-directory-interactive-connect-azure-sql-db/image1.png)

    Uygulama kaydı oluşturulduktan sonra **uygulama kimliği** değer oluşturulur ve görüntülenir.

    ![Görüntülenen uygulama kimliği](media/active-directory-interactive-connect-azure-sql-db/image2.png)

2. **Kayıtlı uygulama** > **ayarları** > **gerekli izinler** > **Ekle**

    ![Kayıtlı uygulama için izin ayarları](media/active-directory-interactive-connect-azure-sql-db/sshot-registered-app-settings-required-permissions-add-api-access-c32.png)

3. **Gerekli izinler** > **Ekle** > **bir API seçin** > **Azure SQL veritabanı**

    ![Azure SQL veritabanı için erişim API'ye işlem ekleme](media/active-directory-interactive-connect-azure-sql-db/sshot-registered-app-settings-required-permissions-add-api-access-Azure-sql-db-d11.png)

4. **API erişimi** > **izinleri seçin** > **temsilci izinleri**

    ![Azure SQL veritabanı için API'sine temsilci izinleri](media/active-directory-interactive-connect-azure-sql-db/sshot-add-api-access-azure-sql-db-delegated-permissions-checkbox-e14.png)

### <a name="set-an-azure-ad-admin-for-your-sql-database-server"></a>SQL veritabanı sunucunuz için bir Azure AD Yöneticisi ayarlama


İçin C# çalışması için bir Azure SQL Sunucu Yöneticisi, Azure SQL Sunucunuz için bir Azure AD yönetici ataması gerekiyor. 

 * **SQL Server** > **Active Directory Yöneticisi** > **yönetici Ayarla**

Ekran görüntülerinde de Azure SQL veritabanı için Azure AD yöneticileri ve kullanıcılar hakkında daha fazla bilgi için bkz. [yapılandırma ve SQL veritabanı ile Azure Active Directory kimlik doğrulamasını yönetmek](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server), alt bölümdeki **Azure sağlama Azure SQL veritabanı sunucunuzun Active Directory Yöneticisi**.

### <a name="add-a-non-admin-user-to-a-specific-database-optional"></a>Belirli bir veritabanı (isteğe bağlı) yönetici olmayan kullanıcı ekleme

SQL veritabanı sunucusu için bir Azure AD Yöneticisi çalıştırabilirsiniz C# örnek program. Veritabanında olmaları durumunda bir Azure AD kullanıcısının program çalışabilir. Azure AD SQL yönetici veya veritabanında zaten varsa ve bir Azure AD kullanıcı `ALTER ANY USER` izni veritabanında bir kullanıcı ekleyebilirsiniz.

Bir kullanıcı SQL veritabanıyla ekleyebilir [ `Create User` ](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql?view=sql-server-2017) komutu. Örneğin, `CREATE USER [<username>] FROM EXTERNAL PROVIDER`.

Daha fazla bilgi için [kullanımı Azure SQL veritabanı, yönetilen örneği veya SQL veri ambarı ile kimlik doğrulaması için Active Directory kimlik](sql-database-aad-authentication.md).

## <a name="new-authentication-enum-value"></a>Yeni kimlik doğrulama sabit listesi değeri

C# Örnek dayanır [ `System.Data.SqlClient` ](https://docs.microsoft.com/dotnet/api/system.data.sqlclient) ad alanı. Çok faktörlü kimlik doğrulaması için özel ilgi sabit olan `SqlAuthenticationMethod`, aşağıdaki değerleri vardır:

- `SqlAuthenticationMethod.ActiveDirectoryInteractive`

   Bu değer, MFA uygulamak için bir Azure AD kullanıcı adını kullanın. Bu değer mevcut makalenin odak noktası olur. Bu, etkileşimli bir deneyim MFA kullanıcı bu tutabildiğini, kullanıcı parolasını ve ardından MFA doğrulama için iletişim kutularını görüntüleme üretir. Bu değer, .NET Framework sürümü 4.7.2 ile başlayarak kullanılabilir.

- `SqlAuthenticationMethod.ActiveDirectoryIntegrated`

  Bu değer bir *Federasyon* hesabı. Birleştirilmiş bir hesap için kullanıcı adı, Windows etki alanına adı verilir. Bu kimlik doğrulama yöntemi, mfa'yı desteklemez.

- `SqlAuthenticationMethod.ActiveDirectoryPassword`

  Bir Azure AD kullanıcı adı ve parola gerektiren kimlik doğrulaması için bu değeri kullanın. Azure SQL veritabanı kimlik doğrulaması yapar. Bu yöntem, mfa'yı desteklemez.

## <a name="set-c-parameter-values-from-the-azure-portal"></a>Ayarlama C# Azure portalından parametre değerleri

İçin C# başarıyla çalışması için statik alanlar için uygun değerleri atamanız gerekir. Burada gösterilen örnek değerlerle alanlardır. Ayrıca gösterilen gerekli değerlerin nereden edinebileceğiniz Azure portal konumları şunlardır:

| Statik alan adı | Örnek değer | Azure portalında nerede |
| :---------------- | :------------ | :-------------------- |
| Az_SQLDB_svrName | "my-sqldb-svr.database.windows.net" | **SQL sunucuları** > **ada göre filtrele** |
| AzureAD_UserID | "auser@abc.onmicrosoft.com" | **Azure Active Directory** > **kullanıcı** > **yeni Konuk kullanıcı** |
| Initial_DatabaseName | "Veritabanım" | **SQL sunucuları** > **SQL veritabanları** |
| ClientApplicationID | "a94f9c62-97fe-4d19-b06d-111111111111" | **Azure Active Directory** > **uygulama kayıtları** > **adına göre Ara** > **uygulama kimliği** |
| RedirectUri | Yeni bir Urı'ya ("https://mywebserver.com/") | **Azure Active Directory** > **uygulama kayıtları** > **adına göre Ara** > *[Your-App-regis zaman]*  >  **Ayarları** > **RedirectURIs**<br /><br />Bu makale için geçerli bir değer burada kullanılan değil, RedirectUri için uygundur. |
| &nbsp; | &nbsp; | &nbsp; |

## <a name="verify-with-sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS) ile doğrulama

Çalıştırmadan önce C# programı, Kurulum ve yapılandırmaları SSMS'de doğru olduğunu kontrol etmek için iyi bir fikir olduğunu. Tüm C# program hatalarına ardından daraltıldığı kaynak kodu.

### <a name="verify-sql-database-firewall-ip-addresses"></a>SQL veritabanı güvenlik duvarı IP adresleri doğrulayın

Aynı bilgisayardan çalıştırın planladığınız aynı binada, SSMS çalıştıran C# program. Bu test için tüm **kimlik doğrulaması** moddur Tamam. Veritabanı sunucusu Güvenlik Duvarı'nın IP adresiniz kabul etmiyor herhangi bir göstergesi var olup olmadığını görmek [Azure SQL veritabanı sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları](sql-database-firewall-configure.md) Yardım.

### <a name="verify-azure-active-directory-mfa"></a>Azure Active Directory MFA doğrulayın

SSMS yeniden çalıştırmak bu sefer ile **kimlik doğrulaması** kümesine **Active Directory - MFA desteğiyle Evrensel**. Bu seçenek, SSMS 17,5 veya sonraki bir sürümü gerektirir.

Daha fazla bilgi için [SSMS ve Azure AD için çok faktörlü kimlik doğrulamasını yapılandırma](sql-database-ssms-mfa-authentication-configure.md).

> [!NOTE]
> Konuk kullanıcı veritabanında ise, de - veritabanı için AD etki alanı adı sağlamanız gerekir **seçenekleri** > **AD etki alanı adı veya Kiracı kimliği**. Etki alanı adı Azure Portalı'nda bulmak için seçin **Azure Active Directory** > **özel etki alanı adları**. İçinde C# örnek program, sağlayarak bir etki alanı adı gerekli değildir.

## <a name="c-code-example"></a>C# kod örneği

Örnek C# program dayanır [ *Microsoft.IdentityModel.Clients.activedirectory* ](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory) DLL derleme.

Visual Studio'da bu paket yüklemek için seçin **proje** > **NuGet paketlerini Yönet**. Arama ve yükleme **Microsoft.IdentityModel.Clients.activedirectory**.

### <a name="c-source-code"></a>C# kaynak kodu

```csharp

using System;

// Reference to AD authentication assembly
using Microsoft.IdentityModel.Clients.ActiveDirectory;

using DA = System.Data;
using SC = System.Data.SqlClient;
using AD = Microsoft.IdentityModel.Clients.ActiveDirectory;
using TX = System.Text;
using TT = System.Threading.Tasks;

namespace ADInteractive5
{
    class Program
    {
        // ASSIGN YOUR VALUES TO THESE STATIC FIELDS !!
        static public string Az_SQLDB_svrName = "<Your SQL DB server>";
        static public string AzureAD_UserID = "<Your User ID>";
        static public string Initial_DatabaseName = "<Your Database>";
        // Some scenarios do not need values for the following two fields:
        static public readonly string ClientApplicationID = "<Your App ID>";
        static public readonly Uri RedirectUri = new Uri("<Your URI>");

        public static void Main(string[] args)
        {
            var provider = new ActiveDirectoryAuthProvider();

            SC.SqlAuthenticationProvider.SetProvider(
                SC.SqlAuthenticationMethod.ActiveDirectoryInteractive,
                //SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated,  // Alternatives.
                //SC.SqlAuthenticationMethod.ActiveDirectoryPassword,
                provider);

            Program.Connection();
        }

        public static void Connection()
        {
            SC.SqlConnectionStringBuilder builder = new SC.SqlConnectionStringBuilder();

            // Program._  static values that you set earlier.
            builder["Data Source"] = Program.Az_SQLDB_svrName;
            builder.UserID = Program.AzureAD_UserID;
            builder["Initial Catalog"] = Program.Initial_DatabaseName;

            // This "Password" is not used with .ActiveDirectoryInteractive.
            //builder["Password"] = "<YOUR PASSWORD HERE>";

            builder["Connect Timeout"] = 15;
            builder["TrustServerCertificate"] = true;
            builder.Pooling = false;

            // Assigned enum value must match the enum given to .SetProvider().
            builder.Authentication = SC.SqlAuthenticationMethod.ActiveDirectoryInteractive;
            SC.SqlConnection sqlConnection = new SC.SqlConnection(builder.ConnectionString);

            SC.SqlCommand cmd = new SC.SqlCommand(
                "SELECT '******** MY QUERY RAN SUCCESSFULLY!! ********';",
                sqlConnection);

            try
            {
                sqlConnection.Open();
                if (sqlConnection.State == DA.ConnectionState.Open)
                {
                    var rdr = cmd.ExecuteReader();
                    var msg = new TX.StringBuilder();
                    while (rdr.Read())
                    {
                        msg.AppendLine(rdr.GetString(0));
                    }
                    Console.WriteLine(msg.ToString());
                    Console.WriteLine(":Success");
                }
                else
                {
                    Console.WriteLine(":Failed");
                }
                sqlConnection.Close();
            }
            catch (Exception ex)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("Connection failed with the following exception...");
                Console.WriteLine(ex.ToString());
                Console.ResetColor();
            }
        }
    } // EOClass Program .

    /// <summary>
    /// SqlAuthenticationProvider - Is a public class that defines 3 different Azure AD
    /// authentication methods.  The methods are supported in the new .NET 4.7.2 .
    ///  . 
    /// 1. Interactive,  2. Integrated,  3. Password
    ///  . 
    /// All 3 authentication methods are based on the Azure
    /// Active Directory Authentication Library (ADAL) managed library.
    /// </summary>
    public class ActiveDirectoryAuthProvider : SC.SqlAuthenticationProvider
    {
        // Program._ more static values that you set!
        private readonly string _clientId = Program.ClientApplicationID;
        private readonly Uri _redirectUri = Program.RedirectUri;

        public override async TT.Task<SC.SqlAuthenticationToken>
            AcquireTokenAsync(SC.SqlAuthenticationParameters parameters)
        {
            AD.AuthenticationContext authContext =
                new AD.AuthenticationContext(parameters.Authority);
            authContext.CorrelationId = parameters.ConnectionId;
            AD.AuthenticationResult result;

            switch (parameters.AuthenticationMethod)
            {
                case SC.SqlAuthenticationMethod.ActiveDirectoryInteractive:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,  // "https://database.windows.net/"
                        _clientId,
                        _redirectUri,
                        new AD.PlatformParameters(AD.PromptBehavior.Auto),
                        new AD.UserIdentifier(
                            parameters.UserId,
                            AD.UserIdentifierType.RequiredDisplayableId));
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_1 == '.ActiveDirectoryIntegrated'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserCredential());
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryPassword:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_2 == '.ActiveDirectoryPassword'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserPasswordCredential(
                            parameters.UserId,
                            parameters.Password));
                    break;

                default: throw new InvalidOperationException();
            }
            return new SC.SqlAuthenticationToken(result.AccessToken, result.ExpiresOn);
        }

        public override bool IsSupported(SC.SqlAuthenticationMethod authenticationMethod)
        {
            return authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryInteractive
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryPassword;
        }
    } // EOClass ActiveDirectoryAuthProvider .
} // EONamespace.  End of entire program source code.

```

&nbsp;

#### <a name="actual-test-output-from-c"></a>Gerçek test çıkışı C#

```
[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>> ADInteractive5.exe
In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.
******** MY QUERY RAN SUCCESSFULLY!! ********

:Success

[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>>
```

## <a name="next-steps"></a>Sonraki adımlar

- [SQL Server için Azure AD Yöneticisi hakkında bilgi edinin](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlserveractivedirectoryadministrator)

