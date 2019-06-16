---
title: 'Her zaman şifreli: Windows sertifika deposu - Azure SQL veritabanı | Microsoft Docs'
description: Bu makalede SQL Server Management Studio (SSMS) her zaman şifreli sihirbazını kullanarak hassas verileri bir SQL veritabanında veritabanı şifreleme güvenliğinin nasıl sağlanacağını gösterir. Ayrıca, Windows sertifika depolama alanında, şifreleme anahtarlarını depolamak nasıl gösterir.
keywords: Always Encrypted verileri, sql şifrelemesi, veritabanı şifreleme, hassas verileri şifreleyin
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviwer: ''
manager: craigg
ms.date: 03/08/2019
ms.openlocfilehash: 5226ec05af95cf305008968cf945070532274ee5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61420135"
---
# <a name="always-encrypted-protect-sensitive-data-and-store-encryption-keys-in-the-windows-certificate-store"></a>Her zaman şifreli: Hassas verilerin korunmasına ve şifreleme anahtarlarını Windows sertifika deposuna kaydedin

Bu makalede, hassas verileri bir SQL veritabanında veritabanı şifrelemesi kullanarak güvenli hale getirme işlemini göstermektedir [her zaman şifreli sihirbazını](https://msdn.microsoft.com/library/mt459280.aspx) içinde [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Ayrıca, Windows sertifika depolama alanında, şifreleme anahtarlarını depolamak nasıl gösterir.

Her zaman şifreli Azure SQL veritabanı'nda yeni bir veri şifreleme teknolojisi ve yardımcı olan bir SQL Server istemci ile sunucu arasında taşıma sırasında sunucu üzerinde bekleyen hassas verileri korumak ve veri kullanımda olsa da, hassas verilerin asla sağlama olarak görünür düz metin içinde veritabanı sistemidir. Veri şifreleme sonra yalnızca istemci uygulamaları veya anahtarlarının erişimi uygulama sunucuları düz metin verilere erişebilir. Ayrıntılı bilgi için bkz. [(veritabanı altyapısı)'Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx).

Always Encrypted kullanılacak veritabanını yapılandırdıktan sonra şifrelenmiş verilerle çalışmak için Visual Studio ile C# bir istemci uygulaması oluşturacaksınız.

Her zaman şifreli için bir Azure SQL veritabanı hakkında bilgi edinmek için bu makaledeki adımları izleyin. Bu makalede, aşağıdaki görevlerin nasıl gerçekleştirileceğini öğreneceksiniz:

