---
title: Yüksek kullanılabilirlik - Azure SQL veritabanı hizmeti | Microsoft Docs
description: Azure SQL veritabanı hizmet yüksek kullanılabilirlik yetenekleri ve özellikleri hakkında bilgi edinin
services: sql-database
author: jovanpop-msft
manager: craigg
ms.service: sql-database
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: jovanpop
ms.reviewer: carlrab, sashan
ms.openlocfilehash: 1aab8dfd3a4bcc33cddb71dec08157ee7eb68f8d
ms.sourcegitcommit: 465ae78cc22eeafb5dfafe4da4b8b2138daf5082
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44324657"
---
# <a name="high-availability-and-azure-sql-database"></a>Yüksek kullanılabilirlik ve Azure SQL veritabanı

Azure SQL veritabanı yüksek oranda kullanılabilir platformu süresinin, Bakım ve kapalı kalma süreleri hakkında endişelenmenize gerek kalmadan çalışan % 99,99 oranında ve veritabanınız çalışır durumda olduğunu garanti eden bir hizmet olarak veritabanıdır. SQL Server veritabanınıza her zaman iş yükünüzü etkilemeden yükseltilmiş ve yama olmasını sağlar Azure bulutunda barındırılan tam olarak yönetilen bir SQL Server veritabanı altyapısı işlem budur. Bir örneği yama veya üzerinden başarısız olduğunda, kapalı kalma süresi genellikle gözümüzün değil ise, [yeniden deneme mantığı uyguluyor](sql-database-develop-overview.md#resiliency) uygulamanızda. Bir yük devretmeyi tamamlamak için 60 saniyeden uzun süredir, bir destek talebi açmanız gerekir. Azure SQL veritabanı, verilerinizi her zaman kullanılabilir olduğundan emin olmanın hatta en kritik durumlarda hızla kurtarabilirsiniz.

Azure platformu, tam olarak yöneten her bir Azure SQL veritabanı ve veri kaybı olmadan ve yüksek miktarda veri kullanılabilirliği garanti eder. Azure, düzeltme eki uygulama, yedekleme, çoğaltma, hata algılama, temel alınan olası donanım, yazılım veya ağ hatası, dağıtımı hata düzeltmeleri, yük devretmeleri, veritabanı yükseltme işlemleri ve diğer bakım görevleri otomatik olarak işler. SQL Server mühendisleri insanın yöntemler, tüm bakım işlemleri veritabanına hayatındaki %0,01 saatten daha az tamamlandığından emin olduktan uyguladınız. Bu mimari, kaydedilmiş veri hiçbir zaman kayıp olduğundan ve bakım işlemlerini iş yükünü etkilemeden gerçekleştirildiğinden emin olmak için tasarlanmıştır. Bakım pencereleri veya kapalı kalma süreleri, veritabanı yükseltme ya da korunan iş yükü durdurmak gereksinim duymanız yoktur. Azure SQL veritabanında yerleşik yüksek kullanılabilirlik veritabanı hiçbir zaman tek yazılım mimarinizdeki hata noktası olmaz garanti eder.

Azure SQL veritabanı altyapı hataları durumda bile % 99,99 kullanılabilirlik sağlamak üzere bulut ortamı için ayarlanmış bir SQL Server veritabanı altyapısı mimarisini temel alır. Azure SQL veritabanı'nda kullanılan iki yüksek kullanılabilirlik Mimari modeli vardır (her ikisi de % 99,99 kullanılabilirlik sağlama):
- İşlem ve depolama ayrılması tabanlı standart/genel amaçlı modeli. Yüksek kullanılabilirlik ve güvenilirlik depolama katmanının bu mimari bir model kullanır, ancak bazı olası performans düşüşü bakım etkinlikleri sırasında olabilir.
- Veritabanı altyapısı işlemlerin bir kümeye bağlı premium/iş kritik modeli. Bu mimari bir model, her zaman bir kullanılabilir veritabanı altyapısı düğüm Çekirdeği ve en az düzeyde performans etkisi bile bakım etkinlikleri sırasında iş yüküne sahip bir olgu kullanır.

Azure, yükseltir ve son kullanıcılar için en düşük kapalı kalma süresi ile temel işletim sistemi, sürücüler ve SQL Server Veritabanı Altyapısı'nın şeffaf bir şekilde yamaları. Azure SQL veritabanı, SQL Server veritabanı altyapısı ve Windows işletim sistemi en son kararlı sürümü üzerinde çalışan ve kullanıcıların çoğunun yükseltmeleri sürekli olarak gerçekleştirilen dikkat edin değil.

## <a name="standardgeneral-purpose-availability"></a>Standart/genel amaçlı kullanılabilirlik

Standart kullanılabilirlik, temel/standart/genel amaçlı katmanda uygulanan % 99,99 SLA'sı ifade eder. Bu mimari modelinde yüksek kullanılabilirlik ve depolama katmanındaki veriler, çoğaltma işlem ve depolama katmanları ayrılması tarafından sağlanır.

Aşağıdaki şekilde, standart mimari modelinde ayrılmış işlem ve depolama katmanları ile dört düğüm gösterilmektedir.

![İşlem ve depolama ayrımı](media/sql-database-managed-instance/general-purpose-service-tier.png)

Standart kullanılabilirlik modelinde iki katman vardır:

- Sqlserver.exe işlem çalışıyor ve yalnızca geçici ve önbelleğe alınan verileri (örneğin-planı önbellek, arabellek havuzu, sütun depolama havuzu) içeren bir durum bilgisi olmayan bilgi işlem katmanı. Azure Service Fabric tarafından işlem başlatır, düğümün sistem durumu denetimleri ve gerekirse, başka bir yere yük devretme gerçekleştirir, bu durum bilgisi olmayan SQL Server düğümünde çalıştırılır.
- Azure Premium Depolama'da depolanan veritabanı dosyaları (.mdf/.ldf) ile bir durum bilgisi olan veri katmanı. Azure depolama, veri kaybı olmadan bir veritabanı dosyasına yerleştirilir herhangi bir kayıt olacaktır garanti eder. Azure depolama, SQL Server işlem çökse bile her kayıt günlük dosyasında veya sayfa veri dosyasında korunur sağlar yerleşik veri kullanılabilirlik/yedeklilik sahiptir.

Veritabanı altyapısı veya işletim sistemi yükseltme olduğunda, bazı altyapının parçası başarısız olursa veya Sql Server işleminde bazı önemli sorun algılanırsa, başka bir durum bilgisi olmayan bir işlem düğümüne yüklenecek Azure Service Fabric durum bilgisi olmayan SQL Server işlemi taşınır. Yük devretme süresini en aza indirmek için yük devretme durumunda yeni işlem hizmetini çalıştırmak için bekleyen bir dizi düğümü yedek yok. Verileri Azure depolama katmanında etkilenmez ve yeni oluşturulmuş SQL Server işlemi için veri/günlük dosyalar eklenir. Bu işlem, % 99,99 oranında kullanılabilirlik garanti eder, ancak bu geçiş süresi nedeniyle çalıştıran ağır iş yükü üzerindeki bazı performans etkileri olabilir ve yeni SQL Server düğümü olgu soğuk önbellek ile başlar.

## <a name="premiumbusiness-critical-availability"></a>Premium/iş açısından kritik kullanılabilirlik

Azure SQL veritabanı Premium katmanında Premium kullanılabilirlik etkinleştirilir ve devam eden bakım işlemleri nedeniyle bir performans etkisi kabul edilemez kullanımlı iş yükleri için tasarlanmıştır.

Premium modelde, Azure SQL veritabanı, işlem ve depolama tek düğümde tümleştirir. Bu mimari modelinde yüksek kullanılabilirlik çoğaltma 4 düğümlü dağıtılan hesaplama (SQL Server veritabanı altyapısı işlem) ve depolama (yerel olarak bağlı SSD) tarafından gerçekleştirilir [Always On kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) kümesi.

![Veritabanı altyapısı düğüm kümesi](media/sql-database-managed-instance/business-critical-service-tier.png)

SQL Server veritabanı altyapısı işlem ve mdf/ldf dosyaları temel alınan aynı düğümde yerel olarak bağlı SSD depolamasında iş yükünüz için düşük gecikme süresi sağlayan yerleştirilir. Yüksek kullanılabilirlik, standart kullanılarak gerçekleştirilir [Always On kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). Her müşterinin iş yükü ve verilerin kopyalarını içeren üç bir ikincil işlemleri için erişilebilir olan bir birincil veritabanı ile veritabanı düğümden oluşan bir kümede veritabanıdır. Birincil düğümden ikincil düğümlerine değişiklikleri herhangi bir nedenle birincil düğüm kilitlenmesi durumunda, veriler ikincil çoğaltmalarda kullanılabilir olmasını sağlamak için sürekli iter. Yük devretme SQL Server veritabanı altyapısı tarafından gerçekleştirilir: bir ikincil çoğaltma birincil düğüm haline gelir ve yeterli düğümleri sağlamak için yeni bir ikincil çoğaltma oluşturulur. İş yükü, yeni birincil düğüme otomatik olarak yönlendirilir.

Ayrıca, iş açısından kritik küme, birincil İş yükünüzün performansını engellememelidir salt okunur sorguları (örneğin, raporları) çalıştırmak için kullanılan yerleşik salt okunur düğümü sağlar. 

## <a name="zone-redundant-configuration-preview"></a>Bölge yedekli yapılandırma (Önizleme)

Varsayılan olarak, yerel depolama yapılandırmaları için çekirdek kümesi çoğaltmalarını aynı veri merkezinde oluşturulur. Sunulmasıyla birlikte [Azure kullanılabilirlik alanları](../availability-zones/az-overview.md), farklı kullanılabilirlik bölgelerine aynı bölgedeki çekirdek kümeleri farklı kopyaya koyun olanağına sahip. Bir tek hata noktası ortadan kaldırmak için Denetim halka ayrıca birden çok bölge arasında üç ağ geçidi halkaları (GW) çoğaltılır. Belirli bir ağ geçidi kademesine yönlendirme tarafından denetlenen [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM). Bölge yedekli yapılandırması ek veritabanı artıklığı oluşturmadığından, kullanılabilirlik alanları (Önizleme) kullanımı, Premium veya iş açısından kritik hizmet katmanlarında kullanılabilir hiçbir ek maliyet. Bölge yedekli veritabanı seçerek, Premium veya iş açısından kritik veritabanlarınızı dayanıklı çok daha büyük bir uygulama mantığının herhangi bir değişiklik yapmadan geri dönülemez veri merkezi kesintilerine hataları gibi yapabilirsiniz. Mevcut tüm Premium veya iş açısından kritik veritabanı veya havuzlar için bölge yedekli yapılandırması dönüştürebilirsiniz.

Bölge yedekli çekirdek kümesi, aralarındaki bazı uzaklığı ile farklı veri merkezlerinde çoğaltmaları olduğundan, artan ağ gecikme süresi işleme süresini artırabilir ve böylece bazı OLTP iş yüklerinin performansını etkileyebilir. Her zaman dilimi yedeklilik devre dışı bırakarak tek bölgeli yapılandırmaya geri dönebilirsiniz. Bu işlem, bir veri işlemi boyutu ve normal hizmet düzeyi hedefi (SLO) güncelleştirme benzer. İşlemin sonunda, veritabanı veya havuz bir bölge yedekli halka dışında bir tek bir bölge halkası ya da tam tersi geçirilir.

> [!IMPORTANT]
> Bölge yedekli veritabanları ve elastik havuzlar şu anda yalnızca Premium hizmet katmanında desteklenmiyor. Genel önizlemesi, yedeklemeleri ve Denetim sırasında kayıtları RA-GRS depolama alanında depolanır ve bu nedenle bir bölge çapında kesinti olması durumunda otomatik olarak kullanılabilir olmayabilir. 

Bölge yedekli sürümü, yüksek kullanılabilirlik mimarisi ile Aşağıdaki diyagramda gösterilmiştir:
 
![yüksek kullanılabilirlik mimarisi bölgesel olarak yedekli](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="read-scale-out"></a>Okuma ölçeği genişletme
Açıklandığı gibi Premium ve iş açısından kritik hizmet katmanlarına yüksek kullanılabilirlik hem tek bir bölge ve bölge yedekli yapılandırmaları için çekirdek-ayarlar ve Always On teknoloji yararlanın. AlwaysOn avantajlarını çoğaltmaların her zaman işlemsel olarak tutarlı bir durumda olduğundan biridir. Çoğaltmaları birincil aynı performans düzeyinde olduğundan, uygulamanın bakım hiçbir ek salt okunur iş yükleri için bu ek kapasite yararlanabilirsiniz maliyeti (okuma ölçeği genişletme). Bu şekilde salt okunur sorguların ana okuma-yazma iş yükünden yalıtılmış ve performansını etkilemez. Ölçeklendirme özelliği yönelik Okuma içeren mantıksal uygulamalar için analytics gibi salt okunur iş yüklerinin ayrılmış ve bu nedenle bu ek kapasite için birincil bağlanmadan yararlanabiliriz. 

Belirli bir veritabanıyla okuma ölçek genişletme özelliğini kullanmak için açıkça veritabanını oluştururken veya daha sonra çağırarak PowerShell kullanarak yapılandırmasını değiştirmeyi etkinleştirmeniz gerekir [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) veya [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlet'leri veya Azure Resource Manager REST API aracılığıyla [veritabanları - oluşturma veya güncelleştirme](/rest/api/sql/databases/createorupdate) yöntemi.

Bir veritabanı için okuma ölçeği genişletme etkinleştirildikten sonra o veritabanına bağlanan uygulamalar okuma-yazma çoğaltmaya veya salt okunur bir çoğaltmaya göre veritabanının yönlendirilirsiniz `ApplicationIntent` uygulamanın yapılandırılmış özelliği bağlantı dizesi. Hakkında bilgi için `ApplicationIntent` özelliğine bakın [belirterek uygulama amacı](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent). 

Okuma ölçeği genişletme devre dışı bırakıldı veya bir desteklenmeyen hizmet katmanında ReadScale özelliğini ayarlamanız, okuma / yazma kopyasına, bağımsız olarak tüm bağlantıları yönlendirilmiş `ApplicationIntent` özelliği.

## <a name="conclusion"></a>Sonuç
Azure SQL veritabanı Azure platformu ile derin şekilde tümleşiktir ve Service Fabric üzerinde yüksek oranda hata algılama ve kurtarma, veri koruma için Azure depolama BLOB'ları ve kullanılabilirlik için daha yüksek hata toleransı için bağlıdır. Azure SQL veritabanı, aynı zamanda, çoğaltma ve yük devretme için SQL Server ürün Always On kullanılabilirlik grubu teknolojisini tam olarak yararlanır. Bu teknolojiler birleşimi uygulamaların tam olarak bir karma depolama modelinin avantajlarından ve En zorlu SLA desteği sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Azure kullanılabilirlik alanları](../availability-zones/az-overview.md)
- Hakkında bilgi edinin [Service Fabric](../service-fabric/service-fabric-overview.md)
- Hakkında bilgi edinin [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) 
