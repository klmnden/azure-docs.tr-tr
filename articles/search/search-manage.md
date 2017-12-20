---
title: "Azure portalında Azure arama için Hizmet Yönetimi"
description: "Azure Search, Azure portalını kullanarak Microsoft Azure üzerinde barındırılan bulut arama hizmeti yönetin."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: c87d1fdd-b3b8-4702-a753-6d7e29dbe0a2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/09/2017
ms.author: heidist
ms.openlocfilehash: 916a08aacca428530bc4f728d5de422e04bed8bc
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="service-administration-for-azure-search-in-the-azure-portal"></a>Azure portalında Azure arama için Hizmet Yönetimi
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Azure Search zengin arama deneyimi özel uygulamalara oluşturmak için kullanılan tam olarak yönetilen, bulut tabanlı arama hizmetidir. Bu makalede yer almaktadır *Hizmet Yönetim* nda gerçekleştirebileceğiniz görevler [Azure portal](https://portal.azure.com) zaten sağlanmış bir arama hizmeti için. *Hizmet Yönetimi* basit tasarım, aşağıdaki görevler için sınırlı olduğu:

* Yönetme ve güvenliğini erişimi *api anahtarlarından* okuma veya yazma erişimi hizmetiniz için kullanılır.
* Hizmet kapasite, bölümler ve çoğaltmalar değiştirerek ayarlayın.
* Hizmet katmanının maksimum sınırlarına göre kaynak kullanımı izleyin.

Dikkat *yükseltme* bir yönetim görevi olarak listelenmemiş. Hizmet sağlandığında kaynaklar için farklı bir katmana taşıma yeni bir hizmet gerektirir. Ayrıntılar için bkz [bir Azure Search hizmeti oluşturma](search-create-service-portal.md).

> [!Tip]
> Arama trafiği veya sorgu performansını analiz etme konusunda yardım için mi arıyorsunuz? Arama terimleri kişiler, sorgu birim ve nasıl başarılı arama sonuçları kazanç Öngörüler dizininizdeki belirli belgelere müşteriler yönlendirmede ' dir. Yönergeler için bkz [Azure arama için arama trafiği analiz](search-traffic-analytics.md), [izlemek kullanım ve sorgu ölçümleri](search-monitor-usage.md), ve [performans ve en iyi duruma getirme](search-performance-optimization.md).

<a id="admin-rights"></a>

## <a name="administrator-rights"></a>Yönetici hakları
Sağlama veya hizmet yetkisini bir Azure Abonelik Yöneticisi veya ortak yönetici tarafından yapılabilir.

İçinde bir hizmet, hizmet URL'sini ve yönetici api anahtarı için erişimi olan herkes hizmet okuma-yazma erişimi vardır. Okuma-yazma erişimi eklemek, silmek veya API anahtarları, dizinler, dizin oluşturucular, veri kaynakları, zamanlamaları ve rol atamalarını aracılığıyla uygulanan dahil olmak üzere sunucu nesneleri değiştirme olanağı sağlar [RBAC tanımlı rolleri](#rbac).

Azure Search tüm kullanıcı etkileşimi bu modlarından birini içinde döner: okuma-yazma erişimi (yönetici hakları) hizmetine veya salt okunur erişim hizmetine (sorgu hakları). Daha fazla bilgi için bkz: [API anahtarlarını Yönet](#manage-keys).

<a id="sys-info"></a>

## <a name="set-rbac-roles-for-administrative-access"></a>Yönetici erişimi için RBAC rolleri Ayarla
Azure sağlayan bir [genel rol tabanlı yetkilendirme modelini](../active-directory/role-based-access-control-configure.md) tüm hizmetler için yönetilen portalı veya Resource Manager API'leri. Sahibi, katkıda bulunan ve okuyucu rolleri Hizmet Yönetimi Active Directory Kullanıcıları, grupları ve her role atanmış güvenlik sorumluları için düzeyini belirler. 

Azure arama için RBAC izinleri aşağıdaki yönetim görevleri belirler:

| Rol | Görev |
| --- | --- |
| Sahip |Oluşturun veya hizmet veya hizmetinde, API anahtarları, dizinler, dizin oluşturucular, Dizin Oluşturucu veri kaynakları ve dizin oluşturucu zamanlamaları gibi herhangi bir nesnede silin.<p>Sayıları ve depolama boyutu da dahil olmak üzere hizmetin durumunu görüntüleyin.<p>Ekleyin veya rol üyeliğini (yalnızca bir sahibi rol üyeliğini yönetebilir) silin.<p>Abonelik yöneticileri ve hizmet sahiplerini otomatik üyelik sahipleri rolüne sahiptir. |
| Katılımcı |Aynı düzeyde erişim sahibi, RBAC rol yönetimi eksi. Örneğin, katılımcı yeniden ve görüntüleyebilirsiniz `api-key`, rol üyeliklerini değiştiremez ancak. |
| Okuyucu |Hizmet durumu ve sorgu anahtarları görüntüleyin. Bu rolün üyeleri hizmet yapılandırmasında değişiklik yapamaz veya yönetici anahtarları görüntüleyebilirsiniz. |

Rol Hizmeti uç noktası erişim hakkı vermeyin. Arama hizmeti işlemleri, dizin yönetimi, dizin oluşturma ve sorgular gibi arama verileri api anahtarlarından, değil rolleri denetlenir. Daha fazla bilgi için bkz: "Yetkilendirmesi Yönetimi karşılık veri işlemleri için" [rol tabanlı erişim denetimi nedir](../active-directory/role-based-access-control-what-is.md).

<a id="secure-keys"></a>
## <a name="logging-and-system-information"></a>Günlüğe kaydetme ve sistem bilgileri
Azure arama günlük dosyaları için bir bireysel hizmet ya da portalı üzerinden veya programlama arabirimleri sağlamıyor. Temel katman ve üstünde, Microsoft Azure Search hizmetler için hizmet düzeyi sözleşmelerine (SLA) göre % 99,9 kullanılabilirliğini izler. Hizmeti yavaş veya istek işleme SLA eşiklerin altına düştüğünde, destek ekiplerini kullanabilecekleri günlük dosyalarını gözden geçirin ve sorunu gidermeye.

Hizmet hakkında genel bilgi açısından, aşağıdaki yollarla bilgi elde edebilirsiniz:

* Portalda hizmet panosunda, bildirimleri, özellikler ve durum iletileri.
* Kullanarak [PowerShell](search-manage-powershell.md) veya [Yönetimi REST API](https://docs.microsoft.com/rest/api/searchmanagement/) için [hizmeti özelliklerini alma](https://docs.microsoft.com/rest/api/searchmanagement/services), veya dizin kaynak kullanımına durumu.
* Aracılığıyla [trafiği analytics arama](search-traffic-analytics.md), daha önce belirtildiği gibi.

<a id="manage-keys"></a>

## <a name="manage-api-keys"></a>API anahtarları Yönet
Bir arama hizmeti için tüm istekleri özellikle hizmetinizin api oluşturulan anahtarı gerekir. Bu API anahtarı, Arama Hizmeti uç noktanızı erişimi kimlik doğrulaması için tek bir mekanizmadır. 

Bir API anahtarı rastgele oluşturulmuş sayılar ve harflerden oluşan bir dizedir. Aracılığıyla [RBAC izinleri](#rbac), silebilir veya anahtarları okumak, ancak kullanıcı tanımlı bir parola ile bir anahtar değiştirilemiyor. 

İki tür anahtarları, arama hizmetinize erişmek için kullanılır:

* Yönetici (hizmet karşı herhangi bir okuma-yazma işlemi için geçerli)
* Sorgu (geçerli bir dizin sorgular gibi salt okunur işlemler için)

Hizmet sağladığında yönetici api anahtarı oluşturulur. Olarak belirtilen iki yönetici anahtarları vardır *birincil* ve *ikincil* bunları tutmak için doğrudan, ancak aslında bunlar birbirinin yerine kullanılabilir. Böylece, bir hizmete erişimi kaybetmeden dönebilirsiniz her hizmetin iki yönetici anahtarları vardır. Her iki yönetici anahtarını yeniden oluşturmak, ancak toplam yönetici anahtar sayısı ekleyemezsiniz. En fazla arama hizmeti başına iki yönetici anahtarları yoktur.

Sorgu anahtarları arama doğrudan çağıran istemci uygulamalar için tasarlanmıştır. En fazla 50 sorgu anahtarları oluşturabilirsiniz. Uygulama kodunda arama URL'sini ve bir sorgu api anahtarını hizmet salt okunur erişime izin vermek için belirtin. Uygulama kodunuz, ayrıca, uygulamanız tarafından kullanılan dizini belirtir. Birlikte, uç noktası, salt okunur erişim için bir API anahtarı ve bir hedef dizin istemci uygulamanızı bağlantısından kapsam ve erişim düzeylerini tanımlayın.

Almak veya api anahtarlarından yeniden oluşturmak için hizmet panosunu açın. Tıklatın **ANAHTARLARI** kaydırarak açmak Anahtar Yönetimi sayfasında. Yeniden veya anahtarlar oluşturmak için sayfanın en üstünde komutlardır. Varsayılan olarak, yalnızca yönetici anahtarları oluşturulur. Sorgu api anahtarlarından el ile oluşturulması gerekir.

 ![][9]

<a id="rbac"></a>

## <a name="secure-api-keys"></a>Güvenli API anahtarları
Anahtar güvenlik portal ya da Resource Manager arabirimleri (PowerShell veya komut satırı arabirimi) aracılığıyla erişimi kısıtlayarak sağlamış. Belirtildiği gibi abonelik yöneticileri görüntülemek ve tüm API anahtarlarını yeniden oluştur. Önlem olarak, yönetici anahtarlarına kimlerin erişebileceğini anlamak için rol atamalarını gözden geçirin.

1. Hizmet panosunda kaydırarak açmak kullanıcılar dikey erişim simgesine tıklayın.
   ![][7]
2. Kullanıcılar, var olan rol atamalarını gözden geçirin. Beklendiği gibi abonelik yöneticileri tam erişime sahip rolünü aracılığıyla hizmetine zaten sahiptir.
3. Daha fazla ayrıntıya için tıklatın **abonelik yöneticileri** genişletin ve ardından arama hizmetinizde ortak yönetim haklarına sahip kişileri görmek için rol ataması listesi.

Erişim izinleri görüntülemek için başka bir yolu **rolleri** kullanıcılar dikey. Bunun yapılması, kullanılabilir roller ve kullanıcıların veya grupların her role atanmış sayısını görüntüler.

<a id="sub-5"></a>

## <a name="monitor-resource-usage"></a>Kaynak kullanımını izleme
Panosunda, kaynak izleme hizmet panosunu ve hizmet sorgulayarak edinebilirsiniz birkaç ölçümleri gösterilen bilgiler sınırlıdır. Hizmet panosunda kullanım bölümünde, hızlı bir şekilde bölüm kaynak düzeyleri, uygulamanız için yeterli olup olmadığını belirleyebilirsiniz.

Search Hizmeti REST API'sini kullanarak, belgeler ve dizin sayısına program aracılığıyla alabilirsiniz: 

* [Dizin istatistikleri Al](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)
* [Count belgeleri](https://docs.microsoft.com/rest/api/searchservice/count-documents)

## <a name="disaster-recovery-and-service-outages"></a>Olağanüstü durum kurtarma ve hizmet kesintilerine karşı

Verilerinizi hurda ancak küme veya veri merkezi düzeyinde bir kesinti durumunda Azure Search hizmetinin anlık yük devretme sağlamaz. Veri merkezinde bir küme başarısız olursa, işletim ekibi algılamak ve hizmet geri için çalışır. Hizmet geri yükleme sırasında kapalı kalma yaşayacaktır. Hizmet kullanılamazlık dengelemek için hizmet iadeleri isteyebilir [hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). 

Hizmetin sürekli Microsoft'un denetimi dışında yıkıcı hataları durumunda gerekliyse, verebilir [ek bir hizmet sağlamak](search-create-service-portal.md) farklı bir bölgeye ve dizinleri emin olmak için coğrafi çoğaltma stratejisiyle gerçekleştir Tüm hizmetler arasında tam yedekli.

Kullanan müşteriler [dizin oluşturucular](search-indexer-overview.md) doldurmak ve dizinleri yenilemek için olağanüstü durum kurtarma aynı veri kaynağı yararlanarak coğrafi özgü dizin oluşturucular aracılığıyla işleyebilir. Farklı bölgelerde, her bir dizin oluşturucu çalıştıran iki Hizmetleri coğrafi yedeklilik sağlamak için aynı veri kaynağından dizin. Ayrıca coğrafi olarak yedekli veri kaynaklarından sıralıyorsanız, artımlı birincil çoğaltmalardan dizin oluşturma, Azure Search'te dizin oluşturucular yalnızca gerçekleştirebilir unutmayın. Bir yük devretme olayından dizin oluşturucu yeni birincil çoğaltma yeniden işaret edecek şekilde emin olun. 

Dizin oluşturucular kullanmazsanız, uygulama kodunuz itme nesnelere ve farklı arama hizmetleri verileri paralel olarak kullanırsınız. Daha fazla bilgi için bkz: [performans ve Azure Search'te iyileştirme](search-performance-optimization.md).

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Azure Search birincil veri depolama çözümü olduğundan, biz Self Servis yedekleme ve geri yükleme için resmi bir mekanizma sağlamaz. Uygulama kodunuz oluşturmak ve bir dizinini doldurmak için kullanılan bir dizin yanlışlıkla silerseniz gerçek geri yükleme seçeneğidir. 

Bir dizini yeniden oluşturmak için (var olduğu varsayılarak) silin, dizin hizmetinde yeniden oluşturun ve birincil veri deposundan verileri alarak yeniden. Alternatif olarak, size ulaşmak [müşteri desteği]() bölgesel bir kesintinin ise hurda dizinler için.


<a id="scale"></a>

## <a name="scale-up-or-down"></a>Ölçek büyütme veya küçültme
Her arama hizmeti en az bir çoğaltma ve bir bölüm ile başlar. İçin kaydolduysanız bir [özel kaynakları sağlar katmanı](search-limits-quotas-capacity.md), tıklatın **ölçek** döşeme kaynak kullanımı ayarlamak için hizmet panosunda.

Hizmet ya da kaynak aracılığıyla kapasite eklediğinizde, bunları otomatik olarak kullanır. Başka bir eylem yapmanız gereken, ancak yeni kaynak etkisini gerçekleşen önce kısa bir gecikme olduğunu. 15 dakika veya ek kaynakları sağlamak üzere daha fazla sürebilir.

 ![][10]

### <a name="add-replicas"></a>Çoğaltmaları ekleme
Sorgular (QPS) saniyede artırmayı veya yüksek kullanılabilirlik elde çoğaltmaları ekleyerek yapılır. Her yineleme bir kopyasını bir dizin sahiptir, bu nedenle bir daha fazla çoğaltma eklemek için daha fazla dizin hizmeti sorgu isteği işlemek için kullanılabilen çevirir. Yüksek kullanılabilirlik için en az 3 çoğaltmaları gereklidir (bkz [kapasite planlaması](search-capacity-planning.md) Ayrıntılar için).

Daha fazla çoğaltmaları sahip bir arama hizmeti çok sayıda dizinleri Yük Dengeleme sorgu isteklerini yükleyebilirsiniz. Sorgu toplu düzeyini verildiğinde, sorgu işleme isteklerine hizmet dizin fazla kopyasını olduğunda daha hızlı olacak. Sorgu gecikmesi karşılaşıyorsanız, Ek çoğaltmalar çevrimiçi olduktan sonra performansını olumlu bir etki bekleyebilirsiniz.

Çoğaltmaları ekledikçe sorgu işleme artar rağmen onu değil tam olarak çift veya hizmetinize çoğaltmaları ekledikçe Üçlü. Sorgu performansına impinge dış etkenler tüm arama uygulamaları tabidir. Karmaşık sorgular ve ağ gecikmesi için değişimler sorgusu yanıt sürelerini katkıda bulunan iki faktörlerdir.

### <a name="add-partitions"></a>Bölüm Ekle
Çoğu hizmet uygulamaları bölümleri yerine daha fazla çoğaltmaları yerleşik ihtiyacı vardır. Artan belge sayısı gerekli olduğu durumlarda, standart hizmet için kaydolduysanız bölümleri ekleyebilirsiniz. Temel katman, ek bölümler için sağlamaz.

Standart katmanı bölümler 12'ün katları eklenir (özellikle, 1, 2, 3, 4, 6 veya 12). Bu bir parçalama yapıdır. Tüm yüklenebilir 1 bölüme depolanan veya 2, 3, 4, 6 ve 12 bölümleri (bölüm başına tek parça) eşit olarak bölünmüş 12 parça bir dizin oluşturulur.

### <a name="remove-replicas"></a>Çoğaltmaları Kaldır
Sonra nokta yüksek sorgu birimlerin (örneğin, tatil satış üzerinden sonra) arama sorgu yüklerinin normalleştirilmiş sonra çoğaltmaları azaltabilir.

Bunu yapmak için daha düşük bir sayı geri çoğaltma kaydırıcıyı taşıyın. Yapmanız gereken başka bir adım vardır. Yineleme sayısını azaltmayı veri merkezindeki sanal makineleri siler. Sorgu ve veri alımı işlemlerinizin artık vm'lerinde daha az önce çalışır. Alt sınır bir çoğaltmadır.

### <a name="remove-partitions"></a>Bölümlerini Kaldır
Çoğaltmalar, hiçbir ek çaba çaba gerektiren kaldırma kullanılmasının aksine, azaltılabilir olandan daha fazla depolama alanı kullanıyorsanız yapmak için bazı iş olabilir. Örneğin, üç bölüm çözümünüz kullanıyorsanız, yeni depolama alanı gerekenden daha az ise downsizing bir veya iki bölüm için bir hata oluşturur. Beklediğiniz gibi seçimlerinizi dizinleri ve belgeler boşaltın ya da geçerli yapılandırmayı korumak için ilişkili bir dizin içinde silmek üzeresiniz.

Hangi dizin parça belirli bölümlerinde depolanan belirten algılama yöntemi yoktur. Sahip olduğunuz bölüm sayısı tarafından kullanılabilmesi için bir boyut için depolama alanını azaltmasını gerekir böylece her bölüm yaklaşık 25 GB depolama sağlar. Bir birime geri dönmek istiyorsanız, tüm 12 parça sığması gerekir.

Gelecekteki planlamaya yardımcı olmak için depolama denetlemek isteyebilirsiniz (kullanarak [dizin istatistikleri almak](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)) ne kadar gerçekten kullandığınız görmek için. 

<a id="advanced-deployment"></a>

## <a name="best-practices-on-scale-and-deployment"></a>Ölçek ve dağıtım üzerinde en iyi yöntemler
Bu 30 dakikalık videoda, coğrafi olarak dağıtılan iş yükleri de dahil olmak üzere gelişmiş dağıtım senaryoları için en iyi uygulamaları gözden geçirir. Ayrıca bkz [performans ve Azure Search'te iyileştirme](search-performance-optimization.md) aynı noktaları kapak Yardım sayfaları için.

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<a id="next-steps"></a>

## <a name="next-steps"></a>Sonraki adımlar
Hizmet Yönetimi Kavramları anladığınızda, kullanmayı [PowerShell](search-manage-powershell.md) görevleri otomatik hale getirmek için.

Ayrıca gözden geçirme öneririz [performans ve en iyi duruma getirme makale](search-performance-optimization.md).

Başka bir önceki bölümünde belirtildiği videoyu izlemek için önerilir. Bu bölümde belirtilen teknikleri daha ayrıntılı bilgi sağlar.

<!--Image references-->
[7]: ./media/search-manage/rbac-icon.png
[8]: ./media/search-manage/Azure-Search-Manage-1-URL.png
[9]: ./media/search-manage/Azure-Search-Manage-2-Keys.png
[10]: ./media/search-manage/Azure-Search-Manage-3-ScaleUp.png