* Her zaman şifreli sihirbazını SSMS'de oluşturulacağı [her zaman şifreli anahtarları](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
  * Oluşturma bir [sütun ana anahtarı (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
  * Oluşturma bir [sütun şifreleme anahtarı (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
* Bir veritabanı tablosu oluşturun ve sütun şifreleme.
* Ekler, seçer ve şifrelenmiş sütundaki verileri görüntüleyen bir uygulama oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, şunları yapmanız gerekir:

* Bir Azure hesabı ve aboneliği Yoksa, oturum açmak için bir [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) 13.0.700.242 sürümü veya üzeri.
* [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) veya üzeri (istemci bilgisayarda).
* [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).

## <a name="create-a-blank-sql-database"></a>Boş bir SQL veritabanı oluşturma

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Tıklayın **kaynak Oluştur** > **veri + depolama** > **SQL veritabanı**.
3. Oluşturma bir **boş** adlı veritabanı **Clinic** yeni veya var olan bir sunucu üzerinde. Azure portalında veritabanı oluşturma hakkında ayrıntılı yönergeler için bkz: [ilk Azure SQL veritabanınızı](sql-database-single-database-get-started.md).

    ![Boş veritabanı oluşturma](./media/sql-database-always-encrypted/create-database.png)

Öğreticide daha sonra bağlantı dizesi gerekir. Veritabanı oluşturulduktan sonra yeni Clinic veritabanına gidin ve bağlantı dizesini kopyalayın. Herhangi bir zamanda bağlantı dizesini elde edebilirsiniz, ancak Azure portalında işiniz kopyalamak daha kolaydır.

1. Tıklayın **SQL veritabanları** > **Clinic** > **veritabanı bağlantı dizelerini Göster**.
2. İçin bağlantı dizesini kopyalayın **ADO.NET**.

    ![Bağlantı dizesini kopyalayın](./media/sql-database-always-encrypted/connection-strings.png)

## <a name="connect-to-the-database-with-ssms"></a>SSMS ile veritabanına bağlanma

SSMS'yi açın ve Clinic veritabanı sunucusuna bağlanın.

1. SSMS’i açın. (Tıklayın **Connect** > **veritabanı altyapısı** açmak için **sunucuya Bağlan** penceresi açık değilse).
2. Sunucu adı ve kimlik bilgilerini girin. Sunucu adı SQL veritabanı dikey penceresinde bulunabilir ve bağlantı dizesini, daha önce kopyaladığınız. Tam sunucu adını içeren tür *database.windows.net*.

    ![Bağlantı dizesini kopyalayın](./media/sql-database-always-encrypted/ssms-connect.png)

Varsa **yeni güvenlik duvarı kuralı** penceresi açılır; oturum açma ve Azure'a let SSMS sizin için yeni bir güvenlik duvarı kuralı oluşturun.

## <a name="create-a-table"></a>Bir tablo oluşturma

Bu bölümde, Hasta verileri tutmak için bir tablo oluşturacaksınız. Bu normal bir tablo başlangıçta--olacaktır şifreleme sonraki bölümde yapılandıracak.

1. Genişletin **veritabanları**.
2. Sağ **Clinic** tıklayın ve veritabanı **yeni sorgu**.
3. Yeni sorgu penceresine aşağıdaki Transact-SQL (T-SQL) yapıştırın ve **yürütme** bu.

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

## <a name="encrypt-columns-configure-always-encrypted"></a>(Her zaman şifreli yapılandırma) sütun şifreleme

SSMS kolayca ayarlayarak CMK CEK ve şifrelenmiş sütunlar için Always Encrypted yapılandırmak için bir sihirbaz sağlar.

1. Genişletin **veritabanları** > **Clinic** > **tabloları**.
2. Sağ **Hastalara** tablosunu seçip **şifrelemek sütunları** her zaman şifreli sihirbazını açmak için:

    ![Sütun şifreleme](./media/sql-database-always-encrypted/encrypt-columns.png)

Her zaman şifreli sihirbazını aşağıdaki bölümleri içerir: **Sütun seçimini**, **ana anahtarı yapılandırma** (CMK) **doğrulama**, ve **özeti**.

### <a name="column-selection"></a>Sütun Seçimi

Tıklayın **sonraki** üzerinde **giriş** sayfasını açmak için **sütun seçimi** sayfası. Bu sayfada, şifrelemek istediğiniz sütunları seçersiniz [şifreleme türünü ve hangi sütun şifreleme anahtarı (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) kullanılacak.

Şifreleme **SSN** ve **BirthDate** her hasta için bilgi. **SSN** sütun eşitlik aramalar, birleştirmeler ve Grupla destekler belirlenimci şifreleme kullanır. **BirthDate** sütun işlemlerini desteklemeyen rastgele şifreleme kullanır.

Ayarlama **şifreleme türü** için **SSN** sütuna **Deterministic** ve **doğum tarihi** sütuna **Randomized** . **İleri**’ye tıklayın.

![Sütun şifreleme](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Ana anahtarını yapılandırma

**Ana anahtarı yapılandırma** sayfasıdır Burada, CMK ayarlayın ve anahtar depolama sağlayıcısı seçin CMK depolanacağı. Şu anda bir CMK Windows sertifika deposu, Azure anahtar kasası veya bir donanım güvenlik modülü (HSM) depolayabilirsiniz. Bu öğreticide, anahtarlarınızı Windows sertifika deposuna gösterilmektedir.

Doğrulayın **Windows sertifika deposu** tıklatın ve seçili **sonraki**.

![Ana anahtarını yapılandırma](./media/sql-database-always-encrypted/master-key-configuration.png)

### <a name="validation"></a>Doğrulama

Sütunları şifrelemek veya daha sonra çalıştırmak için bir PowerShell komut dosyasını kaydedin. Bu öğreticide, seçin **artık tamamlanması devam** tıklatıp **sonraki**.

### <a name="summary"></a>Özet

Ayarların doğru olduğunu ve tıklayın doğrulayın **son** her zaman şifreli için kurulumu tamamlamak için.

![Özet](./media/sql-database-always-encrypted/summary.png)

### <a name="verify-the-wizards-actions"></a>Sihirbaz eylemleri doğrulayın

Sihirbaz tamamlandıktan sonra veritabanı her zaman şifreli için ayarlanmıştır. Sihirbaz aşağıdaki eylemleri:

* Bir CMK oluşturuldu.
* Bir CEK oluşturuldu.
* Seçili sütunları şifreleme için yapılandırılmış. **Hastalara** tablo şu anda hiç veri yok, ancak seçili olan sütunlardaki var olan tüm verileri artık şifrelenir.

SSMS anahtarların oluşturulması giderek doğrulayabilirsiniz **Clinic** > **güvenlik** > **her zaman şifreli anahtarları**. Artık, sihirbaz sizin için oluşturulan yeni anahtarları görebilirsiniz.

## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Şifrelenmiş veriler ile uygun bir istemci uygulaması oluşturma

Always Encrypted ayarlamak, gerçekleştiren bir uygulama oluşturabilirsiniz *ekler* ve *seçer* şifrelenmiş sütun. Örnek uygulamayı çalıştırabilmeniz için bunu aynı bilgisayarda her zaman şifreli sihirbazını çalıştığı çalıştırmanız gerekir. Başka bir bilgisayarda uygulamayı çalıştırmak için Always Encrypted sertifikalarınızı istemci uygulamasını çalıştıran bilgisayara dağıtmanız gerekir.  

> [!IMPORTANT]
> Uygulamanızı kullanmalısınız [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) düz metin veri sütunları Always Encrypted ile sunucuya geçerken nesneleri. SqlParameter nesneleri kullanmadan değişmez değerler geçirerek bir özel durumu oluşur.

1. Visual Studio'yu açın ve yeni bir C# konsol uygulaması oluşturun. Projeniz emin olun kümesine **.NET Framework 4.6** veya üzeri.
2. Projeyi adlandırın **AlwaysEncryptedConsoleApp** tıklatıp **Tamam**.

![Yeni konsol uygulaması](./media/sql-database-always-encrypted/console-app.png)

## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Always Encrypted'ı etkinleştirmek için bağlantı dizesini değiştirme

Bu bölümde, veritabanı bağlantı dizenizi Always Encrypted'ı etkinleştirmek açıklanmaktadır. "Örnek konsol uygulaması her zaman şifrelenir." sonraki bölümde, az önce oluşturduğunuz konsol uygulaması değiştirir

Always Encrypted'ı etkinleştirmek için eklemeniz gerekir **sütun şifreleme ayarı** anahtar sözcüğü, bağlantı dizesi ve ayarlamak **etkin**.

Bu doğrudan bağlantı dizesinde ayarlayabilirsiniz veya kullanarak ayarlayabilirsiniz bir [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). Sonraki bölümde örnek uygulamada nasıl kullanılacağını gösterir **SqlConnectionStringBuilder**.

> [!NOTE]
> Bu, her zaman şifreli için belirli bir istemci uygulaması gerekli yalnızca değişikliktir. Bağlantı dizesini harici olarak depolayan var olan bir uygulama varsa (diğer bir deyişle, bir yapılandırma dosyasında), Always Encrypted, herhangi bir kod değişikliği olmadan etkinleştirmek mümkün olabilir.

### <a name="enable-always-encrypted-in-the-connection-string"></a>Bağlantı dizesinde Always Encrypted'ı etkinleştir

Aşağıdaki anahtar sözcüğü, bağlantı dizesi ekleyin:

    Column Encryption Setting=Enabled

### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Her zaman bir SqlConnectionStringBuilder ile şifrelenmiş etkinleştir

Aşağıdaki kod her zaman şifreli ayarlayarak etkinleştirme gösterir [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) için [etkin](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="always-encrypted-sample-console-application"></a>Her zaman şifreli örnek konsol uygulaması

Bu örnek gösterir nasıl yapılır:

* Always Encrypted'ı etkinleştirmek için bağlantı dizesini değiştirin.
* Veriler şifrelenmiş sütuna ekleyin.
* Şifrelenmiş bir sütunda belirli bir değeri filtreleyerek bir kaydı seçin.

Öğesinin içeriğini değiştirin **Program.cs** aşağıdaki kod ile. Geçerli bağlantı dizenizi Azure portalından doğrudan Main yönteminin üzerine satır genel connectionString değişken için bağlantı dizesini değiştirin. Bu kod için yapmanız gereken tek değişiklik budur.

Always Encrypted nasıl çalıştığını görmek için uygulamayı çalıştırın.

```cs
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
using System.Globalization;

namespace AlwaysEncryptedConsoleApp
{
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Data Source = SPE-T640-01.sys-sqlsvr.local; Initial Catalog = Clinic; Integrated Security = true";

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

            CultureInfo culture = CultureInfo.CreateSpecificCulture("en-US");
            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985", culture)
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993", culture)
            });


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
```

## <a name="verify-that-the-data-is-encrypted"></a>Verilerin şifrelendiğinden emin olun

Sorgulayarak sunucunun gerçek veriler şifrelenir hızlı bir şekilde denetleyebilirsiniz **Hastalara** SSMS ile veri. (Burada sütun şifreleme ayarı henüz etkinleştirilmedi, geçerli bir bağlantı kullanın.)

Clinic veritabanında aşağıdaki sorguyu çalıştırın.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Herhangi bir düz metin veri içermemesi şifrelenmiş sütunları görebilirsiniz.

   ![Yeni konsol uygulaması](./media/sql-database-always-encrypted/ssms-encrypted.png)

Düz metin verilerine erişmek için SSMS kullanmak için ekleyebilirsiniz **sütun şifreleme ayarı = etkin** bağlantı parametresi.

1. SSMS'de, sunucuya sağ tıklayın **Nesne Gezgini**ve ardından **Bağlantıyı Kes**.
2. Tıklayın **Connect** > **veritabanı altyapısı** açmak için **sunucuya Bağlan** penceresi ve ardından **seçenekleri**.
3. Tıklayın **ek bağlantı parametreleri** ve türü **sütun şifreleme ayarı = etkin**.

    ![Yeni konsol uygulaması](./media/sql-database-always-encrypted/ssms-connection-parameter.png)
4. Aşağıdaki sorguyu çalıştırın **Clinic** veritabanı.

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Şimdi, şifrelenmiş sütunlarda düz metin verileri görebilirsiniz.

    ![Yeni konsol uygulaması](./media/sql-database-always-encrypted/ssms-plaintext.png)

> [!NOTE]
> SSMS (veya herhangi bir istemci) başka bir bilgisayardan bağlanıyorsanız, şifreleme anahtarları erişim sahip değil ve verilerin şifresini çözmek mümkün olmayacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Always Encrypted kullanan bir veritabanı oluşturduktan sonra aşağıdakileri yapmak isteyebilirsiniz:

* Bu örnek, farklı bir bilgisayardan çalıştırın. Düz metin verilerine erişimi olmaz ve başarıyla çalışmaz şifreleme anahtarlarına erişimi olmaz.
* [Döndürme ve anahtarlarınızı Temizleme](https://msdn.microsoft.com/library/mt607048.aspx).
* [Always Encrypted ile zaten şifrelenmiş verileri geçirmek](https://msdn.microsoft.com/library/mt621539.aspx).
* [Always Encrypted sertifikalarını diğer istemci makinelerine dağıtma](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) ("Yapmadan sertifikalar kullanılabilir uygulamaların ve kullanıcıların" bölümüne bakın).

## <a name="related-information"></a>İlgili bilgiler

* [(İstemci geliştirme) her zaman şifreli](https://msdn.microsoft.com/library/mt147923.aspx)
* [Saydam veri şifrelemesi](https://msdn.microsoft.com/library/bb934049.aspx)
* [SQL Server şifreleme](https://msdn.microsoft.com/library/bb510663.aspx)
* [Her zaman şifreli sihirbazını](https://msdn.microsoft.com/library/mt459280.aspx)
* [Always Encrypted'e blogu](https://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)