---
title: Azure İzleyici’yi kullanmaya başlama
description: Kaynaklarınızın çalışmasını anlamak ve verilere dayalı işlem yapmak için Azure İzleyici kullanmaya başlayın.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/25/2018
ms.author: johnkem
ms.component: ''
ms.openlocfilehash: 70807db256f72b77bb29db3f6f59474a892f2939
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263370"
---
# <a name="get-started-with-azure-monitor"></a>Azure İzleyici’yi kullanmaya başlama
Azure İzleyici, Azure kaynaklarını izlemeye yönelik tek bir kaynak sağlayan platform hizmetidir. Azure izleme ile görselleştirme, sorgulama yapabilir, yol, arşiv ve aksi halde ölçümleri ve Azure kaynaklarında'ten gelen günlükleri eylemi gerçekleştirin. Bu verileri Azure portal kullanarak ile çalışabilirsiniz [İzleyici PowerShell cmdlet'leri](insights-powershell-samples.md), [platformlar arası CLI](insights-cli-samples.md), veya [Azure İzleyici REST API'lerini](https://msdn.microsoft.com/library/dn931943.aspx). Bu makalede portal gösterim amacıyla kullanılarak Azure İzleyici’nin temel bileşenlerinden birkaç tanesi gösterilecektir.

## <a name="walkthrough"></a>Kılavuz
1. Portalı'nda gidin **tüm hizmetleri** ve Bul **İzleyici** seçeneği. Bu seçeneği sol gezinti çubuğundan kolayca erişilebilmesi için sık kullanılanlar listenize eklemek üzere yıldız simgesine tıklayın.

    ![Hizmet listesinde İzleyici](./media/monitoring-get-started/monitor-more-services.png)
2. Tıklatın **İzleyici** seçeneği açık **İzleyici** sayfası. Bu sayfa, izleme ayarları ve verileri birlikte bir birleştirilmiş görünüme sağlar. İlk için açılır **genel bakış** bölümü. Genel Bakış tüm izleme uyarıları, hataları ve kaynakları ile ilgili hizmet sistem durumu danışma dökümünü gösterir.  

    ![İzleme Gezinti](./media/monitoring-get-started/monitor-blade-nav.png)

    Azure İzleyici, verileri üç temel kategoride izler: **etkinlik günlüğü**, **ölçümler** ve **tanılama günlükleri**.
3. Etkinlik günlüğü bölümünün gösterildiğinden emin olmak için **Etkinlik günlüğü**’ne tıklayın.

    [**Etkinlik günlüğü**](monitoring-overview-activity-logs.md), aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen tüm işlemleri açıklar. Etkinlik Günlüğü’nü kullanarak aboneliğinizdeki kaynaklarla ilgili herhangi bir oluşturma, güncelleştirme veya silme işlemine ilişkin ‘ne, kim ve ne zaman’ sorularına yanıt bulabilirsiniz. Örneğin, Etkinlik Günlüğü bir web uygulamasının ne zaman ve kim tarafından durdurulduğunu söyler. Etkinlik Günlüğü olayları platforma depolanır ve 90 gün boyunca sorgulanabilir.

    ![Etkinlik Günlüğü](./media/monitoring-get-started/monitor-act-log-blade.png)

    Ortak filtrelere yönelik sorgular oluşturup kaydedebilir ve sonra ölçütlerinizi karşılayan olayların gerçekleşip gerçekleşmediğinden her zaman haberdar olmak için en önemli sorguları bir portal panosuna sabitleyebilirsiniz.
4. Görünümü son bir haftadaki belirli bir kaynak grubu ile filtreleyin, ardından **Kaydet** düğmesine tıklayın. Sorgunuz bir ad verin.

    ![Etkinlik günlüğü sorgusunu kaydedin](./media/monitoring-get-started/monitor-act-log-save.png)
5. Şimdi **Sabitle** düğmesine tıklayın.

    ![Etkinlik günlüğü için sabitle düğmesine tıklayın](./media/monitoring-get-started/monitor-act-log-pin.png)

    Bu kılavuzdaki görünümlerin birçoğu panoya sabitlenebilir. Bunun yapılması, hizmetlerinize ilişkin çalışma verilerine ait tek bir bilgi kaynağı oluşturmanıza yardımcı olur.
6. Panonuza geri dönün. Şu anda sorgunun (ve sonuç sayısının) panonuzda gösterildiğini görebilirsiniz. Bu hızlı bir şekilde, aboneliğinizde son oluşan herhangi bir yüksek profilli eylem görmek istiyorsanız, örneğin yeni bir rolü atandı veya VM silindi yararlı olur.

    ![Panosuna sabitlediğiniz etkinlik günlükleri](./media/monitoring-get-started/monitor-act-log-db.png)
7. **İzleyici** kutucuğuna geri dönüp **Ölçümler** bölümüne tıklayın. İlk kaynak filtreleme ve sayfanın en üstünde açılan Seçenekleri'ni kullanarak seçerek seçmeniz gerekir.

    ![Ölçümler için kaynak filtreleme](./media/monitoring-get-started/monitor-met-filter.png)

    Tüm Azure kaynakları [**ölçümler**](monitoring-overview-metrics.md) gösterir. Bu görünüm, kaynaklarınızın performansını kolayca anlayabilmeniz için tüm ölçümleri tek bir cam bölmede bir araya getirir. Ayrıca bizim marka denetleyin [deneyimi grafik yeni ölçümü](https://aka.ms/azuremonitor/new-metrics-charts) tıklayarak **ölçümleri (Önizleme)** sekmesi.
8. Bir kaynak seçtikten sonra tüm kullanılabilir ölçümler sayfanın sol tarafında görünür. Ölçümleri seçip grafik türü ile saat aralığını değiştirerek birden fazla ölçümün grafiğini tek seferde oluşturabilirsiniz. Ayrıca bu kaynak üzerinde oluşturulmuş tüm ölçüm uyarılarını görüntüleyebilirsiniz.

    ![Ölçüm dikey penceresi](./media/monitoring-get-started/monitor-metric-blade.png)

   > [!NOTE]
   > Bazı ölçümlerini etkinleştirerek kullanılabilirdir [Application Insights](../application-insights/app-insights-overview.md) ve/veya kaynağınız Windows veya Linux Azure tanılama uzantısını.
   >
   >

9. Grafiğiniz hazır olduğunda **Sabitle** düğmesini kullanarak grafiği panoya sabitleyebilirsiniz.
10. Geri dönüp **İzleyici** tıklatıp **tanılama günlükleri**.

    ![Tanılama günlükleri dikey penceresi](./media/monitoring-get-started/monitor-diaglogs-blade.png)

    [**Tanılama günlükleri**](monitoring-overview-of-diagnostic-logs.md), kendi çalışması hakkında veriler sağlayan belirli bir kaynak *tarafından* gösterilen günlüklerdir. Örneğin, Ağ Güvenliği Grup Kuralı Sayaçları ve Mantıksal Uygulama İş Akışı Günlükleri, tanılama günlüğü türleridir. Bu günlükler bir depolama hesabına depolanabilir, Event Hub’da yayınlanabilir ve/veya [Log Analytics](../log-analytics/log-analytics-overview.md)’e gönderilebilir. Log Analytics, Microsoft'un gelişmiş arama ve uyarı vermeye yönelik işletimsel bilgi ürünüdür.

    Portalda, aboneliğinizdeki tüm kaynakların listesini görüntüleyebilir ve tanılama günlüklerinin etkin olup olmadığını belirlemek üzere bu listeyi filtreleyebilirsiniz.
    > [!NOTE]
    > Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
    >
    > *Örneğin*: Bir Olay Hub'ındaki 'Gelen İletiler' ölçümü, kuyruk düzeyi temelinde araştırılıp grafiği oluşturulabilir. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
    >
    >

11. Tanılama günlükleri sayfasında bir kaynağa tıklayın. Tanılama günlükleriniz bir depolama hesabına kaydediliyorsa doğrudan indirebileceğiniz saatlik günlüklerin bir listesini görürsünüz.

    ![Bir kaynağın tanılama günlükleri](./media/monitoring-get-started/monitor-diaglogs-detail.png)

    Bir depolama hesabına arşivleme, Event Hubs’da yayınlama veya Log Analytics çalışma alanına gönderme amacıyla ayarlarınızı düzenlemek veya değiştirmek için **Tanılama Ayarları**’na da tıklayabilirsiniz.

    ![Tanılama günlüklerini etkinleştirme](./media/monitoring-get-started/monitor-diaglogs-enable.png)

    Log Analytics için tanılama günlüklerini ayarladıysanız bu tanılama günlüklerini portalın **Günlük arama** bölümünde arayabilirsiniz.
12. Gidin **uyarıları (Klasik)** İzleyici sayfasının bölümünde.

    ![genel kullanıma yönelik uyarılar dikey penceresi](./media/monitoring-get-started/monitor-alerts-nopp.png)

    Tüm yönetmek burada [ **Klasik uyarıları** ](monitoring-overview-alerts.md) Azure kaynaklarınızı üzerinde. Bu ölçümleri, etkinlik günlüğü olaylarını, Application Insights web testleri (konumlara) ve Application Insights öngörülü tanılama uyarılarını içerir. Uyarıları eylem gruplara bağlanın. [Eylem grupları](monitoring-action-groups.md) kişilere bildirmek veya bir uyarı oluşturulduğunda belirli eylemleri gerçekleştirmek için bir yol sağlar.

13. Uyarı oluşturmak için **Ölçüm uyarısı ekle**’ye tıklayın.

    ![ölçüm uyarısı ekleme](./media/monitoring-get-started/monitor-alerts-add.png)

    Bundan sonra uyarının durumunu dilediğiniz zaman kolayca görmek için uyarıyı panonuza sabitleyebilirsiniz.

    Ayrıca Azure İzleyici artık sahiptir [ **yeni uyarılar** ](https://aka.ms/azuremonitor/near-real-time-alerts) sıklığı her dakika kadar düşük yapılamıyor.

14. İzleyici bölümünde [Application Insights](../application-insights/app-insights-overview.md) uygulamaları ve [Log Analytics](../log-analytics/log-analytics-overview.md) yönetim çözümleriyle ilgili bağlantılar da bulunur. Bu diğer Microsoft ürünleri, Azure İzleyici ile kapsamlı tümleştirmeye sahiptir.
15. Application Insights veya Log Analytics kullanmıyorsanız Azure İzleyici mevcut izleme, günlüğe kaydetme ve uyarı verme ürünleriyle bir ortaklığa sahip olabilir. Tam liste ve tümleştirme yönergeleri için [ortaklar sayfamıza](monitoring-partners.md) bakın.

Aşağıdaki adımları izleyerek ve tüm ilgili kutucukları panoya sabitleyerek uygulamanızın ve altyapınızın aşağıdaki gibi kapsamlı görünümlerini oluşturabilirsiniz:

![Azure İzleyici panosu](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Sonraki adımlar
* Okuma [izleme araçları tüm Azure genel bakış](monitoring-overview.md) Azure İzleyici bunlarla nasıl çalıştığını anlamak için.
