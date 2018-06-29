---
title: Yüksek kullanılabilirlik - Azure SQL veritabanı hizmetinin | Microsoft Docs
description: Azure SQL Database hizmeti yüksek kullanılabilirlik yetenekleri ve özellikleri hakkında bilgi edinin
services: sql-database
author: jovanpop-msft
manager: craigg
ms.service: sql-database
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: jovanpop
ms.reviewer: carlrab, sashan
ms.openlocfilehash: a9874681d59d193fc3c3d0fd4271e2a6a0fb0dc6
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060393"
---
# <a name="high-availability-and-azure-sql-database"></a>Yüksek kullanılabilirlik ve Azure SQL veritabanı

Azure SQL veritabanı yüksek oranda kullanılabilir bir veritabanınız çalışır durumda olduğunu garanti hizmet ve süresinin, Bakım ve kapalı kalma süreleri hakkında kaygısı olmadan çalışan % 99,99 olarak Platform veritabanıdır. SQL Server veritabanınız her zaman, iş yükünü etkilemeden yükseltilmiş/düzeltme eki sağlanır Azure bulutta barındırılan tam olarak yönetilen bir SQL Server veritabanı altyapısı işlem budur. Azure SQL veritabanı, verileriniz her zaman kullanılabilir olduğundan emin olmanın hatta en kritik durumlarda hızla kurtarabilirsiniz.

Azure platformu tam olarak her Azure SQL veritabanı yönetir ve veri kaybı ve veri kullanılabilirliği yüksek bir yüzdesini güvence altına alır. Azure otomatik olarak düzeltme eki uygulama, yedeklemeler, çoğaltma, hata algılama, temel alınan olası donanım, yazılım veya ağ hataları, dağıtma hata düzeltmeleri, yük devretme, veritabanı yükseltme ve diğer bakım görevlerini işler. SQL Server mühendisleri tüm bakım işlemleri, veritabanı ömrü %0,01 saatten daha az tamamlandığından emin olduktan en çok bilinen yöntemler uyguladık. Bu mimari, kaydedilmiş veri hiç kayıp olduğundan ve bakım işlemleri iş yükünü etkilemeden gerçekleştirildiğinden emin olmak için tasarlanmıştır. Bakım pencereleri veya veritabanı yükseltme ya da korunan iş yükü durdurmanızı ihtiyaç duyması durumunda kapalı kalma süreleri yok. Azure SQL veritabanında yerleşik yüksek kullanılabilirlik veritabanının hiçbir zaman tek yazılım Mimarinizi hata noktası olmaz güvence altına alır.

Azure SQL uygulanan iki yüksek kullanılabilirlik modeli vardır:

- % 99,99 kullanılabilirlik ancak bazı olası performans düşüşü bakım etkinlikleri sırasında sağlayan standart/genel amaçlı modeli.
- Sağlar premium/iş kritik modeli, hatta bakım etkinlikleri sırasında iş yüküne en düşük performans etkisi olan % 99,99 kullanılabilirlik de sağlar.

Azure yükseltir ve son kullanıcılar için en az kapalı kalma süresi ile temel işletim sistemi, sürücüler ve SQL Server Veritabanı Altyapısı'nın saydam yamaları. Azure SQL veritabanı en son kararlı sürümü SQL Server veritabanı motoru ve Windows işletim sistemi üzerinde çalışır ve kullanıcıların çoğunun yükseltmeleri sürekli olarak gerçekleştirilen fark etmesi değil.

## <a name="standard-availability"></a>Standart kullanılabilirliği

Standart kullanılabilirlik Basic/standart/genel amaçlı katmanında uygulanır % 99,99 SLA ifade eder. Kullanılabilirlik işlem ve depolama katmanları ayrılması tarafından sağlanır. Standart kullanılabilirlik modelinde iki katmanı vardır:

- Sqlserver.exe işlemi çalışmıyor ve yalnızca geçici ve önbelleğe alınmış verileri (örneğin – plan önbelleğinin, arabellek havuzu, sütun deposu havuzu) içeren bir durum bilgisiz işlem katmanı. Bu durum bilgisi olmayan SQL Server düğümü Azure Service Fabric tarafından işlemi başlatır, düğüm durumunu denetler ve gerekirse, başka bir yere yük devretme gerçekleştiren çalıştırılır.
- Azure Premium depolama diskleri depolanan veritabanı dosyaları (.mdf/.ldf) ile bir durum bilgisi olan veri katmanı. Azure depolama herhangi bir veritabanı dosyada yerleştirilen herhangi bir kayıt hiçbir veri kaybı olacaktır güvence altına alır. Azure depolama her kayıt günlük dosyasında veya sayfası veri dosyasındaki SQL Server işlemi bozulsa korunacak sağlar yerleşik veri kullanılabilirliği/artıklık sahiptir.

Her veritabanı motoru veya işletim sistemi yükseltme veya Sql Server işleminde bazı önemli sorun algılanırsa, Azure Service Fabric başka bir durum bilgisiz işlem düğümüne durum bilgisi olmayan SQL Server işlemi taşıyın. Azure depolama katmanı verilerde etkilenmez ve veri/günlük dosyalarını yeni başlatılmış SQL Server işlemine bağlı. Beklenen yük devretme zamanı saniye cinsinden ölçülen. Bu işlem % 99,99 kullanılabilirlik garanti eder ancak geçiş süresi nedeniyle çalışmakta olan bazı performans etkileri ağır iş yükü üzerinde olabilir ve yeni SQL Server düğümü olgu soğuk önbelleği ile başlatır.

## <a name="premium-availability"></a>Premium kullanılabilirliği

Premium kullanılabilirlik Azure SQL veritabanı Premium katmanındaki etkindir ve devam eden bakım işlemleri nedeniyle bir performans etkisi genişliğinin kullanılmasını yoğun iş yükleri için tasarlanmıştır.

Premium modelinde, Azure SQL veritabanı, işlem ve depolama tek düğümde tümleştirir. SQL Server veritabanı altyapısı işlem ve mdf/ldf dosyalarını temel alınan aynı düğümde, iş yükü için düşük gecikmeli sağlayarak yerel olarak bağlı SSD depolama ile yerleştirilir.

Yüksek kullanılabilirlik, standart kullanılarak gerçekleştirilir [Always On kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). Her veritabanı, müşteri iş yükü için erişilebilir olan bir birincil veritabanı ve veri kopyalarını içeren birkaç ikincil işlemleri ile veritabanı düğümlerinin bir kümedir. Birincil düğüm ikincil düğümlerine değişiklikleri birincil düğüm herhangi bir nedenle çökerse verileri ikincil çoğaltmaların üzerine kullanılabilir olduğundan emin olmak için sürekli iter. Yük devretme SQL Server veritabanı altyapısı tarafından işlenir – bir ikincil çoğaltma birincil düğüm olur ve yeni bir ikincil çoğaltma yeterli düğümleri emin olmak için oluşturulur. İş yükünü otomatik olarak yeni bir birincil düğüme yönlendirilir. Yük devretme zamanı milisaniye cinsinden ölçülür ve yeni birincil örnek isteklere hizmet vermek devam etmek hemen hazır.

## <a name="zone-redundant-configuration-preview"></a>Bölge olarak yedekli yapılandırması (Önizleme)

Varsayılan olarak, yerel depolama yapılandırmaları için çekirdek kümesi çoğaltmalar aynı veri merkezinde oluşturulur. Girişiyle [Azure kullanılabilirlik bölgeleri](../availability-zones/az-overview.md), farklı bir kullanılabilirlik bölgeleri aynı bölgede çekirdek kümeleri farklı çoğaltmaları koyun seçeneğine sahipsiniz. Tek bir hata noktası ortadan kaldırmak için Denetim halkası ayrıca birden çok dilimlerinde üç ağ geçidi çalma (GW) çoğaltılır. Belirli ağ geçidi halka için yönlendirme tarafından denetlenen [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM). Bölge olarak yedekli yapılandırması ek veritabanı artıklığı, Premium kullanılabilirlik bölgelerinde kullanımını oluşturmaz veya iş kritik (Önizleme) hizmet katmanları hiçbir ek kullanılabilir olduğundan maliyeti. Bölge olarak yedekli veritabanını seçerek, Premium yapabilir veya iş kritik (Önizleme) uygulama mantığının herhangi bir değişiklik yapılmadan geri dönülemez veri merkezi kesintilerini dahil hataları, çok daha büyük bir dizi esnek veritabanları. Bölge olarak yedekli yapılandırması da mevcut Premium ya da iş kritik veritabanları veya havuzları (Önizleme) dönüştürebilirsiniz.

Bölge olarak yedekli çekirdek kümesi aralarında bazı uzaklıklı farklı veri merkezlerinde çoğaltmaları olduğundan, artan ağ gecikmesi yürütme süresini artırabilir ve bu nedenle bazı OLTP iş yüklerinin performansını etkileyebilir. Her zaman dilimi artıklık ayarını devre dışı bırakarak tek bölge yapılandırmaya geri dönebilirsiniz. Bu işlem veri işlemi bir boyuta sahiptir ve normal hizmet düzeyi hedefi (SLO) güncelleştirme benzer. İşlemin sonunda, veritabanı veya havuzu bir bölge olarak yedekli halka dışında tek bir bölge halka (veya tersi) geçirilir.

> [!IMPORTANT]
> Bölge olarak yedekli veritabanları ve esnek havuzlar olan şu anda yalnızca Premium hizmet katmanında desteklenir. Genel Önizleme, yedeklemeler ve Denetim sırasında kayıtları RA-GRS depolama alanında depolanır ve bu nedenle bir bölge genelinde kesinti durumunda otomatik olarak kullanılamayabilir. 

Yüksek kullanılabilirlik mimarisi bölge olarak yedekli sürümü tarafından Aşağıdaki diyagramda gösterilmiştir:
 
![yüksek kullanılabilirlik mimarisi bölge olarak yedekli](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="read-scale-out"></a>Genişleme okuma
Açıklandığı gibi Premium ve iş kritik (Önizleme) katmanları Dengeleme çekirdek-ayarlar ve her zaman açık teknoloji hem de tek bir bölge ve bölge olarak yedekli yapılandırmalarını yüksek kullanılabilirlik için hizmet. AlwaysOn yararları çoğaltmaların her zaman işlemsel olarak tutarlı durumda olduğundan biridir. Çoğaltmaları birincil olarak aynı performans düzeyine sahip olduğundan, uygulama, salt okunur iş yüklerini hiçbir ek de bakım bu ek kapasite yararlanabilir maliyeti (okuma genişleme). Bu şekilde salt okunur sorguları ana okuma-yazma yükünden yalıtılması ve kendi performansını etkilemez. Ölçeklendirme özelliği kullanılmaya okuma mantıksal olarak içeren uygulamalar için salt okunur iş yükleri çözümlemeleri gibi ayrılmış ve bu nedenle bu ek kapasite için birincil bağlanmadan yararlanan. 

Belirli bir veritabanıyla okuma genişleme özelliği kullanmak için açıkça veritabanı oluşturulurken veya daha sonra çağırarak PowerShell kullanarak yapılandırmasını değiştirmeyi etkinleştirmeniz gerekir [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) veya [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlet'leri kullanarak Azure Resource Manager REST API'si aracılığıyla veya [veritabanları - oluştur veya Güncelleştir](/rest/api/sql/databases/createorupdate) yöntemi.

Bir veritabanı için okuma genişleme etkinleştirildikten sonra o veritabanına bağlanan uygulamaları okuma-yazma çoğaltmaya veya salt okunur çoğaltmaya göre veritabanının yönlendirilirsiniz `ApplicationIntent` uygulamanın içinde yapılandırılmış özelliği bağlantı dizesi. Hakkında bilgi için `ApplicationIntent` özelliği, bkz: [belirtme uygulama amacı](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent). 

Okuma genişleme devre dışıysa veya desteklenmeyen hizmet katmanında ReadScale özelliği ayarlarsanız okuma-yazma çoğaltmaya, bağımsız olarak tüm bağlantılar yönlendirilir `ApplicationIntent` özelliği.  

> [!NOTE]
> Salt okunur yönlendirmeye oluşturmayacaktır olsa bile bir standart ya da bir genel amaçlı veritabanı üzerinde okuma ölçek genişletme etkinleştirmek mümkündür yönelik ayrı bir çoğaltma oturumu. Bu, standart/genel amacı ve Premium/iş kritik katmanlar arasında yukarı ve aşağı doğru ölçeklendirme mevcut uygulamaları desteklemek için gerçekleştirilir.  

Okuma genişleme özelliği oturum düzeyinde tutarlılık destekler. Bağlantı hatası neden sonra çoğaltma kullanılabilir olmamasından salt okunur oturum bağlanırsa, farklı bir kopyasına yönlendirilebilir. Olası olsa da, eski veri kümesinin işleme'de neden olabilir. Bir uygulama bir okuma-yazma oturumu kullanarak verileri yazar ve hemen salt okunur oturumu kullanarak okur, benzer şekilde, yeni veriler hemen görünür değil mümkündür.

## <a name="conclusion"></a>Sonuç
Azure SQL veritabanı ile Azure platformu son derece tümleşiktir ve Service Fabric yüksek oranda hata algılama ve veri koruma için Azure Storage Bloblarında ve daha yüksek hata toleransı için kullanılabilirlik bölgeleri kurtarma bağlıdır. Aynı anda Azure SQL veritabanı tam SQL Server kutusunu ürünün çoğaltma ve yük devretme için her zaman üzerindeki kullanılabilirlik grubu teknolojisini kullanır. Bu teknolojiler birleşimi tam olarak bir karma depolama modelinin faydaları hayata geçirmek ve zorlu SLA desteklemek uygulamaların sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Azure kullanılabilirlik bölgeleri](../availability-zones/az-overview.md)
- Hakkında bilgi edinin [Service Fabric](../service-fabric/service-fabric-overview.md)
- Hakkında bilgi edinin [Azure trafik Yöneticisi](../traffic-manager/traffic-manager-overview.md) 
