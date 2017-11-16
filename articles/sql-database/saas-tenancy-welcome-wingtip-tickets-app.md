---
title: "Wingtips uygulaması - Azure SQL veritabanı'na Hoş Geldiniz | Microsoft Docs"
description: "Veritabanı kiralama modelleri hakkında ve Azure SQL veritabanı'nda bulut ortamı için örnek Wingtips SaaS uygulaması hakkında bilgi edinin."
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: billgib
manager: craigg
editor: MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: billgib;genemi
ms.openlocfilehash: e10a954ba57782f4f79131ab583b5a73edf4ba02
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="welcome-to-the-wingtip-tickets-sample-saas-azure-sql-database-tenancy-app"></a>Wingtip biletleri örnek SaaS Azure SQL veritabanı kiralama uygulaması'na Hoş Geldiniz

Wingtip biletleri örnek SaaS Azure SQL veritabanı Kiracı uygulama ve onun öğreticiler Hoş Geldiniz. Veritabanı kiralama uygulamanızda barındırılması için ödeme istemcileriniz uygulamanızı sağlayan veri yalıtım modunu gösterir. Diğer istemci ile bir veritabanını paylaşan veya üzerinde şu anda basitleştirmek için kendisi için tam bir veritabanı ya da her istemci sahip.

## <a name="wingtip-tickets-app"></a>Wingtip biletleri uygulama

Wingtip biletleri örnek uygulama farklı veritabanı kiralama modelleri tasarımına etkilerini ve yönetimi için çok kiracılı SaaS uygulamaları gösterir. Eşlik eden öğreticileri doğrudan bu aynı etkileri açıklanmaktadır. Wingtip biletleri Azure SQL veritabanı oluşturulur.

Wingtip biletleri gerçek SaaS istemcileri tarafından kullanılan çeşitli tasarım ve yönetim senaryoları işlemek için tasarlanmıştır. Desenler ortaya çıktı kullanım için Wingtip anahtarların hesaba.

Kendi Azure aboneliğinize beş dakika içinde Wingtip biletleri uygulamayı yükleyebilir. Örnek veri ekleme çeşitli kiracılar için yüklemeyi içerir. Yüklemeleri olmamasından etkileşim veya birbiriyle müdahale güvenle modelleri için yönetim komut dosyaları ve uygulama yükleyebilirsiniz.

#### <a name="code-in-github"></a>Github'da kodu

Uygulama kodu ve yönetim komut dosyaları, Github'da bulunmaktadır:

