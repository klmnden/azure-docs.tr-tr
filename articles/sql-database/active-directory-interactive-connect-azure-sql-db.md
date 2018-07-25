---
title: ActiveDirectoryInteractive bağlayan SQL | Microsoft Docs
description: Açıklamalar, SqlAuthenticationMethod.ActiveDirectoryInteractive modunu kullanarak Azure SQL veritabanı'na bağlanma ile C# kod örneği.
services: sql-database
author: GithubMirek
manager: craigg
ms.service: sql-database
ms.custom: active directory
ms.topic: conceptual
ms.date: 04/06/2018
ms.author: MirekS
ms.reviewer: GeneMi
ms.openlocfilehash: 3d6eb70b3ce9072dc2c51220af89549022b5dacf
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39238277"
---
# <a name="use-activedirectoryinteractive-mode-to-connect-to-azure-sql-database"></a>Azure SQL veritabanı'na bağlanmak için ActiveDirectoryInteractive modunu kullan

Bu makalede, Microsoft Azure SQL veritabanına bağlanan bir çalıştırılabilir C# kod örneği sağlar. C# programı Azure AD'ye multi-Factor authentication (MFA) destekler kimlik doğrulaması, etkileşimli modunu kullanır. Örneğin, bir bağlantı denemesi cep telefonunuza gönderilen bir doğrulama kodu ekleyebilirsiniz.

