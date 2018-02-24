---
title: "Her zaman şifreli: SQL veritabanı - Azure anahtar kasası | Microsoft Docs"
description: "Bu makalede SQL Server Management Studio'da her zaman şifreli Sihirbazı kullanarak veri şifrelemesi ile bir SQL veritabanındaki hassas verileri güvenli gösterilmiştir."
keywords: "veri şifreleme, şifreleme anahtarı, bulut şifreleme"
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
ms.assetid: 6ca16644-5969-497b-a413-d28c3b835c9b
ms.service: sql-database
ms.custom: security
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: sstein
ms.openlocfilehash: ca4566ced525f0cb732afc15d96d9ef73fd8cff5
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a>Her zaman şifreli: SQL veritabanındaki hassas verileri korumak ve Azure anahtar kasası, şifreleme anahtarlarını saklamak

Bu makalede veri şifreleme kullanarak bir SQL veritabanında hassas verilerin güvenliğini sağlamak gösterilmiştir [her zaman şifreli Sihirbazı](https://msdn.microsoft.com/library/mt459280.aspx) içinde [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Azure anahtar kasası her şifreleme anahtarı depolamak nasıl yapacağınızı gösterir yönergeleri de içerir.

Her zaman şifreli yeni bir veri şifreleme Azure SQL Database ve SQL Server'da, taşıma ve istemci ve sunucu arasında veri kullanımdayken sırasında bekleyen sunucu üzerinde hassas verilerin korunmasına yardımcı olur teknolojisidir. Her zaman şifreli hassas verileri hiçbir zaman veritabanı sistem içinde düz metin olarak görünmesini sağlar. Veri şifreleme yapılandırdıktan sonra yalnızca istemci uygulamaları veya anahtarlara erişimi uygulama sunucuları düz metin verilere erişebilir. Ayrıntılı bilgi için bkz: [(veritabanı altyapısı)'her zaman şifreli](https://msdn.microsoft.com/library/mt163865.aspx).

Her zaman şifreli kullanılacak veritabanını yapılandırdıktan sonra C# şifrelenmiş verilerle çalışmak için Visual Studio ile bir istemci uygulaması oluşturur.

Her zaman şifreli bir Azure SQL veritabanı için ayarlama hakkında bilgi edinmek ve bu makaledeki adımları izleyin. Bu makalede, aşağıdaki görevleri gerçekleştirmek öğreneceksiniz:

* Her zaman şifreli Sihirbazı'nı SSMS oluşturulacağı [her zaman şifreli anahtarları](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
  * Oluşturma bir [sütun ana anahtar (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
  * Oluşturma bir [sütun şifreleme anahtarı (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
* Bir veritabanı tablosu oluşturmak ve sütunları şifreleyebilirsiniz.
* Ekler, seçer ve şifrelenmiş sütunlar verileri görüntüleyen bir uygulama oluşturun.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici için ihtiyacınız vardır:

* Bir Azure hesabı ve aboneliği Yoksa, kaydolun bir [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) sürüm 13.0.700.242 veya sonraki bir sürümü.
* [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) veya üstünü (istemci bilgisayarda).
* [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* [Azure PowerShell](/powershell/azure/overview), sürüm 1.0 veya üstü. Tür **(Get-Module azure - listavailable birlikte). Sürüm** PowerShell sürümünü çalıştırıyorsanız görmek için.

## <a name="enable-your-client-application-to-access-the-sql-database-service"></a>İstemci uygulamanızın SQL Database hizmetine erişmek etkinleştirme
İstemci uygulamanızın SQL Database hizmeti bir Azure Active Directory (AAD) uygulamasını ayarlama ve kopyalama erişmek etkinleştirmeniz gerekir *uygulama kimliği* ve *anahtar* için gereken Uygulamanız için kimlik doğrulaması.

Alınacak *uygulama kimliği* ve *anahtar*, adımları [bir Azure Active Directory kaynaklara erişebilir uygulama ve hizmet sorumlusu oluşturmak](../azure-resource-manager/resource-group-create-service-principal-portal.md).

## <a name="create-a-key-vault-to-store-your-keys"></a>Anahtarlarınızı depolamak için bir anahtar kasası oluşturma
İstemci uygulamanızı yapılandırılır ve uygulama Kimliğinizi sahip göre bir anahtar kasası oluşturun ve siz ve uygulamanızı Kasası'nın gizli anahtarları (her zaman şifreli anahtarlar) erişebilmesi için erişim ilkesini yapılandırmak için zaman yapılır. *Oluşturma*, *almak*, *listesi*, *oturum*, *doğrulayın*, *wrapKey*, ve *unwrapKey* yeni bir sütun ana anahtar oluşturma ve şifreleme SQL Server Management Studio ile ayarlamak için gerekli izinleri.

Aşağıdaki komut dosyasını çalıştırarak, bir anahtar kasası hızlı bir şekilde oluşturabilirsiniz. Ayrıntılı bir açıklaması ve bu cmdlet'leri ve oluşturma ve bir anahtar kasası yapılandırma hakkında daha fazla bilgi için bkz: [Azure anahtar kasası ile çalışmaya başlama](../key-vault/key-vault-get-started.md).

    $subscriptionName = '<your Azure subscription name>'
    $userPrincipalName = '<username@domain.com>'
    $applicationId = '<application ID from your AAD application>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $vaultName = 'AeKeyVault'


    Login-AzureRmAccount
    $subscriptionId = (Get-AzureRmSubscription -SubscriptionName $subscriptionName).Id
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $resourceGroupName -Location $location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
    Set-AzureRmKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $applicationId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list




## <a name="create-a-blank-sql-database"></a>Boş bir SQL veritabanı oluşturma
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Git **kaynak oluşturma** > **veritabanları** > **SQL veritabanı**.
3. Oluşturma bir **boş** adlı veritabanı **Clinic** yeni veya var olan bir sunucuda. Azure portalında bir veritabanı oluşturmak konusunda ayrıntılı yönergeler için bkz: [ilk Azure SQL veritabanınızı](sql-database-get-started-portal.md).
   
    ![Boş veritabanı oluşturma](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

Bağlantı gerekir dize öğreticide daha sonra bu nedenle veritabanı oluşturduktan sonra yeni Clinic veritabanına göz atın ve bağlantı dizesini kopyalayın. Bağlantı dizesi dilediğiniz zaman alabilir, ancak Azure portalında kopyalamak kolaydır.

1. Git **SQL veritabanları** > **Clinic** > **veritabanı bağlantı dizelerini Göster**.
2. Bağlantı dizesini kopyalayın **ADO.NET**.
   
    ![Bağlantı dizesini kopyalayın](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)

## <a name="connect-to-the-database-with-ssms"></a>SSMS ile veritabanına bağlanma
SSMS açın ve Clinic veritabanı sunucusuna bağlanın.

1. SSMS açın. (Git **Bağlan** > **veritabanı altyapısı** açmak için **sunucuya Bağlan** açık değilse, pencere.)
2. Sunucu adı ve kimlik bilgilerini girin. Sunucu adı SQL veritabanı dikey penceresinde bulunabilir ve daha önce kopyaladığınız bağlantı dizesinde. Tam sunucu adını yazın dahil olmak üzere *database.windows.net*.
   
    ![Bağlantı dizesini kopyalayın](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

Varsa **yeni güvenlik duvarı kuralı** penceresi açılır, oturum açma Azure ve let SSMS sizin için yeni bir güvenlik duvarı kuralı oluşturun.

## <a name="create-a-table"></a>Bir tablo oluşturma
Bu bölümde, Hasta verileri tutmak için bir tablo oluşturacaksınız. Başlangıçta değil şifrelenmiş--şifreleme sonraki bölümde yapılandıracak.

1. Genişletme **veritabanları**.
2. Sağ **Clinic** veritabanı ve tıklatın **yeni sorgu**.
3. Aşağıdaki Transact-SQL (T-SQL) yeni sorgu penceresine yapıştırın ve **yürütme** onu.

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


## <a name="encrypt-columns-configure-always-encrypted"></a>(Her zaman şifreli yapılandırma) sütunları şifrele
SSMS kolayca ayarlayarak sütun ana anahtar, sütun şifreleme anahtarı ve şifrelenmiş sütunlar, her zaman şifreli yapılandırmanıza yardımcı olacak bir sihirbaz sağlar.

1. Genişletme **veritabanları** > **Clinic** > **tabloları**.
2. Sağ **hastalar** tablo ve seçin **şifrelemek sütunları** her zaman şifreli Sihirbazı'nı açmak için:
   
    ![Sütunları şifrele](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

Her zaman şifreli Sihirbazı aşağıdaki bölümleri içerir: **sütun seçimi**, **ana anahtar yapılandırma**, **doğrulama**, ve **Özet**.

### <a name="column-selection"></a>Sütun Seçimi
Tıklatın **sonraki** üzerinde **giriş** sayfasını açmak için **sütun seçimi** sayfası. Bu sayfada, şifrelemek istediğiniz sütunları seçecektir [şifreleme türünü ve hangi sütun şifreleme anahtarı (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) kullanmak için.

Şifreleme **SSN** ve **doğum tarihi** bilgi için her bir süre bekleyin. SSN sütun eşitlik aramaları, birleştirmeler ve Grupla destekleyen belirleyici şifreleme kullanır. Doğum tarihi sütun işlemlerini desteklemeyen rastgele şifreleme kullanır.

Ayarlama **şifreleme türü** SSN sütun için **Deterministic** ve doğum tarihi sütununu **Randomized**. **İleri**’ye tıklayın.

![Sütunları şifrele](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a>Ana anahtar yapılandırma
**Ana anahtar yapılandırma** sayfasıdır Burada, CMK ayarlamak ve anahtar depolama sağlayıcısı seçin CMK depolanacağı. Şu anda bir CMK Windows sertifika deposunda, Azure anahtar kasası veya bir donanım güvenlik modülü (HSM) depolayabilirsiniz.

Bu öğretici Azure anahtar kasası, anahtarları depolamak nasıl gösterir.

1. Seçin **Azure anahtar kasası**.
2. İstenen anahtar kasası aşağı açılan listeden seçin.
3. **İleri**’ye tıklayın.

![Ana anahtar yapılandırma](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)

### <a name="validation"></a>Doğrulama
Sütunları şifrelemek veya daha sonra çalıştırmak için bir PowerShell komut dosyasını kaydedin. Bu öğretici için seçin **şimdi tamamlamak için devam** tıklatıp **sonraki**.

### <a name="summary"></a>Özet
Ayarların doğru olduğunu tıklatın olduğunu doğrulayıp **son** her zaman şifreli için kurulumu tamamlamak için.

![Özet](./media/sql-database-always-encrypted-azure-key-vault/summary.png)

### <a name="verify-the-wizards-actions"></a>Sihirbazın Eylemler doğrulayın
Sihirbaz tamamlandıktan sonra veritabanı her zaman şifreli için ayarlanır. Sihirbaz aşağıdaki eylemleri:

* Bir sütun ana anahtar oluşturulur ve Azure anahtar Kasası ' depolanır.
* Bir sütun şifreleme anahtarı oluşturulur ve Azure anahtar Kasası ' depolanır.
* Seçili sütunları şifreleme için yapılandırılmış. Hastalar tablosu şu anda hiç veri içermiyor, ancak mevcut verileri seçili olan sütunlardaki şimdi şifrelenir.

SSMS anahtarlarında oluşturulmasını genişleterek doğrulayabilirsiniz **Clinic** > **güvenlik** > **her zaman şifreli anahtarları**.

## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Şifrelenmiş veriler ile çalışan istemci uygulaması oluşturma
Her zaman şifreli ayarlamak, gerçekleştiren bir uygulama oluşturabilirsiniz *ekler* ve *seçer* şifrelenmiş sütunlar üzerinde.  

> [!IMPORTANT]
> Uygulamanızı kullanmalısınız [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) nesneler her zaman şifreli sütunlarla sunucuya düz metin veri geçirme olduğunda. Değişmez değerler SqlParameter nesnelerini kullanmadan geçirme bir özel durum neden olur.
> 
> 

1. Visual Studio'yu açın ve yeni C# oluşturma **konsol uygulaması** (2015 ve daha önceki Visual Studio) veya **konsol uygulaması (.NET Framework)** (2017 ve daha sonra Visual Studio). Emin olun, projenizin ayarlanmış **.NET Framework 4.6** veya sonraki bir sürümü.
2. Proje adı **AlwaysEncryptedConsoleAKVApp** tıklatıp **Tamam**.
3. Giderek aşağıdaki NuGet paketi yüklemesi **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.

Bu iki kod satırı Paket Yöneticisi konsolunda çalıştırın.

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Her zaman şifreli etkinleştirmek için bağlantı dizesini değiştirin
Bu bölümde, her zaman şifreli veritabanı bağlantı dizenizi etkinleştirmek açıklanmaktadır.

Her zaman şifreli etkinleştirmek için eklemeniz gerekir **sütun şifreleme ayarı** bağlantınızı anahtar dize ve ayarlamak **etkin**.

Bu doğrudan bağlantı dizesinde ayarlayabilirsiniz veya kullanarak ayarlayabilirsiniz [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). Sonraki bölümde örnek uygulamada nasıl kullanılacağını gösterir **SqlConnectionStringBuilder**.

### <a name="enable-always-encrypted-in-the-connection-string"></a>Her zaman şifreli bağlantı dizesinde etkinleştir
Aşağıdaki anahtar sözcüğü bağlantı dizenizi ekleyin.

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a>Her zaman SqlConnectionStringBuilder ile şifrelenmiş etkinleştir
Aşağıdaki kodu her zaman şifreli ayarlayarak nasıl etkinleştirileceği gösterilmiştir [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) için [etkin](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-the-azure-key-vault-provider"></a>Azure anahtar kasası sağlayıcısını Kaydet
Aşağıdaki kod, ADO.NET sürücüsüyle Azure anahtar kasası sağlayıcısını kaydetmek gösterilmiştir.

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



## <a name="always-encrypted-sample-console-application"></a>Her zaman şifreli örnek konsol uygulaması
Bu örnek gösterilmektedir nasıl yapılır:

* Her zaman şifreli etkinleştirmek için bağlantı dizesini değiştirin.
* Azure anahtar kasası uygulamanın anahtar depolama sağlayıcısı olarak kaydedin.  
* Veriler şifreli sütunlara ekleyin.
* Bir kayıt şifrelenmiş sütununda belirli bir değeri filtreleyerek seçin.

Değiştir **Program.cs** aşağıdaki kod ile. Geçerli bağlantı dizenizi Azure portalından ana yöntemiyle doğrudan önündeki satır genel connectionString değişkeninde için bağlantı dizesini değiştirin. Bu kod için yapmanız gereken tek değişiklik budur.

Her zaman şifreli eylemde görmek için uygulamayı çalıştırın.

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



## <a name="verify-that-the-data-is-encrypted"></a>Verilerin şifrelendiğinden emin olun
SSMS hastalar verilerle sorgulayarak sunucuda gerçek veri şifrelenir hızlı bir şekilde kontrol edebilirsiniz (geçerli bağlantınızı kullanarak nerede **sütun şifreleme ayarı** henüz etkin değil).

Clinic veritabanında aşağıdaki sorguyu çalıştırın.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Şifrelenmiş sütunlar herhangi bir düz metin veri içermeyen görebilirsiniz.

   ![Yeni konsol uygulaması](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)

SSMS düz metin verilere erişmek için kullanılacak ekleyebileceğiniz *sütun şifreleme ayarı = etkin* bağlantı parametresi.

1. SSMS, sunucunuzun sağ **Nesne Gezgini** ve **Bağlantıyı Kes**.
2. Tıklatın **Bağlan** > **veritabanı altyapısı** açmak için **sunucuya Bağlan** penceresini açın ve **seçenekleri**.
3. Tıklatın **ek bağlantı parametrelerini** ve türü **sütun şifreleme ayarı = etkin**.
   
    ![Yeni konsol uygulaması](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)
4. Clinic veritabanında aşağıdaki sorguyu çalıştırın.
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     Şimdi şifrelenmiş sütunlardaki düz metin verileri görebilirsiniz.

    ![Yeni konsol uygulaması](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a>Sonraki adımlar
Her zaman şifreli kullanan bir veritabanı oluşturduktan sonra aşağıdakileri yapmak isteyebilirsiniz:

* [Döndürün ve anahtarlarınızı Temizleme](https://msdn.microsoft.com/library/mt607048.aspx).
* [Zaten her zaman şifreli ile şifrelenmiş veri geçişi](https://msdn.microsoft.com/library/mt621539.aspx).

## <a name="related-information"></a>İlgili bilgiler
* [Her zaman şifreli (istemci geliştirme)](https://msdn.microsoft.com/library/mt147923.aspx)
* [Saydam veri şifrelemesi](https://msdn.microsoft.com/library/bb934049.aspx)
* [SQL Server şifrelemesi](https://msdn.microsoft.com/library/bb510663.aspx)
* [Her zaman şifreli Sihirbazı](https://msdn.microsoft.com/library/mt459280.aspx)
* [Her zaman şifreli blogu](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

