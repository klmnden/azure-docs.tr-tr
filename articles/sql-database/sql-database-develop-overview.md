---
title: SQL Veritabanı Uygulaması Geliştirmeye Genel Bakış | Microsoft Belgeleri
description: SQL Veritabanına bağlanan uygulamalar için kullanılabilen bağlantı kitaplıkları ve en iyi uygulamalar hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: genemi
manager: craigg
ms.date: 06/20/2018
ms.openlocfilehash: 58f902edcd417809d1bb47a231cb1c2ac2f579d1
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063600"
---
# <a name="sql-database-application-development-overview"></a>SQL veritabanı uygulaması geliştirmeye genel bakış
Bu makalede, geliştiricilerin Azure SQL Veritabanı ile bağlantı kurmak üzere kod yazarken dikkat etmesi gereken noktalara yer verilmiştir.

> [!TIP]
> Sunucu oluşturma, sunucu tabanlı güvenlik duvarı oluşturma, sunucu özelliklerini görüntüleme, SQL Server Management Studio'yu kullanarak bağlanma, ana veritabanını sorgulama, örnek veritabanı ve boş veritabanı oluşturma, veritabanı özelliklerini sorgulama, SQL Server Management Studio kullanarak bağlanma ve örnek veritabanını sorgulama adımlarını gösteren bir öğreticiye ihtiyacınız varsa bkz. [Başlangıç Öğreticisi](sql-database-get-started-portal.md).
>

## <a name="language-and-platform"></a>Dil ve platform
Çeşitli programlama dilleri ve platformları için kod örnekleri mevcuttur. Kod örneklerinin bağlantılarını şu sayfada bulabilirsiniz: 

* Daha fazla bilgi: [SQL veritabanı ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md).

## <a name="tools"></a>Araçlar 
Açık kaynak araçlarla yararlanabileceğiniz [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli), [VS Code](https://code.visualstudio.com/). Ayrıca, Azure SQL Veritabanı [Visual Studio](https://www.visualstudio.com/downloads/) ve [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) gibi Microsoft araçlarıyla birlikte çalışır.  Azure Yönetim Portalı, PowerShell ve REST API'leri de ek verimlilik elde etmenize yardımcı olabilir.

## <a name="resource-limitations"></a>Kaynak sınırlamaları
Azure SQL veritabanı iki farklı sistemle bir veritabanının kullanabileceği kaynakları yönetir: kaynak İdaresi hem de sınırlar. Daha fazla bilgi için bkz.

- [DTU tabanlı kaynak modeli sınırlarını - tek veritabanı](sql-database-dtu-resource-limits-single-databases.md)
- [DTU tabanlı kaynak modeli sınırlarını - elastik havuzlar](sql-database-dtu-resource-limits-elastic-pools.md)
- [Sanal çekirdek tabanlı kaynak sınırları - tek veritabanları](sql-database-vcore-resource-limits-single-databases.md)
- [Sanal çekirdek tabanlı kaynak sınırları - elastik havuzlar](sql-database-vcore-resource-limits-elastic-pools.md)

## <a name="security"></a>Güvenlik
Azure SQL Veritabanı, bir SQL Veritabanında erişim sınırlama, veri koruma ve izleme etkinlikleri için kaynaklar sunar.

* Daha fazla bilgi: [SQL veritabanınızın güvenliğini sağlama](sql-database-security-overview.md).

## <a name="authentication"></a>Kimlik Doğrulaması
* Azure SQL Veritabanı, SQL Server kimlik doğrulama kullanıcıları ve oturum açma bilgilerinin yanı sıra [Azure Active Directory kimlik doğrulama](sql-database-aad-authentication.md) kullanıcılarını ve oturum açma bilgilerini destekler.
* Varsayılan *ana* veritabanını kullanma yerine belirli bir veritabanını belirtmeniz gerekir.
* Başka bir veritabanına geçiş yapmak için SQL Veritabanında Transact-SQL **USE myDatabaseName;** deyimini kullanamazsınız.
* Daha fazla bilgi: [SQL veritabanı güvenliği: veritabanı erişim ve oturum açma güvenliğini yönetme](sql-database-manage-logins.md).

## <a name="resiliency"></a>Dayanıklılık
SQL Veritabanına bağlanırken geçici bir hata oluşması halinde kodunuzun çağrıyı yeniden denemesi gerekir.  Yeniden deneme mantığının SQL Veritabanını aynı anda yeniden deneme yapan birden fazla istemciyle boğmaması için geri alma mantığını kullanmasını öneririz.

* Kod örnekleri: yeniden deneme mantığı gösteren kod örnekleri için tercih ettiğiniz dili için örneklerine bakın: [SQL veritabanı ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md).
* Daha fazla bilgi: [SQL veritabanı istemci programları için hata iletileri](sql-database-develop-error-messages.md).

## <a name="managing-connections"></a>Bağlantıları yönetme
* İstemci bağlantısı mantığınızda varsayılan zaman aşımını 30 saniye olacak şekilde geçersiz kılın.  15 saniyelik varsayılan değer, internet kullanan bağlantılar için çok kısadır.
* [Bağlantı havuzu](http://msdn.microsoft.com/library/8xx3tyca.aspx) kullanıyorsanız programınız etkin olarak kullanmadığında ve yeniden kullanma hazırlığı yapmadığında bağlantıyı kapatın.

## <a name="network-considerations"></a>Ağ konuları
* İstemci programınızı barındıran bilgisayarda güvenlik duvarının 1433 numaralı bağlantı noktasından giden TCP iletişimine izin verdiğinden emin olun.  Daha fazla bilgi: [bir Azure SQL veritabanı güvenlik duvarını](sql-database-configure-firewall-settings.md).
* İstemciniz bir Azure sanal makine (VM) üzerinde çalışırken istemci programınız SQL veritabanına bağlanır, VM'de belirli bağlantı noktası aralıklarını açmanız gerekir. Daha fazla bilgi: [ADO.NET 4.5 ve SQL veritabanı için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).
* Azure SQL veritabanı istemci bağlantıları, bazen Proxy'yi atlar ve veritabanı ile doğrudan etkileşim. 1433 dışındaki bağlantı noktaları önemli hale gelmiştir. Daha fazla bilgi için [Azure SQL veritabanı bağlantı mimarisi](sql-database-connectivity-architecture.md) ve [ADO.NET 4.5 ve SQL veritabanı için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="data-sharding-with-elastic-scale"></a>Esnek ölçeklendirme ile veri parçalama
Esnek ölçek dışarı (ve içeri) doğru ölçeklenmesi işlemini basitleştirir. 

* [Azure SQL veritabanı ile çok kiracılı SaaS uygulamaları için Tasarım Düzenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* [Verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md).
* [Azure SQL veritabanı esnek ölçeklendirme önizlemesini kullanmaya başlama](sql-database-elastic-scale-get-started.md).

## <a name="next-steps"></a>Sonraki adımlar
Tüm [SQL Veritabanı özelliklerini](sql-database-technical-overview.md) keşfedin.
