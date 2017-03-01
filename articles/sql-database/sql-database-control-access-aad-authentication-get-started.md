---
title: "AAD kimlik doğrulaması: Azure SQL Veritabanı güvenlik duvarları, kimlik doğrulaması, erişim | Microsoft Belgeleri"
description: "Bu başlangıç öğreticisinde SQL Server Management Studio ve Transact-SQL kullanarak sunucu ve veritabanı düzeyinde güvenlik duvarları, Azure Active Directory kimlik doğrulaması, oturum açma bilgileri, kullanıcılar ve rollerle çalışarak Azure SQL Veritabanı sunucuları ve veritabanlarına erişim izni vermeyi ve bunu kontrol etmeyi öğreneceksiniz."
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
ms.sourcegitcommit: 7d061c083b23de823d373c30f93cccfe1c856ba3
ms.openlocfilehash: 8a6dc7d3dca80782a55e13b53180b1542b61544b
ms.lasthandoff: 02/18/2017


---
# <a name="azure-ad-authentication-access-and-database-level-firewall-rules"></a>Azure AD kimlik doğrulaması, erişimi ve veritabanı düzeyinde güvenlik duvarı kuralları
Bu öğreticide SQL Server Management Studio kullanarak Azure Active Directory kimlik doğrulaması, oturumlar, kullanıcılar ve veritabanı rolleriyle çalışarak Azure SQL Veritabanı sunucuları ve veritabanlarına erişim izni vermeyi öğreneceksiniz. Şunları öğreneceksiniz:

- Asıl veritabanı ve kullanıcı veritabanlarındaki kullanıcı izinlerini görüntüleme
- Azure Active Directory kimlik doğrulamasını temel alan oturum açma bilgileri ve kullanıcılar oluşturma
- Kullanıcılara sunucu çapında ve veritabanına özgü izinler verme
- Bir veritabanında yönetici olmayan kullanıcıyla oturum açma
- Veritabanı kullanıcıları için veritabanı düzeyinde güvenlik duvarı kuralları oluşturma
- Sunucu yöneticileri için sunucu düzeyinde güvenlik duvarı kuralları oluşturma

**Tahmini süre**: Bu öğreticinin tamamlanması yaklaşık 45 dakika alacaktır (önkoşulları karşıladığınız varsayılarak).

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure hesabınız olmalıdır. [Ücretsiz bir Azure hesabı açabilir](https://azure.microsoft.com/free/) veya [Visual Studio abonelik avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/). 

* Azure portalına, abonelik sahibi veya katkıda bulunan rolü üyesi olan bir hesap kullanarak bağlanabiliyor olmanız gerekir. Rol tabanlı erişim denetimi (RBAC) ile ilgili daha fazla bilgi için bkz. [Azure portalında erişim yönetimini kullanmaya başlama](../active-directory/role-based-access-control-what-is.md).

* [Azure portalı ve SQL Server Management Studio aracılığıyla Azure SQL Veritabanı sunucularını, veritabanlarını ve güvenlik duvarı kurallarını kullanmaya başlama](sql-database-get-started.md) öğreticisini veya bu öğreticinin [PowerShell sürümünü](sql-database-get-started-powershell.md) tamamladınız. Tamamlamadıysanız, bu öğretici önkoşulunu tamamlayın veya devam etmeden önce bu öğreticinin [PowerShell sürümünün](sql-database-get-started-powershell.md) sonundaki PowerShell betiğini yürütün.

   > [!NOTE]
   > SQL Server kimlik doğrulamasıyla ilgili bir öğretici olan [SQL kimlik doğrulaması, oturumlar ve kullanıcı hesapları, veritabanı rolleri, izinler, sunucu düzeyinde güvenlik duvarı kuralları ve veritabanı düzeyinde güvenlik duvarı kuralları](sql-database-control-access-sql-authentication-get-started.md) öğreticisini tamamlamak isteğe bağlıdır, ancak belirtilen öğreticide yer verilen kavramlar burada tekrarlanmamaktadır. İlgili öğreticiyi aynı bilgisayarlarda tamamladıysanız (aynı IP adreslerine sahip) bu öğreticideki sunucu ve veritabanı düzeyinde güvenlik duvarlarıyla ilgili yordamlar gerekli değildir ve bu nedenle isteğe bağlı olarak işaretlenmiştir. Ayrıca bu öğreticideki ekran görüntülerinde de ilgili öğreticiyi tamamladığınız kabul edilmiştir. 
   >

* Azure Active Directory oluşturdunuz ve kayıtlarla doldurdunuz. Daha fazla bilgi edinmek için bkz. [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../active-directory/active-directory-aadconnect.md), [Kendi etki alanı adınızı Azure AD'ye ekleme](../active-directory/active-directory-add-domain.md), [Microsoft Azure artık Windows Server Active Directory ile federasyonu destekliyor](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Azure AD dizininizi yönetme](https://msdn.microsoft.com/library/azure/hh967611.aspx), [Azure AD'yi Windows PowerShell kullanarak yönetme](https://msdn.microsoft.com/library/azure/jj151815.aspx) ve [Karma Kimlik için gerekli bağlantı noktaları ve protokoller](../active-directory/active-directory-aadconnect-ports.md).

> [!NOTE]
> Bu öğretici, şu konuların içeriğini öğrenmenize yardımcı olur: [SQL Veritabanında erişim ve denetim](sql-database-control-access.md), [Oturum açma bilgileri, kullanıcılar ve veritabanı rolleri](sql-database-manage-logins.md), [Sorumlular](https://msdn.microsoft.com/library/ms181127.aspx), [Veritabanı rolleri](https://msdn.microsoft.com/library/ms189121.aspx), [SQL Veritabanı güvenlik duvarı kuralları](sql-database-firewall-configure.md) ve [Azure Active Directory kimlik doğrulaması](sql-database-aad-authentication.md). 
>  

## <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Azure hesabınızı kullanarak Azure portalında oturum açma
[Var olan aboneliğinizi](https://account.windowsazure.com/Home/Index) kullanarak Azure portala bağlanmak için aşağıdaki adımları uygulayın.

1. Tercih ettiğiniz tarayıcınızı açın ve [Azure portal](https://portal.azure.com/)’a bağlanın.
2. [Azure Portal](https://portal.azure.com/) oturum açın.
3. **Oturum açma** sayfasında aboneliğinize ait kimlik bilgilerini sağlayın.
   
   ![Oturum aç](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="provision-an-azure-active-directory-admin-for-your-sql-logical-server"></a>SQL mantıksal sunucunuz için bir Azure Active Directory yöneticisi sağlama

Öğreticinin bu bölümünde Azure portalında mantıksal sunucunuzun güvenlik yapılandırması bilgilerini görüntüleyeceksiniz.

1. Mantıksal sunucunuzun **SQL Server** dikey penceresini açın ve **Genel bakış** sayfasındaki bilgileri inceleyin. Azure Active Directory yöneticisinin yapılandırılmamış olduğunu göreceksiniz.

   ![Azure portalında sunucu yönetici hesabı](./media/sql-database-control-access-aad-authentication-get-started/sql_admin_portal.png)

2. **Temel Bileşenler** bölmesindeki **Yapılandırılmamış**'a tıklayarak **Active Directory yöneticisi** dikey penceresini açın.

   ![AAD dikey penceresi](./media/sql-database-control-access-aad-authentication-get-started/aad_blade.png)

3. **Yönetici ayarlar**'ya tıklayarak **Yönetici ekle** dikey penceresini açın ve sunucunuzun Active Directory yöneticisi olarak belirlemek istediğiniz Active Directory kullanıcı veya grup hesabını seçin.

   ![AAD yönetici hesabı seçme](./media/sql-database-control-access-aad-authentication-get-started/aad_admin.png)

4. **Seç**'e ve ardından **Kaydet**'e tıklayın.

   ![Seçilen AAD yönetici hesabını kaydetme](./media/sql-database-control-access-aad-authentication-get-started/aad_admin_save.png)

> [!NOTE]
> Bu sunucunun bağlantı bilgilerini gözden geçirmek için [Sunucuları yönetme](sql-database-manage-servers-portal.md) bölümüne gidin. Bu öğretici dizisinde tam sunucu adı "sqldbtutorialserver.database.windows.net" olarak belirlenmiştir.
>

## <a name="connect-to-sql-server-using-sql-server-management-studio-ssms"></a>SQL sunucusuna, SQL Server Management Studio (SSMS) kullanarak bağlanma

1. Eğer henüz yapmadıysanız, [Download SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio’yu İndir) sayfasından SSMS’in en son sürümünü indirin ve yükleyin. SSMS'nin son sürümü, güncel kalabilmek için indirebileceğiniz yeni bir sürüm olduğunda size bir istem gönderir.

2. Yükledikten sonra, Windows arama kutusuna **Microsoft SQL Server Management Studio** yazın ve SSMS'yi açmak için **Enter**'a basın.

   ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)

3. **Sunucuya bağlan** iletişim kutusunda Active Directory kimlik doğrulama yöntemlerinden birini seçin ve uygun kimlik doğrulama bilgilerini girin. Yöntem seçme hakkında bilgi için bkz. [Azure Active Directory kimlik doğrulaması](sql-database-aad-authentication.md) ve [Azure AD MFA için SSMS desteği](sql-database-ssms-mfa-authentication.md).

   ![aad ile sunucuya bağlanma](./media/sql-database-control-access-aad-authentication-get-started/connect_to_server_with_aad.png)

4. SQL Server Kimlik Doğrulamasını kullanarak SQL sunucunuza bağlanmak için gereken bilgileri ve Sunucu yönetici hesabını girin.

5. **Bağlan**'a tıklayın.

   ![sunucuya aad ile bağlandı](./media/sql-database-control-access-aad-authentication-get-started/connected_to_server_with_aad.png)

## <a name="view-the-server-admin-account-and-its-permissions"></a>Sunucu yönetici hesabını ve izinlerini görüntüleme 
Öğreticinin bu bölümünde sunucu yönetici hesabı ve asıl veritabanı ile kullanıcı veritabanlarındaki izinleri hakkındaki bilgileri görüntüleyeceksiniz.

1. Nesne Gezgini'nde sırasıyla **Veritabanları**, **Sistem veritabanı**, **asıl**, **Güvenlik** ve **Kullanıcılar** bölümlerini genişletin. Active Directory yöneticisi için asıl veritabanında bir kullanıcı hesabı oluşturulmuş olduğunu göreceksiniz. Active Directory yöneticisi kullanıcı hesabı için oturum açma bilgisi oluşturulmadığını da göreceksiniz.

   ![AAD yöneticisi için asıl veritabanı kullanıcı hesabı](./media/sql-database-control-access-aad-authentication-get-started/master_database_user_account_for_AAD_admin.png)

   > [!NOTE]
   > Görünen diğer kullanıcı hesapları hakkında bilgi için bkz. [Sorumlular](https://msdn.microsoft.com/library/ms181127.aspx).
   >

2. Nesne Gezgini'nde **asıl**'a sağ tıklayıp **Yeni Sorgu**'yu seçerek asıl veritabanına bağlı bir sorgu penceresi açın.
3. Sorguyu yürüten kullanıcı hakkında bilgi almak için sorgu penceresinde aşağıdaki sorguyu yürütün. Bu sorguyu yürüten kullanıcı hesabı için user@microsoft.com döndürülür (bu yordamın ilerleyen bölümlerinde kullanıcı veritabanını sorguladığımızda farklı bir sonuç göreceğiz).

   ```
   SELECT USER;
   ```

   ![asıl veritabanında kullanıcı seçme sorgusu](./media/sql-database-control-access-aad-authentication-get-started/select_user_query_in_master_database.png)

4. Active Directory yönetici kullanıcısının izinleri hakkında bilgi almak için sorgu penceresinde aşağıdaki sorguyu yürütün. Active Directory yönetici kullanıcısının asıl veritabanına bağlanma, oturum açma bilgisi ve kullanıcı oluşturma, sys.sql_logins tablosundan bilgi seçme ve dbmanager ile dbcreator veritabanı rollerine kullanıcı ekleme izinlerine sahip olduğunu göreceksiniz. Bu izinler, tüm kullanıcıların izinleri devraldığı ortak role verilen izinlere (belirli tablolardan bilgi seçme izinleri gibi) ek olarak verilmiştir. Daha fazla bilgi için bkz. [İzinler](https://msdn.microsoft.com/library/ms191291.aspx).

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
   WHERE p.name = 'user@microsoft.com';
   ```

   ![asıl veritabanındaki aad yönetici izinleri](./media/sql-database-control-access-aad-authentication-get-started/aad_admin_permissions_in_master_database.png)

6. Nesne Gezgini'nde sırasıyla **blankdb**, **Güvenlik** ve **Kullanıcılar** bölümlerini genişletin. Bu veritabanında user@microsoft.com adlı bir kullanıcı hesabı olmadığını göreceksiniz.

   ![blankdb içindeki kullanıcı hesapları](./media/sql-database-control-access-aad-authentication-get-started/user_accounts_in_blankdb.png)

7. Nesne Gezgini'nde **blankdb**'ye sağ tıklayıp **Yeni Sorgu**'ya tıklayın.

8. Sorguyu yürüten kullanıcı hakkında bilgi almak için sorgu penceresinde aşağıdaki sorguyu yürütün. Bu sorguyu yürüten kullanıcı hesabı için dbo döndürülür (varsayılan olarak Sunucu yöneticisi oturum açma bilgisi, her bir kullanıcı veritabanındaki dbo kullanıcı hesabına eşlenir).

   ```
   SELECT USER;
   ```

   ![blankdb veritabanında kullanıcı seçme sorgusu](./media/sql-database-control-access-aad-authentication-get-started/select_user_query_in_blankdb_database.png)

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

   ![blankdb veritabanındaki sunucu yöneticisi izinleri](./media/sql-database-control-access-aad-authentication-get-started/aad_admin_permissions_in_blankdb_database.png)

10. İsterseniz önceki üç adımı AdventureWorksLT kullanıcı veritabanı için tekrarlayabilirsiniz.

## <a name="create-a-new-user-in-the-adventureworkslt-database-with-select-permissions"></a>AdventureWorksLT veritabanında SELECT izinlerine sahip yeni bir kullanıcı oluşturma

Öğreticinin bu bölümünde AdventureWorksLT veritabanında bir kullanıcının Azure AD kullanıcısının asıl adını veya bir Azure AD grubunun görünen adını temel alan bir kullanıcı hesabı oluşturacak, ortak rolün üyesi olan bu kullanıcının izinlerini test edecek, bu kullanıcıya SELECT izinleri verecek ve ardından bu kullanıcının izinlerini tekrar test edeceksiniz.

> [!NOTE]
> Veritabanı düzeyinde kullanıcılar ([bağımsız kullanıcılar](https://msdn.microsoft.com/library/ff929188.aspx)) veritabanınızın taşınabilirlik düzeyini artırır. Bu özelliği sonraki öğreticilerde inceleyeceğiz.
>

1. Nesne Gezgini'nde **AdventureWorksLT**'ye sağ tıklayıp **Yeni Sorgu**'yu seçerek AdventureWorksLT veritabanına bağlı bir sorgu penceresi açın.
2. Aşağıdaki deyimi yürüterek aaduser1 adlı Microsoft etki alanı kullanıcısı için AdventureWorksLT veritabanında bir kullanıcı hesabı oluşturun.

   ```
   CREATE USER [aaduser1@microsoft.com]
   FROM EXTERNAL PROVIDER;
   ```
   ![yeni kullanıcı aaduser1@microsoft.com AdventureWorksLT](./media/sql-database-control-access-aad-authentication-get-started/new_user_aaduser1@microsoft.com_aw.png)

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
   WHERE p.name = 'aaduser1@microsoft.com';
   ```

   ![bir kullanıcı veritabanında yeni kullanıcı izinleri](./media/sql-database-control-access-aad-authentication-get-started/new_user_permissions_in_user_database.png)

4. user1 kullanıcısıyla AdventureWorksLT veritabanında tablo sorgulamak için aşağıdaki sorguları yürütün.

   ```
   EXECUTE AS USER = 'aaduser1@microsoft.com';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![select izni yok](./media/sql-database-control-access-aad-authentication-get-started/no_select_permissions.png)

5. user1 kullanıcısına SalesLT şemasının ProductCategory tablosunda SELECT izinlerini vermek için aşağıdaki deyimi yürütün.

   ```
   GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to [aaduser1@microsoft.com];
   ```

   ![select izinleri verme](./media/sql-database-control-access-aad-authentication-get-started/grant_select_permissions.png)

6. user1 kullanıcısıyla AdventureWorksLT veritabanında tablo sorgulamak için aşağıdaki sorguları yürütün.

   ```
   EXECUTE AS USER = 'aaduser1@microsoft.com';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![select izinleri](./media/sql-database-control-access-aad-authentication-get-started/select_permissions.png)

## <a name="create-a-database-level-firewall-rule-for-adventureworkslt-database-users"></a>AdventureWorksLT veritabanı kullanıcıları için veritabanı düzeyinde güvenli duvarı oluşturma

> [!NOTE]
> SQL Server kimlik doğrulamasına yönelik ilgili öğreticide ([SQL kimlik doğrulaması ve yetkilendirmesi](sql-database-control-access-sql-authentication-get-started.md)) açıklanan eşdeğer yordamı tamamladıysanız ve aynı IP adresine sahip aynı bilgisayarı kullanarak öğreniyorsanız bu yordamı tamamlamanız gerekmez.
>

Öğreticinin bu bölümünde farklı IP adresine sahip bir bilgisayardan yeni kullanıcı hesabını kullanarak oturum açmayı deneyecek, Sunucu yöneticisi olarak veritabanı düzeyinde güvenlik duvarı kuralı oluşturacak ve ardından bu yeni veritabanı düzeyinde güvenlik duvarını kullanarak başarıyla oturum açacaksınız. 

> [!NOTE]
> [Veritabanı düzeyinde güvenlik duvarı kuralları](sql-database-firewall-configure.md) veritabanınızın taşınabilirlik düzeyini artırır. Bu özelliği sonraki öğreticilerde inceleyeceğiz.
>

1. Önceden sunucu düzeyinde güvenlik duvarı kuralı oluşturmadığınız başka bir bilgisayarda SQL Server Management Studio'yu açın.

   > [!IMPORTANT]
   > Her zaman [SQL Server Management Studio'yu indirme](https://msdn.microsoft.com/library/mt238290.aspx) sayfasındaki talimatları kullanarak en son SSMS sürümünü kullanın. 
   >

2. **Sunucuya bağlan** penceresinde SQL Server kimlik doğrulamasını kullanarak aaduser1@microsoft.com hesabıyla bağlantı kurmak için sunucu adını ve kimlik doğrulaması bilgilerini girin. 
    
   ![Güvenlik duvarı kuralı olmadan aaduser1@microsoft.com olarak bağlanma&1;](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule1.png)

3. **Seçenekler**'e tıklayarak bağlanmak istediğiniz veritabanını belirtin ve ardından **Bağlantı Özellikleri** sekmesindeki **Veritabanına Bağlan** açılır kutusuna **AdventureWorksLT** yazın.
   
   ![Güvenlik duvarı kuralı olmadan aaduser1 olarak bağlanma2](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule2.png)

4. **Bağlan**'a tıklayın. SQL Veritabanına bağlanmaya çalıştığınız bilgisayarın veritabanına erişim sağlayan güvenlik duvarı kuralına sahip olmadığını belirten bir iletişim kutusu açılır. Açılan iletişim kutusu daha önce güvenlik duvarlarında yaptığınız işlemlere bağlı olarak iki farklı şekilde görünebilir ancak genelde aşağıdaki birinci iletişim kutusu gösterilir.

   ![Güvenlik duvarı kuralı olmadan user1 olarak bağlanma3](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule3.png)

   ![Güvenlik duvarı kuralı olmadan user1 olarak bağlanma4](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule4.png)

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

   ![veritabanı düzeyinde güvenlik duvarı ekleme&4;](./media/sql-database-control-access-aad-authentication-get-started/aaduser1_add_rule_aw.png)

8. Yeniden diğer bilgisayara geçin ve **Sunucuya Bağlan** iletişim kutusunda **Bağlan**'a tıklayarak AdventureWorksLT veritabanına aaduser1 olarak bağlanın. 

9. Nesne Gezgini'nde sırasıyla **Veritabanları**, **AdventureWorksLT** ve **Tablolar** bölümlerini genişletin. user1 kullanıcısının yalnızca **SalesLT.ProductCategory** tablosunu görüntüleme iznine sahip olduğuna dikkat edin. 

10. Nesne Gezgini'nde **SalesLT.ProductCategory**'ye sağ tıklayın ve **İlk 1000 Satırı Seç**'e tıklayın.   

## <a name="next-steps"></a>Sonraki adımlar
- SQL Veritabanında erişim ve denetime genel bakış için bkz. [SQL Veritabanında erişim ve denetim](sql-database-control-access.md).
- SQL Veritabanındaki oturum açma bilgileri, kullanıcılar ve veritabanı rollerine genel bakış için bkz. [Oturum açma bilgileri, kullanıcılar ve veritabanı rolleri](sql-database-manage-logins.md).
- Veritabanı sorumluları hakkında daha fazla bilgi için bkz. [Sorumlular](https://msdn.microsoft.com/library/ms181127.aspx).
- Veritabanı rolleri hakkında daha fazla bilgi için bkz. [Veritabanı rolleri](https://msdn.microsoft.com/library/ms189121.aspx).
- SQL Veritabanındaki güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [SQL Veritabanı güvenlik duvarı kuralları](sql-database-firewall-configure.md).


