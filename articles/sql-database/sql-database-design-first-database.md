---
title: 'Öğretici: SSMS kullanarak ilk Azure SQL veritabanınızı tasarlama | Microsoft Docs'
description: SQL Server Management Studio ile ilk Azure SQL veritabanınızı tasarlamayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.topic: tutorial
author: CarlRabeler
ms.author: carlrab
ms.reviewer: v-masebo
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 9fa36b9b87a8e9591b0c863826cd2278a29ba28e
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956066"
---
# <a name="tutorial-design-your-first-azure-sql-database-using-ssms"></a>Öğretici: SSMS kullanarak ilk Azure SQL veritabanınızı tasarlama

Azure SQL veritabanı, bir ilişkisel veritabanı olarak-hizmet (DBaaS), Microsoft Cloud (Azure) ' dir. Bu öğreticide, Azure portalını ve [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx)'yu (SSMS) kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

> [!div class="checklist"]
> * Azure portalında veritabanı oluşturma*
> * Azure portalında sunucu düzeyinde güvenlik duvarı kuralı ayarlama
> * SSMS ile veritabanına bağlanma
> * SSMS ile tablo oluşturma
> * BCP ile toplu veri yükleme
> * SSMS ile veri sorgulama

* Azure aboneliğiniz yoksa, [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

> [!NOTE]
> Bu öğreticinin amaçları doğrultusunda, [DTU tabanlı satın alma modelini](sql-database-service-tiers-dtu.md) kullanıyoruz ancak [Sanal çekirdek tabanlı satın alma modelini ](sql-database-service-tiers-vcore.md) seçme olanağınız da vardır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için yüklediğiniz emin olun:

- [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (en son sürüm)
- [BCP ve SQLCMD](https://www.microsoft.com/download/details.aspx?id=36433) (en son sürüm)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-blank-database"></a>Boş veritabanı oluşturma

Azure SQL veritabanı bir dizi [işlem ve depolama kaynağı](sql-database-service-tiers-dtu.md) ile oluşturulur. Veritabanı içinde oluşturulan bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ve bir [Azure SQL veritabanı mantıksal sunucusu](sql-database-features.md).

Boş bir SQL veritabanı oluşturmak için aşağıdaki adımları izleyin.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.

1. **Yeni** sayfasında, Azure Market bölümünde **Veritabanları**’nı seçin ve ardından **Öne Çıkan** bölümünde **SQL Veritabanı**’na tıklayın.

   ![create empty-database](./media/sql-database-design-first-database/create-empty-database.png)

   1. Doldurun **SQL veritabanı** önceki görüntüde gösterildiği gibi aşağıdaki bilgilerle oluşturur:

      | Ayar       | Önerilen değer | Açıklama |
      | ------------ | ------------------ | ------------------------------------------------- |
      | **Veritabanı adı** | *Veritabanınız* | Geçerli veritabanı adları için bkz [veritabanı tanımlayıcıları](/sql/relational-databases/databases/database-identifiers). |
      | **Abonelik** | *yourSubscription*  | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
      | **Kaynak grubu** | *yourResourceGroup* | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/naming-conventions). |
      | **Kaynak seçme** | Boş veritabanı | Boş bir veritabanı oluşturulması gerektiğini belirtir. |

   1. Tıklayın **sunucu** varolan sunucusu kullanmak veya oluşturmaya ve veritabanınız için yeni bir sunucu yapılandırın. Sunucu seçin veya tıklatın **yeni sunucu oluştur** doldurun **yeni sunucu** formunu aşağıdaki bilgilerle:

      | Ayar       | Önerilen değer | Açıklama |
      | ------------ | ------------------ | ------------------------------------------------- |
      | **Sunucu adı** | Genel olarak benzersiz bir ad | Geçerli sunucu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/naming-conventions). |
      | **Sunucu yöneticisi oturum açma bilgileri** | Geçerli bir ad | Geçerli oturum açma adları için bkz [veritabanı tanımlayıcıları](/sql/relational-databases/databases/database-identifiers). |
      | **Parola** | Geçerli bir parola | Parolanız en az 8 karakter bulunmalı ve şu kategorilerin üçünden karakterler kullanmanız gerekir: büyük harf karakterler, küçük harf karakterler, sayılar ve alfasayısal olmayan karakter. |
      | **Konum** | Geçerli bir konum | Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/). |

      ![create database-server](./media/sql-database-design-first-database/create-database-server.png)

      **Seç**'e tıklayın.

   1. Hizmet katmanını, DTU veya sanal çekirdek sayısını ve depolama alanı miktarını belirtmek için **Fiyatlandırma katmanı**’na tıklayın. Dtu/sanal çekirdek ve her hizmet katmanı için kullanılabilir olan depolama sayısı seçeneklerini araştırın. Varsayılan olarak, **standart** [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) seçilir, ancak seçme seçeneğiniz [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).

      > [!IMPORTANT]
      > 1 TB'den fazla depolama Premium katmanında şu anda aşağıdakiler dışındaki tüm bölgelerde: UK Kuzey Batı Orta ABD, Birleşik Krallık South2, Çin Doğu, USDoDCentral, Almanya Orta, USDoDEast, ABD Devleti Southwest, ABD Devleti Güney Orta, Almanya Kuzeydoğu, Çin Kuzey, ABD Devleti Doğu. Diğer bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır. Bkz. [P11 P15 Geçerli Sınırlamalar]( sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).

      Hizmet katmanını, dtu'ların sayısını ve depolama miktarını seçtikten sonra **Uygula**.

   1. Girin bir **harmanlama** boş bir veritabanı için (Bu öğretici için varsayılan değeri kullanın). Harmanlamalar hakkında daha fazla bilgi için bkz. [Harmanlamalar](/sql/t-sql/statements/collations)

