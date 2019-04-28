---
title: SQL veri ambarı veritabanında güvenli | Microsoft Docs
description: Bir veritabanını Azure SQL veri ambarı çözümleri geliştirmek için güvenli hale getirmek için ipuçları.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 179925fc7411a1ccf3de02d7b6298cc66f93bc66
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61126949"
---
# <a name="secure-a-database-in-sql-data-warehouse"></a>SQL veri ambarı veritabanını koruma
> [!div class="op_single_selector"]
> * [Güvenliğe genel bakış](sql-data-warehouse-overview-manage-security.md)
> * [Kimlik doğrulaması](sql-data-warehouse-authentication.md)
> * [Şifreleme (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Şifreleme (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

Bu makalede, Azure SQL Data Warehouse veritabanınızı güvenli hale getirme temellerini gösterilmektedir. Başlatılan erişim sınırlama için kaynaklara sahip bu makalede alır, özellikle veri koruma ve izleme etkinlikleri bir veritabanı.

## <a name="connection-security"></a>Bağlantı güvenliği
Bağlantı Güvenliği, veritabanı bağlantılarını güvenlik duvarı kuralları ve bağlantı şifrelemesi kullanarak kısıtlamayı ve bu bağlantıların güvenliğini sağlamayı kapsar.

Güvenlik duvarı kuralları hem sunucu hem de veritabanı tarafından açık olarak beyaz listeye eklenmeyen IP adreslerinden gelen bağlantıları reddetmek için kullanılır. Uygulamanızın veya istemci bilgisayarınızın genel IP adresi bağlantılara izin vermek için Azure portalı, REST API veya PowerShell kullanarak sunucu düzeyinde güvenlik duvarı kuralı oluşturmanız gerekir. En iyi uygulama olarak sunucu güvenlik duvarınızdan geçmesine izin verilen IP adresi aralıklarını mümkün olduğunca sınırlı tutmanız gerekir.  Azure SQL veri ambarı kullanarak yerel bilgisayarınızdan erişmek için ağ ve yerel bilgisayar güvenlik duvarının 1433 numaralı TCP bağlantı noktasını giden iletişimine izin verdiğinden emin olun.  

SQL veri ambarı sunucu düzeyinde güvenlik duvarı kurallarını kullanır. Veritabanı düzeyinde güvenlik duvarı kurallarını desteklemez. Daha fazla bilgi için [Azure SQL veritabanı güvenlik duvarını][Azure SQL Database firewall], [sp_set_firewall_rule][sp_set_firewall_rule].

SQL veri ambarınıza bağlantıları varsayılan olarak şifrelenir.  Şifreleme devre dışı bırakmak için bağlantı ayarlarını değiştirme göz ardı edilir.

## <a name="authentication"></a>Kimlik Doğrulaması
Kimlik doğrulaması, veritabanına bağlanırken kimliğinizi nasıl kanıtlayacağınızı belirtir. SQL veri ambarı, SQL Server kimlik doğrulamasını bir kullanıcı adı ve parola ile ve Azure Active Directory ile şu anda destekler. 

Veritabanınıza ait mantıksal sunucuyu oluşturduktan sonra kullanıcı adı ve parola belirleyerek "sunucu yöneticisi" oturum açma bilgisi oluşturdunuz. Bu kimlik bilgilerini kullanarak veritabanı sahibi veya SQL Server kimlik doğrulaması aracılığıyla "dbo" olarak sunucudaki tüm veritabanları için doğrulayabilir.

Ancak, kimlik doğrulaması için en iyi uygulama, kuruluşunuzdaki kullanıcılar farklı bir hesap kullanmalıdır. Bu şekilde uygulamaya verilen izinleri sınırlayabilir ve uygulama kodunuzun SQL ekleme saldırısına açık olması durumunda kötü niyetli etkinlik risklerini azaltabilirsiniz. 

SQL Server kimliği doğrulanmış bir kullanıcı oluşturmak için Bağlan **ana** veritabanı sunucunuzda, Sunucu Yöneticisi oturum açma ve yeni bir sunucu oturum açma oluşturun.  Ayrıca, Azure SQL veri ambarı kullanıcılar için asıl veritabanında bir kullanıcı oluşturmak için bir fikirdir. Ana veritabanında kullanıcı oluşturma, bir veritabanı adı belirtmeden SSMS gibi araçları kullanarak oturum açmasına olanak sağlar.  Ayrıca bunları bir SQL Server'da tüm veritabanlarını görüntülemek için nesne Gezgini'ni kullanma sağlar.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Ardından, bağlanın, **SQL veri ambarı veritabanı** , Sunucu Yöneticisi oturum açma ile oluşturduğunuz sunucu oturumuna dayanarak bir veritabanı kullanıcısı oluşturmalıdır.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Bir kullanıcı oturum açma bilgileri oluşturmak veya yeni veritabanları oluşturmak gibi ek işlemleri gerçekleştirme izni vermek için kullanıcıya atamak `Loginmanager` ve `dbmanager` ana veritabanında rolü. Bu ek roller ve SQL veritabanı için kimlik doğrulaması hakkında daha fazla bilgi için bkz. [veritabanlarını ve oturum açma bilgileri Azure SQL veritabanı'nda yönetme][Managing databases and logins in Azure SQL Database].  Daha fazla bilgi için [SQL veri ambarı tarafından Azure Active Directory kimlik doğrulaması kullanarak bağlanma][Connecting to SQL Data Warehouse By Using Azure Active Directory Authentication].

## <a name="authorization"></a>Yetkilendirme
Yetkilendirme, bir Azure SQL veri ambarı veritabanında yapabilecekleriniz için ifade eder. Yetkilendirme ayrıcalıkları rol üyelikleri ve izinleri ile belirlenir. En iyi uygulama olarak, kullanıcılarınıza gerekli olan en düşük ayrıcalıkları tanımanız gerekir. Rolleri yönetmek için aşağıdaki saklı yordamları kullanabilirsiniz:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

Bağlantı kurmak için kullandığınız sunucu yöneticisi hesabı, veritabanında tüm işlemleri gerçekleştirme yetkisi olan db_owner rolünün üyesidir. Bu hesabı şema yükseltmeleri ve diğer yönetimsel işlemlerde kullanmak üzere saklayın. Uygulamanızdan veritabanına, uygulamanızın ihtiyaç duyduğu en düşük ayrıcalıklarla bağlanmak için daha sınırlı izinlere sahip olan "ApplicationUser" hesabını kullanın.

Başka bir kullanıcı Azure SQL veri ambarı ile neler yapabileceğinizi sınırlamak için yol vardır:

* Ayrıntılı [izinleri] [ Permissions] denetimi işlemleri tek sütun, tablolar, görünümler, şemalar, yordamlar ve diğer veritabanı nesneleri sağlar. Ayrıntılı izinler, en çok denetimi olan ve gerekli minimum izinleri vermek için kullanın. 
* [Veritabanı rolleri] [ Database roles] dışındaki db_datareader ve db_datawriter daha güçlü uygulama kullanıcı hesapları veya daha az yönetim hesapları oluşturmak için kullanılabilir. Yerleşik sabit veritabanı rollerine izinleri vermek için kolay bir yol sağlar, ancak gerekenden daha fazla izinleri verme de neden olabilir.
* [Saklı yordamlar] [ Stored procedures] veritabanında gerçekleştirilebilecek eylemler sınırlandırılabilir için kullanılabilir.

Aşağıdaki örnek, kullanıcı tanımlı bir şeması okuma erişimi verir.
```sql
--CREATE SCHEMA Test
GRANT SELECT ON SCHEMA::Test to ApplicationUser
```

Azure portalından veritabanlarını ve mantıksal sunucuları yönetme veya Azure Resource Manager API'sini kullanarak, portal kullanıcı hesabınızın rol atamaları tarafından denetlenir. Daha fazla bilgi için [Azure portalında rol tabanlı erişim denetimi][Role-based access control in Azure portal].

## <a name="encryption"></a>Şifreleme
Azure SQL veri ambarı saydam veri şifrelemesi (TDE), kötü amaçlı etkinlik tehditlerine karşı şifreleme ve şifre çözme bekleyen verilerinizi koruma yardımcı olur.  Veritabanınızı şifrelemek, ilişkili yedeklemeler ve işlem günlük dosyaları, uygulamalarınızı herhangi bir değişiklik gerektirmeden şifrelenir. TDE, tüm veritabanı depolama veritabanı şifreleme anahtarı olarak adlandırılan bir simetrik anahtarı'nı kullanarak şifreler. 

SQL veritabanı'nda veritabanı şifreleme anahtarını bir yerleşik bir sunucu sertifikası tarafından korunur. Yerleşik bir sunucu sertifikası, her SQL veritabanı sunucusu için benzersizdir. Microsoft, bu sertifikalar otomatik olarak en az 90 günde döndürür. SQL veri ambarı tarafından kullanılan şifreleme algoritması AES-256'dır. TDE genel bir açıklaması için bkz. [saydam veri şifrelemesi][Transparent Data Encryption].

Veritabanını kullanarak şifreleyebilirsiniz [Azure portalında] [ Encryption with Portal] veya [T-SQL][Encryption with TSQL].

## <a name="next-steps"></a>Sonraki adımlar
Ayrıntılar ve farklı protokoller, SQL veri ambarına bağlanma ile ilgili örnekler için bkz. [SQL Data Warehouse'a bağlanma][Connect to SQL Data Warehouse].

<!--Image references-->

<!--Article references-->
[Connect to SQL Data Warehouse]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Connecting to SQL Data Warehouse By Using Azure Active Directory Authentication]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Azure SQL Database firewall]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Database roles]: https://msdn.microsoft.com/library/ms189121.aspx
[Managing databases and logins in Azure SQL Database]: https://msdn.microsoft.com/library/ee336235.aspx
[Permissions]: https://msdn.microsoft.com/library/ms191291.aspx
[Stored procedures]: https://msdn.microsoft.com/library/ms190782.aspx
[Transparent Data Encryption]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Role-based access control in Azure portal]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure
