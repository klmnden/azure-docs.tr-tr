---
title: SQL Veritabanı Uygulaması Geliştirmeye Genel Bakış | Microsoft Belgeleri
description: SQL Veritabanına bağlanan uygulamalar için kullanılabilen bağlantı kitaplıkları ve en iyi uygulamalar hakkında bilgi edinin.
services: sql-database
author: stevestein
manager: craigg
ms.reviewer: genemi
ms.service: sql-database
ms.custom: develop apps
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 37f143ce9dabdc2d0eaf6c60dd551c682a141c17
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="sql-database-application-development-overview"></a>SQL veritabanı uygulaması geliştirmeye genel bakış
Bu makalede, geliştiricilerin Azure SQL Veritabanı ile bağlantı kurmak üzere kod yazarken dikkat etmesi gereken noktalara yer verilmiştir.

> [!TIP]
> Sunucu oluşturma, sunucu tabanlı güvenlik duvarı oluşturma, sunucu özelliklerini görüntüleme, SQL Server Management Studio'yu kullanarak bağlanma, ana veritabanını sorgulama, örnek veritabanı ve boş veritabanı oluşturma, veritabanı özelliklerini sorgulama, SQL Server Management Studio kullanarak bağlanma ve örnek veritabanını sorgulama adımlarını gösteren bir öğreticiye ihtiyacınız varsa bkz. [Başlangıç Öğreticisi](sql-database-get-started-portal.md).
>

## <a name="language-and-platform"></a>Dil ve platform
Çeşitli programlama dilleri ve platformları için kod örnekleri mevcuttur. Kod örneklerinin bağlantılarını şu sayfada bulabilirsiniz: 

* Daha fazla bilgi: [SQL Database ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md).

## <a name="tools"></a>Araçlar 
Gibi açık kaynaklı araçları yararlanabilirsiniz [cheetah](https://github.com/wunderlist/cheetah), [sql CLI](https://www.npmjs.com/package/sql-cli), [VS Code](https://code.visualstudio.com/). Ayrıca, Azure SQL Veritabanı [Visual Studio](https://www.visualstudio.com/downloads/) ve [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) gibi Microsoft araçlarıyla birlikte çalışır.  Azure Yönetim Portalı, PowerShell ve REST API'leri de ek verimlilik elde etmenize yardımcı olabilir.

## <a name="resource-limitations"></a>Kaynak sınırlamaları
Azure SQL Veritabanı, bir veritabanı tarafından kullanılabilecek kaynakları iki farklı sistemle yönetir: Kaynak İdaresi ve Sınır Zorlama.

* Daha fazla bilgi: [Azure SQL veritabanı DTU tabanlı kaynak modeli sınırları](sql-database-dtu-resource-limits.md) ve [Azure SQL veritabanı vCore tabanlı kaynak modeli sınırları (Önizleme)](sql-database-vcore-resource-limits.md).

## <a name="security"></a>Güvenlik
Azure SQL Veritabanı, bir SQL Veritabanında erişim sınırlama, veri koruma ve izleme etkinlikleri için kaynaklar sunar.

* Daha fazla bilgi: [SQL veritabanınızın güvenliğini sağlama](sql-database-security-overview.md).

## <a name="authentication"></a>Kimlik Doğrulaması
* Azure SQL Veritabanı, SQL Server kimlik doğrulama kullanıcıları ve oturum açma bilgilerinin yanı sıra [Azure Active Directory kimlik doğrulama](sql-database-aad-authentication.md) kullanıcılarını ve oturum açma bilgilerini destekler.
* Varsayılan *ana* veritabanını kullanma yerine belirli bir veritabanını belirtmeniz gerekir.
* Başka bir veritabanına geçiş yapmak için SQL Veritabanında Transact-SQL **USE myDatabaseName;** deyimini kullanamazsınız.
* Daha fazla bilgi: [SQL veritabanı güvenlik: veritabanı erişimi ve oturum açma güvenlik yönetmek](sql-database-manage-logins.md).

## <a name="resiliency"></a>Dayanıklılık
SQL Veritabanına bağlanırken geçici bir hata oluşması halinde kodunuzun çağrıyı yeniden denemesi gerekir.  Yeniden deneme mantığının SQL Veritabanını aynı anda yeniden deneme yapan birden fazla istemciyle boğmaması için geri alma mantığını kullanmasını öneririz.

* Kod örnekleri: örnekler için tercih ettiğiniz dili gösteren kod örnekleri mantığı yeniden dene için bkz: [SQL Database ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md).
* Daha fazla bilgi: [SQL veritabanı istemci programları için hata iletileri](sql-database-develop-error-messages.md).

## <a name="managing-connections"></a>Bağlantıları yönetme
* İstemci bağlantısı mantığınızda varsayılan zaman aşımını 30 saniye olacak şekilde geçersiz kılın.  15 saniyelik varsayılan değer, internet kullanan bağlantılar için çok kısadır.
* [Bağlantı havuzu](http://msdn.microsoft.com/library/8xx3tyca.aspx) kullanıyorsanız programınız etkin olarak kullanmadığında ve yeniden kullanma hazırlığı yapmadığında bağlantıyı kapatın.

## <a name="network-considerations"></a>Ağ konuları
* İstemci programınızı barındıran bilgisayarda güvenlik duvarının 1433 numaralı bağlantı noktasından giden TCP iletişimine izin verdiğinden emin olun.  Daha fazla bilgi: [bir Azure SQL veritabanı güvenlik duvarı yapılandırma](sql-database-configure-firewall-settings.md).
* İstemciniz bir Azure sanal makine (VM) üzerinde çalışırken, istemci programınızı SQL veritabanına bağlanıyorsa, belirli bağlantı noktası aralıkları VM'de açmanız gerekir. Daha fazla bilgi: [ADO.NET 4.5 ve SQL veritabanı için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).
* Azure SQL veritabanı istemci bağlantıları bazen proxy sunucusunu atlamak ve veritabanı ile doğrudan etkileşim. 1433 dışındaki bağlantı noktaları önemli hale gelmiştir. Daha fazla bilgi için [Azure SQL veritabanı bağlantısı mimarisi](sql-database-connectivity-architecture.md) ve [ADO.NET 4.5 ve SQL veritabanı için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="data-sharding-with-elastic-scale"></a>Esnek ölçeklendirme ile veri parçalama
Esnek ölçeklendirme (ve) ölçeklendirme işlemini basitleştirir. 

* [Azure SQL veritabanı ile çok kiracılı SaaS uygulamaları için Tasarım desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* [Veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md).
* [Azure SQL veritabanı esnek ölçek Önizleme kullanmaya başlama](sql-database-elastic-scale-get-started.md).

## <a name="next-steps"></a>Sonraki adımlar
Tüm [SQL Veritabanı özelliklerini](sql-database-technical-overview.md) keşfedin.
