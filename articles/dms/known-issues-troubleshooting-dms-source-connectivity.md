---
title: Bilinen sorunları/Azure veritabanı geçiş Hizmeti'nin kaynak veritabanlarına bağlanma ile ilgili hataları giderme hakkında daha fazla makale | Microsoft Docs
description: Azure veritabanı geçiş Hizmeti'nin kaynak veritabanlarına bağlanma ile ilgili bilinen sorunları/hatalarını giderme hakkında bilgi edinin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/28/2019
ms.openlocfilehash: a4ebd1d4c85631cc6ebc1f5787e7545b78d62b5e
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462856"
---
# <a name="troubleshoot-dms-errors-when-connecting-to-source-databases"></a>Kaynak veritabanlarına bağlanırken DMS hatalarını giderme

Aşağıdaki makalede, Azure veritabanı geçiş hizmeti (DMS), kaynak veritabanına bağlanırken karşılaşabileceğiniz olası sorunları nasıl çözeceğinize hakkında ayrıntılı bilgi sağlanır. Kaynak veritabanı, belirli bir türü her bölümüne ilişkili hata listesi, ayrıntı ve nasıl bağlantı sorunlarını giderme hakkındaki bilgiler için bağlantılarla birlikte karşılaşabilirsiniz.

## <a name="sql-server"></a>SQL Server

Kaynak SQL Server veritabanına bağlanma ile ilgili olası sorunları ilişkili ve bunların nasıl, aşağıdaki tabloda verilmiştir.

