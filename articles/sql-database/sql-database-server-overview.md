---
title: "Azure SQL Veritabanı Mantıksal Sunucusuna Genel Bakış | Microsoft Belgeleri"
description: "Bu sayfada Azure SQL mantıksal sunucuları ile çalışmaya yönelik kurallar ve dikkat edilmesi gereken noktalar sunulmaktadır."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: servers
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/01/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: 17e2830dceeaa313dd0fd7d406bf68a75b6f900e


---
# <a name="azure-sql-database-logical-servers"></a>Azure SQL Veritabanı mantıksal sunucuları

Bu konu başlığında, Azure SQL mantıksal sunucuları ile çalışmaya yönelik kurallar ve dikkat edilmesi gereken noktalar sunulmaktadır. Azure SQL veritabanları hakkında bilgi için bkz. [SQL veritabanları](sql-database-overview.md).

## <a name="what-is-an-azure-sql-database-logical-server"></a>Azure SQL Veritabanı mantıksal sunucusu nedir?
Azure SQL Veritabanı mantıksal sunucusu birden fazla veritabanı için merkezi bir yönetim noktası olarak görev yapar. SQL Veritabanı'nda bir sunucu, şirket içi ortamda alışkın olabileceğiniz bir SQL Server örneğinden farklı olan mantıksal bir yapıdır. SQL Veritabanı hizmeti, veritabanlarının mantıksal sunuculara göre konumuyla ilgili özel bir garanti vermez ve örnek düzeyinde erişim ya da özellik sunmaz. Azure SQL mantıksal sunucuları hakkında daha fazla bilgi edinmek için bkz. [Mantıksal sunucular](sql-database-server-overview.md). 

Azure SQL Veritabanı mantıksal sunucusu:

- Bir Azure aboneliği içinde oluşturulur, ancak içerdiği kaynaklarla birlikte başka bir aboneliğe taşınabilir
- Veritabanları, elastik havuzlar ve veri ambarları için üst kaynaktır
- Veritabanları, elastik havuzlar ve veri ambarları için ad alanı sağlar
- Güçlü kullanım ömrü semantiğine sahip mantıksal bir kapsayıcıdır; bir sunucuyu sildiğinizde içindeki veritabanlarını, elastik havuzları ve veri ambarlarını siler
- Azure rol tabanlı erişim denetimine (RBAC) katılır; bir sunucudaki veritabanları ve elastik havuzlar, sunucudan erişim haklarını devralır
- Azure kaynak yönetiminde veritabanları ile elastik havuzların kimliğine ait yüksek düzeyli bir öğedir (veritabanları ve havuzlar için URL şemasına bakın)
- Bir bölgedeki kaynakları birlikte bulundurur
- Veritabanı erişimi için bağlantı uç noktası sağlar (<serverName>.database.windows.net)
- Bir ana veritabanına bağlanarak DMV’ler aracılığıyla içerdiği kaynaklarla ilgili meta verilere erişim sağlar 
- Veritabanları için geçerli olan yönetim ilkelerinin kapsamını sağlar: oturum açma, güvenlik duvarı, denetim, tehdit algılama, vb. 
- Üst abonelik içinde bir kota ile sınırlandırılır (abonelik başına altı sunucu - [bkz. Abonelik limitleri](../azure-subscription-service-limits.md))
- İçerdiği kaynaklara yönelik veritabanı kotası ile DTU kotasının kapsamını sağlar (V12’de 45000 DTU gibi)
- İçerdiği kaynaklarda etkinleştirilmiş özelliklerin sürüm kapsamıdır (en son sürüm V12)
- Sunucu düzeyinde asıl kullanıcı bilgileri bir sunucudaki tüm veritabanlarını yönetebilir
- Şirketinizde sunucu üzerindeki bir veya daha fazla veritabanına erişim verilmiş SQL Server örneklerinde bulunanlara benzer kullanıcı bilgileri içerebilir ve sınırlı yönetici hakları alabilir. Daha fazla bilgi için bkz. [Kullanıcı Bilgileri](sql-database-manage-logins.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-sql-database-logical-server"></a>Bir Azure SQL Veritabanı mantıksal sunucusuna nasıl bağlanıp kimliğimi doğrulayabilirim?

- **Kimlik doğrulama ve yetkilendirme**: Azure SQL Veritabanı SQL kimlik doğrulamasını ve Azure Active Directory Kimlik Doğrulaması’nı destekler (belirli kısıtlamalar vardır; kimlik doğrulama için bkz. [Azure Active Directory Kimlik Doğrulaması ile SQL Veritabanı’na bağlanma](sql-database-aad-authentication.md). Azure SQL veritabanlarına sunucunun asıl veritabanı üzerinden; bir kullanıcının veritabanına ise doğrudan bağlanarak kimliğinizi doğrulayabilirsiniz. Daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanında Veritabanlarını ve Oturum Açma İşlemlerini Yönetme](sql-database-manage-logins.md). Windows Kimlik Doğrulaması desteklenmez. 
- **TDS**: Microsoft Azure SQL Veritabanı, tablosal veri akışı (TDS) protokolü istemcisinin 7.3 veya sonraki sürümlerini destekler.
- **TCP/IP**: Yalnızca TCP/IP bağlantılarına izin verilir.
- **SQL Veritabanı güvenlik duvarı**: SQL Veritabanı güvenlik duvarı, verilerinizin korunmasına yardımcı olmak amacıyla siz hangi bilgisayarların izni olduğunu belirtene kadar veritabanını sunucunuza veya onun veritabanlarına yönelik tüm erişimi engeller. Bkz. [Güvenlik Duvarları](sql-database-firewall-configure.md)

## <a name="what-collations-are-supported"></a>Hangi harmanlamalar destekleniyor?

Microsoft Azure SQL Veritabanı (ana veritabanı dahil) tarafından kullanılan varsayılan veritabanı harmanlaması **SQL_LATIN1_GENERAL_CP1_CI_AS** şeklindedir; buradaki **LATIN1_GENERAL** İngilizce (Amerika Birleşik Devletleri), **CP1** kod sayfası 1252, **CI** büyük-küçük harfe duyarlı ve **AS** is aksan duyarlıdır. V12 veritabanı harmanlamalarının oluşturulduktan sonra değiştirilmesi önerilmez. Harmanlamalar hakkında daha fazla bilgi edinmek için bkz. [HARMANLAMA (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="what-are-the-naming-requirements-for-database-objects"></a>Veritabanı öğeleri için adlandırma gereksinimleri nelerdir?

Tüm yeni öğelerin adları tanımlayıcılara ilişkin SQL Server kurallarına uygun olmalıdır. Daha fazla bilgi edinmek için bkz. [Tanımlayıcılar](https://msdn.microsoft.com/library/ms175874.aspx).

## <a name="what-features-are-supported"></a>Hangi özellikler desteklenir?

Desteklenen özellikler hakkında bilgi edinmek için bkz. [Özellikler](sql-database-features.md). Belirli türlerdeki özelliklere yönelik destek olmayışı hakkında daha fazla arka plan bilgisi edinmek için ayrıca bkz. [Azure SQL Veritabanı Transact-SQL farkları](sql-database-transact-sql-information.md).

## <a name="how-do-i-manage-a-logical-server"></a>Bir mantıksal sunucuyu nasıl yönetebilirim?

Azure SQL Veritabanı mantıksal sunucularını birkaç yöntemle yönetebilirsiniz:
- [Azure Portal](sql-database-manage-portal.md)
- [PowerShell](sql-database-manage-powershell.md)
- [REST](/rest/api/sql/)

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL Veritabanı hizmeti hakkında bilgi edinmek için bkz. [SQL Veritabanı nedir?](sql-database-technical-overview.md)
- Desteklenen özellikler hakkında bilgi edinmek için bkz. [Özellikler](sql-database-features.md)
- Azure SQL veritabanlarına genel bakış için bkz. [SQL veritabanına genel bakış](sql-database-overview.md)
- Transact-SQL desteği ve farklılıkları hakkında bilgi edinmek için bkz. [Azure SQL Veritabanı Transact-SQL farklılıkları](sql-database-transact-sql-information.md).
- **Hizmet katmanınızı** temel alan özel kaynak kotaları ve sınırlamalar hakkında bilgi edinmek için. Hizmet katmanlarına genel bir bakış için bkz. [SQL Veritabanı hizmet katmanları](sql-database-service-tiers.md).
- Güvenlik özelliklerine genel bakış için bkz. [Azure SQL Veritabanı Güvenliğine Genel Bakış](sql-database-security-overview.md).
- Sürücü kullanılabilirliği ve SQL Veritabanı desteği hakkında bilgi edinmek için bkz. [SQL Veritabanı ve SQL Server için Bağlantı Kitaplıkları](sql-database-libraries.md).




<!--HONumber=Dec16_HO4-->


