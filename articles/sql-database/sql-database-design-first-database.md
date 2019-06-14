---
title: "Öğretici: SSMS kullanarak Azure SQL veritabanı'nda ilk ilişkisel veritabanı tasarlama | Microsoft Docs"
description: Tek bir veritabanında SQL Server Management Studio kullanarak Azure SQL veritabanı'nda ilk ilişkisel veritabanınızı tasarlamayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.topic: tutorial
author: stevestein
ms.author: sstein
ms.reviewer: v-masebo
manager: craigg
ms.date: 02/08/2019
ms.openlocfilehash: fc3b1cdfee76bbee7676170fa69a1c53a495dc53
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67051131"
---
# <a name="tutorial-design-a-relational-database-in-a-single-database-within-azure-sql-database-using-ssms"></a>Öğretici: SSMS kullanarak Azure SQL veritabanı içinde tek bir veritabanında ilişkisel veritabanı tasarlama

Azure SQL veritabanı, bir ilişkisel veritabanı olarak-hizmet (DBaaS), Microsoft Cloud (Azure) ' dir. Bu öğreticide, Azure portalını ve [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms)'yu (SSMS) kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

> [!div class="checklist"]
> - Azure portalı * kullanarak tek veritabanı oluşturma
> - Azure portalını kullanarak bir sunucu düzeyinde IP güvenlik duvarı kuralı oluşturma
> - SSMS ile veritabanına bağlanma
> - SSMS ile tablo oluşturma
> - BCP ile toplu veri yükleme
> - SSMS ile veri sorgulama

\* Azure aboneliğiniz yoksa, [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

> [!NOTE]
> Bu öğreticinin amaçları doğrultusunda, tek bir veritabanını kullanıyoruz. Ayrıca, bir elastik havuzdaki havuza alınmış bir veritabanı veya yönetilen bir örneği bir örneği veritabanında da kullanabilirsiniz. Yönetilen örnek bağlantı için bu yönetilen örnek hızlı başlangıçlara bakın: [Hızlı Başlangıç: Bir Azure SQL veritabanı yönetilen örneğine bağlanmak için Azure VM yapılandırma](sql-database-managed-instance-configure-vm.md) ve [hızlı başlangıç: Noktadan siteye bağlantı, şirket içinden Azure SQL veritabanı yönetilen örneği için yapılandırma](sql-database-managed-instance-configure-p2s.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için yüklediğiniz emin olun:

- [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (en son sürüm)
- [BCP ve SQLCMD](https://www.microsoft.com/download/details.aspx?id=36433) (en son sürüm)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-blank-single-database"></a>Boş bir tek veritabanı oluşturma

Azure SQL veritabanı'nda tek bir veritabanı bir dizi işlem ve depolama kaynakları oluşturulur. Veritabanı içinde oluşturulan bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ve kullanarak yönetilen bir [veritabanı sunucusu](sql-database-servers.md).

Boş bir tek veritabanı oluşturmak için aşağıdaki adımları izleyin.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **Yeni** sayfasında, Azure Market bölümünde **Veritabanları**’nı seçin ve ardından **Öne Çıkan** bölümünde **SQL Veritabanı**’na tıklayın.

   ![create empty-database](./media/sql-database-design-first-database/create-empty-database.png)

3. Doldurun **SQL veritabanı** önceki görüntüde gösterildiği gibi aşağıdaki bilgilerle oluşturur:

    | Ayar       | Önerilen değer | Açıklama |
    | ------------ | ------------------ | ------------------------------------------------- |
    | **Veritabanı adı** | *Veritabanınız* | Geçerli veritabanı adları için bkz [veritabanı tanımlayıcıları](/sql/relational-databases/databases/database-identifiers). |
    | **Abonelik** | *yourSubscription*  | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
    | **Kaynak grubu** | *yourResourceGroup* | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/naming-conventions). |
    | **Kaynak seçme** | Boş veritabanı | Boş bir veritabanı oluşturulması gerektiğini belirtir. |

4. Tıklayın **sunucu** mevcut bir veritabanı sunucusunu kullanabilir veya oluşturmaya ve yeni bir veritabanı sunucusunu yapılandırın. Mevcut bir sunucu seçin veya tıklatın **yeni sunucu oluştur** doldurun **yeni sunucu** formunu aşağıdaki bilgilerle:

    | Ayar       | Önerilen değer | Açıklama |
    | ------------ | ------------------ | ------------------------------------------------- |
    | **Sunucu adı** | Genel olarak benzersiz bir ad | Geçerli sunucu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/naming-conventions). |
    | **Sunucu yöneticisi oturum açma bilgileri** | Geçerli bir ad | Geçerli oturum açma adları için bkz [veritabanı tanımlayıcıları](/sql/relational-databases/databases/database-identifiers). |
    | **Parola** | Geçerli bir parola | Parolanız en az 8 karakter bulunmalı ve şu kategorilerin üçünden karakterler kullanmanız gerekir: büyük harf karakterler, küçük harf karakterler, sayılar ve alfasayısal olmayan karakter. |
    | **Konum** | Geçerli bir konum | Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/). |

    ![create database-server](./media/sql-database-design-first-database/create-database-server.png)

5. **Seç**'e tıklayın.
6. Hizmet katmanını, DTU veya sanal çekirdek sayısını ve depolama alanı miktarını belirtmek için **Fiyatlandırma katmanı**’na tıklayın. Dtu/sanal çekirdek ve her hizmet katmanı için kullanılabilir olan depolama sayısı seçeneklerini araştırın.

    Hizmet katmanını, Dtu veya sanal çekirdek sayısı ve depolama alanı miktarını seçtikten sonra **Uygula**.

7. Girin bir **harmanlama** boş bir veritabanı için (Bu öğretici için varsayılan değeri kullanın). Harmanlamalar hakkında daha fazla bilgi için bkz. [Harmanlamalar](/sql/t-sql/statements/collations)

8. Tamamladığınıza göre şimdi **SQL veritabanı** formunda, tıklayın **Oluştur** tek veritabanı sağlanması. Bu adım birkaç dakika sürebilir.

9. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.

   ![bildirim](./media/sql-database-design-first-database/notification.png)

## <a name="create-a-server-level-ip-firewall-rule"></a>Sunucu düzeyinde IP güvenlik duvarı kuralı oluşturma

SQL veritabanı hizmeti, sunucu düzeyinde bir IP Güvenlik Duvarı oluşturur. Bu güvenlik duvarı dış uygulama ve araçların bir güvenlik duvarı kuralı kendi IP Güvenlik Duvarı üzerinden izin vermesi dışında sunucuya ve sunucu üzerindeki herhangi bir veritabanına bağlanmasını engeller. Tek veritabanı dış bağlantıları etkinleştirmek için ilk IP adresini (veya IP adresi aralığı) için bir IP güvenlik duvarı kuralı eklemeniz gerekir. Oluşturmak için bu adımları bir [SQL veritabanı sunucu düzeyinde IP güvenlik duvarı kuralı](sql-database-firewall-configure.md).

> [!IMPORTANT]
> SQL veritabanı hizmeti, 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Bu hizmetine bir kurumsal ağ içinden bağlanmaya çalışıyorsanız, 1433 numaralı bağlantı noktası üzerinden giden trafiğe ağınızın güvenlik duvarı tarafından izin verilmiyor. Yöneticiniz 1433 numaralı bağlantı noktasını açmadığı sürece bu durumda, tek bir veritabanına bağlanamıyor.

1. Dağıtım tamamlandıktan sonra tıklayın **SQL veritabanları** 'a tıklayın ve sol taraftaki menüden *veritabanınız* üzerinde **SQL veritabanları** sayfası. Veritabanınıza ilişkin genel bakış sayfası açılır ve tam gösteren **sunucu adı** (gibi *yourserver.database.windows.net*) ve daha fazla yapılandırma seçenekleri sağlar.

2. Sunucunuzun ve veritabanları için SQL Server Management Studio'dan bağlanmak için bu tam sunucu adını kopyalayın.

   ![sunucu adı](./media/sql-database-design-first-database/server-name.png)

3. Araç çubuğunda **Sunucu güvenlik duvarını ayarla**’ya tıklayın. SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır.

   ![sunucu düzeyinde IP güvenlik duvarı kuralı](./media/sql-database-design-first-database/server-firewall-rule.png)

4. Tıklayın **istemci IP'si Ekle** geçerli IP adresinizi yeni bir IP güvenlik duvarı kuralına eklemek için araç çubuğunda. Bir IP güvenlik duvarı kuralı, tek bir IP adresi veya bir IP adresi aralığı için 1433 numaralı bağlantı noktasını açabilirsiniz.

5. **Kaydet**’e tıklayın. SQL veritabanı sunucusu üzerindeki 1433 numaralı bağlantı noktasını açma geçerli IP adresiniz için sunucu düzeyinde IP güvenlik duvarı kuralı oluşturulur.

6. **Tamam**’a tıklayın ve sonra **Güvenlik duvarı ayarları** sayfasını kapatın.

IP adresiniz, artık IP Güvenlik Duvarı üzerinden geçirebilirsiniz. Artık SQL Server Management Studio veya seçtiğiniz başka bir aracı kullanarak tek veritabanına bağlanabilirsiniz. Daha önce oluşturduğunuz sunucu yönetici hesabı kullandığınızdan emin olun.

> [!IMPORTANT]
> Varsayılan olarak, SQL veritabanı IP Güvenlik Duvarı üzerinden erişim tüm Azure Hizmetleri için etkindir. Tüm Azure hizmetleri için devre dışı bırakmak isterseniz bu sayfadaki **KAPALI** öğesine tıklayın.

## <a name="connect-to-the-database"></a>Veritabanına bağlanın

Kullanım [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) veritabanınızı tek bir bağlantı kurmak için.

1. SQL Server Management Studio’yu açın.
2. **Sunucuya Bağlan** iletişim kutusuna şu bilgileri girin:

   | Ayar       | Önerilen değer | Açıklama |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Sunucu türü** | Veritabanı altyapısı | Bu değer gereklidir. |
   | **Sunucu adı** | Tam sunucu adı | Örneğin, *yourserver.database.windows.net*. |
   | **Kimlik Doğrulaması** | SQL Server Kimlik Doğrulaması | SQL kimlik doğrulaması biz Bu öğreticide yapılandırdığınız yalnızca kimlik doğrulaması türüdür. |
   | **Oturum açma** | Sunucu yöneticisi hesabı | Sunucuyu oluştururken belirttiğiniz hesap. |
   | **Parola** | Sunucu yöneticisi hesabınızın parolası | Sunucuyu oluştururken belirttiğiniz parola. |

   ![sunucuya bağlan](./media/sql-database-design-first-database/connect.png)

3. **Sunucuya bağlan** iletişim kutusunda **Seçenekler**’e tıklayın. İçinde **veritabanına bağlan** bölümünde, girin *veritabanınız* bu veritabanına bağlanmak için.

    ![sunucuda veritabanına bağlanma](./media/sql-database-design-first-database/options-connect-to-db.png)  

4. **Bağlan**'a tıklayın. **Nesne Gezgini** SSMS'de penceresi açılır.

5. İçinde **Nesne Gezgini**, genişletme **veritabanları** ve ardından *veritabanınız* nesneleri örnek veritabanında görüntüleyin.

   ![veritabanı nesneleri](./media/sql-database-design-first-database/connected.png)  

## <a name="create-tables-in-your-database"></a>Veritabanınızdaki tablolar oluşturma

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

2. Sorgu penceresinde aşağıdaki sorguyu yürüterek veritabanınızda dört tablo oluşturun:

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

3. Genişletin **tabloları** düğümünde *veritabanınız* içinde **Nesne Gezgini** oluşturduğunuz tabloları görmek için.

   ![ssms tables-created](./media/sql-database-design-first-database/ssms-tables-created.png)

## <a name="load-data-into-the-tables"></a>Tablolara veri yükleme

1. Adlı bir klasör oluşturun *sampleData* veritabanınızın örnek verilerini depolamak için indirilenler klasörünüzde.

2. Aşağıdaki bağlantılara sağ tıklayın ve kaydedin *sampleData* klasör.

   - [SampleCourseData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCourseData)
   - [SamplePersonData](https://sqldbtutorial.blob.core.windows.net/tutorials/SamplePersonData)
   - [SampleStudentData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleStudentData)
   - [SampleCreditData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCreditData)

3. Bir komut istemi penceresi açın ve gidin *sampleData* klasör.

4. İçin değerleri değiştirerek tablolara örnek veriler eklemek için aşağıdaki komutları yürütün *sunucu*, *veritabanı*, *kullanıcı*, ve *parola* ortamınız için değerlerle.

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

2. Bir sorgu penceresine aşağıdaki sorguyu yürütün:

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
> - Tek veritabanı oluşturma
> - Bir sunucu düzeyinde IP güvenlik duvarı kuralı ayarlama
> - [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) kullanarak veritabanına bağlanma
> - Tablo oluşturma
> - Toplu veri yükleme
> - Bu verileri sorgulama

Visual Studio ve C# kullanarak veritabanı tasarlama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure SQL veritabanı içinde tek bir veritabanında ilişkisel veritabanı tasarlama C# ve ADO.NET](sql-database-design-first-database-csharp.md)