1. Tamamladığınıza göre şimdi **SQL veritabanı** formunda, tıklayın **Oluştur** veritabanını sağlamak için. Bu adım birkaç dakika sürebilir.

1. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.

     ![bildirim](./media/sql-database-design-first-database/notification.png)

## <a name="create-a-firewall-rule"></a>Bir güvenlik duvarı kuralı oluşturma

SQL veritabanı hizmeti, sunucu düzeyinde bir güvenlik duvarı oluşturur. Güvenlik Duvarı, dış uygulama ve araçların sunucuya ve sunucu üzerindeki herhangi bir veritabanına bağlanmasını engeller. Veritabanınıza dış bağlantıları etkinleştirmek için güvenlik duvarı IP adresiniz için bir kural eklemelisiniz. Oluşturmak için bu adımları bir [SQL veritabanı sunucu düzeyinde güvenlik duvarı kuralı](sql-database-firewall-configure.md).

> [!NOTE]
> SQL veritabanı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Bir kurumsal ağ içerisinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, yöneticinize 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL veritabanı sunucusuna bağlanamıyor.

1. Dağıtım tamamlandıktan sonra tıklayın **SQL veritabanları** 'a tıklayın ve sol taraftaki menüden *veritabanınız* üzerinde **SQL veritabanları** sayfası. Veritabanınıza ilişkin genel bakış sayfası açılır ve tam gösteren **sunucu adı** (gibi *yourserver.database.windows.net*) ve daha fazla yapılandırma seçenekleri sağlar.

1. Sunucunuzun ve veritabanları için SQL Server Management Studio'dan bağlanmak için bu tam sunucu adını kopyalayın.

   ![sunucu adı](./media/sql-database-design-first-database/server-name.png)

1. Araç çubuğunda **Sunucu güvenlik duvarını ayarla**’ya tıklayın. **Güvenlik Duvarı ayarları** SQL veritabanı sunucusu için sayfası açılır.

   ![sunucu güvenlik duvarı kuralı](./media/sql-database-design-first-database/server-firewall-rule.png)

   1. Geçerli IP adresinizi yeni bir güvenlik duvarı kuralına eklemek için araç çubuğunda **İstemci IP’si Ekle** öğesine tıklayın. Güvenlik duvarı kuralı, 1433 numaralı bağlantı noktasını tek bir IP adresi veya bir IP adresi aralığı için açabilir.

   1. **Kaydet**’e tıklayın. Geçerli IP adresiniz için mantıksal sunucuda 1433 numaralı bağlantı noktası açılarak sunucu düzeyinde güvenlik duvarı kuralı oluşturulur.

   1. **Tamam**’a tıklayın ve sonra **Güvenlik duvarı ayarları** sayfasını kapatın.

IP adresiniz, artık güvenlik duvarı üzerinden geçirebilirsiniz. Şimdi, SQL veritabanı sunucusu ve SQL Server Management Studio veya seçtiğiniz başka bir aracı kullanarak, sunucunun veritabanlarına bağlanabilirsiniz. Daha önce oluşturduğunuz sunucu yönetici hesabı kullandığınızdan emin olun.

