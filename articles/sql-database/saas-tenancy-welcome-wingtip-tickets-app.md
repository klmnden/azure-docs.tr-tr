---
title: Wingtips uygulaması - Azure SQL veritabanı'na Hoş Geldiniz | Microsoft Docs
description: Veritabanı kiralama modelleri hakkında ve Azure SQL veritabanı'nda bulut ortamı için örnek Wingtips SaaS uygulaması hakkında bilgi edinin.
keywords: sql veritabanı öğreticisi
services: sql-database
author: billgib
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: billgib
ms.openlocfilehash: b4f91ab1e6debb39980eeedc58249b50b5ac85e0
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="the-wingtip-tickets-saas-application"></a>Wingtip biletleri SaaS uygulaması

Aynı *Wingtip biletleri* SaaS uygulamasının her üç örnekleri uygulanır. Uygulama listesi ve küçük görebildikleri - tiyatrolar, Sinek vb. hedefleme SaaS uygulama raporlama basit bir olaydır. Her salonundan uygulamanın bir kiracı olduğunu ve kendi verilerini: salonundan ayrıntıları, olaylar, müşteriler, bilet siparişler, vb. listesi.  Uygulama Yönetimi komut dosyaları ve öğreticiler, birlikte bir uçtan uca SaaS senaryosunu gösterir. Bu, izleme ve performans, şema yönetimi ve çapraz Kiracı raporlama ve analiz yönetme sağlama kiracılar içerir.

## <a name="three-saas-application-and-tenancy-patterns"></a>Üç SaaS uygulama ve Kiracı desenleri

Uygulamanın üç sürümlerinde kullanılabilir; Her Azure SQL veritabanı farklı bir veritabanı kiralama düzeni araştırır.  İlk Kiracı başına bir tek başına uygulama kendi veritabanıyla kullanır. İkinci bir çok kiracılı uygulama Kiracı başına bir veritabanı kullanır. Üçüncü örnek bir çok kiracılı uygulama parçalı çok Kiracı veritabanları ile kullanır.

![Üç kiralama desenleri][image-three-tenancy-patterns]

 Her bir örnek uygulama kodu, artı yönetim komut dosyaları ve tasarım ve yönetim desenleri çeşitli keşfedin öğreticiler içerir.  Her örnek daha düşük bir değer, en fazla beş dakika dağıtır.  Tasarım ve yönetim farklılıkları karşılaştırmak için üç dağıtılan yan yana olabilir.

## <a name="standalone-application-per-tenant-pattern"></a>Kiracı deseni başına tek başına uygulama

Kiracı deseni başına tek başına app tek bir kiracı uygulama veritabanı ile her bir kiracı için kullanır. Her bir kiracının uygulaması, kendi veritabanı dahil olmak üzere ayrı Azure kaynak grubuna dağıtılır. Kaynak grubu hizmet sağlayıcısının abonelik veya kiracının abonelik dağıtılmış ve kiracının adınıza sağlayıcısı tarafından yönetilir. Kiracı deseni başına tek başına uygulama büyük Kiracı yalıtımı sağlar, ancak genellikle en. birden çok Kiracı arasında kaynakları paylaşmak için fırsat olarak pahalı.  Bu deseni, daha karmaşık olabilir ve hangi kiracılar küçük sayılara dağıtılan uygulamalar için uygundur.  Tek başına dağıtımlarında uygulama her bir kiracı için daha kolay içinde özelleştirilebilir diğer desenleri.  

Kullanıma [öğreticileri] [ docs-tutorials-for-wingtip-sa] ve github'daki kod [.../Microsoft/WingtipTicketsSaaS-StandaloneApp][github-code-for-wingtip-sa].

## <a name="database-per-tenant-pattern"></a>Kiracı deseni başına veritabanı

Kiracı deseni başına Kiracı yalıtımı ile ilgili bir kaygınız ve paylaşılan kaynakları ekonomik kullanılmasına merkezi bir hizmeti çalıştırmak istediğiniz hizmet sağlayıcıları için etkili bir veritabanıdır. Her salonundan veya Kiracı için bir veritabanı oluşturulur ve tüm veritabanları merkezi olarak yönetilir. Veritabanları, maliyet tasarrufu sağlamak için esnek havuzlar ve kiracıların öngörülemeyen iş yükü desenlerinden yararlanır kolay performans yönetimi barındırılabilir. Katalog veritabanına kiracılar ve kendi veritabanlarını arasındaki eşlemeyi tutar. Bu eşleme parça eşleme yönetim özelliklerini kullanarak yönetilen [esnek veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md), uygulamaya verimli bağlantı yönetimi sağlar.

Kullanıma [öğreticileri] [ docs-tutorials-for-wingtip-dpt] ve github'daki kod [.../Microsoft/WingtipTicketsSaaS-DbPerTenant][github-code-for-wingtip-dpt].

## <a name="sharded-multi-tenant-database-pattern"></a>Parçalı çok Kiracı veritabanı düzeni

Çok Kiracı veritabanı, Kiracı ve azaltılmış Kiracı yalıtımı ile kurulumunuza göre daha düşük maliyetli arayan hizmet sağlayıcıları için geçerlidir. Bu desen aşağı Kiracı başına maliyet yürüten tek bir veritabanı içine kiracılar çok sayıda paket sağlar. Yakın sonsuz ölçek tarafından parçalama mümkündür kiracılar birden fazla veritabanı üzerinden. Katalog veritabanına kiracılar veritabanlarına eşler.  

Bu deseni de sağlayan bir *karma* içinde bir veritabanında birden çok kiracıya maliyetle en iyi duruma getirme veya için yalıtım kendi veritabanında tek bir kiracı ile en iyi duruma getirme modeli. Seçimi bir kiracı tarafından Kiracı temelinde ya da Kiracı sağlanan ya da daha sonra uygulama üzerinde hiçbir etkisi olmadan olduğunda yapılabilir.  Kiracılar grupları farklı değerlendirilmesi gerektiğinde bu modeli etkili bir şekilde kullanılabilir. Örneğin, Premium kiracılar kendi veritabanlarına atanabilir sırada paylaşılan veritabanları için düşük maliyetli kiracılar atanabilir. 

Kullanıma [öğreticileri] [ docs-tutorials-for-wingtip-mt] ve github'daki kod [.../Microsoft/WingtipTicketsSaaS-MultiTenantDb][github-code-for-wingtip-mt].

## <a name="next-steps"></a>Sonraki adımlar

#### <a name="conceptual-descriptions"></a>Kavramsal açıklamaları

- Uygulama kiralama düzenleri daha ayrıntılı bir açıklaması için bkz. [çok kiracılı SaaS veritabanı kiralama desenleri][saas-tenancy-app-design-patterns-md]

#### <a name="tutorials-and-code"></a>Öğreticiler ve kod

- Tek başına app Kiracı başına:
    - [Tek başına uygulama öğreticileri ] [ docs-tutorials-for-wingtip-sa].
    - [Github'da tek başına uygulama kodunu][github-code-for-wingtip-sa].

- Veritabanı Kiracı başına:
    - [Kiracı başına veritabanı için öğreticileri][docs-tutorials-for-wingtip-dpt].
    - [Veritabanı github'da Kiracı başına kodunu][github-code-for-wingtip-dpt].

- Parçalı çok kiracılı:
    - [Öğreticileri parçalı çoklu kiracı için][docs-tutorials-for-wingtip-mt].
    - [Github'da parçalı çoklu kiracı için kod][github-code-for-wingtip-mt].



<!-- Image references. -->

[image-three-tenancy-patterns]: media/saas-tenancy-welcome-wingtip-tickets-app/three-tenancy-patterns.png "Üç kiralama desenleri."

<!-- Docs.ms.com references. -->

[saas-tenancy-app-design-patterns-md]: saas-tenancy-app-design-patterns.md

<!-- WWWeb http references. -->

[docs-tutorials-for-wingtip-sa]: https://aka.ms/wingtipticketssaas-sa
[github-code-for-wingtip-sa]: https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp

[docs-tutorials-for-wingtip-dpt]: https://aka.ms/wingtipticketssaas-dpt
[github-code-for-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant

[docs-tutorials-for-wingtip-mt]: https://aka.ms/wingtipticketssaas-mt
[github-code-for-wingtip-mt]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb

