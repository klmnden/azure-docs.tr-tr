---
title: "Yüksek kullanılabilirlik - Azure SQL veritabanı hizmetinin | Microsoft Docs"
description: "Azure SQL Database hizmeti yüksek kullanılabilirlik yetenekleri ve özellikleri hakkında bilgi edinin"
keywords: 
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
ms.assetid: 
ms.service: sql-database
ms.custom: 
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 12/13/2017
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: c0a140c959f14c2e8ceaddad5d323f0900be5d2f
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="high-availability-and-azure-sql-database"></a>Yüksek kullanılabilirlik ve Azure SQL veritabanı
Azure SQL veritabanı PaaS teklifi en başından itibaren Microsoft, yüksek kullanılabilirlik (HA) hizmetine inşa edilmiş ve müşterilerin çalışır, özel mantığı eklemek için gerekli olmaması olan veya HA geçici kararlar müşterilerimizin promise yapmıştır. Microsoft, müşterilerin bir SLA sunar ve HA sistem yapılandırması ve işlem üzerinde tam denetimi korumak. HA SLA bizim makul şekilde denetimi dışında kalan etmenler nedeniyle olan durumlarda toplam bölge hata koruma sağlamaz ve bir bölgede bir SQL veritabanı için geçerlidir (örneğin, doğal afet war, terörist saldırılarını, riots, devlet eylem veya bir ağ davranır veya Aygıt hatası müşteri sitesindeki veya müşteri sitesi ile veri merkezimiz arasındaki dahil olmak üzere, veri merkezleri için dış).

HA sorun alan basitleştirmek için Microsoft aşağıdaki varsayımlar kullanır:
1.  Donanım ve yazılım hatalarını kaçınılmaz
2.  İşletimsel personel hatalarına neden hata yapma
3.  Planlı bakım işlemleri kesintileri neden olur 

Bu tür olayları tek tek sık bulut ölçeğinde, bunların her gün, haftada durumdayken. 

## <a name="fault-tolerant-sql-databases"></a>Hataya dayanıklı SQL veritabanları
Müşteriler kendi veritabanlarını dayanıklılık içinde en ilgilendiğiniz ve dayanıklılık SQL veritabanı hizmetinin bir bütün olarak daha az ilgilendiğiniz. bir hizmet için % 99,99 açık kalma süresi "veritabanı" %0,01 kapalı olan veritabanlarının parçası ise anlamsız hale gelir. Her veritabanı hataya dayanıklı olması ve hataya azaltma hiçbir zaman kaydedilmiş işlemi kaybına neden. 

Veriler için SQL veritabanını doğrudan bağlı diskler/VHD'lerde bağlı yerel depolama (LS) ve Azure Premium Storage sayfa blobları üzerinde temel uzaktaki depolama birimi (RS) kullanır. 
- Yerel depolama yüksek IOPS gereksinimleri olan OLTP uygulamalar için tasarlanmış havuzları ve Premium veritabanlarını kullanılır. 
- Uzaktaki Depolama birimi depolama gerektirir ve bağımsız olarak ölçeklendirebilirsiniz güç işlem küçük, soğuk veya büyük veritabanları için tasarlanmış, temel ve standart hizmet katmanları için kullanılır. Tek sayfa blobu veritabanı ve günlük dosyaları ve yerleşik depolama çoğaltma ve yük devretme mekanizmalarını kullanırlar.

Her iki durumda da çoğaltma, hata algılama ve yük devretme mekanizmaları SQL veritabanı için tam otomatik ve insan etkileşimi olmadan çalışır. Bu mimari, kaydedilmiş veri hiç kayıp olduğundan ve veri dayanıklılığı tüm öncelikli olacağını emin olmak için tasarlanmıştır.

Başlıca yararları:
- Müşteriler tam çoğaltılmış veritabanlarının yapılandırmak veya karmaşık donanım, yazılım, işletim sistemi veya sanallaştırma ortamları bakımını yapmak zorunda kalmadan yararlanır.
- İlişkisel veritabanları tam ACID özellikleri sistem tarafından korunur.
- Yük devretme işlemlerini tam olarak kaydedilmiş bir veri kaybı olmadan otomatik değildir.
- Birincil çoğaltma bağlantıları yönlendirme dinamik olarak hizmet tarafından uygulama mantığı ile yönetilir.
- Otomatik artıklık yüksek düzeyde ekstra ücret ödemeden sağlanır.

> [!NOTE]
> Bildirilmeksizin açıklanan yüksek kullanılabilirlik mimarisidir. 

## <a name="data-redundancy"></a>Veri yedekliği

SQL veritabanı yüksek kullanılabilirlik çözümde dayanır [AlwaysON](/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) SQL Server teknolojisini ve en az farklılıklar LS ve RS veritabanlarıyla çalışır hale getirir. RS sırada kalıcılığı için kullanılan LS yapılandırmada AlwaysON kullanılabilirlik (düşük RTO) için kullanılır. 

## <a name="local-storage"></a>Yerel depolama

LS durumda da, her veritabanı çevrimiçi denetim halkası içinde Yönetim Hizmeti (MS) tarafından duruma getirilir. Bir birincil çoğaltma ve en az iki ikincil çoğaltmaları (çekirdek kümesi) üç bağımsız fiziksel alt sistemleri aynı veri merkezinde bulunan yayılan Kiracı halka içinde yer alır. Birincil çoğaltma ağ geçidi (GW) tarafından gönderilen tüm okuma ve yazma işlemleri ve yazma ikincil çoğaltmalar için zaman uyumsuz olarak çoğaltılır. SQL veritabanı, birincil ve işlem yürütme önce en az bir ikincil çoğaltma için veri yazıldığı çekirdek tabanlı yürütme düzenini kullanır.

[Service Fabric](/azure/service-fabric/service-fabric-overview.md) yük devretme sistem otomatik olarak düğüm başarısız olarak çoğaltmaları yeniden oluşturur ve çekirdek kümesi üyelik düğümleri alınır ve sistem katılma olarak korur. Planlı bakım çekirdek kümesi minimum yineleme sayısı (genellikle 2) giderek önlemek için dikkatle koordine edilen. Bu model Premium veritabanları için iyi çalışır, ancak işlem ve depolama bileşenleri artıklığı gerektirir ve daha yüksek bir maliyet sonuçlanır.

## <a name="remote-storage"></a>Uzaktaki Depolama Birimi

Uzak Depolama yapılandırmaları (temel ve standart katmanları) için veritabanının tam olarak bir kopyasını uzak blob depolama, depolama sistemlerini yeteneklerini dayanıklılık, artıklık ve bit rot algılama yararlanarak tutulur. 

Yüksek kullanılabilirlik mimarisi tarafından Aşağıdaki diyagramda gösterilmiştir:
 
![yüksek kullanılabilirlik mimarisi](./media/sql-database-high-availability/high-availability-architecture.png)

## <a name="failure-detection--recovery"></a>Hata algılama & kurtarma 
Büyük ölçekli bir dağıtılmış sistem hataları güvenle, algılayabilir bir yüksek oranda güvenilir hatası algılama sistemde gerekli hızla ve müşteri için mümkün olduğunca yakın. SQL veritabanı için bu sistem, Azure Service Fabric temel alır. 

Birincil çoğaltma ile birincil çoğaltma başarısız oldu ve tüm okuma ve yazma işlemleri birincil çoğaltmadaki önce gerçekleşmesi için iş devam edemiyor açmadıklarını ve ne zaman hemen açıktır. Kurtarma süresi hedefi (RTO) bir ikincil çoğaltma birincil durum yükseltme Bu işlemi olan 30 saniye ve kurtarma noktası hedefi (RPO) = 0 =. 30 saniye RTO etkisini azaltmak için birkaç kez ile bağlantı başarısız girişimleri için daha küçük bir bekleme süresi yeniden bağlanmayı denemek için en iyi yöntem olacaktır.

Bir ikincil çoğaltma başarısız olduğunda, en az çekirdek-içeren bir küme, hiçbir yedek aşağıya doğru bir veritabanıdır. Service fabric yeniden yapılandırma işlemi başlatır için birincil çoğaltma hatasının izleyen işlemine benzer şekilde hatası kalıcı olup olmadığını belirlemek için kısa bir bekleme sonra başka bir ikincil çoğaltma oluşturulur. İşletim sistemi hatası veya yükseltme gibi geçici hizmet durumunun durumlarda yeni bir çoğaltma hemen yerine yeniden başlatmak başarısız olan düğümün izin vermek için yerleşik olmayan. 

Uzak Depolama yapılandırmaları için SQL veritabanı yükseltmeleri sırasında yük devretme veritabanları için AlwaysON işlevini kullanır. Bunu yapmak için yeni bir SQL örneği önceden planlanmış yükseltme olay bir parçası olarak başlar ve ekler ve veritabanı dosyası uzaktaki depolama biriminden kurtarır. İşlem çökmesi veya diğer planlanmamış olayları, durumunda Windows Fabric örneği kullanılabilirlik yönetir ve, Kurtarma son adım olarak, Uzak veritabanı dosyasını ekler.

## <a name="conclusion"></a>Sonuç
Azure SQL DB Azure platformu ile son derece tümleşiktir ve yüksek oranda hatası algılama ve kurtarma için Service Fabric ve veri koruma için Azure Storage Bloblarında bağlıdır. Aynı anda Azure SQL veritabanı çoğaltma ve yük devretme için SQL Server kutusunu ürün AlwaysOn teknolojisini kullanır. Bu teknolojiler birleşimi tam olarak bir karma depolama modelinin faydaları hayata geçirmek ve zorlu SLA desteklemek uygulamaların sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Service Fabric](/azure/service-fabric/service-fabric-overview.md)
- Hakkında bilgi edinin [Azure trafik Yöneticisi](/traffic-manager/traffic-manager-overview.md) 
