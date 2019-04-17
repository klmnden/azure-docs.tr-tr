---
title: Yüksek kullanılabilirlik - Azure SQL veritabanı hizmeti | Microsoft Docs
description: Azure SQL veritabanı hizmet yüksek kullanılabilirlik yetenekleri ve özellikleri hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab, sashan
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: fd6e383c2631a47daa7bf469c5bec59959453252
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59616858"
---
# <a name="high-availability-and-azure-sql-database"></a>Yüksek kullanılabilirlik ve Azure SQL veritabanı

Azure SQL veritabanı yüksek oranda kullanılabilir platformu süresinin, Bakım ve kapalı kalma süreleri hakkında endişelenmenize gerek kalmadan çalışan % 99,99 oranında ve veritabanınız çalışır durumda olduğunu garanti eden bir hizmet olarak veritabanıdır. SQL Server veritabanınıza her zaman iş yükünüzü etkilemeden yükseltilmiş ve yama olmasını sağlar Azure bulutunda barındırılan tam olarak yönetilen bir SQL Server veritabanı altyapısı işlem budur. Örneği yama veya üzerinden başarısız olduğunda, kapalı kalma süresi genellikle belirgin değil ise, [yeniden deneme mantığı uyguluyor](sql-database-develop-overview.md#resiliency) uygulamanızda. Bir yük devretmeyi tamamlamak için 60 saniyeden uzun süredir, bir destek talebi açmanız gerekir. Azure SQL veritabanı, verilerinizi her zaman kullanılabilir olduğundan emin olmanın hatta en kritik durumlarda hızla kurtarabilirsiniz.

Azure platformu, tam olarak yöneten her bir Azure SQL veritabanı ve veri kaybı olmadan ve yüksek miktarda veri kullanılabilirliği garanti eder. Azure, düzeltme eki uygulama, yedekleme, çoğaltma, hata algılama, temel alınan olası donanım, yazılım veya ağ hatası, dağıtımı hata düzeltmeleri, yük devretmeleri, veritabanı yükseltme işlemleri ve diğer bakım görevleri otomatik olarak işler. SQL Server mühendisleri insanın yöntemler, tüm bakım işlemleri veritabanına hayatındaki %0,01 saatten daha az tamamlandığından emin olduktan uyguladınız. Bu mimari, kaydedilmiş veri hiçbir zaman kayıp olduğundan ve bakım işlemlerini iş yükünü etkilemeden gerçekleştirildiğinden emin olmak için tasarlanmıştır. Bakım pencereleri veya kapalı kalma süreleri, veritabanı yükseltme ya da korunan iş yükü durdurmak gereksinim duymanız yoktur. Azure SQL veritabanında yerleşik yüksek kullanılabilirlik veritabanı hiçbir zaman tek yazılım mimarinizdeki hata noktası olmaz garanti eder.

Azure SQL veritabanı altyapı hataları durumda bile % 99,99 kullanılabilirlik sağlamak üzere bulut ortamı için ayarlanmış bir SQL Server veritabanı altyapısı mimarisini temel alır. Azure SQL veritabanı'nda kullanılan iki yüksek kullanılabilirlik Mimari modeli vardır (her ikisi de % 99,99 kullanılabilirlik sağlama):

- Ayrımı işlem ve depolama bağlı hizmeti katman modeli standart/genel amaçlı. Yüksek kullanılabilirlik ve güvenilirlik depolama katmanının bu mimari bir model kullanır, ancak bazı olası performans düşüşü bakım etkinlikleri sırasında olabilir.
- Veritabanı altyapısı işlemlerin bir kümeye bağlı premium/iş kritik hizmet katman modeli. Bu mimari bir model, her zaman bir kullanılabilir veritabanı altyapısı düğüm Çekirdeği ve en az düzeyde performans etkisi bile bakım etkinlikleri sırasında iş yüküne sahip bir olgu kullanır.

Azure, yükseltir ve son kullanıcılar için en düşük kapalı kalma süresi ile temel işletim sistemi, sürücüler ve SQL Server Veritabanı Altyapısı'nın şeffaf bir şekilde yamaları. Azure SQL veritabanı, SQL Server veritabanı altyapısı ve Windows işletim sistemi en son kararlı sürümü üzerinde çalışan ve kullanıcıların çoğunun yükseltmeleri sürekli olarak gerçekleştirilen dikkat edin değil.

## <a name="basic-standard-and-general-purpose-service-tier-availability"></a>Temel, standart ve genel amaçlı hizmet katmanı kullanılabilirliği

Standart kullanılabilirlik hizmet katmanları temel, standart ve genel amaçlı uygulanan % 99,99 SLA'sı ifade eder. Bu mimari modelinde yüksek kullanılabilirlik ve depolama katmanındaki veriler, çoğaltma işlem ve depolama katmanları ayrılması tarafından sağlanır.

Aşağıdaki şekilde, standart mimari modelinde ayrılmış işlem ve depolama katmanları ile dört düğüm gösterilmektedir.

![İşlem ve depolama ayrımı](media/sql-database-managed-instance/general-purpose-service-tier.png)

Standart kullanılabilirlik modelinde iki katman vardır:

- Çalışmakta olan bir durum bilgisi olmayan bilgi işlem katmanı `sqlserver.exe` işlemek ve yalnızca geçici ve önbelleğe alınan verileri (örneğin-planı önbellek, arabellek havuzu, sütun depolama havuzu) içerir. Azure Service Fabric tarafından işlem başlatır, düğümün sistem durumu denetimleri ve gerekirse, başka bir yere yük devretme gerçekleştirir, bu durum bilgisi olmayan SQL Server düğümünde çalıştırılır.
- Bir durum bilgisi olan veri katmanı ile Azure Blob depolamada depolanan veritabanı dosyaları (.mdf/.ldf). Azure Blob Depolama, veri kaybı olmadan bir veritabanı dosyasına yerleştirilir herhangi bir kayıt olacaktır garanti eder. Azure Blob Depolama, SQL Server işlem çökse bile her kayıt günlük dosyasında veya sayfa veri dosyasında korunur sağlar yerleşik veri kullanılabilirlik/yedeklilik sahiptir.

Veritabanı altyapısı veya işletim sistemi yükseltme olduğunda, bazı altyapının parçası başarısız olursa veya Sql Server işleminde bazı önemli sorun algılanırsa, başka bir durum bilgisi olmayan bir işlem düğümüne yüklenecek Azure Service Fabric durum bilgisi olmayan SQL Server işlemi taşınır. Yük devretme süresini en aza indirmek için yük devretme durumunda yeni işlem hizmetini çalıştırmak için bekleyen bir dizi düğümü yedek yok. Azure Blob depolamadaki verileri etkilenmez ve yeni oluşturulmuş SQL Server işlemi için veri/günlük dosyalar eklenir. Bu işlem, % 99,99 oranında kullanılabilirlik garanti eder, ancak bu geçiş süresi nedeniyle çalıştıran ağır iş yükü üzerindeki bazı performans etkileri olabilir ve yeni SQL Server düğümü olgu soğuk önbellek ile başlar.

## <a name="premium-and-business-critical-service-tier-availability"></a>Premium ve iş açısından kritik hizmet katmanı kullanılabilirliği

Premium kullanılabilirlik, Premium ve iş açısından kritik hizmet katmanlarında Azure SQL veritabanı'nın etkinleştirilmiş ve devam eden bakım işlemleri nedeniyle bir performans etkisi kabul edilemez kullanımlı iş yükleri için tasarlanmıştır.

Premium modelde, Azure SQL veritabanı, işlem ve depolama tek düğümde tümleştirir. Bu mimari modelinde yüksek kullanılabilirlik, çoğaltma işlem (SQL Server veritabanı altyapısı işlem) ve SQL Server'a benzer teknolojisini kullanan 4 düğümlü bir küme olarak dağıtılan depolamanın (yerel olarak bağlı SSD) tarafından gerçekleştirilir [her zaman üzerinde kullanılabilirlik Grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server).

![Veritabanı altyapısı düğüm kümesi](media/sql-database-managed-instance/business-critical-service-tier.png)

Hem SQL veritabanı altyapısı işlem ve arka plandaki mdf/ldf dosyaları aynı düğümde yerel olarak bağlı SSD depolamasında iş yükünüz için düşük gecikme süresi sağlayan yerleştirilir. Yüksek kullanılabilirlik için SQL Server benzer teknolojisi kullanılarak uygulanır [Always On kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). Her müşterinin iş yükü ve verilerin kopyalarını içeren üç bir ikincil işlemleri için erişilebilir olan bir birincil veritabanı ile veritabanı düğümden oluşan bir kümede veritabanıdır. Birincil düğümden ikincil düğümlerine değişiklikleri herhangi bir nedenle birincil düğüm kilitlenmesi durumunda, veriler ikincil çoğaltmalarda kullanılabilir olmasını sağlamak için sürekli iter. Yük devretme, Azure Service Fabric tarafından işlenir: bir ikincil çoğaltma birincil düğüm haline gelir ve yeterli düğümleri sağlamak için yeni bir ikincil çoğaltma oluşturulur. İş yükü, yeni birincil düğüme otomatik olarak yönlendirilir.

Ayrıca, iş açısından kritik küme yerleşiktir [okuma ölçeği genişletme](sql-database-read-scale-out.md) sağlayan ücretsiz, yetenek ücret performans engellememelidir salt okunur sorguları (örneğin, raporları) çalıştırmak için kullanılan yerleşik salt okunur düğüm Birincil iş yükünü.

## <a name="zone-redundant-configuration"></a>Bölge yedekli yapılandırması

Varsayılan olarak, yerel depolama yapılandırmaları için çekirdek kümesi çoğaltmalarını aynı veri merkezinde oluşturulur. Sunulmasıyla birlikte [Azure kullanılabilirlik alanları](../availability-zones/az-overview.md), farklı kullanılabilirlik bölgelerine aynı bölgedeki çekirdek kümeleri farklı kopyaya koyun olanağına sahip. Bir tek hata noktası ortadan kaldırmak için Denetim halka ayrıca birden çok bölge arasında üç ağ geçidi halkaları (GW) çoğaltılır. Belirli bir ağ geçidi kademesine yönlendirme tarafından denetlenen [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM). Bölge yedekli yapılandırması ek veritabanı artıklığı oluşturmadığından, hiçbir ek kullanılabilirlik alanları kullanımını, Premium veya iş açısından kritik hizmet katmanlarında kullanılabilir maliyeti. Bölge yedekli veritabanı seçerek, Premium veya iş açısından kritik veritabanlarınızı dayanıklı çok daha büyük bir uygulama mantığının herhangi bir değişiklik yapmadan geri dönülemez veri merkezi kesintilerine hataları gibi yapabilirsiniz. Mevcut tüm Premium veya iş açısından kritik veritabanı veya havuzlar için bölge yedekli yapılandırması dönüştürebilirsiniz.

Bölge yedekli çekirdek kümesi, aralarındaki bazı uzaklığı ile farklı veri merkezlerinde çoğaltmaları olduğundan, artan ağ gecikme süresi işleme süresini artırabilir ve böylece bazı OLTP iş yüklerinin performansını etkileyebilir. Her zaman dilimi yedeklilik devre dışı bırakarak tek bölgeli yapılandırmaya geri dönebilirsiniz. Bu işlem, bir veri işlemi boyutu ve normal bir hizmet katmanı güncelleştirme benzer. İşlemin sonunda, veritabanı veya havuz bir bölge yedekli halka dışında bir tek bir bölge halkası ya da tam tersi geçirilir.

> [!IMPORTANT]
> Bölge yedekli veritabanları ve elastik havuzlar şu anda yalnızca Premium hizmet katmanında desteklenmiyor. Varsayılan, yedeklemeleri ve denetim kayıtları, RA-GRS depolama alanında depolanır ve bu nedenle bir bölge çapında kesinti olması durumunda otomatik olarak kullanılabilir olmayabilir. 

Bölge yedekli sürümü, yüksek kullanılabilirlik mimarisi ile Aşağıdaki diyagramda gösterilmiştir:

![yüksek kullanılabilirlik mimarisi bölgesel olarak yedekli](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="accelerated-database-recovery-adr"></a>Hızlandırılmış veritabanı kurtarma (ADR)

[Veritabanı kurtarma (ADR) hızlandırılmış](sql-database-accelerated-database-recovery.md) özellikle uzun olduğu durumda, veritabanı kullanılabilirlik büyük ölçüde geliştiren yeni bir SQL veritabanı altyapısı özelliği çalıştıran işlem, SQL veritabanı altyapısı kurtarma işlemini yeniden tasarlanmasını tarafından. ADR, tek veritabanları, elastik havuzlar ve Azure SQL veri ambarı şu anda kullanılabilir.

## <a name="conclusion"></a>Sonuç

Azure SQL veritabanı Azure platformu ile derin şekilde tümleşiktir ve Service Fabric üzerinde yüksek oranda hata algılama ve kurtarma, veri koruma için Azure Blob Depolama ve kullanılabilirlik için daha yüksek hata toleransı için bağlıdır. Azure SQL veritabanı, aynı zamanda, çoğaltma ve yük devretme için SQL Server ürün Always On kullanılabilirlik grubu teknolojisini tam olarak yararlanır. Bu teknolojiler birleşimi uygulamaların tam olarak bir karma depolama modelinin avantajlarından ve En zorlu SLA desteği sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Azure kullanılabilirlik alanları](../availability-zones/az-overview.md)
- Hakkında bilgi edinin [Service Fabric](../service-fabric/service-fabric-overview.md)
- Hakkında bilgi edinin [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)
- Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için daha fazla seçenek için bkz [iş sürekliliği](sql-database-business-continuity.md)
