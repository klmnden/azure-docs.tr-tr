---
title: Wingtips'e app - Azure SQL veritabanı'na | Microsoft Docs
description: Azure SQL veritabanı'nda bulut ortamı için örnek Wingtips'e SaaS uygulaması ve veritabanı kiralama modelleri hakkında bilgi edinin.
keywords: sql veritabanı öğreticisi
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: billgib
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 963d7d44ef3ef77604fc5a9faac479a9e4c91ee6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61487378"
---
# <a name="the-wingtip-tickets-saas-application"></a>Wingtip bilet SaaS uygulaması

Aynı *Wingtip bilet* SaaS uygulaması, her üç örnekleri uygulanır. Uygulama listesi ve SaaS uygulama küçük venues - sinema salonlarında, kulüpleri hedefleyen çağrı oluşturma basit bir olaydır. Her mekan uygulamasının Kiracı olup kendi veri içerir: mekan ayrıntıları, olayları, müşterilerin, bilet siparişler vb. listeler.  Yönetim betikleri ve öğreticiler, uygulama bir uçtan uca SaaS senaryo gösterilir. Bu, izleme ve performans, şema yönetimi ve kiracılar arası raporlama ve analiz yönetme sağlama kiracılar içerir.

## <a name="three-saas-application-and-tenancy-patterns"></a>Üç SaaS uygulaması ve kiracılı desenleri

Uygulamanın üç sürümleri mevcuttur; Her Azure SQL veritabanı'nda farklı veritabanı kiralama desen keşfediyor.  İlk Kiracı başına tek başına uygulama ile kendi veritabanı kullanır. İkincisi, Kiracı başına bir veritabanı ile çok kiracılı uygulama kullanır. Üçüncü örnek çok kiracılı uygulama parçalı çok kiracılı veritabanlarıyla kullanır.

![Üç çok kiracılı desenleri][image-three-tenancy-patterns]

 Her örnek, uygulama kodunun yanı sıra yönetim betikleri ve bir dizi tasarım ve yönetim düzenlerini keşfetmek öğreticiler içerir.  Her örnek, daha düşük bir değer, beş dakika dağıtır.  Tasarım ve yönetim farklılıkları karşılaştırabilmeniz üç dağıtılan yan yana olabilir.

## <a name="standalone-application-per-tenant-pattern"></a>Kiracı deseni başına tek başına uygulama

Kiracı deseni başına tek başına uygulama tek kiracılı bir uygulama, her Kiracı için bir veritabanı kullanır. Kendi veritabanı dahil olmak üzere, her bir kiracının uygulama ayrı bir Azure kaynak grubuna dağıtılır. Kaynak grubunu, hizmet sağlayıcının abonelik veya kiracının aboneliğinin dağıtılmış ve kiracının adınıza sağlayıcısı tarafından yönetilen. Kiracı deseni başına tek başına uygulama büyük Kiracı yalıtımı sağlar, ancak genellikle en iyi olan pahalı kaynakları birden çok Kiracı arasında paylaşmak için bir fırsat olarak.  Bu düzen, daha karmaşık olabilir ve kiracıların daha küçük sayılar için dağıtılan uygulamalar için uygundur.  Tek başına dağıtımlarında, uygulamayı her Kiracı için daha kolay özelleştirilebilir diğer desenleri.  

Kullanıma [öğreticiler] [ docs-tutorials-for-wingtip-sa] ve github'daki [.../Microsoft/WingtipTicketsSaaS-StandaloneApp][github-code-for-wingtip-sa].

## <a name="database-per-tenant-pattern"></a>Veritabanı başına Kiracı düzeni

Veritabanı başına Kiracı düzeni Kiracı yalıtımı ile ilgili olan ve maliyet açısından verimli paylaşılan kaynaklar kullanımına izin verir, merkezi bir hizmete çalıştırmak istediğiniz hizmet sağlayıcıları için etkili olur. Her mekan veya Kiracı için bir veritabanı oluşturulur ve tüm veritabanlarına merkezi olarak yönetilir. Veritabanları, elastik havuzlar, maliyet tasarrufu sağlamak ve kiracıların öngörülemez iş yükü desenlerinden yararlanır kolay performans yönetimi, barındırılabilir. Katalog veritabanı, kiracılar ve bunların veritabanları arasındaki eşlemeyi içerir. Bu eşleme parça eşleme yönetimi özelliklerini kullanarak yönetilen [elastik veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md), uygulamaya verimli bağlantı yönetimi sağlar.

Kullanıma [öğreticiler] [ docs-tutorials-for-wingtip-dpt] ve github'daki [.../Microsoft/WingtipTicketsSaaS-DbPerTenant][github-code-for-wingtip-dpt].

## <a name="sharded-multi-tenant-database-pattern"></a>Parçalı çok kiracılı veritabanı deseni

Çok kiracılı veritabanları, Kiracı ve azaltılmış Kiracı yalıtımı ile tamamdır başına daha düşük maliyetli aranıyor hizmet sağlayıcıları için geçerlidir. Bu düzen, Kiracı başına maliyet aşağı sürüş tek bir veritabanı içine kiracılar çok sayıda paket sağlar. Neredeyse sınırsız ölçeklendirme ile parçalama mümkündür kiracılar genelinde birden çok veritabanı. Katalog veritabanı için veritabanlarını kiracılar eşler.  

Bu düzen ayrıca sağlar bir *karma* içinde maliyeti ile birden fazla Kiracı veritabanı için en iyi duruma getirme, veya yapabilirsiniz kendi veritabanında tek bir kiracı yalıtımı için en iyi duruma getirme modeli. Kiracı sağlanan ya da daha sonra uygulama üzerinde herhangi bir etkisi olduğunda seçimi ya da bir kiracı tarafından Kiracı temelinde yapılabilir.  Bu model, kiracıların grupları farklı şekilde değerlendirilmesi gerektiğinde etkili bir şekilde kullanılabilir. Örneğin, Premium kiracılar kendi veritabanlarına atanabilirken paylaşılan veritabanları için düşük maliyetli kiracılar atanabilir. 

Kullanıma [öğreticiler] [ docs-tutorials-for-wingtip-mt] ve github'daki [.../Microsoft/WingtipTicketsSaaS-MultiTenantDb][github-code-for-wingtip-mt].

## <a name="next-steps"></a>Sonraki adımlar

#### <a name="conceptual-descriptions"></a>Kavramsal açıklamaları

- Daha ayrıntılı bir açıklaması uygulama kiracılı desenleri ve kullanılabilir [çok kiracılı SaaS veritabanı kiracılı desenleri][saas-tenancy-app-design-patterns-md]

#### <a name="tutorials-and-code"></a>Öğreticiler ve kod

- Kiracı başına tek başına uygulama:
    - [Tek başına uygulama için öğreticiler][docs-tutorials-for-wingtip-sa].
    - [Github'da tek başına uygulama kodunu][github-code-for-wingtip-sa].

- Kiracı başına veritabanı:
    - [Kiracı başına veritabanı için öğreticiler][docs-tutorials-for-wingtip-dpt].
    - [GitHub üzerinde Kiracı başına veritabanı için kod][github-code-for-wingtip-dpt].

- Parçalı çok kiracılı:
    - [Öğreticileri için parçalı çok kiracılı][docs-tutorials-for-wingtip-mt].
    - [Github'da parçalı çok kiracılı kodunu][github-code-for-wingtip-mt].



<!-- Image references. -->

[image-three-tenancy-patterns]: media/saas-tenancy-welcome-wingtip-tickets-app/three-tenancy-patterns.png "Üç kiracılı desenleri."

<!-- Docs.ms.com references. -->

[saas-tenancy-app-design-patterns-md]: saas-tenancy-app-design-patterns.md

<!-- WWWeb http references. -->

[docs-tutorials-for-wingtip-sa]: https://aka.ms/wingtipticketssaas-sa
[github-code-for-wingtip-sa]: https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp

[docs-tutorials-for-wingtip-dpt]: https://aka.ms/wingtipticketssaas-dpt
[github-code-for-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant

[docs-tutorials-for-wingtip-mt]: https://aka.ms/wingtipticketssaas-mt
[github-code-for-wingtip-mt]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb

