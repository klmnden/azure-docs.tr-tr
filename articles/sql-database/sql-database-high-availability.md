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
ms.author: sashan
ms.reviewer: carlrab, sashan
manager: craigg
ms.date: 04/17/2019
ms.openlocfilehash: ec9f5aa8163ea9bb838b1a95ab8ad49233a72643
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59698238"
---
# <a name="high-availability-and-azure-sql-database"></a>Yüksek kullanılabilirlik ve Azure SQL veritabanı

Azure SQL veritabanı'nda yüksek kullanılabilirlik mimarisi, veritabanınızı garantisi ve çalışan % 99,99 oranında süre için bakım işlemleri ve kesintileri etkisini hakkında endişelenmeden hedefidir. Azure, otomatik olarak düzeltme eki uygulama, yedekleme, Windows ve SQL yükseltmeleri hem de temel alınan donanım, yazılım veya ağ hataları gibi planlanmamış olaylar gibi kritik bakım görevlerini işler.  Temel alınan SQL örneği yama veya üzerinden başarısız olduğunda, kapalı kalma süresi belirgin değil ise, [yeniden deneme mantığı uyguluyor](sql-database-develop-overview.md#resiliency) uygulamanızda. Azure SQL veritabanı, verilerinizi her zaman kullanılabilir olduğundan emin olmanın hatta en kritik durumlarda hızla kurtarabilirsiniz.

Yüksek oranda kullanılabilirlik çözümü kaydedilmiş veri hiçbir zaman iş yükünüz, bakım işlemleri etkilemez hataları nedeniyle, kayıp olduğundan ve veritabanının tek bir yazılım mimarinizdeki hata noktası olmayacaktır emin olmak için tasarlanmıştır. Bakım pencereleri veya kapalı kalma süreleri, veritabanı yükseltme ya da korunan iş yükü durdurmak gereksinim duymanız yoktur. 

Azure SQL veritabanı'nda kullanılan iki yüksek kullanılabilirlik Mimari modeli vardır:

- İşlem ve depolama ayrılması tabanlı standart kullanılabilirlik modeli.  Bu, yüksek kullanılabilirlik ve güvenilirlik Uzak Depolama katmanının kullanır. Bu mimari, bakım etkinlikleri sırasında bazı performans düşüşü tolere edebilen bütçeye yönelik iş uygulamaları hedefler.
- Veritabanı altyapısı işlemlerin bir kümeye bağlı premium kullanılabilirlik modeli. Bu olduğundan her zaman kullanılabilir veritabanı altyapısı düğüm çekirdeği bir olgu üzerinde kullanır. Bu mimari yüksek g/ç performans, yüksek işlem oranına ve Garantisi ile görev açısından kritik uygulamaları hedefleyen bakım etkinlikleri sırasında iş yükünüz için çok az performans etkisi.

Azure SQL veritabanı, SQL Server veritabanı altyapısı ve Windows işletim sistemi en son kararlı sürümü üzerinde çalışan ve kullanıcıların çoğu yükseltmeleri sürekli olarak gerçekleştirildiğinden emin dikkat edin değil.

## <a name="basic-standard-and-general-purpose-service-tier-availability"></a>Temel, standart ve genel amaçlı hizmet katmanı kullanılabilirliği

Bu hizmet katmanları, standart kullanılabilirlik mimarisinden yararlanır. Ayrılmış işlem ve depolama katmanları ile dört farklı düğümlerde aşağıdaki şekilde gösterilmiştir.

![İşlem ve depolama ayrımı](media/sql-database-high-availability/general-purpose-service-tier.png)

Kullanılabilirlik standart modele iki katman içerir:

- Çalışan durum bilgisi olmayan bilgi işlem katmanını `sqlserver.exe` işlemek ve TempDB, model veritabanı, planı önbellek, arabellek havuzu ve sütun havuz deposu gibi ekli SSD yalnızca geçici ve önbelleğe alınmış verileri içerir. Bu durum bilgisi olmayan düğüm başlatır Azure Service Fabric tarafından işletilen `sqlserver.exe`, düğümün sistem durumu denetimleri ve gerekirse, başka bir düğüme yük devretme gerçekleştirir.
- Bir durum bilgisi olan veri katmanı ile Azure Blob depolamada depolanan veritabanı dosyalarını (.mdf/.ldf). Azure blob depolama, yerleşik veri kullanılabilirlik ve yedeklilik özelliği vardır. SQL Server işlem çökse bile günlük dosyası veya sayfa veri dosyasındaki her bir kaydı korunur güvence altına alır.

Veritabanı altyapısı veya işletim sistemi yükseltme ya da bir arıza tespit olduğunda, Azure Service Fabric durum bilgisi olmayan SQL Server işlemi ücretsiz yeterli kapasiteye sahip başka bir durum bilgisi olmayan bir işlem düğümüne yüklenecek taşınır. Azure Blob storage'da veri taşıma tarafından etkilenmez ve verileri/günlük dosyalarını yeni oluşturulmuş SQL Server işleme bağlı. Bu işlem, % 99,99 oranında kullanılabilirlik garanti eder, ancak yeni SQL Server örneği ile soğuk önbellek başlatılmasından bu yana ağır bir iş yükü geçişi sırasında bazı performans düşüşü karşılaşabilirsiniz.

## <a name="premium-and-business-critical-service-tier-availability"></a>Premium ve iş açısından kritik hizmet katmanı kullanılabilirliği

Premium ve iş açısından kritik hizmet katmanları yararlanarak tümleşen Premium kullanılabilirlik modeli bilgi işlem kaynakları (SQL Server veritabanı altyapısı işlem) ve tek bir düğüm üzerinde (yerel olarak bağlı SSD) depolama. Yüksek kullanılabilirlik, işlem ve depolama için ek düğümler üç için dört - düğüm kümesi oluşturma çoğaltılması yoluyla elde edilir. 

![Veritabanı altyapısı düğüm kümesi](media/sql-database-high-availability/business-critical-service-tier.png)

Temel alınan veritabanı dosyalarını (.mdf/.ldf), düşük gecikmeli GÇ iş yükünüz için sağlamak için eklenen SSD depolama alanına yerleştirilir. Yüksek kullanılabilirlik için SQL Server benzer bir teknolojiyi kullanarak gerçekleştirilen [Always On kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). Küme üç ikincil çoğaltmaları (işlem ve depolama) ve okuma-yazma müşteri iş yükleri için erişilebilir olan tek bir birincil çoğaltma (SQL Server işlem) içerir, verilerin kopyalarını içeren. Birincil düğüm, sürekli olarak ikincil düğümlere sırayla değişiklikleri gönderir ve verilerin en az bir ikincil çoğaltma için her bir işlem yapmadan önce eşitlenmesi sağlanır. Bu işlem, herhangi bir nedenle birincil düğüm kilitlenmesi durumunda olduğunu her zaman yük devretme için tam bir eşitleme bir düğüm garanti eder. Yük devretme, Azure Service Fabric tarafından başlatılır. İkincil çoğaltma yeni birincil düğüm olduktan sonra küme yeterli düğümleri (çekirdek kümesi) sahip olmak için başka bir ikincil çoğaltma oluşturulur. Yük devretme işlemi tamamlandıktan sonra SQL bağlantılar yeni birincil düğüme otomatik olarak yönlendirilir.

Ek bir avantaj olarak premium kullanılabilirlik model salt okunur SQL bağlantıları ikincil çoğaltmalardan birine yeniden yönlendirme özelliğini içerir. Bu özelliğin adı [okuma ölçeği genişletme](sql-database-read-scale-out.md). Bu, ek %100 birincil çoğaltmadan analiz iş yükleri gibi salt okunur işlemler yükünü azaltmak için ek ücret ödemeden kapasite işlem sağlar.

## <a name="zone-redundant-configuration"></a>Bölge yedekli yapılandırması

Varsayılan olarak, küme düğümlerinin premium kullanılabilirlik modeli için aynı veri merkezinde oluşturulur. Sunulmasıyla birlikte [Azure kullanılabilirlik alanları](../availability-zones/az-overview.md), SQL veritabanı, aynı bölgedeki farklı kullanılabilirlik bölgelerine kümedeki farklı kopyaya yerleştirebileceğiniz. Bir tek hata noktası ortadan kaldırmak için Denetim halka ayrıca birden çok bölge arasında üç ağ geçidi halkaları (GW) çoğaltılır. Belirli bir ağ geçidi kademesine yönlendirme tarafından denetlenen [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM). Bölge yedekli yapılandırmayı Premium veya iş açısından kritik hizmet katmanlarına ek veritabanı artıklığı oluşturmadığından olmadan etkinleştirebilirsiniz ek bir maliyet. Bölge yedekli Yapılandırması'nı seçerek, Premium veya iş açısından kritik veritabanlarınızı dayanıklı uygulama mantığı herhangi bir değişiklik yapmadan geri dönülemez veri merkezi kesintilerine hataları gibi daha büyük bir dizi yapabilirsiniz. Mevcut tüm Premium veya iş açısından kritik veritabanı veya havuzlar için bölge yedekli yapılandırması dönüştürebilirsiniz.

Bölge yedekli veritabanları, aralarındaki bazı uzaklığı ile farklı veri merkezlerinde çoğaltmaları olduğundan, artan ağ gecikme süresi işleme süresini artırabilir ve böylece bazı OLTP iş yüklerinin performansını etkileyebilir. Her zaman dilimi yedeklilik devre dışı bırakarak tek bölgeli yapılandırmaya geri dönebilirsiniz. Çevrimiçi bir işlem normal bir hizmet katmanı yükseltme benzer işlemidir. İşlemin sonunda, veritabanı veya havuz bir bölge yedekli halka dışında bir tek bir bölge halkası ya da tam tersi geçirilir.

> [!IMPORTANT]
> Bölge yedekli veritabanları ve elastik havuzlar şu anda yalnızca Premium ve iş açısından kritik hizmet katmanlarında desteklenir. Varsayılan, yedeklemeleri ve denetim kayıtları, RA-GRS depolama alanında depolanır ve bu nedenle bir bölge çapında kesinti olması durumunda otomatik olarak kullanılabilir olmayabilir. 

Bölge yedekli sürümü, yüksek kullanılabilirlik mimarisi ile Aşağıdaki diyagramda gösterilmiştir:

![yüksek kullanılabilirlik mimarisi bölgesel olarak yedekli](./media/sql-database-high-availability/zone-redundant-business-critical-service-tier.png)

## <a name="accelerated-database-recovery-adr"></a>Hızlandırılmış veritabanı kurtarma (ADR)

[Veritabanı kurtarma (ADR) hızlandırılmış](sql-database-accelerated-database-recovery.md) işlemleri çalıştıran özellikle uzun olduğu durumda, veritabanı kullanılabilirlik büyük ölçüde geliştiren yeni bir SQL veritabanı altyapısı özelliği. ADR, tek veritabanları, elastik havuzlar ve Azure SQL veri ambarı şu anda kullanılabilir.

## <a name="conclusion"></a>Sonuç

Azure SQL veritabanı, Azure platformuyla tamamen tümleştirilmiş bir yerleşik yüksek kullanılabilirlik çözümü sunar. Service Fabric için hata algılama ve kurtarma, veri koruma için Azure Blob Depolama ve kullanılabilirlik için daha yüksek hata toleransı bağlıdır. Ayrıca, Azure SQL veritabanı, çoğaltma ve yük devretme için SQL Server Always On kullanılabilirlik grubu teknolojisinden yararlanır. Bu teknolojilerin birlikte tam olarak bir karma depolamanın avantajları modelini ve En zorlu SLA'ları destekleyen hayata geçirmek uygulamaları etkinleştirir.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Azure kullanılabilirlik alanları](../availability-zones/az-overview.md)
- Hakkında bilgi edinin [Service Fabric](../service-fabric/service-fabric-overview.md)
- Hakkında bilgi edinin [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)
- Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için daha fazla seçenek için bkz [iş sürekliliği](sql-database-business-continuity.md)
