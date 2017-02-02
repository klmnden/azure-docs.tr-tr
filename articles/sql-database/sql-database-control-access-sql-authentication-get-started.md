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
ms.date: 01/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 356cc4c6d8e25d36880e4b12bf471326e61990c3
ms.openlocfilehash: 275a33567fa1472573bc8abc87948ad306e853f0


---
# <a name="sql-database-tutorial-sql-server-authentication-logins-and-user-accounts-database-roles-permissions-server-level-firewall-rules-and-database-level-firewall-rules"></a>SQL Veritabanı Öğreticisi: SQL Server kimlik doğrulaması, oturum açma bilgileri ve kullanıcı hesapları, veritabanı kuralları, izinler, sunucu düzeyinde güvenlik duvarı kuralları ve veritabanı düzeyinde güvenlik duvarı kuralları
Bu başlangıç öğreticisinde SQL Server Management Studio kullanarak SQL Server kimlik doğrulaması, oturum açma bilgileri, kullanıcılar ve veritabanı rolleriyle çalışarak Azure SQL Veritabanı sunucuları ve veritabanlarına erişim izni vermeyi öğreneceksiniz. Şunları öğreneceksiniz:

- Asıl veritabanı ve kullanıcı veritabanlarındaki kullanıcı izinlerini görüntüleme
- SQL Server kimlik doğrulamasını temel alan oturum açma bilgileri ve kullanıcılar oluşturma
- Kullanıcılara sunucu çapında ve veritabanına özgü izinler verme
- Bir veritabanında yönetici olmayan kullanıcıyla oturum açma
- Veritabanı kullanıcıları için veritabanı düzeyinde güvenlik duvarı kuralları oluşturma
- Sunucu yöneticileri için sunucu düzeyinde güvenlik duvarı kuralları oluşturma

**Tahmini süre**: Bu öğreticinin tamamlanması yaklaşık 45 dakika alacaktır (önkoşulları karşıladığınız varsayılarak).

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure hesabınız olmalıdır. [Ücretsiz bir Azure hesabı açabilir](/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abonelik avantajlarını etkinleştirebilirsiniz](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Azure portalına, abonelik sahibi veya katkıda bulunan rolü üyesi olan bir hesap kullanarak bağlanabiliyor olmanız gerekir. Rol tabanlı erişim denetimi (RBAC) ile ilgili daha fazla bilgi için bkz. [Azure portalında erişim yönetimini kullanmaya başlama](../active-directory/role-based-access-control-what-is.md).

* [Azure portalı ve SQL Server Management Studio aracılığıyla Azure SQL Veritabanı sunucularını, veritabanlarını ve güvenlik duvarı kurallarını kullanmaya başlama](sql-database-get-started.md) öğreticisini veya bu öğreticinin [PowerShell sürümünü](sql-database-get-started-powershell.md) tamamladınız. Tamamlamadıysanız, bu öğretici önkoşulunu tamamlayın veya devam etmeden önce bu öğreticinin [PowerShell sürümünün](sql-database-get-started-powershell.md) sonundaki PowerShell betiğini yürütün.

> [!NOTE]
> Bu öğretici, şu konuların içeriğini öğrenmenize yardımcı olur: [SQL Veritabanında erişim ve denetim](sql-database-control-access.md), [Oturum açma bilgileri, kullanıcılar ve veritabanı rolleri](sql-database-manage-logins.md), [Sorumlular](https://msdn.microsoft.com/library/ms181127.aspx), [Veritabanı rolleri](https://msdn.microsoft.com/library/ms189121.aspx) ve [SQL Veritabanı güvenlik duvarı kuralları](sql-database-firewall-configure.md).
>  

## <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Azure hesabınızı kullanarak Azure portalında oturum açma
[Var olan aboneliğinizi](https://account.windowsazure.com/Home/Index) kullanarak Azure portala bağlanmak için aşağıdaki adımları uygulayın.

1. Tercih ettiğiniz tarayıcınızı açın ve [Azure portal](https://portal.azure.com/)’a bağlanın.
2. [Azure Portal](https://portal.azure.com/) oturum açın.
3. **Oturum açma** sayfasında aboneliğinize ait kimlik bilgilerini sağlayın.
   
   ![Oturum aç](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="view-information-about-the-security-configuration-for-your-logical-server"></a>Mantıksal sunucunuzun güvenlik yapılandırması hakkındaki bilgileri görüntüleme

Öğreticinin bu bölümünde Azure portalında mantıksal sunucunuzun güvenlik yapılandırması bilgilerini görüntüleyeceksiniz.

1. Mantıksal sunucunuzun **SQL Server** dikey penceresini açın ve **Genel bakış** sayfasındaki bilgileri inceleyin.

   ![Azure portalında sunucu yönetici hesabı](./media/sql-database-control-access-sql-authentication-get-started/sql_admin_portal.png)

2. Mantıksal sunucudaki Sunucu yönetici hesabının adını not edin. Parolayı anımsamıyorsanız **Parolayı sıfırla**'ya tıklayarak yeni bir parola belirleyin.

> [!NOTE]
> Bu sunucunun bağlantı bilgilerini gözden geçirmek için [Sunucu ayarlarını görüntüleme veya güncelleştirme](sql-database-view-update-server-settings.md) bölümüne gidin. Bu öğretici dizisinde tam sunucu adı "sqldbtutorialserver.database.windows.net" olarak belirlenmiştir.
>

## <a name="connect-to-sql-server-using-sql-server-management-studio-ssms"></a>SQL sunucusuna, SQL Server Management Studio (SSMS) kullanarak bağlanma

1. Eğer henüz yapmadıysanız, [Download SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio’yu İndir) sayfasından SSMS’in en son sürümünü indirin ve yükleyin. SSMS'nin son sürümü, güncel kalabilmek için indirebileceğiniz yeni bir sürüm olduğunda size bir istem gönderir.

2. Yükledikten sonra, Windows arama kutusuna **Microsoft SQL Server Management Studio** yazın ve SSMS'yi açmak için **Enter**'a basın.

   ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)

3. **Sunucuya Bağlan** iletişim kutusunda SQL Server Kimlik Doğrulamasını kullanarak SQL sunucunuza bağlanmak için gereken bilgileri ve Sunucu yönetici hesabını girin.

   ![sunucuya bağlan](./media/sql-database-get-started/connect-to-server.png)

4. **Bağlan**'a tıklayın.

   ![sunucuya bağlanıldı](./media/sql-database-get-started/connected-to-server.png)

## <a name="view-the-server-admin-account-and-its-permissions"></a>Sunucu yönetici hesabını ve izinlerini görüntüleme 
Öğreticinin bu bölümünde sunucu yönetici hesabı ve asıl veritabanı ile kullanıcı veritabanlarındaki izinleri hakkındaki bilgileri görüntüleyeceksiniz.

1. Nesne Gezgini'nde **Güvenlik** ve **Oturum açma bilgileri**'ni genişleterek Azure SQL Veritabanı sunucunuzdaki var olan oturum açma bilgilerini görüntüleyin. Sunucu yönetici hesabı için sağlama sırasında belirtilen bir oturum açma bilgisi görünür. Bu öğretici dizisi için sqladmin oturum açma bilgisidir.

   ![Sunucu yöneticisi oturum açma](./media/sql-database-control-access-sql-authentication-get-started/server_admin_login.png)

2. Nesne Gezgini'nde sırasıyla **Veritabanları**, **Sistem veritabanı**, **asıl**, **Güvenlik** ve **Kullanıcılar** bölümlerini genişletin. Sunucu yöneticisi oturum açma bilgileri için asıl veritabanında oturum açma bilgilerindeki hesapla aynı ada sahip (adların eşleşmesi gerekmez ancak karışıklığı önlemek için tavsiye edilir) bir kullanıcı hesabı oluşturulduğuna dikkat edin.

   ![sunucu yöneticisi için asıl veritabanı kullanıcı hesabı](./media/sql-database-control-access-sql-authentication-get-started/master_database_user_account_for_server_admin.png)

   > [!NOTE]
   > Görünen diğer kullanıcı hesapları hakkında bilgi için bkz. [Sorumlular](https://msdn.microsoft.com/library/ms181127.aspx).
   >

3. Nesne Gezgini'nde **asıl**'a sağ tıklayıp **Yeni Sorgu**'yu seçerek asıl veritabanına bağlı bir sorgu penceresi açın.
4. Sorguyu yürüten kullanıcı hakkında bilgi almak için sorgu penceresinde aşağıdaki sorguyu yürütün. Bu sorguyu yürüten kullanıcı hesabı için sqladmin döndürülür (bu yordamın ilerleyen bölümlerinde kullanıcı veritabanını sorguladığımızda farklı bir sonuç göreceğiz).

   ```
   SELECT USER;
   ```

   ![asıl veritabanında kullanıcı seçme sorgusu](./media/sql-database-control-access-sql-authentication-get-started/select_user_query_in_master_database.png)

5. sqladmin kullanıcısının izinleri hakkında bilgi almak için sorgu penceresinde aşağıdaki sorguyu yürütün. sqladmin kullanıcısının asıl veritabanına bağlanma, oturum açma bilgisi ve kullanıcı oluşturma, sys.sql_logins tablosundan bilgi seçme ve dbmanager ile dbcreator veritabanı rollerine kullanıcı ekleme izinlerine sahip olduğunu göreceksiniz. Bu izinler, tüm kullanıcıların izinleri devraldığı ortak role verilen izinlere (belirli tablolardan bilgi seçme izinleri gibi) ek olarak verilmiştir. Daha fazla bilgi için bkz. [İzinler](https://msdn.microsoft.com/library/ms191291.aspx).

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

6. Nesne Gezgini'nde sırasıyla **blankdb**, **Güvenlik** ve **Kullanıcılar** bölümlerini genişletin. Bu veritabanında sqladmin adlı bir kullanıcı hesabı olmadığını göreceksiniz.

   ![blankdb içindeki kullanıcı hesapları](./media/sql-database-control-access-sql-authentication-get-started/user_accounts_in_blankdb.png)

7. Nesne Gezgini'nde **blankdb**'ye sağ tıklayıp **Yeni Sorgu**'ya tıklayın.

8. Sorguyu yürüten kullanıcı hakkında bilgi almak için sorgu penceresinde aşağıdaki sorguyu yürütün. Bu sorguyu yürüten kullanıcı hesabı için dbo döndürülür (varsayılan olarak Sunucu yöneticisi oturum açma bilgisi, her bir kullanıcı veritabanındaki dbo kullanıcı hesabına eşlenir).

   ```
   SELECT USER;
   ```

   ![blankdb veritabanında kullanıcı seçme sorgusu](./media/sql-database-control-access-sql-authentication-get-started/select_user_query_in_blankdb_database.png)

9. dbo kullanıcısının izinleri hakkında bilgi almak için sorgu penceresinde aşağıdaki sorguyu yürütün. dbo aynı zamanda ortak rolün ve db_owner sabit veritabanı rolünün üyesidir. Daha fazla bilgi için bkz. [Veritabanı Düzeyinde Roller](https://msdn.microsoft.com/library/ms189121.aspx).

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

10. İsterseniz önceki üç adımı AdventureWorksLT kullanıcı veritabanı için tekrarlayabilirsiniz.

## <a name="create-a-new-user-in-the-adventureworkslt-database-with-select-permissions"></a>AdventureWorksLT veritabanında SELECT izinlerine sahip yeni bir kullanıcı oluşturma

Öğreticinin bu bölümünde AdventureWorksLT veritabanında bir kullanıcı hesabı oluşturacak, ortak rolün üyesi olan bu kullanıcının izinlerini test edecek, bu kullanıcıya SELECT izinleri verecek ve ardından bu kullanıcının izinlerini tekrar test edeceksiniz.

> [!NOTE]
> Veritabanı düzeyinde kullanıcılar ([bağımsız kullanıcılar](https://msdn.microsoft.com/library/ff929188.aspx)) veritabanınızın taşınabilirlik düzeyini artırır. Bu özelliği sonraki öğreticilerde inceleyeceğiz.
>

1. Nesne Gezgini'nde **AdventureWorksLT**'ye sağ tıklayıp **Yeni Sorgu**'yu seçerek AdventureWorksLT veritabanına bağlı bir sorgu penceresi açın.
2. AdventureWorksLT veritabanında user1 adlı bir kullanıcı oluşturmak için aşağıdaki deyimi yürütün.

   ```
   CREATE USER user1
   WITH PASSWORD = 'p@ssw0rd';
   ```
   ![yeni kullanıcı user1 AdventureWorksLT](./media/sql-database-control-access-sql-authentication-get-started/new_user_user1_aw.png)

3. user1 kullanıcısının izinleri hakkında bilgi almak için sorgu penceresinde aşağıdaki sorguyu yürütün. user1 kullanıcısının yalnızca ortak rolden devralınan izinlere sahip olduğuna dikkat edin.

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

4. user1 kullanıcısıyla AdventureWorksLT veritabanında tablo sorgulamak için aşağıdaki sorguları yürütün.

   ```
   EXECUTE AS USER = 'user1';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![select izni yok](./media/sql-database-control-access-sql-authentication-get-started/no_select_permissions.png)

5. user1 kullanıcısına SalesLT şemasının ProductCategory tablosunda SELECT izinlerini vermek için aşağıdaki deyimi yürütün.

   ```
   GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to user1;
   ```

   ![select izinleri verme](./media/sql-database-control-access-sql-authentication-get-started/grant_select_permissions.png)

6. user1 kullanıcısıyla AdventureWorksLT veritabanında tablo sorgulamak için aşağıdaki sorguları yürütün.

   ```
   EXECUTE AS USER = 'user1';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![select izinleri](./media/sql-database-control-access-sql-authentication-get-started/select_permissions.png)

## <a name="create-a-database-level-firewall-rule-for-an-adventureworkslt-database-user"></a>AdventureWorksLT veritabanı kullanıcısı için veritabanı düzeyinde güvenli duvarı oluşturma

Öğreticinin bu bölümünde farklı IP adresine sahip bir bilgisayardan oturum açmayı deneyecek, Sunucu yöneticisi olarak veritabanı düzeyinde güvenlik duvarı kuralı oluşturacak ve ardından bu yeni veritabanı düzeyinde güvenlik duvarını kullanarak oturum açacaksınız. 

> [!NOTE]
> [Veritabanı düzeyinde güvenlik duvarı kuralları](sql-database-firewall-configure.md) veritabanınızın taşınabilirlik düzeyini artırır. Bu özelliği sonraki öğreticilerde inceleyeceğiz.
>

1. Önceden sunucu düzeyinde güvenlik duvarı kuralı oluşturmadığınız başka bir bilgisayarda SQL Server Management Studio'yu açın.

   > [!IMPORTANT]
   > Her zaman [SQL Server Management Studio'yu indirme](https://msdn.microsoft.com/library/mt238290.aspx) sayfasındaki talimatları kullanarak en son SSMS sürümünü kullanın. 
   >

2. **Sunucuya bağlan** penceresinde SQL Server kimlik doğrulamasını kullanarak user1 hesabıyla bağlantı kurmak için sunucu adını ve kimlik doğrulaması bilgilerini girin. 
    
   ![Güvenlik duvarı kuralı olmadan user1 olarak bağlanma1](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule1.png)

3. **Seçenekler**'e tıklayarak bağlanmak istediğiniz veritabanını belirtin ve ardından **Bağlantı Özellikleri** sekmesindeki **Veritabanına Bağlan** açılır kutusuna **AdventureWorksLT** yazın.
   
   ![Güvenlik duvarı kuralı olmadan user1 olarak bağlanma2](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule2.png)

4. **Bağlan**'a tıklayın. SQL Veritabanına bağlanmaya çalıştığınız bilgisayarın veritabanına erişim sağlayan güvenlik duvarı kuralına sahip olmadığını belirten bir iletişim kutusu açılır. Açılan iletişim kutusu daha önce güvenlik duvarlarında yaptığınız işlemlere bağlı olarak iki farklı şekilde görünebilir ancak genelde aşağıdaki birinci iletişim kutusu gösterilir.

   ![Güvenlik duvarı kuralı olmadan user1 olarak bağlanma3](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule3.png)

   ![Güvenlik duvarı kuralı olmadan user1 olarak bağlanma4](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule4.png)

   > [!NOTE]
   > En yeni SSMS sürümlerinde abonelik kullanıcılarına ve katkıda bulunanlara Microsoft Azure oturumu açma ve sunucu düzeyinde güvenlik duvarı kuralı oluşturma özelliği bulunmaktadır.
   > 

4. Bu iletişim kutusundaki istemci IP adresini 7. adımda kullanmak üzere kopyalayın.
5. **İptal**'e tıklayın ancak **Sunucuya Bağlan** iletişim kutusunu kapatmayın.
6. Önceden sunucu düzeyinde güvenlik duvarı kuralı oluşturduğunuz bilgisayara dönün ve Sunucu yönetici hesabınızı kullanarak sunucuya bağlanın.
7. AdventureWorksLT veritabanına Sunucu yöneticisi olarak bağlandıktan sonra yeni sorgu penceresinde aşağıdaki [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) deyimini 4. adımdaki IP adresiyle yürüterek veritabanı düzeyinde güvenlik duvarı oluşturun:

   ```
   EXEC sp_set_database_firewall_rule @name = N'AdventureWorksLTFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

   ![güvenlik duvarı kuralı ekleme](./media/sql-database-control-access-sql-authentication-get-started/user1_add_rule_aw.png)

8. Yeniden diğer bilgisayara geçin ve **Sunucuya Bağlan** iletişim kutusunda **Bağlan**'a tıklayarak AdventureWorksLT veritabanına user1 olarak bağlanın. 

   ![Güvenlik duvarı kuralıyla user1 olarak bağlanma 1](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_rule1.png)

9. Nesne Gezgini'nde sırasıyla **Veritabanları**, **AdventureWorksLT** ve **Tablolar** bölümlerini genişletin. user1 kullanıcısının yalnızca **SalesLT.ProductCategory** tablosunu görüntüleme iznine sahip olduğuna dikkat edin. 

   ![user1 olarak bağlanma ve nesneleri görüntüleme 1](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_view_objects1.png)

10. Nesne Gezgini'nde **SalesLT.ProductCategory**'ye sağ tıklayın ve **İlk 1000 Satırı Seç**'e tıklayın.   

   ![user1 sorgu 1](./media/sql-database-control-access-sql-authentication-get-started/user1_query1.png)

   ![user1 sorgu 1 sonuçları](./media/sql-database-control-access-sql-authentication-get-started/user1_query1_results.png)

## <a name="create-a-new-user-in-the-blankdb-database-with-dbowner-database-role-permissions-and-a-database-level-firewall-rule"></a>blankdb veritabanında db_owner veritabanı rolü izinleri ve veritabanı düzeyinde güvenlik duvarı kuralıyla yeni bir kullanıcı oluşturma

Öğreticinin bu bölümünde blankdb veritabanında db_owner veritabanı rolü izinlerine sahip bir kullanıcı ve Sunucu yönetici hesabını kullanarak bu veritabanı için veritabanı düzeyinde güvenlik duvarı oluşturacaksınız. 

1. Sunucu yönetici hesabıyla SQL Veritabanına bağlı olan bilgisayara geçin.
2. blankdb veritabanına bağlandıktan sonra bir sorgu penceresi açın ve aşağıdaki deyimi yürüterek blankdb veritabanında blankadmin adlı bir kullanıcı oluşturun.

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
 
7. Öğrenme ortamınız için gerekli olması halinde bu kullanıcı için ek bir veritabanı düzeyinde güvenlik duvarı kuralı oluşturabilirsiniz. 

## <a name="create-a-new-login-and-user-in-the-master-database-with-dbmanager-permissions-and-create-a-server-level-firewall-rule"></a>Asıl veritabanında dbmanager izinlerine sahip yeni bir oturum açma adı, kullanıcı ve sunucu düzeyinde güvenlik duvarı oluşturma

Öğreticinin bu bölümünde asıl veritabanında yeni kullanıcı veritabanlarını oluşturma ve yönetme izinlerine sahip bir oturum açma adı ve kullanıcı oluşturacaksınız. Ayrıca Transact-SQL'de [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) deyimini kullanarak ek bir sunucu düzeyinde güvenlik duvarı kuralı da oluşturacaksınız.

> [!NOTE]
> Sunucu yönetici hesabı sahibinin başka bir kullanıcıya veritabanı oluşturma izinlerini verebilmesi için asıl veritabanında oturum açma bilgisi oluşturma ve oturum açma bilgisinden kullanıcı hesabı oluşturma gereklidir. Ancak oturum açma bilgisi oluşturma ve oturum açma bilgilerinden kullanıcı oluşturma, olağanüstü durum kurtarma planlamasının bir parçası olarak planlama ve gerçekleştirme dahil olmak üzere ortamınızın taşınabilirliğini azaltır. Bunun sonuçları sonraki öğreticilerde incelenecektir.
>

1. Sunucu yönetici hesabıyla SQL Veritabanına bağlı olan bilgisayara geçin.
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

4. Aynı sorgu penceresinde aşağıdaki [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) sorgusunu ortamınıza uygun bir IP adresiyle yürüterek sunucu düzeyinde güvenlik duvarı oluşturun:

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

### <a name="adventureworkslt-database"></a>AdventureWorksLT veritabanı
Bu deyimleri uygun IP adreslerini veya aralığını ekleme Server Yönetici hesabı kullanarak AdventureWorksLT veritabanında yürütün.

```
CREATE USER user1 WITH PASSWORD = 'p@ssw0rd';
GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to user1;
EXEC sp_set_database_firewall_rule @name = N'AdventureWorksLTFirewallRule', 
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
- Azure Active Directory kimlik doğrulamasını kullanan bir öğretici için bkz. [SQL Veritabanı Öğreticisi: AAD kimlik doğrulaması, oturum açma bilgileri ve kullanıcı hesapları, veritabanı kuralları, izinler, sunucu düzeyinde güvenlik duvarı kuralları ve veritabanı düzeyinde güvenlik duvarı kuralları](sql-database-control-access-sql-authentication-get-started.md).




<!--HONumber=Jan17_HO3-->


