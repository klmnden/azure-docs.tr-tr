---
title: Azure Search için Hizmet Yönetimi portalında - Azure Search
description: Azure Search hizmeti, Azure portalını kullanarak Microsoft Azure üzerinde barındırılan bulut arama hizmeti yönetin.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: d5820c927b88eba37eaf092dfd4b209180bfc8eb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60565446"
---
# <a name="service-administration-for-azure-search-in-the-azure-portal"></a>Azure portalında Azure arama için Hizmet Yönetimi
> [!div class="op_single_selector"]
> * [PowerShell](search-manage-powershell.md)
> * [REST API](https://docs.microsoft.com/rest/api/searchmanagement/)
> * [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Portal](search-manage.md)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Azure arama, özel uygulamalarda zengin arama deneyimi oluşturmak için kullanılan tam olarak yönetilen, bulut tabanlı arama hizmetidir. Bu makalede de gerçekleştirebileceğiniz hizmeti yönetim görevleri kapsar [Azure portalında](https://portal.azure.com) zaten sağlanmış bir arama hizmeti için. Hizmet Yönetimi basit tasarıma göre aşağıdaki görevler için sınırlı:

> [!div class="checklist"]
> * Erişimi yönetme *api anahtarlarını* hizmetinize okuma veya yazma için kullanılır.
> * Hizmet kapasite ayırma bölümleri ve çoğaltmalarını değiştirerek ayarlayın.
> * Hizmet katmanının maksimum sınırlara göre kaynak kullanımını izleyin.

Dikkat *yükseltme* bir yönetim görevi olarak listelenmemiş. Hizmet sağlandığında kaynakların ayrıldığından farklı bir katmana taşıyarak yeni bir hizmettir. Ayrıntılar için bkz [bir Azure Search hizmeti oluşturma](search-create-service-portal.md).

> [!Tip]
> Arama trafiği ya da sorgu performansını analiz etme hakkında Yardım almak mı istiyorsunuz? Arayın ve nasıl başarılı bir arama sonuçları belirli belgeleri dizininize yol gösterici müşterilere koşulları kişiler sorgu birimi izleyebilirsiniz. Daha fazla bilgi için [için Azure Search arama trafiği analizi](search-traffic-analytics.md), [kullanım ve sorgu ölçümlerini izlemek](search-monitor-usage.md), ve [performans ve iyileştirme](search-performance-optimization.md).

<a id="admin-rights"></a>

## <a name="administrator-rights"></a>Yönetici hakları
Sağlama veya yetkisini alma hizmeti bir Azure aboneliğinin Yöneticisi veya ortak yönetici tarafından yapılabilir.

İçinde bir hizmet, hizmet URL'sini ve yönetici api anahtarı erişimi olan herkes hizmete okuma-yazma erişimi vardır. Okuma-yazma erişimi eklemek, silmek veya aracılığıyla uygulanan API anahtarları, dizinler, dizin oluşturucular, veri kaynakları, zamanlamalar ve rol atamaları dahil, sunucu nesneleri değiştirme olanağı sağlar [RBAC tanımlı roller](search-security-rbac.md).

Azure Search ile tüm kullanıcı etkileşimi Bu modlardan biri içinde kalan: okuma-yazma erişimi (yönetici hakları) hizmetine veya salt okunur erişim (sorgu hakları) hizmetine. Daha fazla bilgi için [api anahtarlarını yönetme](search-security-api-keys.md).

<a id="sys-info"></a>

## <a name="logging-and-system-information"></a>Günlüğe kaydetme ve sistem bilgileri
Portal ya da programlama arabirimleri aracılığıyla belirli bir hizmet için günlük dosyalarını Azure Search'ü kullanıma sunmuyor. Temel katmanında ve üzeri, Microsoft, tüm Azure Search Hizmetleri için % 99,9 kullanılabilirlik hizmet düzeyi sözleşmeleri (SLA) başına izler. Hizmeti yavaş veya istek üretilen işini SLA eşiğin altında düştüğünde, destek ekipleri kullanabilecekleri günlük dosyalarını gözden geçirin ve sorunu çözün.

Hizmetinizi hakkında genel bilgi açısından, aşağıdaki yollarla bilgi edinebilirsiniz:

* Portalda hizmet panosundaki, bildirimler, özellikler ve durum iletileri.
* Kullanarak [PowerShell](search-manage-powershell.md) veya [Yönetimi REST API'si](https://docs.microsoft.com/rest/api/searchmanagement/) için [hizmeti özelliklerini alma](https://docs.microsoft.com/rest/api/searchmanagement/services), ya da dizin kaynak kullanımını durumu.
* Aracılığıyla [trafik analizi arama](search-traffic-analytics.md), daha önce belirtildiği gibi.

<a id="sub-5"></a>

## <a name="monitor-resource-usage"></a>Kaynak kullanımını izleme
Panoda, kaynak izleme hizmet panosunu ve hizmet sorgulayarak edinebilirsiniz birkaç ölçümleri gösterilen bilgiler sınırlıdır. Kullanım bölümünde, hizmet Panosu üzerinde hızlı bir şekilde bölüm kaynak düzeylerini uygulamanız için uygun olup olmadığını belirleyebilirsiniz. Yakalayın ve günlüğe kaydedilen olayları kalıcı hale getirmek istiyorsanız Azure izleme gibi dış kaynaklar sağlayabilirsiniz. Daha fazla bilgi için [izleme Azure Search](search-monitor-usage.md).

Arama hizmeti REST API'si kullanarak, belgeler ve dizinlerde bir sayısına programlı bir şekilde alabilirsiniz: 

* [Dizin istatistiklerini alma](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)
* [Belge sayısı](https://docs.microsoft.com/rest/api/searchservice/count-documents)

## <a name="disaster-recovery-and-service-outages"></a>Olağanüstü durum kurtarma ve hizmet kesintilerine

Biz verilerinizi hurda olsa da, küme veya veri merkezi düzeyinde bir kesinti oluşursa, Azure Search hizmetinin anında yük devretme sağlamaz. Veri merkezinde bir küme başarısız olursa, operasyon ekibinin algılamak ve hizmetini geri yüklemek için çalışır. Hizmeti geri yükleme sırasında kapalı kalma süresi yaşar, ancak hizmet olarak kullanım dışı kalması için hizmet iadeleri isteyebilir [hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). 

Sürekli hizmet Microsoft'un kontrolü dışında yıkıcı hataları durumunda gerekliyse, verebilir [ek bir hizmet sağlama](search-create-service-portal.md) farklı bir bölge ve dizinleri emin olmak için bir coğrafi çoğaltma stratejisi olan uygulama tüm hizmetlerde tam yedekli.

Kullanan müşteriler [dizin oluşturucular](search-indexer-overview.md) doldurmak ve dizinleri yenilemek için aynı veri kaynağını yararlanarak coğrafi özel dizin oluşturucular aracılığıyla olağanüstü durum kurtarma başa çıkabilir. Çalıştıran her bir dizin oluşturucu, farklı bölgelerde iki hizmet coğrafi yedeklilik elde etmek için aynı veri kaynağından dizin. Ayrıca coğrafi olarak yedekli veri kaynaklarından sıralıyorsanız, artımlı birincil çoğaltmalardan dizin oluşturma, Azure Search dizin oluşturucularında yalnızca gerçekleştirebilir unutmayın. Bir yük devretme olayından içinde dizin oluşturucuyu yeniden yeni birincil çoğaltmaya işaret edecek şekilde emin olun. 

Dizin oluşturucular kullanmazsanız, uygulama kodunuz için anında iletme nesneleri ve veri farklı arama hizmetleri için paralel olarak kullanırsınız. Daha fazla bilgi için [performans ve iyileştirme Azure Search'te](search-performance-optimization.md).

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Azure Search birincil veri depolama çözümü olduğundan, Self Servis yedekleme ve geri yükleme için resmi bir mekanizma sunmuyoruz. Uygulama kodunuza oluşturup dizini doldurmak için kullanılan bir dizin yanlışlıkla silerseniz pratikte geri yükleme seçeneğidir. 

Bir dizini yeniden oluşturmak için (mevcut varsayılarak) silin, dizin hizmetinde yeniden oluşturun ve yeniden yükleyin, birincil veri deposundan verileri alarak.


<a id="scale"></a>

## <a name="scale-up-or-down"></a>Ölçek büyütme veya küçültme
Her arama hizmeti bir çoğaltma ve bir bölüm en düşük ile başlar. Kaydolup varsa bir [ayrılmış kaynaklar sağlayan katmanı](search-limits-quotas-capacity.md), tıklayın **ölçek** kaynak kullanımı ayarlamak için hizmet panosuna kutucuk.

Kapasite ya da kaynak aracılığıyla eklediğinizde, hizmet bunları otomatik olarak kullanır. Başka bir işlem yapmanız gereken, ancak yeni kaynak etkisini gerçekleşmeden önce bir gecikme olduğunu. 15 dakika veya ek kaynakları sağlamak için daha fazla sürebilir.

 ![][10]

### <a name="add-replicas"></a>Çoğaltmalar ekleyerek
Sorgular / saniye (QPS) artırmayı veya yüksek kullanılabilirlik elde etmek, çoğaltmalar ekleyerek gerçekleştirilir. Her yineleme, bir dizinin bir kopyasını olduğundan bir daha fazla çoğaltma ekleme için hizmet sorgu istekleri işlemek için daha fazla dizin çevirir. Yüksek kullanılabilirlik için en az 3 çoğaltma gereklidir (bkz [kapasite planlaması](search-capacity-planning.md) Ayrıntılar için).

Daha fazla çoğaltma içeren bir arama hizmeti çok sayıda dizin üzerinde Yük Dengeleme sorgu isteklerini yükleyebilirsiniz. Sorgu toplu düzeyini göz önünde bulundurulduğunda, sorgu aktarım hızı isteğe hizmet vermek kullanılabilir dizin daha fazla kopyasını olduğunda daha hızlı olacak. Sorgu gecikme yaşıyorsanız performans ek çoğaltmalar çevrimiçi olduktan sonra olumlu bir etkisi bekleyebilirsiniz.

Çoğaltmaları ekledikçe, sorgu aktarım hızı artar olsa da, bu kesin olmayan bir şekilde çift veya hizmetinize çoğaltmaları ekledikçe Üçlü desteklemez. Sorgu performansı üzerindeki impinge dış faktörler uygulamaları tüm ara tabidir. Karmaşık sorgular ve ağ gecikmesi farklılıklara sorgu yanıt süreleri katkıda bulunan iki faktörlerdir.

### <a name="add-partitions"></a>Bölüm Ekle
Çoğu hizmet uygulamaları bölümler yerine daha fazla çoğaltma için yerleşik bir ihtiyacı vardır. Daha fazla belge sayısı gerekli olduğu durumlarda, standart hizmet için kaydolduysanız bölümleri ekleyebilirsiniz. Temel katman, ek bölümler için sağlamaz.

Standart katmanında 12'ın katları şeklinde bölümler eklenir (özellikle, 1, 2, 3, 4, 6 veya 12). Bu parçalama bir yapıdır. Dizin, tüm 1 bölüme depolanan veya 2, 3, 4, 6 veya 12 bölüme (bölüm başına tek parça) eşit olarak bölünmüş 12 parçalardaki oluşturulur.

### <a name="remove-replicas"></a>Yinelemeleri Kaldır
Sonraki dönemler yüksek sorgu birimlerin (örneğin, tatil satış üzerinden sonra) arama sorgu yüklerini normalleştirilmiş sonra çoğaltmaları azaltmak için kaydırıcıyı kullanabilirsiniz. Başka bir adım bulunmanıza gerek yoktur. Çoğaltma sayısını azaltmayı veri merkezindeki sanal makineleri siler. Sorgu ve veri alma işlemlerinizi daha az sanal makinelerinden önce artık çalışmayacak. Bir yineleme en düşük gereksinimdir.

### <a name="remove-partitions"></a>Bölümlerini Kaldır
Çoğaltmalar, fazladan çaba sarf gerektiren kaldırma kullanılmasının, azaltılabilir çok daha fazla depolama alanı kullanıyorsanız yapmanız için biraz çalışmanız olabilir. Örneğin, üç bölüm çözümünüz kullanıyorsanız, yeni depolama alanı dizininizi barındırmak için gerekenden daha az ise bir veya iki bölüm downsizing bir hata oluşturur. Bekleyebileceğiniz gibi seçimlerinizi dizinlere veya boşaltın veya geçerli yapılandırmayı korumak için ilişkili bir dizin içindeki belgeler silmek üzeresiniz.

Hangi dizin parçalar belirli bölümleri üzerinde depolanan söyleyen algılama yöntemi yoktur. Sahip olduğunuz bölüm sayısı tarafından kullanılabilmesi için bir boyut için depolama azaltmak ihtiyacınız olacak şekilde her bölüm yaklaşık 25 GB depolama alanı sağlar. Bir birime geri dönmek istiyorsanız, tüm 12 parçalar sığması gerekir.

Gelecekteki planlamaya yardımcı olmak için depolama denetlemek isteyebilirsiniz (kullanarak [dizin istatistiklerini alma](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)) gerçekte ne kadar kullanıldığını görmek için. 

<a id="advanced-deployment"></a>

## <a name="best-practices-on-scale-and-deployment"></a>Ölçek ve dağıtım üzerinde en iyi yöntemler
Bu 30 dakikalık videoda, coğrafi olarak dağıtılmış iş yükleri de dahil, gelişmiş dağıtım senaryoları için en iyi uygulamaları gözden geçirir. Ayrıca bkz [performans ve iyileştirme Azure Search'te](search-performance-optimization.md) aynı noktalarını kapsayan Yardım sayfaları için.

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<a id="next-steps"></a>

## <a name="next-steps"></a>Sonraki adımlar
Hizmet Yönetimi Kavramları anladığınızda, kullanmayı [PowerShell](search-manage-powershell.md) görevlerini otomatikleştirmek için.

Ayrıca incelemeniz önerilir [performans ve iyileştirme makale](search-performance-optimization.md).

Başka bir önceki bölümde belirtilen video izlemek için önerilir. Bu, bu bölümünde belirtilen yöntemlerden birini daha ayrıntılı bilgi sağlar.

<!--Image references-->
[10]: ./media/search-manage/Azure-Search-Manage-3-ScaleUp.png



