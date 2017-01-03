---
title: "Bir SQL Veritabanını SSMS ile yönetme | Microsoft Belgeleri"
description: "SQL Server Management Studio&quot;yu kullanarak SQL Veritabanı sunucularını ve veritabanlarını nasıl yöneteceğinizi öğrenin."
services: sql-database
documentationcenter: .net
author: stevestein
manager: jhubbard
editor: tysonn
ms.assetid: da6f3608-5993-4134-a497-ff2811e9f31f
ms.service: sql-database
ms.custom: overview
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/29/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: d9ff74a49742fa77f5989b8b05e0567e3ca81dc5
ms.openlocfilehash: 89cb8827745b31b3a77b64d5cafd586957d60d30


---
# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>SQL Server Management Studio kullanarak Azure SQL Database’i yönetme
> [!div class="op_single_selector"]
> * [Azure portal](sql-database-manage-portal.md)
> * [SSMS](sql-database-manage-azure-ssms.md)
> * [PowerShell](sql-database-manage-powershell.md)
> 
> 

SQL Server Management Studio'yu (SSMS) kullanarak Azure SQL Veritabanı sunucularını ve veritabanlarını yönetebilirsiniz. Bu konu başlığı, sık kullanılan SSMS işlevlerini ayrıntılı bir şekilde açıklamaktadır. Başlamadan önce Azure SQL Veritabanında bir sunucu ve veritabanı oluşturmuş olmanız gerekir. Daha fazla bilgi için bkz. [İlk Azure SQL Veritabanınızı oluşturma](sql-database-get-started.md) ve [SSMS kullanarak bağlanma ve sorgulama](sql-database-connect-query-ssms.md).

> [!TIP]
> Sunucu oluşturma, sunucu tabanlı güvenlik duvarı oluşturma, sunucu özelliklerini görüntüleme, SQL Server Management Studio'yu kullanarak bağlanma, ana veritabanını sorgulama, örnek veritabanı ve boş veritabanı oluşturma, veritabanı özelliklerini sorgulama, SQL Server Management Studio kullanarak bağlanma ve örnek veritabanını sorgulama adımlarını gösteren bir öğreticiye ihtiyacınız varsa bkz. [Başlangıç Öğreticisi](sql-database-get-started.md).
>

Azure SQL Veritabanı ile çalışırken SSMS'nin en son sürümünü kullanmanız önerilir. 

> [!IMPORTANT]
> Azure ve SQL Veritabanında yapılan son güncelleştirmelerle çalışmak üzere sürekli geliştirildiğinden her zaman SSMS'nin en son sürümünü kullanın. En son sürümü edinmek için bkz. [SQL Server Management Studio'yu indirme](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="create-and-manage-azure-sql-databases"></a>Azure SQL veritabanlarını oluşturma ve yönetme
**Ana** veritabanına bağlıyken sunucuda veritabanı oluşturabilir ve var olan veritabanlarını değiştirebilir veya bırakabilirsiniz. Aşağıdaki adımlarda Management Studio ile sık kullanılan veritabanı yönetim görevlerini nasıl tamamlayacağınız açıklanmaktadır. Bu görevleri gerçekleştirmek için **ana** veritabanına sunucunuzu kurarken oluşturduğunuz sunucu düzeyi asıl oturum açma bilgileriyle bağlandığınızdan emin olun.

Management Studio'da sorgu penceresi açmak için Veritabanları klasörünü açın, **Sistem Veritabanları** klasörünü genişletin, **ana** girişine sağ tıklayın ve ardından **Yeni Sorgu**'ya tıklayın.

* Veritabanı oluşturmak için **CREATE DATABASE** deyimini kullanın. Daha fazla bilgi için bkz. [CREATE DATABASE (SQL Veritabanı)](https://msdn.microsoft.com/library/dn268335.aspx). Aşağıdaki deyim **myTestDB** adlı bir veritabanı oluşturur ve 250 GB maksimum boyuta sahip bir Standart S0 Sürümü veritabanı olduğunu belirtir.
  
      CREATE DATABASE myTestDB
      (EDITION='Standard',
       SERVICE_OBJECTIVE='S0');

Sorguyu çalıştırmak için **Yürüt**'e tıklayın.

* Var olan bir veritabanının adını ve sürümünü değiştirmek gibi değişiklikler yapmak için **ALTER DATABASE** deyimini kullanın. Daha fazla bilgi için bkz. [ALTER DATABASE (SQL Veritabanı)](https://msdn.microsoft.com/library/ms174269.aspx). Aşağıdaki deyim, önceki adımda oluşturduğunuz veritabanının sürümünü Standart S1 olarak değiştirir.
  
      ALTER DATABASE myTestDB
      MODIFY
      (SERVICE_OBJECTIVE='S1');
* Var olan bir veritabanını silmek için **DROP DATABASE** deyimini kullanın. Daha fazla bilgi için bkz. [DROP DATABASE (SQL Veritabanı)](https://msdn.microsoft.com/library/ms178613.aspx). Aşağıdaki deyim **myTestDB** veritabanını siler ancak sonraki adımlarda oturum açma bilgisi oluşturmak için kullanacağınızdan bu adımda çalıştırmayın.
  
      DROP DATABASE myTestBase;
* Ana veritabanındaki **sys.databases** görünümünü kullanarak tüm veritabanlarının ayrıntılarını görüntüleyebilirsiniz. Var olan tüm veritabanlarını görüntülemek için aşağıdaki deyimi çalıştırın:
  
      SELECT * FROM sys.databases;
* SQL Veritabanında **USE** deyimi, veritabanları arasında geçiş yapmak için desteklenmez. Bunun yerine hedef veritabanıyla doğrudan bağlantı kurmanız gerekir.

> [!NOTE]
> Veritabanı oluşturan veya değiştiren Transact-SQL deyimlerinin çoğu kendi toplu işi içinde çalıştırılmalıdır ve diğer Transact-SQL deyimleriyle gruplanamaz. Daha fazla bilgi için deyime özgü bilgilere bakın.
> 
> 

## <a name="create-and-manage-logins"></a>Oturum açma bilgisi oluşturma ve yönetme
**Ana** veritabanında oturum açma bilgileri ve bu oturum açma bilgilerinin hangilerinin veritabanı veya başka oturum açma bilgisi oluşturma iznine sahip olduğu bilgisi yer alır. Oturum açma bilgilerini yönetmek için **ana** veritabanına sunucunuzu kurarken oluşturduğunuz sunucu düzeyi asıl oturum açma bilgileriyle bağlanın. **CREATE LOGIN**, **ALTER LOGIN** veya **DROP LOGIN** deyimlerini kullanarak tüm sunucu için oturum açma bilgilerini yöneten ana veritabanında sorgu çalıştırabilirsiniz. Daha fazla bilgi için bkz. [SQL Veritabanında Veritabanlarını ve Oturum Açma Bilgilerini Yönetme](http://msdn.microsoft.com/library/azure/ee336235.aspx). 

* Sunucu düzeyi oturum açma bilgisi oluşturmak için **CREATE LOGIN** deyimini kullanın. Daha fazla bilgi için bkz. [CREATE LOGIN (SQL Veritabanı)](https://msdn.microsoft.com/library/ms189751.aspx). Aşağıdaki deyim **login1** adlı bir oturum açma bilgisi oluşturur. **password1** yerine istediğiniz parolayı yazabilirsiniz.
  
      CREATE LOGIN login1 WITH password='password1';
* Veritabanı düzeyinde izin vermek için **CREATE USER** deyimini kullanın. Tüm oturum açma bilgilerinin **ana** veritabanında oluşturulması gerekir. Bir oturum açma bilgisinin farklı bir veritabanına bağlanabilmesi için ilgili veritabanında **CREATE USER** deyimini kullanarak veritabanı düzeyinde izin vermeniz gerekir. Daha fazla bilgi için bkz. [CREATE USER (SQL Veritabanı)](https://msdn.microsoft.com/library/ms173463.aspx). 
* login1 adlı kullanıcıya **myTestDB** adlı veritabanında izin vermek için aşağıdaki adımları uygulayın:
  
  1. Oluşturduğunuz **myTestDB** adlı veritabanını görüntülemek üzere Nesne Gezginini yenilemek için Nesne Gezgininde sunucu adına sağ tıklayın ve **Yenile**'ye tıklayın.  
     
     Bağlantıyı kapattıysanız Dosya menüsünden **Nesne Gezginine Bağlan**'ı seçerek yeniden bağlanabilirsiniz.
  2. **myTestDB** veritabanına sağ tıklayın ve **Yeni Sorgu**'yu seçin.
  3. **login1** sunucu düzeyi oturum açma bilgisine karşılık gelen **login1User** adlı veritabanı kullanıcısını oluşturmak için aşağıdaki deyimi myTestDB veritabanında çalıştırın.
     
         CREATE USER login1User FROM LOGIN login1;
* Kullanıcı hesabına veritabanında uygun izin düzeyini vermek için **sp\_addrolemember** saklı yordamını kullanın. Daha fazla bilgi için bkz. [sp_addrolemember (Transact-SQL)](http://msdn.microsoft.com/library/ms187750.aspx). Aşağıdaki deyim **login1User** kullanıcısını **db\_datareader** rolüne ekleyerek **login1User** kullanıcısına veritabanı için yalnızca okuma izni verir.
  
      exec sp_addrolemember 'db_datareader', 'login1User';    
* Var olan oturum açma bilgilerinden birini değiştirmek için (oturum açma bilgilerinin parolasını değiştirme gibi) **ALTER LOGIN** deyimini kullanın. Daha fazla bilgi için bkz. [ALTER LOGIN (SQL Veritabanı)](https://msdn.microsoft.com/library/ms189828.aspx). **ALTER LOGIN** deyiminin **ana** veritabanında çalıştırılması gerekir. Veritabanına bağlı olan sorgu penceresine dönün. Aşağıdaki deyim **login1** oturum açma bilgisini parolayı değiştirmek için düzenler. **newPassword** yerine istediğiniz parolayı, **oldPassword** yerine de oturum açma bilgisinin geçerli parolasını yazın.
  
      ALTER LOGIN login1
      WITH PASSWORD = 'newPassword'
      OLD_PASSWORD = 'oldPassword';
* Var olan oturum açma bilgisini silmek için **DROP LOGIN** deyimini kullanın. Sunucu düzeyindeki bir oturum açma bilgisini sildiğinizde ilgili veritabanı kullanıcı hesapları da silinir. Daha fazla bilgi için bkz. [DROP DATABASE (SQL Veritabanı)](https://msdn.microsoft.com/library/ms178613.aspx). **DROP LOGIN** deyiminin **ana** veritabanında çalıştırılması gerekir. Bu deyim **login1** oturum açma bilgisini siler.
  
      DROP LOGIN login1;
* Ana veritabanında oturum açma bilgilerini görüntülemek için kullanabileceğiniz **sys.sql\_logins** görünümü vardır. Var olan tüm oturum açma bilgilerini görüntülemek için aşağıdaki deyimi çalıştırın:
  
      SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-views"></a>Dinamik Yönetim Görünümlerini kullanarak SQL Veritabanını izleme
SQL Veritabanı, tek bir veritabanını izlemek için kullanabileceğiniz birden fazla dinamik yönetim görevini destekler. Bu görünümlerle alabileceğiniz izleme verisi türüne birkaç örnek aşağıda verilmiştir. Ayrıntılı bilgiler ve daha fazla kullanım örneği için bkz. [Dinamik Yönetim Görünümlerini Kullanarak SQL Veritabanı İzleme](https://msdn.microsoft.com/library/azure/ff394114.aspx).

* Dinamik yönetim görünümünde sorgu çalıştırmak için **VIEW DATABASE STATE** izinleri gereklidir. Belirli bir veritabanı kullanıcısına **VIEW DATABASE STATE** iznini vermek için veritabanına bağlanın ve aşağıdaki deyimi veritabanında çalıştırın:
  
      GRANT VIEW DATABASE STATE TO login1User;
* Veritabanı boyutunu **sys.dm\_db\_partition\_stats** görünümünü kullanarak hesaplayabilirsiniz. **sys.dm\_db\_partition\_stats** görünümü, veritabanındaki tüm bölümler için sayfa ve satır sayısı bilgilerini döndürür. Bu bilgileri kullanarak veritabanı boyutunu hesaplayabilirsiniz. Aşağıdaki sorgu veritabanınızın boyutunu megabayt cinsinden döndürür:
  
      SELECT SUM(reserved_page_count)*8.0/1024
      FROM sys.dm_db_partition_stats;   
* Geçerli kullanıcı bağlantıları ve veritabanıyla ilişkilendirilmiş dahili görevlerle ilgili bilgi almak için **sys.dm\_exec\_connections** ve **sys.dm\_exec\_sessions** görünümlerini kullanın. Aşağıdaki sorgu geçerli bağlantıyla ilgili bilgileri döndürür:
  
      SELECT
          e.connection_id,
          s.session_id,
          s.login_name,
          s.last_request_end_time,
          s.cpu_time
      FROM
          sys.dm_exec_sessions s
          INNER JOIN sys.dm_exec_connections e
            ON s.session_id = e.session_id;
* Önbelleğe alınmış sorgu planlarıyla ilgili toplu performans istatistiklerini almak için **sys.dm\_exec\_query\_stats** görünümünü kullanın. Aşağıdaki sorgu ortalama CPU süresine göre sıralanmış ilk beş sorgu hakkındaki bilgileri döndürür.
  
      SELECT TOP 5 query_stats.query_hash AS "Query Hash",
          SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
          MIN(query_stats.statement_text) AS "Statement Text"
      FROM
          (SELECT QS.*,
          SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
          ((CASE statement_end_offset
              WHEN -1 THEN DATALENGTH(ST.text)
              ELSE QS.statement_end_offset END
                  - QS.statement_start_offset)/2) + 1) AS statement_text
           FROM sys.dm_exec_query_stats AS QS
           CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
      GROUP BY query_stats.query_hash
      ORDER BY 2 DESC;




<!--HONumber=Jan17_HO1-->


