---
title: "SQL Veritabanı Uygulaması Geliştirmeye Genel Bakış | Microsoft Belgeleri"
description: "SQL Veritabanına bağlanan uygulamalar için kullanılabilen bağlantı kitaplıkları ve en iyi uygulamalar hakkında bilgi edinin."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: genemi
ms.assetid: 67c02204-d1bd-4622-acce-92115a7cde03
ms.service: sql-database
ms.custom: development
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: 18dc3cce7451d90b6b65b990b80c05e7f6decb56


---
# <a name="sql-database-application-development-overview"></a>SQL Veritabanı Uygulaması Geliştirmeye Genel Bakış
Bu makalede, geliştiricilerin Azure SQL Veritabanı ile bağlantı kurmak üzere kod yazarken dikkat etmesi gereken noktalara yer verilmiştir.

> [!TIP]
> Sunucu oluşturma, sunucu tabanlı güvenlik duvarı oluşturma, sunucu özelliklerini görüntüleme, SQL Server Management Studio'yu kullanarak bağlanma, ana veritabanını sorgulama, örnek veritabanı ve boş veritabanı oluşturma, veritabanı özelliklerini sorgulama, SQL Server Management Studio kullanarak bağlanma ve örnek veritabanını sorgulama adımlarını gösteren bir öğreticiye ihtiyacınız varsa bkz. [Başlangıç Öğreticisi](sql-database-get-started.md).
>

## <a name="language-and-platform"></a>Dil ve platform
Çeşitli programlama dilleri ve platformları için kod örnekleri mevcuttur. Kod örneklerinin bağlantılarını şu sayfada bulabilirsiniz: 

* Daha Fazla Bilgi: [SQL Veritabanı ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md)

## <a name="resource-limitations"></a>Kaynak sınırlamaları
Azure SQL Veritabanı, bir veritabanı tarafından kullanılabilecek kaynakları iki farklı sistemle yönetir: Kaynak İdaresi ve Sınır Zorlama.

* Daha Fazla Bilgi: [Azure SQL Veritabanı kaynak limitleri](sql-database-resource-limits.md)

## <a name="security"></a>Güvenlik
Azure SQL Veritabanı, bir SQL Veritabanında erişim sınırlama, veri koruma ve izleme etkinlikleri için kaynaklar sunar.

* Daha Fazla Bilgi: [SQL Veritabanınızı güvenli hale getirme](sql-database-security-overview.md)

## <a name="authentication"></a>Kimlik Doğrulaması
* Azure SQL Veritabanı, SQL Server kimlik doğrulama kullanıcıları ve oturum açma bilgilerinin yanı sıra [Azure Active Directory kimlik doğrulama](sql-database-aad-authentication.md) kullanıcılarını ve oturum açma bilgilerini destekler.
* Varsayılan *ana* veritabanını kullanma yerine belirli bir veritabanını belirtmeniz gerekir.
* Başka bir veritabanına geçiş yapmak için SQL Veritabanında Transact-SQL **USE myDatabaseName;** deyimini kullanamazsınız.
* Daha fazla bilgi: [SQL Veritabanı güvenliği: Veritabanı erişim ve oturum açma güvenliğini yönetme](sql-database-manage-logins.md)

## <a name="resiliency"></a>Dayanıklılık
SQL Veritabanına bağlanırken geçici bir hata oluşması halinde kodunuzun çağrıyı yeniden denemesi gerekir.  Yeniden deneme mantığının SQL Veritabanını aynı anda yeniden deneme yapan birden fazla istemciyle boğmaması için geri alma mantığını kullanmasını öneririz.

* Kod örnekleri: Yeniden deneme mantığını gösteren kod örnekleri için şu sayfada istediğiniz dile ait örnekleri inceleyebilirsiniz: [SQL Veritabanı ve SQL Server için bağlantı kitapları](sql-database-libraries.md)
* Daha fazla bilgi: [SQL Veritabanı istemci programları için hata iletileri](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Bağlantıları Yönetme
* İstemci bağlantısı mantığınızda varsayılan zaman aşımını 30 saniye olacak şekilde geçersiz kılın.  15 saniyelik varsayılan değer, internet kullanan bağlantılar için çok kısadır.
* [Bağlantı havuzu](http://msdn.microsoft.com/library/8xx3tyca.aspx) kullanıyorsanız programınız etkin olarak kullanmadığında ve yeniden kullanma hazırlığı yapmadığında bağlantıyı kapatın.

## <a name="network-considerations"></a>Ağ Konusunda Dikkat Edilmesi Gerekenler
* İstemci programınızı barındıran bilgisayarda güvenlik duvarının 1433 numaralı bağlantı noktasından giden TCP iletişimine izin verdiğinden emin olun.  Daha fazla bilgi: [Azure SQL Veritabanı güvenlik duvarını yapılandırma](sql-database-configure-firewall-settings.md)
* İstemciniz bir Azure sanal makine (VM) üzerinde çalışırken istemci programınız SQL Veritabanı V12 bağlantısı kuruyorsa, VM'de belirli bağlantı noktası aralıklarını açmanız gerekir. Daha fazla bilgi: [ADO.NET 4.5 ve SQL Veritabanı V12 için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md)
* Bazen Azure SQL Veritabanı V12 ile yapılan istemci bağlantıları, proxy'yi atlar ve veritabanı ile doğrudan etkileşim kurar. 1433 dışındaki bağlantı noktaları önemli hale gelmiştir. Daha fazla bilgi: [ADO.NET 4.5 ve SQL Veritabanı V12 için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md)

## <a name="data-sharding-with-elastic-scale"></a>Elastik Ölçeklendirmeyle Veri Parçalama
Elastik Ölçeklendirme, ölçek büyütme (ve küçültme) işlemlerini kolaylaştırır. 

* [Azure SQL Veritabanı ile Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md)
* [Azure SQL Veritabanı Elastik Ölçeklendirmeyi Kullanmaya Başlama](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Sonraki adımlar
Tüm [SQL Veritabanı özelliklerini](https://azure.microsoft.com/services/sql-database/) keşfedin.




<!--HONumber=Dec16_HO4-->


