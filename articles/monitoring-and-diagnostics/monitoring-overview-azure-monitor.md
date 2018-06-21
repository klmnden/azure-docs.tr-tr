---
title: Azure İzleyiciye Genel Bakış
description: Azure İzleyici uyarılar, web kancaları, otomatik ölçeklendirme ve otomasyonda kullanılan istatistikleri toplar. Makalede ayrıca diğer Microsoft izleme seçenekleri listelenmiştir.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: overview
ms.date: 03/28/2018
ms.author: robb
ms.custom: mvc
ms.component: ''
ms.openlocfilehash: a96991c424b4709002d46b6b7abe1e884c3605dd
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264594"
---
# <a name="overview-of-azure-monitor"></a>Azure İzleyici’ye genel bakış
Bu makalede Microsoft Azure’daki Azure İzleyici hizmetine genel bir bakış sunulmaktadır. Azure İzleyici’nin ne yaptığı açıklanmış ve Azure İzleyici’yi nasıl kullanacağınız hakkında ek bilgiler verilmiştir.  Videolu bir tanıtım tercih ederseniz, bu makalenin sonundaki sonraki adımlar bağlantılarına bakın. 

## <a name="azure-monitor-and-microsofts-other-monitoring-products"></a>Azure İzleyici ve Microsoft’un diğer izleme ürünleri
Azure İzleyici, çoğu Microsoft Azure’daki çoğu hizmet için temel düzey altyapı ölçümleri ve günlükleri sağlar. Henüz verilerini Azure İzleyiciye göndermeyen Azure Hizmetleri gelecekte bunu yapacaktır.

