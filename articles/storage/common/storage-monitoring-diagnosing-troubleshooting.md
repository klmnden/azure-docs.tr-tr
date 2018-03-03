---
title: "İzleme, tanılama ve Azure Storage sorunlarını giderme | Microsoft Docs"
description: "Storage analytics, istemci-tarafı günlüğe kaydetme ve tanımlamak için diğer üçüncü taraf araçları tanılamak ve Azure Storage ile ilgili sorunları giderme gibi özellikleri kullanabilirsiniz."
services: storage
documentationcenter: 
author: fhryo-msft
manager: jahogg
editor: tysonn
ms.assetid: d1e87d98-c763-4caa-ba20-2cf85f853303
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: fhryo-msft
ms.openlocfilehash: b89071048594e1e11efb321da3d0b48005824b46
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Microsoft Azure Storage izleme, tanılama ve sorun giderme
[!INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Genel Bakış
Tanılama ve bulut ortamında bulunan bir dağıtılmış uygulama sorunlarını giderme geleneksel ortamlarda daha karmaşık olabilir. Uygulamalar şirket içinde bir mobil cihazda veya bu ortamları bileşimi bir PaaS veya Iaas altyapısında dağıtılabilir. Genellikle, uygulamanızın ağ trafiği ortak ve özel ağlar geçiş yapabilir ve uygulamanızı Microsoft Azure depolama tabloları, Bloblar, kuyruklar gibi birden çok depolama teknolojileri kullanabilir veya diğer veri ek dosyaları gibi saklar olarak ilişkisel ve belge veritabanları.

Bu tür uygulamalar başarılı bir şekilde yönetmek için proaktif olarak izlemek ve tanılamak ve bunları ve bunların bağımlı teknolojiler tüm yönlerini sorunlarını gidermek nasıl anlamanız gerekir. Bir Azure Depolama Hizmetleri kullanıcı olarak sürekli beklenmeyen değişiklikler (örneğin, normal yanıt süreleri daha yavaş) davranışı, uygulamanızın kullandığı depolama hizmetleri izlemek ve günlük daha ayrıntılı verileri toplamak ve kapsamlı bir sorunu çözümlemek için kullanmanız gerekir. İzleme hem günlük elde tanılama bilgileri uygulamanızı karşılaşılan sorun kök nedenini belirlemenize yardımcı olur. Ardından sorunu gidermek ve düzeltmek için atabileceğiniz uygun adımları belirleyin. Azure depolama çekirdeği Azure hizmeti ve müşteriler için Azure altyapıyı çözümlerinin çoğu önemli bir parçasını oluşturur. Azure depolama izleme, tanılama ve depolama sorunlarını bulut tabanlı uygulamalar kolaylaştıran özellikler içerir.

> [!NOTE]
> Azure dosyaları şu anda kaydını desteklemiyor.
> 

Uçtan uca Azure Storage uygulamalarda sorun giderme uygulamalı kılavuzu için bkz: [uçtan uca Azure Storage ölçümleri ve günlüğe kaydetme, AzCopy ve ileti Çözümleyicisi'ni kullanarak sorun giderme](../storage-e2e-troubleshooting.md).

* [Giriş]
  * [Bu kılavuz nasıl düzenlenir]
* [, depolama hizmet izlemesini]
  * [Hizmet durumu izleme]
  * [Kapasite izleme]
  * [Kullanılabilirlik izlemesi]
  * [Performans izleme]
* [depolama sorunları tanılama]
  * [Hizmet sistem durumu sorunları]
  * [performans sorunları]
  * [Hatalarını tanılama]
  * [Depolama öykünücüsü sorunları]
  * [Depolama günlük araçları]
  * [Ağ günlük araçlarını kullanarak]
* [uçtan uca izleme]
  * [Günlük verileri ilişkilendirme]
  * [İstemci istek kimliği]
  * [sunucu istek kimliği]
  * [Zaman damgaları]
* [sorun giderme kılavuzluğu]
  * [ölçümleri Göster yüksek AverageE2ELatency ve düşük AverageServerLatency]
  * [Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor, ancak istemci yüksek gecikme durumu yaşıyor]
  * [Ölçümler yüksek AverageServerLatency gösteriyor]
  * [Kuyrukta ileti tesliminde beklenmeyen gecikmeler yaşıyorsunuz]
  * [ölçümleri Göster artışı içinde PercentThrottlingError]
  * [ölçümleri Göster artışı içinde PercentTimeoutError]
  * [Ölçümler PercentNetworkError’da artış gösteriyor]
  * [İstemci HTTP 403 (Yasak) iletileri alma]
  * [İstemci HTTP 404 (bulunamadı) iletileri alma]
  * [İstemci HTTP 409 (Çakışma) iletileri alma]
  * [ölçümleri Göster düşük PercentSuccess veya analytics günlük girişlerini sahip hareket durumu işlemler ClientOtherErrors,]
  * [Kapasite ölçümlerini beklenmeyen artışı depolama kapasitesi kullanımı Göster]
  * [Sorununuzu geliştirme veya test için depolama öykünücüsünü kullanarak ortaya çıkar.]
  * [.NET için Azure SDK'sını yükleme sorunlarla]
  * [Bir depolama hizmetindeki farklı bir sorun olması]
  * [Windows sanal makinelerde VHD'ler sorunlarını giderme](../../virtual-machines/windows/troubleshoot-vhds.md)   
  * [Linux sanal makineleri VHD'ler sorunlarını giderme](../../virtual-machines/linux/troubleshoot-vhds.md)
  * [Windows Azure dosyaları sorunlarını giderme](../files/storage-troubleshoot-windows-file-connection-problems.md)   
  * [Linux Azure dosyaları sorunlarını giderme](../files/storage-troubleshoot-linux-file-connection-problems.md)
* [ekler]
  * [ek 1: HTTP ve HTTPS trafiğini yakalamak için Fiddler kullanarak]
  * [ek 2: ağ trafiğini yakalamak için Wireshark kullanarak]
  * [ek 3: ağ trafiğini yakalamak için Microsoft Message Analyzer'ı kullanarak]
  * [Ek 4: Ölçümleri görüntülemek ve verileri günlüğe kaydetmek için Excel kullanma]
  * [Ek 5: Visual Studio Team Services için Application Insights ile izleme]

## <a name="introduction"></a>Giriş
Bu kılavuz, Azure depolama çözümlemeleri gibi özelliklerinin nasıl kullanılacağını göstermektedir istemci-tarafı Azure Storage istemci kitaplığı ve diğer üçüncü taraf araçları tanımlamak, tanılama ve Azure Storage sorun giderme için günlük kaydı ile ilgili sorunlar.

![][1]

Bu kılavuz, Azure Storage Hizmetleri ve BT uzmanlarının gibi çevrimiçi hizmetlere yönetmekten sorumlu kullandığınız çevrimiçi hizmetlere geliştiricileri öncelikle tarafından okunacak yöneliktir. Bu kılavuzun hedefi şunlardır:

* Sistem durumunu ve performansını Azure Storage hesaplarınızı sürdürmenize yardımcı olmak için.
* Bir sorunu veya bir uygulamadaki sorun Azure depolama birimine ilişkili olup olmadığını karar vermenize yardımcı olacak araçlar ve gerekli işlemleri ile sağlamak için.
* Azure depolama birimine ilgili sorunları çözmek için işlem yapılabilir yönlendirme ile sağlamak için.

### <a name="how-this-guide-is-organized">Bu kılavuz nasıl düzenlenir</a>
Bölüm "[, depolama hizmet izlemesini]" sistem durumu ve Azure Storage Analytics ölçümleri (Storage ölçümleri) kullanarak, Azure depolama hizmetleri performansını izleme açıklar.

Bölüm "[depolama sorunları tanılama]" Azure Storage Analytics günlüğü (depolama oturum açma) kullanarak sorunları tanılamak açıklar. Ayrıca, istemci kitaplıklarından birini tesislerde .NET veya Java için Azure SDK'sı için depolama istemci kitaplığı gibi kullanarak istemci tarafı günlük kaydını etkinleştirmek nasıl açıklanır.

Bölüm "[uçtan uca izleme]" çeşitli günlük dosyalarını ve ölçüm verilerini yer alan bilgilerin nasıl ilişkilendirebilirsiniz açıklar.

Bölüm "[sorun giderme kılavuzluğu]" karşılaşabileceğiniz bazı yaygın depolama ile ilgili sorunlar için sorun giderme kılavuzu sağlar.

"[ekler]" Çözümleme ağ paket verileri, HTTP/HTTPS iletileri, çözümleme için fiddler'ı ve Microsoft Message Analyzer'ı ilişkilendirme için günlük verileri için Wireshark ve Netmon gibi diğer araçları kullanma hakkında bilgi içerir.

## <a name="monitoring-your-storage-service">Depolama hizmet izleme</a>
Windows performans izleme ile hakkında bilginiz varsa, depolama ölçümlerini Windows Performans İzleyicisi sayaçları Azure Storage denk olarak düşünebilirsiniz. Depolama ölçümlerini ölçümleri (Windows Performans İzleyicisi terminolojisi sayaçları) hizmet kullanılabilirliği, hizmet isteklerinin toplam sayısı veya hizmetine başarılı istek yüzdesi gibi kapsamlı bir kümesini bulur. Kullanılabilir ölçümler tam bir listesi için bkz: [Storage Analytics Ölçüm tablosu şeması](http://msdn.microsoft.com/library/azure/hh343264.aspx). Her saat veya dakikada ölçümleri toplama için depolama birimi hizmeti isteyip istemediğinizi belirtebilirsiniz. Ölçümleri etkinleştirmek ve depolama hesaplarınızı izlemek hakkında daha fazla bilgi için bkz: [depolama ölçümlerini etkinleştirme ve ölçüm verilerini görüntüleme](http://go.microsoft.com/fwlink/?LinkId=510865).

Görüntülemek istediğiniz hangi saatlik ölçümleri seçebilirsiniz [Azure portal](https://portal.azure.com) bir saatlik ölçümü belirli bir eşiği aştığında, yöneticilerin e-posta ile bildirim kuralları yapılandırın. Daha fazla bilgi için bkz: [uyarı bildirimleri alma](/azure/monitoring-and-diagnostics/monitoring-overview-alerts). 

Depolama hizmetinin en iyi çaba kullanarak ölçümleri toplar, ancak her depolama işlemi kayıt.

Azure Portal'da ölçümleri kullanılabilirlik, toplam istek sayısı ve bir depolama hesabı için ortalama gecikme süresi numaraları gibi görüntüleyebilirsiniz. Bir bildirim kuralı da kullanılabilirlik belirli bir düzeyin altına düşerse bir yönetici sizi uyarmak için ayarlanmış olan. Bu verileri görüntülemelerini, bir olası araştırma için % 100 olan tablo hizmeti başarı oranı alanıdır (daha fazla bilgi için bkz "[ölçümleri Göster düşük PercentSuccess veya analytics günlük girişlerini sahip hareket durumu işlemler ClientOtherErrors,]").

Sürekli olarak sağlıklı ve tarafından beklendiği gibi gerçekleştirme olduklarından emin olmak için Azure uygulamalarınızı izlemeniz gerekir:

* Bazı temel ölçümleri geçerli verileri karşılaştırmak ve Azure depolama ve uygulamanızın davranışını önemli değişiklikler belirlemenize olanak tanır uygulaması oluşturma. Taban çizgisi ölçümlerinizi değerlerini çoğu durumda, uygulamaya özel olacaktır ve uygulamanızı test etme performans olduğunda bunları oluşturmanız gerekir.
* Dakika ölçümleri kaydetme ve bunları beklenmeyen hatalar ve hata ani gibi daha fazla bilgi için etkin olarak izlemek için kullanarak sayar veya oranları isteyin.
* Saatlik ölçümleri kaydetme ve ortalama değerler gibi izlemek üzere onları kullanmasına hata sayıları ortalama ve oranları isteyin.
* Sonraki bölümde açıklandığı gibi tanılama araçlarını kullanarak olası sorunları Araştırıyor "[depolama sorunları tanılama]."

Aşağıdaki resimde grafiklerde nasıl saatlik ölçümlerini oluşur ortalaması ani etkinliğinde gizleyebilirsiniz gösterilmektedir. Bir hızda isteklerinin gerçekleşmekte olan gerçekten dalgalanmaları ölçümleri ortaya dakika sırasında göstermek için saatlik ölçümleri görünür.

![][3]

Bu bölüm geri kalanı izlemek hangi ölçümleri açıklar ve neden.

### <a name="monitoring-service-health">Hizmet durumu izleme</a>
Kullanabileceğiniz [Azure portal](https://portal.azure.com) dünyanın tüm Azure bölgeleri Depolama Birimi Hizmeti (ve diğer Azure hizmetleriyle) durumunu görüntülemek için. Etkinleştirir izleme, bir sorun varsa, denetim dışında hemen görmek için uygulamanız için kullandığınız bölgede depolama hizmeti etkiliyor.

[Azure portal](https://portal.azure.com) çeşitli Azure hizmetlerine etkileyen olayların bildirimleri de sağlayabilirsiniz.
Not: Bu bilgiler bunların geçmiş veriler üzerinde önceden kullanılabilir [Azure hizmet Panosu'nu](http://status.azure.com).

Sırada [Azure portal](https://portal.azure.com) sistem durumu bilgilerini toplar gelen (Inside out izleme), Azure veri merkezleri içinde ayrıca düzenli aralıklarla Azure barındırılan web uygulamanız birden çok konumdan erişmek yapay işlemler oluşturmak için bir dış bileşenini yaklaşım benimsenmesi deneyebilirsiniz. Tarafından sunulan hizmetler [Dynatrace](http://www.dynatrace.com/en/synthetic-monitoring) ve Visual Studio Team Services için Application Insights Bu yaklaşımın örnekler verilmiştir. Ek Visual Studio Team Services için Application Insights hakkında daha fazla bilgi için bkz: "[ek 5: Visual Studio Team Services için Application Insights ile izleme](#appendix-5)."

### <a name="monitoring-capacity">Kapasite izleme</a>
Depolama ölçümleri BLOB'ları tipik olarak depolanan veriler büyük oranda için hesap için blob hizmeti için kapasite ölçümlerini yalnızca depolar (yazıldığı sırada, tablolar ve Kuyruklar kapasitesini izlemek için depolama ölçümleri kullanmak mümkün değildir). Bu verilerde bulabilirsiniz **$MetricsCapacityBlob** Blob hizmeti için izleme etkinleştirilirse tablo. Depolama ölçümleri günde bir kez bu verileri kaydeder ve değerini kullanabilir **RowKey** satır kullanıcı verilerini ilişkili bir varlık içerip içermediğini belirlemek için (değer **veri**) ya da analiz verileri (değer **analytics**). Saklı her varlık kullanılan depolama alanı miktarı hakkında bilgi içerir (**kapasite** bayt cinsinden ölçülür) ve kapsayıcılar geçerli sayısı (**ContainerCount**) ve blobları (**ObjectCount**) depolama hesabı kullanımda. Depolanan kapasite ölçümleri hakkında daha fazla bilgi için **$MetricsCapacityBlob** tablo için bkz: [Storage Analytics Ölçüm tablosu şeması](http://msdn.microsoft.com/library/azure/hh343264.aspx).

> [!NOTE]
> Bu değerler, depolama hesabının kapasite limitlerini yaklaştığı erken bir uyarı için izlemeniz gerekir. Azure portalında birleşik depolama kullanımı aşıyor veya belirttiğiniz eşiklerin altına düştüğünde size bildirmek için uyarı kuralı ekleyebilirsiniz.
> 
> 

Yardım almak için blog gönderisine bakın BLOB'lar gibi çeşitli depolama nesneleri boyutunu tahmin etme [anlama Azure depolama faturalama – bant genişliği, işlemleri ve kapasite](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

### <a name="monitoring-availability">Kullanılabilirlik izlemesi</a>
Depolama hizmetlerinin kullanılabilirliğini değeri izleyerek depolama hesabınızdaki izlemeniz gerekir **kullanılabilirlik** saat veya dakika ölçümleri tablo sütununda — **$MetricsHourPrimaryTransactionsBlob**, **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$MetricsCapacityBlob**. **Kullanılabilirlik** sütunu içeriyor hizmeti veya satırın tarafından temsil edilen API işlemi kullanılabilirliğini gösteren bir yüzde değeri ( **RowKey** satır ölçümleri bir bütün olarak hizmet veya belirli bir API işlemi için bulunup bulunmadığını gösterir).

% 100'değerinden küçük bir değer gösterir bazı depolama istekleri başarısız oluyor. Bunlar farklı hata türleri ile isteklerinin sayısı gibi göster ölçüm verilerini diğer sütunlardaki inceleyerek neden çözümleyemiyor görebilirsiniz **ServerTimeoutError**. Görmeyi beklemelisiniz **kullanılabilirlik** sonbaharda geçici olarak hizmet sırasında geçici sunucu zaman aşımı gibi nedenlerle % 100 aşağıda taşır bölümleri daha iyi yük dengelemesi isteğine; yeniden deneme mantığı, istemci uygulamanızda aralıklı gibi koşullar işlemelidir. Makaleyi [depolama Analytics günlüğe yazılan işlemler ve durum iletileri](http://msdn.microsoft.com/library/azure/hh343260.aspx) depolama ölçümlerini içerir işlem türlerini listeler kendi **kullanılabilirlik** hesaplama.

İçinde [Azure portal](https://portal.azure.com), olmadığını bildirmek için uyarı kuralı ekleyebilirsiniz **kullanılabilirlik** hizmet belirttiğiniz bir eşiğin altına düşmesi için.

"[sorun giderme kılavuzluğu]" başlığına bakın kullanılabilirliğini ilgili bazı yaygın depolama hizmeti sorunlar anlatılmaktadır.

### <a name="monitoring-performance">Performans izleme</a>
Depolama Hizmetleri performansını izlemek için aşağıdaki ölçümleri saat ve dakika ölçümleri tablolardaki kullanabilirsiniz.

* Değerler **AverageE2ELatency** ve **AverageServerLatency** sütunları göster ortalama süre depolama hizmeti veya API işlem türü için işlem istekleri sürüyor. **AverageE2ELatency** isteği okumak ve yanıt isteğini işlemek için harcanan süre ek olarak göndermek için geçen süre dahildir uçtan uca gecikme ölçüsüdür (istek depolama ulaştığında bu nedenle ağ gecikmesi içerir Hizmeti); **AverageServerLatency** bir ölçüdür yalnızca işlem süresi ve bu nedenle istemcinin iletişim kurmasına ilgili tüm ağ gecikmesi dışlar. Bölümüne bakın "[ölçümleri Göster yüksek AverageE2ELatency ve düşük AverageServerLatency]" neden Tartışması için bu kılavuzda daha sonra bu iki değer arasında önemli bir fark olabilir.
* Değerler **Totalıngress** ve **TotalEgress** gelen ve depolama hizmeti dışında veya belirli bir API işlemi türü üzerinden giderek sütunlar veri, toplam miktarını bayt cinsinden gösterir.
* Değerler **TotalRequests** Sütun Göster API işlemi depolama hizmetini alma isteklerinin toplam sayısı. **TotalRequests** depolama hizmetinin aldığı isteklerinin toplam sayısı.

Genellikle, bu değerleri beklenmeyen değişiklikleri araştırma gerektiren bir sorun olduğunu bir göstergesi olarak izlenir.

İçinde [Azure portal](https://portal.azure.com), bu hizmet için performans ölçümleri hiçbirini altına düşmesine ya da belirttiğiniz bir eşiği aşması durumunda sizi bilgilendirmesi için uyarı kuralı ekleyebilirsiniz.

"[sorun giderme kılavuzluğu]" başlığına bakın performansı ile ilgili bazı yaygın depolama hizmeti sorunlar anlatılmaktadır.

## <a name="diagnosing-storage-issues">Depolama sorunları tanılama</a>
Çeşitli yollarla, uygulamanızda bir sorun veya sorun uyumlu hale gelebilir vardır dahil olmak üzere:

* Uygulamanın kilitlenmesine veya çalışmayı durdurmasına neden önemli bir hata.
* Önceki bölümde açıklandığı gibi izlemekte ölçümleri temel değerlerinden önemli değişiklikler "[, depolama hizmet izlemesini]."
* Raporlar, uygulamanızın belirli bazı işleminin beklendiği gibi tamamlanmadığını kullanıcılardan veya bazı özelliği çalışmıyor.
* Uygulama içinde oluşturulan hatalar, günlük dosyalarında veya başka bir bildirim yöntem aracılığıyla görünür.

Genellikle, Azure depolama hizmetleri ile ilgili sorunları geniş dört kategoriden ayrılır:

* Uygulamanız, kullanıcılar tarafından bildirilen ya da performans ölçümleri değişikliklerinden ortaya bir performans sorunu var.
* Bir veya daha fazla bölgelerdeki Azure Storage altyapı ile ilgili bir sorun yoktur.
* Uygulamanızı kullanıcılarınız tarafından bildirilen veya izlemenizi hata sayısı ölçümleri bir artış tarafından açığa hatayla karşılaşıyor.
* Geliştirme ve test sırasında yerel depolama öykünücüsünü kullanarak; Depolama öykünücüsü sahip kullanım için özellikle ilgili bazı sorunlarla karşılaşabilirsiniz.

Aşağıdaki bölümlerde izleyeceğiniz adımlar verilmiştir tanılamak ve her şu dört kategoriden sorunlarını gidermek için. Bölüm "[sorun giderme kılavuzluğu]" Bu kılavuzda daha sonra karşılaşabileceğiniz bazı yaygın sorunlar için daha fazla ayrıntı sağlar.

### <a name="service-health-issues">Hizmet sistem durumu sorunları</a>
Hizmet durumu genellikle denetimi dışında sorunlardır. [Azure portal](https://portal.azure.com) depolama hizmetleri de dahil olmak üzere Azure Hizmetleri ile devam eden sorunları hakkında bilgi sağlar. Depolama hesabınızı oluştururken okuma erişimli coğrafi olarak yedekli depolama için ettiyseniz verileriniz birincil konumda kullanılamaz hale gelirse sonra uygulamanızın geçici olarak ikincil konumdaki salt okunur kopyaya geçiş yapabilirsiniz. Uygulamanızı ikincil okumak için birincil ve ikincil depolama konumları kullanarak arasında geçiş yapabilir ve azaltılmış işlevsellik modunda salt okunur verileri ile çalışabilmek için gerekir. Azure Storage istemci kitaplıkları, birincil depolama biriminden okuma başarısız olursa, ikincil depolama biriminden okuyabilen bir yeniden deneme ilkesi tanımlamanıza olanak sağlar. Uygulamanız, ayrıca ikincil konumdaki verileri sonuçta tutarlı olduğundan emin olması gerekir. Daha fazla bilgi için blog gönderisine bakın [Azure Depolama artıklık seçenekleri ve okuma erişimli coğrafi olarak yedekli depolama](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

### <a name="performance-issues">performans sorunları</a>
Bir uygulamanın performansı, özellikle kullanıcının bakış açısıyla, öznel olabilir. Bu nedenle, bir performans sorunu olabilecek yerleri belirlemek için temel ölçümlere sahip olmak önemlidir. Birçok faktöre bir Azure depolama hizmeti istemci uygulaması perspektifinden performansını etkileyebilir. Bu etkenler depolama birimi hizmeti, istemci veya ağ altyapısı çalışabilir; Bu nedenle performans sorunu kaynağını tanımlamak için bir strateji olması önemlidir.

Büyük olasılıkla konumunu ölçümleri performans sorundan neden tanımladıktan sonra tanılama ve daha ayrıntılı sorun giderme için ayrıntılı bilgi için günlük dosyalarını kullanabilirsiniz.

Bölüm "[sorun giderme kılavuzluğu]" Bu kılavuzda daha sonra karşılaşabileceğiniz bazı yaygın performans ile ilgili sorunlar hakkında daha fazla bilgi sağlar.

### <a name="diagnosing-errors">Hatalarını tanılama</a>
Uygulamanızın kullanıcılarının, istemci uygulaması tarafından bildirilen hataların bilgilendirebilirsiniz. Depolama ölçümleri de kaydeder, depolama hizmetleri farklı hata türlerinden sayısı gibi **NetworkError**, **ClientTimeoutError**, veya **AuthorizationError**. Depolama ölçümleri farklı hata türlerinin sayısı yalnızca kayıtları olsa da, sunucu tarafı, istemci tarafı ve ağ günlüklerini inceleyerek istekleri ayrı ayrı hakkında daha fazla ayrıntı elde edebilirsiniz. Genellikle, depolama hizmet tarafından döndürülen HTTP durum kodunu neden isteği başarısız göstergesidir verecektir.

> [!NOTE]
> Bazı aralıklı hatalar görmeyi beklediğiniz unutmayın: Örneğin, geçici ağ koşulları nedeniyle hatalar veya uygulama hataları.
> 
> 

Aşağıdaki kaynaklar, depolama ile ilgili durum ve hata kodları anlamak için kullanışlıdır:

* [Ortak REST API hata kodları](http://msdn.microsoft.com/library/azure/dd179357.aspx)
* [BLOB hizmeti hata kodları](http://msdn.microsoft.com/library/azure/dd179439.aspx)
* [Kuyruk hizmeti hata kodları](http://msdn.microsoft.com/library/azure/dd179446.aspx)
* [Tablo hizmeti hata kodları](http://msdn.microsoft.com/library/azure/dd179438.aspx)
* [Dosya hizmeti hata kodları](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues">Depolama öykünücüsü sorunları</a>
Azure SDK'sı bir geliştirme iş istasyonunda çalıştırabilirsiniz bir depolama öykünücüsü içerir. Bu öykünücüsü çoğu Azure storage Hizmetleri davranışını taklit eder ve geliştirme ve test etme sırasında bir Azure aboneliği ve Azure storage hesabı gerek kalmadan Azure storage hizmetleri kullanan uygulamaları çalıştırmak etkinleştirme kullanışlıdır.

"[sorun giderme kılavuzluğu]" bölümünde bu kılavuzun storage öykünücüsü kullanarak karşılaşılan bazı yaygın sorunları açıklar.

### <a name="storage-logging-tools">Depolama günlük araçları</a>
Depolama günlük depolama istekleri Azure depolama hesabınızdaki sunucu tarafı günlüğe kaydedilmesini sağlar. Sunucu tarafı günlüğünü etkinleştirin ve günlük veri erişimi hakkında daha fazla bilgi için bkz: [depolama günlüğünü etkinleştirme ve erişim günlüğü verilerini](http://go.microsoft.com/fwlink/?LinkId=510867).

.NET için depolama istemci kitaplığı, uygulamanız tarafından gerçekleştirilen depolama işlemleri ilişkili istemci tarafı günlük verilerini toplamanıza olanak sağlar. Daha fazla bilgi için bkz. [.NET Depolama İstemci Kitaplığı ile İstemci Tarafı Günlük Kaydı](http://go.microsoft.com/fwlink/?LinkId=510868).

> [!NOTE]
> Bazı durumlarda (örneğin, SAS yetkilendirme hataları), bir kullanıcı sunucu tarafı depolama günlüklerinde hiçbir istek verileri bulabilirsiniz hata bildirebilir. Sorunun nedenini istemcide ise araştırmak için depolama istemci kitaplığı günlüğe kaydetme özelliklerini kullanın ya da ağ araştırmak için ağ izleme araçları kullanın.
> 
> 

### <a name="using-network-logging-tools">Ağ günlük araçlarını kullanarak</a>
İstemci ve sunucu değişimi verileri ve temel ağ koşulları hakkında ayrıntılı bilgi sağlamak için istemci ve sunucu arasındaki trafiği yakalayabilirsiniz. Yararlı ağ günlük araçlarını içerir:

* [Fiddler](http://www.telerik.com/fiddler) üstbilgiler ve HTTP ve HTTPS istek ve yanıt iletileri yükü verilerini incelemek sağlayan proxy hata ayıklama ücretsiz bir Web. Daha fazla bilgi için bkz: [ek 1: kullanarak HTTP ve HTTPS trafiğini yakalamak için fiddler'ı](#appendix-1).
* [Microsoft Ağ İzleyicisi'nin (Netmon)](http://www.microsoft.com/download/details.aspx?id=4865) ve [Wireshark](http://www.wireshark.org/) boş ağ çok çeşitli ağ protokolleri ayrıntılı paket bilgilerini görüntülemek etkinleştirmeniz protokol Çözümleyicileri için. Wireshark hakkında daha fazla bilgi için bkz: "[ek 2: ağ trafiğini yakalamak için Wireshark kullanarak](#appendix-2)".
* Microsoft Message Analyzer, Netmon ve, ağ paket verilerini yakalama yanı sıra yerini alır, görüntülemek ve diğer Araçları'ndan yakalanan günlük verileri çözümlemek için yardımcı olan Microsoft için kullanılan bir araçtır. Daha fazla bilgi için "[ek 3: ağ trafiğini yakalamak için Microsoft Message Analyzer kullanarak](#appendix-3)".
* İstemci makinenizde ağ üzerinden Azure depolama hizmetine bağlanabileceğini denetlemek için bir temel bağlantı testi yapmak istiyorsanız, bu standart kullanarak bunu yapamazsınız **ping** istemcide aracı. Ancak, kullanabileceğiniz [ **tcping** aracı](http://www.elifulkerson.com/projects/tcping.php) bağlantılarını denetlemek için.

Çoğu durumda, depolama günlüğe kaydetme ve depolama istemci kitaplığı günlük verilerini bir sorunu tanılamak için yeterli olacaktır, ancak bazı senaryolarda, bu ağ günlük araçları sağlayabilir daha ayrıntılı bilgi gerekebilir. Örneğin, HTTP ve HTTPS iletilerini görüntülemek için Fiddler kullanarak, gönderilen ve bir istemci uygulaması depolama işlemleri nasıl yeniden deneme incelemek sağlayacağı depolama hizmetlerinden üst bilgisi ve yük verileri görüntülemenize olanak tanır. Protokol Çözümleyicileri Wireshark gibi kayıp paketlerin ve bağlantı sorunları gidermenize olanak TCP verileri görüntülemek etkinleştirme paket düzeyinde çalışır. İleti Çözümleyicisi hem HTTP hem de TCP katmanına çalışabilir.

## <a name="end-to-end-tracing">Uçtan uca izleme</a>
Uçtan uca izleme çeşitli günlük dosyalarını kullanarak olası sorunları Araştırıyor için kullanışlı bir tekniktir. Burada sorunu gidermenize yardımcı olacak ayrıntılı bilgi için günlük dosyalarında aramaya başlamak bir göstergesi olarak ölçümleri verilerinizden tarih/saat bilgileri kullanabilirsiniz.

### <a name="correlating-log-data">Günlük verileri ilişkilendirme</a>
İstemci uygulamaları günlüklerinden görüntülerken, ağ izler ve sunucu tarafı depolama günlük ilişkilendirmek için kritik arasında farklı günlük dosyaları ister. Günlük dosyaları bağıntı tanımlayıcıları yararlı farklı alan sayısını içerir. İstemci istek kimliği, farklı günlüklerine girişleri ilişkilendirmek için kullanılacak en kullanışlı bir alandır. Ancak bazı durumlarda, sunucu istek kimliği ya da zaman damgaları kullanmak yararlı olabilir. Aşağıdaki bölümler bu seçenekler hakkında daha fazla ayrıntı sağlar.

### <a name="client-request-id">İstemci istek kimliği</a>
Depolama istemcisi kitaplığı otomatik olarak her istek için bir benzersiz istemci istek kimliği oluşturur.

* İstemci istek kimliği için depolama istemci kitaplığı oluşturur istemci tarafı günlüğüne görünür **istemci istek kimliği** istekle ilgili her günlük girişinin alanı.
* Bir Fiddler tarafından yakalanan gibi bir ağ izlemesi istemci istek kimliği istek iletilerindeki görünür **x-ms-istemci-request-id** HTTP üstbilgisi değeri.
* Sunucu tarafı depolama günlük, istemci istek kimliği istemci istek kimliği sütununda görünür.

> [!NOTE]
> Birden çok isteği aynı istemci istek kimliği (depolama istemci kitaplığı yeni bir değer otomatik olarak atar rağmen) istemci bu değer atanabilir olduğundan paylaşmak mümkündür. İstemci yinelerken tüm girişimler aynı istemci istek kimliği paylaşma İstemciden gönderilen bir toplu işlem söz konusu olduğunda, toplu bir tek istemci istek kimliği vardır.
> 
> 

### <a name="server-request-id">Sunucu istek kimliği</a>
Depolama hizmeti sunucusu isteği kimlikleri otomatik olarak oluşturur.

* Sunucu tarafı depolama günlüğü, sunucu istek kimliği görünür **istek kimliği üst bilgisi** sütun.
* Bir Fiddler tarafından yakalanan gibi bir ağ izlemesi sunucu istek kimliği yanıt iletilerini görünür **x-ms-request-id** HTTP üstbilgisi değeri.
* Depolama istemcisi kitaplığı oluşturur istemci tarafı günlüğüne, sunucu istek kimliği görünür **işlemi metin** sunucu yanıtı ayrıntılarını gösteren günlük girişi için sütun.

> [!NOTE]
> Her yeniden deneme girişimi istemci ve toplu işe dahil her işlem bir benzersiz sunucu istek kimliği var. şekilde depolama hizmetinin her zaman benzersiz bir sunucuyu istek kimliği aldığı, her istek için atar
> 
> 

Depolama istemcisi kitaplığı oluşturursa bir **StorageException** istemci **RequestInformation** özelliği içeren bir **RequestResult** içeren nesnesinin bir **ServiceRequestID** özelliği. Aynı zamanda erişebilirsiniz bir **RequestResult** nesnesinin bir **OperationContext** örneği.

Aşağıdaki kod örneği, özel bir ayarlanacağı gösterilmiştir **ClientRequestId** ekleyerek değeri bir **OperationContext** depolama hizmetine istek nesnesi. Ayrıca nasıl alınacağını gösterir **ServerRequestId** değeri yanıt iletisi.

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

### <a name="timestamps">Zaman damgaları</a>
Zaman damgaları, ilgili günlük girişlerini bulun, ancak her saat eğriltme bulunabilecek sunucu ve istemci arasında dikkatli olun için de kullanabilirsiniz. Artı veya eksi istemci zaman damgasını temel sunucu tarafı girdileri eşleştirme için 15 dakika arayın. Ölçümleri içeren BLOB'ları için blob meta verileri blob içinde depolanan ölçümleri için zaman aralığını gösteren unutmayın. Bu zaman aralığı aynı dakika veya saat için birçok ölçümleri BLOB'ları varsa yararlı olur.

## <a name="troubleshooting-guidance"></a>Sorun giderme kılavuzu
Bu bölümde tanı koymaya yardımcı olur ve bazı yaygın sorunların çoğunu uygulamanızı sorun giderme Azure storage hizmetleri kullanırken karşılaşabileceğiniz. Belirli sorununuzu ilgili bilgileri bulmak için aşağıdaki listeyi kullanın.

**Sorun giderme karar ağacı**

---
Sorununuzu depolama hizmetlerden biri performansını ilişkilidir?

* [ölçümleri Göster yüksek AverageE2ELatency ve düşük AverageServerLatency]
* [Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor, ancak istemci yüksek gecikme durumu yaşıyor]
* [Ölçümler yüksek AverageServerLatency gösteriyor]
* [Kuyrukta ileti tesliminde beklenmeyen gecikmeler yaşıyorsunuz]

---
Sorununuzu depolama hizmetlerden biri kullanılabilirliğini ilişkilidir?

* [ölçümleri Göster artışı içinde PercentThrottlingError]
* [ölçümleri Göster artışı içinde PercentTimeoutError]
* [Ölçümler PercentNetworkError’da artış gösteriyor]

---
 İstemci uygulamanız bir HTTP 4XX (örneğin, 404) yanıt Depolama hizmetinden alıyor?

* [İstemci HTTP 403 (Yasak) iletileri alma]
* [İstemci HTTP 404 (bulunamadı) iletileri alma]
* [İstemci HTTP 409 (Çakışma) iletileri alma]

---
[ölçümleri Göster düşük PercentSuccess veya analytics günlük girişlerini sahip hareket durumu işlemler ClientOtherErrors,]

---
[Kapasite ölçümlerini beklenmeyen artışı depolama kapasitesi kullanımı Göster]

---
[Sanal makinelerin çok sayıda ekli VHD'ler sahip beklenmeyen yeniden başlatmalar yaşıyor]

---
[Sorununuzu geliştirme veya test için depolama öykünücüsünü kullanarak ortaya çıkar.]

---
[.NET için Azure SDK'sını yükleme sorunlarla]

---
[Bir depolama hizmetindeki farklı bir sorun olması]

---
### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Yüksek AverageE2ELatency ve düşük AverageServerLatency ölçümleri Göster
Aşağıda gösterimden [Azure portal](https://portal.azure.com) izleme aracı gösteren bir örnek nerede **AverageE2ELatency** önemli ölçüde daha yüksek olan **AverageServerLatency**.

![][4]

Depolama hizmetinin yalnızca ölçüm hesaplar **AverageE2ELatency** başarılı istekler için ve aksine **AverageServerLatency**, istemcinin veri gönderme ve alma süresini içerir Depolama hizmeti durduruldu. Bu nedenle, birbirinden **AverageE2ELatency** ve **AverageServerLatency** yavaş yanıt olan istemci uygulaması nedeniyle veya ağdaki koşulları nedeniyle olabilir.

> [!NOTE]
> Ayrıca görüntüleyebilirsiniz **E2ELatency** ve **ServerLatency** tek depolama işlemleri günlük depolama için oturum verileri.
> 
> 

#### <a name="investigating-client-performance-issues"></a>İstemci performans sorunlarını
Yavaş yanıt istemci için olası nedenler sınırlı sayıda kullanılabilir bağlantıları veya iş parçacığının olmamasına veya CPU, bellek veya ağ bant genişliği gibi kaynaklar yetersiz içerir. (Örneğin depolama hizmeti için zaman uyumsuz çağrılar kullanarak) daha etkili olması için istemci kodu değiştirerek ya da daha büyük bir sanal makine (daha fazla sayıda çekirdek ve daha fazla bellek ile) kullanarak bu sorunu çözmek mümkün olabilir.

Tablo ve kuyruk Hizmetleri için Nagle algoritması de yüksek neden olabilir **AverageE2ELatency** kıyasla **AverageServerLatency**: post daha fazla bilgi için bkz: [Nagle'nın Algoritmasıdır küçük isteklerini doğru değil kolay](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx). Kullanarak kod Nagle algoritmasını devre dışı bırakabilirsiniz **ServicePointManager** sınıfını **System.Net** ad alanı. Bu tablonun herhangi çağrı yapmak veya sıra Hizmetleri bu zaten bağlantıları etkilemez beri uygulamanızda açmak önce yapmanız gerekir. Aşağıdaki örnek geldiği **Application_Start** çalışan rolü yöntemi.

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);
ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
tableServicePoint.UseNagleAlgorithm = false;
ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
queueServicePoint.UseNagleAlgorithm = false;
```

İstemci uygulamanızı gönderilirken kaç isteklerini görmek için istemci tarafı günlüklerini ve onay genel .NET için CPU, .NET atık toplama, ağ kullanımı veya bellek gibi istemci performans sorunları ilgili denetlemeniz gerekir. .NET istemci uygulamaları sorun giderme için başlangıç noktası olarak bkz [hata ayıklama, izleme ve profil oluşturma](http://msdn.microsoft.com/library/7fe0dd2y).

#### <a name="investigating-network-latency-issues"></a>Ağ gecikmesi sorunları araştırma
Genellikle, ağdan kaynaklanan yüksek uçtan uca gecikme geçici durumları yüzünde olur. Atılan paketlerin gibi her iki geçici ve kalıcı ağ sorunları Wireshark veya Microsoft Message Analyzer gibi araçları kullanarak araştırabilirsiniz.

Ağ sorununu gidermek için Wireshark kullanma hakkında daha fazla bilgi için bkz: "[ek 2: ağ trafiğini yakalamak için Wireshark kullanarak]."

Ağ sorununu gidermek üzere Microsoft Message Analyzer kullanma hakkında daha fazla bilgi için bkz: "[ek 3: ağ trafiğini yakalamak için Microsoft Message Analyzer'ı kullanarak]."

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Düşük AverageE2ELatency ve düşük AverageServerLatency ölçümleri gösterir, ancak istemci yüksek gecikme yaşanıyor
Bu senaryoda, en olası nedeni depolama hizmetine erişirken depolama istekleri gecikmeden ' dir. Neden istemciden istekleri üzerinden blob hizmeti kuran değil araştırmanız gerekir.

Kullanılabilir bağlantılar veya iş parçacığının sınırlı sayıda olan istekleri gönderirken geciktirme istemcisi için olası nedenlerden biri.

Ayrıca, istemcinin birden çok deneme çalışıp çalışmadığını denetleyin ve bu ise, nedenini araştırın. İstemci birden çok deneme çalışıp çalışmadığını belirlemek için aşağıdakileri yapabilirsiniz:

* Storage Analytics günlüklerini inceleyin. Birden çok deneme gerçekleştiği birden çok işlem aynı istemci istek kimliği ile ancak farklı sunucu isteği kimlikleri görürsünüz.
* İstemci günlüklerini inceleyin. Ayrıntılı günlük kaydını yeniden deneme oluştuğunu gösterir.
* Kodunuzdaki hataları ayıklamanıza ve özelliklerini denetleme **OperationContext** istekle ilişkili nesne. İşlem yeniden değilse, **RequestResults** özelliği, birden çok benzersiz sunucu isteği kimlikleri içerecektir. Ayrıca her istek için başlangıç ve bitiş zamanlarını kontrol edebilirsiniz. Kod örneği bölümünde daha fazla bilgi için bkz [sunucu istek kimliği].

İstemcinin herhangi bir sorun varsa, paket kaybı gibi olası ağ sorunları araştırmanız gerekir. Ağ sorunları araştırmak için Wireshark veya Microsoft Message Analyzer gibi araçlar kullanın.

Ağ sorununu gidermek için Wireshark kullanma hakkında daha fazla bilgi için bkz: "[ek 2: ağ trafiğini yakalamak için Wireshark kullanarak]."

Ağ sorununu gidermek üzere Microsoft Message Analyzer kullanma hakkında daha fazla bilgi için bkz: "[ek 3: ağ trafiğini yakalamak için Microsoft Message Analyzer'ı kullanarak]."

### <a name="metrics-show-high-AverageServerLatency"></a>Yüksek AverageServerLatency ölçümleri Göster
Yüksek olması durumunda **AverageServerLatency** blob indirme isteği için aynı blob (veya BLOB kümesi) için yinelenen istekleri olup olmadığını görmek için depolama günlüğü günlükleri kullanmanız gerekir. BLOB karşıya yükleme istekleri için hangi blok boyutu istemci (daha az okuma ayrıca değerinden 64 K olmadıkça boyutu 64 K içinde ek yüklerini sonuçlanabilir öbekleri örneğin bloklarını) kullanarak, birden çok istemci blokları para aynı blob'una için karşıya yüklediğiniz varsa ise, araştırmanız gereken Paralel. Aşan içinde neden istekleri sayısında ani için dakika başına ölçümleri de denetlemelisiniz ikinci ölçeklenebilirlik hedefleri başına: Ayrıca bkz. "[ölçümleri Göster artışı içinde PercentTimeoutError]."

Yüksek görüyorsanız **AverageServerLatency** blob karşıdan var. Yinelenen olduğunda istekleri aynı blob veya BLOB kümesi sonra Azure önbelleği veya Azure içerik teslim ağı (CDN) kullanarak bu Blob önbelleği göz önünde bulundurmanız gerekir. Karşıya yükleme istekleri için daha büyük bir blok boyutu kullanarak üretilen işi artırabilir. Tablolara sorgular için de aynı sorgu işlemleri gerçekleştirmek ve burada veri sık değiştirmez istemcilerde istemci tarafı önbelleğe alma uygulamak mümkündür.

Yüksek **AverageServerLatency** değerleri hatalı tasarlanmış tablolar veya tarama işlemleri sonucunda ya da, ekleme ve başına koruma deseni takip sorguları belirtisi de olabilir. Daha fazla bilgi için "[ölçümleri Göster artışı içinde PercentThrottlingError]".

> [!NOTE]
> Kapsamlı denetim listesi Performans Denetim burada bulabilirsiniz: [Microsoft Azure depolama performans ve ölçeklenebilirlik Yapılacaklar listesi](storage-performance-checklist.md).
> 
> 

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>Bir kuyruk iletisi Teslimde beklenmeyen gecikme yaşıyor
Bir uygulama bir sıraya bir ileti ekler zaman kuyruktan okunmak üzere kullanılabilir hale geldiği tarih arasında bir gecikme karşılaşıyorsanız, sorunu tanılamak için aşağıdaki adımları uygulayın:

* Uygulama başarıyla iletileri kuyruğa ekleme doğrulayın. Uygulama değil yeniden deneniyor onay **AddMessage** birkaç kez önce başarılı yöntemi. Depolama istemci kitaplığı günlükleri gerçekleştirdi tüm depolama işlemlerini gösterir.
* Saat eğriltme kuyruğuna ileti ekler çalışan rolü arasında ve ileti kolaylaştırır kuyruktaki iletileri okur çalışan rolü görünür işlemde bir gecikme olur gibi doğrulayın.
* Kuyruktan iletileri okur çalışan rolü başarısız olup olmadığını denetleyin. Bir kuyruk istemci çağırırsa **GetMessage** yöntem, ancak onay ile yanıt başarısız, ileti kalacak sırasına kadar görünmez **invisibilityTimeout** süresi sona erene. Bu noktada, ileti yeniden işlemek için kullanılabilir hale gelir.
* Kuyruk uzunluğu zaman içinde büyüyen olmadığını denetleyin. Diğer çalışanlar sıra üzerinde yerleştirme tüm iletileri işlemek kullanılabilir yeterli çalışan yoksa bu durum oluşabilir. Ayrıca Ölçümleri silme isteklerinin başarısız olma ve dequeue iletiyi silmek için yinelenen başarısız denemeleri gösterebilir iletilerde sayısı olmadığını görmek için kontrol edin.
* Beklenenden daha yüksek olan sıra işlemleri için depolama günlüğü günlüklerini inceleyin **E2ELatency** ve **ServerLatency** normalden daha uzun bir zaman aralığında üzerinden değerleri.

### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Ölçümleri artışı içinde PercentThrottlingError Göster
Bir depolama birimi hizmeti ölçeklenebilirlik hedeflerini aşan azaltma hataları oluşur. Tek bir istemci emin olun veya Kiracı için depolama hizmeti kısıtlamaları başkalarının ödün verme pahasına hizmetini kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md) depolama hesapları için ölçeklenebilirlik hedefleri ve depolama hesapları içindeki bölümler için performans hedefleri hakkında ayrıntılar için.

Varsa **PercentThrottlingError** ölçüm azaltma bir hata ile başarısız olan istek yüzdesi cinsinden artışı Göster, iki senaryodan biri araştırmanız gereken:

* [PercentThrottlingError geçici artış]
* [PercentThrottlingError hata kalıcı artış]

Bir artış **PercentThrottlingError** genellikle bir artış depolama istek sayısı ile aynı zamanda oluşur veya uygulamanızı test etme başlangıçta olduğunda yük. Bu ayrıca kendi istemci "503 Sunucu meşgul" veya "500 işlem zaman aşımı" HTTP durum iletileri depolama işlemleri olarak bildiriminde.

#### <a name="transient-increase-in-PercentThrottlingError">PercentThrottlingError geçici artış</a>
Ani değerinde görüyorsanız **PercentThrottlingError** uygulama yüksek etkinlik dönemleri ile çakıştığı, bir üstel (doğrusal değil) geri alma stratejisi yeniden deneme için istemci uygulama. Geri alma yeniden deneme hemen bölüm azaltmak ve ani trafiğinin çıkışı kesintisiz uygulamanıza yardımcı olur. Yeniden deneme ilkelerini için depolama istemci kitaplığı kullanılarak uygulama hakkında daha fazla bilgi için bkz: [Microsoft.WindowsAzure.Storage.RetryPolicies Namespace](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx).

> [!NOTE]
> Ani değeri de görebilirsiniz **PercentThrottlingError** , değil çakıştığı uygulama için yüksek etkinlik nokta: Burada en olası nedeni yük dengelemeyi iyileştirmek için bölümleri taşıma depolama hizmetidir.
> 
> 

#### <a name="permanent-increase-in-PercentThrottlingError">PercentThrottlingError hata kalıcı artış</a>
Tutarlı bir şekilde yüksek bir değer için görüyorsanız **PercentThrottlingError** kalıcı bir artış işlem birimlerinizi veya ilk yükleme yaparken aşağıdaki sınamaları, uygulamanızda sonra uygulamanızın depolama bölümleri nasıl kullandığını ve olup bir depolama hesabı ölçeklenebilirlik hedefleri yaklaştığını değerlendirmeniz gerekiyor. (Tek bir bölümü olarak sayılır) bir sıranın hatalarda azaltma görüyorsanız, örneğin, daha sonra ek sıraları arasında birden çok bölüm işlemleri yaymak için kullanmayı düşünmelisiniz. Bir tabloda hatalar azaltma görüyorsanız, geniş bir bölüm anahtarı değerlerini kullanarak, işlemler arasında birden çok bölüm yaymak için farklı bir bölümleme şeması kullanarak dikkate almanız gerekir. Bir ortak bu sorunun nedeni burada bölüm anahtarı olarak tarihi seçin ve ardından belirli bir tarihte tüm veriler yazılır tek bir bölüm prepend ve append koruma Desen: yük altında bu yazma tıkanıklığa neden olabilir. Farklı bir bölümleme tasarım göz önünde bulundurun veya blob storage kullanarak daha iyi bir çözüm olabilir olup olmadığını değerlendirin. Ayrıca azaltma trafiğinizi ani sonucunda oluştuğunu olup olmadığını denetleyin ve desen isteklerinin düzgünleştirme yolları araştırın.

Arasında birden çok bölüm işlemlerinizi dağıtırsanız, hala için depolama hesabı ölçeklenebilirlik sınırları farkında olmanız gerekir. Örneğin, her işleme 2.000 1 KB iletileri saniye başına en fazla on sıraları kullandıysanız, depolama hesabı için saniye başına 20.000 ileti genel sınırını konumunda bulunur. Saniye başına birden fazla 20.000 varlıkları işlemek gereken birden çok depolama hesabı kullanmayı düşünmelisiniz. İstekleri ve varlıkları boyutunu depolama hizmeti istemcileriniz olduğunda kısıtlar üzerinde bir etkisi olduğunu aklınızda bulundurmanız gerekir: büyük istekleri ve varlıkları varsa, daha erken kısıtlanan.

Verimli sorgu tasarımı tablo bölümleri için ölçeklenebilirlik sınırları isabet yapmanıza da neden olabilir. Örneğin, her bir varlık erişmek bir sorgu, yalnızca bir yüzde varlıkların bir bölümünde seçer, ancak bir bölümdeki tüm varlıklar tarar bir filtre ile gerekir. Bu bölümdeki işlemleri toplam sayısı doğru okuma her varlık sayar; Bu nedenle, ölçeklenebilirlik hedefleri kolayca erişebilir.

> [!NOTE]
> Performans testi herhangi verimsiz sorgu tasarımı, uygulamanızda ortaya çıkarır.
> 
> 

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Ölçümleri artışı içinde PercentTimeoutError Göster
Bir artış ölçümlerinizi Göster **PercentTimeoutError** depolama hizmetlerinizi biri için. Aynı anda istemci "500 işlem zaman aşımı" HTTP durum iletilerini yüksek miktarda depolama işlemlerinden alır.

> [!NOTE]
> Yeni bir sunucuya bir bölüm taşıyarak yük bakiyelerini isteklerini zaman aşımı hataları depolama hizmeti geçici olarak görebilirsiniz.
> 
> 

**PercentTimeoutError** ölçümüdür aşağıdaki ölçümleri toplamı: **ClientTimeoutError**, **AnonymousClientTimeoutError**, **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**, ve **SASServerTimeoutError**.

Sunucu zaman aşımı, sunucuda bir hata nedeniyle oluşur. Sunucu üzerinde bir işlemi istemci tarafından belirtilen zaman aşımı süresi aşıldığından istemci zaman aşımları gerçekleşir; Örneğin, depolama istemci kitaplığı kullanılarak bir istemci bir işlem için bir zaman aşımı kullanarak ayarlayabilirsiniz **ServerTimeout** özelliği **QueueRequestOptions** sınıfı.

Sunucu zaman aşımı daha fazla araştırma gerektiren depolama birimi hizmeti bir sorun olduğunu gösteriyor. Ölçümleri hizmeti için ölçeklenebilirlik sınırları basarsa olmadığını görmek için ve bu soruna neden trafiğinin herhangi bir ani belirlemek için kullanabilirsiniz. Zaman zaman ortaya çıkan bir sorundur, yük dengeleyici nedeniyle olabilir hizmet etkinliği. Sorun kalıcı ise ve ölçeklenebilirlik sınırları hizmetinin basarsa, uygulamanız tarafından neden değil, bir destek sorununu tetiklemelidir. İstemci zaman aşımları için zaman aşımı istemci ve istemci zaman aşımı değeri ayarlayın ya da değişiklik uygun bir değere ayarlayın veya nasıl performansı depolama hizmetindeki işlemlerinin örneğin tablo sorguları en iyi duruma getirme veya iletilerinizi boyutunu azaltma artırabilirsiniz araştırmak karar vermeniz gerekir.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Ölçümleri artışı içinde PercentNetworkError Göster
Bir artış ölçümlerinizi Göster **PercentNetworkError** depolama hizmetlerinizi biri için. **PercentNetworkError** ölçümüdür aşağıdaki ölçümleri toplamı: **NetworkError**, **AnonymousNetworkError**, ve **SASNetworkError**. İstemci bir depolama isteği yaptığında, depolama birimi hizmeti bir ağ hatası algıladığında, bu oluşur.

Bu hatanın en yaygın nedeni bir istemcidir depolama hizmetinde bir zaman aşımı süresi dolmadan önce bağlantısı kesiliyor. Neden ve ne zaman istemci ve storage hizmetinden kesilene anlamak için istemci kodu araştırın. İstemciden gelen ağ bağlantısı sorunları araştırmak için Wireshark, Microsoft Message Analyzer veya Tcping de kullanabilirsiniz. Bu araçları açıklanan [ekler].

### <a name="the-client-is-receiving-403-messages">İstemci HTTP 403 (Yasak) iletileri alma</a>
İstemci uygulamanızın HTTP 403 (Yasak) hataları atma, olası bir nedeni istemci (diğer olası nedenleri saat eğriltme, geçersiz anahtarlar ve boş üstbilgileri içerse) depolama isteği gönderdiğinde, süresi dolmuş bir paylaşılan erişim imzası (SAS) kullanıyor demektir. Süresi dolmuş bir SAS anahtarı neden olduğunda, sunucu tarafı depolama günlüğü günlük verileri herhangi bir giriş görürsünüz değil. Aşağıdaki tabloda bu sorunun oluşmasını gösterilmektedir depolama istemci kitaplığı tarafından oluşturulan istemci-tarafı günlüğünden bir örnek gösterilmektedir:

| Kaynak | Ayrıntı düzeyi | Ayrıntı düzeyi | İstemci istek kimliği | İşlemi metin |
| --- | --- | --- | --- | --- |
| Microsoft.WindowsAzure.Storage |Bilgi |3 |85d077ab-… |Konum modu PrimaryOnly başına birincil konumla işlemi başlatılıyor. |
| Microsoft.WindowsAzure.Storage |Bilgi |3 |85d077ab -… |Eşzamanlı istek https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14 için başlangıç&amp;sr c =&amp;si mypolicy =&amp;SIG = OFnd4Rd7z01fIvh % 2BmcR6zbudIH2F5Ikm % 2FyhNYZEmJNQ % 3B&amp;API sürümü 2014-02-14 =. |
| Microsoft.WindowsAzure.Storage |Bilgi |3 |85d077ab -… |Yanıtı bekleniyor. |
| Microsoft.WindowsAzure.Storage |Uyarı |2 |85d077ab -… |Yanıt bekleme sırasında özel durum oluştu: Uzak sunucu bir hata döndürdü: (403) Yasak. |
| Microsoft.WindowsAzure.Storage |Bilgi |3 |85d077ab -… |Yanıtı alındı. Durum kodu 403, istek kimliği = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, Content-MD5 = =, ETag =. |
| Microsoft.WindowsAzure.Storage |Uyarı |2 |85d077ab -… |İşlem sırasında özel durum oluştu: Uzak sunucu bir hata döndürdü: (403) Yasak... |
| Microsoft.WindowsAzure.Storage |Bilgi |3 |85d077ab -… |İşlem yeniden denetleniyor. Yeniden deneme sayısı = 0, HTTP durum kodu 403, özel durum = = uzak sunucusu bir hata döndürdü: (403) Yasak... |
| Microsoft.WindowsAzure.Storage |Bilgi |3 |85d077ab -… |Sonraki konumu, birincil, konum Modu'na bağlı ayarlandı. |
| Microsoft.WindowsAzure.Storage |Hata |1 |85d077ab -… |Yeniden deneme ilkesi için bir yeniden deneme izin vermedi. Uzak sunucu ile başarısız olan bir hata döndürdü: (403) Yasak. |

Bu senaryoda, istemcinin sunucuya belirteç göndermeden önce SAS belirteci neden doluyor araştırmanız gerekir:

* Genellikle, hemen kullanmak bir istemci için bir SAS oluşturduğunuzda bir başlangıç saati ayarlanmadı. Depolama hizmetinin henüz geçerli olmayan bir SAS alma mümkündür sonra geçerli saati ve depolama hizmeti kullanılarak SAS oluşturma ana bilgisayar arasında küçük saat fark olduğunda.
* Çok kısa süre sonu zamanı SAS ayarlı değil. Yeniden SAS ve depolama hizmeti oluşturma konak küçük saat farklılıkları görünüşe göre beklenenden daha önce süresi dolacak bir SAS yol açabilir.
* SAS anahtarını sürüm parametresinde mu (örneğin **sv 2015-04-05 =**) kullandığınız depolama istemci kitaplığı sürümüyle eşleşen? Her zaman en son sürümünü kullanmanızı öneririz [depolama istemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/).
* Depolama erişim tuşlarınızı oluşturursanız, var olan tüm SAS belirteci geçersiz. Önbellek istemci uygulamalar için uzun süre sonu zamanı ile SAS belirteci oluşturursanız, bu sorun ortaya çıkabilir.

Ardından SAS belirteçleri oluşturmak için depolama istemci kitaplığı kullanıyorsanız, geçerli bir belirteci oluşturmak kolaydır. Bununla birlikte, Storage REST API'sini kullanarak ve el ile SAS belirteci oluşturma görürsünüz [paylaşılan erişim imzası için temsilci seçme erişimle](http://msdn.microsoft.com/library/azure/ee395415.aspx).

### <a name="the-client-is-receiving-404-messages">İstemci HTTP 404 (bulunamadı) iletileri alma</a>
İstemci uygulaması sunucudan bir HTTP 404 (bulunamadı) iletisi alırsa, bu istemci (bir varlık, tablo, blob, kapsayıcısı veya sıra gibi) kullanmak için çalışıyordu nesne depolama hizmetinde yok anlamına gelir. Gibi bir dizi Bu, olası nedenleri vardır:

* [İstemci veya başka bir işlem nesne daha önce silinmiş]
* [Bir paylaşılan erişim imzası (SAS) yetkilendirme sorunu]
* [İstemci tarafı JavaScript kodu nesneye erişim izni yok]
* [Ağ hatası]

#### <a name="client-previously-deleted-the-object">İstemci veya başka bir işlem nesne daha önce silinmiş</a>
Okuma, güncelleştirme veya bir depolama hizmetindeki veri silmek için istemci nerede çalışıyor senaryolarda sunucu tarafı günlüklerinde ve storage hizmetinden söz konusu Nesne silindi önceki bir işlemi tanımlamak genellikle kolaydır. Genellikle, başka bir kullanıcı veya işlem nesnesini sildi günlük verilerini gösterir. Sunucu tarafı depolama oturum günlüğüne, ne zaman bir istemci bir nesne silindi işlem türü ve istenen nesnesi-anahtar sütun gösterir.

İstemci yeni bir nesne oluşturma koşuluyla, bu bir HTTP 404 (bulunamadı) yanıt olarak sonuçları neden bir nesne eklemek için bir istemci nerede çalışıyor senaryoda, hemen belirgin olmayabilir. Ancak, istemci istemcinin bir sıra bulamıyor olmalıdır bir ileti oluşturuyorsanız blob kapsayıcısını bulamadı olmalıdır blob oluşturuyorsanız ve istemci bir satır ekleme, bu tabloyu bulamadı olmalıdır.

İstemci depolama hizmetine belirli isteklere gönderdiğinde bir ayrıntılı anlaşılması için depolama istemci Kitaplığı'ndan istemci-tarafı günlüğünü kullanabilirsiniz.

İstemci için bunu oluşturmayı blob kapsayıcı bulamadığında depolama istemci kitaplığı tarafından oluşturulan aşağıdaki istemci-tarafı günlüğü sorun gösterilmektedir. Bu günlük dosyası şu depolama işlemleri ayrıntılarını içerir:

| İstek Kimliği | İşlem |
| --- | --- |
| 07b26a5d-... |**DeleteIfExists** blob kapsayıcısını silmek için yöntem. Bu işlem içeren Not bir **HEAD** kapsayıcı varlığını denetlemek için istek. |
| e2d06d78… |**CreateIfNotExists** yöntemi blob kapsayıcı oluşturun. Bu işlem içeren Not bir **HEAD** kapsayıcı varlığını denetleyen istek. **HEAD** 404 bir ileti döndürür ancak devam eder. |
| de8b1c3c-... |**UploadFromStream** blob oluşturmak için yöntemi. **PUT** istek 404 iletisiyle başarısız olur |

Günlük girişleri:

| İstek Kimliği | İşlemi metin |
| --- | --- |
| 07b26a5d-... |Https://domemaildist.blob.core.windows.net/azuremmblobcontainer eşzamanlı isteği başlatılıyor. |
| 07b26a5d-... |StringToSign = HEAD............x-ms-client-request-id:07b26a5d-....x-ms-date:Tue, 03 Jun 2014 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Yanıtı bekleniyor. |
| 07b26a5d-... |Yanıtı alındı. Durum kodu 200, istek kimliği = eeead849... = Content-MD5 =, ETag = &quot;0x8D14D2DC63D059B&quot;. |
| 07b26a5d-... |Yanıt Üstbilgileri işlemi geri kalanı ile devam etmeden başarıyla işlendi. |
| 07b26a5d-... |Yanıt gövdesi yükleniyor. |
| 07b26a5d-... |İşlem başarıyla tamamlandı. |
| 07b26a5d-... |Https://domemaildist.blob.core.windows.net/azuremmblobcontainer eşzamanlı isteği başlatılıyor. |
| 07b26a5d-... |StringToSign = DELETE............x-ms-client-request-id:07b26a5d-....x-ms-date:Tue, 03 Jun 2014 10:33:12    GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Yanıtı bekleniyor. |
| 07b26a5d-... |Yanıtı alındı. Durum kodu 202, istek kimliği = 6ab2a4cf-..., Content-MD5 = =, ETag =. |
| 07b26a5d-... |Yanıt Üstbilgileri işlemi geri kalanı ile devam etmeden başarıyla işlendi. |
| 07b26a5d-... |Yanıt gövdesi yükleniyor. |
| 07b26a5d-... |İşlem başarıyla tamamlandı. |
| e2d06d78-... |Zaman uyumsuz isteği https://domemaildist.blob.core.windows.net/azuremmblobcontainer başlatılıyor.</td> |
| e2d06d78-... |StringToSign = HEAD............x-ms-client-request-id:e2d06d78-....x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Yanıtı bekleniyor. |
| de8b1c3c-... |Https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt eşzamanlı isteği başlatılıyor. |
| de8b1c3c-... |StringToSign PUT =... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-MS-BLOB-Type:BlockBlob.x-MS-Client-Request-id:de8b1c3c-...x-MS-Date:TUE, 03 Haz 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt. |
| de8b1c3c-... |İstek veri yazmak hazırlanıyor. |
| e2d06d78-... |Yanıt bekleme sırasında özel durum oluştu: Uzak sunucu bir hata döndürdü: (404) bulunamadı... |
| e2d06d78-... |Yanıtı alındı. Durum kodu 404, istek kimliği = 353ae3bc-..., Content-MD5 = =, ETag =. |
| e2d06d78-... |Yanıt Üstbilgileri işlemi geri kalanı ile devam etmeden başarıyla işlendi. |
| e2d06d78-... |Yanıt gövdesi yükleniyor. |
| e2d06d78-... |İşlem başarıyla tamamlandı. |
| e2d06d78-... |Zaman uyumsuz isteği https://domemaildist.blob.core.windows.net/azuremmblobcontainer başlatılıyor. |
| e2d06d78-... |StringToSign = PUT...0.........x-ms-client-request-id:e2d06d78-....x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Yanıtı bekleniyor. |
| de8b1c3c-... |Yazma isteği verileri. |
| de8b1c3c-... |Yanıtı bekleniyor. |
| e2d06d78-... |Yanıt bekleme sırasında özel durum oluştu: Uzak sunucu bir hata döndürdü: (409) çakışma... |
| e2d06d78-... |Yanıtı alındı. Durum kodu 409, istek kimliği = c27da20e-..., Content-MD5 = =, ETag =. |
| e2d06d78-... |Hata yanıt gövdesi yükleniyor. |
| de8b1c3c-... |Yanıt bekleme sırasında özel durum oluştu: Uzak sunucu bir hata döndürdü: (404) bulunamadı... |
| de8b1c3c-... |Yanıtı alındı. Durum kodu 404, istek kimliği = 0eaeab3e-..., Content-MD5 = =, ETag =. |
| de8b1c3c-... |İşlem sırasında özel durum oluştu: Uzak sunucu bir hata döndürdü: (404) bulunamadı... |
| de8b1c3c-... |Yeniden deneme ilkesi için bir yeniden deneme izin vermedi. Uzak sunucu ile başarısız olan bir hata döndürdü: (404) bulunamadı... |
| e2d06d78-... |Yeniden deneme ilkesi için bir yeniden deneme izin vermedi. Uzak sunucu ile başarısız olan bir hata döndürdü: (409) çakışma... |

Bu örnekte, istemci gelen istekleri araya ekleme günlüğü gösterir, **CreateIfNotExists** yöntemi (istek kimliği e2d06d78...) gelen istekleri ile **UploadFromStream** yöntemi (de8b1c3c-...). İstemci uygulaması bu yöntemleri zaman uyumsuz olarak çağırma çünkü bu Interleaving olur. Bu kapsayıcıda blob herhangi bir veriyi karşıya yüklemeye çalışmadan önce bu kapsayıcı oluşturduğundan emin olmak için istemci zaman uyumsuz kodu değiştirin. İdeal olarak, tüm kapsayıcıları önceden oluşturmanız gerekir.

#### <a name="SAS-authorization-issue"></a>Bir paylaşılan erişim imzası (SAS) yetkilendirme sorunu
İstemci uygulaması işlemi için gerekli izinleri içermez bir SAS anahtarı kullanmayı denerse, depolama birimi hizmeti istemcisi için bir HTTP 404 (bulunamadı) iletisi döndürür. Aynı anda için sıfır olmayan bir değer de görürsünüz **SASAuthorizationError** ölçümleri içinde.

Aşağıdaki tabloda bir örnek sunucu tarafı günlük iletisi depolama günlüğü günlük dosyasından gösterilmektedir:

| Ad | Değer |
| --- | --- |
| İstek başlangıç zamanı | 2014-05-30T06:17:48.4473697Z |
| İşlem türü     | GetBlobProperties            |
| İstek durumu     | SASAuthorizationError        |
| HTTP durum kodu   | 404                          |
| Kimlik doğrulaması türü| SAS                          |
| Hizmet türü       | Blob                         |
| İstek URL'si        | https://domemaildist.blob.core.windows.net/azureimblobcontainer/blobCreatedViaSAS.txt |
| &nbsp;                 |   ? sv 2014-02-14 = & sr = c & si = mypolicy & SIG XXXXX =&;API sürümü 2014-02-14 = |
| İstek Kimliği üst bilgisi  | a1f348d5-8032-4912-93ef-b393e5252a3b |
| İstemci istek kimliği  | 2d064953-8436-4ee0-aa0c-65cb874f7929 |


Kendisi için bu izinleri verilmemiş bir işlemi gerçekleştirmek istemci uygulamanızın neden deniyor araştırın.

#### <a name="JavaScript-code-does-not-have-permission"></a>İstemci tarafı JavaScript kodu nesneye erişim izni yok
JavaScript istemci kullanıyorsanız ve depolama hizmeti HTTP 404 iletileri döndürüyor, tarayıcıda aşağıdaki JavaScript hataları kontrol edin:

```
SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.
```

> [!NOTE]
> İstemci tarafı JavaScript sorunlarını giderirken, tarayıcı ve depolama hizmeti arasında alınıp iletileri izlemek için Internet Explorer'da F12 geliştirici araçlarını kullanabilirsiniz.
> 
> 

Web tarayıcısı uyguladığından bu hatalar ortaya [aynı kaynak İlkesi](http://www.w3.org/Security/wiki/Same_Origin_Policy) bir web sayfası bir API farklı bir etki alanında etki alanından sayfa çağırmasını engelleyen güvenlik kısıtlaması gelir.

JavaScript sorunu çözmek için istemci erişimi için depolama hizmeti arası kaynak kaynak paylaşımı (CORS) yapılandırabilirsiniz. Daha fazla bilgi için bkz: [Azure Storage Hizmetleri için çıkış noktaları arası kaynak paylaşımı (CORS) Destek](http://msdn.microsoft.com/library/azure/dn535601.aspx).

Aşağıdaki kod örneği, blob depolama hizmetinin bir blob'a erişmek JavaScript Contoso etki alanında çalışan izin vermek için blob hizmetinin nasıl yapılandırılacağı gösterilmektedir:

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
Bazı durumlarda, kayıp ağ paketlerini istemciye HTTP 404 iletilerini döndürmek depolama hizmeti neden olabilir. Örneğin, istemci uygulamanız tablo hizmetinden bir varlık silinirken bir depolama özel durumu raporlama throw istemci bkz bir "HTTP 404 (bulunamadı)" Tablo hizmetinden durum iletisi. Tablo depolama hizmeti tabloda incelediğinizde, istendiği gibi hizmet varlığı silmek olduğunu görürsünüz.

İstek için tablo hizmeti tarafından atanan istek kimliği (7e84f12d...) istemci özel durum ayrıntıları içerir: arama tarafından sunucu tarafı depolama günlüklerinde İstek Ayrıntıları bulmak için bu bilgileri kullanabilirsiniz **istek kimliği üstbilgisi**  günlük dosyasında sütun. Ölçümler, bu gibi hataları oluşur ve bu hata ölçümleri kayıtlı saate göre günlük dosyalarını aramak belirlemek için de kullanabilirsiniz. Bu günlük girişi silme bir "HTTP (404) istemci başka bir hata" durum iletisiyle başarısız olduğunu gösterir. Aynı günlük girişi istemci tarafından oluşturulan istek Kimliğini de içerir **istemci istek kimliği** sütun (813ea74f...).

Sunucu tarafı günlük da aynı olan başka bir giriş içerir **istemci istek kimliği** değeri (813ea74f...) başarılı bir silme işlemi aynı varlık için ve aynı istemciden. Başarısız istek silmeden önce bu başarılı silme işlemi çok kısa bir süre içinde gerçekleşen.

Bu senaryonun en olası nedeni, istemci başarılı oldu, ancak onay (belki de bir geçici ağ sorunu nedeniyle) sunucusundan almadı tablo hizmeti için bir silme isteği varlık için gönderilen ' dir. İstemci ardından otomatik olarak işlem yeniden (aynı kullanarak **istemci istek kimliği**), bu yeniden deneme varlık zaten silinmiş olduğundan başarısız oldu.

Bu sorun sık sık olursa, tablo hizmetinden bildirimleri almak istemci neden başarısız araştırmanız gerekir. Zaman zaman ortaya çıkan bir sorundur, "HTTP (404) bulunamadı" hatasını yakalamak ve istemcide oturum ancak devam etmek istemcinin izin.

### <a name="the-client-is-receiving-409-messages"></a>İstemci HTTP 409 (Çakışma) iletileri alma
Aşağıdaki tabloda, iki istemci işlemleri için sunucu tarafı günlüğünden bir ayıklama gösterilmektedir: **DeleteIfExists** göre hemen ardından **CreateIfNotExists** aynı blob kapsayıcı adı kullanarak. Her istemci işlemi sunucuya ilk gönderilen iki isteklerinde sonuçları bir **GetContainerProperties** kapsayıcı, arkasından var olup olmadığını denetlemek için istek **DeleteContainer** veya  **CreateContainer** isteği.

| Zaman damgası | İşlem | Sonuç | Kapsayıcı adı | İstemci istek kimliği |
| --- | --- | --- | --- | --- |
| 05:10:13.7167225 |GetContainerProperties |200 |mmcont |c9f52c89-… |
| 05:10:13.8167325 |DeleteContainer |202 |mmcont |c9f52c89-… |
| 05:10:13.8987407 |GetContainerProperties |404 |mmcont |bc881924-… |
| 05:10:14.2147723 |CreateContainer |409 |mmcont |bc881924-… |

İstemci uygulaması kodu siler ve aynı adı kullanarak bir blob kapsayıcısını hemen yeniden oluşturur: **CreateIfNotExists** yöntemi (istemci istek kimliği bc881924-...) sonunda HTTP 409 (Çakışma) hatasıyla başarısız oluyor. Ne zaman bir istemci blob kapsayıcılar, tablolar veya adından önce kısa bir süre yoktur sıraları siler yeniden kullanılabilir hale gelir.

Silme/yeniden oluşturun düzeni ortak ise yeni kapsayıcılar oluşturduğunda istemci uygulaması benzersiz kapsayıcı adları kullanmanız gerekir.

### <a name="metrics-show-low-percent-success"></a>Düşük PercentSuccess ölçümleri göster veya ClientOtherErrors işlem durumundaki işlemlerini analytics günlük girdilerine sahip
**PercentSuccess** ölçüm başarılı oldu, HTTP durum kodu göre işlemlerinin yüzde yakalar. 2XX durum kodları ile işlemlerini saymak durum kodları 3XX, 4XX ve 5XX aralıklardaki işlemleriyle başarısız ve daha düşük olarak sayılır ancak olarak başarılı **PercentSucess** ölçüm değeri. Sunucu tarafı depolama günlük dosyalarında bir işlem durumuyla işlemlerini kaydedilir **ClientOtherErrors**.

Bu işlemleri tamamladınız ve bu nedenle kullanılabilirliği gibi diğer ölçümleri etkilemez dikkate almak önemlidir. Başarıyla yürütülen ancak başarısız HTTP durum kodları sonuçlanabilir işlemlerinin bazı örnekler şunlardır:

* **ResourceNotFound** (değil bulunan 404), örneğin bir istekten GET var olmayan bir blob için.
* **ResouceAlreadyExists** (409 çakışma), örneğin bir **CreateIfNotExist** kaynağın zaten bulunduğu işlem.
* **ConditionNotMet** (değil değiştiren 304), bir istemci gönderdiğinde gibi örneğin bir koşullu işlem bir **ETag** değeri ve bir HTTP **If-None-Match** yalnızca varsa, görüntüyü istek üstbilgisi Son işlem itibaren güncelleştirilmiş.

Depolama Hizmetleri sayfasına dönmek ortak REST API hata kodları listesini bulabilirsiniz [ortak REST API hata kodları](http://msdn.microsoft.com/library/azure/dd179357.aspx).

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Kapasite ölçümlerini beklenmeyen artışı depolama kapasitesi kullanımı Göster
Kapasite kullanımı depolama hesabınızdaki beklenmeyen değişiklikleri ani görürseniz, kullanılabilirlik ölçümlerinizi bakarak nedenlerini araştırabilirsiniz; sayısı başarısız silme istekleri uygulamaya özgü temizleme işlemleri, serbest bırakma olması için alan boşaltın beklenen sahip (örneğin beklendiği gibi çalışmıyor olabilir olarak kullanarak blob storage miktarında artış neden, örneğin, bir artış alan boşaltıp için kullanılan SAS belirteci süresi dolduğundan).

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>Sorununuzu geliştirme veya test için depolama öykünücüsünü kullanarak ortaya çıkar.
Genellikle depolama öykünücüsünü geliştirme sırasında kullanmak ve Azure storage hesabı gereksinimini ortadan kaldırmak için sınayın. Depolama öykünücüsü kullanırken oluşabilecek yaygın sorunlar verilmiştir:

* [Özellik "X" depolama öykünücüsünde çalışmıyor]
* [Hata "HTTP üst bilgilerinden biri için değer doğru biçimde değil" depolama öykünücüsünü kullanırken]
* [Depolama öykünücüsü çalıştıran yönetici ayrıcalıkları gerektirir]

#### <a name="feature-X-is-not-working">Özellik "X" depolama öykünücüsünde çalışmıyor</a>
Depolama öykünücüsü tüm dosya hizmeti gibi Azure depolama hizmetleri özelliklerini desteklemez. Daha fazla bilgi için bkz. [Geliştirme ve Sınama için Azure Storage Öykünücüsünü Kullanma](storage-use-emulator.md).

Depolama öykünücüsü desteklemediği özellikler için Azure depolama hizmeti bulutta kullanın.

#### <a name="error-HTTP-header-not-correct-format">Hata "HTTP üst bilgilerinden biri için değer doğru biçimde değil" depolama öykünücüsünü kullanırken</a>
Yerel depolama öykünücüsü ve yöntem çağrıları karşı depolama istemci kitaplığı gibi kullanan uygulamanızı test **CreateIfNotExists** "HTTP üst bilgilerinden biri için değer doğru değil hata iletisiyle başarısız biçimi." Bu, kullanmakta olduğunuz depolama öykünücüsü sürümüne kullandığınız depolama istemci kitaplığı sürümü desteklemiyor gösterir. Depolama istemcisi kitaplığı üstbilgisi ekler **x-ms-version** kolaylaştırır tüm istekler için. Depolama öykünücüsü değeri tanımıyor varsa **x-ms-version** üstbilgisi, isteği reddeder.

Depolama kitaplık istemci günlükleri değerini görmek için kullanabileceğiniz **x-ms-version üstbilgi** onu gönderiyor. Değerini de görebilirsiniz **x-ms-version üstbilgi** istekleri, istemci uygulamasından izlemek için fiddler'ı kullanıyorsanız.

Bu senaryo genellikle yükleyin ve depolama öykünücüsü güncelleştirmeden depolama istemci kitaplığı en son sürümünü kullanmanız halinde oluşur. , Depolama öykünücüsünün en son sürümünü yükleyin veya Bulut depolama öykünücüsü yerine geliştirme ve test için kullanmak.

#### <a name="storage-emulator-requires-administrative-privileges">Depolama öykünücüsü çalıştıran yönetici ayrıcalıkları gerektirir</a>
Depolama öykünücüsü çalıştırdığınızda, yönetici kimlik bilgileri istenir. Bu, yalnızca ilk kez depolama öykünücüsünü başlatırken oluşur. Depolama öykünücüsü ayarladıktan sonra tekrar çalıştırmak için yönetici ayrıcalıkları gerekmez.

Daha fazla bilgi için bkz. [Geliştirme ve Sınama için Azure Storage Öykünücüsünü Kullanma](storage-use-emulator.md). Ayrıca yönetici ayrıcalıkları gerektirir Visual Studio depolama öykünücüsü de başlatabilirsiniz.

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>.NET için Azure SDK'sını yükleme sorunlarla
SDK'yı yüklemeye çalıştığınızda, yerel makinenizde depolama öykünücüsünü yüklenmeye çalışılırken başarısız olur. Yükleme günlüğü şu iletilerden birini içerir:

* CAQuietExec: Hata: SQL örneği erişilemiyor
* CAQuietExec: Hata: veritabanı oluşturulamadı

Var olan LocalDB yükleme ile ilgili bir sorunu nedenidir. Varsayılan olarak, depolama öykünücüsü Azure storage Hizmetleri benzetimini yapar, veri kalıcı hale getirmek için yerel veritabanı kullanır. SDK'yı yüklemeyi denemeden önce bir komut istemi penceresinde aşağıdaki komutları çalıştırarak LocalDB örneğinizi sıfırlayabilirsiniz.

```
sqllocaldb stop v11.0
sqllocaldb delete v11.0
delete %USERPROFILE%\WAStorageEmulatorDb3*.*
sqllocaldb create v11.0
```

**Silmek** komutu, önceki depolama öykünücüsünü yüklemelerinden eski tüm veritabanı dosyaları kaldırır.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>Bir depolama hizmetindeki farklı bir sorun olması
Önceki sorun giderme bölümleri bir depolama hizmetle ilgili sorun içermiyorsa, tanılama ve sorunu gidermek için aşağıdaki yaklaşımı benimsemeye.

* Beklenen taban çizgisinin davranış gelen herhangi bir değişiklik olduğunu görmek için ölçümlerinizi denetleyin. Ölçümleri, geçici veya kalıcı bir sorundur ve hangi depolama işlemleri sorunu etkileyen olup olmadığını belirlemek mümkün olabilir.
* Ölçüm bilgileri oluşan hatalar hakkında daha ayrıntılı bilgi için sunucu tarafı günlük verileri aramanıza yardımcı olması için kullanabilirsiniz. Bu bilgiler sorunu gidermenize ve sorunu çözmek yardımcı olabilir.
* Sunucu tarafı günlüklerdeki bilgiler başarıyla sorunu gidermek için yeterli değilse, istemci uygulaması ve Fiddler, Wireshark ve Microsoft Message Analyzer ağınız araştırmaya gibi araçları davranışını araştırmak için depolama istemci kitaplığı istemci-tarafı günlükleri'ni kullanabilirsiniz.

Fiddler'ı kullanma hakkında daha fazla bilgi için bkz: "[ek 1: HTTP ve HTTPS trafiğini yakalamak için Fiddler kullanarak]."

Wireshark kullanma hakkında daha fazla bilgi için bkz: "[ek 2: ağ trafiğini yakalamak için Wireshark kullanarak]."

Microsoft Message Analyzer kullanma hakkında daha fazla bilgi için bkz: "[ek 3: ağ trafiğini yakalamak için Microsoft Message Analyzer'ı kullanarak]."

## <a name="appendices"></a>Ekler
Ekler, tanılama ve Azure Storage (ve diğer hizmetleri) ile ilgili sorunları giderme zaman yararlı bulabilirsiniz çeşitli araçlar açıklanmaktadır. Bu araçları Azure Storage parçası olmayan ve üçüncü taraf ürünleri bazılarıdır. Bu nedenle, bu eklerin açıklanan araçları tarafından Microsoft Azure veya Azure Storage yaptığınız herhangi bir destek sözleşmesi kapsamında değildir ve lisans ve destek seçenekleri sağlayıcılardan bu araçların değerlendirme işleminizin bir parçası olarak bu nedenle incelemeniz gerekir.

### <a name="appendix-1"></a>Ek 1: HTTP ve HTTPS trafiğini yakalamak için fiddler'ı kullanma
[Fiddler](http://www.telerik.com/fiddler) istemci uygulamanız ve kullandığınız Azure depolama hizmeti arasındaki HTTP ve HTTPS trafiği çözümleme için yararlı bir araçtır.

> [!NOTE]
> Fiddler HTTPS trafiği kod çözme; dikkatli bir şekilde nasıl bunu yapar anlamak ve güvenlik etkilerini anlama için Fiddler belgelerine okumanız gerekir.
> 
> 

Bu ekte fiddler'ı yüklediğiniz yerel makine ve Azure storage Hizmetleri arasındaki trafiği yakalamak için fiddler'ı yapılandırmak nasıl kısa bir kılavuz sağlar.

Fiddler başlatıldıktan sonra HTTP ve HTTPS trafiği yerel makinenizde yakalama başlar. Fiddler'ı denetleme için yararlı bazı komutlar şunlardır:

* Durdurun ve trafiği yakalama başlatın. Ana menüde Git **dosya** ve ardından **trafiği Yakala** üzerinde ve yakalamak değiştirin.
* Yakalanan trafik verileri kaydedin. Ana menüde Git **dosya**, tıklatın **kaydetmek**ve ardından **tüm oturumları**: Bu trafiğin oturum arşiv dosyasında kaydetmenize olanak sağlar. Bir oturum arşiv çözümleme için daha sonra yeniden yükleyin veya Microsoft Desteği'ne istediyseniz gönderin.

Fiddler yakalar trafik miktarını sınırlamak için yapılandırdığınız filtreleri kullanabilirsiniz **filtreleri** sekmesi. Aşağıdaki ekran görüntüsü yalnızca gönderilen trafiğini yakalar bir filtre gösterir **contosoemaildist.table.core.windows.net** depolama uç noktası:

![][5]

### <a name="appendix-2"></a>Ek 2: Ağ trafiğini yakalamak için Wireshark kullanma
[Wireshark](http://www.wireshark.org/) çok çeşitli ağ protokolleri ayrıntılı paket bilgilerini görüntülemek sağlayan bir ağ protokolü çözümleyici.

Aşağıdaki yordam Wireshark tablo hizmetine Azure depolama hesabınızdaki yüklendiği yerel makineden trafiği için ayrıntılı paket bilgilerini yakalama gösterir.

1. Wireshark yerel makinenizde başlatın.
2. İçinde **Başlat** bölümünde, yerel ağ arabirim veya internet'e bağlı arabirimler seçin.
3. Tıklatın **yakalama seçenekleri**.
4. Filtre ekleme **yakalama filtresi** metin kutusu. Örneğin, **contosoemaildist.table.core.windows.net konak** yalnızca tablo Hizmeti uç bilgisayardan veya gönderilen paketleri yakalamak için Wireshark yapılandıracak **contosoemaildist** depolama hesabı. Kullanıma [yakalama filtreleri tam listesi](http://wiki.wireshark.org/CaptureFilters).
   
   ![][6]
5. Tıklatın **Başlat**. Wireshark tüm istemci uygulamanızı yerel makinenizde kullanın veya tablo Hizmeti uç noktasından paketleri göndermek yakalar.
6. Tamamladığınızda, ana menü tıklatıldığında **yakalama** ve ardından **durdurmak**.
7. Yakalanan verileri bir Wireshark yakalama dosyasında kaydetmek için ana menüde'ı **dosya** ve ardından **kaydetmek**.

WireShark mevcut herhangi bir hata vurgulayın **packetlist** penceresi. De kullanabilirsiniz **uzman bilgisi** penceresi (tıklatın **Çözümle**, ardından **uzman bilgisi**) hataların ve uyarıların özetini görüntülemek için.

![][7]

Ayrıca TCP verileri sağ tıklayıp seçerek uygulama katmanı tarafından görülen şekilde TCP verileri görüntülemeyi seçebilir **izleyin TCP akışı**. Bu, döküm yakalama filtresi olmadan yakalanmış durumunda faydalı olur. Daha fazla bilgi için bkz: [TCP akışları aşağıdaki](http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html).

![][8]

> [!NOTE]
> Wireshark kullanma hakkında daha fazla bilgi için bkz: [Wireshark Kullanıcı Kılavuzu](http://www.wireshark.org/docs/wsug_html_chunked).
> 
> 

### <a name="appendix-3"></a>Ek 3: Ağ trafiğini yakalamak Microsoft Message Analyzer kullanma
Wireshark benzer şekilde ağ trafiğini yakalamak ve Microsoft Message Analyzer Fiddler benzer şekilde HTTP ve HTTPS trafiğini yakalamak için kullanın.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Microsoft Message Analyzer kullanarak bir web izleme oturumu yapılandırın
Microsoft Message Analyzer, Microsoft Message Analyzer uygulamayı çalıştırın kullanarak HTTP ve HTTPS trafiği için ve ardından web izleme oturumu yapılandırmak için **dosya** menüsünde tıklatın **yakalama/izleme**. Kullanılabilir izleme senaryoları listesinde seçin **Web Proxy**. Ardından **izleme senaryo Yapılandırması** paneli, buna **HostnameFilter** metin kutusuna, depolama noktalarınızı adlarını ekleyin (Bu adlarında arayabilir [Azure portal](https://portal.azure.com)). Örneğin, Azure depolama hesabınızın adını ise **contosodata**, aşağıdaki eklemelisiniz **HostnameFilter** textbox:

```
contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net
```

> [!NOTE]
> Ana bilgisayar adı bir boşluk karakteri ayırır.
> 
> 

İzleme verilerini toplamaya başlamak hazır olduğunuzda **Başlat ile** düğmesi.

Microsoft Message Analyzer hakkında daha fazla bilgi için **Web Proxy** izlemek için bkz: [Microsoft PEF WebProxy sağlayıcısı](http://technet.microsoft.com/library/jj674814.aspx).

Yerleşik **Web Proxy** Microsoft Message Analyzer içinde izleme Fiddler üzerinde temel; istemci-tarafı HTTPS trafiği yakalayabilir ve şifrelenmemiş HTTPS iletileri görüntüler. **Web Proxy** izleme works şifrelenmemiş iletileri erişim sağlayan tüm HTTP ve HTTPS trafiği için yerel bir ara yapılandırarak.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Microsoft Message Analyzer kullanarak ağ sorunlarını tanılama
Microsoft Message Analyzer'ı kullanmanın yanı sıra **Web Proxy** istemci uygulaması ve depolama hizmeti arasında HTTP/HTTPs trafiğini ayrıntılarını yakalamak için izleme, yerleşik de kullanabilirsiniz **yerel bağlantı katmanı** ağ paket bilgileri yakalamak için izleme. Bırakılan paketleri ile Wireshark yakalamak ve tanılama benzer verilerini yakalamak için sorunları gibi ağ bu etkinleştirir.

Aşağıdaki ekran görüntüsünde bir örneği gösterir **yerel bağlantı katmanı** bazı izleme **bilgilendirme** içinde iletileri **DiagnosisTypes** sütun. Bir simgeyi tıklatarak **DiagnosisTypes** sütun iletisinin ayrıntılarını gösterir. Bu örnekte, sunucu ileti #305 yeniden aktarılan, bu onay istemciden almadığı için:

![][9]

Microsoft Message Analyzer izleme oturumu oluşturduğunuzda, izleme gürültü miktarını azaltmak için filtreler belirtebilirsiniz. Üzerinde **yakalama / izleme** izleme tanımladığınız yerlerde sayfasını tıklatın **yapılandırma** bağlantısına **Microsoft-Windows-NDIS-PacketCapture**. Aşağıdaki ekran görüntüsünde TCP trafiği için üç depolama hizmetleri IP adreslerini filtreler bir yapılandırması gösterilmektedir:

![][10]

Microsoft Message Analyzer yerel bağlantı katmanı izleme hakkında daha fazla bilgi için bkz: [PEF NDIS PacketCapture Microsoft sağlayıcısı](http://technet.microsoft.com/library/jj659264.aspx).

### <a name="appendix-4"></a>Ek 4: Ölçümleri görüntülemek ve verileri günlüğe kaydetmek için Excel kullanma
Birçok aracı depolama ölçüm verilerini görüntüleme ve analiz için verileri Excel'e yükleme kolaylaştırır sınırlandırılmış biçimde Azure tablo depolaması indirmesine olanak sağlar. Excel'e yükleyebilir sınırlandırılmış biçimde depolama günlük verileri Azure blob depolama biriminden zaten var. Ancak, uygun sütun başlıkları bilgilerini temel eklemeniz gerekecektir [depolama Analytics günlük biçimi](http://msdn.microsoft.com/library/azure/hh343259.aspx) ve [Storage Analytics Ölçüm tablosu şeması](http://msdn.microsoft.com/library/azure/hh343264.aspx).

Blob depolama alanından indirdikten sonra depolama günlüğü verileri Excel'e aktarmak için:

* Üzerinde **veri** menüsünde tıklatın **metindeki**.
* Önce görüntülemek istediğiniz günlük dosyasına gözatın **alma**.
* Adım 1 / **Metin Alma Sihirbazı**seçin **sınırlandırılmış**.

Adım 1 / **Metin Alma Sihirbazı**seçin **noktalı** yalnızca ayırıcı olarak ve çift tırnak işareti olarak seçin **Metin niteleyicisi**. Ardından **son** ve çalışma kitabınızı verileri nereye'i seçin.

### <a name="appendix-5"></a>Ek 5: Visual Studio Team Services için Application Insights ile izleme
Ayrıca, Visual Studio Team Services için performans ve kullanılabilirlik izlemesi parçası olarak Application Insights özelliğini kullanabilirsiniz. Bu araç şunları yapabilir:

* Web hizmetiniz kullanılabilir ve yanıt verebilir durumda olduğundan emin olun. Uygulamanızı bir web sitesi veya web hizmeti kullanan bir cihaz uygulaması olup, dünyanın konumlardan birkaç dakikada URL'nizi test ve bir sorun olup olmadığını bilmek olanak verir.
* Hızlı bir şekilde herhangi bir performans sorunları veya web hizmetiniz durumlar tanılayın. CPU veya diğer kaynakları uzatılır değilse, özel durumlar Yığın izlemeleri edinin öğrenmek ve günlük izlemelerini kolayca arayın. Microsoft, uygulamanın performansı kabul edilebilir sınırlar düşerse, bir e-posta gönderebilirsiniz. .NET ve Java web Hizmetleri'ni izleyebilirsiniz.

Daha fazla bilgi bulabilirsiniz [Application Insights nedir](../../application-insights/app-insights-overview.md).

<!--Anchors-->
[Giriş]: #introduction
[Bu kılavuz nasıl düzenlenir]: #how-this-guide-is-organized

[, depolama hizmet izlemesini]: #monitoring-your-storage-service
[Hizmet durumu izleme]: #monitoring-service-health
[Kapasite izleme]: #monitoring-capacity
[Kullanılabilirlik izlemesi]: #monitoring-availability
[Performans izleme]: #monitoring-performance

[depolama sorunları tanılama]: #diagnosing-storage-issues
[Hizmet sistem durumu sorunları]: #service-health-issues
[performans sorunları]: #performance-issues
[Hatalarını tanılama]: #diagnosing-errors
[Depolama öykünücüsü sorunları]: #storage-emulator-issues
[Depolama günlük araçları]: #storage-logging-tools
[Ağ günlük araçlarını kullanarak]: #using-network-logging-tools

[uçtan uca izleme]: #end-to-end-tracing
[Günlük verileri ilişkilendirme]: #correlating-log-data
[İstemci istek kimliği]: #client-request-id
[sunucu istek kimliği]: #server-request-id
[Zaman damgaları]: #timestamps

[sorun giderme kılavuzluğu]: #troubleshooting-guidance
[ölçümleri Göster yüksek AverageE2ELatency ve düşük AverageServerLatency]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor, ancak istemci yüksek gecikme durumu yaşıyor]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Ölçümler yüksek AverageServerLatency gösteriyor]: #metrics-show-high-AverageServerLatency
[Kuyrukta ileti tesliminde beklenmeyen gecikmeler yaşıyorsunuz]: #you-are-experiencing-unexpected-delays-in-message-delivery

[ölçümleri Göster artışı içinde PercentThrottlingError]: #metrics-show-an-increase-in-PercentThrottlingError
[PercentThrottlingError geçici artış]: #transient-increase-in-PercentThrottlingError
[PercentThrottlingError hata kalıcı artış]: #permanent-increase-in-PercentThrottlingError
[ölçümleri Göster artışı içinde PercentTimeoutError]: #metrics-show-an-increase-in-PercentTimeoutError
[Ölçümler PercentNetworkError’da artış gösteriyor]: #metrics-show-an-increase-in-PercentNetworkError

[İstemci HTTP 403 (Yasak) iletileri alma]: #the-client-is-receiving-403-messages
[İstemci HTTP 404 (bulunamadı) iletileri alma]: #the-client-is-receiving-404-messages
[İstemci veya başka bir işlem nesne daha önce silinmiş]: #client-previously-deleted-the-object
[Bir paylaşılan erişim imzası (SAS) yetkilendirme sorunu]: #SAS-authorization-issue
[İstemci tarafı JavaScript kodu nesneye erişim izni yok]: #JavaScript-code-does-not-have-permission
[Ağ hatası]: #network-failure
[İstemci HTTP 409 (Çakışma) iletileri alma]: #the-client-is-receiving-409-messages

[ölçümleri Göster düşük PercentSuccess veya analytics günlük girişlerini sahip hareket durumu işlemler ClientOtherErrors,]: #metrics-show-low-percent-success
[Kapasite ölçümlerini beklenmeyen artışı depolama kapasitesi kullanımı Göster]: #capacity-metrics-show-an-unexpected-increase
[Sorununuzu geliştirme veya test için depolama öykünücüsünü kullanarak ortaya çıkar.]: #your-issue-arises-from-using-the-storage-emulator
[Özellik "X" depolama öykünücüsünde çalışmıyor]: #feature-X-is-not-working
[Hata "HTTP üst bilgilerinden biri için değer doğru biçimde değil" depolama öykünücüsünü kullanırken]: #error-HTTP-header-not-correct-format
[Depolama öykünücüsü çalıştıran yönetici ayrıcalıkları gerektirir]: #storage-emulator-requires-administrative-privileges
[.NET için Azure SDK'sını yükleme sorunlarla]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[Bir depolama hizmetindeki farklı bir sorun olması]: #you-have-a-different-issue-with-a-storage-service

[ekler]: #appendices
[ek 1: HTTP ve HTTPS trafiğini yakalamak için Fiddler kullanarak]: #appendix-1
[ek 2: ağ trafiğini yakalamak için Wireshark kullanarak]: #appendix-2
[ek 3: ağ trafiğini yakalamak için Microsoft Message Analyzer'ı kullanarak]: #appendix-3
[Ek 4: Ölçümleri görüntülemek ve verileri günlüğe kaydetmek için Excel kullanma]: #appendix-4
[Ek 5: Visual Studio Team Services için Application Insights ile izleme]: #appendix-5

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
