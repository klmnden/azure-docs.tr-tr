---
title: Azure SQL veritabanı yönetilen örneği güvenliği Azure AD sunucusu (oturum açma bilgileri) prensiplerinin | Microsoft Docs
description: Teknikleri ve güvenli bir Azure SQL veritabanı yönetilen örneği için özellikleri hakkında bilgi edinin ve Azure AD sunucu sorumlusu (oturum açma bilgileri) kullanın
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: tutorial
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
manager: craigg
ms.date: 02/20/2019
ms.openlocfilehash: 5d168264cbc392e1ba426707429f47dea70d1ea8
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58882064"
---
# <a name="tutorial-managed-instance-security-in-azure-sql-database-using-azure-ad-server-principals-logins"></a>Öğretici: Azure AD sunucu sorumlusu (oturum açma) kullanarak Azure SQL veritabanı yönetilen örnek güvenlik

Yönetilen örnek, en son SQL Server (Enterprise Edition) veritabanı altyapısı olan şirket neredeyse tüm güvenlik özellikleri sağlar:

- Yalıtılmış bir ortamda erişimi sınırlandırma
- Kimlik (Azure AD, SQL kimlik doğrulaması) gerektiren kimlik doğrulama mekanizmaları kullanma
- Rol tabanlı üyelikler ve izinler ile yetkilendirme kullanın.
- Güvenlik özelliklerini etkinleştirme

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> - Bir Azure Active Directory (AD) sunucusu asıl (oturum açma) için bir yönetilen örnek oluşturma
> - Azure AD sunucusu Sorumlular (oturum açma bilgileri) yönetilen örnek izinleri
> - Azure AD kullanıcıları Azure AD sunucusu ilkeleri (oturum açma bilgileri) oluşturun.
> - Azure AD kullanıcıları için izinleri atayın ve veritabanı güvenliğini yönetme
> - Azure AD kullanıcılarla kimliğe bürünme kullanma
> - Veritabanları arası sorgular ile Azure AD kullanıcılarını kullanın.
> - Tehdit koruması, Denetim, veri maskeleme ve şifreleme gibi güvenlik özellikleri hakkında bilgi edinin

> [!NOTE]
> Yönetilen örnek için Azure AD sunucusu ilkeleri (oturum açma bilgileri) olan **genel Önizleme**.

Daha fazla bilgi için bkz. [Azure SQL veritabanı yönetilen örneği'ne genel bakış](sql-database-managed-instance-index.yml) ve [özellikleri](sql-database-managed-instance.md) makaleler.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) (SSMS)
- Bir Azure SQL veritabanı yönetilen örneği
  - Bu makaleyi izleyin: [Hızlı Başlangıç: Bir Azure SQL veritabanı yönetilen örneği oluşturma](sql-database-managed-instance-get-started.md)
- Yönetilen Örneğinize erişmek kullanabilir ve [yönetilen örnek için bir Azure AD Yöneticisi sağlanan](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance). Daha fazla bilgi için bkz:
    - [Yönetilen örnek uygulamanızı bağlayın](sql-database-managed-instance-connect-app.md) 
    - [Yönetilen örnek bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md)
    - [Yapılandırma ve Azure Active Directory kimlik doğrulaması SQL ile yönetme](sql-database-aad-authentication-configure.md)

## <a name="limiting-access-to-your-managed-instance"></a>Yönetilen Örneğinize erişimi sınırlandırma

Yönetilen örnekleri yalnızca özel bir IP adresi erişilebilir. Yönetilen örnek Ağ dışından bir yönetilen örneğine bağlanmak kullanılabilir olan hiçbir hizmet uç noktaları vardır. Çok yalıtılmış bir SQL Server ortamı, uygulamalara veya kullanıcılara şirket gibi bir bağlantı önce yönetilen örnek ağa (VNet) erişim kurulabilir. Daha fazla bilgi için şu makaleye bakın [uygulamanızın yönetilen örneğe bağlanma](sql-database-managed-instance-connect-app.md).

> [!NOTE] 
> Yönetilen örnekleri yalnızca kendi sanal ağ içinde erişilebildiğinden [SQL veritabanı güvenlik duvarı kuralları](sql-database-firewall-configure.md) geçerli değildir. Yönetilen örnek sahip kendi [yerleşik güvenlik duvarı](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).

## <a name="create-an-azure-ad-server-principal-login-for-a-managed-instance-using-ssms"></a>Bir Azure AD sunucusu asıl (oturum açma) için SSMS kullanarak bir yönetilen örnek oluşturma

İlk Azure AD sunucusu (oturum açma) asıl standart SQL Server hesabı tarafından (azure dışı AD) oluşturulmalıdır bir `sysadmin`. Örnekleri, yönetilen Örneğinize bağlanmak için aşağıdaki makalelere bakın:

- [Hızlı Başlangıç: Yönetilen bir örneğine bağlanmak için Azure sanal makinesi yapılandırma](sql-database-managed-instance-configure-vm.md)
- [Hızlı Başlangıç: Noktadan siteye bağlantı için bir yönetilen örnek şirket içinden yapılandırın.](sql-database-managed-instance-configure-p2s.md)

> [!IMPORTANT]
> Yönetilen örnek'kurmak için kullanılan Azure AD Yöneticisi, bir Azure AD sorumlusu (oturum açma) içinde sunucusu yönetilen örnek oluşturmak için kullanılamaz. İlk Azure AD sunucu sorumlusu (oturum açma) kullanarak bir SQL Server hesabı oluşturmalısınız bir `sysadmin`. Azure AD sunucu sorumlusu (oturum açma bilgileri) büyüyecek haline sonra kaldırılacak geçici bir sınırlama budur. Oturumu oluşturmak için bir Azure AD yönetici hesabı kullanmayı denerseniz aşağıdaki hatayı görürsünüz: `Msg 15247, Level 16, State 1, Line 1 User does not have permission to perform this action.`

1. Yönetilen Örneğinize olan standart bir SQL Server hesabı (azure dışı AD) kullanarak oturum bir `sysadmin`kullanarak [SQL Server Management Studio](sql-database-managed-instance-configure-p2s.md#use-ssms-to-connect-to-the-managed-instance).

2. İçinde **Nesne Gezgini**, sunucuya sağ tıklayın ve seçin **yeni sorgu**.

3. Sorgu penceresinde, bir oturum açma için yerel bir oluşturmak için aşağıdaki sözdizimini kullanın. Azure AD hesabı:

    ```sql
    USE master
    GO
    CREATE LOGIN login_name FROM EXTERNAL PROVIDER
    GO
    ```

    Bu örnek, bir oturum açma hesabı oluşturur. nativeuser@aadsqlmi.onmicrosoft.com.

    ```sql
    USE master
    GO
    CREATE LOGIN [nativeuser@aadsqlmi.onmicrosoft.com] FROM EXTERNAL PROVIDER
    GO
    ```

4. Araç çubuğunda **yürütme** oturumu oluşturmak için.

5. Aşağıdaki T-SQL komutunu çalıştırarak yeni eklenen oturum açma denetleyin:

    ```sql
    SELECT *  
    FROM sys.server_principals;  
    GO
    ```

    ![Yerel login.png](media/sql-database-managed-instance-security-tutorial/native-login.png)

Daha fazla bilgi için [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current).

## <a name="granting-permissions-to-allow-the-creation-of-managed-instance-logins"></a>Yönetilen örnek oturum açma bilgileri oluşturulmasına izin vermek için izinleri veriliyor

Diğer Azure AD sunucu sorumlusu (oturum açma bilgileri) oluşturmak için SQL Server rollerini veya izinleri sorumlusuna (SQL veya Azure AD) verilmelidir.

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

- Oturum açma SQL sorumlunun parçası olan oturumları olup olmadığını `sysadmin` rol için bir Azure AD hesabı oluştur oturum açma için create komutu kullanabilirsiniz.

### <a name="azure-ad-authentication"></a>Azure AD kimlik doğrulaması

- Yeni oluşturulan Azure AD, asıl sunucu (oturum açma) diğer Azure AD kullanıcıları, grupları ve uygulamaları için başka bir oturum açma bilgisi oluşturma olanağı izin vermek için oturum açma izni `sysadmin` veya `securityadmin` sunucu rolü. 
- En azından **ALTER ANY LOGIN** diğer Azure AD sunucusu ilkeleri (oturum açma bilgileri) oluşturmak için Azure AD sunucu sorumlusuna (oturum açma) izin verilmelidir. 
- Varsayılan olarak, yeni oluşturulan için Azure AD sunucusu ilkeleri (oturum açma bilgileri) ana dalda standart izni: **SQL bağlanma** ve **herhangi bir veritabanını GÖRÜNTÜLEMEK**.
- `sysadmin` Sunucu rolü, yönetilen örneği içinde birçok Azure AD sunucu sorumlusu (oturum açma bilgileri) verilebilir.

Oturum açma eklemek için `sysadmin` sunucu rolü:

1. Yeniden oturum yönetilen örneğine veya SQL sorumlusu ile var olan bağlantıyı kullanın bir `sysadmin`.

1. İçinde **Nesne Gezgini**, sunucuya sağ tıklayın ve seçin **yeni sorgu**.

1. Asıl sunucu Azure AD (oturum açma) verme `sysadmin` aşağıdaki T-SQL söz dizimini kullanarak sunucu rolü:

    ```sql
    ALTER SERVER ROLE sysadmin ADD MEMBER login_name
    GO
    ```

    Aşağıdaki örnek verir `sysadmin` oturum açmak için sunucu rolü nativeuser@aadsqlmi.onmicrosoft.com

    ```sql
    ALTER SERVER ROLE sysadmin ADD MEMBER [nativeuser@aadsqlmi.onmicrosoft.com]
    GO
    ```

## <a name="create-additional-azure-ad-server-principals-logins-using-ssms"></a>SSMS'yi kullanarak sunucu ilkeleri (oturum açma bilgileri) başka bir Azure AD'miz oluşturma

Asıl sunucu Azure AD (oturum açma) oluşturuldu ve sağlanan sonra `sysadmin` ayrıcalıkları, oturum açma kullanarak ek oturumlar oluşturabilirsiniz **gelen dış sağlayıcı** yan tümcesiyle **CREATE LOGIN**.

1. Azure AD sunucu asıl (oturum açma) ile SQL Server Management Studio kullanarak yönetilen örneğe bağlanın. Yönetilen örnek ana bilgisayar adınızı girin. Ssms'de kimlik doğrulaması için bir Azure AD hesabıyla oturum açarken seçmek için üç seçenek vardır:

   - Active Directory - MFA desteğiyle Evrensel
   - Active Directory - Parola
   - Active Directory - Tümleşik </br>

     ![SSMS oturum açma prompt.png](media/sql-database-managed-instance-security-tutorial/ssms-login-prompt.png)

     Daha fazla bilgi için şu makaleye bakın: [SQL Veritabanı ve SQL Veri Ambarı ile Evrensel Kimlik Doğrulaması (MFA için SSMS desteği)](sql-database-ssms-mfa-authentication.md)

1. Seçin **Active Directory - MFA desteğiyle Evrensel**. Bu, bir multi-Factor Authentication (MFA) oturum açma penceresi açar. Azure AD parolanızla oturum açın.

    ![mfa-login-prompt.png](media/sql-database-managed-instance-security-tutorial/mfa-login-prompt.png)

1. Ssms'de **Nesne Gezgini**, sunucuya sağ tıklayın ve seçin **yeni sorgu**.
1. Sorgu penceresinde, başka bir Azure AD hesap oturum açma oluşturmak için aşağıdaki sözdizimini kullanın:

    ```sql
    USE master
    GO
    CREATE LOGIN login_name FROM EXTERNAL PROVIDER
    GO
    ```

    Bu örnek, Azure AD kullanıcı için bir oturum oluşturur bob@aadsqlmi.net, olan etki alanı aadsqlmi.net ile Azure AD aadsqlmi.onmicrosoft.com Federasyon.

    Aşağıdaki T-SQL komutunu yürütün. Federasyon Azure AD hesapları olan şirket içi Windows oturum açma bilgileri ve kullanıcılar için yönetilen örnek değiştirmeler.

    ```sql
    USE master
    GO
    CREATE LOGIN [bob@aadsqlmi.net] FROM EXTERNAL PROVIDER
    GO
    ```

1. Bir veritabanı kullanarak yönetilen örnek oluşturma [CREATE DATABASE](/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current) söz dizimi. Bu veritabanı, kullanıcı oturum açma bilgileri, sonraki bölümde test etmek için kullanılır.
    1. İçinde **Nesne Gezgini**, sunucuya sağ tıklayın ve seçin **yeni sorgu**.
    1. Sorgu penceresinde, adlı bir veritabanı oluşturmak için aşağıdaki sözdizimini kullanın. **MyMITestDB**.

        ```sql
        CREATE DATABASE MyMITestDB;
        GO
        ```

1. Azure AD'de bir grup için bir yönetilen örnek oturum açma oluşturun. Yönetilen örnek için oturum açma ekleyebilmek için önce Azure AD'de mevcut grubu gerekir. Bkz: [temel bir grup oluşturma ve Azure Active Directory'yi kullanarak üye ekleme](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Bir grup oluşturmak _mygroup_ ve bu gruba üye ekleme.

1. SQL Server Management Studio'da yeni bir sorgu penceresi açın.

    Bu örnek adlı bir grup var varsayar _mygroup_ Azure AD'de. Aşağıdaki komutu yürütün:

    ```sql
    USE master
    GO
    CREATE LOGIN [mygroup] FROM EXTERNAL PROVIDER
    GO
    ```

1. Yönetilen örnek yeni oluşturulan oturum açma veya Grup ile bir test olarak oturum açın. Yönetilen örnek için yeni bir bağlantı açmak ve yeni oturum açma kimlik doğrulaması yapılırken kullanın.
1. İçinde **Nesne Gezgini**, sunucuya sağ tıklayın ve seçin **yeni sorgu** yeni bağlantı için.
1. Yeni oluşturulan sunucu izinlerini denetlemek aşağıdaki komutu yürüterek Azure AD sunucu sorumlusu (oturum açma):

      ```sql
      SELECT * FROM sys.fn_my_permissions (NULL, 'DATABASE')
      GO
      ```

> [!NOTE]
> Azure AD Konuk kullanıcılar için yönetilen örnek oturum açma bilgileri, yalnızca bir Azure AD grubunun bir parçası eklendiğinde desteklenir. Bir Azure AD Konuk kullanıcı yönetilen örnek için başka bir Azure AD'den ait Azure AD'ye davet hesaptır. Örneğin, joe@contoso.com (Azure AD hesabı) veya steve@outlook.com (MSA hesabı), Azure AD aadsqlmi bir gruba eklenebilir. Bir oturum açma kullanıcıları grubuna eklendikten sonra yönetilen örneğinde oluşturulabilir **ana** grubu kullanmak için veritabanı **CREATE LOGIN** söz dizimi. Bu grubun üyeleri olan konuk kullanıcılar, kendi geçerli oturumları kullanarak yönetilen örneğe bağlanabilir (örneğin, joe@contoso.com veya steve@outlook.com).

## <a name="create-an-azure-ad-user-from-the-azure-ad-server-principal-login-and-give-permissions"></a>Bir Azure AD kullanıcısının asıl sunucu (oturum açma) Azure AD'den oluşturma ve izinleri verme

SQL Server şirket içi ile yaptığı gibi yetkilendirme ayrı veritabanlarına yönetilen örneğinde çok aynı şekilde çalışır. Bir kullanıcı bir veritabanında var olan bir oturum oluşturuldu ve bu veritabanı için izinleri ile sağlanan veya bir veritabanı rolüne eklenir.

Biz adlı bir veritabanı oluşturduğunuza göre **MyMITestDB**, ve yalnızca varsayılan izinleri olan bir oturum açma, sonraki adım bir kullanıcı, oturum açma işlemi oluşturmaktır. Şu anda oturum açma yönetilen bağlanabilir örneğini ve tüm veritabanlarına bakın, ancak veritabanlarıyla etkileşim kuramaz. Varsayılan izinleri olan Azure AD hesabıyla oturum açın ve yeni oluşturulan veritabanına genişletmeye çalışırsanız, aşağıdaki hatayı görürsünüz:

![SSMS-db-not-accessible.png](media/sql-database-managed-instance-security-tutorial/ssms-db-not-accessible.png)

Veritabanı izinleri vererek daha fazla bilgi için bkz: [veritabanı altyapısı izinleri ile çalışmaya başlama](/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions).

### <a name="create-an-azure-ad-user-and-create-a-sample-table"></a>Bir Azure AD kullanıcısı oluşturun ve örnek tablo oluşturma

1. Yönetilen örnek kullanarak günlük bir `sysadmin` SQL Server Management Studio'yu kullanarak hesap.
1. İçinde **Nesne Gezgini**, sunucuya sağ tıklayın ve seçin **yeni sorgu**.
1. Sorgu penceresinde, bir Azure AD kullanıcısının asıl Azure AD sunucusundan (oturum açma) oluşturmak için aşağıdaki sözdizimini kullanın:

    ```sql
    USE <Database Name> -- provide your database name
    GO
    CREATE USER user_name FROM LOGIN login_name
    GO
    ```

    Aşağıdaki örnek, bir kullanıcı oluşturur bob@aadsqlmi.net oturumunda bob@aadsqlmi.net:

    ```sql
    USE MyMITestDB
    GO
    CREATE USER [bob@aadsqlmi.net] FROM LOGIN [bob@aadsqlmi.net]
    GO
    ```

1. Ayrıca, bir grup olan bir Azure AD kullanıcısının asıl Azure AD sunucusundan (oturum açma) oluşturmak için de desteklenir.

    Aşağıdaki örnek, Azure AD grubu için bir oturum oluşturur _mygroup_ Azure AD'NİZDE bulunmaktadır.

    ```sql
    USE MyMITestDB
    GO
    CREATE USER [mygroup] FROM LOGIN [mygroup]
    GO
    ```

    Ait tüm kullanıcıların **mygroup** erişip **MyMITestDB** veritabanı.

    > [!IMPORTANT]
    > Oluştururken bir **kullanıcı** bir Azure AD sunucusu sorumlusundan (oturum açma), user_name aynı login_name olarak belirtin. **oturum açma**.

    Daha fazla bilgi için [CREATE USER](/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current).

1. Yeni Sorgu penceresinde aşağıdaki T-SQL komutunu kullanarak bir test tablo oluşturun:

    ```sql
    USE MyMITestDB
    GO
    CREATE TABLE TestTable
    (
    AccountNum varchar(10),
    City varchar(255),
    Name varchar(255),
    State varchar(2)
    );
    ```

1. Bağlantı SSMS'de oluşturulduğu kullanıcı hesabı ile oluşturun. Tablo göremez fark edeceksiniz **TestTable** tarafından oluşturulan `sysadmin` daha önce. Veritabanından veri okumak için gerekli izinlere sahip kullanıcı sağlamak ihtiyacımız var.

1. Kullanıcının sahip olduğu aşağıdaki komutu yürüterek geçerli izni denetleyebilirsiniz:

    ```sql
    SELECT * FROM sys.fn_my_permissions('MyMITestDB','DATABASE')
    GO
    ```

### <a name="add-users-to-database-level-roles"></a>Veritabanı düzeyinde rollerine kullanıcı ekleme

Kullanıcı veritabanındaki verileri görmek sağladığımız [veritabanı düzeyinde roller](/sql/relational-databases/security/authentication-access/database-level-roles) kullanıcı.

1. Yönetilen örnek kullanarak günlük bir `sysadmin` SQL Server Management Studio'yu kullanarak hesap.

1. İçinde **Nesne Gezgini**, sunucuya sağ tıklayın ve seçin **yeni sorgu**.

1. Azure AD kullanıcı vermek `db_datareader` aşağıdaki T-SQL söz dizimini kullanarak veritabanı rolü:

    ```sql
    Use <Database Name> -- provide your database name
    ALTER ROLE db_datareader ADD MEMBER user_name
    GO
    ```

    Aşağıdaki örnek, kullanıcının sağlar bob@aadsqlmi.net ve grubu _mygroup_ ile `db_datareader` izinlerini **MyMITestDB** veritabanı:

    ```sql
    USE MyMITestDB
    GO
    ALTER ROLE db_datareader ADD MEMBER [bob@aadsqlmi.net]
    GO
    ALTER ROLE db_datareader ADD MEMBER [mygroup]
    GO
    ```

1. Onay veritabanında oluşturulan Azure AD kullanıcısı mevcut aşağıdaki komutu yürüterek:

    ```sql
    SELECT * FROM sys.database_principals
    GO
    ```

1. Yönetilen örnek için yeni bir bağlantı eklendi kullanıcı oluşturmak `db_datareader` rol.
1. Veritabanında genişletin **Nesne Gezgini** tablo görmek için.

    ![SSMS test table.png](media/sql-database-managed-instance-security-tutorial/ssms-test-table.png)

1. Yeni bir sorgu penceresi açın ve aşağıdaki SELECT deyimi çalıştırın:

    ```sql
    SELECT *
    FROM TestTable
    ```

    Tablodaki verileri görebildiğine misiniz? Döndürülen sütunları görmeniz gerekir.

    ![test tablosu query.png SSMS](media/sql-database-managed-instance-security-tutorial/ssms-test-table-query.png)

## <a name="impersonating-azure-ad-server-level-principals-logins"></a>Azure AD sunucu düzeyi asıl hesaplar (oturum açma bilgileri) kimliğine bürünme

Yönetilen örnek, Azure AD sunucu düzeyi asıl hesaplar (oturum açma bilgileri), kimliğe bürünme özelliğini destekler.

### <a name="test-impersonation"></a>Test kimliğe bürünme

1. Yönetilen örnek kullanarak günlük bir `sysadmin` SQL Server Management Studio'yu kullanarak hesap.

1. İçinde **Nesne Gezgini**, sunucuya sağ tıklayın ve seçin **yeni sorgu**.

1. Sorgu penceresinde, yeni bir saklı yordam oluşturmak için aşağıdaki komutu kullanın:

    ```sql
    USE MyMITestDB
    GO  
    CREATE PROCEDURE dbo.usp_Demo  
    WITH EXECUTE AS 'bob@aadsqlmi.net'  
    AS  
    SELECT user_name();  
    GO
    ```

1. Kimliğine bürünüyorsunuz saklı yordamı yürütülürken kullanıcı olduğunu görmek için aşağıdaki komutu kullanın **bob\@aadsqlmi.net**.

    ```sql
    Exec dbo.usp_Demo
    ```

1. Kimliğe bürünme EXECUTE AS oturum açma deyimini kullanarak test edin:

    ```sql
    EXECUTE AS LOGIN = 'bob@aadsqlmi.net'
    GO
    SELECT SUSER_SNAME()
    REVERT
    GO
    ```

> [!NOTE]
> Yalnızca parçası olan SQL sunucu düzeyi ilkeleri (oturum açma bilgileri) `sysadmin` rol, Azure AD sorumlusu hedefleme şu işlemler yürütebilirsiniz: 
> - EXECUTE AS USER
> - EXECUTE AS LOGIN

## <a name="using-cross-database-queries-in-managed-instances"></a>Veritabanları arası sorgular kullanarak yönetilen örnekler

Veritabanları arası sorgular, Azure AD sunucu sorumlusu (oturum açma bilgileri) ile Azure AD hesapları için desteklenir. Azure AD grubu ile platformlar arası sorgu testi için biz başka bir veritabanı ve tablo oluşturmanız gerekir. Bir zaten varsa, başka bir veritabanı ve tablo oluşturma atlayabilirsiniz.

1. Yönetilen örnek kullanarak günlük bir `sysadmin` SQL Server Management Studio'yu kullanarak hesap.
1. İçinde **Nesne Gezgini**, sunucuya sağ tıklayın ve seçin **yeni sorgu**.
1. Sorgu penceresinde, adlı bir veritabanı oluşturmak için aşağıdaki komutu kullanın. **MyMITestDB2** ve adlı tablo **TestTable2**:

    ```sql
    CREATE DATABASE MyMITestDB2;
    GO
    USE MyMITestDB2
    GO
    CREATE TABLE TestTable2
    (
    EmpId varchar(10),
    FirstName varchar(255),
    LastName varchar(255),
    Status varchar(10)
    );
    ```

1. Yeni Sorgu penceresinde, kullanıcı oluşturmak için aşağıdaki komutu yürütün _mygroup_ yeni veritabanı **MyMITestDB2**ve bu veritabanında SELECT izinleri verecek _mygroup_:

    ```sql
    USE MyMITestDB2
    GO
    CREATE USER [mygroup] FROM LOGIN [mygroup]
    GO
    GRANT SELECT TO [mygroup]
    GO
    ```

1. Yönetilen örnek SQL Server Management Studio kullanarak Azure AD grubunun bir üyesi olarak oturum _mygroup_. Yeni bir sorgu penceresi açın ve veritabanları arası SELECT deyimini yürütün:

    ```sql
    USE MyMITestDB
    SELECT * FROM MyMITestDB2..TestTable2
    GO
    ```

    Tablo sonuçları görmeniz gerekir **TestTable2**.

## <a name="additional-scenarios-supported-for-azure-ad-server-principals-logins-public-preview"></a>Azure AD sunucu sorumlusu (oturum açma bilgileri) (genel Önizleme) için desteklenen ek senaryoları 

- SQL aracı yönetimi ve iş yürütme için Azure AD sunucusu sorumluları (oturum açma bilgileri) desteklenir.
- Veritabanı yedekleme ve geri yükleme işlemleri (oturum açma bilgileri) Azure AD sunucu sorumlusu tarafından yürütülebilir.
- [Denetim](sql-database-managed-instance-auditing.md) Azure AD sunucu sorumlusu (oturum açma bilgileri) ve kimlik doğrulama olayları ile ilgili tüm deyimler.
- Adanmış yönetici bağlantısı için üye olan Azure AD sunucu sorumlusu (oturum açma bilgileri) `sysadmin` sunucu rolü.
- Azure AD sunucusu ilkeleri (oturum açma) kullanarak desteklenen [sqlcmd yardımcı programını](/sql/tools/sqlcmd-utility) ve [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) aracı.
- Oturum açma tetikleyicileri, Azure AD sunucu sorumlusu (oturum açma bilgileri) gelen oturum açma olayları için desteklenir.
- Hizmet aracısı ve DB posta, Azure AD sunucu sorumlusu (oturum açma) kullanarak kurulum olabilir.


## <a name="next-steps"></a>Sonraki adımlar

### <a name="enable-security-features"></a>Güvenlik özelliklerini etkinleştirme

Şu [yönetilen örnek özellikleri güvenlik özellikleri](sql-database-managed-instance.md#azure-sql-database-security-features) kapsamlı bir liste için bir makale birkaç yolla yapılandırarak veritabanınızın güvenliğini sağlama. Aşağıdaki güvenlik özellikleri ele alınmıştır:

- [Yönetilen örnek denetleme](sql-database-managed-instance-auditing.md) 
- [Always encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine)
- [Tehdit algılama](sql-database-managed-instance-threat-detection.md) 
- [Dinamik veri maskeleme](/sql/relational-databases/security/dynamic-data-masking)
- [Satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security) 
- [Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)

### <a name="managed-instance-capabilities"></a>Yönetilen örnek özellikleri

Yönetilen örnek özellikleri eksiksiz bir genel bakış için bkz:

> [!div class="nextstepaction"]
> [Yönetilen örnek özellikleri](sql-database-managed-instance.md)
