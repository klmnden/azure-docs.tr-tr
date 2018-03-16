---
title: "Azure SQL veritabanı müşteri uygulamaları teknik | Microsoft Docs"
description: "Teknik Ayrıntılar iş sorunlarını çözmek için Azure SQL veritabanı'nın müşteri implementatons hakkında bilgi edinin"
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: reference
ms.topic: article
ms.date: 03/03/2017
ms.author: carlrab
ms.openlocfilehash: 22f462010f636731cdceed5105e6057d2d45f005
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="azure-sql-database-customer-implementation-technical-studies"></a>Azure SQL veritabanı müşteri uygulaması teknik incelemeleri

- [Daxko](sql-database-implementation-daxko.md): Daxko/CSI yazılım karşılaştığı bir sınama: uygunluk ve dinlenme merkezlerinin müşteri tabanı, kapsamlı Kurumsal yazılım çözümü başarı sayesinde hızlı bir şekilde, büyüyen ancak BT altyapısı ihtiyaçları için tutma Müşteri temel büyüyen şirket test edildi BT personeli kullanıcının. Şirket gittikçe büyüyen veritabanlarını yönetmek için özellikle yükselen işlemlerinin yükünü kısıtlanmış. Da kötüsü, bu işlem yükünü şirketin yazılım yeni mobility özellikleri gibi yeni girişimleri için geliştirme kaynakları içine kesme.

- [GEP](sql-database-implementation-gep.md): GEP, yazılım ve etkilerini, işlerini işlemleri, stratejilerini ve finansal performanslarını en üst düzeye çıkarmak tedarik kılavuzları dünyanın etkinleştirme hizmetleri sunar. Danışmanlık ve yönetilen Hizmetleri yanı sıra, akıllı GEP®, bulut tabanlı ve kapsamlı tedarik yazılım platformu tarafından şirket sunar. Ancak, kendi GEP tarafından akıllı şirket içi veri merkezi gibi çözümleri destekleyecek şekilde çalışırken sınırlamaları GEP çalıştırılır: gerekli Yatırımlar hızı yüksek ve Mevzuat gereklilikleri diğer ülkelerdeki yapmış gerekli Yatırımlar hala göz korkutucu daha fazla. Buluta taşıyarak GEP BT kaynaklarını, daha az BT işlemleri hakkında ve odaklanmak değerinin yeni kaynaklar için müşterilerinin dünya çapında geliştirme hakkında daha fazla endişe izin veren serbest.

- [SnelStart](sql-database-implementation-snelstart.md): SnelStart Hollanda popüler finansal ve iş-yönetimi yazılımı küçük ve orta ölçekli işletmeler (SMB) sağlar. 55,000 müşterilerine 110 çalışanlar, bir BT personeli 35 dahil olmak üzere bir personeli tarafından sunulur. Azure üzerinde oluşturulmuş bir yazılım olarak-hizmet (SaaS) teklifi için Masaüstü yazılımlarını taşıyarak, SnelStart en yerleşik Hizmetleri tanıdık ortamlarında C# kullanarak ve performans ve ölçeklenebilirlik hiçbiri üzerinden - tarafından en iyi duruma getirme Yönetimi otomatikleştirme yapılan veya Esnek havuzları kullanan altında sağlama işletmeler. Azure kullanarak şirket içi ve bulut arasında taşıma müşteriler ortadan SnelStart sağlar.

- [Umbraco](sql-database-implementation-umbraco.md): müşteri dağıtımları basitleştirmek için Umbraco-bir hizmet olarak (UaaS) Umbraco eklendi: şirket içi dağıtımlar için ihtiyacını ortadan kaldıran bir yazılım olarak-hizmet (SaaS) sunumu yerleşik ölçeklendirme sağlar ve yönetimini kaldırır Ürün çözüm yerine yenilik Yönetimi odaklanmaya geliştiriciler etkinleştirerek ek yükü. Umbraco, Microsoft Azure tarafından sunulan esnek hizmet olarak platform (PaaS) modelinde bağlı olarak bu avantajlar sağlayabilir.

- [Çekirdek](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database): çekirdek % 70'sql Database tarafından DTU düşürürken anahtar veritabanının iş yükü Katlar.

- [İsteği](https://customers.microsoft.com/story/quest): rtifika kendi Spotlight bir hedefi aklınıza tutarak ile SQL Server Enterprise hizmeti sunar: veritabanı uzmanları için en iyi aracı veri güvenliğini sağlamak için geçici verileri taşıma ve veritabanı işlemleri izleme vermek için. Microsoft Azure ve Azure SQL veritabanını kullanarak Spotlight ile SQL Server Veritabanı yöneticileri izleyebilir, algılamak, tanılama ve kendi masalarını durduğunu olduğunuz veya evden çalışan SQL Server performans sorunları çözmek için bir yol sağlar.

- [Microsoft Dynamics](https://customers.microsoft.com/story/dynamics365operationsproductteam): en iyi yöntemler ve müşterilere tam olarak yönetilen bir yazılım olarak sunmak için Azure SQL veritabanına geçirme işlemleri ürün ekibinin deneyimi Dynamics 365 gelen öğrenilen dersler kısa Bu örnek olay incelemesi vurgular sunumu bir hizmet (SaaS). Azure SQL veritabanı kullanarak, işletim ekibi Dynamics 365 yönetebilir ve önemli ölçüde daha az personel hizmetiyle çalışır ve otomatik veritabanı yedeklemeyi, veritabanı yedekleme bekletme gibi giden kutusu yönetilebilirlik özellikleri ile kolayca ölçeklendirme , yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri. Bu Önemsiz Otomasyon veritabanlarıyla sağlama yeteneği yanı sıra, Azure SQL veritabanı konumu büyük ölçekli hizmetler kurmak için harika bir platform olmaya ödünç.

- [Microsoft OneDrive ve SharePoint Online](https://customers.microsoft.com/story/microsoft-azure-sql-database-dicrete-manufacturing-united-states): kısa Bu örnek olay incelemesi taşıyın Microsoft OneDrive ve SharePoint Online'nın arkasında Öykü Azure SQL veritabanına bildirir ve bu geçiş neredeyse sınırsız Esnek Kapasite yönetiminin nasıl etkin açıklar Ayrıca önemli ölçüde işlem maliyetleri ve altyapı ek yükünü azaltırken.
