---
title: Yük devretme grupları - Azure SQL veritabanı | Microsoft Docs
description: Otomatik Yük devretme grupları, çoğaltma ve yük devretme otomatik / eşgüdümlü bir grup veritabanı bir SQL veritabanı sunucusunda veya tüm veritabanı yönetilen örneğinde yönetmenize olanak sağlayan bir SQL veritabanı özelliğidir.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 05/06/2019
ms.openlocfilehash: 595d1b84aab55a77f21a9840c5bae9ee996424be
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415925"
---
# <a name="use-auto-failover-groups-to-enable-transparent-and-coordinated-failover-of-multiple-databases"></a>Birden fazla veritabanının saydam ve Eşgüdümlü yük devretmeyi etkinleştirmek için otomatik yük devretme grupları kullanma

Otomatik Yük devretme grupları, çoğaltma ve yük devretme grubunun veritabanı bir SQL veritabanı sunucusunda veya başka bir bölgede bir yönetilen örnek içindeki tüm veritabanlarına yönetmenize olanak sağlayan bir SQL veritabanı özelliğidir. Aynı temel teknoloji olarak kullandığı [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md). Yük devretme el ile başlatabilir veya kullanıcı tanımlı bir ilkesini temel alarak SQL veritabanı hizmeti için temsilci. İkinci seçeneği, otomatik olarak geri dönülemez bir arıza ya da birincil bölgedeki SQL veritabanı hizmetinizin kullanılabilirliğini tam veya kısmi kaybı ile sonuçlanır diğer planlanmamış bir olay sonra ikincil bir bölgede birden çok ilişkili veritabanlarını kurtarmanıza olanak tanır. Ayrıca, okunabilir ikincil veritabanı salt okunur sorgu iş yükleri yük boşaltması için kullanabilirsiniz. Otomatik Yük devretme grupları, birden çok veritabanı içerdiğinden, bu veritabanları birincil sunucuda yapılandırılması gerekir. Yük devretme grubundaki veritabanları için birincil ve ikincil sunucular, aynı abonelikte olmalıdır. Otomatik Yük devretme grupları için farklı bir bölgedeki tek bir ikincil sunucu grubundaki tüm veritabanlarının çoğaltma destekler.

> [!NOTE]
> SQL veritabanı sunucusu ve tek veya havuza alınmış veritabanlarıyla çalışmayı aynı veya farklı bölgelerde birden fazla ikincil veritabanı istediğinizde kullanın [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md).

Ne zaman birinde veya birkaçında otomatik yük devretme grubu sonuçlarında veritabanlarında etkileyen herhangi bir kesinti, otomatik yük devretme İlkesi ile birlikte otomatik yük devretme grupları kullanmıyorsunuz demektir. Ayrıca, otomatik yük devretme grupları okuma-yazma sağlar ve kalan salt okuma dinleyici uç noktalarının yük devretme sırasında açısından bir farklılık göstermez. El ile veya otomatik yük devretme etkinleştirme kullanmanıza bakılmaksızın, yük devretme grubunda tüm ikincil veritabanlarını birincil geçer. Veritabanı yük devretme tamamlandıktan sonra yeni bölge için Uç noktalara yönlendirmek için DNS kaydını otomatik olarak güncelleştirilir. Belirli RPO ve RTO verileri için bkz: [iş Sürekliliğine genel bakış](sql-database-business-continuity.md).

Otomatik Yük devretme grupları, SQL veritabanı sunucusundaki veritabanlarının etkiler veya otomatik yük devretme örneği sonuçları yönetilen herhangi bir kesinti, otomatik yük devretme İlkesi ile birlikte kullandığınızda. Otomatik Yük devretme grubu kullanarak yönetebilirsiniz:

- [Azure portalı](sql-database-implement-geo-distributed-database.md)
- [PowerShell: Yük devretme grubu](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- [REST API: Yük devretme grubu](https://docs.microsoft.com/rest/api/sql/failovergroups).

Yük devretme işleminden sonra yeni birincil sunucu ve veritabanı için kimlik doğrulama gereksinimleri yapılandırıldığından emin olun. Ayrıntılar için bkz [olağanüstü durum kurtarma sonrasında SQL veritabanı güvenlik](sql-database-geo-replication-security-config.md).

Gerçek iş sürekliliği elde etmek için veri merkezleri arasında veritabanı yedeklilik ekleyerek yalnızca çözümün parçasıdır. Bir arıza kurtarma hizmeti oluşturan tüm bileşenlerini ve tüm bağımlı hizmetlerin gerektirir sonra bir uygulama (hizmet) için uçtan uca kurtarma. İstemci yazılımını (örneğin, tarayıcı özel bir JavaScript ile), web ön uçları, depolama ve DNS bu bileşenlerin örnekleridir. Tüm bileşenlerin aynı hatalara karşı dayanıklı ve kurtarma süresi hedefi, uygulamanızın içinde (RTO) kullanılabilir hale önemlidir. Bu nedenle, tüm bağımlı hizmetleri tanımlayın ve yetenekleri sağlarlar ve garanti anlamak gerekir. Ardından, bağımlı olduğu hizmetlerin yük devretme sırasında hizmetinizin emin olmak için yeterli adımları atmanız gerekir. Olağanüstü durum kurtarma çözümleri tasarlama hakkında daha fazla bilgi için bkz. [olağanüstü durum kurtarma'yı kullanarak etkin coğrafi çoğaltma için bulut çözümleri tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="auto-failover-group-terminology-and-capabilities"></a>Otomatik Yük devretme grubu terminoloji ve özellikleri

- **Yük devretme grubu (SİS)**

  Bir yük devretme grubu, tek bir SQL veritabanı sunucusu ya da tüm veya bazı birincil veritabanı birincil bölgede kesinti nedeniyle kullanılamıyor, başka bir bölgede bir birim olarak yük devredebilir tek bir yönetilen örnek içinde yönetilen bir veritabanı için kullanılan bir gruptur. Yönetilen örnek için rdoc, tüm kullanıcı veritabanlarına örneğinde bir yük devretme grubu içerir ve bu nedenle bir örneği üzerinde yalnızca bir yük devretme grupları yapılandırılabilir.

- **SQL veritabanı sunucuları**

     SQL veritabanı sunucuları ile bir yük devretme grubunda bazılarını veya tümünü tek bir SQL veritabanı sunucusu kullanıcı veritabanlarında yerleştirilebilir. Ayrıca, SQL veritabanı sunucusu, tek bir SQL veritabanı sunucusunda birden çok yük devretme grupları destekler.

- **Birincil**

  SQL veritabanı sunucu veya yük devretme grubunda birincil veritabanlarını barındıran yönetilen örnek.

- **İkincil**

  SQL veritabanı sunucu veya yük devretme grubundaki ikincil veritabanlarını barındıran yönetilen örnek. İkincil birincil olarak aynı bölgede olamaz.

- **Tek veritabanları yük devretme grubuna ekleme**

  Birden fazla tek veritabanı aynı SQL veritabanı sunucusunda aynı yük devretme grubuna koyabilirsiniz. Tek bir veritabanı yük devretme grubuna eklerseniz, otomatik olarak ikincil sunucuda aynı sürümü ve işlem boyutu kullanarak ikincil bir veritabanı oluşturur.  Yük devretme grubu oluşturulduğunda bu sunucu belirtildi. İkincil bir veritabanı ikincil sunucuya zaten olan bir veritabanının eklerseniz, söz konusu coğrafi çoğaltma bağlantısı grubu tarafından devralınır. Yeni bir ikincil, ikincil sunucuya eklediğinizde, ikincil veritabanına yük devretme grubunun parçası olmayan bir Server'da zaten bir veritabanı oluşturulur.
  
  > [!IMPORTANT]
  > Yönetilen bir örneği, tüm kullanıcı veritabanlarına çoğaltılır. Kullanıcı veritabanı çoğaltması için bir alt kümesi yük devretme grubunda seçemezsiniz.

- **Veritabanları, elastik havuzunda yük devretme grubuna ekleme**

  İçinde bir elastik havuz tüm veya birden fazla veritabanı aynı yük devretme grubuna koyabilirsiniz. Birincil veritabanını bir elastik havuzda ise ikincil (ikincil havuzu) aynı ada sahip bir elastik havuzdaki otomatik olarak oluşturulur. İkincil sunucu yük devretme grubu tarafından oluşturulacak ikincil veritabanlarını barındırmak için yeterli kapasite ve aynı tam adını içeren bir elastik havuzun içerdiğinden emin olmanız gerekir. Bir veritabanı zaten ikincil havuzda ikincil veritabanına sahip havuzun eklerseniz, söz konusu coğrafi çoğaltma bağlantısı grubu tarafından devralınır. İkincil veritabanına yük devretme grubunun parçası olmayan bir Server'da zaten bir veritabanı eklediğinizde, yeni bir ikincil ikincil havuzunda oluşturulur.
  
- **DNS bölgesi**

  Yeni bir örneği oluşturulduğunda otomatik olarak oluşturulan bir benzersiz kimliği. Bu örnek için çoklu etki alanı (SAN) sertifika aynı DNS bölgesinde herhangi bir örneğine istemci bağlantılarının kimliğini doğrulamak için sağlanır. Aynı yük devretme grubunda iki yönetilen örnek, DNS bölgesini paylaşmanız gerekir. 
  
  > [!NOTE]
  > Bir DNS bölgesi kimliği SQL veritabanı sunucuları için oluşturulan yük devretme grupları için gerekli değildir.

- **Yük devretme grubu okuma / yazma dinleyici**

  Geçerli birincil ait URL işaret eden DNS CNAME kaydı oluşturulmuş. Yük devretme sonrasında birincil değiştiğinde şeffaf bir şekilde birincil veritabanına bağlanmak okuma-yazma SQL uygulamalar sağlar. Bir SQL veritabanı sunucusuna yük devretme grubu oluşturulduğunda, dinleyici URL'si için DNS CNAME kaydı olarak biçimlendirilmiş `<fog-name>.database.windows.net`. Yönetilen örneğinde yük devretme grubu oluşturulduğunda, dinleyici URL'si için DNS CNAME kaydı olarak biçimlendirilmiş `<fog-name>.zone_id.database.windows.net`.

- **Yük devretme grubu salt okunur dinleyicisi**

  Bir DNS CNAME kaydı ikincil ait URL işaret eden salt okunur dinleyicisi işaret eden oluşturulmuş. Belirtilen yük dengeleyici kurallarını kullanarak ikincil şeffaf bir şekilde bağlanmak salt okunur SQL uygulamalar sağlar. Bir SQL veritabanı sunucusuna yük devretme grubu oluşturulduğunda, dinleyici URL'si için DNS CNAME kaydı olarak biçimlendirilmiş `<fog-name>.secondary.database.windows.net`. Yönetilen örneğinde yük devretme grubu oluşturulduğunda, dinleyici URL'si için DNS CNAME kaydı olarak biçimlendirilmiş `<fog-name>.zone_id.secondary.database.windows.net`.

- **Otomatik Yük devretme İlkesi**

  Varsayılan olarak, bir yük devretme grubu bir otomatik yük devretme İlkesi ile yapılandırılır. SQL veritabanı hizmeti, bir arıza tespit ve yetkisiz kullanım süresi dolmuştur sonra yük devretmeyi tetikler. Sistem tarafından yerleşik kesinti azaltılamaz doğrulamalısınız [yüksek kullanılabilirlik Altyapısı SQL veritabanı hizmeti](sql-database-high-availability.md) ölçek etkisi nedeniyle. Uygulama yük devretme iş akışını denetlemek istiyorsanız, otomatik yük devretme kapatabilirsiniz.

- **Salt okunur bir yük devretme İlkesi**

  Varsayılan olarak, yük devretme işlemlerini salt okuma dinleyici devre dışıdır. İkincil çevrimdışı olduğunda birincil performansını etkilenmez sağlar. Ancak, bu da salt okunur oturumları ikincil kurtarılmadan kadar bağlanmak mümkün olmayacaktır anlamına gelir. Kapalı kalma süresi için salt okunur oturumları tolere olamaz ve olası performans düşüşü birincil çoğaltamaz hem salt okunur ve okuma-yazma trafiği geçici olarak birincil kullanılmak üzere Tamam, salt okunur dinleyici için yük devretme etkinleştirebilirsiniz. Bu durumda, ikincil kullanılabilir değilse, salt okunur trafiği otomatik olarak birincil siteye yönlendirilir.

- **Planlı yük devretme**

   Planlı yük devretme, birincil ve ikincil veritabanlarını birincil role ikincil anahtarlar önce arasında tam eşitleme gerçekleştirir. Bu, veri kaybı olmadan garanti eder. Planlı yük devretme, aşağıdaki senaryolarda kullanılır:

  - Veri kaybı kabul edilebilir olmadığında üretimde olağanüstü durum kurtarma (DR) tatbikatı gerçekleştirme
  - Veritabanları için farklı bir bölgede yeniden Yerleştir
  - Kesinti sonra birincil bölgeye veritabanları (yeniden çalışma) azaltılabilir döndürür.

- **Planlanmamış yük devretme**

   Planlanmamış veya zorlamalı yük devretme hemen ikincil birincil ile herhangi bir eşitleme olmadan birincil role geçirir. Bu işlem veri kaybına yol açar. Birincil erişilebilir olmadığı durumlarda, planlanmamış yük devretme kesintiler sırasında kurtarma yöntemi olarak kullanılır. Özgün birincil yeniden çevrimiçi olduğunda, bunu otomatik olarak eşitleme yeniden ve yeni ikincil olur.

- **El ile yük devretme**

  Otomatik Yük devretme yapılandırması bağımsız olarak herhangi bir zamanda yük devretme el ile başlatabilir. Otomatik Yük devretme İlkesi yapılandırılmamışsa, el ile yük devretme yük devretme grubunda ikincil veritabanlarını kurtarmak için gereklidir. Zorlanmış veya kolay yük devretme (tam veri eşitlemesi ile) başlatabilir. İkinci birincil, ikincil bölgeye yeniden konumlandırmak için kullanılabilir. Yük devretme tamamlandığında, DNS kayıtları yeni birincil bağlantı sağlamak için otomatik olarak güncelleştirilir

- **Veri kaybı yetkisiz kullanım süresi**

  Zaman uyumsuz çoğaltması kullanılarak birincil ve ikincil veritabanları eşitlenmiş olduğundan, yük devretme veri kaybına neden olabilir. Otomatik Yük devretme İlkesi, veri kaybı toleransını uygulamanızın yansıtacak şekilde özelleştirebilirsiniz. Yapılandırarak **GracePeriodWithDataLossHours**, sistem için sonuç veri kaybı olasılığı yüksek olan yük devretmeyi başlatmadan önce bekleyeceği süreyi denetleyebilirsiniz.

- **Birden çok yük devretme grupları**

  Yük devretmeleri ölçeğini denetlemek için birden çok yük devretme grupları sunucuları aynı çifti için yapılandırabilirsiniz. Her grup bağımsız olarak yük devreder. Elastik havuzlar, çok kiracılı bir uygulama kullanıyorsa, birincil ve ikincil veritabanlarının havuzdaki her karıştırmak için bu özelliği kullanabilirsiniz. Bu şekilde kesinti etkisini kiracılar yalnızca yarısını için azaltabilir.

  > [!NOTE]
  > Yönetilen örnek, birden çok yük devretme grupları desteklemez.
  
## <a name="permissions"></a>İzinler
Bir yük devretme grubu için izinler aracılığıyla yönetilir [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md). [SQL Server Katılımcısı](../role-based-access-control/built-in-roles.md#sql-server-contributor) rolü, yük devretme grupları yönetmek için gereken tüm izinlere sahiptir. 

### <a name="create-failover-group"></a>Yük devretme grubu oluşturma
Bir yük devretme grubu oluşturmak için birincil ve ikincil sunucular ve yük devretme grubundaki tüm veritabanları için RBAC yazma erişimi gerekir. Bir yönetilen örnek için hem birincil ve ikincil yönetilen örnek için RBAC yazma erişimi gerekir, ancak tek tek yönetilen örnek veritabanları eklenebilir veya bir yük devretme grubundan kaldırılmış olduğundan izinleri ayrı ayrı veritabanlarına ilgili değildir. 

### <a name="update-a-failover-group"></a>Bir yük devretme grubu güncelleştirme
Bir yük devretme grubu güncelleştirme için RBAC gerekir. yük devretme grubu ve geçerli birincil sunucu veya yönetilen örnek tüm veritabanlarına yazma erişimi.  

### <a name="failover-a-failover-group"></a>Bir yük devretme grubu yük devretme
Bir yük devretme grubu yük için RBAC yeni birincil sunucu yük devretme grubuna yazma erişimi gerekir veya yönetilen örneği. 

## <a name="best-practices-of-using-failover-groups-with-single-databases-and-elastic-pools"></a>Yük devretme grupları, tek veritabanları ve elastik havuzlar ile kullanmanın en iyi uygulamalar

Otomatik Yük devretme grubu, birincil SQL veritabanı sunucusunda yapılandırılmalıdır ve farklı bir Azure bölgesinde ikincil SQL veritabanı sunucusuna bağlanır.  Grupları, bu sunuculara tüm veya bazı veritabanları içerebilir. Aşağıdaki diyagramda, birden çok veritabanını ve otomatik yük devretme grubu kullanarak coğrafi olarak yedekli bir bulut uygulamasının tipik yapılandırma gösterilmektedir.

![Otomatik Yük devretme](./media/sql-database-auto-failover-group/auto-failover-group.png)

İş sürekliliği aklınızda ile bir hizmet tasarlarken aşağıdaki genel yönergeleri izleyin:

- **Yük devretme işlemlerini birden fazla veritabanını yönetmek için bir veya birden çok yük devretme grupları kullanma**

  Bir veya daha çok yük devretme grupları, farklı bölgelerde (birincil ve ikincil sunucular) iki sunucu arasında oluşturulabilir. Her grup, tüm veya bazı birincil veritabanı birincil bölgede kesinti nedeniyle kullanılamıyor, bir birim olarak kurtarılan bir veya birden çok veritabanı içerebilir. Yük devretme grubu, birincil olarak aynı hizmet hedefi ile coğrafi-ikincil veritabanı oluşturur. Mevcut bir coğrafi çoğaltma ilişkisi için yük devretme grubuna eklerseniz, coğrafi-ikincil aynı hizmet katmanına ve işlem boyutu birincil olarak yapılandırıldığından emin olun.

- **OLTP iş yükü için okuma / yazma dinleyici kullanın**

  OLTP işlemleri gerçekleştirirken kullanmak `<fog-name>.database.windows.net` sunucunun URL'sini ve bağlantı otomatik olarak birincil siteye yönlendirilir. Bu URL, yük devretme sonrasında değiştirmez. Not yük devretme, yalnızca istemci DNS önbelleği yenilendiğini sonra istemci bağlantıları yeni birincil siteye yönlendirilir DNS kaydı güncelleştirmeniz gerekir.

- **Salt okunur iş yükü için salt okunur bir dinleyici kullanın**

  Verilerin belirli eskime dayanıklı olan, mantıksal olarak yalıtılmış bir salt okunur yükünü varsa, ikincil veritabanı uygulamada kullanabilirsiniz. Salt okunur oturumları kullanmanızın `<fog-name>.secondary.database.windows.net` sunucunun URL'sini ve bağlantı otomatik olarak yönlendirilir ikincil. Ayrıca bağlantı dizesi kullanarak hedefi okuma belirtmek önerilir **ApplicationIntent salt okunur =**.

- **Performans düşüşü için hazırlıklı olmalıdır**

  SQL yük devretme karar, uygulamanın veya kullanılan diğer hizmetlerin geri kalanından bağımsızdır. Uygulama "tek bir bölge ve bazı durumlarda başka bazı bileşenleri ile karışık". Performans düşüşü önlemek için DR bölgesinde yedekli uygulama dağıtımı emin olun ve aşağıdaki adımları [güvenlik yönergeleri ağ](#failover-groups-and-network-security).

  > [!NOTE]
  > Farklı bağlantı dizesini kullanmak DR bölgesinde uygulama yok.  

- **Veri kaybı için hazırlama**

  Kesinti algılanırsa, SQL tarafından belirtilen süre boyunca bekler **GracePeriodWithDataLossHours**. Varsayılan değer 1 saattir. Veri kaybını göze alamayacağı, ayarladığınızdan emin olun **GracePeriodWithDataLossHours** 24 saat gibi yeterince büyük bir sayı. Birincil siteden ikincil bölgeden başarısız için el ile grubu yük devretme kullanın.

  > [!IMPORTANT]
  > 800 ya da daha az dtu'ları ve coğrafi çoğaltmayı kullanarak 250'den fazla veritabanları ile elastik havuzları artık planlanan yük devretmeler sorunlarla karşılaşabilirsiniz ve performansı düştü.  Bu sorunları yazma yoğunluklu iş yükleri için coğrafi çoğaltma uç noktaları coğrafyaya göre değişim yaygın olarak ayrılıyorsa ya da her veritabanı için birden çok ikincil uç noktaları kullanıldığında ortaya daha yüksektir.  Coğrafi çoğaltma gecikmesi zamanla artar, bu sorunları belirtileri gösterilir.  Bu gecikme kullanılarak izlenebilir [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database).  Bu sorun oluşursa, risk azaltma işlemleri havuz Dtu sayısını artırmak veya coğrafi olarak çoğaltılmış veritabanlarının aynı havuzdaki sayısını azaltmayı içerir.

## <a name="best-practices-of-using-failover-groups-with-managed-instances"></a>Yük devretme grupları ile yönetilen örnekleri kullanmanın en iyi uygulamalar

Otomatik Yük devretme grubu birincil örneğinde yapılandırılmalıdır ve farklı bir Azure bölgesinde ikinci örneğine bağlanın.  Örneğindeki tüm veritabanlarını ikincil örneğine çoğaltılır. Aşağıdaki diyagramda, yönetilen örneği ile otomatik yük devretme grubu bir coğrafi olarak yedekli bir bulut uygulamasının tipik yapılandırma gösterilmektedir.

![Otomatik Yük devretme](./media/sql-database-auto-failover-group/auto-failover-group-mi.png)

> [!IMPORTANT]
> Yönetilen örnek için otomatik yük devretme grupları şu genel Önizleme aşamasındadır.

Uygulamanız, yönetilen örnek, veri katmanı olarak kullanıyorsa, iş sürekliliği için tasarlarken şu genel yönergeleri izleyin:

- **İkincil örneği birincil örnekle aynı DNS bölgesi oluşturma**

  Birincil örneğe yönelik bağlantının kesintiye yük devretmenin ardından birincil ve ikincil emin olmak için örnekleri aynı DNS bölgesinde olmalıdır. Aynı çoklu etki alanı (SAN) sertifika ya da yük devretme grubunda iki örnek istemci bağlantılarının kimliğini doğrulamak için kullanılabilir garanti eder. Uygulamanızı Üretim dağıtımı için hazır olduğunda, ikincil bir örneği farklı bir bölgede oluşturun ve DNS bölgesini birincil örneğiyle paylaştığı emin olun. Belirterek bunu yapabilirsiniz bir `DNS Zone Partner` Azure portalı, PowerShell veya REST API'yi kullanarak isteğe bağlı parametre. 

  Birincil örnekle aynı adlı DNS bölgesinde ikincil örneği oluşturma hakkında daha fazla bilgi için bkz. [yönetilen örnek (Önizleme) ile yük devretme grupları yönetme](#powershell-managing-failover-groups-with-managed-instances-preview).

- **İki örnek arasındaki çoğaltma trafiğinin etkinleştir**

  Her örnek kendi Vnet'ine izole edilmiş olduğundan, bu sanal ağlar arasında iki yönlü trafik izin verilmesi gerekir. Bkz: [Azure VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md)

- **Bir yük devretme grubu yük devretme tüm örneğinin yönetmek için yapılandırma**

  Yük devretme grubu yük devretmesi örnekteki tüm veritabanlarını yönetebilir. Bir grup oluşturulduğunda, her bir örneği veritabanında otomatik olarak coğrafi-ikincil örneğine çoğaltılan olacaktır. Yük devretme grupları, bir alt kümesinin veritabanlarını kısmi bir yük devretmeyi başlatmak için kullanamazsınız.

  > [!IMPORTANT]
  > Bir veritabanını birincil örneğinden kaldırılırsa, bu da otomatik olarak coğrafi ikincil örnekteki bırakılır.

- **OLTP iş yükü için okuma / yazma dinleyici kullanın**

  OLTP işlemleri gerçekleştirirken kullanmak `<fog-name>.zone_id.database.windows.net` sunucunun URL'sini ve bağlantı otomatik olarak birincil siteye yönlendirilir. Bu URL, yük devretme sonrasında değiştirmez. Yük devretme, yalnızca istemci DNS önbelleği yenilendiğini sonra istemci bağlantıları yeni birincil siteye yönlendirilir için DNS kaydı güncelleştirmeniz gerekir. İkincil örneği ile birincil DNS bölgesi paylaştığından, istemci uygulaması aynı SAN sertifika kullanarak bağlanmayı mümkün olacaktır.

- **Coğrafi çoğaltmalı ikincil salt okunur sorgular için doğrudan bağlanma**

  Verilerin belirli eskime dayanıklı olan, mantıksal olarak yalıtılmış bir salt okunur yükünü varsa, ikincil veritabanı uygulamada kullanabilirsiniz. Doğrudan coğrafi çoğaltmalı ikincil veritabanına bağlanmak için `server.secondary.zone_id.database.windows.net` sunucusu olarak URL ve bağlantı yapılan doğrudan coğrafi çoğaltmalı ikincil veritabanına.

  > [!NOTE]
  > Belirli hizmet katmanlarında, Azure SQL veritabanı kullanımını destekler. [salt okunur çoğaltmalar](sql-database-read-scale-out.md) yüklemek ve bir salt okunur çoğaltma kapasitesini kullanmaya Bakiye salt okunur sorgu iş yükleri için `ApplicationIntent=ReadOnly` bağlantı parametresi dize. Coğrafi çoğaltmalı ikincil yapılandırıldığında, ya da salt okunur çoğaltma birincil konumda veya coğrafi olarak çoğaltılmış bir konuma bağlanmak için bu özelliği kullanabilirsiniz.
  > - Salt okunur bir çoğaltması birincil konumda bağlanmasını sağlayan `<fog-name>.zone_id.database.windows.net`.
  > - Salt okunur bir çoğaltması ikincil konumdaki bağlanmasını sağlayan `<fog-name>.secondary.zone_id.database.windows.net`.

- **Performans düşüşü için hazırlıklı olmalıdır**

  SQL yük devretme karar, uygulamanın veya kullanılan diğer hizmetlerin geri kalanından bağımsızdır. Uygulama "tek bir bölge ve bazı durumlarda başka bazı bileşenleri ile karışık". Performans düşüşü önlemek için DR bölgesinde yedekli uygulama dağıtımı emin olun ve aşağıdaki adımları [güvenlik yönergeleri ağ](#failover-groups-and-network-security).

- **Veri kaybı için hazırlama**

  Kesinti algılanırsa, sıfır veri kaybı bizim bilgi en iyi ise SQL otomatik olarak okuma / yazma yük devretmeyi tetikler. Aksi takdirde, belirtilen süre boyunca bekler `GracePeriodWithDataLossHours`. Belirttiyseniz `GracePeriodWithDataLossHours`, veri kaybı için hazır. Genel olarak, kesintiler sırasında Azure kullanılabilirlik ayrıcalık tanır. Veri kaybını göze alamayacağı, 24 saat gibi yeterince büyük bir sayı GracePeriodWithDataLossHours ayarladığınızdan emin olun.

  DNS güncelleştirme okuma-yazma dinleyici, yük devretme hemen başlatıldıktan sonra gerçekleşir. Bu işlem veri kaybına neden olmaz. Ancak, veritabanı rolleri geçiş işleminin normal koşullar altında 5 dakikaya kadar sürebilir. Tamamlanana kadar yeni birincil örnekteki bazı veritabanları yine de salt okunur olacaktır. PowerShell kullanarak yük devretme başlatılana, tüm işlemi zaman uyumlu olur. Azure portalını kullanarak başlatılır, UI tamamlanma durumu gösterir. REST API kullanarak başlatılır, standart Azure Resource Manager'ın yoklama mekanizması tamamlanma durumunu izlemek için kullanın.

  > [!IMPORTANT]
  > El ile grubu yük devretme seçimlerine özgün konuma geri taşımak için kullanın. Yük devretme neden oldu kesinti giderildikten sonra birincil veritabanlarınızı özgün konuma taşıyabilirsiniz. Bunu yapmak için el ile yük devretme grubunun başlatmalıdır.

## <a name="failover-groups-and-network-security"></a>Yük devretme grupları ve ağ güvenliği

Veri katmanı için ağ erişimi belirli bir bileşeni ya da VM gibi bileşenleri kısıtlanmıştır güvenlik kuralları gerektiren bazı uygulamalar için web hizmeti vb. Bu gereksinim, iş sürekliliği tasarım için bazı zorluklar ve yük devretme grupları kullanımını gösterir. Kısıtlı erişim uygularken aşağıdaki seçenekleri göz önünde bulundurun.

### <a name="using-failover-groups-and-virtual-network-rules"></a>Yük devretme grupları ve sanal ağ kuralları kullanma

Kullanıyorsanız [sanal ağ hizmet uç noktaları ve kuralları](sql-database-vnet-service-endpoint-rule-overview.md) SQL veritabanınıza erişimi kısıtlamak için her sanal ağ hizmet uç noktası geçerli tek bir Azure bölgesine olduğunu unutmayın. Uç nokta, alt ağından gelen iletişimi kabul etmek üzere diğer bölgeler etkinleştirmez. Bu nedenle, yalnızca aynı bölgede dağıtılan istemci uygulamaları birincil veritabanına bağlanabilirsiniz. Yük devretme, bir sunucu (ikincil) farklı bir bölgede yönlendirdi SQL istemci oturumlarını sonuçlanır olduğundan, bu oturumların, bölgesinin dışındaki bir istemcinin kaynaklandığı başarısız olur. Sanal ağ kuralları katılan sunucular eklenirse, bu nedenle, otomatik yük devretme İlkesi etkinleştirilemez. El ile yük devretme desteği için aşağıdaki adımları izleyin:

1. Ön uç bileşenleri, uygulamanızın (web hizmeti, sanal makineler vb.) ikincil bölgedeki yedekli kopyalarını sağlama
2. Yapılandırma [sanal ağ kuralları](sql-database-vnet-service-endpoint-rule-overview.md) birincil ve ikincil bir sunucu için ayrı ayrı
3. Etkinleştirme [ön uç yük devretme trafik Yöneticisi yapılandırması kullanma](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)
4. Kesinti algılandığında el ile yük devretme başlatın. Bu seçenek, ön uç ve veri katmanı arasında tutarlı bir gecikme gerektiren uygulamalar için iyileştirilmiştir ve ön uç, veri katmanı veya her ikisi de kesintiden etkilendiğinde kurtarmayı destekler.

> [!NOTE]
> Kullanıyorsanız **salt okuma dinleyici** yük dengelemek için bir salt okunur iş yükü, ikincil veritabanına bağlanabilmesi için bu iş yükünü bir VM veya ikincil bölgedeki diğer kaynak yürütüldüğünden emin olun.

### <a name="using-failover-groups-and-sql-database-firewall-rules"></a>Yük devretme grupları ve SQL veritabanı güvenlik duvarı kuralları kullanma

İş sürekliliği planınızın otomatik yük devretme grupları kullanarak yük devretme gerektiriyorsa, geleneksel güvenlik duvarı kuralları kullanarak SQL veritabanınıza erişimi kısıtlayabilirsiniz.  Otomatik Yük devretme desteği için aşağıdaki adımları izleyin:

1. [Bir genel IP oluşturma](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)
2. [Bir genel yük dengeleyici oluşturma](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-a-basic-load-balancer) ve genel IP atayın.
3. [Bir sanal ağ ve sanal makineler oluşturma](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-back-end-servers) , ön uç bileşenleri
4. [Ağ güvenlik grubu oluşturma](../virtual-network/security-overview.md) ve gelen bağlantıları yapılandırın.
5. Giden bağlantılar 'Sql' kullanarak Azure SQL veritabanı'na açık olduğundan emin olun [hizmet etiketi](../virtual-network/security-overview.md#service-tags).
6. Oluşturma bir [SQL veritabanı güvenlik duvarı kuralı](sql-database-firewall-configure.md) 1. adımda oluşturduğunuz genel IP adresinden gelen trafiğe izin verme.

Hakkında daha fazla bilgi için giden erişimi nasıl yapılandırılacağı ve hangi IP güvenlik duvarı kurallarında kullanmak için bkz: [yük dengeleyici giden bağlantılar](../load-balancer/load-balancer-outbound-connections.md).

Yukarıdaki yapılandırma, otomatik yük devretme ön uç bileşenlerini bağlantılarından engellemez ve uygulama ön ucu ve veri katmanı arasında daha uzun gecikme etkilenmemesini varsayar sağlayacaktır.

> [!IMPORTANT]
> Bölgesel kesintiler için iş sürekliliği sağlamak için hem ön uç bileşenleri hem de veritabanı için coğrafi artıklık emin olmanız gerekir.

## <a name="enabling-geo-replication-between-managed-instances-and-their-vnets"></a>Yönetilen örnekleri kendi sanal ağlar arasındaki coğrafi çoğaltma etkinleştirme

Birincil ve ikincil iki farklı bölgede yönetilen örnekleri arasında bir yük devretme grubu ayarladığınızda, her örnek bağımsız bir sanal ağ kullanarak yalıtılmıştır. İzin vermek için bu sanal ağlar arasındaki çoğaltma trafiğinin emin bu önkoşulların karşılandığından:

1. İki yönetilen örnek, farklı Azure bölgelerinde olması gerekir.
2. İkincil olması gerekir (kullanıcı veritabanları) boş.
3. Birincil ve ikincil yönetilen örnekleri aynı kaynak grubunda olması gerekir.
4. Yönetilen örnek üzerinden bağlanması gereken bir parçası olan sanal ağlar bir [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md). Küresel VNet eşlemesi desteklenmiyor.
5. İki yönetilen örnek sanal ağlar, çakışan IP adreslerine sahip olamaz.
6. Ağ güvenlik grupları (Bu bağlantı noktası 5022 NSG) bu tür ve 11000 aralığı ayarlamak gereken ~ 12000 olan açık gelen ve giden bağlantılar diğer yönetilen örnekli alt ağ için. Örnekler arasında çoğaltma trafiğine izin verecek şekilde budur

   > [!IMPORTANT]
   > NSG güvenlik kuralları liderlerine takılan veritabanı kopyalama işlemleri için yanlış.

7. İkincil örneği doğru DNS bölgesi kimliği ile yapılandırılır. Yönetilen örnek, DNS bölgesi özelliğidir ve Kimliğine konak adı adresi dahil edilir. Bölge kimliği, ilk yönetilen örnek, her bir sanal ağ oluşturulur ve aynı alt ağdaki diğer tüm örnekleri aynı kimliği atanır, rastgele bir dize olarak oluşturulur. DNS bölgesi, kendilerine atandıktan sonra değiştirilemez. Aynı yük devretme grubunda yer alan yönetilen örnekleri, DNS bölgesini paylaşmanız gerekir. Bunun için birincil örneğinin bölge kimliği DnsZonePartner parametresinin değeri ikincil örneği oluşturulurken geçirerek. 

## <a name="upgrading-or-downgrading-a-primary-database"></a>Yükseltme veya bir birincil veritabanı önceki sürüme indirme

Yükseltme veya birincil farklı işlem boyuta (aynı hizmet katmanında, genel amaçlı ve iş açısından kritik arasında değil) tüm ikincil veritabanlarını kesmeden düşürebilirsiniz. Yükseltme yaparken, tüm ikincil veritabanları yükseltmeniz ve ardından birincil yükseltmeniz önerilir. Önceki sürüme indirirken ters sırada: birincil ilk sürümüne düşürürseniz ve tüm ikincil veritabanları düşürme. Bu öneri, yükseltme ya da farklı bir hizmet katmanına düşürebilirsiniz zorlanır.

Bu sıra, özellikle daha düşük bir SKU'ya konumunda ikincil burada aşırı ve bir yükseltme veya düşürme işlemi sırasında reseeded bir sorundan kaçınmak için önerilir. Birincil salt okunur, birincil karşı tüm okuma-yazma iş yükleri etkileyen çoğaltamaz hale getirerek, sorun kaçınmanız. 

> [!NOTE]
> Yük devretme grubu yapılandırmasının bir parçası olarak ikincil bir veritabanı oluşturduysanız, bu ikincil veritabanını indirgemek için önerilmez. Bu veri katmanınızın yük devretme etkinleştirildikten sonra normal iş yükünü işlemek için yeterli kapasiteye sahip sağlamaktır.

## <a name="preventing-the-loss-of-critical-data"></a>Önemli verilerin kaybını önleme

Geniş alan ağları yüksek gecikme nedeniyle, sürekli kopyalama bir zaman uyumsuz çoğaltma mekanizması kullanır. Bir hata oluşması durumunda zaman uyumsuz çoğaltma bazı veri kaybı kaçınılmaz yapar. Ancak, bazı uygulamalar, veri kaybı olmadan gerektirebilir. Bu kritik güncelleştirmeler korumak için bir uygulama geliştiricisi çağırabilirsiniz [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) işlem Sistemi'ne hemen sonra sistem yordamı. Çağırma **sp_wait_for_database_copy_sync** son kabul edilen işlem ikincil veritabanına gönderilene kadar çağıran iş parçacığını engeller. Ancak, yeniden yürütülmesi ve ikincil kaydedilen iletilen işlemler için beklemez. **sp_wait_for_database_copy_sync** belirli sürekli kopyalama bağlantısı kapsamlıdır. Birincil veritabanına bağlantı haklarına sahip herhangi bir kullanıcı, bu yordam çağırabilir.

> [!NOTE]
> **sp_wait_for_database_copy_sync** yük devretmeden sonra veri kaybı yaşanmasını önler, ancak tam eşitleme yönelik okuma erişimi garanti etmez. Gecikme nedeniyle bir **sp_wait_for_database_copy_sync** yordam çağrısı önemli ölçüde fazla olabilir ve aramanın zaman işlem günlüğünün boyutuna bağlıdır.

## <a name="failover-groups-and-point-in-time-restore"></a>Yük devretme grupları ve zaman içinde nokta geri yükleme

Belirli bir noktaya geri yükleme ile yük devretme grupları kullanma hakkında daha fazla bilgi için bkz: [işaret zaman Kurtarma (PITR) içinde](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="programmatically-managing-failover-groups"></a>Program aracılığıyla yük devretme grupları yönetme

Otomatik Yük devretme grupları ve etkin daha önce açıklandığı gibi coğrafi çoğaltma ayrıca Azure PowerShell ve REST API'sini kullanarak program aracılığıyla yönetilebilir. Aşağıdaki tabloda kullanılabilir komut kümesi açıklanmaktadır. Etkin coğrafi çoğaltma içeren bir Azure Resource Manager API'leri kümesi yönetimi için de dahil olmak üzere [Azure SQL veritabanı REST API](https://docs.microsoft.com/rest/api/sql/) ve [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure/overview). Bu API'ler, kaynak gruplarının kullanımı gerektirir ve rol tabanlı güvenlik (RBAC) desteği. Uygulama erişim rolleri hakkında daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi](../role-based-access-control/overview.md).

### <a name="powershell-manage-sql-database-failover-with-single-databases-and-elastic-pools"></a>PowerShell: SQL veritabanı yük devretme ile tek veritabanları ve elastik havuzları yönetme

| Cmdlet | Açıklama |
| --- | --- |
| [New-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasefailovergroup) |Bu komut, bir yük devretme grubu oluşturur ve birincil ve ikincil sunucularda kaydeder|
| [Remove-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabasefailovergroup) | Yük devretme grubuna sunucudan kaldırır ve tüm siler ikincil veritabanları grubu dahil |
| [Get-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Yük devretme grubu yapılandırmasını alır. |
| [Set-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasefailovergroup) |Yük devretme grubu yapılandırmasını değiştirir |
| [Anahtar AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/switch-azsqldatabasefailovergroup) | İkincil sunucuya Yük devretme grubu yük devretme Tetikleyicileri |
| [Add-AzSqlDatabaseToFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/add-azsqldatabasetofailovergroup)|Bir veya daha fazla veritabanı için bir Azure SQL veritabanı yük devretme grubu ekler|
|  | |

> [!IMPORTANT]
> Örnek betik için bkz: [yapılandırma ve tek bir veritabanı için bir yük devretme grubu yük devretme](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md).
>

### <a name="powershell-managing-failover-groups-with-managed-instances-preview"></a>PowerShell: Yönetilen örnek (Önizleme) ile yük devretme grupları yönetme

#### <a name="install-the-newest-pre-release-version-of-powershell"></a>En yeni PowerShell yayın öncesi sürümünü yükleyin

1. PowerShellGet modülü 1.6.5 (veya en yeni önizleme sürümü) güncelleştirin. Bkz: [PowerShell Önizleme site](https://www.powershellgallery.com/packages/AzureRM.Sql/4.11.6-preview).

   ```powershell
      install-module PowerShellGet -MinimumVersion 1.6.5 -force
   ```

2. Yeni bir PowerShell penceresinde aşağıdaki komutları yürütün:

   ```powershell
      import-module PowerShellGet
      get-module PowerShellGet #verify version is 1.6.5 (or newer)
      install-module azurerm.sql -RequiredVersion 4.5.0-preview -AllowPrerelease –Force
      import-module azurerm.sql
   ```

#### <a name="powershell-commandlets-to-create-an-instance-failover-group"></a>Bir örnek yük devretme grubu oluşturmak için PowerShell cmdlet'leri

| API | Açıklama |
| --- | --- |
| New-AzureRmSqlDatabaseInstanceFailoverGroup |Bu komut, bir yük devretme grubu oluşturur ve birincil ve ikincil sunucularda kaydeder|
| Set-AzureRmSqlDatabaseInstanceFailoverGroup |Yük devretme grubu yapılandırmasını değiştirir|
| Get-AzureRmSqlDatabaseInstanceFailoverGroup |Yük devretme grubu yapılandırmasını alır.|
| Switch-AzureRmSqlDatabaseInstanceFailoverGroup |İkincil sunucuya Yük devretme grubu yük devretme Tetikleyicileri|
| Remove-AzureRmSqlDatabaseInstanceFailoverGroup | Bir yük devretme grubunu kaldırır|

### <a name="rest-api-manage-sql-database-failover-groups-with-single-and-pooled-databases"></a>REST API: Tek ve havuza alınmış veritabanları ile SQL veritabanı yük devretme grupları yönetme

| API | Açıklama |
| --- | --- |
| [Oluşturma veya yük devretme grubu güncelleştirme](https://docs.microsoft.com/rest/api/sql/failovergroups/createorupdate) | Oluşturur veya bir yük devretme grubu güncelleştirir |
| [Yük devretme grubunu sil](https://docs.microsoft.com/rest/api/sql/failovergroups/delete) | Yük devretme grubuna sunucudan kaldırır |
| [Yük devretme (Planlı)](https://docs.microsoft.com/rest/api/sql/failovergroups/failover) | Geçerli birincil sunucudan bu sunucuya devreder. |
| [Zorla yük devretme, veri kaybı izin ver](https://docs.microsoft.com/rest/api/sql/failovergroups/forcefailoverallowdataloss) |üzerinden bu sunucu için geçerli birincil sunucudan ails. Bu işlem veri kaybına neden. |
| [Get Failover Group](https://docs.microsoft.com/rest/api/sql/failovergroups/get) | Bir yük devretme grubunu alır. |
| [Sunucu tarafından yük devretme grupları Listele](https://docs.microsoft.com/rest/api/sql/failovergroups/listbyserver) | Sunucu yük devretme gruplarını listeler. |
| [Yük devretme grubu güncelleştirme](https://docs.microsoft.com/rest/api/sql/failovergroups/update) | Bir yük devretme grubu güncelleştirir. |
|  | |

### <a name="rest-api-manage-failover-groups-with-managed-instances-preview"></a>REST API: Yönetilen örnek (Önizleme) ile yük devretme grupları yönetme

| API | Açıklama |
| --- | --- |
| [Oluşturma veya yük devretme grubu güncelleştirme](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/createorupdate) | Oluşturur veya bir yük devretme grubu güncelleştirir |
| [Yük devretme grubunu sil](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/delete) | Yük devretme grubuna sunucudan kaldırır |
| [Yük devretme (Planlı)](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/failover) | Geçerli birincil sunucudan bu sunucuya devreder. |
| [Zorla yük devretme, veri kaybı izin ver](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/forcefailoverallowdataloss) |üzerinden bu sunucu için geçerli birincil sunucudan ails. Bu işlem veri kaybına neden. |
| [Get Failover Group](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/get) | Bir yük devretme grubunu alır. |
| [Yük devretme grupları - konuma göre listesi](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/listbylocation) | Bir konumdaki yük devretme gruplarını listeler. |

## <a name="next-steps"></a>Sonraki adımlar

- Örnek betikler için bkz:
  - [Etkin coğrafi çoğaltmayı kullanarak tek bir veritabanını yapılandırma ve tek bir veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
  - [Etkin coğrafi çoğaltmayı kullanarak havuza alınan veritabanını yapılandırma ve havuza alınmış veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
  - [Tek bir veritabanı için bir yük devretme grubunu yapılandırma ve yük devretme grubunun yükünü devretme](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md)
- Veritabanı otomatik yedeklemeler Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı otomatik yedeklerinde](sql-database-automated-backups.md).
- Otomatik yedekleme, kurtarma için kullanma hakkında bilgi edinmek için [hizmet tarafından başlatılan yedeklemelerden veritabanını geri yükleme](sql-database-recovery-using-backups.md).
- Yeni birincil sunucu ve veritabanı için kimlik doğrulama gereksinimleri hakkında bilgi edinmek için bkz. [olağanüstü durum kurtarma sonrasında SQL veritabanı güvenlik](sql-database-geo-replication-security-config.md).
