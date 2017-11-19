---
title: "İlk Azure SQL veritabanınızı tasarım | Microsoft Docs"
description: "İlk Azure SQL veritabanınızı Azure portalında ve SQL Server Management Studio ile tasarım öğrenin."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop databases
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 08/25/2017
ms.author: carlrab
ms.openlocfilehash: 329003c7c4abe89f4af04473ee3664605b2ea81f
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="design-your-first-azure-sql-database"></a>İlk Azure SQL veritabanınızı tasarlama

Azure SQL veritabanı bir ilişkisel veritabanı-olarak-a (DBaaS) (Azure) Microsoft Cloud hizmetidir. Bu öğreticide, Azure portalını kullanmayı öğrenin ve [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) için: 

> [!div class="checklist"]
> * Azure portalında bir veritabanı oluşturun
> * Azure portalında bir sunucu düzeyinde güvenlik duvarı kuralı ayarlayın
> * Veritabanı SSMS ile bağlanma
> * SSMS ile tabloları oluşturma
> * Yığın BCP ile veri yükleme
> * SSMS ile bu verileri Sorgulama
> * Veritabanını önceki bir geri [geri yükleme noktası](sql-database-recovery-using-backups.md#point-in-time-restore) Azure portalında

Bir Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için yüklediğinizden emin olun:
- En yeni sürümünü [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS).
- En yeni sürümünü [BCP ve SQLCMD](https://www.microsoft.com/download/details.aspx?id=36433).

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-blank-sql-database"></a>Boş bir SQL veritabanı oluşturma

Azure SQL veritabanı bir dizi [işlem ve depolama kaynağı](sql-database-service-tiers.md) ile oluşturulur. Veritabanı bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ve bir [Azure SQL Veritabanı mantıksal sunucusu](sql-database-features.md) içinde oluşturulur. 

Boş bir SQL veritabanı oluşturmak için aşağıdaki adımları izleyin. 

1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.

2. **Yeni** penceresinden **Veritabanları**’nı seçin ve **Yeni** sayfasında **SQL Veritabanı** altından **Oluştur**’u seçin.

   ![Boş veritabanı oluşturma](./media/sql-database-design-first-database/create-empty-database.png)

3. SQL Veritabanı formunu, önceki görüntüde gösterildiği gibi aşağıdaki bilgilerle doldurun:   

   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Veritabanı adı** | mySampleDatabase | Geçerli veritabanı adları için bkz. [Veritabanı Tanımlayıcıları](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers). | 
   | **Abonelik** | Aboneliğiniz  | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
   | **Kaynak grubu** | myResourceGroup | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Kaynak seçin** | Boş veritabanı | Boş bir veritabanı oluşturulması gerektiğini belirtir. |

4. Yeni veritabanınız için yeni bir sunucu oluşturup yapılandırmak üzere **Sunucu**’ya tıklayın. Doldurmak **yeni sunucu form** aşağıdaki bilgilerle: 

   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Sunucu adı** | Genel olarak benzersiz bir ad | Geçerli sunucu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). | 
   | **Sunucu yöneticisi oturum açma bilgileri** | Geçerli bir ad | Geçerli oturum açma adları için bkz. [Veritabanı Tanımlayıcıları](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).|
   | **Parola** | Geçerli bir parola | Parolanız en az sekiz karakter olmalıdır ve aşağıdaki kategoriden üçünden karakterler içermelidir: büyük harf karakterler, küçük harfler, sayılar ve alfasayısal olmayan karakter. |
   | **Konum** | Geçerli bir konum | Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/). |

   ![create database-server](./media/sql-database-design-first-database/create-database-server.png)

5. **Seç**'e tıklayın.

6. Hizmet katmanını, DTU'ların sayısını ve depolama alanı miktarını belirtmek için **Fiyatlandırma katmanı**’na tıklayın. Dtu'lar ve her hizmet katmanı için kullanılabilir olan depolama sayısı seçeneklerini araştırın. 

7. Bu öğretici için seçin **standart** hizmet katmanı ve seçmek için kaydırıcıyı kullanın **100 Dtu'lar (S3)** ve **400** GB depolama alanı.

   ![create database-s1](./media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

8. **Ek Depolama** seçeneğini kullanmak için önizleme koşullarını kabul edin. 

   > [!IMPORTANT]
   > \* Mevcut depolama alanından büyük depolama alanları önizleme aşamasındadır ve ek maliyetler uygulanır. Ayrıntılar için bkz. [SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). 
   >
   >\* Premium katmanlarda 1 TB’den fazla depolama alanı şu bölgelerde sunulmaktadır: ABD Doğu2, Batı ABD, US Gov Virginia, Batı Avrupa, Almanya Orta, Güneydoğu Asya, Japonya Doğu, Avustralya Doğu, Kanada Orta ve Kanada Doğu. Bkz. [P11 P15 Geçerli Sınırlamalar](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
   > 

9. Sunucu katmanını, DTU'ların sayısını ve depolama alanı miktarını seçtikten sonra **Uygula**’ya tıklayın.  

10. Seçin bir **harmanlama** boş veritabanı için (Bu öğretici için varsayılan değeri kullanın). Harmanlamaları hakkında daha fazla bilgi için bkz: [harmanlamaları](https://docs.microsoft.com/sql/t-sql/statements/collations)

11. SQL Veritabanı formunu tamamladıktan sonra veritabanını sağlamak için **Oluştur**’a tıklayın. Sağlama birkaç dakika sürer. 

12. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.
    
     ![bildirim](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

SQL Veritabanı hizmeti, güvenlik duvarını belirli IP adreslerine açmaya yönelik bir güvenlik duvarı kuralı oluşturulmadıkça, dış uygulama ve araçların sunucuya ya da sunucu üzerindeki herhangi bir veritabanına bağlanmasını engelleyen sunucu düzeyinde bir güvenlik duvarı kuralı oluşturur. SQL Veritabanı güvenlik duvarı üzerinden yalnızca IP adresinize yönelik dış bağlantıları etkinleştirmek üzere istemcinizin IP adresi için bir [SQL Veritabanı sunucu düzeyi güvenlik duvarı kuralı](sql-database-firewall-configure.md) oluşturmak için bu adımları izleyin. 

> [!NOTE]
> SQL Veritabanı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Bir kurumsal ağ içerisinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL Veritabanı sunucunuza bağlanamazsınız.
>

1. Dağıtım tamamlandıktan sonra, soldaki menüden **SQL veritabanları**'na ve ardından **SQL veritabanları** sayfasında **mySampleDatabase** öğesine tıklayın. Veritabanınıza ilişkin genel bakış sayfası açılır ve tam sunucu adı (örneğin, **mynewserver-20170824.database.windows.net**) görüntülenerek daha fazla yapılandırma seçeneği sunulur. 

2. Sonraki hızlı başlangıçlarda sunucunuza ve veritabanlarına bağlanmak için bu tam sunucu adını kopyalayın. 

   ![sunucu adı](./media/sql-database-get-started-portal/server-name.png) 

3. Tıklatın **ayarlayın sunucu Güvenlik Duvarı** araç çubuğunda. SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır. 

   ![sunucu güvenlik duvarı kuralı](./media/sql-database-get-started-portal/server-firewall-rule.png) 

4. Geçerli IP adresinizi yeni bir güvenlik duvarı kuralına eklemek için araç çubuğunda **İstemci IP’si Ekle** öğesine tıklayın. Güvenlik duvarı kuralı, 1433 numaralı bağlantı noktasını tek bir IP adresi veya bir IP adresi aralığı için açabilir.

5. **Kaydet** düğmesine tıklayın. Geçerli IP adresiniz için mantıksal sunucuda 1433 numaralı bağlantı noktası açılarak sunucu düzeyinde güvenlik duvarı kuralı oluşturulur.

6. **Tamam**’a tıklayın ve sonra **Güvenlik duvarı ayarları** sayfasını kapatın.

Artık SQL Server Management Studio’yu veya seçtiğiniz başka bir aracı kullanarak, daha önce oluşturduğunuz sunucu yöneticisi hesabıyla bu IP adresinden SQL Veritabanı sunucusuna ve sunucuya ait veritabanlarına bağlanabilirsiniz.

> [!IMPORTANT]
> Varsayılan olarak, SQL Veritabanı güvenlik duvarı üzerinden erişim tüm Azure hizmetleri için etkindir. Tüm Azure hizmetleri için devre dışı bırakmak isterseniz bu sayfadaki **KAPALI** öğesine tıklayın.

## <a name="sql-server-connection-information"></a>SQL Server bağlantı bilgileri

Azure SQL Veritabanı sunucunuzun tam sunucu adını Azure portaldan alabilirsiniz. SQL Server Management Studio kullanarak sunucunuza bağlanmak için tam sunucu adını kullanırsınız.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın Azure portal sayfasındaki **Temel Bilgiler** bölmesinde, **Sunucu adını** bulup kopyalayın.

   ![bağlantı bilgileri](./media/sql-database-get-started-portal/server-name.png)

## <a name="connect-to-the-database-with-ssms"></a>Veritabanı SSMS ile bağlanma

Kullanım [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) Azure SQL Database sunucunuza bağlantı kuramıyor.

1. SQL Server Management Studio’yu açın.

2. **Sunucuya Bağlan** iletişim kutusuna şu bilgileri girin:

   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Sunucu türü | Veritabanı altyapısı | Bu değer gereklidir |
   | Sunucu adı | Tam sunucu adı | Adı, şunun gibi olmalıdır: **mynewserver20170824.database.windows.net**. |
   | Kimlik Doğrulaması | SQL Server Kimlik Doğrulaması | Bu öğreticide yapılandırdığımız tek kimlik doğrulaması türü SQL Kimlik Doğrulamasıdır. |
   | Oturum Aç | Sunucu yöneticisi hesabı | Bu, sunucuyu oluştururken belirttiğiniz hesaptır. |
   | Parola | Sunucu yöneticisi hesabınızın parolası | Bu, sunucuyu oluştururken belirttiğiniz paroladır. |

   ![sunucuya bağlan](./media/sql-database-connect-query-ssms/connect.png)

3. **Sunucuya bağlan** iletişim kutusunda **Seçenekler**’e tıklayın. **Veritabanına bağlan** bölümünde bu veritabanına bağlanmak için **mySampleDatabase** yazın.

   ![sunucuda veritabanına bağlanma](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. **Bağlan**'a tıklayın. SSMS’te Nesne Gezgini penceresi açılır. 

5. Nesne Gezgini’nde **Veritabanları**’nı ve ardından **mySampleDatabase** öğesini genişleterek nesneleri örnek veritabanında görüntüleyin.

   ![Veritabanı nesneleri](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="create-tables-in-the-database"></a>Veritabanında tabloları oluşturma 

Bir öğrenci yönetimi sistemi kullanarak üniversiteler için model dört tablolar ile bir veritabanı şeması oluşturma [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference):

- Kişi
- Ders
- Öğrenci
- Bu model bir öğrenci yönetim sistemi üniversiteler için kredi

Aşağıdaki diyagramda bu tablolar birbirleriyle nasıl ilişkili olduğunu gösterir. Bu tablolar bazıları diğer tablolardaki sütunlara başvuru. Örneğin, Öğrenci tabloya başvuruyorsa **PersonId** sütunu **kişi** tablo. Bu öğretici tablolarda birbirleriyle nasıl ilişkili olduğunu anlamak için diyagram üzerinde çalışın. Etkin veritabanı tabloları oluşturma ilişkin ayrıntılı bir bakış için bkz: [etkili veritabanı tabloları oluşturma](https://msdn.microsoft.com/library/cc505842.aspx). Veri türleri seçme hakkında daha fazla bilgi için bkz: [veri türleri](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql).

> [!NOTE]
> De kullanabilirsiniz [Tablo Tasarımcısı'nda SQL Server Management Studio](https://msdn.microsoft.com/library/hh272695.aspx) oluşturma ve tabloları tasarlayın. 

![Tablo ilişkileri](./media/sql-database-design-first-database/tutorial-database-tables.png)

1. Nesne Gezgini’nde **mySampleDatabase** öğesine sağ tıklayıp **Yeni Sorgu**’ya tıklayın. Veritabanınıza bağlı boş bir sorgu penceresi açılır.

2. Sorgu penceresinde, dört tablonun veritabanınızda oluşturmak için aşağıdaki sorguyu çalıştırın: 

   ```sql 
   -- Create Person table

   CREATE TABLE Person
   (
   PersonId   INT IDENTITY PRIMARY KEY,
   FirstName   NVARCHAR(128) NOT NULL,
   MiddelInitial NVARCHAR(10),
   LastName   NVARCHAR(128) NOT NULL,
   DateOfBirth   DATE NOT NULL
   )
   
   -- Create Student table
 
   CREATE TABLE Student
   (
   StudentId INT IDENTITY PRIMARY KEY,
   PersonId  INT REFERENCES Person (PersonId),
   Email   NVARCHAR(256)
   )
   
   -- Create Course table
 
   CREATE TABLE Course
   (
   CourseId  INT IDENTITY PRIMARY KEY,
   Name   NVARCHAR(50) NOT NULL,
   Teacher   NVARCHAR(256) NOT NULL
   ) 

   -- Create Credit table
 
   CREATE TABLE Credit
   (
   StudentId   INT REFERENCES Student (StudentId),
   CourseId   INT REFERENCES Course (CourseId),
   Grade   DECIMAL(5,2) CHECK (Grade <= 100.00),
   Attempt   TINYINT,
   CONSTRAINT  [UQ_studentgrades] UNIQUE CLUSTERED
   (
   StudentId, CourseId, Grade, Attempt
   )
   )
   ```

   ![Tabloları oluşturma](./media/sql-database-design-first-database/create-tables.png)

3. Oluşturduğunuz tabloları görmek için SQL Server Management Studio nesne Gezgini'nde 'tablolar' düğümünü genişletin.

   ![SSMS tablolarının oluşturulması](./media/sql-database-design-first-database/ssms-tables-created.png)

## <a name="load-data-into-the-tables"></a>Veri tablolarına yükleme

1. Adlı bir klasör oluşturun **SampleTableData** yüklemeleri klasörünüzdeki veritabanınız için örnek verileri saklamak için. 

2. Aşağıdaki bağlantılar sağ tıklayın ve bunların içine Kaydet **SampleTableData** klasör. 

   - [SampleCourseData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCourseData)
   - [SamplePersonData](https://sqldbtutorial.blob.core.windows.net/tutorials/SamplePersonData)
   - [SampleStudentData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleStudentData)
   - [SampleCreditData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCreditData)

3. Bir komut istemi penceresi açın ve SampleTableData klasöre gidin.

4. Örnek veri değerlerini değiştirme tabloları eklemek için aşağıdaki komutları yürütün **ServerName**, **DatabaseName**, **kullanıcıadı**, ve  **Parola** ortamınız için değerlere sahip.
  
   ```bcp
   bcp Course in SampleCourseData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Person in SamplePersonData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Student in SampleStudentData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Credit in SampleCreditData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   ```

Ayrıca, daha önce oluşturduğunuz tablolara şimdi örnek veri yüklemiş olduğunuz.

## <a name="query-data"></a>Verileri sorgulama

Veritabanı tablolarından bilgi almak için aşağıdaki sorguları yürütün. Bkz: [SQL sorguları yazma](https://technet.microsoft.com/library/bb264565.aspx) SQL sorguları yazma hakkında daha fazla bilgi için. İlk sorguyu kendi sınıfında 75 %'den daha yüksek bir düzeyde olan ' tarafından Dominick Pope' öğrettin tüm Öğrenciler bulmak için dört tablonun tamamını birleştirir. İkinci sorguyu dört tablonun tamamını birleştirir ve 'Barış Coleman' herhangi bir zamanda kaydolduğu tüm kursları bulur.

1. Bir SQL Server Management Studio sorgu penceresinde, aşağıdaki sorguyu çalıştırın:

   ```sql 
   -- Find the students taught by Dominick Pope who have a grade higher than 75%

   SELECT  person.FirstName,
   person.LastName,
   course.Name,
   credit.Grade
   FROM  Person AS person
   INNER JOIN Student AS student ON person.PersonId = student.PersonId
   INNER JOIN Credit AS credit ON student.StudentId = credit.StudentId
   INNER JOIN Course AS course ON credit.CourseId = course.courseId
   WHERE course.Teacher = 'Dominick Pope' 
   AND Grade > 75
   ```

2. Bir SQL Server Management Studio sorgu penceresinde, sorguyu çalıştırın:

   ```sql
   -- Find all the courses in which Noe Coleman has ever enrolled

   SELECT  course.Name,
   course.Teacher,
   credit.Grade
   FROM  Course AS course
   INNER JOIN Credit AS credit ON credit.CourseId = course.CourseId
   INNER JOIN Student AS student ON student.StudentId = credit.StudentId
   INNER JOIN Person AS person ON person.PersonId = student.PersonId
   WHERE person.FirstName = 'Noe'
   AND person.LastName = 'Coleman'
   ```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Bir veritabanını daha önceki bir noktaya geri yükleme

Yanlışlıkla bir tabloyu sildiniz düşünün. Bu, kolayca kurtaramazsınız şeydir. Azure SQL veritabanı-35 gün son yukarı zamanda herhangi bir noktaya kadar geri dönün ve zaman yeni bir veritabanına geri yükleme bu noktası sağlar. Silinen verilerinizi kurtarmak için bu veritabanını kullanabilirsiniz. Tabloları eklenmeden önce aşağıdaki adımları örnek veritabanını bir noktaya geri yükleme.

1. Veritabanınız için SQL veritabanı sayfasında, tıklatın **geri** araç çubuğunda. **Geri** sayfası açılır.

   ![Geri yükleme](./media/sql-database-design-first-database/restore.png)

2. Doldurmak **geri** form gerekli bilgileri:
    * Veritabanı adı: Bir veritabanı adı sağlayın 
    * Noktası zamanında: Seçin **noktası zaman** geri yükleme formundaki sekmesi 
    * Geri yükleme noktası: veritabanı değiştirilmeden önce oluşan bir süre seçin
    * Hedef sunucu: bir veritabanı geri yüklenirken bu değeri değiştirilemiyor 
    * Esnek veritabanı havuzu: seçin **yok**  
    * Fiyatlandırma katmanı: seçin **20 Dtu'lar** ve **40 GB** depolama.

   ![geri yükleme noktası](./media/sql-database-design-first-database/restore-point.png)

3. Tıklatın **Tamam** veritabanına geri yükleme [zaman içinde bir noktaya geri](sql-database-recovery-using-backups.md#point-in-time-restore) tabloları eklenmeden önce. Bir veritabanını zaman içinde farklı bir noktaya geri yükleme oluşturur yinelenen bir veritabanı noktası itibariyle özgün veritabanı ile aynı sunucuda, belirttiğiniz süre için saklama dönemi içinde olduğu sürece, [hizmet katmanı](sql-database-service-tiers.md).

## <a name="next-steps"></a>Sonraki adımlar 
Bu öğreticide, temel veritabanı görevlerini gibi bir veritabanı ve tablo oluşturma hakkında bilgi edindiniz, yükleme ve verileri sorgulamak ve veritabanını zaman içinde daha önceki bir noktaya geri. Şunları öğrendiniz:
> [!div class="checklist"]
> * Veritabanı oluşturma
> * Bir güvenlik duvarı kuralı ayarlamak
> * Veritabanı ile bağlanmak [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS)
> * Tabloları oluşturma
> * Yığın veri yükleme
> * Bu verileri Sorgulama
> * SQL veritabanı kullanarak zaman içinde daha önceki bir noktaya veritabanını geri [geri yükleme noktası](sql-database-recovery-using-backups.md#point-in-time-restore) özellikleri

Visual Studio ve C# kullanarak veritabanını tasarlama hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
>[Bir Azure SQL veritabanı tasarlama ve C# ve ADO.NET bağlantı](sql-database-design-first-database-csharp.md)