| Hata         | Neden ve ayrıntılı sorun giderme |
| ------------- | ------------- |
| SQL bağlantısı başarısız oldu. SQL Server ile bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi. Örnek adının doğru olduğundan ve bu SQL Server Uzak bağlantılara izin verecek şekilde yapılandırıldığından emin olun.<br> | Hizmet kaynak sunucu bulamadığında bu hata oluşur. Sorunu gidermek için bu makaleye bakın [kaynak SQL Server dinamik bağlantı noktası kullanırken bağlanma veya adlandırılmış örneğine hata](https://docs.microsoft.com/azure/dms/known-issues-troubleshooting-dms#error-connecting-to-source-sql-server-when-using-dynamic-port-or-named-instance). |
| **Hata 53** -SQL bağlantısı başarısız oldu. (Ayrıca, hata için 1, 2, 5, 53, 233, 258, 1225, kodları 11001)<br><br> | Hizmet kaynak sunucuya bağlanıyorsanız bu hata oluşur. Sorunu gidermek için aşağıdaki kaynaklara bakın ve işlemi yeniden deneyin. <br><br>  [Bağlantı sorununu gidermek için Etkileşimli Kullanıcı Kılavuzu](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server)<br><br> [Azure SQL veritabanına geçirme SQL Server için Önkoşullar](https://docs.microsoft.com/azure/dms/pre-reqs#prerequisites-for-migrating-sql-server-to-azure-sql-database) <br><br> [Azure SQL veritabanı yönetilen örneğine geçirme SQL Server için Önkoşullar](https://docs.microsoft.com/azure/dms/pre-reqs#prerequisites-for-migrating-sql-server-to-an-azure-sql-database-managed-instance) |
| **Hata 18456** -oturum açma başarısız oldu.<br> | Hizmet verilen T-SQL kimlik bilgilerini kullanarak kaynak veritabanına bağlanamıyorsa bu hata oluşur. Sorunu gidermek için girilen kimlik bilgilerini doğrulayın. Ayrıca başvurabilirsiniz [MSSQLSERVER_18456](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error?view=sql-server-2017) veya bu altındaki notta bulunan listelenen sorun giderme belgeleri için tablo ve yeniden deneyin. |
| Hatalı biçimlendirilmiş AccountName değeri '{0}' sağlanan. Etkialanıadı\kullanıcıadı AccountName için beklenen biçim:<br> | Kullanıcının Windows kimlik doğrulaması seçer, ancak kullanıcı adı geçersiz bir biçimde sağlar, bu hata oluşur. Sorunu gidermek için ya da kullanıcı adı doğru biçimde Windows kimlik doğrulama ve seçin için sağlamak **SQL kimlik doğrulaması**. |

## <a name="aws-rds-mysql"></a>AWS RDS MySQL

Bir kaynak AWS RDS Mysql'i veritabanı'na bağlanma ile ilgili olası sorunları ilişkili ve bunların nasıl, aşağıdaki tabloda verilmiştir.

| Hata         | Neden ve ayrıntılı sorun giderme |
| ------------- | ------------- |
| **Hata [2003]** [HY000] - bağlantı başarısız oldu. Hata [HY000] [MySQL] [ODBC x.x(w) sürücüsü] '{server}' (10060) üzerinde MySQL sunucusuna bağlanamıyor | MySQL ODBC sürücüsü kaynak sunucuya bağlanıyorsanız bu hata oluşur. Sorunu gidermek için bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurun ve işlemi yeniden deneyin.<br> |
| **Hata [2005]** [HY000] - bağlantı başarısız oldu. Hata [HY000] [MySQL] [ODBC x.x(w) sürücüsü] bilinmeyen MySQL server konağı '{server}' | Hizmet kaynak ana bilgisayar üzerinde RDS'yi bulamazsa, bu hata oluşur. Sorun, listelenen kaynak mevcut değil veya RDS altyapı ile ilgili bir sorun olduğundan ya da olabilir. Sorunu gidermek için bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurun ve işlemi yeniden deneyin.<br> |
| **Hata [1045]** [HY000] - bağlantı başarısız oldu. Hata [HY000] [MySQL] [ODBC x.x(w) sürücüsü] erişim kullanıcı '{user}'@'{server}' için reddedildi (parola kullanarak: EVET) | MySQL ODBC sürücüsü geçersiz kimlik bilgileri nedeniyle kaynak sunucuya bağlanıyorsanız bu hata oluşur. Girdiğiniz kimlik bilgilerini doğrulayın. Sorun devam ederse, bu kaynak bilgisayara doğru kimlik bilgilerine sahip olun. Konsolunda parolayı sıfırlamanız gerekebilir. Hala sorunla karşılaşırsanız, bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurun ve işlemi yeniden deneyin.<br> |
| **Hata [9002]** [HY000] - bağlantı başarısız oldu. Hata [HY000] [MySQL] [ODBC x.x(w) sürücüsü] bağlantı dizesini olabilir doğru. Başvurular için portalı ziyaret edin.| Bağlantı dizesi ile ilgili bir sorun nedeniyle bağlantı başarısız olursa bu hata oluşur. Sağlanan bağlantı dizesi geçerli olduğunu doğrulayın. Sorunu gidermek için bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurun ve işlemi yeniden deneyin.<br> |
| **İkili günlük hatası. Değişken binlog_format değerine sahip '{value}'. Lütfen 'satır' olarak değiştirin.** | İkili günlüğüne bir hata varsa, bu hata oluşur; değişken binlog_format yanlış değerine sahiptir. Sorunu gidermek için 'Satır' binlog_format parametre grubu içindeki değiştirin ve ardından örneğini yeniden başlatın. Daha fazla bilgi için bkz: [ikili günlük seçenekleri ve değişkenleri](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html) veya [AWS RDS MySQL veritabanı günlük dosyalarını belgeleri](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MySQL.html).<br> |

> [!NOTE]
> Bir kaynak AWS RDS Mysql'i veritabanına bağlanmak için ilgili sorunları giderme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
> * [Amazon RDS bağlantı sorunlarını giderme](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.Connecting)
> * [Amazon RDS veritabanı Örneğim bağlanma sorunlarını nasıl giderebilirim?](https://aws.amazon.com/premiumsupport/knowledge-center/rds-cannot-connect)

## <a name="aws-rds-postgresql"></a>AWS RDS PostgreSQL

Kaynak AWS RDS PostgreSQL veritabanına bağlanma ile ilgili olası sorunları ilişkili ve bunların nasıl, aşağıdaki tabloda verilmiştir.

| Hata         | Neden ve ayrıntılı sorun giderme |
| ------------- | ------------- |
| **Hata [101]** [08001] - bağlantı başarısız oldu. Hata [08001] zaman aşımı süresi doldu. | Postgres sürücüsünü kaynak sunucuya bağlanıyorsanız bu hata oluşur. Sorunu gidermek için bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurun ve işlemi yeniden deneyin. |
| **Hata: Parametre wal_level değerine sahip '{value}'. Lütfen 'mantıksal' çoğaltma izin verecek şekilde değiştirin.** | Parametre wal_level yanlış bir değer varsa, bu hata oluşur. Sorunu gidermek için parametre grubu içindeki rds.logical_replication 1 olarak değiştirin ve örnek yeniden. Daha fazla bilgi için bkz: [Azure DMS kullanarak PostgreSQL için geçiş için ön koşullar](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#prerequisites) veya [Amazon RDS üzerinde PostgreSQL](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html). |

> [!NOTE]
> Bir kaynak AWS RDS PostgreSQL veritabanına bağlanmak için ilgili sorunları giderme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
> * [Amazon RDS bağlantı sorunlarını giderme](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.Connecting)
> * [Amazon RDS veritabanı Örneğim bağlanma sorunlarını nasıl giderebilirim?](https://aws.amazon.com/premiumsupport/knowledge-center/rds-cannot-connect)

## <a name="aws-rds-sql-server"></a>AWS RDS SQL Server

Kaynak AWS RDS SQL Server veritabanına bağlanma ile ilgili olası sorunları ilişkili ve bunların nasıl, aşağıdaki tabloda verilmiştir.

| Hata         | Neden ve ayrıntılı sorun giderme |
| ------------- | ------------- |
| **Hata 53** -SQL bağlantısı başarısız oldu. SQL Server ile bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi. Örnek adının doğru olduğundan ve bu SQL Server Uzak bağlantılara izin verecek şekilde yapılandırıldığından emin olun. (sağlayıcısı: Adlandırılmış kanal sağlayıcısı, hata: 40 - bir SQL Server bağlantısı açılamadı | Hizmet kaynak sunucuya bağlanıyorsanız bu hata oluşur. Sorunu gidermek için bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurun ve işlemi yeniden deneyin. |
| **Hata 18456** -oturum açma başarısız oldu. '{User}' kullanıcısı için oturum açma başarısız oldu | Kaynak veritabanı için T-SQL kimlik bilgileri sağlanan hizmet bağlanamıyorsa bu hata oluşur. Sorunu gidermek için girilen kimlik bilgilerini doğrulayın. Ayrıca başvurabilirsiniz [MSSQLSERVER_18456](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error?view=sql-server-2017) veya bu altındaki notta bulunan listelenen sorun giderme belgeleri için tablo ve yeniden deneyin. |
| **Hata 87** -bağlantı dizesi geçerli değil. SQL Server ile bağlantı kurulmaya çalışılırken ağ ile ilişkili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi. Örnek adının doğru olduğundan ve bu SQL Server Uzak bağlantılara izin verecek şekilde yapılandırıldığından emin olun. (sağlayıcısı: SQL ağ arabirimleri, hata: 25 - bağlantı dizesi geçerli değil) | Hizmet, geçersiz bir bağlantı dizesi nedeniyle kaynak sunucuya bağlanamıyorsanız, bu hata oluşur. Sorunu gidermek için sağlanan bağlantı dizesini doğrulayın. Sorun devam ederse, bu tablonun altındaki notta listelenen sorun giderme belgelerine başvurun ve işlemi yeniden deneyin. |
| **Hata - sunucu sertifikası güvenilir değil.** Sunucu ile başarılı bir bağlantı kuruldu, ancak daha sonra oturum açma işlemi sırasında bir hata oluştu. (sağlayıcısı: SSL Sağlayıcısı, hata: 0 - sertifika zinciri güvenilmeyen bir yetkili tarafından verilmiş.) | Kullanılan sertifika güvenilir değilse bu hata oluşur. Sorunu gidermek için güvenilir ve sunucuda etkinleştirmeniz bir sertifika bulmanız gerekir. Alternatif olarak, bağlama sırasında güven sertifikası seçeneğini seçebilirsiniz. Bu eylem, yalnızca kullanılan sertifika ile ilgili bilgi sahibi olduğunuz ve buna güvenmesi yapın. <br> Kendinden imzalı bir sertifika kullanılarak şifrelenir ve SSL bağlantıları güçlü güvenlik sağlamayan--adam-de-adam saldırılarına oldukları. Bir üretim ortamında otomatik olarak imzalanan sertifikaları kullanarak SSL veya internet'e bağlı sunucuları kullanmayın. <br> Daha fazla bilgi için bkz: [SSL kullanarak bir Microsoft SQL Server veritabanı örneğiniz ile](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/SQLServer.Concepts.General.SSL.Using.html) veya [Öğreticisi: RDS SQL Server'ı DMS kullanarak Azure'a geçirme](https://docs.microsoft.com/azure/dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online#prerequisites). |
| **Hata 300** -kullanıcı gerekli izinlere sahip değil. Nesne '{server}', '{veritabanı}' veritabanı VIEW SERVER STATE izni reddedildi | Kullanıcı geçişi gerçekleştirmek için izne sahip değilse bu hata oluşur. Sorunu gidermek için başvurmak [Server izin ver - Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql?view=sql-server-2017) veya [Öğreticisi: RDS SQL Server'ı DMS kullanarak Azure'a geçirme](https://docs.microsoft.com/azure/dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online#prerequisites) daha fazla ayrıntı için. |

> [!NOTE]
> Kaynak için AWS RDS SQL Server bağlanmayla ilgili sorunları giderme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
>
> * [SQL Server’a bağlantı hatalarını çözme](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server)
> * [Amazon RDS veritabanı Örneğim bağlanma sorunlarını nasıl giderebilirim?](https://aws.amazon.com/premiumsupport/knowledge-center/rds-cannot-connect)

## <a name="known-issues"></a>Bilinen sorunlar

* [Bilinen sorunları/geçiş sınırlamalarıyla birlikte Azure SQL veritabanı çevrimiçi geçişleri](https://docs.microsoft.com/azure/dms/known-issues-azure-sql-online)
* [MySQL için Azure veritabanı çevrimiçi geçişleri ile bilinen sorunları/geçiş sınırlamaları](https://docs.microsoft.com/azure/dms/known-issues-azure-mysql-online)
* [PostgreSQL için Azure veritabanı çevrimiçi geçişleri ile bilinen sorunları/geçiş sınırlamaları](https://docs.microsoft.com/azure/dms/known-issues-azure-postgresql-online)

## <a name="next-steps"></a>Sonraki adımlar

* Makaleyi görüntüleyin [Azure veritabanı geçiş hizmeti PowerShell](https://docs.microsoft.com/powershell/module/azurerm.datamigration/?view=azurermps-6.13.0#data_migration).
* Makaleyi görüntüleyin [nasıl MySQL için Azure veritabanı'nda Azure portalını kullanarak sunucu parametrelerini yapılandırma](https://docs.microsoft.com/azure/mysql/howto-server-parameters).
* Makaleyi görüntüleyin [için Azure veritabanı geçiş hizmetini kullanarak önkoşullara genel bakış](https://docs.microsoft.com/azure/dms/pre-reqs).
* Bkz: [kullanarak Azure veritabanı geçiş hizmeti hakkında SSS](https://docs.microsoft.com/azure/dms/faq).