> [!IMPORTANT]
> Varsayılan olarak, SQL veritabanı güvenlik duvarı üzerinden erişim tüm Azure Hizmetleri için etkindir. Tüm Azure hizmetleri için devre dışı bırakmak isterseniz bu sayfadaki **KAPALI** öğesine tıklayın.

## <a name="connect-to-the-database"></a>Veritabanı'na bağlanma

Kullanım [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) Azure SQL veritabanı sunucusunda bir bağlantı kurmak için.

1. SQL Server Management Studio’yu açın.

1. **Sunucuya Bağlan** iletişim kutusuna şu bilgileri girin:

   | Ayar       | Önerilen değer | Açıklama |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Sunucu türü** | Veritabanı altyapısı | Bu değer gereklidir. |
   | **Sunucu adı** | Tam sunucu adı | Örneğin, *yourserver.database.windows.net*. |
   | **Kimlik doğrulaması** | SQL Server Kimlik Doğrulaması | SQL kimlik doğrulaması biz Bu öğreticide yapılandırdığınız yalnızca kimlik doğrulaması türüdür. |
   | **Oturum açma** | Sunucu yöneticisi hesabı | Sunucuyu oluştururken belirttiğiniz hesap. |
   | **Parola** | Sunucu yöneticisi hesabınızın parolası | Sunucuyu oluştururken belirttiğiniz parola. |

   ![sunucuya bağlan](./media/sql-database-design-first-database/connect.png)

   1. **Sunucuya bağlan** iletişim kutusunda **Seçenekler**’e tıklayın. İçinde **veritabanına bağlan** bölümünde, girin *veritabanınız* bu veritabanına bağlanmak için.

      ![sunucuda veritabanına bağlanma](./media/sql-database-design-first-database/options-connect-to-db.png)  

   1. **Bağlan**'a tıklayın. **Nesne Gezgini** SSMS'de penceresi açılır.

1. İçinde **Nesne Gezgini**, genişletme **veritabanları** ve ardından *veritabanınız* nesneleri örnek veritabanında görüntüleyin.

   ![veritabanı nesneleri](./media/sql-database-design-first-database/connected.png)  

## <a name="create-tables-in-the-database"></a>Veritabanında tablo oluşturma

[Transact-SQL](/sql/t-sql/language-reference) kullanarak üniversiteler için bir öğrenci yönetim sistemi modelleyen dört tablo ile bir veritabanı şeması oluşturun:

- Kişi
- Ders
- Öğrenci
- Kredi

Aşağıdaki diyagramda bu tabloların birbirleriyle nasıl ilişkili olduğu gösterilmektedir. Bu tablolardan bazıları başka tablolardaki sütunlara başvurur. Örneğin, *Öğrenci* tablo başvuruları *Personıd* sütununun *kişi* tablo. Bu öğreticideki tabloların birbirleriyle ilişkisini anlamak için diyagram üzerinde çalışın. Etkili veritabanı tabloları oluşturmaya ilişkin ayrıntılı bir bakış için bkz. [Etkili veritabanı tabloları oluşturma](https://msdn.microsoft.com/library/cc505842.aspx). Veri türleri seçme hakkında bilgi için bkz. [Veri türleri](/sql/t-sql/data-types/data-types-transact-sql).

> [!NOTE]
> Tablolarınızı oluşturup tasarlamak için [SQL Server Management Studio’daki tablo tasarımcısını](/sql/ssms/visual-db-tools/design-database-diagrams-visual-database-tools) da kullanabilirsiniz.

![Tablo ilişkileri](./media/sql-database-design-first-database/tutorial-database-tables.png)

1. İçinde **Nesne Gezgini**, sağ *veritabanınız* seçip **yeni sorgu**. Veritabanınıza bağlı boş bir sorgu penceresi açılır.

1. Sorgu penceresinde aşağıdaki sorguyu yürüterek veritabanınızda dört tablo oluşturun:

   ```sql
   -- Create Person table
   CREATE TABLE Person
   (
       PersonId INT IDENTITY PRIMARY KEY,
       FirstName NVARCHAR(128) NOT NULL,
       MiddelInitial NVARCHAR(10),
       LastName NVARCHAR(128) NOT NULL,
       DateOfBirth DATE NOT NULL
   )

   -- Create Student table
   CREATE TABLE Student
   (
       StudentId INT IDENTITY PRIMARY KEY,
       PersonId INT REFERENCES Person (PersonId),
       Email NVARCHAR(256)
   )

   -- Create Course table
   CREATE TABLE Course
   (
       CourseId INT IDENTITY PRIMARY KEY,
       Name NVARCHAR(50) NOT NULL,
       Teacher NVARCHAR(256) NOT NULL
   )

   -- Create Credit table
   CREATE TABLE Credit
   (
       StudentId INT REFERENCES Student (StudentId),
       CourseId INT REFERENCES Course (CourseId),
       Grade DECIMAL(5,2) CHECK (Grade <= 100.00),
       Attempt TINYINT,
       CONSTRAINT [UQ_studentgrades] UNIQUE CLUSTERED
       (
           StudentId, CourseId, Grade, Attempt
       )
   )
   ```

   ![Tablo oluşturma](./media/sql-database-design-first-database/create-tables.png)

1. Genişletin **tabloları** düğümünde *veritabanınız* içinde **Nesne Gezgini** oluşturduğunuz tabloları görmek için.

   ![ssms tables-created](./media/sql-database-design-first-database/ssms-tables-created.png)

## <a name="load-data-into-the-tables"></a>Tablolara veri yükleme

1. Adlı bir klasör oluşturun *sampleData* veritabanınızın örnek verilerini depolamak için indirilenler klasörünüzde.

1. Aşağıdaki bağlantılara sağ tıklayın ve kaydedin *sampleData* klasör.

   - [SampleCourseData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCourseData)
   - [SamplePersonData](https://sqldbtutorial.blob.core.windows.net/tutorials/SamplePersonData)
   - [SampleStudentData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleStudentData)
   - [SampleCreditData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCreditData)

1. Bir komut istemi penceresi açın ve gidin *sampleData* klasör.

1. İçin değerleri değiştirerek tablolara örnek veriler eklemek için aşağıdaki komutları yürütün *sunucu*, *veritabanı*, *kullanıcı*, ve *parola* ortamınız için değerlerle.
  
   ```cmd
   bcp Course in SampleCourseData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   bcp Person in SamplePersonData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   bcp Student in SampleStudentData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   bcp Credit in SampleCreditData -S <server>.database.windows.net -d <database> -U <user> -P <password> -q -c -t ","
   ```

Daha önce oluşturduğunuz tablolara örnek veriler yüklediniz.

## <a name="query-data"></a>Verileri sorgulama

Veritabanı tablolarından bilgi almak için aşağıdaki sorguları yürütün. Bkz: [yazma SQL sorguları](https://technet.microsoft.com/library/bb264565.aspx) SQL sorguları yazma hakkında daha fazla bilgi için. İlk sorgu, 75 %'dan daha yüksek bir düzeyde olması ' Dominick Pope' tarafından verilen öğrencileri bulmak için dört tablonun tamamını birleştirir. İkinci sorgu dört tablonun tamamını birleştirir ve 'Noe Coleman' hiç olmadığı kadar kaydetmiştir dersleri bulur.

1. SQL Server Management Studio sorgu penceresinde aşağıdaki sorguyu yürütün:

   ```sql
   -- Find the students taught by Dominick Pope who have a grade higher than 75%
   SELECT  person.FirstName, person.LastName, course.Name, credit.Grade
   FROM  Person AS person
       INNER JOIN Student AS student ON person.PersonId = student.PersonId
       INNER JOIN Credit AS credit ON student.StudentId = credit.StudentId
       INNER JOIN Course AS course ON credit.CourseId = course.courseId
   WHERE course.Teacher = 'Dominick Pope'
       AND Grade > 75
   ```

1. Bir sorgu penceresine aşağıdaki sorguyu yürütün:

   ```sql
   -- Find all the courses in which Noe Coleman has ever enrolled
   SELECT  course.Name, course.Teacher, credit.Grade
   FROM  Course AS course
       INNER JOIN Credit AS credit ON credit.CourseId = course.CourseId
       INNER JOIN Student AS student ON student.StudentId = credit.StudentId
       INNER JOIN Person AS person ON person.PersonId = student.PersonId
   WHERE person.FirstName = 'Noe'
       AND person.LastName = 'Coleman'
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, birçok temel veritabanı görevlerini öğrendiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Veritabanı oluşturma
> * Güvenlik duvarı kuralı ayarlama
> * [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) kullanarak veritabanına bağlanma
> * Tablo oluşturma
> * Toplu veri yükleme
> * Bu verileri sorgulama

Visual Studio ve C# kullanarak veritabanı tasarlama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [C# ve ADO.NET ile bir Azure SQL veritabanı tasarlama ve bağlama](sql-database-design-first-database-csharp.md)
