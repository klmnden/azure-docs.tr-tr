---
title: "Azure SQL veritabanına genel bakış | Microsoft Belgeleri"
description: "Bu sayfada Azure SQL veritabanlarına genel bir bakış sağlanmıştır."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: single databases
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/28/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: 315fb49ba25c46afaa6571d9249ecd1c8da13e91


---
# <a name="azure-sql-database-overview"></a>Azure SQL veritabanına genel bakış
Bu konu başlığında, Azure SQL veritabanlarına genel bir bakış sağlanmıştır. Azure SQL mantıksal sunucuları hakkında bilgi edinmek için bkz. [Mantıksal sunucular](sql-database-server-overview.md).

## <a name="what-is-azure-sql-database"></a>Azure SQL veritabanı nedir?
Azure SQL Veritabanı’ndaki her bir veritabanı bir mantıksal sunucuyla ilişkilidir. Veritabanları aşağıdaki biçimlerde olabilir:

- [Kendi kaynak kümesine](sql-database-what-is-a-dtu.md#what-are-database-transaction-units-dtus) (DTU) sahip tek veritabanı
- [Bir kaynak kümesini (eDTU) paylaşan](sql-database-what-is-a-dtu.md#what-are-elastic-database-transaction-units-edtus) bir [elastik havuzun](sql-database-elastic-pool.md) bir parçası
- Tek başına veya havuza eklenmiş veritabanlarından oluşan [parçalı veritabanlarından oluşan ölçeği genişletilmiş kümenin](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) bir parçası
- [Çok kiracılı SaaS tasarım desenine](sql-database-design-patterns-multi-tenancy-saas-applications.md) katılan ve veritabanları tek başına ya da havuza eklenmiş (veya her ikisi de) olabilecek bir veritabanı kümesinin bir parçası 

## <a name="how-do-i-connect-and-authenticate-to-an-azure-sql-database"></a>Bir Azure SQL veritabanına nasıl bağlanıp kimliğimi doğrulayabilirim?

- **Kimlik doğrulama ve yetkilendirme**: Azure SQL Veritabanı SQL kimlik doğrulamasını ve Azure Active Directory Kimlik Doğrulaması’nı destekler (belirli kısıtlamalar vardır; kimlik doğrulama için bkz. [Azure Active Directory Kimlik Doğrulaması ile SQL Veritabanı’na bağlanma](sql-database-aad-authentication.md). Azure SQL veritabanlarına sunucunun asıl veritabanı üzerinden; bir kullanıcının veritabanına ise doğrudan bağlanarak kimliğinizi doğrulayabilirsiniz. Daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanında Veritabanlarını ve Oturum Açma İşlemlerini Yönetme](sql-database-manage-logins.md). Windows Kimlik Doğrulaması desteklenmez. 
- **TDS**: Microsoft Azure SQL Veritabanı, tablosal veri akışı (TDS) protokolü istemcisinin 7.3 veya sonraki sürümlerini destekler.
- **TCP/IP**: Yalnızca TCP/IP bağlantılarına izin verilir.
- **SQL Veritabanı güvenlik duvarı**: SQL Veritabanı güvenlik duvarı, verilerinizin korunmasına yardımcı olmak amacıyla siz hangi bilgisayarların izni olduğunu belirtene kadar veritabanını sunucunuza veya onun veritabanlarına yönelik tüm erişimi engeller. Bkz. [Güvenlik Duvarları](sql-database-firewall-configure.md)

## <a name="what-collations-are-supported"></a>Hangi harmanlamalar destekleniyor?
Microsoft Azure SQL Veritabanı tarafından kullanılan varsayılan veritabanı harmanlaması **SQL_LATIN1_GENERAL_CP1_CI_AS** şeklindedir; buradaki **LATIN1_GENERAL** İngilizce (Amerika Birleşik Devletleri), **CP1** kod sayfası 1252, **CI** büyük-küçük harfe duyarlı ve **AS** is aksan duyarlıdır. V12 veritabanları için harmanlamayı değiştirmek mümkün değildir. Harmanlamanın nasıl ayarlanacağı hakkında daha fazla bilgi edinmek için bkz. [HARMANLAMA (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="what-are-the-naming-requirements-for-database-objects"></a>Veritabanı öğeleri için adlandırma gereksinimleri nelerdir?

Tüm yeni öğelerin adları tanımlayıcılara ilişkin SQL Server kurallarına uygun olmalıdır. Daha fazla bilgi edinmek için bkz. [Tanımlayıcılar](https://msdn.microsoft.com/library/ms175874.aspx).

## <a name="what-features-are-supported-by-azure-sql-databases"></a>Azure SQL veritabanları tarafından hangi özellikler destekleniyor?

Desteklenen özellikler hakkında bilgi edinmek için bkz. [Özellikler](sql-database-features.md). Belirli türlerdeki özelliklere yönelik destek olmayışı hakkında daha fazla arka plan bilgisi edinmek için ayrıca bkz. [Azure SQL Veritabanı Transact-SQL farkları](sql-database-transact-sql-information.md).

## <a name="how-do-i-manage-an-azure-sql-database"></a>Bir Azure SQL veritabanını nasıl yönetebilirim?

Azure SQL Veritabanı mantıksal sunucularını birkaç yöntemle yönetebilirsiniz:
- [Azure Portal](sql-database-manage-portal.md)
- [PowerShell](sql-database-manage-powershell.md)
- [Transact-SQL](sql-database-manage-azure-ssms.md)
- [REST](/rest/api/sql/)

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL Veritabanı hizmeti hakkında bilgi edinmek için bkz. [SQL Veritabanı nedir?](sql-database-technical-overview.md)
- Desteklenen özellikler hakkında bilgi edinmek için bkz. [Özellikler](sql-database-features.md)
- Azure SQL mantıksal sunucularına yönelik bir genel bakış için bkz. [SQL Veritabanı mantıksal sunucusuna genel bakış](sql-database-server-overview.md)
- Transact-SQL desteği ve farklılıkları hakkında bilgi edinmek için bkz. [Azure SQL Veritabanı Transact-SQL farklılıkları](sql-database-transact-sql-information.md).
- **Hizmet katmanınızı** temel alan özel kaynak kotaları ve sınırlamalar hakkında bilgi edinmek için. Hizmet katmanlarına genel bir bakış için bkz. [SQL Veritabanı hizmet katmanları](sql-database-service-tiers.md).
- Güvenlik özelliklerine genel bakış için bkz. [Azure SQL Veritabanı Güvenliğine Genel Bakış](sql-database-security-overview.md).
- Sürücü kullanılabilirliği ve SQL Veritabanı desteği hakkında bilgi edinmek için bkz. [SQL Veritabanı ve SQL Server için Bağlantı Kitaplıkları](sql-database-libraries.md).




<!--HONumber=Dec16_HO4-->


