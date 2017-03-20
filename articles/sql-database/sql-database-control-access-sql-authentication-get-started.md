---
title: "SQL kimlik doğrulaması: Azure SQL Veritabanı güvenlik duvarları, kimlik doğrulaması, erişim | Microsoft Belgeleri"
description: "Bu başlangıç öğreticisinde SQL Server Management Studio ve Transact-SQL kullanarak sunucu ve veritabanı düzeyinde güvenlik duvarları, SQL kimlik doğrulaması, oturum açma bilgileri, kullanıcılar ve rollerle çalışarak Azure SQL Veritabanı sunucuları ve veritabanlarına erişim izni vermeyi ve bunu kontrol etmeyi öğreneceksiniz."
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 67797b09-f5c3-4ec2-8494-fe18883edf7f
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: a5084b62a309dba433e2b363322b9a9c362bcdc3
ms.lasthandoff: 03/10/2017


---
# <a name="sql-server-authentication-access-and-database-level-firewall-rules"></a>SQL Server kimlik doğrulaması, erişimi ve veritabanı düzeyinde güvenlik duvarı kuralları

Bu öğreticide SQL Server Management Studio kullanarak SQL Server kimlik doğrulaması, oturumları, kullanıcılar ve veritabanı rolleriyle çalışarak Azure SQL Veritabanı sunucuları ve veritabanlarına erişim izni vermeyi öğreneceksiniz. Bu öğreticiyi tamamladıktan sonra aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

- SQL Server kimlik doğrulamasını temel alan oturum açma bilgileri ve kullanıcılar oluşturma
- Rollere kullanıcı ekleme ve rollere izin verme
- T-SQL’yi kullanarak veri tabanı düzeyinde ve sunucu düzeyinde güvenlik duvarı kuralı oluşturma 
- SSMS’yi kullanarak belirli bir veritabanına kullanıcı olarak bağlanma
- Asıl veritabanı ve kullanıcı veritabanlarındaki kullanıcı izinlerini görüntüleme

**Tahmini süre**: Bu öğreticinin tamamlanması yaklaşık 45 dakika alacaktır (önkoşulları karşıladığınız varsayılarak).

> [!NOTE]
> Bu öğretici, şu konu başlıklarının içeriğini öğrenmenize yardımcı olur: [SQL Veritabanında erişim ve denetim](sql-database-control-access.md), [Oturumlar, kullanıcılar ve veritabanı rolleri](sql-database-manage-logins.md), [Sorumlular](https://msdn.microsoft.com/library/ms181127.aspx), [Veritabanı rolleri](https://msdn.microsoft.com/library/ms189121.aspx) ve [SQL Veritabanı güvenlik duvarı kuralları](sql-database-firewall-configure.md). Azure Active Directory kimlik doğrulaması hakkındaki bir öğretici için bkz. [Azure AD Kimlik Doğrulaması’nı kullanmaya başlama](sql-database-control-access-aad-authentication-get-started.md).
>  

## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure hesabı**. Bir Azure hesabınız olmalıdır. [Ücretsiz bir Azure hesabı açabilir](https://azure.microsoft.com/free/) veya [Visual Studio abonelik avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/). 

* **Azure oluşturma izinleri**. Azure portalına, abonelik sahibi veya katkıda bulunan rolü üyesi olan bir hesap kullanarak bağlanabiliyor olmanız gerekir. Rol tabanlı erişim denetimi (RBAC) ile ilgili daha fazla bilgi için bkz. [Azure portalında erişim yönetimini kullanmaya başlama](../active-directory/role-based-access-control-what-is.md).

* **SQL Server Management Studio**. SQL Server Management Studio’nun en son sürümünü [Download SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio’yu İndirin) sayfasından indirip yükleyebilirsiniz. SSMS için sürekli yeni özellikler yayınlandığından Azure SQL Veritabanı’na bağlanırken her zaman en son sürümü kullanın.

* **Taban sunucu ve veritabanları** Bu öğreticide kullanılan bir sunucu ve iki veritabanını yükleyip yapılandırmak için **Azure’a Dağıt** düğmesine tıklayın. Düğmeye tıklandığında **Şablondan dağıt** dikey penceresi açılır; yeni bir kaynak grubu oluşturun ve oluşturulacak yeni sunucu için **Yönetici Oturum Açma Parolası** belirtin:

   [![indir](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fsqldbtutorial.blob.core.windows.net%2Ftemplates%2Fsqldbgetstarted.json)


## <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Azure hesabınızı kullanarak Azure portalında oturum açma
Bu yordamdaki adımlarda Azure hesabınızı kullanarak Azure portalına nasıl bağlanılacağı açıklanır](https://account.windowsazure.com/Home/Index).

1. Tercih ettiğiniz tarayıcınızı açın ve [Azure portal](https://portal.azure.com/)’a bağlanın.
2. [Azure Portal](https://portal.azure.com/) oturum açın.
3. **Oturum açma** sayfasında aboneliğinize ait kimlik bilgilerini sağlayın.
   
   ![Oturum aç](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="view-logical-server-security-information-in-the-azure-portal"></a>Azure portalında mantıksal sunucu güvenlik bilgilerini görüntüleme

Bu yordamdaki adımlar, Azure portalında mantıksal sunucunuzun güvenlik yapılandırmasıyla ilgili bilgileri nasıl görüntüleyeceğinizi açıklar.

1. Sunucunuzun **SQL Server** dikey penceresini açın ve **Genel bakış** sayfasındaki bilgileri görüntüleyin.

   ![Azure portalında sunucu yönetici hesabı](./media/sql-database-control-access-sql-authentication-get-started/sql_admin_portal.png)

2. Mantıksal sunucunuzdaki sunucu yöneticisinin adını not edin. 

3. Parolayı anımsamıyorsanız **Parolayı sıfırla**'ya tıklayarak yeni bir parola belirleyin.

4. Bu sunucunun bağlantı bilgilerini edinmeniz gerekiyorsa **Özellikler**’e tıklayın.

## <a name="view-server-admin-permissions-using-ssms"></a>SSMS’yi kullanarak sunucu yöneticisi izinlerini görüntüleme

Bu yordamdaki adımlar, sunucu yöneticisi hesabı ve bu hesabın asıl veritabanı ile kullanıcı veritabanlarındaki izinleri hakkındaki bilgileri nasıl görüntüleyeceğinizi açıklar.

1. SQL Server Management Studio'yu açın ve SQL Server Kimlik Doğrulaması ile Sunucu yöneticisi hesabını kullanarak sunucunuza yönetici olarak bağlanın.

   ![sunucuya bağlan](./media/sql-database-get-started/connect-to-server.png)

2. **Bağlan**'a tıklayın.

   ![sunucuya bağlanıldı](./media/sql-database-get-started/connected-to-server.png)

3. Nesne Gezgini’nde **Güvenlik**’i ve ardından **Oturumlar**’ı genişleterek sunucunuzda açık olan oturumları görüntüleyin. Yeni bir sunucuda açılan tek oturum, sunucu yöneticisi hesabıyla açılan oturumdur.

   ![Sunucu yöneticisi oturum açma](./media/sql-database-control-access-sql-authentication-get-started/server_admin_login.png)

4. Bu veritabanındaki sunucu yöneticisi oturumu için oluşturulan kullanıcı hesabını görüntülemek için Nesne Gezgini’nde sırasıyla **Veritabanları**, **Sistem veritabanları**, **asıl**, **Güvenlik** ve **Kullanıcılar**’ı genişletin.

   ![sunucu yöneticisi için asıl veritabanı kullanıcı hesabı](./media/sql-database-control-access-sql-authentication-get-started/master_database_user_account_for_server_admin.png)

   > [!NOTE]
   > Kullanıcılar düğümünde görünen diğer kullanıcı hesapları hakkında bilgi için bkz. [Sorumlular](https://msdn.microsoft.com/library/ms181127.aspx).
   >

5. Nesne Gezgini'nde **asıl**'a sağ tıklayıp **Yeni Sorgu**'yu seçerek asıl veritabanına bağlı bir sorgu penceresi açın.
6. Sorguyu yürüten kullanıcı hakkında bilgi almak için sorgu penceresinde aşağıdaki sorguyu yürütün. 

   ```
   SELECT USER;
   ```

   ![asıl veritabanında kullanıcı seçme sorgusu](./media/sql-database-control-access-sql-authentication-get-started/select_user_query_in_master_database.png)

7. **Asıl** veritabanındaki sqladmin kullanıcısının izinleri hakkında bilgi döndürmek için sorgu penceresinde aşağıdaki sorguyu yürütün. 

   ```
   SELECT prm.permission_name
      , prm.class_desc
      , prm.state_desc
      , p2.name as 'Database role'
      , p3.name as 'Additional database role' 
   FROM sys.database_principals p
   JOIN sys.database_permissions prm
      ON p.principal_id = prm.grantee_principal_id
      LEFT JOIN sys.database_principals p2
      ON prm.major_id = p2.principal_id
      LEFT JOIN sys.database_role_members r
      ON p.principal_id = r.member_principal_id
      LEFT JOIN sys.database_principals p3
      ON r.role_principal_id = p3.principal_id
   WHERE p.name = 'sqladmin';
   ```

   ![asıl veritabanındaki sunucu yönetici izinleri](./media/sql-database-control-access-sql-authentication-get-started/server_admin_permissions_in_master_database.png)

   >[!NOTE]
   > Sunucu yöneticisi asıl veritabanına bağlanma, oturum ve kullanıcı oluşturma, sys.sql_logins tablosundan bilgi seçme ve dbmanager ile dbcreator veritabanı rollerine kullanıcı ekleme izinlerine sahiptir. Bu izinler, tüm kullanıcıların izinleri devraldığı ortak role verilen izinlere (belirli tablolardan bilgi seçme izinleri gibi) ek olarak verilmiştir. Daha fazla bilgi için bkz. [İzinler](https://msdn.microsoft.com/library/ms191291.aspx).
   >

8. Bu veritabanındaki (ve her bir kullanıcı veritabanındaki) sunucu yöneticisi oturumu için oluşturulan kullanıcı hesabını görüntülemek için Nesne Gezgini’nde sırasıyla **blankdb**, **Güvenlik** ve **Kullanıcılar**’ı genişletin.

   ![blankdb içindeki kullanıcı hesapları](./media/sql-database-control-access-sql-authentication-get-started/user_accounts_in_blankdb.png)

9. Nesne Gezgini'nde **blankdb**'ye sağ tıklayıp **Yeni Sorgu**'ya tıklayın.

10. Sorguyu yürüten kullanıcı hakkında bilgi almak için sorgu penceresinde aşağıdaki sorguyu yürütün.

   ```
   SELECT USER;
   ```

   ![blankdb veritabanında kullanıcı seçme sorgusu](./media/sql-database-control-access-sql-authentication-get-started/select_user_query_in_blankdb_database.png)

11. dbo kullanıcısının izinleri hakkında bilgi almak için sorgu penceresinde aşağıdaki sorguyu yürütün. 

   ```
   SELECT prm.permission_name
      , prm.class_desc
      , prm.state_desc
      , p2.name as 'Database role'
      , p3.name as 'Additional database role' 
   FROM sys.database_principals AS p
   JOIN sys.database_permissions AS prm
      ON p.principal_id = prm.grantee_principal_id
      LEFT JOIN sys.database_principals AS p2
      ON prm.major_id = p2.principal_id
      LEFT JOIN sys.database_role_members r
      ON p.principal_id = r.member_principal_id
      LEFT JOIN sys.database_principals AS p3
      ON r.role_principal_id = p3.principal_id
   WHERE p.name = 'dbo';
   ```

   ![blankdb veritabanındaki sunucu yöneticisi izinleri](./media/sql-database-control-access-sql-authentication-get-started/server_admin_permissions_in_blankdb_database.png)

   > [!NOTE]
   > Dbo kullanıcısı hem ortak rolün ve hem de db_owner sabit veritabanı rolünün üyesidir. Daha fazla bilgi için bkz. [Veritabanı Düzeyinde Roller](https://msdn.microsoft.com/library/ms189121.aspx).
   >

## <a name="create-a-new-user-with-select-permissions"></a>SELECT izinleriyle yeni kullanıcı oluşturma

Bu yordamdaki adımlarda veritabanı düzeyinde kullanıcı oluşturma, yeni bir kullanıcının (ortak rol aracılığıyla) varsayılan izinlerini test etme, bir kullanıcıya **SELECT** izinleri verme ve bu değiştirilen izinleri görüntüleme işlemlerinin nasıl yapılacağı açıklanır.

> [!NOTE]
> [İçerilen kullanıcılar](https://msdn.microsoft.com/library/ff929188.aspx) olarak da bilinen veritabanı düzeyindeki kullanıcılar, veritabanınızın taşınabilirliğini artırır. Taşınabilirliğin avantajları hakkında bilgi edinmek için bkz. [Azure SQL Veritabanı güvenliğini coğrafi geri yükleme veya ikincil bir sunucuya yük devretme gerçekleştirecek şekilde yapılandırma ve yönetme](sql-database-geo-replication-security-config.md).
>

1. Nesne Gezgini'nde **sqldbtutorialdb**'ye sağ tıklayıp **Yeni Sorgu**'ya tıklayın.
2. AdventureWorksLT veritabanında **user1** adlı bir kullanıcı oluşturmak için bu sorgu penceresinde aşağıdaki deyimi yürütün.

   ```
   CREATE USER user1
   WITH PASSWORD = 'p@ssw0rd';
   ```
   ![new user user1 sqldbtutorialdb](./media/sql-database-control-access-sql-authentication-get-started/new_user_user1_aw.png)

3. user1 kullanıcısının izinleri hakkında bilgi almak için sorgu penceresinde aşağıdaki sorguyu yürütün.

   ```
   SELECT prm.permission_name
      , prm.class_desc
      , prm.state_desc
      , p2.name as 'Database role'
      , p3.name as 'Additional database role' 
   FROM sys.database_principals AS p
   JOIN sys.database_permissions AS prm
      ON p.principal_id = prm.grantee_principal_id
      LEFT JOIN sys.database_principals AS p2
      ON prm.major_id = p2.principal_id
      LEFT JOIN sys.database_role_members r
      ON p.principal_id = r.member_principal_id
      LEFT JOIN sys.database_principals AS p3
      ON r.role_principal_id = p3.principal_id
   WHERE p.name = 'user1';
   ```

   ![bir kullanıcı veritabanında yeni kullanıcı izinleri](./media/sql-database-control-access-sql-authentication-get-started/new_user_permissions_in_user_database.png)

   > [!NOTE]
   > Veritabanındaki yeni kullanıcılar yalnızca ortak rolden devralınmış izinlere sahip olur.
   >

4. sqldbtutorialdb database veritabanındaki SalesLT.ProductCategory tablosunu **user1** olarak yalnızca ortak rolden devralınmış izinlerle sorgulamaya çalışmak için **EXECUTE AS USER** deyimini kullanarak aşağıdaki sorguları yürütün.

   ```
   EXECUTE AS USER = 'user1';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![select izni yok](./media/sql-database-control-access-sql-authentication-get-started/no_select_permissions.png)

   > [!NOTE]
   > Varsayılan olarak, ortak rol tarafından kullanıcı nesneleri üzerinde **SELECT** izinleri verilmez.
   >

5. SalesLT şemasının ProductCategory tablosunda **user1** kullanıcısına **SELECT** izinleri vermek için aşağıdaki deyimi yürütün.

   ```
   GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to user1;
   ```

   ![select izinleri verme](./media/sql-database-control-access-sql-authentication-get-started/grant_select_permissions.png)

6. sqldbtutorialdb veritabanındaki SalesLT.ProductCategory tablosunu **user1** olarak başarılı bir biçimde sorgulamak için aşağıdaki sorguları yürütün.

   ```
   EXECUTE AS USER = 'user1';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![select izinleri](./media/sql-database-control-access-sql-authentication-get-started/select_permissions.png)

## <a name="create-a-database-level-firewall-rule-using-t-sql"></a>T-SQL kullanarak veritabanı düzeyinde güvenlik duvarı kuralı oluşturma

Bu yordamdaki adımlarda, [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) sistem saklı yordamını kullanarak nasıl veritabanı düzeyinde güvenlik duvarı kuralı oluşturulacağı açıklanır. Veritabanı düzeyindeki bir güvenlik duvarı kuralı, sunucu yöneticisinin kullanıcılara yalnızca belirli veritabanları için Azure SQL Veritabanı güvenlik duvarından geçmesine izin vermesine imkan tanır.

> [!NOTE]
> [Veritabanı düzeyinde güvenlik duvarı kuralları](sql-database-firewall-configure.md), veritabanınızın taşınabilirliğini artırır. Taşınabilirliğin avantajları hakkında bilgi edinmek için bkz. [Azure SQL Veritabanı güvenliğini coğrafi geri yükleme veya ikincil bir sunucuya yük devretme gerçekleştirecek şekilde yapılandırma ve yönetme](sql-database-geo-replication-security-config.md).
>

> [!IMPORTANT]
> Veritabanı düzeyindeki bir güvenlik duvarı kuralını test etmek için başka bir bilgisayardan bağlanın (veya Azure portalında sunucu düzeyindeki güvenlik duvarı kuralını silin.)
>

1. Sunucu düzeyinde bir güvenlik duvarı kuralı içermeyen bir bilgisayarda SQL Server Management Studio’yu açın.

2. **Sunucuya bağlan** penceresinde SQL Server kimlik doğrulamasını kullanarak **user1** hesabıyla bağlantı kurmak için sunucu adını ve kimlik doğrulaması bilgilerini girin. 
    
   ![Güvenlik duvarı kuralı olmadan user1 olarak bağlanma1](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule1.png)

3. **Seçenekler**'e tıklayarak bağlanmak istediğiniz veritabanını belirtin ve ardından **Bağlantı Özellikleri** sekmesindeki **Veritabanına Bağlan** açılır kutusuna **sqldbtutorialdb** yazın.
   
   ![Güvenlik duvarı kuralı olmadan user1 olarak bağlanma2](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule2.png)

4. **Bağlan**'a tıklayın. 

   SQL Veritabanına bağlanmaya çalıştığınız bilgisayarın veritabanına erişim sağlayan güvenlik duvarı kuralına sahip olmadığını belirten bir iletişim kutusu açılır. 

   ![Güvenlik duvarı kuralı olmadan user1 olarak bağlanma4](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule4.png)


5. Bu iletişim kutusundaki istemci IP adresini 8. adımda kullanmak üzere kopyalayın.
6. **Tamam**’a tıklayarak hata iletişim kutusunu kapatın, ancak **Sunucuya Bağlan** iletişim kutusunu kapatmayın.
7. Önceden sunucu düzeyinde güvenlik duvarı kuralı oluşturduğunuz bir bilgisayara dönün. 
8. SSMS’de sunucu yöneticisi olarak sqldbtutorialdb veritabanına bağlanın ve 5. adımda belirlenen IP adresini (veya adres aralığını) kullanarak veritabanı düzeyinde bir güvenlik duvarı oluşturmak için aşağıdaki deyimi yürütün.  

   ```
   EXEC sp_set_database_firewall_rule @name = N'sqldbtutorialdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

   ![güvenlik duvarı kuralı ekleme](./media/sql-database-control-access-sql-authentication-get-started/user1_add_rule_aw.png)

9. Yeniden diğer bilgisayara geçin ve **Sunucuya Bağlan** iletişim kutusunda **Bağlan**'a tıklayarak sqldbtutorialdb veritabanına user1 olarak bağlanın. 

   > [!NOTE]
   > Veritabanı düzeyindeki güvenlik duvarı kuralını oluşturduktan sonra kuralın etkin hale gelmesi 5 dakikayı bulabilir.
   >

10. Başarılı bir şekilde bağlandıktan sonra Nesne Gezgini’ndeki **Veritabanları**’nı genişletin. **user1**’in **sqldbtutorialdb** veritabanını yalnızca görüntüleyebildiğini fark edeceksiniz.

   ![Güvenlik duvarı kuralıyla user1 olarak bağlanma&1;](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_rule1.png)

11. **sqldbtutorialdb**’yi ve ardından **Tablolar**’ı genişletin. user1 kullanıcısının yalnızca **SalesLT.ProductCategory** tablosunu görüntüleme iznine sahip olduğuna dikkat edin. 

   ![user1 olarak bağlanma ve nesneleri görüntüleme&1;](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_view_objects1.png)

## <a name="create-a-new-user-as-dbowner-and-a-database-level-firewall-rule"></a>db_owner olarak yeni bir kullanıcı ve veritabanı düzeyinde bir güvenlik duvarı kuralı oluşturun

Bu yordamdaki adımlarda db_owner veritabanı rolü izinleriyle başka bir veritabanında kullanıcı oluşturma ve bu veritabanı için veritabanı düzeyinde bir güvenlik duvarı oluşturma işlemlerinin nasıl gerçekleştirileceği açıklanır. **db_owner** rolü üyeliğine sahip bu yeni kullanıcı yalnızca bu tek veritabanı için bağlanma ve yönetme yetkisine sahip olur.

1. Sunucu yönetici hesabıyla SQL Veritabanına bağlanmış olan bilgisayara geçin.
2. **blankdb** veritabanına bağlandıktan sonra bir sorgu penceresi açın ve aşağıdaki deyimi yürüterek blankdb veritabanında blankadmin adlı bir kullanıcı oluşturun.

   ```
   CREATE USER blankdbadmin
   WITH PASSWORD = 'p@ssw0rd';
   ```

3. Aynı sorgu penceresinde aşağıdaki deyimi yürüterek blankdbadmin kullanıcısını db_owner veritabanı rolüne ekleyin. Bu kullanıcı artık blankdb veritabanını yönetmek için gerekli olan tüm eylemleri gerçekleştirebilir.

   ```
   ALTER ROLE db_owner ADD MEMBER blankdbadmin; 
   ```

4. Aynı sorgu penceresinde aşağıdaki [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) deyimini önceki yordamın 4. adımında yer alan IP adresiyle (veya bu veritabanı kullanıcıları için IP adresi aralığıyla) yürüterek veritabanı düzeyinde güvenlik duvarı oluşturun:

   ```
   EXEC sp_set_database_firewall_rule @name = N'blankdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

5. Bilgisayar değiştirin (veritabanı düzeyinde güvenlik duvarını oluşturduğunuz bilgisayara geçin) ve blankdb veritabanına blankdbadmin kullanıcı hesabını kullanarak bağlanın.
6. blankdb veritabanına bağlandıktan sonra bir sorgu penceresi açın ve aşağıdaki deyimi yürüterek blankdb veritabanında blankdbuser1 adlı bir kullanıcı oluşturun.

   ```
   CREATE USER blankdbuser1
   WITH PASSWORD = 'p@ssw0rd';
   ```
 
7. Öğrenme ortamınız için gerekli olması halinde bu kullanıcı için ek bir veritabanı düzeyinde güvenlik duvarı kuralı oluşturabilirsiniz. Bununla birlikte, veritabanı düzeyindeki güvenlik duvarı kuralını bir IP adresi aralığı kullanarak oluşturduysanız bunu yapmanıza gerek kalmayabilir.

## <a name="grant-dbmanager-permissions-and-create-a-server-level-firewall-rule"></a>dbmanager izinleri verme ve sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

Bu yordamdaki adımlarda, asıl veritabanında yeni kullanıcı veritabanları oluşturma ve yönetme izinlerine sahip bir oturum ve kullanıcı oluşturma işleminin nasıl gerçekleştirileceği açıklanır. Bu adımlar, Transact-SQL'de [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) deyimini kullanarak sunucu düzeyinde ek bir güvenlik duvarı kuralı oluşturma hakkında da bilgi sağlar. 

> [!IMPORTANT]
>Sunucu düzeyindeki ilk güvenlik duvarı kuralı mutlaka Azure’da oluşturulmalıdır (Azure portalında, PowerShell veya REST API kullanılarak).
>

> [!IMPORTANT]
> Sunucu yöneticisinin veritabanı oluşturma izinlerini başka bir kullanıcıya devredebilmesi için asıl veritabanında oturumlar oluşturması ve bir oturumdan kullanıcı hesabı oluşturması gerekir. Bununla birlikte, önce çeşitli oturumlar oluşturup sonra bu oturumlardan kullanıcı oluşturmak, ortamınızın taşınabilirliğini azaltır.
>

1. Sunucu yönetici hesabıyla SQL Veritabanına bağlanmış olan bilgisayara geçin.
2. Asıl veritabanına bağlandıktan sonra bir sorgu penceresi açın ve aşağıdaki deyimi yürüterek asıl veritabanında dbcreator adlı bir oturum açma bilgisi oluşturun.

   ```
   CREATE LOGIN dbcreator
   WITH PASSWORD = 'p@ssw0rd';
   ```

3. Aynı sorgu penceresinde, 

   ```
   CREATE USER dbcreator
   FROM LOGIN dbcreator;
   ```

3. Aynı sorgu penceresinde aşağıdaki sorguyu yürüterek dbcreator kullanıcısını dbmanager veritabanı rolüne ekleyin. Bu kullanıcı artık veritabanı oluşturabilir ve oluşturduğu veritabanlarını yönetebilir.

   ```
   ALTER ROLE dbmanager ADD MEMBER dbcreator; 
   ```

4. Aynı sorgu penceresinde, ortamınız için uygun bir IP adresiyle [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) komutunu yürüterek sunucu düzeyinde bir güvenlik duvarı oluşturmak için aşağıdaki sorguyu yürütün:

   ```
   EXEC sp_set_firewall_rule @name = N'dbcreatorFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

5. Bilgisayar değiştirin (sunucu düzeyinde güvenlik duvarını oluşturduğunuz bilgisayara geçin) ve asıl veritabanına dbcreator kullanıcı hesabını kullanarak bağlanın.
6. Asıl veritabanında bir sorgu penceresi açın ve aşağıdaki sorguyu yürüterek foo adlı bir veritabanı oluşturun.

   ```
   CREATE DATABASE FOO (EDITION = 'basic');
   ```
 7. İsterseniz aşağıdaki deyimi kullanarak bu veritabanını silebilir, ücretlendirmeyi durdurabilirsiniz:

   ```
   DROP DATABASE FOO;
   ```

## <a name="complete-script"></a>Betiğin tamamı

Oturum açma bilgisi ve kullanıcı oluşturmak, onları rollere eklemek, onlara izin vermek, veritabanı düzeyinde güvenlik duvarı kuralları oluşturmak ve sunucu düzeyinde güvenlik duvarı kuralları oluşturmak için aşağıdaki deyimleri sunucunuzdaki uygun veritabanlarında yürütün.

### <a name="master-database"></a>asıl veritabanı
Bu deyimleri uygun IP adreslerini veya aralığını ekleme Server Yönetici hesabı kullanarak asıl veritabanında yürütün.

```
CREATE LOGIN dbcreator WITH PASSWORD = 'p@ssw0rd';
CREATE USER dbcreator FROM LOGIN dbcreator;
ALTER ROLE dbmanager ADD MEMBER dbcreator;
EXEC sp_set_firewall_rule @name = N'dbcreatorFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
```

### <a name="sqldbtutorialdb-database"></a>sqldbtutorialdb database
Bu deyimleri sunucu yöneticisi hesabını kullanarak ve uygun IP adreslerini veya aralığını ekleyerek sqldbtutorialdb veritabanında yürütün.

```
CREATE USER user1 WITH PASSWORD = 'p@ssw0rd';
GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to user1;
EXEC sp_set_database_firewall_rule @name = N'sqldbtutorialdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
```

### <a name="blankdb-database"></a>blankdb veritabanı
Bu deyimleri uygun IP adreslerini veya aralığını ekleme Server Yönetici hesabı kullanarak blankdb veritabanında yürütün.

```
CREATE USER blankdbadmin
   WITH PASSWORD = 'p@ssw0rd';
ALTER ROLE db_owner ADD MEMBER blankdbadmin;
EXEC sp_set_database_firewall_rule @name = N'blankdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
CREATE USER blankdbuser1
   WITH PASSWORD = 'p@ssw0rd';
```

## <a name="next-steps"></a>Sonraki adımlar
- SQL Veritabanında erişim ve denetime genel bakış için bkz. [SQL Veritabanında erişim ve denetim](sql-database-control-access.md).
- SQL Veritabanındaki oturum açma bilgileri, kullanıcılar ve veritabanı rollerine genel bakış için bkz. [Oturum açma bilgileri, kullanıcılar ve veritabanı rolleri](sql-database-manage-logins.md).
- Veritabanı sorumluları hakkında daha fazla bilgi için bkz. [Sorumlular](https://msdn.microsoft.com/library/ms181127.aspx).
- Veritabanı rolleri hakkında daha fazla bilgi için bkz. [Veritabanı rolleri](https://msdn.microsoft.com/library/ms189121.aspx).
- SQL Veritabanındaki güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [SQL Veritabanı güvenlik duvarı kuralları](sql-database-firewall-configure.md).
- Azure Active Directory kimlik doğrulamasını kullanmaya yönelik bir öğretici için bkz. [Azure AD kimlik doğrulaması ve yetkilendirme](sql-database-control-access-aad-authentication-get-started.md).