- **Tek başına app** modeli: *(gün içinde yakında.)*
- **Kiracı başına veritabanı** modeli: [WingtipSaaS depo](https://github.com/Microsoft/WingtipSaaS/).
- **Parçalı çok kiracılı** modeli *karma*: *(gün içinde yakında.)*

Wingtip biletleri uygulama için temel aynı bir kod tüm listelenen önceki modelleri için yeniden kullanılır. Kendi SaaS projeleri başlatmak için kod github'dan kullanabilirsiniz.



## <a name="major-database-tenancy-models"></a>Ana veritabanı kiralama modelleri

Wingtip anahtarları listeleme ve SaaS uygulama raporlama bir olay olur. Wingtip görebildikleri tarafından gerekli hizmetleri sağlar. Aşağıdaki öğeler her salonundan için geçerlidir:

- Uygulamanızda barındırılması için ödeme yapar.
- Olan bir *Kiracı* Wingtip içinde.
- Olayları barındırır. Aşağıdaki olaylar oynayan:
    - Bilet fiyatları.
    - Bilet satış.
    - Bilet satın müşteriler.

Uygulama Yönetimi komut dosyaları ve öğreticiler, birlikte tam bir SaaS senaryo gösterir. Senaryo, aşağıdaki etkinliklerin içerir:

- Kiracılar sağlama.
- İzleme ve performansı yönetme.
- Şema Yönetimi.
- Çapraz Kiracı raporlama ve analiz.

Bu etkinlikler ne olursa olsun ölçek gerekli adresindeki sağlanır.



## <a name="code-samples-for-each-tenancy-model"></a>Her bir kiracı modeli için kod örnekleri

Uygulama modelleri kümesi vurgulanmış. Ancak, diğer uygulamalardan iki veya daha fazla model öğelerini karışımı.

#### <a name="standalone-app-model"></a>Tek başına uygulama modeli

![Tek başına uygulama modeli][standalone-app-model-62s]

Bu model tek kiracılı uygulama kullanır. Bu nedenle, bu model yalnızca bir veritabanı gerekir ve yalnızca bir kiracı için verileri depolar. Kiracı veritabanında diğer kiracıdan tam yalıtımı özgürlüğüne.

Kendi çalıştırmak her istemci için birçok farklı istemcilere uygulamanızı örneklerini satış yaparken, bu model kullanabilirsiniz. İstemci yalnızca Kiracı olur. Veritabanı yalnızca bir istemci için veri depolarken veritabanının istemci birçok müşteri için verileri depolar.

- *(Bu model için öğreticileri, burada birkaç gün içinde yayımlanır. Bir bağlantı burada olacaktır.)*

#### <a name="database-per-tenant"></a>Kiracı başına veritabanı

![Her Kiracı model veritabanı][database-per-tenant-model-35d]

Bu model uygulama örneğinde birden çok kiracıya sahiptir. Henüz yeni her bir kiracı için başka bir veritabanı yalnızca yeni Kiracı tarafından kullanım için ayrılır.

Bu model, her bir kiracı için tam veritabanı yalıtım sağlar. Azure SQL veritabanı hizmetinin bu modeli yatkýn yapmak için açıdan çok yönlülük sahiptir.

- [Bir SQL veritabanı çok kiracılı SaaS uygulama örneği giriş] [ saas-dbpertenant-wingtip-app-overview-15d] -bu modeli hakkında daha fazla bilgi yer almaktadır.

#### <a name="sharded-multi-tenant-databases-the-hybrid"></a>Parçalı çok Kiracı veritabanı, karma

![Parçalı çok Kiracı veritabanı modeli, karma][sharded-multitenantdb-model-hybrid-79m]

Bu model uygulama örneğinde birden çok kiracıya sahiptir. Bu model, birden çok kiracıya bazı veya tüm veritabanlarını da sahiptir. Bu model, böylece veritabanı yalıtım değeri tam değilse istemciler daha fazla ödeme yapabildiği farklı hizmet katmanları sunarak için uygundur.

Her veritabanı şeması, bir kiracı tanımlayıcı içerir. Yalnızca bir kiracı depolayan bile bu veritabanları Kiracı tanımlayıcısıdır.

- *(Bu model için öğreticileri, burada birkaç gün içinde yayımlanır. Bir bağlantı burada olacaktır.)*




## <a name="tutorials-for-each-tenancy-model"></a>Her bir kiracı modeli için eğitim

Her bir kiracı modeli tarafından aşağıdaki belgelenmiştir:

- Eğitmen makaleler kümesidir.
- Kaynak kodu modele adanmış bir Github deposuna depolanır:
    - Wingtip biletleri uygulama kodu.
    - Yönetim senaryoları için komut dosyası kodu.

#### <a name="tutorials-for-management-scenarios"></a>Öğreticileri yönetim senaryoları için

Eğitmen makaleler her model için aşağıdaki yönetim senaryoları kapsar:

- Kiracı sağlama.
- Performans izleme ve yönetim.
- Şema Yönetimi.
- Çapraz Kiracı raporlama ve analiz.
- Bir kiracı zaman içinde önceki bir noktaya geri yükleme.
- Olağanüstü durum kurtarma.



## <a name="next-steps"></a>Sonraki adımlar

- [Bir SQL veritabanı çok kiracılı SaaS uygulama örneği giriş] [ saas-dbpertenant-wingtip-app-overview-15d] -bu modeli hakkında daha fazla bilgi yer almaktadır.

- [Çok kiracılı SaaS veritabanı kiralama desenleri][multi-tenant-saas-database-tenancy-patterns-60p]



<!-- Image references. -->

[standalone-app-model-62s]: media/saas-tenancy-welcome-wingtip-tickets-app/model-standalone-app.png "Tek başına uygulama modeli"

[database-per-tenant-model-35d]: media/saas-tenancy-welcome-wingtip-tickets-app/model-database-per-tenant.png "Her Kiracı model veritabanı"

[sharded-multitenantdb-model-hybrid-79m]: media/saas-tenancy-welcome-wingtip-tickets-app/model-sharded-multitenantdb-hybrid.png "Parçalı çok Kiracı veritabanı modeli, karma"



<!-- Article references. -->

[saas-dbpertenant-wingtip-app-overview-15d]: saas-dbpertenant-wingtip-app-overview.md

[multi-tenant-saas-database-tenancy-patterns-60p]: saas-tenancy-app-design-patterns.md

