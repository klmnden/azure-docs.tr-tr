---
title: "SQL veri ambarı veritabanında güvenli | Microsoft Docs"
description: "Çözümleri geliştirme için Azure SQL veri ambarı veritabanında güvenliğini sağlamaya yönelik ipuçları."
services: sql-data-warehouse
documentationcenter: NA
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: 8fa2f5ca-4cf5-4418-99a2-4dc745799850
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 6ea45c40bc428282faf24b4a08f8b0d345adb3fd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="secure-a-database-in-sql-data-warehouse"></a>SQL veri ambarı veritabanında güvenli
> [!div class="op_single_selector"]
> * [Güvenlik genel bakış](sql-data-warehouse-overview-manage-security.md)
> * [Kimlik doğrulaması](sql-data-warehouse-authentication.md)
> * [Şifreleme (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Şifreleme (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

Bu makalede, Azure SQL Data Warehouse veritabanınızın güvenliğini sağlama temellerini anlatılmaktadır. Özellikle, bu makalede, erişimi sınırlamak için kaynaklar başlama verileri koruma ve bir veritabanı etkinliklerini izleme.

## <a name="connection-security"></a>Bağlantı güvenliği
Bağlantı Güvenliği, veritabanı bağlantılarını güvenlik duvarı kuralları ve bağlantı şifrelemesi kullanarak kısıtlamayı ve bu bağlantıların güvenliğini sağlamayı kapsar.

Güvenlik duvarı kuralları hem sunucu hem de veritabanı tarafından açık olarak beyaz listeye eklenmeyen IP adreslerinden gelen bağlantıları reddetmek için kullanılır. Uygulamanızı veya istemci makinenin ortak IP adresi bağlantılara izin vermek için ilk Azure portalı, REST API veya PowerShell kullanarak bir sunucu düzeyinde güvenlik duvarı kuralı oluşturmanız gerekir. En iyi uygulama olarak sunucu güvenlik duvarınızdan geçmesine izin verilen IP adresi aralıklarını mümkün olduğunca sınırlı tutmanız gerekir.  Azure SQL Data Warehouse, yerel bilgisayarınızdan erişmek için ağ ve yerel bilgisayar güvenlik duvarının TCP bağlantı noktası 1433 giden iletişim kurmasına olanak tanıyan emin olun.  Daha fazla bilgi için bkz: [Azure SQL veritabanı Güvenlik Duvarı][Azure SQL Database firewall], [sp_set_firewall_rule][sp_set_firewall_rule], ve [sp_set_database_ firewall_rule][sp_set_database_firewall_rule].

SQL veri ambarı için bağlantıları varsayılan olarak şifrelenir.  Şifreleme devre dışı bırakmak için bağlantı ayarlarını değiştirme göz ardı edilir.

## <a name="authentication"></a>Kimlik Doğrulaması
Kimlik doğrulaması, veritabanına bağlanırken kimliğinizi nasıl kanıtlayacağınızı belirtir. SQL veri ambarı SQL Server kimlik doğrulamasını bir kullanıcı adı ve parola yanı sıra Azure Active Directory şu anda destekler. 

Veritabanınıza ait mantıksal sunucuyu oluşturduktan sonra kullanıcı adı ve parola belirleyerek "sunucu yöneticisi" oturum açma bilgisi oluşturdunuz. Bu kimlik bilgilerini kullanarak, veritabanı sahibi ya da SQL Server kimlik doğrulaması yoluyla "dbo" olarak sunucu üzerindeki herhangi bir veritabanı doğrulayabilir.

Ancak, en iyi uygulama, kuruluşunuzdaki kullanıcılar farklı bir hesap kimlik doğrulaması için kullanmanız gerekir. Bu şekilde uygulamaya verilen izinler sınırlayabilir ve durumda uygulama kodunuz SQL ekleme saldırısına karşı savunmasız olan kötü amaçlı etkinliğin riskleri azaltın. 

SQL Server kimliği doğrulanmış bir kullanıcı oluşturmak için bağlanmak **ana** veritabanı, Sunucu Yöneticisi oturum açma ile sunucunuzdaki ve yeni bir sunucu oturum açma oluşturun.  Ayrıca, Azure SQL Data Warehouse kullanıcılar için ana veritabanındaki bir kullanıcı oluşturmak için bir fikirdir. Master kullanıcı oluşturma, kullanıcının bir veritabanı adı belirtmeden SSMS gibi araçları kullanarak oturum açma izin verir.  Ayrıca SQL server üzerinde tüm veritabanlarını görüntülemek için Nesne Gezgini kullanmalarını sağlar.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Ardından, bağlanmak, **SQL Data Warehouse veritabanı** , Sunucu Yöneticisi oturum açma ile oluşturduğunuz sunucu oturum açma tabanlı bir veritabanı kullanıcısı oluşturmalıdır.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Bir kullanıcı oturumu açma oluşturma veya yeni veritabanları oluşturma gibi ek işlemleri yapacaksanız, bunlar ayrıca atanması gerekir `Loginmanager` ve `dbmanager` ana veritabanında roller. Bu ilave roller ve bir SQL veritabanı kimlik doğrulaması hakkında daha fazla bilgi için bkz: [veritabanları ve Azure SQL veritabanında oturumları yönetme][Managing databases and logins in Azure SQL Database].  SQL veri ambarı için Azure AD hakkında daha fazla bilgi için bkz: [SQL veri ambarı tarafından kullanarak Azure Active Directory kimlik doğrulaması için bağlanma][Connecting to SQL Data Warehouse By Using Azure Active Directory Authentication].

## <a name="authorization"></a>Yetkilendirme
Bir Azure SQL Data Warehouse veritabanı içinde neler yapabileceğinizi için yetkilendirme başvuruyor ve bu kullanıcı hesabınızın rol üyeliklerini ve izinleri tarafından denetlenir. En iyi uygulama olarak, kullanıcılarınıza gerekli olan en düşük ayrıcalıkları tanımanız gerekir. Azure SQL veri ambarı bu T-SQL rolleriyle yönetmenizi kolaylaştırır:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

Bağlantı kurmak için kullandığınız sunucu yöneticisi hesabı, veritabanında tüm işlemleri gerçekleştirme yetkisi olan db_owner rolünün üyesidir. Bu hesabı şema yükseltmeleri ve diğer yönetimsel işlemlerde kullanmak üzere saklayın. Uygulamanızdan veritabanına, uygulamanızın ihtiyaç duyduğu en düşük ayrıcalıklarla bağlanmak için daha sınırlı izinlere sahip olan "ApplicationUser" hesabını kullanın.

Bir kullanıcının Azure SQL Veritabanında yapabileceklerini sınırlamak için kullanabileceğiniz başka yöntemler de mevcuttur:

* Ayrıntılı [izinleri] [ Permissions] denetim yapabileceğiniz işlemleri ayrı ayrı sütunlarda tablolar, görünümler, yordamlar ve veritabanındaki diğer nesneleri sağlar. Çoğu denetimi yoktur ve gerekli minimum izinleri vermek için ayrıntılı izinleri kullanın. Ayrıntılı izin sistemi biraz karmaşıktır ve etkili bir şekilde kullanmak için bazı incelemesi gerektirir.
* [Veritabanı rolleri] [ Database roles] dışında db_datareader ve db_datawriter daha güçlü uygulama kullanıcı hesaplarını veya daha az güçlü yönetim hesaplarını oluşturmak için kullanılabilir. Yerleşik sabit veritabanı rollerinin izinlerini vermek için kolay bir yol sağlar, ancak gerekenden daha fazla izin verme neden olabilir.
* [Saklı yordamlar] [ Stored procedures] veritabanı üzerinde gerçekleştirilecek eylemler sınırlamak için kullanılabilir.

Veritabanlarını ve mantıksal sunucuları Klasik Azure Portalından veya Azure Resource Manager API kullanarak yönetmek, portalınızın kullanıcı hesabına atanan roller tarafından denetlenir. Bu konu hakkında daha fazla bilgi için bkz: [Azure portalında rol tabanlı erişim denetimi][Role-based access control in Azure Portal].

## <a name="encryption"></a>Şifreleme
Azure SQL veri ambarı saydam veri şifreleme (TDE'nin) gerçek zamanlı şifreleme ve şifre çözme REST verilerinizin gerçekleştirerek kötü amaçlı etkinliği tehdide karşı korunmasına yardımcı olur.  Veritabanınızı şifrelerken, ilişkili yedeklemeler ve işlem günlüğü dosyalarını uygulamalarınızın herhangi bir değişiklik gerektirmeden şifrelenir. TDE, veritabanının tamamını Depolama veritabanı şifreleme anahtarını adlı bir simetrik anahtar kullanarak şifreler. SQL veritabanında veritabanı şifreleme anahtarını bir yerleşik bir sunucu sertifikası tarafından korunur. Yerleşik bir sunucu sertifikası her SQL veritabanı sunucusu için benzersizdir. Microsoft, bu sertifikalar otomatik olarak en az 90 günde döndürür. SQL veri ambarı tarafından kullanılan şifreleme algoritması AES-256'dır. TDE genel bir açıklaması için bkz [saydam veri şifreleme][Transparent Data Encryption].

Veritabanını kullanarak şifreleyebilirsiniz [Azure Portal] [ Encryption with Portal] veya [T-SQL][Encryption with TSQL].

## <a name="next-steps"></a>Sonraki adımlar
Ayrıntılar ve farklı protokoller ile SQL veri ambarı bağlanma ilişkin örnekler için bkz: [SQL Data Warehouse Bağlan][Connect to SQL Data Warehouse].

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
[Role-based access control in Azure Portal]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure
