---
title: "Her zaman şifreli: Azure SQL veritabanı - Windows sertifika deposunda | Microsoft Docs"
description: "Bu makalede, SQL Server Management Studio (SSMS) her zaman şifreli sihirbazını kullanarak bir SQL veritabanında veritabanı şifreleme ile hassas verilerin güvenliğini sağlamak nasıl gösterilmektedir. Ayrıca, Windows sertifika deposunda, şifreleme anahtarlarını depolamak nasıl gösterir."
keywords: "her zaman şifreli verileri, sql şifrelemesi, veritabanı şifreleme, hassas verileri şifrelemek"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: ce7e052e-8bf6-4d7c-9204-4c6f4afeba4b
ms.service: sql-database
ms.custom: security
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: sstein
ms.openlocfilehash: 8e86648195811a666a197b6ee06ad610a1c8d568
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-the-windows-certificate-store"></a>Her zaman şifreli: SQL veritabanındaki hassas verileri korumak ve şifreleme anahtarlarınızı Windows sertifika deposunda depola

Bu makalede hassas verileri bir SQL veritabanında veritabanı şifreleme ile kullanarak güvenli hale getirmek gösterilmiştir [her zaman şifreli Sihirbazı](https://msdn.microsoft.com/library/mt459280.aspx) içinde [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Ayrıca, Windows sertifika deposunda, şifreleme anahtarlarını depolamak nasıl gösterir.

Her zaman şifreli bir yeni veri şifreleme Azure SQL veritabanındaki bir teknolojidir ve yardımcı olan bir SQL Server istemci ve sunucu arasında hareket sırasında sunucuda, rest hassas verileri korumak ve veri kullanımdayken bu hassas verileri asla sağlama veritabanı sistem içinde düz metin olarak görünür. Veri şifrelemek sonra yalnızca istemci uygulamaları veya anahtarlara erişimi uygulama sunucuları düz metin verilere erişebilir. Ayrıntılı bilgi için bkz: [(veritabanı altyapısı)'her zaman şifreli](https://msdn.microsoft.com/library/mt163865.aspx).

Her zaman şifreli kullanılacak veritabanını yapılandırdıktan sonra C# şifrelenmiş verilerle çalışmak için Visual Studio ile bir istemci uygulaması oluşturacaksınız.

Her zaman şifreli bir Azure SQL veritabanı için ayarlama hakkında bilgi edinmek için bu makaledeki adımları izleyin. Bu makalede, aşağıdaki görevleri gerçekleştirmek öğreneceksiniz:

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

## <a name="create-a-blank-sql-database"></a>Boş bir SQL veritabanı oluşturma
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Tıklatın **kaynak oluşturma** > **veri + depolama** > **SQL veritabanı**.
3. Oluşturma bir **boş** adlı veritabanı **Clinic** yeni veya var olan bir sunucuda. Azure portalında bir veritabanı oluşturma hakkında ayrıntılı yönergeler için bkz: [ilk Azure SQL veritabanınızı](sql-database-get-started-portal.md).
   
    ![Boş veritabanı oluşturma](./media/sql-database-always-encrypted/create-database.png)

Öğreticide daha sonra bağlantı dizesi gerekir. Veritabanı oluşturulduktan sonra yeni Clinic veritabanına gidin ve bağlantı dizesini kopyalayın. Bağlantı dizesi dilediğiniz zaman alabilir, ancak Azure portalında olduğunuzda kopyalamak kolaydır.

1. Tıklatın **SQL veritabanları** > **Clinic** > **veritabanı bağlantı dizelerini Göster**.
2. Bağlantı dizesini kopyalayın **ADO.NET**.
   
    ![Bağlantı dizesini kopyalayın](./media/sql-database-always-encrypted/connection-strings.png)

## <a name="connect-to-the-database-with-ssms"></a>SSMS ile veritabanına bağlanma
SSMS açın ve Clinic veritabanı sunucusuna bağlanın.

1. SSMS açın. (Tıklatın **Bağlan** > **veritabanı altyapısı** açmak için **sunucuya Bağlan** penceresi açık değilse).
2. Sunucu adı ve kimlik bilgilerini girin. Sunucu adı SQL veritabanı dikey penceresinde bulunabilir ve daha önce kopyaladığınız bağlantı dizesinde. Tam sunucu adını içeren tür *database.windows.net*.
   
    ![Bağlantı dizesini kopyalayın](./media/sql-database-always-encrypted/ssms-connect.png)

Varsa **yeni güvenlik duvarı kuralı** penceresi açılır, oturum açma Azure ve let SSMS sizin için yeni bir güvenlik duvarı kuralı oluşturun.

## <a name="create-a-table"></a>Bir tablo oluşturma
Bu bölümde, Hasta verileri tutmak için bir tablo oluşturacaksınız. Bu normal bir tablo başlangıçta--olacaktır şifreleme sonraki bölümde yapılandıracak.

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
SSMS kolayca ayarlayarak CMK CEK ve şifrelenmiş sütunlar, her zaman şifreli yapılandırmak için bir sihirbaz sağlar.

1. Genişletme **veritabanları** > **Clinic** > **tabloları**.
2. Sağ **hastalar** tablo ve seçin **şifrelemek sütunları** her zaman şifreli Sihirbazı'nı açmak için:
   
    ![Sütunları şifrele](./media/sql-database-always-encrypted/encrypt-columns.png)

Her zaman şifreli Sihirbazı aşağıdaki bölümleri içerir: **sütun seçimi**, **ana anahtar yapılandırma** (CMK) **doğrulama**, ve **Özet**.

### <a name="column-selection"></a>Sütun Seçimi
Tıklatın **sonraki** üzerinde **giriş** sayfasını açmak için **sütun seçimi** sayfası. Bu sayfada, şifrelemek istediğiniz sütunları seçecektir [şifreleme türünü ve hangi sütun şifreleme anahtarı (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) kullanmak için.

Şifreleme **SSN** ve **doğum tarihi** bilgi için her bir süre bekleyin. **SSN** sütun eşitlik aramaları, birleştirmeler ve Grupla destekleyen belirleyici şifreleme kullanır. **Doğum tarihi** sütun işlemlerini desteklemeyen rastgele şifreleme kullanır.

Ayarlama **şifreleme türü** için **SSN** sütuna **Deterministic** ve **doğum tarihi** sütuna **Randomized**. **İleri**’ye tıklayın.

![Sütunları şifrele](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Ana anahtar yapılandırma
**Ana anahtar yapılandırma** sayfasıdır Burada, CMK ayarlamak ve anahtar depolama sağlayıcısı seçin CMK depolanacağı. Şu anda bir CMK Windows sertifika deposunda, Azure anahtar kasası veya bir donanım güvenlik modülü (HSM) depolayabilirsiniz. Bu öğretici, anahtarlarınızı Windows sertifika deposunda depola gösterilmektedir.

Doğrulayın **Windows sertifika deposunda** seçilir ve tıklatın **sonraki**.

![Ana anahtar yapılandırma](./media/sql-database-always-encrypted/master-key-configuration.png)

### <a name="validation"></a>Doğrulama
Sütunları şifrelemek veya daha sonra çalıştırmak için bir PowerShell komut dosyasını kaydedin. Bu öğretici için seçin **şimdi tamamlamak için devam** tıklatıp **sonraki**.

### <a name="summary"></a>Özet
Ayarların doğru olduğunu tıklatın olduğunu doğrulayıp **son** her zaman şifreli için kurulumu tamamlamak için.

![Özet](./media/sql-database-always-encrypted/summary.png)

### <a name="verify-the-wizards-actions"></a>Sihirbazın Eylemler doğrulayın
Sihirbaz tamamlandıktan sonra veritabanı her zaman şifreli için ayarlanır. Sihirbaz aşağıdaki eylemleri:

* Bir CMK oluşturulur.
* Bir CEK oluşturulur.
* Seçili sütunları şifreleme için yapılandırılmış. **Hastalar** tablo şu anda hiçbir veri olsa da, seçili olan sütunlardaki mevcut verileri artık şifrelenir.

SSMS anahtarlarında oluşturulmasını giderek doğrulayabilirsiniz **Clinic** > **güvenlik** > **her zaman şifreli anahtarları**. Sihirbaz sizin için oluşturulan yeni anahtarlar artık görebilirsiniz.

## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Şifrelenmiş veriler ile çalışan istemci uygulaması oluşturma
Her zaman şifreli ayarlamak, gerçekleştiren bir uygulama oluşturabilirsiniz *ekler* ve *seçer* şifrelenmiş sütunlar üzerinde. Örnek Uygulama başarıyla çalışması için onu aynı bilgisayarda her zaman şifreli Sihirbazı'nı çalıştırdığınız çalıştırmalısınız. Uygulamayı başka bir bilgisayarda çalıştırmak için her zaman şifreli sertifikalarınızı istemci uygulaması çalıştıran bilgisayara dağıtmanız gerekir.  

> [!IMPORTANT]
> Uygulamanızı kullanmalısınız [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) nesneler her zaman şifreli sütunlarla sunucuya düz metin veri geçirme olduğunda. Değişmez değerler SqlParameter nesnelerini kullanmadan geçirme bir özel durum neden olur.
> 
> 

1. Visual Studio'yu açın ve yeni bir C# konsol uygulaması oluşturun. Emin olun, projenizin ayarlanmış **.NET Framework 4.6** veya sonraki bir sürümü.
2. Proje adı **AlwaysEncryptedConsoleApp** tıklatıp **Tamam**.

![Yeni konsol uygulaması](./media/sql-database-always-encrypted/console-app.png)

## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Her zaman şifreli etkinleştirmek için bağlantı dizesini değiştirin
Bu bölümde, her zaman şifreli veritabanı bağlantı dizenizi etkinleştirmek açıklanmaktadır. "Her zaman örnek konsol uygulaması şifrelenmiş." sonraki bölümde oluşturduğunuz konsol uygulaması değiştirir

Her zaman şifreli etkinleştirmek için eklemeniz gerekir **sütun şifreleme ayarı** bağlantınızı anahtar dize ve ayarlamak **etkin**.

Bu doğrudan bağlantı dizesinde ayarlayabilirsiniz veya kullanarak ayarlayabilirsiniz bir [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). Sonraki bölümde örnek uygulamada nasıl kullanılacağını gösterir **SqlConnectionStringBuilder**.

> [!NOTE]
> Bu, her zaman şifreli için belirli bir istemci uygulaması gerekli yalnızca değişikliktir. Harici olarak kendi bağlantı dizesi depolar var olan bir uygulamanız varsa, (diğer bir deyişle, bir yapılandırma dosyasında), her zaman şifreli herhangi bir kod değiştirmeden etkinleştirmek olabilir.
> 
> 

### <a name="enable-always-encrypted-in-the-connection-string"></a>Her zaman şifreli bağlantı dizesinde etkinleştir
Aşağıdaki anahtar sözcüğü bağlantı dizenizi ekleyin:

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Her zaman bir SqlConnectionStringBuilder ile şifrelenmiş etkinleştir
Aşağıdaki kodu her zaman şifreli ayarlayarak nasıl etkinleştirileceği gösterilmiştir [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) için [etkin](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a>Her zaman şifreli örnek konsol uygulaması
Bu örnek gösterilmektedir nasıl yapılır:

* Her zaman şifreli etkinleştirmek için bağlantı dizesini değiştirin.
* Veriler şifreli sütunlara ekleyin.
* Bir kayıt şifrelenmiş sütununda belirli bir değeri filtreleyerek seçin.

Değiştir **Program.cs** aşağıdaki kod ile. Geçerli bağlantı dizenizi Azure portalından Main yönteminin doğrudan üstündeki satırın genel connectionString değişkeninde için bağlantı dizesini değiştirin. Bu kod için yapmanız gereken tek değişiklik budur.

Her zaman şifreli eylemde görmek için uygulamayı çalıştırın.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;

    namespace AlwaysEncryptedConsoleApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
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

            InsertPatient(new Patient() {
                SSN = "999-99-0001", FirstName = "Orlando", LastName = "Gee", BirthDate = DateTime.Parse("01/04/1964") });
            InsertPatient(new Patient() {
                SSN = "999-99-0002", FirstName = "Keith", LastName = "Harris", BirthDate = DateTime.Parse("06/20/1977") });
            InsertPatient(new Patient() {
                SSN = "999-99-0003", FirstName = "Donna", LastName = "Carreras", BirthDate = DateTime.Parse("02/09/1973") });
            InsertPatient(new Patient() {
                SSN = "999-99-0004", FirstName = "Janet", LastName = "Gates", BirthDate = DateTime.Parse("08/31/1985") });
            InsertPatient(new Patient() {
                SSN = "999-99-0005", FirstName = "Lucy", LastName = "Harrington", BirthDate = DateTime.Parse("05/06/1993") });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
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
Sorgulayarak sunucuda gerçek veri şifrelenir hızlı bir şekilde denetleyebilirsiniz **hastalar** SSMS ile verileri. (Burada sütun şifreleme ayarı henüz etkinleştirilmedi geçerli bağlantınızı kullanın.)

Clinic veritabanında aşağıdaki sorguyu çalıştırın.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Şifrelenmiş sütunlar herhangi bir düz metin veri içermeyen görebilirsiniz.

   ![Yeni konsol uygulaması](./media/sql-database-always-encrypted/ssms-encrypted.png)

SSMS düz metin verilere erişmek için kullanılacak ekleyebileceğiniz **sütun şifreleme ayarı = etkin** bağlantı parametresi.

1. SSMS, sunucunuzun sağ **Object Explorer**ve ardından **Bağlantıyı Kes**.
2. Tıklatın **Bağlan** > **veritabanı altyapısı** açmak için **sunucuya Bağlan** penceresi ve ardından **seçenekleri**.
3. Tıklatın **ek bağlantı parametrelerini** ve türü **sütun şifreleme ayarı = etkin**.
   
    ![Yeni konsol uygulaması](./media/sql-database-always-encrypted/ssms-connection-parameter.png)
4. Aşağıdaki sorguyu çalıştırın **Clinic** veritabanı.
   
        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
   
     Şimdi şifrelenmiş sütunlardaki düz metin verileri görebilirsiniz.

    ![Yeni konsol uygulaması](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [!NOTE]
> Farklı bir bilgisayardan SSMS (veya herhangi bir istemci) ile bağlanıyorsanız, şifreleme anahtarlarını erişemeyecektir ve verilerin şifresini çözmek mümkün olmaz.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Her zaman şifreli kullanan bir veritabanı oluşturduktan sonra aşağıdakileri yapmak isteyebilirsiniz:

* Bu örnek, farklı bir bilgisayardan çalıştırın. Düz metin verilere erişimi olmaz ve başarılı bir şekilde çalışmayacak şekilde şifreleme anahtarları erişimi olmayacaktır.
* [Döndürün ve anahtarlarınızı Temizleme](https://msdn.microsoft.com/library/mt607048.aspx).
* [Zaten her zaman şifreli ile şifrelenmiş veri geçişi](https://msdn.microsoft.com/library/mt621539.aspx).
* [Diğer istemci makineler için her zaman şifreli sertifikalarını dağıtma](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) ("Yapmadan sertifikalar kullanılabilir uygulamaları ve kullanıcıların" bölümüne bakın).

## <a name="related-information"></a>İlgili bilgiler
* [Her zaman şifreli (istemci geliştirme)](https://msdn.microsoft.com/library/mt147923.aspx)
* [Saydam veri şifreleme](https://msdn.microsoft.com/library/bb934049.aspx)
* [SQL Server Encryption](https://msdn.microsoft.com/library/bb510663.aspx)
* [Her zaman şifreli Sihirbazı](https://msdn.microsoft.com/library/mt459280.aspx)
* [Her zaman şifreli blogu](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)

