---
title: 'Her zaman şifreli: SQL veritabanı - Azure anahtar kasası | Microsoft Docs'
description: Bu makalede SQL Server Management Studio'da her zaman şifreli sihirbazını kullanarak veri şifrelemesi ile SQL veritabanındaki hassas verilerin güvenliğinin nasıl sağlanacağını gösterir.
keywords: veri şifreleme, şifreleme anahtarı, bulut şifreleme
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: ''
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: bcda6ac723101d6a907a10c5163ae1baf0ad2214
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66168101"
---
# <a name="always-encrypted-protect-sensitive-data-and-store-encryption-keys-in-azure-key-vault"></a>Her zaman şifreli: Hassas verilerin korunmasına ve şifreleme anahtarları Azure Key Vault'ta depolama

Bu makalede veri şifreleme kullanarak SQL veritabanındaki hassas verilerin güvenliğini sağlamak gösterilmektedir [her zaman şifreli sihirbazını](https://msdn.microsoft.com/library/mt459280.aspx) içinde [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Ayrıca, her bir şifreleme anahtarı Azure Key Vault'ta depolamak nasıl gösterilecektir yönergeleri içerir.

Her zaman şifreli bir yeni veri şifreleme Azure SQL veritabanı ve sunucu üzerinde bekleyen hassas verileri istemci ve sunucu arasında ve veri kullanımdayken taşıma sırasında korunmasına yardımcı olan bir SQL Server teknolojisidir. Her zaman şifreli hassas verileri hiçbir zaman içinde bir veritabanı sistemi düz metin olarak görünmesini sağlar. Veri şifreleme yapılandırdıktan sonra yalnızca istemci uygulamaları veya anahtarlarının erişimi uygulama sunucuları düz metin verilere erişebilir. Ayrıntılı bilgi için bkz. [(veritabanı altyapısı)'Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx).

Always Encrypted kullanılacak veritabanını yapılandırdıktan sonra şifrelenmiş verilerle çalışmak için Visual Studio ile C# bir istemci uygulaması oluşturacaksınız.

Bu makaledeki adımları izleyin ve her zaman şifreli için bir Azure SQL veritabanı ayarlamayı öğrenin. Bu makalede, aşağıdaki görevlerin nasıl gerçekleştirileceğini öğreneceksiniz:

* Her zaman şifreli sihirbazını SSMS'de oluşturulacağı [Always Encrypted anahtarları](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
  * Oluşturma bir [sütun ana anahtarı (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
  * Oluşturma bir [sütun şifreleme anahtarı (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
* Bir veritabanı tablosu oluşturun ve sütun şifreleme.
* Ekler, seçer ve şifrelenmiş sütundaki verileri görüntüleyen bir uygulama oluşturun.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

Bu öğreticide, şunları yapmanız gerekir:

* Bir Azure hesabı ve aboneliği Yoksa, oturum açmak için bir [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) 13.0.700.242 sürümü veya üzeri.
* [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) veya üzeri (istemci bilgisayarda).
* [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* [Azure PowerShell](/powershell/azure/overview).

## <a name="enable-your-client-application-to-access-the-sql-database-service"></a>İstemci uygulamanızın SQL veritabanı hizmetine erişim
İstemci uygulamanızı Azure Active Directory (AAD) uygulama ayarını ve kopyalama SQL veritabanı hizmetine erişim etkinleştirmelisiniz *uygulama kimliği* ve *anahtarı* için ihtiyacınız olacak Uygulamanızın kimlik doğrulaması.

Alınacak *uygulama kimliği* ve *anahtarı*, adımları [bir Azure Active Directory kaynaklarına erişmek uygulama ve hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="create-a-key-vault-to-store-your-keys"></a>Anahtarlarınızı depolamak için key vault oluşturma
İstemci uygulamanızı yapılandırılır ve uygulamanızın uygulama Kimliğine sahip olduğunu, anahtar kasası oluşturma ve kasanın gizli anahtarları (her zaman şifreli anahtarları) hem de uygulamanızın erişebilmesi için kendi erişim ilkesini yapılandırmak için zaman var. *Oluşturma*, *alma*, *listesi*, *oturum*, *doğrulayın*, *wrapKey*, ve *unwrapKey* izinler yeni bir sütun ana anahtarı oluşturma ve SQL Server Management Studio ile şifreleme ayarlama gerekli değil.

Aşağıdaki betiği çalıştırarak, bir anahtar kasası hızlıca oluşturabilirsiniz. Bu cmdlet'ler ve oluşturma ve bir anahtar Kasası'nı yapılandırma hakkında daha fazla bilgi ayrıntılı bir açıklaması için bkz. [Azure anahtar kasası nedir?](../key-vault/key-vault-overview.md).

```powershell
    $subscriptionName = '<your Azure subscription name>'
    $userPrincipalName = '<username@domain.com>'
    $applicationId = '<application ID from your AAD application>'
    $resourceGroupName = '<resource group name>'
    # Use the same resource group name when creating your SQL Database below
    $location = '<datacenter location>'
    $vaultName = 'AeKeyVault'


    Connect-AzAccount
    $subscriptionId = (Get-AzSubscription -SubscriptionName $subscriptionName).Id
    Set-AzContext -SubscriptionId $subscriptionId

    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzKeyVault -VaultName $vaultName -ResourceGroupName $resourceGroupName -Location $location

    Set-AzKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
    Set-AzKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $applicationId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list
```



## <a name="create-a-blank-sql-database"></a>Boş bir SQL veritabanı oluşturma
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Git **kaynak Oluştur** > **veritabanları** > **SQL veritabanı**.
3. Oluşturma bir **boş** adlı veritabanı **Clinic** yeni veya var olan bir sunucu üzerinde. Azure portalında bir veritabanı oluşturma hakkında ayrıntılı yönergeler için bkz. [ilk Azure SQL veritabanınızı](sql-database-single-database-get-started.md).
   
    ![Boş veritabanı oluşturma](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

Bağlantı ihtiyacınız olacak dize öğreticinin ilerleyen bölümlerinde, bu nedenle veritabanı oluşturduktan sonra yeni Clinic veritabanına göz atın ve bağlantı dizesini kopyalayın. Herhangi bir zamanda bağlantı dizesini elde edebilirsiniz, ancak Azure portalında kopyalamak kolaydır.

1. Git **SQL veritabanları** > **Clinic** > **veritabanı bağlantı dizelerini Göster**.
2. İçin bağlantı dizesini kopyalayın **ADO.NET**.
   
    ![Bağlantı dizesini kopyalayın](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)

## <a name="connect-to-the-database-with-ssms"></a>SSMS ile veritabanına bağlanma
SSMS'yi açın ve Clinic veritabanı sunucusuna bağlanın.

1. SSMS’i açın. (Git **Connect** > **veritabanı altyapısı** açmak için **sunucuya Bağlan** penceresi açık değilse.)
2. Sunucu adı ve kimlik bilgilerini girin. Sunucu adı SQL veritabanı dikey penceresinde bulunabilir ve bağlantı dizesini, daha önce kopyaladığınız. Tam sunucu adını da dahil olmak üzere *database.windows.net*.
   
    ![Bağlantı dizesini kopyalayın](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

Varsa **yeni güvenlik duvarı kuralı** penceresi açılır; oturum açma ve Azure'a let SSMS sizin için yeni bir güvenlik duvarı kuralı oluşturun.

## <a name="create-a-table"></a>Bir tablo oluşturma
Bu bölümde, Hasta verileri tutmak için bir tablo oluşturacaksınız. Başlangıçta değil şifrelenmiş--sonraki bölümde şifreleme yapılandıracaksınız.

1. Genişletin **veritabanları**.
2. Sağ **Clinic** tıklayın ve veritabanı **yeni sorgu**.
3. Yeni sorgu penceresine aşağıdaki Transact-SQL (T-SQL) yapıştırın ve **yürütme** bu.

```sql
        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO
```

## <a name="encrypt-columns-configure-always-encrypted"></a>(Her zaman şifreli yapılandırma) sütun şifreleme
SSMS kolayca ayarlayarak sütun ana anahtarı, sütun şifreleme anahtarı ve şifrelenmiş sütunlar için Always Encrypted yapılandırmanıza yardımcı olacak bir sihirbaz sağlar.

1. Genişletin **veritabanları** > **Clinic** > **tabloları**.
2. Sağ **Hastalara** tablosunu seçip **şifrelemek sütunları** her zaman şifreli sihirbazını açmak için:
   
    ![Sütun şifreleme](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

Her zaman şifreli sihirbazını aşağıdaki bölümleri içerir: **Sütun seçimini**, **ana anahtarı yapılandırma**, **doğrulama**, ve **özeti**.

### <a name="column-selection"></a>Sütun Seçimi
Tıklayın **sonraki** üzerinde **giriş** sayfasını açmak için **sütun seçimi** sayfası. Bu sayfada, şifrelemek istediğiniz sütunları seçersiniz [şifreleme türünü ve hangi sütun şifreleme anahtarı (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) kullanılacak.

Şifreleme **SSN** ve **BirthDate** her hasta için bilgi. SSN sütun eşitlik aramalar, birleştirmeler ve Grupla destekler belirlenimci şifreleme kullanır. Doğum tarihi sütun işlemlerini desteklemeyen rastgele şifreleme kullanır.

Ayarlama **şifreleme türü** SSN sütunu için **Deterministic** ve doğum tarihi sütununu **Randomized**. **İleri**’ye tıklayın.

![Sütun şifreleme](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a>Ana anahtarını yapılandırma
**Ana anahtarı yapılandırma** sayfasıdır Burada, CMK ayarlayın ve anahtar depolama sağlayıcısı seçin CMK depolanacağı. Şu anda bir CMK Windows sertifika deposu, Azure anahtar kasası veya bir donanım güvenlik modülü (HSM) depolayabilirsiniz.

Bu öğreticide, anahtarlarınızı Azure anahtar Kasası'nda depolama işlemi gösterilmektedir.

1. Seçin **Azure anahtar kasası**.
2. İstenen anahtar kasası, aşağı açılan listeden seçin.
3. **İleri**’ye tıklayın.

![Ana anahtarını yapılandırma](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)

### <a name="validation"></a>Doğrulama
Sütunları şifrelemek veya daha sonra çalıştırmak için bir PowerShell komut dosyasını kaydedin. Bu öğreticide, seçin **artık tamamlanması devam** tıklatıp **sonraki**.

### <a name="summary"></a>Özet
Ayarların doğru olduğunu ve tıklayın doğrulayın **son** her zaman şifreli için kurulumu tamamlamak için.

![Özet](./media/sql-database-always-encrypted-azure-key-vault/summary.png)

### <a name="verify-the-wizards-actions"></a>Sihirbaz eylemleri doğrulayın
Sihirbaz tamamlandıktan sonra veritabanı her zaman şifreli için ayarlanmıştır. Sihirbaz aşağıdaki eylemleri:

* Sütun ana anahtarı oluşturulur ve Azure anahtar Kasası'nda depolanır.
* Sütun şifreleme anahtarı oluşturulur ve Azure anahtar Kasası'nda depolanır.
* Seçili sütunları şifreleme için yapılandırılmış. Hastalara tablo şu anda hiç veri yok, ancak seçili olan sütunlardaki var olan tüm verileri artık şifrelenir.

SSMS anahtarların oluşturulması genişleterek doğrulayabilirsiniz **Clinic** > **güvenlik** > **her zaman şifreli anahtarları**.

## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Şifrelenmiş veriler ile uygun bir istemci uygulaması oluşturma
Always Encrypted ayarlamak, gerçekleştiren bir uygulama oluşturabilirsiniz *ekler* ve *seçer* şifrelenmiş sütun.  

> [!IMPORTANT]
> Uygulamanızı kullanmalısınız [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) düz metin veri sütunları Always Encrypted ile sunucuya geçerken nesneleri. SqlParameter nesneleri kullanmadan değişmez değerler geçirerek bir özel durumu oluşur.
> 
> 

1. Visual Studio'yu açın ve yeni C# oluşturma **konsol uygulaması** (Visual Studio 2015 veya önceki) veya **konsol uygulaması (.NET Framework)** (Visual Studio 2017 ve üzeri). Projeniz emin olun kümesine **.NET Framework 4.6** veya üzeri.
2. Projeyi adlandırın **AlwaysEncryptedConsoleAKVApp** tıklatıp **Tamam**.
3. Giderek aşağıdaki NuGet paketlerini yükleme **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.

Bu iki kod satırlarını Paket Yöneticisi Konsolu'nda çalıştırın.

```powershell
    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```


## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Always Encrypted'ı etkinleştirmek için bağlantı dizesini değiştirme
Bu bölümde, veritabanı bağlantı dizenizi Always Encrypted'ı etkinleştirmek açıklanmaktadır.

Always Encrypted'ı etkinleştirmek için eklemeniz gerekir **sütun şifreleme ayarı** anahtar sözcüğü, bağlantı dizesi ve ayarlamak **etkin**.

Bu doğrudan bağlantı dizesinde ayarlayabilirsiniz veya kullanarak ayarlayabilirsiniz [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). Sonraki bölümde örnek uygulamada nasıl kullanılacağını gösterir **SqlConnectionStringBuilder**.

### <a name="enable-always-encrypted-in-the-connection-string"></a>Bağlantı dizesinde Always Encrypted'ı etkinleştir
Aşağıdaki anahtar sözcüğü, bağlantı dizenizi ekleyin.

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a>SqlConnectionStringBuilder ile her zaman şifreli etkinleştir
Aşağıdaki kod her zaman şifreli ayarlayarak etkinleştirme gösterir [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) için [etkin](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

```CS
    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;
```

## <a name="register-the-azure-key-vault-provider"></a>Azure Key Vault sağlayıcısını Kaydet
Aşağıdaki kod, ADO.NET sürücüsü ile Azure Key Vault sağlayıcısının kaydettirmek gösterilmektedir.

```C#
    private static ClientCredential _clientCredential;

    static void InitializeAzureKeyVaultProvider()
    {
       _clientCredential = new ClientCredential(applicationId, clientKey);

       SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
          new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

       Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
          new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

       providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
       SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
    }
```

## <a name="always-encrypted-sample-console-application"></a>Her zaman şifreli örnek konsol uygulaması
Bu örnek gösterir nasıl yapılır:

* Always Encrypted'ı etkinleştirmek için bağlantı dizesini değiştirin.
* Azure Key Vault, uygulamanın anahtar depolama sağlayıcısı olarak kaydedin.  
* Veriler şifrelenmiş sütuna ekleyin.
* Şifrelenmiş bir sütunda belirli bir değeri filtreleyerek bir kaydı seçin.

Öğesinin içeriğini değiştirin **Program.cs** aşağıdaki kod ile. Geçerli bağlantı dizenizi Azure portalından Main yöntemiyle doğrudan önündeki satır genel connectionString değişken için bağlantı dizesini değiştirin. Bu kod için yapmanız gereken tek değişiklik budur.

Always Encrypted nasıl çalıştığını görmek için uygulamayı çalıştırın.
```CS
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider;

    namespace AlwaysEncryptedConsoleAKVApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"<connection string from the portal>";
        static string applicationId = @"<application ID from your AAD application>";
        static string clientKey = "<key from your AAD application>";


        static void Main(string[] args)
        {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993")
            });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        private static ClientCredential _clientCredential;

        static void InitializeAzureKeyVaultProvider()
        {

            _clientCredential = new ClientCredential(applicationId, clientKey);

            SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
              new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

            Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
              new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

            providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        }

        public async static Task<string> GetToken(string authority, string resource, string scope)
        {
            var authContext = new AuthenticationContext(authority);
            AuthenticationResult result = await authContext.AcquireTokenAsync(resource, _clientCredential);

            if (result == null)
                throw new InvalidOperationException("Failed to obtain the access token");
            return result.AccessToken;
        }

        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
     VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }
```


## <a name="verify-that-the-data-is-encrypted"></a>Verilerin şifrelendiğinden emin olun
Hastalara verileri SSMS ile sorgulama yaparak sunucunun gerçek veriler şifrelenir hızlıca göz atabilirsiniz (geçerli bağlantıyla burada **sütun şifreleme ayarı** henüz etkin değil).

Clinic veritabanında aşağıdaki sorguyu çalıştırın.

```sql
    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
```

Herhangi bir düz metin veri içermemesi şifrelenmiş sütunları görebilirsiniz.

   ![Yeni konsol uygulaması](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)

SSMS düz metin verilerine erişmek için kullanmak için önce kullanıcı Azure anahtar kasası için uygun izinlere sahip olduğundan emin olmak gerekir: *alma*, *unwrapKey*, ve *doğrulayın*. Ayrıntılı bilgi için bkz. [oluştur ve Store sütun ana anahtarları (her zaman şifreli)](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted).

Ardından Ekle *sütun şifreleme ayarı = etkin* bağlantınızı sırasında parametre.

1. SSMS'de, sunucuya sağ tıklayın **Nesne Gezgini** ve **Bağlantıyı Kes**.
2. Tıklayın **Connect** > **veritabanı altyapısı** açmak için **sunucuya Bağlan** penceresini açın ve **seçenekleri**.
3. Tıklayın **ek bağlantı parametreleri** ve türü **sütun şifreleme ayarı = etkin**.
   
    ![Yeni konsol uygulaması](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)
4. Clinic veritabanında aşağıdaki sorguyu çalıştırın.

   ```sql
      SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   ```

     Şimdi, şifrelenmiş sütunlarda düz metin verileri görebilirsiniz.
     ![Yeni konsol uygulaması](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a>Sonraki adımlar
Always Encrypted kullanan bir veritabanı oluşturduktan sonra aşağıdakileri yapmak isteyebilirsiniz:

* [Döndürme ve anahtarlarınızı Temizleme](https://msdn.microsoft.com/library/mt607048.aspx).
* [Always Encrypted ile zaten şifrelenmiş verileri geçirmek](https://msdn.microsoft.com/library/mt621539.aspx).

## <a name="related-information"></a>İlgili bilgiler
* [(İstemci geliştirme) her zaman şifreli](https://msdn.microsoft.com/library/mt147923.aspx)
* [Saydam veri şifrelemesi](https://msdn.microsoft.com/library/bb934049.aspx)
* [SQL Server şifreleme](https://msdn.microsoft.com/library/bb510663.aspx)
* [Her zaman şifreli sihirbazını](https://msdn.microsoft.com/library/mt459280.aspx)
* [Always Encrypted'e blogu](https://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