Microsoft, şirket içi kurulumları olan geliştiriciler, geliştirici operasyonları ve BT operasyonları için de ek izleme olanakları sağlayan ek ürün ve hizmetler sunar. Bu farklı ürün ve hizmetlere bir genel bakış ve bunların birlikte nasıl çalıştıklarının açıklaması için bkz. [Microsoft Azure'da izleme](monitoring-overview.md).

## <a name="portal-overview-page"></a>Portala genel bakış sayfası

Azure İzleyici’de kullanıcılara yardımcı olan bir giriş sayfası vardır: 
- Azure tarafından sunulan izleme olanaklarını öğrenin.
- Azure platformunu ve premium izleme olanaklarını keşfedin, yapılandırın ve kullanıma alın.

Bu sayfa, kullanıma alma aşaması da dahil olmak üzere her konu için giriş noktanızdır. Farklı hizmetlerden önemli sorunları içerir ve kullanıcının bunları bağlam içinde görmesine olanak verir.
 
![İşlem dışı kaynaklar için izleme ve tanılama modeli](./media/monitoring-overview-azure-monitor/monitor-overview-ux2.png)

Sayfayı açtığınızda, okuma erişiminiz olan abonelikler arasından seçim yapabilirsiniz. Seçilen bir abonelik için şunları görebilirsiniz:

- **Tetiklenen uyarılar ve uyarı kaynakları** - Bu tabloda özet sayıları, uyarı kaynakları ve seçilen sürede uyarıların kaç kez tetiklendiği gösterilir. Hem eski hem de yeni uyarılar için geçerlidir. [Yeni Azure uyarıları](monitoring-overview-unified-alerts.md) hakkında daha fazla bilgi edinin. 
- **Etkinlik Günlüğü Hataları** -Azure kaynaklarınızdan herhangi biri bir hata düzeyi önem derecesine sahip olay kaydederse genel bir kayıt sayısı görebilir ve etkinlik günlüğü sayfasında tıklayarak her bir olayı inceleyebilirsiniz.
- **Azure Hizmet Durumu** -Hizmet Durumu hizmet sorunları, planlı bakım etkinlikleri ve sistem durumu danışma sayısını görebilirsiniz. Azure Hizmet Durumu, Azure altyapı sorunları hizmetlerinizi etkilediğinde kişiselleştirilmiş bilgiler sağlar.  Daha fazla bilgi için bkz. [Azure Hizmet Durumu](../service-health/service-health-overview.md).  
- **Application Insights** -Geçerli abonelikteki her AppInsights kaynağı için KPI'leri görün. KPI'lar; ASP.NET web uygulamaları, Java, Node ve genel uygulama türleri üzerinde sunucu tarafı uygulama izleme için en iyi duruma getirilmiştir. KPI'lar istek sıklığı, yanıt süresi, hata oranı ve kullanılabilirlik yüzdesi ölçümlerini içerir. 

Log Analytics veya Application Insights kullanmıyorsanız veya geçerli abonelikte herhangi bir Azure Uyarısı yapılandırmadıysanız, sayfa devreye alma işlemini başlatmak için bağlantılar sağlar.



## <a name="azure-monitor-sources---compute-subset"></a>Azure İzleyici Kaynakları - İşlem alt kümesi

![İşlem dışı kaynaklar için izleme ve tanılama modeli](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-compute_v6.png)


Buradaki İşlem hizmetleri şunları içerir: 
- Cloud Services 
- Virtual Machines 
- Sanal Makine ölçek kümeleri 
- Service Fabric

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Uygulama - Tanılama Günlükleri, Uygulama Günlükleri ve Ölçümler
Uygulamalar, işlem modelinde konuk işletim sistemi üzerinde çalıştırılabilir. Kendi günlüklerini ve ölçümlerini yayınlarlar. Azure İzleyici çoğu uygulama düzeyi ölçümleri ve günlükleri toplamak için Azure tanılama uzantısını (Windows veya Linux) kullanır. Türlere şunlar dahildir:

* Performans sayaçları
* Uygulama Günlükleri
* Windows Olay Günlükleri
* .NET Olay Kaynağı
* IIS Günlükleri
* Bildirim tabanlı ETW
* Kilitlenme Bilgi Dökümleri
* Müşteri Hata Günlükleri

Tanılama uzantısı olmadan yalnızca CPU kullanımı gibi birkaç ölçüm kullanılabilir. 

### <a name="host-and-guest-vm-metrics"></a>Konak ve Konuk VM ölçümleri
Daha önce listelenen işlem kaynaklarının ayrılmış bir konak VM ve konuk işletim sistemi ile etkileşimi vardır. Konak VM ve konuk işletim sistemi Hyper-V hiper yönetici modelinde kök VM ve konuk VM ile eşdeğerdir. İkisinde de ölçüm toplayabilirsiniz. Konuk işletim sisteminde de tanılama günlüklerini toplayabilirsiniz.   

### <a name="activity-log"></a>Etkinlik Günlüğü
Azure altyapısı tarafından görülen şekliyle kaynağınız hakkında bilgi için (daha önce işlemsel veya denetim günlüklerini olarak adlandırılmıştır) Etkinlik Günlüğünde arama yapabilirsiniz. Günlük, kaynakların oluşturulduğu veya yok edildiği zamanlar gibi bilgiler içerir.  Daha fazla bilgi için bkz. [Etkinlik Günlüğüne genel bakış](monitoring-overview-activity-logs.md). 

## <a name="azure-monitor-sources---everything-else"></a>Azure İzleyici Kaynakları - diğer her şey

![İşlem kaynakları için izleme ve tanılama modeli](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-non-compute_v6.png)


### <a name="resource---metrics-and-diagnostics-logs"></a>Kaynak - Ölçümler ve Tanılama Günlükleri
Toplanabilir ölçümler ve tanılama günlükleri kaynak türüne bağlı olarak değişir. Örneğin, Web Apps Disk G/Ç ve CPU yüzdesi hakkında istatistikler sağlar. Bu ölçümler kuyruk boyutu ve ileti işleme gibi ölçümleri sağlayan Service Bus kuyruğunda yoktur. Her kaynak için toplanabilir ölçümlerin bir listesi [desteklenen ölçümler](monitoring-supported-metrics.md) altında bulunabilir. 

### <a name="host-and-guest-vm-metrics"></a>Konak ve Konuk VM ölçümleri
Kaynağınız ve belirli bir Konak ve Konuk VM’leri arasında 1’e 1 bir eşleme olması gerekmediğinden ölçümler kullanılabilir değildir.

### <a name="activity-log"></a>Etkinlik Günlüğü
Etkinlik günlüğü işlem kaynakları ile aynıdır.  

## <a name="uses-for-monitoring-data"></a>İzleme Verilerinin Kullanımı
Verilerinizi topladıktan sonra Azure İzleyici'de aşağıdakileri yapabilirsiniz.

### <a name="route"></a>Yol
İzleme verilerinin başka konumlara akışını sağlayabilirsiniz. 

Örneklere şunlar dahildir:

- Daha zengin görselleştirme ve çözümleme araçlarını kullanmak için Application Insights'a gönderebilirsiniz.
- Üçüncü taraf araçlarına yönlendirmek için Event Hubs'a gönderebilirsiniz. 

### <a name="store-and-archive"></a>Yedekleme ve Arşivleme
Bazı izleme verileri Azure İzleyici'de belirli bir süre boyunca depolanır ve kullanılabilir. 
- Ölçümler 90 gün süreyle depolanır. 
- Etkinlik günlüğü girişleri 90 gün süreyle depolanır. 
- Tanılama günlükleri depolanmaz. 

Verileri yukarıda listelenen sürelerden daha uzun süre depolamak istiyorsanız Azure depolamayı kullanabilirsiniz. İzleme verileri, ayarladığınız bekletme ilkesine göre depolama hesabınızda tutulur. Verilerin Azure depolama alanında kapladığı alan için ödeme yapmanız gerekir. 

Bu verileri kullanmanın birkaç yolu:

- Yazıldıktan sonra verileri okumak ve işlemek için Azure içinde veya dışında diğer araçları kullanabilirsiniz.
- Verileri uzun süreliğine tutmak için yerel olarak indirebilir veya buluttaki tutma ilkenizi değiştirebilirsiniz.  
- Verilerinizi arşiv amacıyla Azure Depolama'da süresiz olarak bırakabilirsiniz. 

### <a name="query"></a>Sorgu
Sistem ya da Azure depolamada verilere erişmek için Azure İzleyici REST API'sini, çapraz platform Komut Satırı Arabirimi (CLI) komutlarını, PowerShell cmdlet'lerini veya .NET SDK'sini kullanabilirsiniz

Örneklere şunlar dahildir:

* Yazdığınız özel bir izleme uygulaması için veri alma
* Özel sorgular oluşturma ve bu verileri bir üçüncü taraf uygulamaya gönderme.

### <a name="visualize"></a>Görselleştirme
İzleme verilerinizi grafik ve tablo olarak görselleştirmek eğilimleri doğrudan veriye bakmaya göre daha hızlı bulmanıza yardımcı olur.  

Görselleştirme yöntemlerinden bazıları şunlardır:

* Azure portalı kullanma
* Azure Application Insights’a veri yönlendirme
* Microsoft PowerBI’a veri yönlendirme
* Canlı akış kullanarak veya aracın Azure depolama alanındaki bir arşivden okumasını sağlayarak verileri bir üçüncü taraf görselleştirme aracına yönlendirme


### <a name="automate"></a>Otomatikleştirme
> [!NOTE]
> Microsoft Azure’daki uyarıların evriminin bir parçası olarak, uyarılar için birleşik bir deneyim kullanıma sunulmuştur. Daha fazla ayrıntı için bkz. [yeni Azure uyarıları](monitoring-overview-unified-alerts.md)

Azure uyarılarında izleme verilerini uyarıları veya işlemleri tetiklemek için kullanabilirsiniz. Örneklere şunlar dahildir:

* Uygulama yüküne işlem örneklerinin ölçeğini genişletmek veya daraltmak için veri kullanma.
* Ölçüm ya da günlük koşullara göre e-posta gönderme. 
* Azure dışında bir sistemde bir eylemi çalıştırmak için bir web URL'si (web kancası) çağırma
* Çeşitli görevleri gerçekleştirmek için Azure otomasyonunda bir runbook başlatma

## <a name="methods-of-accessing-azure-monitor"></a>Azure İzleyici'ye erişim yöntemleri
Genel olarak, aşağıdaki yöntemlerden birini kullanarak veri izleme, yönlendirme ve alma yöntemlerini yönetebilirsiniz. Tüm eylemler veya veri türleri için tüm yöntemler kullanılabilir olmayabilir.

* [Azure Portal](https://portal.azure.com)
* [PowerShell](insights-powershell-samples.md)  
* [Platformlar arası Komut Satırı Arabirimi (CLI)](insights-cli-samples.md)
* [REST API](https://docs.microsoft.com/rest/api/monitor/)
* [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## <a name="next-steps"></a>Sonraki adımlar
Şu konular hakkında daha fazla bilgi edinin:
- Azure İzleyici'nin bir videosu şu adresten edinilebilir:  
[Azure İzleyici’yi kullanmaya başlama](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor). 
- Azure İzleyici’yi kullanabileceğiniz bir senaryoyu açıklayan bir video için bkz. [Microsoft Azure izleme ve tanılamayı keşfedin](https://channel9.msdn.com/events/Ignite/2016/BRK2234) ve [Ignite 2016'dan bir videoda Azure İzleyicisi](https://myignite.microsoft.com/videos/4977).
- Azure İzleyici arabirimini [Azure İzleyici ile çalışmaya başlama](monitoring-get-started.md) makalesinde öğrenebilirsiniz
- Bulut Hizmeti, Sanal Makine ölçek kümeleri veya Service Fabric uygulamasında sorunları tanılamaya çalışıyorsanız [Azure Tanılama Uzantıları](../azure-diagnostics.md)’nı ayarlayın.
- App Service Web uygulamanızda sorunları tanılamaya çalışıyorsanız [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) kullanın.
- Depolama Blob'ları, Tabloları veya Kuyruklarını kullanırken [Azure Depolama sorunlarını giderme](../storage/common/storage-e2e-troubleshooting.md)
- [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/)