SQL araçları için MFA desteği hakkında daha fazla bilgi için bkz. [Azure Active Directory desteği SQL Server veri Araçları (SSDT)](https://docs.microsoft.com/sql/ssdt/azure-active-directory).




## <a name="sqlauthenticationmethod-activedirectoryinteractive-enum-value"></a>SqlAuthenticationMethod. ActiveDirectoryInteractive sabit listesi değeri

.NET Framework sürüm 4.7.2, enum başlangıç [ **SqlAuthenticationMethod** ](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlauthenticationmethod) yeni bir değere sahip **. ActiveDirectoryInteractive**. İstemci C# programı tarafından kullanıldığında, bu sabit listesi değeri, Azure SQL veritabanında kimlik doğrulaması için mfa'yı destekleyen Azure AD'ye etkileşimli modu kullanın yönlendirir. Ardından, programı çalıştıran kullanıcının aşağıdaki iletişim kutuları görür:

1. Bir Azure AD kullanıcı adını görüntüleyen ve, Azure AD kullanıcısının parolası soran bir iletişim kutusu.
    - Parola gerekiyorsa, bu iletişim kutusu görüntülenmez. Kullanıcının etki alanını Azure AD ile birleştirildiyse parola gereklidir.

    MFA kullanıcı Azure AD'de ayarladığı ilke tarafından uygulanan, aşağıdaki iletişim kutularının yanındaki görüntülenir.

2. Yalnızca kullanıcı MFA senaryo düştüğünü ilk kez sistem ek bir iletişim kutusu görüntüler. İletişim kutusunun metin iletileri gönderilecek bir cep telefonu numarası için sorar. Her ileti sağlar *doğrulama kodu* , kullanıcının sonraki iletişim kutusuna girmeniz gerekir.

3. Sistem cep telefonuna gönderilen MFA doğrulama kodu ister başka bir iletişim kutusu.

Azure AD MFA gerektirecek şekilde yapılandırma hakkında daha fazla bilgi için bkz: [bulutta Azure multi Factor Authentication kullanmaya başlama](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-cloud).

Bu iletişim kutularının ekran görüntüleri için bkz. [SQL Server Management Studio ve Azure AD için çok faktörlü kimlik doğrulamasını yapılandırma](sql-database-ssms-mfa-authentication-configure.md).

> [!TIP]
> .NET Framework API'ları tüm türleri için genel arama sayfamıza kullanışlı bizim için şu bağlantıdaki kullanılabilir **.NET API Browser** aracı:
>
> [https://docs.microsoft.com/dotnet/api/](https://docs.microsoft.com/dotnet/api/)
>
> Tür adı isteğe bağlı eklenmiş ekleyerek **? terimi =** parametresi, arama sayfası bizim için hazırdır bizim sonucu vardır:
>
> [https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod](https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod)


## <a name="preparations-for-c-by-using-the-azure-portal"></a>Azure portalını kullanarak C# ' ta, hazırlıkları

Sahip olduğunuz varsayılır bir [Azure SQL veritabanı sunucusu oluşturan](sql-database-get-started-portal.md) ve kullanılabilir.


### <a name="a-create-an-app-registration"></a>A. Bir uygulama kaydı oluşturma

Azure AD kimlik doğrulamasını kullanmak için C# istemci programınız bir GUID olarak sağlamalısınız bir *ClientID* çalıştığında programınız bağlanın. Bir uygulama kaydı tamamlama oluşturur ve Azure portalında GUİD'ini görüntüler olarak etiketlenmiş **uygulama kimliği**. Gezinti adımlar aşağıdaki gibidir:

1. Azure portalında &gt; **Azure Active Directory** &gt; **uygulama kaydı**

    ![Uygulama kaydı](media\active-directory-interactive-connect-azure-sql-db\sshot-create-app-registration-b20.png)

2. **Uygulama kimliği** değer oluşturulur ve görüntülenir.

    ![Görüntülenen uygulama kimliği](media\active-directory-interactive-connect-azure-sql-db\sshot-application-id-app-regis-mk49.png)

3. **Kayıtlı uygulama** &gt; **ayarları** &gt; **gerekli izinler** &gt; **Ekle**

    ![Kayıtlı uygulama için izin ayarları](media\active-directory-interactive-connect-azure-sql-db\sshot-registered-app-settings-required-permissions-add-api-access-c32.png)

4. **Gerekli izinler** &gt; **API erişimi Ekle** &gt; **bir API seçin** &gt; **Azure SQL veritabanı**

    ![Azure SQL veritabanı için erişim API'ye işlem ekleme](media\active-directory-interactive-connect-azure-sql-db\sshot-registered-app-settings-required-permissions-add-api-access-Azure-sql-db-d11.png)

5. **API erişimi** &gt; **izinleri seçin** &gt; **temsilci izinleri**

    ![Azure SQL veritabanı için API'sine temsilci izinleri](media\active-directory-interactive-connect-azure-sql-db\sshot-add-api-access-azure-sql-db-delegated-permissions-checkbox-e14.png)


### <a name="b-set-azure-ad-admin-on-your-sql-database-server"></a>B. Azure AD Yöneticisi, SQL veritabanı sunucusuna Ayarla

Her Azure SQL veritabanı sunucusu, Azure AD'ye kendi SQL mantıksal sunucusu vardır. C# senaryomuz için Azure SQL Sunucunuz için bir Azure AD Yöneticisi ayarlamanız gerekir.

1. **SQL Server** &gt; **Active Directory Yöneticisi** &gt; **yönetici Ayarla**

    - Ekran görüntülerinde de Azure SQL veritabanı için Azure AD yöneticileri ve kullanıcılar hakkında daha fazla bilgi için bkz. [yapılandırma ve SQL veritabanı ile Azure Active Directory kimlik doğrulamasını yönetmek](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server), alt bölümdeki **Azure sağlama Azure SQL veritabanı sunucunuzun Active Directory Yöneticisi**.


### <a name="c-prepare-an-azure-ad-user-to-connect-to-a-specific-database"></a>C. Belirli bir veritabanına bağlanmak için bir Azure AD Kullanıcı hazırlama

Azure SQL veritabanı sunucunuza özgü Azure AD'de belirli bir veritabanına erişimi olması bir kullanıcı ekleyebilirsiniz.

Daha fazla bilgi için [kullanımı Azure SQL veritabanı, yönetilen örneği veya SQL veri ambarı ile kimlik doğrulaması için Active Directory kimlik](sql-database-aad-authentication.md).


### <a name="d-add-a-non-admin-user-to-azure-ad"></a>D. Yönetici olmayan bir kullanıcı Azure AD'ye ekleme

SQL veritabanı sunucusu, Azure AD Yöneticisi, SQL veritabanı sunucunuza bağlanmak için kullanılabilir. Ancak, Azure AD'ye yönetici olmayan bir kullanıcı eklemek için daha genel durumu gösterir. Yönetici olmayan kullanıcı bağlanmak için kullanıldığında, Azure AD tarafından uygulanan MFA bu kullanıcı, MFA dizisi çağrılır.




## <a name="azure-active-directory-authentication-library-adal"></a>Azure Active Directory kimlik doğrulama kitaplığı (ADAL)

Ad alanı üzerinde C# programı kullanır **Microsoft.IdentityModel.Clients.activedirectory**. Bu ad alanı için aynı adı taşıyan derlemesine sınıflardır.

- NuGet, derleme ADAL'ı indirip kullanın.
    - [https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)

- C# programının bir derleme desteklemek için derlemesine bir başvuru ekleyin.




## <a name="sqlauthenticationmethod-enum"></a>SqlAuthenticationMethod sabit listesi

C# örneği dayanan bir ad alanları olan **System.Data.SqlClient**. Sabit özel ilgi çekecektir **SqlAuthenticationMethod**. Bu numaralandırma aşağıdaki değerlere sahip:

- **SqlAuthenticationMethod.ActiveDirectory*etkileşimli ***:&nbsp; multi factor authentication MFA elde etmek için bir Azure AD kullanıcı adı ile bunu kullanın.
    - Bu değer mevcut makalenin odak noktası olur. Bu, etkileşimli bir deneyim MFA kullanıcı bu tutabildiğini, kullanıcı parolasını ve ardından MFA doğrulama için iletişim kutularını görüntüleme üretir.
    - Bu değer, .NET Framework sürümü 4.7.2 ile başlayarak kullanılabilir.

- **SqlAuthenticationMethod.ActiveDirectory*tümleşik ***:&nbsp; bu iş için bir *Federasyon* hesabı. Birleştirilmiş bir hesap için kullanıcı adı, Windows etki alanına adı verilir. Bu yöntem, mfa'yı desteklemez.

- **SqlAuthenticationMethod.ActiveDirectory*parola ***:&nbsp; bir Azure AD kullanıcısı ve kullanıcının parolasını gerektiren kimlik doğrulaması için bunu kullanın. Azure SQL veritabanı kimlik doğrulaması gerçekleştirir. Bu yöntem, mfa'yı desteklemez.




## <a name="prepare-c-parameter-values-from-the-azure-portal"></a>C# parametre değerlerini Azure portalından hazırlama

C# programı başarılı çalıştırma için aşağıdaki statik alanlar için uygun değerleri atamanız gerekir. Bu statik alanlar programa parametreleri gibi davranır. Alanları pretend değerlerle burada gösterilir. Ayrıca gösterilen, uygun değerlerin nereden edinebileceğiniz Azure portaldan konumlarda bulunur:


| Statik alan adı | Değer anlatabilirsiniz | Azure portalında nerede |
| :---------------- | :------------ | :-------------------- |
| Az_SQLDB_svrName | "my-sık kullanılan-sqldb-svr.database.windows.net" | **SQL sunucuları** &gt; **ada göre filtrele** |
| AzureAD_UserID | "user9@abc.onmicrosoft.com" | **Azure Active Directory** &gt; **kullanıcı** &gt; **yeni Konuk kullanıcı** |
| Initial_DatabaseName | "ana" | **SQL sunucuları** &gt; **SQL veritabanları** |
| ClientApplicationID | "a94f9c62-97fe-4d19-b06d-111111111111" | **Azure Active Directory** &gt; **uygulama kayıtları**<br /> &nbsp; &nbsp; &gt; **Ada göre Ara** &gt; **uygulama kimliği** |
| RedirectUri | Yeni bir Urı'ya ("https://bing.com/") | **Azure Active Directory** &gt; **uygulama kayıtları**<br /> &nbsp; &nbsp; &gt; **Ada göre Ara** &gt; *[Your-App-regis zaman]* &gt;<br /> &nbsp; &nbsp; **Ayarları** &gt; **RedirectURIs**<br /><br />Bu makale için geçerli bir değer, RedirectUri için uygundur. Değer örneğimizde gerçekten kullanılmaz. |
| &nbsp; | &nbsp; | &nbsp; |


Belirli senaryonuza bağlı olarak, önceki tabloda tüm parametre değerlerini gerekmeyebilir.




## <a name="run-ssms-to-verify"></a>Doğrulamak için SSMS çalıştırın

C# programı çalıştırmadan önce SQL Server Management Studio (SSMS) çalıştırmak yararlıdır. Çalıştırma SSMS, çeşitli yapılandırmalarda doğru olduğunu doğrular. Ardından C# programının herhangi bir hata, yalnızca kaynak koduna daraltır olabilir.


#### <a name="verify-sql-database-firewall-ip-addresses"></a>SQL veritabanı güvenlik duvarı IP adresleri doğrulayın

Aynı bilgisayara aynı binada, C# programı daha sonra çalışacağını SSMS çalıştırın. Hangisi kullanabileceğiniz **kimlik doğrulaması** düşündüğünüz modu, kolay. Veritabanı sunucusu güvenlik duvarı IP adresiniz kabul etmeyen herhangi bir göstergesi varsa, size, gösterildiği gibi düzeltebilirsiniz [Azure SQL veritabanı sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları](sql-database-firewall-configure.md).


#### <a name="verify-multi-factor-authentication-mfa-for-azure-ad"></a>Azure AD için multi-Factor authentication (MFA) doğrulayın.

SSMS yeniden çalıştırmak bu sefer ile **kimlik doğrulaması** kümesine **Active Directory - MFA desteğiyle Evrensel**. Bu seçenek için SSMS 17,5 veya sonraki bir sürümü olmalıdır.

Daha fazla bilgi için [SSMS ve Azure AD için çok faktörlü kimlik doğrulamasını yapılandırma](sql-database-ssms-mfa-authentication-configure.md).




## <a name="c-code-example"></a>C# kod örneği

Bu C# örneği derlemek için adlı DLL derlemesine bir başvuru eklemelisiniz **Microsoft.IdentityModel.Clients.activedirectory**.


#### <a name="reference-documentation"></a>Başvuru belgeleri

- **System.Data.SqlClient** ad alanı:
    - Arama:&nbsp; [https://docs.microsoft.com/dotnet/api/?term=System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/?term=System.Data.SqlClient)
    - Doğrudan:&nbsp; [System.Data.Client](https://docs.microsoft.com/dotnet/api/system.data.sqlclient)

- **Microsoft.IdentityModel.Clients.activedirectory** ad alanı:
    - Arama:&nbsp; [https://docs.microsoft.com/dotnet/api/?term=Microsoft.IdentityModel.Clients.ActiveDirectory](https://docs.microsoft.com/dotnet/api/?term=Microsoft.IdentityModel.Clients.ActiveDirectory)
    - Doğrudan:&nbsp; [Microsoft.IdentityModel.Clients.activedirectory](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory)


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

Görsel görüntüleme için daha iyi, C# programı iki kod blokları şeklinde ayrılır. Programı çalıştırmak için iki kod blokları birlikte yapıştırın.

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

