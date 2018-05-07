---
title: ActiveDirectoryInteractive bağlayan SQL | Microsoft Docs
description: C# kod örneği, Azure SQL veritabanına SqlAuthenticationMethod.ActiveDirectoryInteractive modunu kullanarak bağlanmak için açıklamalar ile.
services: sql-database
author: GithubMirek
manager: jhubbard
ms.service: sql-database
ms.custom: active directory
ms.topic: article
ms.date: 04/06/2018
ms.author: MirekS
ms.reviewer: GeneMi
ms.openlocfilehash: 163b55e0f1727cc2ba9e3ee84eb58fe29f7a005d
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="use-activedirectoryinteractive-mode-to-connect-to-azure-sql-database"></a>Azure SQL veritabanına bağlanmak için ActiveDirectoryInteractive modu kullan

Bu makalede, Microsoft Azure SQL veritabanına bağlanan runnable C# kod örneği sağlar. C# programı etkileşimli mod Azure AD çok faktörlü kimlik doğrulamasını (MFA) destekler kimlik doğrulaması kullanır. Örneğin, bağlantı girişimi cep telefonunuza gönderilen bir doğrulama kodu içerebilir.

SQL araçları için MFA desteği hakkında daha fazla bilgi için bkz: [Azure Active Directory desteği SQL Server veri Araçları (SSDT)](https://docs.microsoft.com/sql/ssdt/azure-active-directory).




## <a name="sqlauthenticationmethod-activedirectoryinteractive-enum-value"></a>SqlAuthenticationMethod. ActiveDirectoryInteractive enum değeri

.NET Framework sürüm 4.7.2, enum başlangıç [ **SqlAuthenticationMethod** ](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlauthenticationmethod) yeni bir değere sahip **. ActiveDirectoryInteractive**. İstemci C# programı tarafından kullanıldığında, bu enum değeri, Azure SQL veritabanında kimlik doğrulaması için MFA destekleme Azure AD etkileşimli mod kullanın yönlendirir. Program sonra çalıştıran kullanıcının aşağıdaki iletişim kutuları görür:

1. Bir Azure AD kullanıcı adını görüntüler ve Azure AD kullanıcısının parolası soran bir iletişim kutusu.
    - Parolasız gerekirse bu iletişim kutusunu görüntülenmez. Kullanıcının etki alanı Azure AD ile birleştirildiyse parola gereklidir.

    Azure AD içinde ayarlanan ilkenin kullanıcıya MFA sınırlamasıdır aşağıdaki iletişim kutuları sonraki görüntülenir.

2. Yalnızca MFA senaryo kullanıcı deneyimleri çok ilk kez sistem ek bir iletişim kutusu görüntüler. İletişim kutusu metin iletileri gönderilecek bir cep telefonu numarasını sorar. Her ileti sağlar *doğrulama kodu* , kullanıcının bir sonraki iletişim kutusuna girmeniz gerekir.

3. Sistem cep telefonuna gönderdi MFA doğrulama kodu ister başka bir iletişim kutusu.

Azure AD MFA gerektirecek şekilde yapılandırma hakkında daha fazla bilgi için bkz: [bulutta Azure multi-Factor Authentication kullanmaya Başlarken](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-cloud).

Bu iletişim kutularının ekran görüntüleri için bkz: [SQL Server Management Studio ve Azure AD için çok faktörlü kimlik doğrulamasını yapılandırın](sql-database-ssms-mfa-authentication-configure.md).

> [!TIP]
> Genel arama sayfamızı .NET Framework API'leri her tür için şu bağlantıya bizim kullanışlı mevcuttur **.NET API tarayıcı** aracı:
>
> [https://docs.microsoft.com/dotnet/api/](https://docs.microsoft.com/dotnet/api/)
>
> Tür adı isteğe bağlı eklenmiş ekleyerek **? terim =** parametresi, arama sayfası bize bizim sonuç hazır ve bekliyor olabilir:
>
> [https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod](https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod)


## <a name="preparations-for-c-by-using-the-azure-portal"></a>Azure portalını kullanarak C# için hazırlık

Sahip olduğunuz varsayılmıştır bir [oluşturulan Azure SQL veritabanı sunucusu](sql-database-get-started-portal.md) ve kullanılabilir.


### <a name="a-create-an-app-registration"></a>A. Bir uygulama kaydı oluşturun

Azure AD kimlik doğrulaması kullanmak için C# istemci programınızı bir GUID olarak sağlamalısınız bir *ClientID* zaman programınızı bağlanmayı dener. Bir uygulama kaydı tamamlama oluşturur ve Azure Portal'da GUID görüntülüyor olarak etiketli **uygulama kimliği**. Gezinti adımlar aşağıdaki gibidir:

1. Azure portal &gt; **Azure Active Directory** &gt; **uygulama kaydı**

    ![Uygulama kaydı](media\active-directory-interactive-connect-azure-sql-db\sshot-create-app-registration-b20.png)

2. **Uygulama kimliği** değer oluşturulur ve görüntülenir.

    ![Görüntülenen uygulama kimliği](media\active-directory-interactive-connect-azure-sql-db\sshot-application-id-app-regis-mk49.png)

3. **Kayıtlı uygulama** &gt; **ayarları** &gt; **gerekli izinleri** &gt; **Ekle**

    ![Kayıtlı uygulama için izin ayarları](media\active-directory-interactive-connect-azure-sql-db\sshot-registered-app-settings-required-permissions-add-api-access-c32.png)

4. **Gerekli izinleri** &gt; **API Ekle erişim** &gt; **bir API seçin** &gt; **Azure SQL veritabanı**

    ![Azure SQL veritabanı için API'sine erişim Ekle](media\active-directory-interactive-connect-azure-sql-db\sshot-registered-app-settings-required-permissions-add-api-access-Azure-sql-db-d11.png)

5. **API erişimini** &gt; **seçin izinleri** &gt; **izinleri için temsilci**

    ![Azure SQL veritabanı için temsilci izinleri API](media\active-directory-interactive-connect-azure-sql-db\sshot-add-api-access-azure-sql-db-delegated-permissions-checkbox-e14.png)


### <a name="b-set-azure-ad-admin-on-your-sql-database-server"></a>B. SQL veritabanı sunucunuz üzerinde Azure AD yönetim ayarlayın

Her Azure SQL veritabanı sunucusu kendi SQL mantıksal sunucusuna Azure ad var. C# senaryomuz için Azure SQL server için Azure AD yönetici ayarlamanız gerekir.

1. **SQL Server** &gt; **Active Directory Yöneticisi** &gt; **yönetici ayarlama**

    - Ekran görüntülerinde Azure SQL veritabanı için Azure AD Yöneticiler ve kullanıcılar hakkında daha fazla bilgi için bkz [yapılandırma ve SQL veritabanı ile Azure Active Directory kimlik doğrulamasını yönetmek](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server), onun bölümündeki **Azure sağlama Azure SQL veritabanı sunucunuz için Active Directory Yöneticisi**.


### <a name="c-prepare-an-azure-ad-user-to-connect-to-a-specific-database"></a>C. Belirli bir veritabanına bağlanmak için bir Azure AD kullanıcısının hazırlama

Azure SQL Database sunucunuza özgü Azure AD'de belirli bir veritabanı erişimi bir kullanıcı ekleyebilirsiniz.

Daha fazla bilgi için bkz: [Azure Active Directory kimlik doğrulamasını kullan SQL veritabanı, yönetilen örneği veya SQL Data Warehouse ile kimlik doğrulaması için](sql-database-aad-authentication.md).


### <a name="d-add-a-non-admin-user-to-azure-ad"></a>D. Azure AD ile yönetici olmayan bir kullanıcı ekleyin

Azure AD yönetim SQL veritabanı sunucusunun SQL veritabanı sunucusuna bağlanmak için kullanılabilir. Ancak, daha fazla genel yönetici olmayan bir kullanıcı için Azure AD eklemek için durumdur. Yönetici olmayan kullanıcı bağlanmak için kullanıldığında, Azure AD tarafından uygulanan MFA sunucusundaki bu kullanıcı, MFA dizisi çağrılır.




## <a name="azure-active-directory-authentication-library-adal"></a>Azure Active Directory Authentication Library (ADAL)

C# programı ad dayanır **Microsoft.IdentityModel.Clients.activedirectory tarafından**. Bu ad alanı için aynı adı taşıyan derlemesindeki sınıflarıdır.

- ADAL derleme karşıdan yükleyip NuGet kullanın.
    - [https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)

- C# programının bir derleme desteklemek için derlemesine başvuru ekleyin.




## <a name="sqlauthenticationmethod-enum"></a>SqlAuthenticationMethod enum

C# örnek dayanan bir ad alanları **System.Data.SqlClient**. Enum özel ilginizi çekecektir **SqlAuthenticationMethod**. Bu numaralandırma aşağıdaki değerlere sahiptir:

- **SqlAuthenticationMethod.ActiveDirectory*etkileşimli ***:&nbsp; çok faktörlü kimlik doğrulaması MFA elde etmek için bir Azure AD kullanıcı adıyla bunu kullanın.
    - Mevcut makaleyi odağını değeridir. Bu, etkileşimli bir deneyim MFA sunucusundaki bu kullanıcı sınırlamasıdır, kullanıcı parolasını ve ardından MFA doğrulama için iletişim kutularını görüntüleme üretir.
    - Bu değer, .NET Framework sürüm 4.7.2 ile başlayarak kullanılabilir.

- **SqlAuthenticationMethod.ActiveDirectory*tümleşik ***:&nbsp; bu iş için bir *federe* hesabı. Birleştirilmiş bir hesap için kullanıcı adı için Windows etki alanı adı verilir. Bu yöntem, MFA desteklemez.

- **SqlAuthenticationMethod.ActiveDirectory*parola ***:&nbsp; bunu bir Azure AD kullanıcısının ve kullanıcının parola gerektiren kimlik doğrulaması için kullanın. Azure SQL veritabanı kimlik doğrulaması gerçekleştirir. Bu yöntem, MFA desteklemez.




## <a name="prepare-c-parameter-values-from-the-azure-portal"></a>C# parametre değerlerini Azure portalından hazırlama

C# programı başarıyla çalışması için aşağıdaki statik alanları uygun değerleri atamanız gerekir. Bu statik alanları programa parametreleri gibi davranır. Alanları pretend değerlerle burada gösterilir. Ayrıca gösterilen Azure portalın doğru değerlerin nereden edinebileceğiniz konumlarda bulunur:


| Statik alan adı | Değer içeriğini | Azure portalında yeri |
| :---------------- | :------------ | :-------------------- |
| Az_SQLDB_svrName | "my-sık kullanılan-sqldb-svr.database.windows.net" | **SQL Server** &gt; **ada göre filtrele** |
| AzureAD_UserID | "user9@abc.onmicrosoft.com" | **Azure Active Directory** &gt; **kullanıcı** &gt; **yeni Konuk kullanıcı** |
| Initial_DatabaseName | "ana" | **SQL Server** &gt; **SQL veritabanları** |
| ClientApplicationID | "a94f9c62-97fe-4d19-b06d-111111111111" | **Azure Active Directory** &gt; **uygulama kayıtlar**<br /> &nbsp; &nbsp; &gt; **Ada göre arama** &gt; **uygulama kimliği** |
| redirectUri | Yeni bir URI ("https://bing.com/") | **Azure Active Directory** &gt; **uygulama kayıtlar**<br /> &nbsp; &nbsp; &gt; **Ada göre arama** &gt; *[Your-App-regis zaman]* &gt;<br /> &nbsp; &nbsp; **Ayarları** &gt; **RedirectURIs**<br /><br />Bu makale için geçerli bir değer, RedirectUri için uygundur. Değer örneğimizde gerçekten kullanılmaz. |
| &nbsp; | &nbsp; | &nbsp; |


Belirli senaryonuza bağlı olarak, önceki tabloda tüm parametrelerin değerlerini gerekmeyebilir.




## <a name="run-ssms-to-verify"></a>SSMS doğrulamak için Çalıştır

C# programı çalıştırmadan önce SQL Server Management Studio (SSMS) çalıştırmak yararlıdır. Çalıştırma SSMS çeşitli yapılandırmaları doğru olduğunu doğrular. Ardından C# programının herhangi bir hata yalnızca kendi kaynak koduna daraltır olabilir.


#### <a name="verify-sql-database-firewall-ip-addresses"></a>SQL veritabanı güvenlik duvarı IP adreslerini doğrulayın

C# programı daha sonra çalıştırılacak aynı bilgisayardan aynı binada SSMS çalıştırın. Hangisi kullanabilirsiniz **kimlik doğrulaması** düşündüğünüz modudur kolay. Veritabanı sunucusu Güvenlik Duvarı'nı IP adresiniz kabul edilmiyor herhangi bir gösterge ise, gösterildiği gibi düzeltebilirsiniz [Azure SQL veritabanı sunucusu ve veritabanı düzeyi güvenlik duvarı kuralları](sql-database-firewall-configure.md).


#### <a name="verify-multi-factor-authentication-mfa-for-azure-ad"></a>Çok faktörlü kimlik doğrulaması (MFA) Azure AD doğrulayın

SSMS yeniden çalıştırın, ancak bu defa ile **kimlik doğrulaması** kümesine **Active Directory - Evrensel MFA desteğiyle**. Bu seçenek için SSMS 17,5 veya sonraki bir sürümü olması gerekir.

Daha fazla bilgi için bkz: [SSMS ve Azure AD için çok faktörlü kimlik doğrulamasını yapılandırın](sql-database-ssms-mfa-authentication-configure.md).




## <a name="c-code-example"></a>C# kod örneği

Bu C# örnek derlemek için adlı DLL derlemesine başvuru ekleyin **Microsoft.IdentityModel.Clients.activedirectory tarafından**.


#### <a name="reference-documentation"></a>Başvuru belgeleri

- **System.Data.SqlClient** ad alanı:
    - Arama:&nbsp; [https://docs.microsoft.com/dotnet/api/?term=System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/?term=System.Data.SqlClient)
    - Doğrudan:&nbsp; [System.Data.Client](https://docs.microsoft.com/dotnet/api/system.data.sqlclient)

- **Microsoft.IdentityModel.Clients.activedirectory tarafından** ad alanı:
    - Arama:&nbsp; [https://docs.microsoft.com/dotnet/api/?term=Microsoft.IdentityModel.Clients.ActiveDirectory](https://docs.microsoft.com/dotnet/api/?term=Microsoft.IdentityModel.Clients.ActiveDirectory)
    - Doğrudan:&nbsp; [Microsoft.IdentityModel.Clients.activedirectory tarafından](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory)


#### <a name="c-source-code-in-two-parts"></a>C# kaynak kodu, iki parça

&nbsp;

```csharp

using System;    // C# ,  part 1 of 2.

// Add a reference to assembly:  Microsoft.IdentityModel.Clients.ActiveDirectory.DLL
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
        static public string Az_SQLDB_svrName = "<YOUR VALUE HERE>";
        static public string AzureAD_UserID = "<YOUR VALUE HERE>";
        static public string Initial_DatabaseName = "master";
        // Some scenarios do not need values for the following two fields:
        static public readonly string ClientApplicationID = "<YOUR VALUE HERE>";
        static public readonly Uri RedirectUri = new Uri("<YOUR VALUE HERE>");

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

```

&nbsp;

#### <a name="second-half-of-the-c-program"></a>C# programı ikinci yarısında

Daha iyi görsel görüntü için C# programı iki kod bloklara ayrılır. Programı çalıştırmak için iki kod blokları birlikte yapıştırın.

&nbsp;

```csharp

    // C# ,  part 2 of 2 ,  to concatenate below part 1.

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

&nbsp;


## <a name="next-steps"></a>Sonraki adımlar

- [Get-AzureRmSqlServerActiveDirectoryAdministrator](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlserveractivedirectoryadministrator)

