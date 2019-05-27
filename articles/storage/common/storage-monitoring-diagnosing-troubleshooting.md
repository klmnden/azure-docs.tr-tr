---
title: İzleme, tanılama ve Azure depolama sorunlarını giderme | Microsoft Docs
description: Depolama analizi, istemci tarafı günlüğe kaydetme ve diğer üçüncü taraf araçları tanımlamak için tanılama ve Azure depolama ile ilgili sorunları giderme gibi özellikleri kullanın.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/11/2017
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: ccafa3431e12b036346c4fd654b2978dc9021471
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65912459"
---
# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Microsoft Azure Storage izleme, tanılama ve sorun giderme
[!INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Genel Bakış
Bir bulut ortamında barındırılan dağıtılmış bir uygulamadaki sorunlarını giderme ve Tanılama, geleneksel ortamlarda daha karmaşık olabilir. Uygulamaları, şirket içi mobil cihaz veya bu ortamların birleşiminde bir PaaS veya Iaas altyapısında dağıtılabilir. Genellikle, uygulamanızın trafiği ortak ve özel ağlar geçiş yapabilir ve uygulamanızı Microsoft Azure depolama tabloları, BLOB'lar, kuyruklar gibi birden çok depolama teknolojilerini kullanabilir veya diğer veri yanı sıra dosyalarını depolar gibi gibi ilişkisel ve belge veritabanları.

Bu tür uygulamalar başarılı bir şekilde yönetmek için proaktif izleme ve tanılama ve sorun giderme bunları ve bunların bağımlı teknolojiler tüm yönlerini anlamak gerekir. Bir Azure Depolama Hizmetleri kullanıcı olarak, sürekli olarak beklenmeyen değişiklikler (örneğin normal yanıt süreleri daha yavaş) davranışı için uygulamanızın kullandığı depolama hizmetlerini izleme ve ayrıntılı verileri toplamak ve olası bir sorunu çözümlemek için günlük kaydı kullanma Derinlik. İzleme hem oturum elde tanılama bilgileri, uygulamanızın karşılaşılan sorunun kök nedenini belirlemek için yardımcı olur. Ardından sorunu gidermek ve çözmek için gerçekleştirebileceğiniz uygun adımları belirlemek. Azure depolama, bir çekirdek Azure hizmeti olan ve müşterileri Azure altyapınıza dağıtım çözümlerinin çoğu önemli bir parçasını oluşturur. Azure depolama izleme, tanılama ve depolama sorunlarını bulut tabanlı uygulamalarınıza basitleştirmek için özellikleri içerir.

> [!NOTE]
> Azure dosyaları şu anda kaydını desteklemiyor.
>

Uçtan uca Azure depolama uygulamalarda sorun giderme uygulamalı kılavuzu için bkz. [için uçtan uca Azure depolama ölçümlerini ve günlüğe kaydetme, AzCopy ve ileti Çözümleyicisi'ni kullanarak sorun giderme](../storage-e2e-troubleshooting.md).

* [Giriş]
  * [Bu kılavuz nasıl düzenlenir]
* [Depolama Hizmeti izleme]
  * [Hizmet durumu izleme]
  * [İzleme kapasitesi]
  * [Kullanılabilirliği izleme]
  * [Performans izleme]
* [Depolama sorunları tanılama]
  * [Hizmet sistem durumu sorunları]
  * [Performans sorunları]
  * [Hataları tanılama]
  * [Depolama öykünücüsü sorunları]
  * [Depolama günlük araçları]
  * [Ağ günlük araçları kullanma]
* [Uçtan uca izleme]
  * [Günlük verileri ilişkilendirme]
  * [İstemci istek kimliği]
  * [Sunucu istek kimliği]
  * [Zaman damgaları]
* [Sorun giderme rehberi]
  * [Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor]
  * [Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor, ancak istemci yüksek gecikme durumu yaşıyor]
  * [Ölçümler yüksek AverageServerLatency gösteriyor]
  * [Kuyrukta ileti tesliminde beklenmeyen gecikmeler yaşıyorsunuz]
  * [Ölçümler PercentThrottlingError’da artış gösteriyor]
  * [Ölçümler PercentTimeoutError’da artış gösteriyor]
  * [Ölçümler PercentNetworkError’da artış gösteriyor]
  * [İstemci HTTP 403 (Yasak) iletilerini alıyor]
  * [İstemci HTTP 404 (Bulunamadı) iletilerini alıyor]
  * [İstemci HTTP 409 (Çakışma) iletileri alma]
  * [Düşük PercentSuccess ölçümleri göster veya ClientOtherErrors işlem durumundaki işlemlerini analytics günlük girdilerine sahip]
  * [Kapasite ölçümlerini beklenmeyen artışı depolama kapasitesi kullanımı Göster]
  * [Sorununuzu geliştirme veya test için depolama öykünücüsünü kullanarak ortaya çıkar.]
  * [.NET için Azure SDK'sını yüklerken sorunlarla karşılaşıyoruz.]
  * [Bir depolama hizmetindeki farklı bir sorun olması]
  * [Windows sanal makinelerinde VHD'ler sorunlarını giderme](../../virtual-machines/windows/troubleshoot-vhds.md)   
  * [Linux sanal makinelerinde VHD'ler sorunlarını giderme](../../virtual-machines/linux/troubleshoot-vhds.md)
  * [Windows Azure dosyaları sorunlarını giderme](../files/storage-troubleshoot-windows-file-connection-problems.md)   
  * [Linux ile Azure dosyaları sorunlarını giderme](../files/storage-troubleshoot-linux-file-connection-problems.md)
* [Ekler]
  * [Ek 1: HTTP ve HTTPS trafiğini yakalamak için Fiddler'ı kullanma]
  * [Ek 2: Ağ trafiğini yakalamak için Wireshark kullanma]
  * [Ek 3: Ağ trafiğini yakalamak için Microsoft Message Analyzer'ı kullanma]
  * [Ek 4: Ölçümleri görüntüleyin ve verileri günlüğe kaydetmek için Excel kullanma]
  * [Ek 5: Azure DevOps için Application Insights ile izleme]

## <a name="introduction"></a>Giriş
Bu kılavuz, Azure depolama analizi gibi özelliklerin nasıl kullanılacağını göstermektedir istemci-tarafı Azure depolama istemci kitaplığı ve diğer üçüncü taraf araçları tanımlamak, tanılamak ve Azure depolama sorunlarını giderme için günlük kaydı ile ilgili sorunlar.

![][1]

Bu kılavuz öncelikli olarak geliştiriciler, Azure depolama hizmetleri ve BT uzmanları gibi çevrimiçi hizmetlere yönetmekten sorumlu kullanan Çevrimiçi Hizmetleri tarafından okunacak yöneliktir. Bu kılavuzun hedefleri şunlardır:

* Sistem durumunu ve performansını Azure depolama hesaplarınızı sürdürmenize yardımcı olmak için.
* Bir uygulamada sorun veya sorununuz Azure Depolama'ya ilgili karar vermenize yardımcı olacak araçlar ve gerekli işlemleri sağlamak için.
* Azure Depolama'ya ilgili sorunları çözmek için eyleme dönüştürülebilir rehberlik sağlamak için.

### <a name="how-this-guide-is-organized"></a>Bu kılavuz nasıl düzenlenir
Bölüm "[Depolama Hizmeti izleme]" sistem durumu ve Azure Storage Analytics ölçümleri (depolama ölçümleri) kullanarak, Azure depolama hizmetleri performansını izleme açıklar.

Bölüm "[depolama sorunları tanılama]" Azure depolama analizi günlüğe kaydetme (depolama oturum açma) kullanarak sorunlarının nasıl tanılandığını açıklar. Ayrıca, .NET veya Java için Azure SDK'sı depolama istemci kitaplığı gibi özellikleri, istemci kitaplıklarından birini kullanarak istemci tarafı günlüğe kaydetmenin nasıl etkinleştirileceği açıklanır.

Bölüm "[uçtan uca izleme]" çeşitli günlük dosyalarını ve ölçüm verilerini yer alan bilgileri nasıl ilişkilendirebilmek açıklanmaktadır.

Bölüm "[sorun giderme rehberi]" bazı depolama ile ilgili yaygın sorunları için sorun giderme rehberi çalıştırdığınızca sağlar.

"[Ekler]" Çözümleme ağ paket verileri, HTTP/HTTPS iletilerinin analiz etmek için fiddler'ı ve Microsoft Message Analyzer'ı ilişkilendirmek için günlük verilerini Wireshark ve Netmon gibi diğer araçları kullanma hakkında bilgi içerir.

## <a name="monitoring-your-storage-service"></a>Depolama Hizmeti izleme
Windows performans izleme ile bilginiz varsa, Windows Performans İzleyicisi sayaçları Azure depolama eşdeğer olacak şekilde depolama ölçümlerini düşünebilirsiniz. ' De depolama ölçümleri, ölçümleri (terminolojisinde Windows Performans İzleyicisi sayaçları) hizmet kullanılabilirliği, hizmet isteklerinin toplam sayısı veya hizmetine başarılı isteklerin yüzdesi gibi kapsamlı bir dizi bulacaksınız. Kullanılabilir ölçümler tam bir listesi için bkz. [Storage Analytics Ölçüm tablosu şeması](https://msdn.microsoft.com/library/azure/hh343264.aspx). Depolama hizmetinin toplamak ve saatte veya dakikada ölçümleri toplamak isteyip istemediğinizi belirtebilirsiniz. Ölçümleri etkinleştirme ve depolama hesaplarınızı izlemek hakkında daha fazla bilgi için bkz. [depolama ölçümlerini etkinleştirme ve ölçüm verilerini görüntüleme](https://go.microsoft.com/fwlink/?LinkId=510865).

Görüntülemek istediğiniz hangi saatlik ölçümlerini seçebilirsiniz [Azure portalında](https://portal.azure.com) ve saatlik bir ölçüm belirli bir eşiği aştığında, yöneticiler e-posta ile bildirim kurallarını yapılandırın. Daha fazla bilgi için [uyarı bildirimleri alma](/azure/monitoring-and-diagnostics/monitoring-overview-alerts).

Depolama hizmeti kullanarak bir en iyi çaba ölçümleri toplar, ancak her depolama işlemi kayıt.

Azure portalında ölçümler bir depolama hesabı için ortalama gecikme rakamları kullanılabilirlik ve toplam istek sayısı gibi görüntüleyebilirsiniz. Bir bildirim kuralı da kullanılabilirlik belirli bir düzeyde düşerse bir yöneticiyi uyarmak için ayarlanmış olan. Bu verileri görüntülemesini, araştırma için bir olası alan % 100 olan tablo hizmeti başarı yüzdesi olan (daha fazla bilgi için bkz "[Düşük PercentSuccess ölçümleri göster veya ClientOtherErrors işlem durumundaki işlemlerini analytics günlük girdilerine sahip]").

Ayrıca, sağlıklı ve tarafından beklendiği gibi gerçekleştirme olduklarından emin olmak için Azure uygulamalarınızın sürekli olarak izlemeniz gerekir:

* Geçerli verileri karşılaştırmak ve Azure depolama ve uygulamanızın davranışını, önemli değişiklikler belirlemenize olanak tanıyan bir uygulama için bazı temel ölçümleri ayarlanıyor. Temel ölçümleriniz değerlerini çoğu durumda, uygulamaya özgü hale gelir ve performans testi uygulamanız olduğunda bunların oluşturmanız gerekir.
* Dakika ölçümlerini kaydetme ve beklenmeyen hataları ve hata artış gibi anormallikleri için etkin şekilde izlemek için kullanmadan sayıları veya istek hızları.
* Saatlik ölçümlerini kaydetme ve ortalama değerler gibi izlemek üzere onları kullanmasına ortalama hata sayılarını ve istek hızları.
* Sonraki bölümde açıklandığı gibi tanılama araçlarını kullanarak olası sorunları araştırma "[depolama sorunları tanılama]."

Aşağıdaki görüntüde grafikleri nasıl ortalaması için saatlik ölçümlerini gerçekleşen ani etkinlik gizleyebilirsiniz göstermektedir. İsteklerin bir hızda gerçekleşen gerçekten dalgalanmaları ölçümleri Göster dakika göstermek için saatlik ölçümlerini görünür.

![][3]

Bu bölümün geri kalanında izlemeniz hangi ölçümleri açıklar ve neden.

### <a name="monitoring-service-health"></a>Hizmet durumu izleme
Kullanabileceğiniz [Azure portalında](https://portal.azure.com) dünyanın dört bir yanındaki tüm Azure bölgelerinde depolama hizmeti (ve diğer Azure Hizmetleri) durumunu görüntülemek için. İzleme etkinleştirir, bir sorun varsa denetiminiz dışında hemen görmek için uygulamanız için kullandığınız bölgede depolama hizmeti etkiliyor.

[Azure portalında](https://portal.azure.com) çeşitli Azure hizmetlerini etkileyen olayların bildirimleri de sağlayabilirsiniz.
Not: Bu bilgiler bunların birlikte çalışarak geçmiş verileri, üzerinde önceden kullanılabilen [Azure hizmet Panosu](https://status.azure.com).

Sırada [Azure portalında](https://portal.azure.com) sistem bilgilerini toplar gelen (Inside out izleme), Azure veri merkezleri içinde ayrıca erişim düzenli aralıklarla yapay işlemler oluşturmak için bir dışarıdan içeriye yaklaşımı benimsemeyi göz önünde Azure'da barındırılan web uygulamanız birden fazla konumdan. Tarafından sunulan hizmetler [Dynatrace](https://www.dynatrace.com/en/synthetic-monitoring) ve Azure DevOps için Application Insights bu yaklaşım bir örnektir. Ek Azure DevOps için Application Insights hakkında daha fazla bilgi için bkz. "[ek 5: Azure DevOps için Application Insights ile izleme](#appendix-5). "

### <a name="monitoring-capacity"></a>İzleme kapasitesi
Depolama ölçümleri bloblar genellikle büyük bir oranı, depolanan verilerin hesabı için blob hizmeti için kapasite ölçümlerini yalnızca depolar (makalenin yazıldığı sırada, tablolar ve Kuyruklar kapasitesini izlemek için depolama ölçümlerini kullanmak mümkün değildir). Bu verileri bulabilirsiniz **$MetricsCapacityBlob** Blob hizmeti için izleme etkinleştirilirse tablo. Depolama ölçümlerini, bu verileri günde bir kez kaydeder ve değerini kullanabilir **RowKey** satırının kullanıcı verileri için ilişkili varlık içerip içermediğini belirlemek için (değer **veri**) veya Analiz verilerini (değer **analytics**). Depolanan her varlık, kullanılan depolama miktarı hakkında bilgi içerir (**kapasite** bayt cinsinden ölçülür) ve kapsayıcılar geçerli sayısı (**ContainerCount**) ve bloblar (**ObjectCount** ) depolama hesabının kullanımda. Depolanan kapasite ölçümleri hakkında daha fazla bilgi için **$MetricsCapacityBlob** tablo bkz [Storage Analytics Ölçüm tablosu şeması](https://msdn.microsoft.com/library/azure/hh343264.aspx).

> [!NOTE]
> Bu değerler, depolama hesabının kapasite sınırları yaklaştığı erken bir uyarı için izlemeniz gerekir. Azure portalında, toplam depolama kullanımı aşıyor veya belirttiğiniz eşiğin altında düştüğünde size bildirmek için uyarı kuralları ekleyebilirsiniz.
>
>

Yardım için blog gönderisine bakın blobları gibi çeşitli depolama nesnelerin boyutunu tahmin etme [anlama Azure depolama Faturalaması – bant genişliği, işlemler ve kapasite](https://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

### <a name="monitoring-availability"></a>Kullanılabilirliği izleme
Depolama Hizmetleri kullanılabilirliğini değeri izleyerek, depolama hesabınızdaki izlemeniz gerekir **kullanılabilirlik** saat veya dakika ölçümlerini tablolarda sütun — **$MetricsHourPrimaryTransactionsBlob** , **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$MetricsMinutePrimaryTransactionsBlob** , **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$MetricsCapacityBlob**. **Kullanılabilirlik** sütunu içeriyor hizmeti veya satırın tarafından temsil edilen API işlemi kullanılabilirliğini gösteren bir yüzde değeri ( **RowKey** ölçümler için satır bulunup bulunmadığını gösterir. Hizmet bir bütün olarak veya belirli bir API işlemi).

% 100'değerinden küçük bir değer bazı depolama istekler başarısız olduğunu gösterir. Neden bunlar gibi farklı hata türleri ile istek sayılarını gösteren ölçüm verilerini diğer sütunları inceleyerek başarısız olduğunu gördüğünüz **ServerTimeoutError**. Görmeyi beklemelisiniz **kullanılabilirlik** fall geçici olarak hizmet sırasında geçici bir sunucu zaman aşımları gibi nedenlerle %100 taşır bölümleri daha iyi Yük Dengeleme isteğine; istemci uygulamanıza yeniden deneme mantığı gerekir aralıklı böylesi işleyin. Makaleyi [depolama analizi günlüğe yazılan işlemler ve durum iletileri](https://msdn.microsoft.com/library/azure/hh343260.aspx) depolama ölçümlerini içeren işlem türleri listeler, **kullanılabilirlik** hesaplama.

İçinde [Azure portalında](https://portal.azure.com), olmadığını bildirmek için uyarı kuralları ekleyebilirsiniz **kullanılabilirlik** için bir hizmet, belirttiğiniz bir eşiğin altına düşüyor.

"[Sorun giderme rehberi]" başlığına kullanılabilirlikle ilgili bazı yaygın depolama hizmeti sorunlar açıklanmaktadır.

### <a name="monitoring-performance"></a>Performans izleme
Depolama Hizmetleri performansını izleme için saat ve dakika ölçümlerini tablolardan aşağıdaki ölçümleri kullanabilirsiniz.

* Değerler **AverageE2ELatency** ve **AverageServerLatency** sütunları göster ortalama süre depolama hizmeti veya API işlem türü için istekleri işleyeceğini sürüyor. **AverageE2ELatency** bir ölçüdür isteği okumak ve isteği işlemek için harcanan süre ek olarak yanıtı göndermek için harcanan süre içeren uçtan uca gecikme süresi (depolama istek ulaştığında bu nedenle ağ gecikmesini içerir Hizmeti); **AverageServerLatency** işleme süresini yalnızca bir ölçüdür ve bu nedenle istemci ile iletişim kurmak için ilgili herhangi bir ağ gecikmesini içermez. Bölümüne bakın "[ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor]" neden hakkında ayrıntılı bilgi için bu kılavuzun devamında bu iki değer arasındaki önemli bir fark olabilir.
* Değerler **Totalıngress** ve **TotalEgress** gelen ve aazure storage izmetinizde dışında veya belirli bir API işlemi türü üzerinden Giden bayt cinsinden toplam veri miktarı sütunları göster.
* Değerler **TotalRequests** sütun depolama hizmeti API işlemi, alma isteklerinin toplam sayısını gösterir. **TotalRequests** aldığı depolama hizmeti isteklerinin toplam sayısıdır.

Genellikle, bu değerlerden herhangi birini beklenmeyen değişiklikleri için araştırma gerektiren bir sorun olduğunu bir gösterge izler.

İçinde [Azure portalında](https://portal.azure.com), bu hizmet için performans ölçümlerini hiçbirini 'un altına düşersek veya belirttiğiniz bir eşiği aşması durumunda sizi bilgilendirmesi için uyarı kuralları ekleyebilirsiniz.

"[Sorun giderme rehberi]" başlığına performansıyla ilgili bazı yaygın depolama hizmeti sorunlar açıklanmaktadır.

## <a name="diagnosing-storage-issues"></a>Depolama sorunları tanılama
Uygulamanızda, bir sorun veya sorun uyumlu hale gelebilir çeşitli yollarla vardır dahil olmak üzere:

* Uygulamanın kilitlenmesine veya çalışmayı durdurmasına neden önemli bir hata.
* Önceki bölümde açıklandığı gibi izlemekte olduğunuz ölçümleri temel değerden önemli değişiklikler "[Depolama Hizmeti izleme]."
* Kullanıcıların uygulamanızın beklendiği gibi başka bir işlem tamamlanmadı veya bazı özellik çalışmadığını bildirir.
* Uygulamanızın içinde oluşturulan hatalar, günlük dosyalarında veya başka bir bildirim yöntemi ile görünür.

Genellikle, Azure depolama hizmetleri ile ilgili sorunları dört kategoriden birine girer:

* Uygulamanız kullanıcılarınız tarafından bildirilen veya performans ölçümleri değişikliklerinden ortaya bir performans sorunu vardır.
* Bir veya daha fazla bölgede Azure depolama altyapısı ile ilgili bir sorun yoktur.
* Uygulamanız kullanıcılarınız tarafından bildirilen veya izlemeniz hata sayısı ölçümleri bir artış tarafından ortaya hatayla karşılaşıyor.
* Geliştirme ve test sırasında yerel depolama öykünücüsü kullanarak; Depolama öykünücüsü sahip kullanım için özellikle ilgili bazı sorunlarla karşılaşabilirsiniz.

Aşağıdaki bölümlerde izlemeniz gereken adımları özetler dört bu kategorilerin her sorunlarını tanılama ve giderme için. Bölüm "[sorun giderme rehberi]" daha sonra bu kılavuzdaki karşılaşabileceğiniz bazı yaygın sorunlar için daha fazla ayrıntı sağlar.

### <a name="service-health-issues"></a>Hizmet sistem durumu sorunları
Hizmet sistem durumu sorunları genellikle denetiminiz dışında kalan cihazlardır. [Azure portalında](https://portal.azure.com) depolama hizmetleri gibi Azure hizmetleriyle ilgili devam eden sorunlar hakkında bilgi sağlar. Okuma erişimli coğrafi olarak yedekli depolama için depolama hesabınızı oluştururken ettiyseniz, birincil konumda verilerinizi kullanılamaz hale gelirse sonra uygulamanızı geçici olarak salt okunur kopyasını ikincil konumdaki geçiş yapabilirsiniz. Uygulamanızı ikincil bölgeden okumak için birincil ve ikincil depolama konumları kullanma arasında geçiş yapabilirsiniz ve işlevsellik modunda salt okunur verilerle çalışabilmek için gerekir. Azure depolama istemci kitaplıkları, birincil depolama alanından okuma başarısız olursa ikincil depolama alanından okuyabilen bir yeniden deneme ilkesi tanımlamanızı sağlar. Uygulamanızı ayrıca ikincil konumdaki verileri sonunda tutarlı olduğundan emin olması gerekir. Daha fazla bilgi için bkz. blog gönderisine [Azure depolama Yedekliliği seçenekleri ve okuma erişimli coğrafi olarak yedekli depolama](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

### <a name="performance-issues"></a>Performans sorunları
Bir uygulamanın performansı, özellikle kullanıcının bakış açısıyla, öznel olabilir. Bu nedenle, bir performans sorunu olabilecek yerleri belirlemek için temel ölçümlere sahip olmak önemlidir. Pek çok etken, bir Azure depolama hizmeti istemci uygulaması perspektifinden performansını etkileyebilir. Bu etkenler, depolama hizmeti, istemci veya ağ altyapısını çalışabilir; Bu nedenle performans sorununun kaynağını tanımlayan bir strateji olması önemlidir.

Ölçümleri performans sorunu nedeni büyük olasılıkla konumunu belirledikten sonra tanılama ve daha ayrıntılı sorun giderme için ayrıntılı bilgi için günlük dosyalarını kullanabilirsiniz.

Bölüm "[sorun giderme rehberi]" daha sonra bu kılavuzdaki karşılaşabileceğiniz bazı yaygın performans ile ilgili sorunlar hakkında daha fazla bilgi sağlar.

### <a name="diagnosing-errors"></a>Hataları tanılama
Uygulamanızın kullanıcılarının, istemci uygulaması tarafından bildirilen hataların bildirebiliriz. Depolama ölçümleri de kaydeder, depolama hizmetlerinde farklı hata türleri sayısı gibi **NetworkError**, **ClientTimeoutError**, veya **AuthorizationError**. Depolama ölçümleri sayıları farklı hata türleri yalnızca kayıt sırasında sunucu tarafı ve istemci tarafı ağ günlükleri inceleyerek tek tek istekleri hakkında daha fazla ayrıntı elde edebilirsiniz. Genellikle, depolama hizmet tarafından döndürülen HTTP durum kodu, isteğin başarısız olma nedenine ilişkin bir gösterge sunar.

> [!NOTE]
> Aralıklı hatalar görmeyi beklemelisiniz unutmayın: Örneğin, geçici ağ koşulları nedeniyle veya uygulama hataları.
>
>

Aşağıdaki kaynaklar, depolamayla ilgili durum ve hata kodlarının anlaşılması konusunda yararlıdır:

* [Genel REST API hata kodları](https://msdn.microsoft.com/library/azure/dd179357.aspx)
* [Blob Hizmeti Hata Kodları](https://msdn.microsoft.com/library/azure/dd179439.aspx)
* [Kuyruk hizmeti hata kodları](https://msdn.microsoft.com/library/azure/dd179446.aspx)
* [Tablo hizmeti hata kodları](https://msdn.microsoft.com/library/azure/dd179438.aspx)
* [Dosya hizmeti hata kodları](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues"></a>Depolama öykünücüsü sorunları
Azure SDK'sı, geliştirme iş istasyonunda çalıştırabileceğiniz bir depolama öykünücüsü içerir. Bu öykünücüsü, çoğu Azure depolama hizmetleri davranışını taklit eder ve geliştirme ve test sırasında bir Azure aboneliği ve Azure depolama hesabınız gerek kalmadan Azure depolama hizmetleri kullanan uygulamaları çalıştırmak etkinleştirme yararlı olur.

"[Sorun giderme rehberi]" bölümünde bu kılavuzun depolama öykünücüsü'nü karşılaşılan bazı yaygın sorunlar açıklanmaktadır.

### <a name="storage-logging-tools"></a>Depolama günlük araçları
Depolama günlüğü, sunucu tarafı günlüğe kaydetme, Azure depolama hesabınızda depolama isteklerinin sağlar. Sunucu tarafı günlük kaydını etkinleştirmek ve günlük verilerine erişme hakkında daha fazla bilgi için bkz. [depolama günlüğünü etkinleştirme ve erişim günlüğü verilerini](https://go.microsoft.com/fwlink/?LinkId=510867).

.NET için depolama istemci kitaplığı, depolama işlemleri, uygulamanız tarafından gerçekleştirilen ilişkili istemci tarafı günlük verilerini toplamanıza olanak tanır. Daha fazla bilgi için bkz. [.NET Depolama İstemci Kitaplığı ile İstemci Tarafı Günlük Kaydı](https://go.microsoft.com/fwlink/?LinkId=510868).

> [!NOTE]
> Bazı durumlarda (örneğin, SAS yetkilendirme hataları), bir kullanıcı için istek verisi sunucu tarafı depolama günlüklerinde bulabilirsiniz hata bildirebilir. Sorunun nedenini istemcide ise araştırmak için depolama istemci Kitaplığı'nın günlüğe kaydetme özellikleri kullanın ya da ağ araştırmak için ağ izleme araçları kullanın.
>
>

### <a name="using-network-logging-tools"></a>Ağ günlük araçları kullanma
İstemci ve sunucu değişimi veri ve temel ağ koşulları hakkında ayrıntılı bilgi sağlamak için istemci ve sunucu arasındaki trafiği yakalayabilirsiniz. Yararlı ağ günlük araçları içerir:

* [Fiddler](https://www.telerik.com/fiddler) hata ayıklama proxy'sine, üst bilgiler ve HTTP ve HTTPS istek ve yanıt iletilerinin yük verisi incelemenize olanak sağlayan ücretsiz bir Web. Daha fazla bilgi için [pur'un ek 1: HTTP ve HTTPS trafiğini yakalamak için Fiddler kullanarak](#appendix-1).
* [Microsoft Ağ İzleyicisi'nin (Netmon)](https://www.microsoft.com/download/details.aspx?id=4865) ve [Wireshark](https://www.wireshark.org/) ücretsiz ağ olan çok çeşitli ağ protokolleri için ayrıntılı paket bilgilerini görüntülemenize olanak tanıyan protokol çözümleyici. Wireshark hakkında daha fazla bilgi için bkz. "[ek 2: Ağ trafiğini yakalamak için Wireshark kullanarak](#appendix-2)".
* Microsoft Message Analyzer, Microsoft'tan Netmon ve bu ağ paket verilerini yakalama yanı sıra yerini alır, görüntüleyin ve diğer araçlardan yakalanan günlük verilerini analiz etmenize yardımcı olur, bir araçtır. Daha fazla bilgi için "[ek 3: Ağ trafiğini yakalamak için Microsoft ileti Çözümleyicisi'ni kullanarak](#appendix-3)".
* İstemci makinenizde ağ üzerinden Azure depolama hizmetine bağlanabildiğinden emin denetlemek için bir temel bağlantısı sınaması gerçekleştirmesini istiyorsanız, bu standart kullanarak bunu yapamazsınız **ping** istemcide aracı. Ancak, kullanabileceğiniz [ **Telnet** aracı](https://www.elifulkerson.com/projects/tcping.php) bağlantıyı denetlemek için.

Çoğu durumda, depolama günlüğe kaydetme ve depolama istemci kitaplığı günlük verilerini bir sorunu tanılamak yeterli olacaktır, ancak bazı senaryolarda bu ağ günlük araçları sağlayan daha ayrıntılı bilgi gerekebilir. Örneğin, HTTP ve HTTPS iletilerini görüntülemek için Fiddler'ı kullanarak, gönderilen ve depolama hizmetleri, bir istemci uygulaması, depolama işlemleri nasıl yeniden deneme incelemek etkinleştirmeyi tercih üst bilgisi ve yük verileri görüntülemenize olanak tanır. Böylece, kayıp paketlerin ve bağlantı sorunlarını gidermek etkinleştirmeyi tercih TCP verileri görüntüleyebilirsiniz paket düzeyinde protokol çözümleyici Wireshark gibi çalışır. İleti Çözümleyicisi'ni, hem HTTP hem de TCP katmanına çalışabilir.

## <a name="end-to-end-tracing"></a>Uçtan uca izleme
Uçtan uca izleme günlük dosyalarının çeşitli kullanarak olası sorunları araştırma için yararlı bir tekniktir. Burada sorunu gidermenize yardımcı olacak ayrıntılı bilgi için günlük dosyalarında bakmaya başlamak bir gösterge olarak ölçüler verilerinizi tarih/saat bilgileri kullanın.

### <a name="correlating-log-data"></a>Günlük verileri ilişkilendirme
Ağ günlükleri istemci uygulamalarından görüntülerken, izler ve sunucu tarafı depolama günlük ilişkilendirebilmesi önemlidir arasında farklı günlük dosyaları ister. Günlük dosyaları, bağıntı tanımlayıcı olarak yararlı olan bazı farklı alanları içerir. İstemci istek kimliği, farklı günlüklerinde girişleri ilişkilendirmek için kullanılacak en kullanışlı bir alandır. Ancak bazı durumlarda Sunucu istek kimliği veya zaman damgası kullanmayı yararlı olabilir. Aşağıdaki bölümler bu seçenekler hakkında daha fazla ayrıntı sağlar.

### <a name="client-request-id"></a>İstemci istek kimliği
Depolama istemcisi kitaplığı, benzersiz istemci istek kimliği her istek için otomatik olarak oluşturur.

* Depolama istemcisi kitaplığı oluşturur istemci tarafı günlüğünde, istemci istek kimliği görünür **istemci istek kimliği** istekle ilgili her günlük girişinin alanı.
* Biri fiddler'ı tarafından yakalanan gibi bir ağ izleme istemci istek kimliği istek iletilerinin içinde görünür olup **x-ms-istemci-request-id** HTTP üstbilgisi değeri.
* Sunucu tarafı depolama günlüğü, istemci istek kimliği istemci istek kimliği sütununda görünür.

> [!NOTE]
> (Depolama istemcisi kitaplığı yeni bir değer otomatik olarak atar rağmen), istemci bu değeri atayabilirsiniz çünkü aynı istemci istek kimliği paylaşmak birden çok istek mümkündür. İstemci yeniden denediğinde tüm girişimleri aynı istemci istek kimliği paylaşıyor. İstemciden gönderilen toplu söz konusu olduğunda, toplu işlem tek bir istemci istek kimliği vardır.
>
>

### <a name="server-request-id"></a>Sunucu istek kimliği
Depolama hizmeti, sunucu istek kimliği otomatik olarak oluşturur.

* Sunucu tarafı depolama günlüğü, sunucu istek kimliği görünür **istek kimliği üst bilgisi** sütun.
* Biri fiddler'ı tarafından yakalanan gibi bir ağ izleme sunucu istek kimliği yanıt iletilerini görünür **x-ms-request-id** HTTP üstbilgisi değeri.
* Depolama istemcisi kitaplığı oluşturur istemci tarafı günlüğünde, sunucu istek kimliği görünür **işlemi metin** sunucu yanıtı ayrıntılarını gösteren günlük girişi için sütun.

> [!NOTE]
> İstemci her yeniden deneme girişimi ve bir toplu işe dahil her işlemin benzersiz bir sunucu istek kimliği sahiptir. depolama hizmeti her zaman benzersiz bir sunucu istek kimliği aldıktan sonra her istek için atar
>
>

Depolama istemcisi kitaplığı oluşturursa bir **StorageException** istemci **RequestInformation** özelliği içeren bir **RequestResult** bir içerenbirnesne **ServiceRequestID** özelliği. Ayrıca erişebileceğiniz bir **RequestResult** nesnesinden bir **OperationContext** örneği.

Aşağıdaki kod örneği, özel bir ayarlanacak gösterilmiştir **Clientrequestıd'ye** değeri ekleyerek bir **OperationContext** depolama hizmetine istek nesnesi. Ayrıca nasıl alınacağını gösterir **ServerRequestId** değeri yanıt iletisi.

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Create an Operation Context that includes custom ClientRequestId string based on constants defined within the application along with a Guid.
OperationContext oc = new OperationContext();
oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

try
{
    CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
    ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
    var downloadToPath = string.Format("./{0}", blob.Name);
    using (var fs = File.OpenWrite(downloadToPath))
    {
        blob.DownloadToStream(fs, null, null, oc);
        Console.WriteLine("\t Blob downloaded to file: {0}", downloadToPath);
    }
}
catch (StorageException storageException)
{
    Console.WriteLine("Storage exception {0} occurred", storageException.Message);
    // Multiple results may exist due to client side retry logic - each retried operation will have a unique ServiceRequestId
    foreach (var result in oc.RequestResults)
    {
            Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
    }
}
```

### <a name="timestamps"></a>Zaman damgaları
Zaman damgaları, ilgili günlük girdilerini bulun, ancak herhangi bir saat eğriltme bulunabilecek sunucu ve istemci arasında dikkatli olun için de kullanabilirsiniz. Artı veya eksi istemcideki bir zaman damgası göre sunucu tarafı girdileri eşleştirme için 15 dakika arayın. Ölçümleri içeren BLOB'ları için blob meta verilerini blobu'nda depolanan ölçümler için zaman aralığını gösterdiğini unutmayın. Aynı dakika veya saat için birçok ölçüm blobunuz varsa, bu zaman aralığı çok yararlıdır.

## <a name="troubleshooting-guidance"></a>Sorun giderme kılavuzu
Bu bölüm, tanı koymaya yardımcı olur ve Azure depolama hizmetlerini kullanırken bazı yaygın sorunların çoğunu, uygulamanızın sorunlarını giderme karşılaşabilirsiniz. Sorununuzla ilgili bilgiyi bulmak için aşağıdaki listeyi kullanın.

**Sorun giderme karar ağacı**

---
Sorununuzu depolama hizmetlerinden birini performansını ilişkilidir?

* [Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor]
* [Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor, ancak istemci yüksek gecikme durumu yaşıyor]
* [Ölçümler yüksek AverageServerLatency gösteriyor]
* [Kuyrukta ileti tesliminde beklenmeyen gecikmeler yaşıyorsunuz]

---
Sorununuzu bir depolama hizmet kullanılabilirliğini ilişkilidir?

* [Ölçümler PercentThrottlingError’da artış gösteriyor]
* [Ölçümler PercentTimeoutError’da artış gösteriyor]
* [Ölçümler PercentNetworkError’da artış gösteriyor]

---
 İstemci uygulamanızın bir depolama hizmetinden bir HTTP 4XX (örneğin, 404) yanıt alıyor?

* [İstemci HTTP 403 (Yasak) iletilerini alıyor]
* [İstemci HTTP 404 (Bulunamadı) iletilerini alıyor]
* [İstemci HTTP 409 (Çakışma) iletileri alma]

---
[Düşük PercentSuccess ölçümleri göster veya ClientOtherErrors işlem durumundaki işlemlerini analytics günlük girdilerine sahip]

---
[Kapasite ölçümlerini beklenmeyen artışı depolama kapasitesi kullanımı Göster]

---
[Sanal makinelerin çok sayıda ekli VHD'lerde sahip beklenmedik bir şekilde yeniden yaşayan]

---
[Sorununuzu geliştirme veya test için depolama öykünücüsünü kullanarak ortaya çıkar.]

---
[.NET için Azure SDK'sını yüklerken sorunlarla karşılaşıyoruz.]

---
[Bir depolama hizmetindeki farklı bir sorun olması]

---
### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor
Aşağıdaki çizimde gelen [Azure portalında](https://portal.azure.com) izleme aracı gösteren bir örnek burada **AverageE2ELatency** önemli ölçüde daha yüksek **AverageServerLatency**.

![][4]

Depolama hizmeti yalnızca ölçüm hesaplar **AverageE2ELatency** başarılı istekler için ve farklı **AverageServerLatency**, veri göndermek ve almak için istemci geçen süreyi içerir Depolama hizmetinden alındı. Bu nedenle, birbirinden **AverageE2ELatency** ve **AverageServerLatency** nedeniyle yavaş yanıt olan istemci uygulaması veya ağ üzerindeki koşullar nedeniyle olabilir.

> [!NOTE]
> Ayrıca görüntüleyebilirsiniz **E2ELatency** ve **ServerLatency** günlük depolama ayrı depolama işlemleri için günlük verilerini.
>
>

#### <a name="investigating-client-performance-issues"></a>İstemci performans sorunlarını araştırma
Yavaş yanıt istemci için olası nedenler sınırlı sayıda kullanılabilir bağlantılar veya iş parçacığı ya da düşük CPU, bellek veya ağ bant genişliği gibi kaynakları içerir. (Örneğin depolama hizmeti için zaman uyumsuz çağrıları kullanarak) daha etkili olması için istemci kodu değiştirerek veya (daha fazla sayıda çekirdek ve daha fazla bellek ile) daha büyük bir sanal makine kullanarak bu sorunu çözmek mümkün olabilir.

Tablo ve kuyruk Hizmetleri, Nagle algoritması da yüksek neden olabilir **AverageE2ELatency** kıyasla **AverageServerLatency**: daha fazla bilgi için gönderiye bakın [Nagle'nın Algoritmasıdır küçük istekler doğrultusunda kolay değil](https://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx). Kullanarak kod Nagle algoritmasında devre dışı bırakabilirsiniz **ServicePointManager** sınıfını **System.Net** ad alanı. Bu kuyruk Hizmetleri zaten bağlantılar etkilemez beri uygulamanızdaki açmak veya tabloya yapılan her çağrı yaptığınız yapmanız gerekir. Aşağıdaki örnek geldiği **uygulama_başlatma** bir çalışan rolünde yöntemi.

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);
ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
tableServicePoint.UseNagleAlgorithm = false;
ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
queueServicePoint.UseNagleAlgorithm = false;
```

Kaç istemci uygulamanızı gönderme istekleri görmek için istemci tarafı günlüklerini ve onay genel .NET için performans sorunlarını istemcinizi CPU, .NET atık toplama, ağ kullanımı veya bellek gibi ilgili denetlemeniz gerekir. .NET istemci uygulamalarında sorun giderme için başlangıç noktası olarak görmek [hata ayıklama, izleme ve profil oluşturma](https://msdn.microsoft.com/library/7fe0dd2y).

#### <a name="investigating-network-latency-issues"></a>Ağ gecikmesi sorunlarını araştırma
Genellikle, ağdan kaynaklanan uçtan uca gecikme süresi yüksek geçici durumları yüzünde olur. Atılan paketlerin gibi her iki geçici ve kalıcı ağ sorunları Wireshark veya Microsoft Message Analyzer gibi araçlarla araştırabilirsiniz.

Ağ sorunları gidermek için Wireshark kullanma hakkında daha fazla bilgi için bkz. "[Ek 2: Ağ trafiğini yakalamak için Wireshark kullanma]. "

Ağ sorunları gidermek için Microsoft ileti Çözümleyicisi'ni kullanma hakkında daha fazla bilgi için bkz. "[Ek 3: Ağ trafiğini yakalamak için Microsoft Message Analyzer'ı kullanma]. "

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor, ancak istemci yüksek gecikme durumu yaşıyor
Bu senaryoda, en olası nedeni depolama istekleri depolama hizmetine erişirken bir gecikme olur. Neden istemcisinden gelen istekleri aracılığıyla blob hizmetine kuran değil araştırmanız gerekir.

İstemci istekleri gönderirken geciktirme olası nedenlerinden birini kullanılabilir bağlantılar veya iş parçacığının sınırlı sayıda yoktur.

Ayrıca, istemcinin birden çok deneme çalışıp çalışmadığını denetleyin ve ise araştırın. İstemcinin birden çok deneme çalışıp çalışmadığını belirlemek için şunları yapabilirsiniz:

* Depolama analizi günlüklerini inceleyin. Birden çok deneme geçekleşmiş ise aynı istemci istek kimliği ile ancak farklı sunucu istek kimliği ile birden çok işlem görürsünüz.
* İstemci günlüklerini inceleyin. Ayrıntılı günlük kaydı, bir yeniden deneme oluştuğunu gösterir.
* Kodunuzdaki hataları ayıklamanıza ve özelliklerini denetleyin **OperationContext** istekle ilişkili nesne. İşlem yeniden denenebilir, **RequestResults** birden fazla benzersiz sunucu istek kimliği özelliği içerir. Ayrıca, her istek için başlangıç ve bitiş zamanlarını kontrol edebilirsiniz. Kod örneği bölümünde daha fazla bilgi için bkz [sunucu istek kimliği].

İstemci herhangi bir sorun varsa, paket kaybı gibi potansiyel ağ sorunlarını araştırmanız gerekir. Ağ sorunları araştırmak için Wireshark veya Microsoft Message Analyzer gibi araçları kullanabilirsiniz.

Ağ sorunları gidermek için Wireshark kullanma hakkında daha fazla bilgi için bkz. "[Ek 2: Ağ trafiğini yakalamak için Wireshark kullanma]. "

Ağ sorunları gidermek için Microsoft ileti Çözümleyicisi'ni kullanma hakkında daha fazla bilgi için bkz. "[Ek 3: Ağ trafiğini yakalamak için Microsoft Message Analyzer'ı kullanma]. "

### <a name="metrics-show-high-AverageServerLatency"></a>Ölçümler yüksek AverageServerLatency gösteriyor
Söz konusu olduğunda yüksek **AverageServerLatency** blob yükleme istekleri için aynı blob (veya BLOB'ları kümesi) için tekrarlanan istekleri olup olmadığını görmek için depolama günlük günlükleri kullanmanız gerekir. Blobu karşıya yükleme istekleri için hangi istemci boyutu (örneğin, okuma işlemleri de değerinden 64 K olmadıkça boyutu 64 K ek yüklerini neden olabilir, daha az chunks blokları) kullanarak ve birden çok istemci aynı blob para blokları yüklediğiniz, bloğudur araştırmanız gerekir Paralel. Aşılmasıyla sonuçlanıyor neden olan istek sayısı artış için dakika başına ölçümler de denetlemelisiniz ikinci ölçeklenebilirlik hedefleri başına: Ayrıca bkz: "[Ölçümler PercentTimeoutError’da artış gösteriyor]."

Yüksek görüyorsanız **AverageServerLatency** için blob indirme var. Yinelenen, istekleri aynı blobu veya BLOB'ları kümesi, ardından bu bloblarda Azure Cache veya Azure Content Delivery kullanarak önbelleğe alma göz önünde bulundurmanız gerekir Ağı (CDN). Karşıya yükleme istekleri için daha büyük bir blok boyutu kullanılarak üretilen işi artırabilir. Tabloları, sorguları için de aynı sorgu işlemleri gerçekleştiren ve verileri burada sık değişmez istemcilerde istemci tarafı önbelleğe alma uygulamak mümkündür.

Yüksek **AverageServerLatency** değerleri kötü tasarlanmış tablolar ve sorgular tarama işlemleri sonucunda ya da ekleme ve başına koruma deseni izleyin belirtisi de olabilir. Daha fazla bilgi için "[Ölçümler PercentThrottlingError’da artış gösteriyor]".

> [!NOTE]
> Kapsamlı denetim listesi Performans Denetim burada bulabilirsiniz: [Microsoft Azure depolama performansı ve ölçeklenebilirlik denetim listesi](storage-performance-checklist.md).
>
>

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>Kuyrukta ileti tesliminde beklenmeyen gecikmeler yaşıyorsunuz demektir
Ardından bir uygulama bir kuyruğa bir ileti ekler zaman kuyruktan okunmak üzere kullanılabilir saat arasında bir gecikme yaşıyorsanız, sorunu tanılamak için aşağıdaki adımları uygulamanız gereken:

* Uygulama başarıyla kuyruğa ileti eklemektir doğrulayın. Uygulama değil yeniden deneniyor onay **AddMessage** birkaç kez önce başarılı yöntemi. Depolama istemci kitaplığı günlükler, depolama işlemlerinin yinelenen olan herhangi bir yeniden deneme gösterilir.
* Saat eğriltme arasında ileti eklediğinde kuyruğun çalışan rolü ve kolaylaştıran kuyruktan ileti okuyan çalışan rolü görünür işlemede bir gecikme olur gibi doğrulayın.
* Kuyruktan iletileri okuyan çalışan rolü başarısız olup olmadığını denetleyin. Bir kuyruğu istemcisi çağırırsa **GetMessage** yöntemi ancak bir bildirim ile yanıt başarısız, ileti kalır kadar kuyruktaki görünmez **invisibilityTimeout** süresi dolmadan. Bu noktada, ileti yeniden işlenmek için kullanılabilir hale gelir.
* Kuyruk uzunluğu zamanla artıyor mu denetleyin. Diğer çalışanlar kuyrukta yerleştirdiğinizi tüm iletileri işlemek kullanılabilir yeterli çalışan yoksa bu durum oluşabilir. Ayrıca silme isteklerinin başarısız olma ve kuyruktan çıkarma iletiyi silmek için yinelenen başarısız denemeleri belirtebilecek iletilerde sayısı, görmek için ölçüm denetleyin.
* Beklenenden daha yüksek olan kuyruk işlemleri için depolama günlüğü günlüklerini inceleyin **E2ELatency** ve **ServerLatency** normalden daha uzun bir süre boyunca değerleri.

### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Ölçümler Percentthrottlingerror'da artış gösteriyor
Depolama hizmeti ölçeklenebilirlik hedefleri aştığında azaltma hataları oluşur. Tek bir istemcisi olmayan emin olun veya Kiracı için depolama hizmet kısıtlamalar, diğerleri çoğaltamaz hizmetini kullanabilirsiniz. Daha fazla bilgi için [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md) depolama hesapları için ölçeklenebilirlik hedefleri ve depolama hesapları bölümleri için performans hedefleri hakkında ayrıntılar için.

Varsa **Percentthrottlingerror'da** ölçüm azaltma bir hata ile başarısız olan istek yüzdesi cinsinden artışı gösterme, araştırmak iki senaryodan biri gerekir:

* [Geçici Percentthrottlingerror'da artış]
* [Kalıcı hata Percentthrottlingerror'da artış]

Artış **Percentthrottlingerror'da** genellikle depolama isteklerinin sayısı arasında bir artış ile aynı zamanda oluşur veya uygulamanızı test etme, başlangıçta olduğunuzda yükle. Bu ayrıca kendi istemci "503 Sunucu meşgul" veya "500 işlem zaman aşımı" HTTP durum iletileri depolama işlemleri gibi bildiriminde.

#### <a name="transient-increase-in-PercentThrottlingError"></a>Geçici Percentthrottlingerror'da artış
Değerini artış görüyorsanız **Percentthrottlingerror'da** yüksek etkinlik dönemlerini uygulama ile çakıştığı, bir üstel (doğrusal değil) geri alma stratejisi için yeniden deneme istemcinizde uygulayın. Geri alma yeniden deneme hemen bölüm azaltmak ve ani trafik kesintisiz uygulamanıza yardımcı olur. Depolama istemci kitaplığı kullanılarak yeniden deneme ilkelerini uygulamanız hakkında daha fazla bilgi için bkz: [Microsoft.Azure.Storage.RetryPolicies ad alanı](/dotnet/api/microsoft.azure.storage.retrypolicies).

> [!NOTE]
> Ayrıca değerini bir artış görebilirsiniz **Percentthrottlingerror'da** , değil çakıştığı yüksek etkinlik dönemlerini uygulama ile: Burada en olası nedeni yük dengelemeyi iyileştirmek üzere bölümler taşıma depolama hizmetidir.
>
>

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Kalıcı hata Percentthrottlingerror'da artış
İçin sürekli olarak yüksek bir değer görüyorsanız **Percentthrottlingerror'da** kalıcı artış, işlem birimleri veya ilk yükleme gerçekleştirirken aşağıdaki uygulama testleri ve değerlendirmek gereken nasıl Uygulamanızı depolama bölümleri ve bir depolama hesabı ölçeklenebilirlik hedeflerini olup yaklaştığını kullanıyor. Azaltma (Bu, tek bir bölüm sayılır) kuyrukta hataları görüyorsanız, örneğin, sonra ek kuyrukları işlemler arasında birden çok bölüm yaymak için kullanmayı düşünmeniz gerekir. Azaltma hataları bir tabloda görüyorsanız, geniş bir bölüm anahtarı değerlerine kullanarak birden çok bölümde işlemlerinizi yaymak için farklı bir bölümleme düzeni kullanmayı göz önünde gerekir. Bu sorunun yaygın nedenlerinden biri, burada bölüm anahtarı olarak tarihi seçin ve ardından tüm veriler belirli bir tarihte yazılır tek bir bölüm prepend ve ekleme koruma desen olmasıdır: yük altında bu yazma tıkanıklığa sonuçlanabilir. Farklı bir bölümleme tasarımı düşünün veya blob depolama kullanarak daha iyi bir çözüm olabilir olup olmadığını değerlendirmek. Ayrıca azaltma, trafiğindeki ani artışları sonucunda oluşup olmadığını kontrol edin ve desen isteklerinin düzgünleştirme yolları inceleyin.

Birden çok bölümde işlemlerinizi dağıtırsanız, yine de için depolama hesabı ölçeklenebilirlik sınırları bilmeniz gerekir. Örneğin, on kuyruklar her saniyede 2.000 1 KB'lık ileti en fazla işleme kullandıysanız, depolama hesabı için saniyede 20.000 iletileri genel sınırını şu olacaktır. Saniye başına 20. 000'den fazla varlıkları işlemek gerekiyorsa, birden fazla depolama hesabı kullanmayı düşünmeniz gerekir. İstekleri ve varlıkların boyutu, depolama hizmeti istemcilerinize kısıtlar üzerinde bir etkisi olduğunu göz önünde bulundurmanız gerekir: büyük istekleri ve varlıkları varsa, daha çabuk kısıtlanabilir.

Verimsiz sorgu tasarım tablo bölümleri için ölçeklenebilirlik sınırları isabet yapmanıza da neden olabilir. Örneğin, bir sorgu ile bir filtre, yalnızca yüzde varlıklar bir bölümde seçer, ancak, bir bölümdeki tüm varlıkları tarar, her varlık erişmek gerekir. Her varlık okuma izlenmezler. Bu bölümde işlem toplam sayısı hesaplanır; Bu nedenle, ölçeklenebilirlik hedefleri kolayca ulaşabilirsiniz.

> [!NOTE]
> Performans testi, uygulamanızdaki tüm verimsiz sorgu tasarımları ortaya.
>
>

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Ölçümler Percenttimeouterror'da artış gösteriyor
Ölçümlerinizin artış Göster **Percenttimeouterror'da** depolama hizmetlerinizi biri için. Aynı zamanda, istemci, depolama işlemlerinin yüksek hacimli "500 işlem zaman aşımı" HTTP durum iletileri alır.

> [!NOTE]
> Depolama hizmeti geçici olarak yeni bir sunucuya bir bölüme taşıyarak yük dengeleyen isteklerini zaman aşımı hataları görebilirsiniz.
>
>

**Percenttimeouterror'da** bir ölçümdür şu ölçümlerden bir toplama: **ClientTimeoutError**, **AnonymousClientTimeoutError**, **SASClientTimeoutError**, **ServerTimeoutError**,  **AnonymousServerTimeoutError**, ve **SASServerTimeoutError**.

Sunucu zaman aşımı sunucuda bir hata nedeniyle. İstemci zaman aşımları Server'da bir işlem istemci tarafından belirtilen zaman aşımı aştığından gerçekleşir; Depolama istemcisi Kitaplığı'nı kullanarak bir istemci bir işlem için bir zaman aşımı kullanarak örneğin, ayarlayabilirsiniz **ServerTimeout** özelliği **QueueRequestOptions** sınıfı.

Sunucu zaman aşımı daha fazla araştırma gerektiren depolama hizmeti bir sorun olduğunu gösteriyor. Ölçüm, hizmetin ölçeklenebilirlik ulaştığını anlayabilmesini varsa bkz ve bu soruna neden trafik herhangi bir ani artış tanımlamak için kullanabilirsiniz. Aralıklı olarak ortaya çıkan bir sorundur, Yük Dengeleme nedeniyle olabilir hizmetindeki etkinlikleri. Bir destek sorunu, sorunu kalıcıdır ve hizmetin ölçeklenebilirlik sınırlarını ulaşma uygulamanız tarafından neden olup olmadığını, harekete geçirmelidir. İstemci zaman aşımları için zaman aşımı istemci ve istemci zaman aşımı değerini ayarlayın ya da değişiklik uygun bir değere ayarlayın veya nasıl performansı depolama hizmetinde işlemlerinin örneğin iyileştirerek artırabilirsiniz araştırmak karar vermeniz gerekir Tablo sorgularınızı veya iletilerinizi boyutunu azaltır.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Ölçümler Percentnetworkerror'da artış gösteriyor
Ölçümlerinizin artış Göster **Percentnetworkerror'da** depolama hizmetlerinizi biri için. **Percentnetworkerror'da** bir ölçümdür şu ölçümlerden bir toplama: **NetworkError**, **AnonymousNetworkError**, ve **SASNetworkError**. Bu, depolama hizmeti, bir ağ hatası algıladığında istemci depolama bir istekte bulunduğunda oluşur.

Bu hatanın en yaygın nedeni, bir istemcidir depolama hizmeti zaman aşımı süresi dolmadan önce bağlantısı kesiliyor. Neden ve ne zaman istemci bağlantısını keser ve storage hizmetinden anlamak için istemci kodu inceleyin. İstemciden gelen ağ bağlantısı sorunları araştırmak için Wireshark, Microsoft ileti Çözümleyicisi'ni veya Telnet kullanabilirsiniz. Bu araçlar, şurada açıklanan [Ekler].

### <a name="the-client-is-receiving-403-messages"></a>İstemci, HTTP 403 (Yasak) iletilerini alma
İstemci uygulamanızda HTTP 403 (Yasak) hataları oluşuyorsa büyük ihtimalle istemci depolama isteği gönderirken süresi dolmuş bir Paylaşılan Erişim İmzası (SAS) kullandığı içindir. (Saat sapması, geçersiz anahtarlar ve boş üst bilgiler gibi başka olası nedenler de vardır.) Sorun süresi dolmuş bir SAS anahtarından kaynaklanıyorsa sunucu tarafı Depolama Günlük Kaydı günlük verilerinde herhangi bir giriş görmezsiniz. Aşağıdaki tabloda, bu sorunun oluşmasını gösterir depolama istemcisi kitaplığı tarafından oluşturulan istemci tarafı günlük bir örnek gösterilmektedir:

| Kaynak | Ayrıntı düzeyi | Ayrıntı düzeyi | İstemci istek kimliği | İşlem metin |
| --- | --- | --- | --- | --- |
| Microsoft.Azure.Storage |Bilgi |3 |85d077ab-… |Konum modunu PrimaryOnly başına, birincil konumla işlemi başlatılıyor. |
| Microsoft.Azure.Storage |Bilgi |3 |85d077ab-... |Eşzamanlı isteği başlatılıyor <https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&sr=c&si=mypolicy&sig=OFnd4Rd7z01fIvh%2BmcR6zbudIH2F5Ikm%2FyhNYZEmJNQ%3D&api-version=2014-02-14> |
| Microsoft.Azure.Storage |Bilgi |3 |85d077ab-... |Yanıt bekleniyor. |
| Microsoft.Azure.Storage |Uyarı |2 |85d077ab-... |Yanıtı beklenirken özel durum: Uzak sunucu hata döndürdü: (403) Yasak. |
| Microsoft.Azure.Storage |Bilgi |3 |85d077ab-... |Yanıt alındı. Durum kodu 403, istek kimliği = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, içerik MD5 = =, ETag =. |
| Microsoft.Azure.Storage |Uyarı |2 |85d077ab-... |İşlem sırasında özel bir durum oluştu: Uzak sunucu hata döndürdü: (403) Yasak... |
| Microsoft.Azure.Storage |Bilgi |3 |85d077ab-... |İşlemi yeniden denetleniyor. Yeniden deneme sayısı = 0, HTTP durum kodu 403, özel durum = = uzak sunucu bir hata döndürdü: (403) Yasak... |
| Microsoft.Azure.Storage |Bilgi |3 |85d077ab-... |Sonraki konuma için birincil konum modunu dayalı olarak ayarlandı. |
| Microsoft.Azure.Storage |Hata |1 |85d077ab-... |Yeniden deneme ilkesi için bir yeniden deneme izin vermedi. Başarısız olan uzak sunucu hata döndürdü: (403) Yasak. |

Bu senaryoda, istemcinin sunucuya belirteç göndermeden önce SAS belirteci neden doluyor araştırmanız gerekir:

* Genellikle bir istemcinin hemen kullanması için SAS oluştururken başlangıç zamanı ayarlamamalısınız. Geçerli zamanı kullanarak SAS belirtecini oluşturan konak ile depolama hizmeti arasında küçük saat farklılıkları varsa depolama hizmeti henüz geçerli olmayan bir SAS alabilir.
* Bir SAS belirtecinin sona erme süresini çok kısa ayarlamayın. Ayrıca, SAS belirtecini oluşturan konakla depolama hizmeti arasındaki küçük saat farklılıkları, bir SAS belirtecinin süresinin beklenenden erken dolmuş gibi görünmesine de neden olabilir.
* Sürüm parametresi SAS anahtarının mu (örneğin **sv = 2015-04-05**) kullandığınız depolama istemci kitaplığı sürümüyle eşleşen? Her zaman en son sürümünü kullanmanızı öneririz [depolama istemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/).
* Depolama erişim anahtarlarınızı yeniden oluşturursanız mevcut SAS belirteçleri geçerliliğini kaybedebilir. İstemci uygulamalarının önbelleğe alması için sona erme süresi uzun olan SAS belirteçleri oluşturursanız bu sorunla karşılaşabilirsiniz.

SAS belirteçleri oluşturmak için Depolama İstemcisi Kitaplığı’nı kullanıyorsanız geçerli bir belirteç oluşturmak kolaydır. Depolama REST API kullanarak ve el ile SAS belirteçleri oluşturmak, ancak bkz [bir paylaşılan erişim imzası ile erişim için temsilci seçme](https://msdn.microsoft.com/library/azure/ee395415.aspx).

### <a name="the-client-is-receiving-404-messages"></a>İstemci, HTTP 404 (bulunamadı) iletilerini alma
İstemci uygulaması sunucudan HTTP 404 (Bulunamadı) iletisi alırsa istemcinin kullanmaya çalıştığı nesne (bir varlık, tablo, blob, kapsayıcı veya kuyruk gibi) depolama hizmetinde mevcut değil demektir. Bunun aşağıdaki gibi çeşitli olası nedenleri vardır:

* [İstemci veya başka bir işlem daha önce nesneyi silmiş]
* [Bir paylaşılan erişim imzası (SAS) yetkilendirme sorunu]
* [İstemci tarafı JavaScript kodu nesneye erişim izni yok]
* [Ağ hatası]

#### <a name="client-previously-deleted-the-object"></a>Daha önce istemci veya başka bir işlem nesnesi silindi.
Okumak, güncelleştirmek veya depolama hizmetindeki verileri silmek için istemci nerede çalışıyor senaryolarda sunucu tarafı günlüklerinde söz konusu nesne Depolama hizmetinden silinen bir önceki işlemi tanımlamak genellikle kolaydır. Genellikle, günlük verilerini başka bir kullanıcı veya işlem nesnesi silindiği gösterilir. Depolama günlüğü sunucu tarafı oturum açma, ne zaman bir istemci bir nesne silindi, işlem türü ve istenen nesne-anahtar sütun gösterir.

İstemci, yeni bir nesne oluşturma koşuluyla, bir HTTP 404 (bulunamadı) yanıt olarak bu sonuçları neden bir nesneyi eklemek için bir istemci nerede çalışıyor senaryosunda, bunu hemen belirgin olmayabilir. Ancak, istemci, istemci bir kuyruk bulamadı olmalıdır bir ileti oluşturuyorsanız blob kapsayıcısını bulamadı olmalıdır bir blob oluşturuyorsanız ve istemci bir satır ekliyorsanız bu tabloyu bulamadı olmalıdır.

İstemci için depolama hizmeti belirli bir istek gönderdiğinde bir ayrıntılı'nın anlayış kazanmak için depolama istemci Kitaplığı'ndan istemci tarafı günlük kullanın.

Depolama istemcisi kitaplığı tarafından oluşturulan aşağıdaki istemci tarafı günlük kaydı, istemci sanal makinesi için blob kapsayıcısı bulamadığında sorunu gösterir. Bu günlük dosyası şu depolama işlemlerinin ayrıntılarını içerir:

| İstek Kimliği | İşlem |
| --- | --- |
| 07b26a5d-... |**DeleteIfExists** blob kapsayıcısını silmek için yöntemi. Bu işlem içeren Not bir **baş** kapsayıcı varlığını denetlemek için istek. |
| e2d06d78... |**Createıfnotexists** blob kapsayıcısı oluşturmak için yöntemi. Bu işlem içeren Not bir **baş** kapsayıcı varlığını denetleyen istek. **Baş** 404 bir ileti döndürür, ancak devam eder. |
| de8b1c3c-... |**UploadFromStream** blob oluşturmak için yöntemi. **PUT** istek 404 iletisiyle başarısız olur |

Günlük girdileri:

| İstek Kimliği | İşlem metin |
| --- | --- |
| 07b26a5d-... |Eşzamanlı isteği başlatma https://domemaildist.blob.core.windows.net/azuremmblobcontainer. |
| 07b26a5d-... |StringToSign = HEAD............x-ms-client-request-id:07b26a5d-....x-ms-date:Tue, 03 Jun 2014 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Yanıt bekleniyor. |
| 07b26a5d-... |Yanıt alındı. Durum kodu 200, istek kimliği = eeead849... = İçerik MD5 =, ETag = &quot;0x8D14D2DC63D059B&quot;. |
| 07b26a5d-... |Yanıt Üstbilgileri işlemi geri kalanıyla devam etmeden başarıyla işlendi. |
| 07b26a5d-... |Yanıt gövdesi indiriliyor. |
| 07b26a5d-... |İşlem başarıyla tamamlandı. |
| 07b26a5d-... |Eşzamanlı isteği başlatma https://domemaildist.blob.core.windows.net/azuremmblobcontainer. |
| 07b26a5d-... |StringToSign = DELETE............x-ms-client-request-id:07b26a5d-....x-ms-date:Tue, 03 Jun 2014 10:33:12    GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Yanıt bekleniyor. |
| 07b26a5d-... |Yanıt alındı. Durum kodu 202, istek kimliği = 6ab2a4cf-..., içerik MD5 = =, ETag =. |
| 07b26a5d-... |Yanıt Üstbilgileri işlemi geri kalanıyla devam etmeden başarıyla işlendi. |
| 07b26a5d-... |Yanıt gövdesi indiriliyor. |
| 07b26a5d-... |İşlem başarıyla tamamlandı. |
| e2d06d78-... |Zaman uyumsuz istek için başlangıç https://domemaildist.blob.core.windows.net/azuremmblobcontainer.</td> |
| e2d06d78-... |StringToSign = HEAD............x-ms-client-request-id:e2d06d78-....x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Yanıt bekleniyor. |
| de8b1c3c-... |Eşzamanlı isteği başlatma https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt. |
| de8b1c3c-... |StringToSign PUT =... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-MS-BLOB-Type:BlockBlob.x-MS-Client-Request-id:de8b1c3c-...x-MS-Date:TUE, 03 Haziran 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt. |
| de8b1c3c-... |İstek verileri yazmak hazırlanıyor. |
| e2d06d78-... |Yanıtı beklenirken özel durum: Uzak sunucu hata döndürdü: (404) bulunamadı... |
| e2d06d78-... |Yanıt alındı. Durum kodu 404, istek kimliği = 353ae3bc-..., içerik MD5 = =, ETag =. |
| e2d06d78-... |Yanıt Üstbilgileri işlemi geri kalanıyla devam etmeden başarıyla işlendi. |
| e2d06d78-... |Yanıt gövdesi indiriliyor. |
| e2d06d78-... |İşlem başarıyla tamamlandı. |
| e2d06d78-... |Zaman uyumsuz istek için başlangıç https://domemaildist.blob.core.windows.net/azuremmblobcontainer. |
| e2d06d78-... |StringToSign = PUT...0.........x-ms-client-request-id:e2d06d78-....x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Yanıt bekleniyor. |
| de8b1c3c-... |Yazma istek verileri. |
| de8b1c3c-... |Yanıt bekleniyor. |
| e2d06d78-... |Yanıtı beklenirken özel durum: Uzak sunucu hata döndürdü: Çakışma (409)... |
| e2d06d78-... |Yanıt alındı. Durum kodu 409, istek kimliği = c27da20e-..., içerik MD5 = =, ETag =. |
| e2d06d78-... |İndirme hatası yanıt gövdesi. |
| de8b1c3c-... |Yanıtı beklenirken özel durum: Uzak sunucu hata döndürdü: (404) bulunamadı... |
| de8b1c3c-... |Yanıt alındı. Durum kodu 404, istek kimliği = 0eaeab3e-..., içerik MD5 = =, ETag =. |
| de8b1c3c-... |İşlem sırasında özel bir durum oluştu: Uzak sunucu hata döndürdü: (404) bulunamadı... |
| de8b1c3c-... |Yeniden deneme ilkesi için bir yeniden deneme izin vermedi. Başarısız olan uzak sunucu hata döndürdü: (404) bulunamadı... |
| e2d06d78-... |Yeniden deneme ilkesi için bir yeniden deneme izin vermedi. Başarısız olan uzak sunucu hata döndürdü: Çakışma (409)... |

Bu örnekte, günlük istemci gelen istekleri Interleaving olduğunu gösterir **Createıfnotexists** gelen istekleri (istek kimliği e2d06d78...) yöntemiyle **UploadFromStream** yöntemi (de8b1c3c-...). Bunun nedeni, istemci uygulamasının zaman uyumsuz olarak bu yöntemleri çağıran bu Interleaving kullanmasıdır. Tüm veriler, kapsayıcıdaki blob karşıya yükleme işlemine başlamadan önce kapsayıcı oluşturduğundan emin olmak için istemci zaman uyumsuz kodu değiştirin. İdeal olarak, tüm kapsayıcıları önceden oluşturmanız gerekir.

#### <a name="SAS-authorization-issue"></a>Paylaşılan erişim imzası (SAS) bir yetkilendirme sorunu
İstemci uygulama, işlemi için gerekli izinleri içermez bir SAS anahtarı kullanmayı denerse, depolama hizmeti istemciye bir HTTP 404 (bulunamadı) hatası iletisi döndürür. Aynı zamanda, ayrıca için sıfır olmayan bir değer görürsünüz **SASAuthorizationError** ölçümleri de.

Aşağıdaki tabloda örnek bir sunucu tarafı günlük ileti günlüğü depolama günlük dosyasından gösterilmektedir:

| Ad | Değer |
| --- | --- |
| İsteğin başlangıç saati | 2014-05-30T06:17:48.4473697Z |
| İşlem türü     | GetBlobProperties            |
| İstek durumu     | SASAuthorizationError        |
| HTTP durum kodu   | 404                          |
| Kimlik doğrulaması türü| SAS                          |
| Hizmet türü       | Blob                         |
| İstek URL'si        | https://domemaildist.blob.core.windows.net/azureimblobcontainer/blobCreatedViaSAS.txt |
| &nbsp;                 |   ?sv=2014-02-14&sr=c&si=mypolicy&sig=XXXXX&;api-version=2014-02-14 |
| İstek Kimliği üst bilgisi  | a1f348d5-8032-4912-93ef-b393e5252a3b |
| İstemci istek kimliği  | 2d064953-8436-4ee0-aa0c-65cb874f7929 |


İstemci uygulamanızı neden olduğu için bu izinler verilmedi bir işlemi gerçekleştirmeye çalışıyor araştırın.

#### <a name="JavaScript-code-does-not-have-permission"></a>İstemci tarafı JavaScript kodunu, nesneye erişim izni yok
JavaScript istemcisi kullanıyorsanız ve depolama hizmeti, HTTP 404 iletilerini döndürmek, tarayıcıda aşağıdaki JavaScript hatalar için denetleyin:

```
SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.
```

> [!NOTE]
> İstemci tarafı JavaScript sorunlarını giderirken, tarayıcı ve depolama hizmeti arasında alınıp verilen iletileri izlemek için Internet Explorer'da F12 geliştirici araçlarını kullanabilirsiniz.
>
>

Web tarayıcısı uyguladığından, bu hatalar ortaya [aynı çıkış noktası İlkesi](https://www.w3.org/Security/wiki/Same_Origin_Policy) bir web sayfası bir API farklı bir etki alanındaki etki alanından sayfa çağırma engelleyen bir güvenlik kısıtlaması gelir.

JavaScript sorunu geçici olarak çözmek için depolama hizmeti için istemci erişim Cross Origin kaynak paylaşımı (CORS) yapılandırabilirsiniz. Daha fazla bilgi için [Azure depolama hizmetleri için çıkış noktaları arası kaynak paylaşımı (CORS) desteği](https://msdn.microsoft.com/library/azure/dn535601.aspx).

Aşağıdaki kod örneği, Contoso etki alanında çalışan JavaScript blob depolama hizmetindeki bir blob erişmesine izin vermek için blob hizmeti yapılandırma işlemi gösterilmektedir:

```csharp
CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
// Set the service properties.
ServiceProperties sp = client.GetServiceProperties();
sp.DefaultServiceVersion = "2013-08-15";
CorsRule cr = new CorsRule();
cr.AllowedHeaders.Add("*");
cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
cr.AllowedOrigins.Add("http://www.contoso.com");
cr.ExposedHeaders.Add("x-ms-*");
cr.MaxAgeInSeconds = 5;
sp.Cors.CorsRules.Clear();
sp.Cors.CorsRules.Add(cr);
client.SetServiceProperties(sp);
```

#### <a name="network-failure"></a>Ağ hatası
Bazı durumlarda, HTTP 404 iletileri istemciye göndermeden depolama hizmeti kayıp ağ paketlerinin neden olabilir. Örneğin, istemci uygulamanızı tablo hizmetinden bir varlık silinirken bir depolama özel durumu raporlama throw istemci bkz bir "HTTP 404 (bulunamadı)" Tablo hizmetinden gelen durum iletisi. Tablo depolama hizmetinin tablosunda araştırırken, hizmet istenen varlık sildi bakın.

İstek Kimliği (7e84f12d...) isteği için tablo hizmeti tarafından atanan istemci özel durum ayrıntıları içerir: istek ayrıntılarını sunucu tarafı depolama günlüklerinde arama yaparak bulmak için bu bilgileri kullanabilirsiniz **istek kimliği üstbilgisi**  günlük dosyasında sütun. Ölçümler, bu gibi hataları ortaya çıktığında ve sonra da bu hatayı ölçümleri kaydedilen süreye göre günlük dosyalarını arayın olduğunda tanımlamak için de kullanabilirsiniz. Bu günlük girişi, silme, bir "HTTP (404) istemci diğer hatası" durum iletisiyle başarısız olduğunu gösterir. Aynı günlük girişi istemci tarafından oluşturulan istek Kimliğini de içerir. **istemci istek kimliği** sütun (813ea74f...).

Sunucu tarafı günlük de aynı başka bir girdiyi içerir **istemci istek kimliği** değeri (813ea74f...) için başarılı bir silme işlemi aynı istemciden ve aynı varlık için. Başarısız istek silmeden önce bu başarılı silme işlemi çok kısa bir süre içinde gerçekleşen.

Bu senaryonun en olası nedeni, istemci varlık için bir silme isteği başarılı oldu, ancak bir bildirim (belki de bir geçici ağ sorunu nedeniyle) sunucudan almadı tablo hizmetine gönderilen ' dir. İstemci ardından otomatik olarak işlem yeniden denenebilir (aynı kullanarak **istemci istek kimliği**), ve varlık zaten silinmiş olduğundan bu yeniden deneme başarısız oldu.

Bu sorun sık sık olursa, tablo hizmetinden bildirimleri almak istemci neden başarısız olduğunu araştırmanıza. Sorun aralıklı ise, "HTTP (404) bulunamadı" hatasını yakalamak ve istemcide oturum ancak devam etmek istemcinin izin.

### <a name="the-client-is-receiving-409-messages"></a>İstemci, HTTP 409 (Çakışma) iletilerini alma
Aşağıdaki tabloda, iki istemci işlemleri için sunucu tarafı günlüğünden ayıklama gösterilmektedir: **DeleteIfExists** hemen bunun ardından **Createıfnotexists** aynı blob kapsayıcı adı kullanıyor. Her istemci işlemi sunucuya ilk gönderilen iki isteklerindeki sonuçları bir **GetContainerProperties** kapsayıcı, ardından olup olmadığını denetlemek için istek **DeleteContainer** veya  **CreateContainer** istek.

| Zaman damgası | İşlem | Sonuç | Kapsayıcı adı | İstemci istek kimliği |
| --- | --- | --- | --- | --- |
| 05:10:13.7167225 |GetContainerProperties |200 |mmcont |c9f52c89-... |
| 05:10:13.8167325 |DeleteContainer |202 |mmcont |c9f52c89-... |
| 05:10:13.8987407 |GetContainerProperties |404 |mmcont |bc881924-… |
| 05:10:14.2147723 |CreateContainer |409 |mmcont |bc881924-… |

İstemci uygulaması kodu siler ve ardından hemen aynı adı kullanarak bir blob kapsayıcısı oluşturur: **Createıfnotexists** yöntemi (istemci istek kimliği bc881924-...) sonunda HTTP 409 (Çakışma) hatasıyla başarısız olur. Bir istemci blob kapsayıcılarını, tabloları veya kuyrukları sildiğinde adın yeniden kullanılabilir hale gelmesi biraz zaman alır.

Silme/yeniden oluşturma düzeni sık tekrarlanıyorsa istemci uygulaması yeni kapsayıcı oluştururken benzersiz kapsayıcı adları kullanmalıdır.

### <a name="metrics-show-low-percent-success"></a>Düşük PercentSuccess ölçümleri göster veya Analiz günlük girişlerini ClientOtherErrors işlem durumundaki işlemlerini sahip
**PercentSuccess** ölçüm başarılı oldu, HTTP durum koduna göre işlemleri yüzdesini yakalar. Durum kodları ile 2XX işlemleri sayısı aralıklarında 3XX, 4XX ve 5XX durum kodları ile işlemleri, başarısız ve daha düşük olarak değerlendirilir ancak olarak başarılı **PercentSuccess** ölçüm değeri. Sunucu tarafı depolama günlük dosyalarında bir işlem durumuyla bu işlemleri kaydedilir **ClientOtherErrors**.

Bu işlemler başarıyla tamamladınız ve kullanılabilirlik gibi diğer ölçümleri etkilemez unutulmaması önemlidir. Başarısız HTTP durum kodlarına yol açabilir ancak başarıyla yürütme işlemlerinin bazı örnekleri şunlardır:

* **ResourceNotFound** (değil bulunan 404), örneğin bir GET isteği mevcut bir bloba'den.
* **ResourceAlreadyExists** (409 çakışma), örneğin bir **CreateIfNotExist** işlem kaynağı zaten mevcut olduğu.
* **ConditionNotMet** (değil değiştiren 304), bir istemci gönderdiğinde gibi örneğin bir koşullu işlemi bir **ETag** değer ve bir HTTP **If-None-Match** yalnızca varsa görüntüyü istemek için üst bilgi Son işlem itibaren güncelleştirilmiş.

Depolama Hizmetleri sayfasında döndüren ortak REST API hata kodlarının listesini bulabilirsiniz [ortak REST API hata kodları](https://msdn.microsoft.com/library/azure/dd179357.aspx).

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Kapasite ölçümleri beklenmedik bir artış depolama kapasitesi kullanımı Göster
Beklenmeyen değişiklikleri, depolama hesabınızdaki kapasite kullanımında ani görürseniz, kullanılabilirlik ölçümlerinizi bakarak nedenleri araştırabilirsiniz; Örneğin, bir artış sayısını silinemedi istekleri uygulamaya özgü temizleme işlemleri boşaltma olması için alan beklediğiniz (örneğin beklendiği gibi çalışmıyor olabilir olarak kullandığınız blob depolama miktarında artış neden olabilir alan boşaltmak için kullanılan SAS belirteçlerini süresi dolduğundan).

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>Sorununuzu geliştirme veya test için depolama öykünücüsünü kullanarak ortaya çıkar.
Genellikle geliştirme sırasında depolama öykünücüsü kullanma ve bir Azure depolama hesabına gereksinimini ortadan kaldırmak için test edin. Depolama öykünücüsü'nü kullanırken oluşabilecek yaygın sorunlar şunlardır:

* [Depolama öykünücüsü'nde özellik "X" çalışmıyor]
* [Hata "HTTP üstbilgileri birinin değeri doğru biçimde değil" depolama öykünücüsü'nü kullanırken]
* [Depolama öykünücüsü'nü çalıştıran yönetimsel ayrıcalıklar gerekiyor]

#### <a name="feature-X-is-not-working"></a>Depolama öykünücüsü'nde özellik "X" çalışmıyor
Depolama öykünücüsü, tüm Azure depolama hizmetleri gibi dosya hizmeti özelliklerini desteklemez. Daha fazla bilgi için bkz. [Geliştirme ve Sınama için Azure Storage Öykünücüsünü Kullanma](storage-use-emulator.md).

Depolama öykünücüsü desteklemediği özellikler için Azure depolama hizmetine bulutta kullanın.

#### <a name="error-HTTP-header-not-correct-format"></a>Hata "HTTP üstbilgileri birinin değeri doğru biçimde değil" depolama öykünücüsü'nü kullanırken
Gibi yerel depolama öykünücüsü ve yöntem çağrıları karşı depolama istemci kitaplığı kullanan uygulamanızı test ettiğiniz **Createıfnotexists** "HTTP üstbilgileri biri için değer doğru değil hata iletisiyle başarısız biçimi." Bu, kullanmakta olduğunuz depolama öykünücüsünün sürümü kullandığınız depolama istemci kitaplığı sürümünü desteklemiyor gösterir. Depolama istemcisi kitaplığı üstbilgisi ekler **x-ms-version** tüm isteklere kolaylaştırır. Depolama öykünücüsü değerini tanımıyor, **x-ms-version** üst bilgi isteği reddeder.

Depolama kitaplığı istemci günlükleri değerini görmek için kullanabileceğiniz **x-ms-version üstbilgi** bunu gönderiyor. Değerini de gördüğünüz **x-ms-version üstbilgi** istekleri istemci uygulamanızı izlemek için fiddler'ı kullanıyorsanız.

Bu senaryo genellikle yükleyin ve depolama öykünücüsü güncelleştirmeden depolama istemci Kitaplığı'nın en son sürümü kullanırsanız oluşur. Ya da depolama öykünücüsünün en son sürümünü yükleyin veya Bulut depolama öykünücüsü yerine geliştirme ve test için kullanın.

#### <a name="storage-emulator-requires-administrative-privileges"></a>Depolama öykünücüsü'nü çalıştıran yönetimsel ayrıcalıklar gerekiyor
Depolama öykünücüsü çalıştırdığınızda sizden yönetici kimlik bilgileri istenir. Bu, yalnızca depolama öykünücüsü ilk kez başlatırken oluşur. Depolama öykünücüsü ayarladıktan sonra tekrar çalıştırmak için yönetici ayrıcalıkları gerekmez.

Daha fazla bilgi için bkz. [Geliştirme ve Sınama için Azure Storage Öykünücüsünü Kullanma](storage-use-emulator.md). Ayrıca yönetici ayrıcalıkları gerektirir Visual Studio depolama öykünücüsünde de başlatabilirsiniz.

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>.NET için Azure SDK'sını yüklerken sorunlarla karşılaşıyoruz.
SDK'sı yüklemeye çalıştığınızda, yerel makinenizde depolama öykünücüsü yüklenmeye çalışılırken başarısız olur. Yükleme günlüğü şu iletilerden birini içerir:

* CAQuietExec:  Hata: SQL örneği erişilemiyor
* CAQuietExec:  Hata: Veritabanı oluşturulamadı

Var olan LocalDB yükleme ile ilgili bir sorun nedenidir. Varsayılan olarak, Azure depolama hizmetleri benzetim, verileri kalıcı hale getirmek için LocalDB depolama öykünücüsü kullanır. SDK'yı yüklemeyi denemeden önce bir komut istemi penceresinde aşağıdaki komutları çalıştırarak uygulamanızı LocalDB örneğini sıfırlayabilirsiniz.

```
sqllocaldb stop v11.0
sqllocaldb delete v11.0
delete %USERPROFILE%\WAStorageEmulatorDb3*.*
sqllocaldb create v11.0
```

**Sil** komut, önceki depolama öykünücüsü yüklemelerinden eski tüm veritabanı dosyaları kaldırır.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>Depolama hizmeti ile farklı bir sorun olması
Önceki sorun giderme bölümlerini bir depolama hizmeti ile karşılaştığınız sorunu eklemezseniz, tanılama ve sorun giderme için aşağıdaki yaklaşımı benimsemeniz.

* Ölçümlerinizin, beklenen taban çizgisinin davranışından herhangi bir değişiklik olduğunu görmek için kontrol edin. Ölçümleri, sorunu geçici veya kalıcı ve hangi depolama işlemleri, sorunu etkileyen olup olmadığını belirlemek mümkün olabilir.
* Ölçüm bilgileri oluşan hatalar hakkında daha ayrıntılı bilgi için sunucu tarafı günlük verileri aramanıza yardımcı olması için kullanabilirsiniz. Bu bilgiler ve sorun gidermek yardımcı.
* Sunucu tarafı günlüklerdeki bilgiler başarıyla sorunu gidermek yeterli değilse, istemci uygulaması ve Fiddler, Wireshark ve Microsoft araçlarıyla davranışını araştırmak için depolama istemci kitaplığı istemci tarafı günlüklerini kullanabilirsiniz Ağınızı araştırmak için ileti Çözümleyicisi'ni kullanın.

Fiddler'ı kullanma hakkında daha fazla bilgi için bkz. "[Ek 1: HTTP ve HTTPS trafiğini yakalamak için Fiddler'ı kullanma]. "

Wireshark kullanma hakkında daha fazla bilgi için bkz. "[Ek 2: Ağ trafiğini yakalamak için Wireshark kullanma]. "

Microsoft ileti Çözümleyicisi'ni kullanma hakkında daha fazla bilgi için bkz. "[Ek 3: Ağ trafiğini yakalamak için Microsoft Message Analyzer'ı kullanma]. "

## <a name="appendices"></a>Ekler
Ek tanılama ve Azure depolama (ve diğer hizmetleri) ile ilgili sorunları giderme yararlanabileceğiniz birçok araç açıklanmaktadır. Bu araçlar, Azure Depolama'nın bir parçası değildir ve bazı üçüncü taraf ürünleri. Bu nedenle, bu eklerin içinde açıklanan araçları, Microsoft Azure veya Azure depolama ile herhangi bir destek sözleşmesi tarafından kapsanmaz ve lisanslama ve Destek seçeneklerini kullanılabilir değerlendirme sürecinizin bir parçası olarak bu nedenle inceleyin Bu araçlar sağlayıcıları.

### <a name="appendix-1"></a>Ek 1: HTTP ve HTTPS trafiğini yakalamak için Fiddler'ı kullanma
[Fiddler](https://www.telerik.com/fiddler) istemci uygulamanızla kullandığınız Azure depolama hizmeti arasında HTTP ve HTTPS trafiğini analiz etmek için kullanışlı bir araçtır.

> [!NOTE]
> Fiddler, HTTPS trafiği çözebilen; dikkatli bir şekilde, bunu nasıl yaptığını anlamak ve güvenlik etkilerini anlamak için fiddler'ı belgeleri okumanız gerekir.
>
>

Bu ekte, Fiddler'ı yüklediğiniz yerel makine ile Azure depolama hizmetleri arasında trafiği yakalamak için fiddler'ı yapılandırmak hakkında kısa bir kılavuz sağlar.

Fiddler başlattıktan sonra yerel makinenizde HTTP ve HTTPS trafiğini yakalamaktan başlar. Fiddler'ı denetleme için bazı yararlı komutlar aşağıda verilmiştir:

* Durdurup trafiğini yakalamaktan çalıştırın. Ana menüde Git **dosya** ve ardından **trafik yakalama** açıp yakalama değiştirilecek.
* Yakalanan trafik verileri kaydedin. Ana menüde Git **dosya**, tıklayın **Kaydet**ve ardından **tüm oturumları**: Bu trafiği bir oturumu arşiv kaydetmenize olanak sağlar. Bir oturum arşiv analiz için daha sonra yeniden yükleyin veya Microsoft Desteği'ne istenirse gönderebilirsiniz.

Fiddler yakalayan trafik miktarını sınırlamak için yapılandırdığınız filtreleri kullanabilirsiniz **filtreleri** sekmesi. Yalnızca gönderilen trafik yakalayan bir filtre aşağıdaki ekran görüntüsünde gösterilmektedir **contosoemaildist.table.core.windows.net** depolama uç noktası:

![][5]

### <a name="appendix-2"></a>Ek 2: Ağ trafiğini yakalamak için Wireshark kullanma
[Wireshark](https://www.wireshark.org/) çok çeşitli ağ protokolleri için ayrıntılı paket bilgilerini görüntülemenize olanak sağlayan bir ağ protokol çözümleyici.

Aşağıdaki yordam Wireshark için tablo hizmeti, Azure depolama hesabınızdaki yüklediğiniz yerel makine giden trafik için ayrıntılı paket bilgilerini yakalama gösterir.

1. Yerel makinenizde Wireshark başlatın.
2. İçinde **Başlat** bölümünde, internet'e bağlı arabirimler ve yerel ağ arabirimi seçin.
3. Tıklayın **kaydetme seçeneklerini**.
4. Bir filtre ekleyerek **yakalama filtresi** metin. Örneğin, **contosoemaildist.table.core.windows.net konak** Wireshark veya tablo Hizmeti uç noktası gönderilen paketlerin yakalamak için yapılandırıp yapılandırmayacağınız **contosoemaildist** depolama hesabı. Kullanıma [yakalama filtreleri tam listesi](https://wiki.wireshark.org/CaptureFilters).

   ![][6]
5. **Başlat**'a tıklayın. Wireshark tüm istemci uygulamanızı yerel makinenizde kullanırken, paketleri tablo Hizmeti uç noktası almasına veya göndermesine yakalar.
6. Tamamladığınızda, ana menüye tıklayarak **yakalama** ardından **Durdur**.
7. Wireshark yakalama dosyasında yakalanan verileri kaydetmek için ana menüde'ı **dosya** ardından **Kaydet**.

WireShark mevcut hataları vurgular **packetlist** penceresi. De kullanabilirsiniz **Uzman bilgileri** penceresi (tıklayın **Çözümle**, ardından **Uzman bilgileri**) hataları ve Uyarıları özetini görüntülemek için.

![][7]

TCP veri sağ tıklatıp seçerek uygulama katmanı tarafından görülen şekilde TCP verileri görüntülemek de seçebilirsiniz **izleyin TCP Stream**. Bu, döküm yakalama filtresi olmadan, yakalanan yararlıdır. Daha fazla bilgi için [TCP akışları aşağıdaki](https://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html).

![][8]

> [!NOTE]
> Wireshark kullanma hakkında daha fazla bilgi için bkz. [Wireshark Kullanıcı Kılavuzu](https://www.wireshark.org/docs/wsug_html_chunked).
>
>

### <a name="appendix-3"></a>Ek 3: Ağ trafiğini yakalamak için Microsoft Message Analyzer'ı kullanma
Fiddler'a benzer bir şekilde HTTP ve HTTPS trafiğini yakalayın ve Wireshark benzer bir şekilde ağ trafiğini yakalamak Microsoft ileti Çözümleyicisi'ni kullanabilirsiniz.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Microsoft ileti Çözümleyicisi'ni kullanarak web izleme oturumu yapılandırma
Microsoft Message Analyzer uygulamayı çalıştırmak, Microsoft Message Analyzer'ı kullanarak HTTP ve HTTPS trafiğini ve ardından web izleme oturumu yapılandırmak için **dosya** menüsünde tıklatın **yakalama/Trace**. Kullanılabilir izleme senaryoları listesinde seçin **Web Proxy**. Ardından **izleme senaryo Yapılandırması** panelinde **HostnameFilter** metin depolama uç noktalarınızı adlarını ekleyin (Bu adları arayabilirsiniz [Azure portalında](https://portal.azure.com)). Örneğin, Azure depolama hesabınızın adını ise **contosodata**, aşağıdaki eklemelisiniz **HostnameFilter** metin:

```
contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net
```

> [!NOTE]
> Bir boşluk karakteri, ana bilgisayar adlarını ayırır.
>
>

İzleme verileri toplamaya başlamak hazır olduğunuzda **Start WITH** düğmesi.

Microsoft Message Analyzer hakkında daha fazla bilgi için **Web Proxy** izlemek için bkz: [Microsoft PEF WebProxy sağlayıcısı](https://technet.microsoft.com/library/jj674814.aspx).

Yerleşik **Web Proxy** Microsoft Message Analyzer izlemede üzerinde fiddler'ı temel alır; istemci tarafı HTTPS trafiği yakalayabilir ve şifrelenmiş HTTPS iletileri görüntüler. **Web Proxy** şifrelenmiş iletileri için erişim sağlayan tüm HTTP ve HTTPS trafiği için bir yerel ara yapılandırarak works izleme.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Microsoft Message Analyzer'ı kullanarak ağ sorunlarını tanılama
Microsoft Message Analyzer'ı kullanmanın yanı sıra **Web Proxy** istemci uygulaması ve depolama hizmeti arasında HTTP/HTTPs trafiğini ayrıntılarını yakalamak için izleme, yerleşik kullanabilirsiniz **yerel bağlantı katmanı**  ağ paket bilgilerini yakalamak için izleme. Bırakılan paketleri ile Wireshark yakalamak ve tanılama benzer verilerini yakalama için gibi ağ sorunları bu etkinleştirir.

Aşağıdaki anlık görüntüde bir örnek gösterilmektedir **yerel bağlantı katmanı** bazı izleme **bilgilendirici** içinde iletileri **DiagnosisTypes** sütun. Simgeye tıklayarak **DiagnosisTypes** sütun iletisi ayrıntılarını gösterir. Bu örnekte, bir bildirim istemciden almadığı için ileti #305 sunucu da yeniden iletilemez:

![][9]

Microsoft ileti Çözümleyicisi'nde bir izleme oturumu oluşturma, izleme gürültü olasılığını azaltmak için filtreler belirtebilirsiniz. Üzerinde **yakalama / Trace** izleme tanımladığınız bir sayfa tıklayın **yapılandırma** yanındaki bağlantı **Microsoft-Windows-NDIS-PacketCapture**. Aşağıdaki ekran görüntüsünde, üç depolama hizmetleri IP adreslerini TCP trafiği filtreleyen bir yapılandırma gösterilmektedir:

![][10]

Microsoft Message Analyzer yerel bağlantı katmanı izleme hakkında daha fazla bilgi için bkz: [Microsoft PEF NDIS PacketCapture sağlayıcısı](https://technet.microsoft.com/library/jj659264.aspx).

### <a name="appendix-4"></a>Ek 4: Ölçümleri görüntüleyin ve verileri günlüğe kaydetmek için Excel kullanma
Birçok araç depolama ölçüm verilerini görüntüleme ve çözümleme için Excel'e verileri yüklemek kolaylaştıran bir sınırlandırılmış biçimde Azure tablo Depolama'dan indirme olanak sağlar. Depolama günlük verilerini Azure blob depolamadan Excel'e yükleyebilir ayrılmış bir biçimde zaten var. Ancak, uygun sütun başlıkları bilgilerine merkezli eklemeniz gerekecektir [depolama analizi günlük biçimi](https://msdn.microsoft.com/library/azure/hh343259.aspx) ve [Storage Analytics Ölçüm tablosu şeması](https://msdn.microsoft.com/library/azure/hh343264.aspx).

Blob depolama alanından indirdikten sonra depolama günlük verilerinizi Excel'e aktarmak için:

* Üzerinde **veri** menüsünü tıklatın **metindeki**.
* İstediğiniz görüntülemek ve günlük dosyasına göz **alma**.
* Adım 1 / **metin İçeri Aktarma Sihirbazı'nı**seçin **sınırlandırılmış**.

Adım 1 / **metin İçeri Aktarma Sihirbazı'nı**seçin **noktalı virgül** yalnızca sınırlayıcı olarak çift tırnak olarak seçin **Metin niteleyicisi**. Ardından **son** ve çalışma kitabınızda veri yerleştirileceği yeri seçin.

### <a name="appendix-5"></a>Ek 5: Azure DevOps için Application Insights ile izleme
Performans ve kullanılabilirlik izlemesi bir parçası olarak Azure DevOps için Application Insights özelliğini de kullanabilirsiniz. Bu araç şunları yapabilir:

* Web hizmetinizin kullanılabilir ve yanıt verdiğinden emin olun. Uygulamanızı bir web sitesi veya web hizmeti kullanan bir cihaz uygulaması olsun, bu konumlardan dünyanın dört bir yanındaki birkaç dakikada URL'nizi test edin ve bir sorun olup olmadığını size bildirmek.
* Herhangi bir web hizmetinizde özel durum veya performans sorunları hızla tanılayın. CPU veya diğer kaynaklara uzatılır, özel durumlardan yığın izlemelerini alın öğrenmek ve günlük izlemelerini kolayca arama yapın. Microsoft, uygulamanın performansı kabul edilebilir sınırları düşerse, bir e-posta gönderebilirsiniz. Hem .NET hem de Java web Hizmetleri'ni izleyebilirsiniz.

Daha fazla bilgi bulabilirsiniz [Application Insights nedir](../../azure-monitor/app/app-insights-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure Storage analytics hakkında daha fazla bilgi için şu kaynaklara bakın:

* [Azure portalında depolama hesabı izleme](storage-monitor-storage-account.md)
* [Depolama analizi](storage-analytics.md)
* [Storage analytics ölçümleri](storage-analytics-metrics.md)
* [Storage analytics ölçüm tablosu şeması](/rest/api/storageservices/storage-analytics-metrics-table-schema)
* [Depolama analizi günlüklerinde](storage-analytics-logging.md)
* [Depolama analizi günlük biçimi](/rest/api/storageservices/storage-analytics-log-format)

<!--Anchors-->
[Giriş]: #introduction
[Bu kılavuz nasıl düzenlenir]: #how-this-guide-is-organized

[Depolama Hizmeti izleme]: #monitoring-your-storage-service
[Hizmet durumu izleme]: #monitoring-service-health
[İzleme kapasitesi]: #monitoring-capacity
[Kullanılabilirliği izleme]: #monitoring-availability
[Performans izleme]: #monitoring-performance

[Depolama sorunları tanılama]: #diagnosing-storage-issues
[Hizmet sistem durumu sorunları]: #service-health-issues
[Performans sorunları]: #performance-issues
[Hataları tanılama]: #diagnosing-errors
[Depolama öykünücüsü sorunları]: #storage-emulator-issues
[Depolama günlük araçları]: #storage-logging-tools
[Ağ günlük araçları kullanma]: #using-network-logging-tools

[Uçtan uca izleme]: #end-to-end-tracing
[Günlük verileri ilişkilendirme]: #correlating-log-data
[İstemci istek kimliği]: #client-request-id
[Sunucu istek kimliği]: #server-request-id
[Zaman damgaları]: #timestamps

[Sorun giderme rehberi]: #troubleshooting-guidance
[Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor, ancak istemci yüksek gecikme durumu yaşıyor]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Ölçümler yüksek AverageServerLatency gösteriyor]: #metrics-show-high-AverageServerLatency
[Kuyrukta ileti tesliminde beklenmeyen gecikmeler yaşıyorsunuz]: #you-are-experiencing-unexpected-delays-in-message-delivery

[Ölçümler PercentThrottlingError’da artış gösteriyor]: #metrics-show-an-increase-in-PercentThrottlingError
[Geçici Percentthrottlingerror'da artış]: #transient-increase-in-PercentThrottlingError
[Kalıcı hata Percentthrottlingerror'da artış]: #permanent-increase-in-PercentThrottlingError
[Ölçümler PercentTimeoutError’da artış gösteriyor]: #metrics-show-an-increase-in-PercentTimeoutError
[Ölçümler PercentNetworkError’da artış gösteriyor]: #metrics-show-an-increase-in-PercentNetworkError

[İstemci HTTP 403 (Yasak) iletilerini alıyor]: #the-client-is-receiving-403-messages
[İstemci HTTP 404 (Bulunamadı) iletilerini alıyor]: #the-client-is-receiving-404-messages
[İstemci veya başka bir işlem daha önce nesneyi silmiş]: #client-previously-deleted-the-object
[Bir paylaşılan erişim imzası (SAS) yetkilendirme sorunu]: #SAS-authorization-issue
[İstemci tarafı JavaScript kodu nesneye erişim izni yok]: #JavaScript-code-does-not-have-permission
[Ağ hatası]: #network-failure
[İstemci HTTP 409 (Çakışma) iletileri alma]: #the-client-is-receiving-409-messages

[Düşük PercentSuccess ölçümleri göster veya ClientOtherErrors işlem durumundaki işlemlerini analytics günlük girdilerine sahip]: #metrics-show-low-percent-success
[Kapasite ölçümlerini beklenmeyen artışı depolama kapasitesi kullanımı Göster]: #capacity-metrics-show-an-unexpected-increase
[Sorununuzu geliştirme veya test için depolama öykünücüsünü kullanarak ortaya çıkar.]: #your-issue-arises-from-using-the-storage-emulator
[Depolama öykünücüsü'nde özellik "X" çalışmıyor]: #feature-X-is-not-working
[Hata "HTTP üstbilgileri birinin değeri doğru biçimde değil" depolama öykünücüsü'nü kullanırken]: #error-HTTP-header-not-correct-format
[Depolama öykünücüsü'nü çalıştıran yönetimsel ayrıcalıklar gerekiyor]: #storage-emulator-requires-administrative-privileges
[.NET için Azure SDK'sını yüklerken sorunlarla karşılaşıyoruz.]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[Bir depolama hizmetindeki farklı bir sorun olması]: #you-have-a-different-issue-with-a-storage-service

[Ekler]: #appendices
[Ek 1: HTTP ve HTTPS trafiğini yakalamak için Fiddler'ı kullanma]: #appendix-1
[Ek 2: Ağ trafiğini yakalamak için Wireshark kullanma]: #appendix-2
[Ek 3: Ağ trafiğini yakalamak için Microsoft Message Analyzer'ı kullanma]: #appendix-3
[Ek 4: Ölçümleri görüntüleyin ve verileri günlüğe kaydetmek için Excel kullanma]: #appendix-4
[Ek 5: Azure DevOps için Application Insights ile izleme]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting/overview.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-2.png
