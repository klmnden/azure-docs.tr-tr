---
title: Yüksek kullanılabilirlik - Azure SQL veritabanı hizmetinin | Microsoft Docs
description: Azure SQL Database hizmeti yüksek kullanılabilirlik yetenekleri ve özellikleri hakkında bilgi edinin
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.topic: article
ms.date: 04/04/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: e85db04206927eaf17cf52c11b536c75a47a088e
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="high-availability-and-azure-sql-database"></a>Yüksek kullanılabilirlik ve Azure SQL veritabanı
Azure SQL veritabanı PaaS teklifi en başından itibaren Microsoft, yüksek kullanılabilirlik (HA) hizmetine inşa edilmiş ve müşterilerin çalışır, özel mantığı ekleyin veya HA geçici kararları için gerekli değildir, müşterilerinin promise yapmıştır. Microsoft, müşterilerin bir SLA sunumu HA sistem yapılandırması ve işlem üzerinde tam denetim tutar. HA SLA durumlarda Microsoft'un makul şekilde denetimi dışında etkenler nedeniyle olan toplam bölge hata koruma sağlamaz ve bir bölgede bir SQL veritabanı için geçerlidir (örneğin, doğal afet, war, terörist saldırılarını, riots, devlet eylemi olaylardan veya ağ veya cihaz arızası müşteri sitelerden veya müşteri siteleri ve Microsoft'un veri merkezi arasında dahil, Microsoft'un veri merkezlerinin dışındaki).

HA sorun alan basitleştirmek için Microsoft aşağıdaki varsayımlar kullanır:
1.  Donanım ve yazılım hatalarını kaçınılmaz
2.  İşletimsel personel hatalarına neden hata yapma
3.  Planlı bakım işlemleri kesintileri neden olur 

Bu tür olayları tek tek sık bulut ölçeğinde olsa da, her gün yoksa her hafta oluşur. 

## <a name="fault-tolerant-sql-databases"></a>Hataya dayanıklı SQL veritabanları
Müşteriler kendi veritabanlarını dayanıklılık içinde en ilgilendiğiniz ve dayanıklılık SQL veritabanı hizmetinin bir bütün olarak daha az ilgilendiğiniz. bir hizmet için % 99,99 açık kalma süresi "veritabanı" %0,01 kapalı olan veritabanlarının parçası ise anlamsız hale gelir. Her veritabanı hataya dayanıklı olması ve hataya azaltma hiçbir zaman kaydedilmiş işlemi kaybına neden. 

Veriler için SQL veritabanını doğrudan bağlı diskler/VHD'lerde bağlı yerel depolama (LS) ve Azure Premium Storage sayfa blobları üzerinde temel uzaktaki depolama birimi (RS) kullanır. 
- İş kritik (Önizleme) veritabanları ve esnek havuzlar için tasarlanmış yüksek IOPS gereksinimleri olan kritik OLTP uygulamalar görev veya yerel depolama Premium'da kullanılır. 
- Uzaktaki Depolama birimi depolama gerektiren ve bağımsız olarak ölçeklendirebilirsiniz güç işlem bütçe yönelik kurumsal iş yükleri için tasarlanmış, temel ve standart hizmet katmanları için kullanılır. Tek sayfa blobu veritabanı ve günlük dosyaları ve yerleşik depolama çoğaltma ve yük devretme mekanizmalarını kullanırlar.

Her iki durumda da çoğaltma, hata algılama ve yük devretme mekanizmaları SQL veritabanı için tam otomatik ve insan etkileşimi olmadan çalışır. Bu mimari, kaydedilmiş veri hiç kayıp olduğundan ve veri dayanıklılığı tüm öncelikli olacağını emin olmak için tasarlanmıştır.

Başlıca yararları:
- Müşteriler tam çoğaltılmış veritabanlarının yapılandırmak veya karmaşık donanım, yazılım, işletim sistemleri veya sanallaştırma ortamları bakımını yapmak zorunda kalmadan yararlanır.
- İlişkisel veritabanları tam ACID özellikleri sistem tarafından korunur.
- Yük devretme işlemlerini tam olarak kaydedilmiş bir veri kaybı olmadan otomatik değildir.
- Birincil çoğaltma bağlantıları yönlendirme dinamik olarak hizmet tarafından uygulama mantığı ile yönetilir.
- Otomatik artıklık yüksek düzeyde ekstra ücret ödemeden sağlanır.

> [!NOTE]
> Bildirilmeksizin açıklanan yüksek kullanılabilirlik mimarisidir. 

## <a name="data-redundancy"></a>Veri yedekliği

SQL veritabanı yüksek kullanılabilirlik çözümde dayanır [Always ON kullanılabilirlik grupları](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) SQL Server teknolojisini ve en az farklılıklar LS ve RS veritabanlarıyla çalışır hale getirir. Kullanılabilirlik (aktif coğrafi çoğaltma tarafından düşük RTO) için kullanılan RS içinde sırasında LS yapılandırmasında kalıcılığını Always ON kullanılabilirlik grubu teknolojisi kullanılır. 

## <a name="local-storage-configuration"></a>Yerel depolama yapılandırması

Bu yapılandırmada, her veritabanı çevrimiçi denetim halkası içinde Yönetim Hizmeti (MS) tarafından duruma getirilir. Bir birincil çoğaltma ve en az iki ikincil çoğaltmaları (çekirdek kümesi) üç bağımsız fiziksel alt sistemleri aynı veri merkezinde bulunan yayılan Kiracı halka içinde yer alır. Birincil çoğaltma ağ geçidi (GW) tarafından gönderilen tüm okuma ve yazma işlemleri ve yazma ikincil çoğaltmalar için zaman uyumsuz olarak çoğaltılır. SQL veritabanı, birincil ve işlem yürütme önce en az bir ikincil çoğaltma için veri yazıldığı çekirdek tabanlı yürütme düzenini kullanır.

[Service Fabric](../service-fabric/service-fabric-overview.md) yük devretme sistem otomatik olarak düğüm başarısız olarak çoğaltmaları yeniden oluşturur ve çekirdek kümesi üyelik düğümleri alınır ve sistem katılma olarak korur. Planlı bakım çekirdek kümesi minimum yineleme sayısı (genellikle 2) giderek önlemek için dikkatle koordine edilen. Bu model (Önizleme) veritabanları Premium ve iş kritik için iyi çalışır, ancak işlem ve depolama bileşenleri artıklığı gerektirir ve daha yüksek bir maliyet sonuçlanır.

## <a name="remote-storage-configuration"></a>Uzak Depolama yapılandırması

Uzak Depolama yapılandırmaları (temel ve standart katmanları) için tam olarak bir kopya dayanıklılık, artıklık ve bit rot algılama için depolama sistemlerini yeteneklerini kullanarak uzak blob storage'da korunur. 

Yüksek kullanılabilirlik mimarisi tarafından Aşağıdaki diyagramda gösterilmiştir:
 
![yüksek kullanılabilirlik mimarisi](./media/sql-database-high-availability/high-availability-architecture.png)

## <a name="failure-detection-and-recovery"></a>Hata algılama ve kurtarma 
Büyük ölçekli bir dağıtılmış sistem hataları güvenle, algılayabilir bir yüksek oranda güvenilir hatası algılama sistemde gerekli hızla ve müşteri için mümkün olduğunca yakın. SQL veritabanı için bu sistem, Azure Service Fabric temel alır. 

Birincil çoğaltma ile birincil çoğaltma başarısız oldu ve tüm okuma ve yazma işlemleri birincil çoğaltmadaki önce gerçekleşmesi için iş devam edemiyor açmadıklarını ve ne zaman hemen açıktır. Kurtarma süresi hedefi (RTO) bir ikincil çoğaltma birincil durum yükseltme Bu işlemi olan 30 saniye ve kurtarma noktası hedefi (RPO) = 0 =. 30 saniye RTO etkisini azaltmak için birkaç kez ile bağlantı başarısız girişimleri için daha küçük bir bekleme süresi yeniden bağlanmayı denemek için en iyi yöntem olacaktır.

Bir ikincil çoğaltma başarısız olduğunda, en az çekirdek-içeren bir küme, hiçbir yedek aşağıya doğru bir veritabanıdır. Service fabric yeniden yapılandırma işlemi başlatır için birincil çoğaltma hatasının izleyen işlemine benzer şekilde hatası kalıcı olup olmadığını belirlemek için kısa bir bekleme sonra başka bir ikincil çoğaltma oluşturulur. İşletim sistemi hatası veya yükseltme gibi geçici hizmet durumunun durumlarda yeni bir çoğaltma hemen yerine yeniden başlatmak başarısız olan düğümün izin vermek için yerleşik olmayan. 

Uzak Depolama yapılandırmaları için SQL veritabanı yükseltme sırasında her zaman açık işlevi yük devretme veritabanları için kullanır. Bunu yapmak için yeni bir SQL örneği önceden planlanmış yükseltme olay bir parçası olarak başlar ve ekler ve veritabanı dosyası uzaktaki depolama biriminden kurtarır. İşlem çökmesi veya diğer planlanmamış olayları, durumunda Windows Fabric örneği kullanılabilirlik yönetir ve, Kurtarma son adım olarak, Uzak veritabanı dosyasını ekler.

## <a name="zone-redundant-configuration-preview"></a>Bölge olarak yedekli yapılandırması (Önizleme)

Varsayılan olarak, yerel depolama yapılandırmaları için çekirdek kümesi çoğaltmalar aynı veri merkezinde oluşturulur. Girişiyle [Azure kullanılabilirlik bölgeleri](../availability-zones/az-overview.md), farklı bir kullanılabilirlik bölgeleri aynı bölgede çekirdek kümeleri farklı çoğaltmaları koyun seçeneğine sahipsiniz. Tek bir hata noktası ortadan kaldırmak için Denetim halkası ayrıca birden çok dilimlerinde üç ağ geçidi çalma (GW) çoğaltılır. Belirli ağ geçidi halka için yönlendirme tarafından denetlenen [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM). Bölge olarak yedekli yapılandırması ek veritabanı artıklığı, Premium kullanılabilirlik bölgelerinde kullanımını oluşturmaz veya iş kritik (Önizleme) hizmet katmanları hiçbir ek kullanılabilir olduğundan maliyeti. Bölge olarak yedekli veritabanını seçerek, Premium yapabilir veya iş kritik (Önizleme) uygulama mantığının herhangi bir değişiklik yapılmadan geri dönülemez veri merkezi kesintilerini dahil hataları, çok daha büyük bir dizi esnek veritabanları. Bölge olarak yedekli yapılandırması da mevcut Premium ya da iş kritik veritabanları veya havuzları (Önizleme) dönüştürebilirsiniz.

Bölge olarak yedekli çekirdek kümesi aralarında bazı uzaklıklı farklı veri merkezlerinde çoğaltmaları olduğundan, artan ağ gecikmesi yürütme süresini artırabilir ve bu nedenle bazı OLTP iş yüklerinin performansını etkileyebilir. Her zaman dilimi artıklık ayarını devre dışı bırakarak tek bölge yapılandırmaya geri dönebilirsiniz. Bu işlem veri işlemi bir boyuta sahiptir ve normal hizmet düzeyi hedefi (SLO) güncelleştirme benzer. İşlemin sonunda, veritabanı veya havuzu bir bölge olarak yedekli halka dışında tek bir bölge halka (veya tersi) geçirilir.

> [!IMPORTANT]
> Bölge olarak yedekli veritabanları ve esnek havuzlar yalnızca desteklenen Premium ve iş kritik (Önizleme) hizmet katmanları. Genel Önizleme, yedeklemeler ve Denetim sırasında kayıtları RA-GRS depolama alanında depolanır ve bu nedenle bir bölge genelinde kesinti durumunda otomatik olarak kullanılamayabilir. 

Yüksek kullanılabilirlik mimarisi bölge olarak yedekli sürümü tarafından Aşağıdaki diyagramda gösterilmiştir:
 
![yüksek kullanılabilirlik mimarisi bölge olarak yedekli](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="read-scale-out"></a>Genişleme okuma
Açıklandığı gibi Premium ve iş kritik (Önizleme) katmanları Dengeleme çekirdek-ayarlar ve AlwaysON teknoloji hem de tek bir bölge ve bölge olarak yedekli yapılandırmalarını yüksek kullanılabilirlik için hizmet. AlwasyON yararları çoğaltmaların her zaman işlemsel olarak tutarlı durumda olduğundan biridir. Çoğaltmaları birincil olarak aynı performans düzeyine sahip olduğundan, uygulama, salt okunur iş yüklerini hiçbir ek de bakım bu ek kapasite yararlanabilir maliyeti (okuma genişleme). Bu şekilde salt okunur sorguları ana okuma-yazma yükünden yalıtılması ve kendi performansını etkilemez. Ölçeklendirme özelliği kullanılmaya okuma mantıksal olarak içeren uygulamalar için salt okunur iş yükleri çözümlemeleri gibi ayrılmış ve bu nedenle bu ek kapasite için birincil bağlanmadan yararlanan. 

Belirli bir veritabanıyla okuma genişleme özelliği kullanmak için açık bir şekilde veritabanı oluşturulurken veya daha sonra çağırarak PowerShell kullanarak yapılandırmasını değiştirmeyi etkinleştirmeniz gerekir [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) veya [ AzureRmSqlDatabase yeni](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlet'leri kullanarak Azure Resource Manager REST API'si aracılığıyla veya [veritabanları - oluştur veya Güncelleştir](/rest/api/sql/databases/createorupdate) yöntemi.

Bir veritabanı için okuma genişleme etkinleştirildikten sonra o veritabanına bağlanan uygulamaları okuma-yazma çoğaltmaya veya salt okunur çoğaltmaya göre veritabanının yönlendirilirsiniz `ApplicationIntent` uygulamanın içinde yapılandırılmış özelliği bağlantı dizesi. Hakkında bilgi için `ApplicationIntent` özelliği, bkz: [uygulama amacı belirtme](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent) 

Okuma genişleme özelliği oturum düzeyinde tutarlılık destekler. Bağlantı hatası neden sonra çoğaltma kullanılabilir olmamasından salt okunur oturum bağlanırsa, farklı bir kopyasına yönlendirilebilir. Olası olsa da, eski veri kümesinin işleme'de neden olabilir. Bir uygulama bir okuma-yazma oturumu kullanarak verileri yazar ve hemen salt okunur oturumu kullanarak okur, benzer şekilde, yeni veriler hemen görünür değil mümkündür.

## <a name="conclusion"></a>Sonuç
Azure SQL veritabanı ile Azure platformu son derece tümleşiktir ve Service Fabric yüksek oranda hata algılama ve veri koruma için Azure Storage Bloblarında ve daha yüksek hata toleransı için kullanılabilirlik bölgeleri kurtarma bağlıdır. Aynı anda Azure SQL veritabanı SQL Server kutusunu ürün çoğaltma ve yük devretme için her zaman açık teknolojisini tam olarak yararlanır. Bu teknolojiler birleşimi tam olarak bir karma depolama modelinin faydaları hayata geçirmek ve zorlu SLA desteklemek uygulamaların sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Azure kullanılabilirlik bölgeleri](../availability-zones/az-overview.md)
- Hakkında bilgi edinin [Service Fabric](../service-fabric/service-fabric-overview.md)
- Hakkında bilgi edinin [Azure trafik Yöneticisi](../traffic-manager/traffic-manager-overview.md) 
