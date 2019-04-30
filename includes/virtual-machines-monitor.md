---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 01/27/2019
ms.author: cynthn
ms.openlocfilehash: 2978da7f2e7ec27ded6b5994570fa50a9032d0d2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61473980"
---
Toplama, görüntüleme ve çözümleme tanılama Vm'lerinizi izleme için birçok fırsat avantajlarından yararlanın ve verilerini günlüğe kaydedebilirsiniz. Basit yapmak için [izleme](../articles/azure-monitor/overview.md) VM'NİZDE, Azure portalında sanal makine için genel bakış ekranını kullanabilirsiniz. Kullanabileceğiniz [uzantıları](../articles/virtual-machines/windows/extensions-features.md) Vm'lerinizde ek ölçüm verilerini toplamak için tanılamayı yapılandırmak için. Gibi daha gelişmiş izleme seçeneklerini kullanabilirsiniz [Application Insights](../articles/azure-monitor/app/app-insights-overview.md) ve [Log Analytics](../articles/azure-monitor/log-query/log-query-overview.md).

## <a name="diagnostics-and-metrics"></a>Tanılama ve ölçümler 

Ayarlama ve izleme koleksiyonunu [tanılama verilerini](https://docs.microsoft.com/cli/azure/vm/diagnostics) kullanarak [ölçümleri](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md) Azure portalı, Azure CLI, Azure PowerShell ve programlama uygulama programlama arabirimleri (API). Örneğin, şunları yapabilirsiniz:

- **Temel ölçümler için VM gözlemleyin.** Azure portalının genel bakış ekranda gösterilen temel ölçümleri CPU kullanımı, ağ kullanımı, toplam disk bayt ve saniye başına disk işlemleri içerir.

- **Önyükleme tanılaması toplanmasını etkinleştirmek ve Azure portalını kullanarak görüntüleyebilirsiniz.** Kendi görüntünüzü Azure veya hatta bir platform görüntüleri önyükleme yapma sırasında neden VM önyüklenebilir olmayan bir duruma gelmesinin birçok nedeni olabilir. Tıklayarak bir VM oluşturduğunuzda önyükleme tanılamaları kolayca etkinleştirebilirsiniz **etkin** ayarlar ekranının izleme bölümünde önyükleme tanılaması için.

    Vm'lerde önyükleme yapılırken önyükleme tanılama Aracısı önyükleme çıktısını yakalar ve Azure depolama alanında depolar. Bu veriler VM önyükleme sorunlarını gidermek için kullanılabilir. Komut satırı araçları'ndan bir VM oluşturduğunuzda önyükleme tanılamaları otomatik olarak etkinleştirilmez. Önyükleme tanılamalarını etkinleştirmeden önce, önyükleme günlüklerini depolamak için bir depolama hesabı oluşturulmalıdır. Azure portalında önyükleme tanılaması etkinleştirirseniz, otomatik olarak bir depolama hesabı oluşturulur.

    Önyükleme tanılaması etkinleştirmediyseniz, sanal Makineyi oluştururken, her zaman daha sonra kullanarak etkinleştirebilirsiniz [Azure CLI](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics), [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.compute/set-azvmbootdiagnostics), veya bir [Azure Resource Manager şablonu](../articles/virtual-machines/windows/extensions-diagnostics-template.md).

- **Konuk işletim sistemi tanılama verilerinin toplanmasını sağlar.** Bir VM oluşturduğunuzda, konuk işletim sistemi Tanılama'yı etkinleştirmek için ayarlar ekranında fırsatına sahip olursunuz. Tanılama verilerinin toplanmasını etkinleştirdiğinizde [Linux için IaaSDiagnostics uzantısı](../articles/virtual-machines/linux/diagnostic-extension.md) veya [IaaSDiagnostics uzantısı Windows](../articles/virtual-machines/windows/ps-extensions-diagnostics.md) ek toplamanızı sağlayan VM eklenir disk, CPU ve bellek verileri.

    Toplanan Tanılama verileri kullanarak, sanal makineleriniz için otomatik ölçeklendirme yapılandırabilirsiniz. Günlükleri, performans oldukça doğru olmadığı durumlarda size bildirmek için uyarılar ayarlayın ve verileri depolamak için de yapılandırabilirsiniz.

## <a name="alerts"></a>Uyarılar

Oluşturabileceğiniz [uyarılar](../articles/azure-monitor/platform/alerts-overview.md) belirli performans ölçümlerine bağlı. Ortalama CPU kullanımı belirli bir eşiği aştığında veya kullanılabilir boş disk alanı belirli bir miktarın altına düşünceye, hakkında uyarı almak sorunları örnekleridir. Uyarılar yapılandırılabilir [Azure portalında](../articles/azure-monitor/platform/alerts-classic-portal.md)kullanarak [Azure PowerShell](../articles/azure-monitor/platform/alerts-classic-portal.md#with-powershell), veya [Azure CLI](../articles/azure-monitor/platform/alerts-classic-portal.md#with-azure-cli).

## <a name="azure-service-health"></a>Azure Hizmet Durumu

[Azure hizmet durumu](../articles/service-health/service-health-overview.md) Azure hizmetlerindeki sorunlardan etkilendiğiniz, ve yardımcı olur, hazırlama için yaklaşan planlı bakım için kişiselleştirilmiş rehberlik ve destek sağlar. Azure hizmet durumu sizi ve takımlarınızı hedefli ve esnek bildirimleri kullanarak sizi uyarır.

## <a name="azure-resource-health"></a>Azure Kaynak Durumu

[Azure kaynak durumu](../articles/service-health/resource-health-overview.md) tanılamanıza ve bir Azure sorunu kaynaklarınızı etkilediğinde destek almanıza yardımcı olur. Kaynaklarınızın güncel ve geçmiş durumu hakkında bilgiler sağlar ve sorunları azaltmaya yardımcı olur. Kaynak Durumu, Azure hizmet sorunları ile ilgili yardıma ihtiyacınız olduğunda teknik destek sağlar.

## <a name="azure-activity-log"></a>Azure etkinlik günlüğü

[Azure etkinlik günlüğü](../articles/azure-monitor/platform/activity-logs-overview.md) oluşan Azure'da abonelik düzeyindeki olayların sağlayan bir abonelik günlüktür. Günlük verileri, hizmet durumu olayları güncelleştirmelerinin Azure Resource Manager işletimsel verileri içerir. Sanal Makineniz için günlüğü görüntülemek için Azure portalında etkinlik günlüğü tıklayabilirsiniz.

Etkinlik günlüğü ile yapabileceğiniz çok şey bazıları şunlardır:

- Oluşturma bir [bir etkinlik günlüğü olayında uyarı](../articles/azure-monitor/platform/activity-logs-overview.md).
- [Olay Hub'ına Stream](../articles/azure-monitor/platform/activity-logs-stream-event-hubs.md) alımı üçüncü taraf hizmeti veya Power BI gibi özel bir analiz çözümü için.
- Power BI kullanarak Analiz [Power BI içerik paketi](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
- [Bir depolama hesabına kaydedin](../articles/azure-monitor/platform/archive-activity-log.md) arşivleme veya el ile İnceleme. Günlük profilini kullanarak elde tutma süresi (gün cinsinden) belirtebilirsiniz.

Etkinlik günlüğü verileri kullanarak da erişebilirsiniz [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights/), [Azure CLI](https://docs.microsoft.com/cli/azure/monitor), veya [İzleyici REST API'leri](https://docs.microsoft.com/rest/api/monitor/).

[Azure tanılama günlükleri](../articles/azure-monitor/platform/diagnostic-logs-overview.md) olan kendi işlemiyle ilgili zengin, sık sık veri sağlayan tarafından sanal makinenizin günlüklerdir. Tanılama günlükleri, VM içinde gerçekleştirilen işlemler hakkında öngörü sağlayan etkinlik günlüğünde farklılık gösterir.

Tanılama günlükleri ile yapabileceklerinizden bazıları şunlardır:

- [Bir depolama hesabına kaydetmekte](../articles/azure-monitor/platform/archive-diagnostic-logs.md) denetim veya el ile İnceleme. Kaynak tanılama ayarlarını kullanarak elde tutma süresi (gün cinsinden) belirtebilirsiniz.
- [Event Hubs için bunları Stream](../articles/azure-monitor/platform/diagnostic-logs-stream-event-hubs.md) alımı üçüncü taraf hizmeti veya Power BI gibi özel bir analiz çözümü için.
- Bunları analiz [Log Analytics](../articles/log-analytics/log-analytics-azure-storage.md).

## <a name="advanced-monitoring"></a>Gelişmiş izleme

- [Azure İzleyici](../articles/azure-monitor/overview.md) bulut izler ve şirket içi Ortamlarınızdaki kullanılabilirliği ve performansı korumak için bir hizmettir. Bu bulut alınan telemetri üzerinde çalışan toplama ve analiz için kapsamlı bir çözüm sunar ve şirket içi Ortamlarınızdaki. Uygulamalarınızın performansını anlamanıza ve uygulamalarla bağlı oldukları kaynakları etkileyen sorunları önceden tespit etmenize yardımcı olur. Bir uzantı yükleyebileceğiniz bir [Linux VM](../articles/virtual-machines/linux/extensions-oms.md) veya [Windows VM](../articles/virtual-machines/windows/extensions-oms.md) günlük verilerini toplamak ve Log Analytics çalışma alanında depolamak için Log Analytics aracısını yükler.

    Windows ve Linux Vm'leri için Log Analytics aracısını yükleyerek günlüklerinin toplanması için önerilen yöntem olduğu. Bir VM'ye Log Analytics aracısını yüklemek için en kolay yolu aracılığıyladır [Log Analytics VM uzantısını](../articles/log-analytics/log-analytics-azure-vm-extension.md). Uzantıyı kullanmak yükleme işlemini kolaylaştırır ve aracıyı belirttiğiniz Log Analytics çalışma alanına veri göndermek üzere otomatik olarak yapılandırır. Ayrıca aracı otomatik olarak yükseltilerek her zaman en yeni özellik ve düzeltmelere sahip olmanız sağlanır.

- [Ağ İzleyicisi](../articles/network-watcher/network-watcher-monitoring-overview.md) olduğu ağ ile ilgili olarak, VM'yi ve ilişkili kaynakları izlemenize olanak sağlar. Ağ İzleyicisi Aracısı uzantısı yükleyebileceğiniz bir [Linux VM](../articles/virtual-machines/linux/extensions-nwa.md) veya [Windows VM](../articles/virtual-machines/windows/extensions-nwa.md).

- [VM'ler için Azure İzleyici](../articles/azure-monitor/insights/vminsights-overview.md) Azure sanal makinelerinizi (VM) Windows ve Linux Vm'leri, farklı işlemler ve diğer kaynakları ve dış birbirine bağımlılıkları da dahil olmak üzere, sistem durumu ve performansı çözümleyerek ölçekli olarak izler. işler. 

## <a name="next-steps"></a>Sonraki adımlar
- İzleyeceğiniz adımlarda size yol [Azure PowerShell ile Windows sanal makine izleme](../articles/virtual-machines/windows/tutorial-monitoring.md) veya [Azure CLI ile Linux sanal makinesi izleme](../articles/virtual-machines/linux/tutorial-monitoring.md).
- Geçici olarak en iyi uygulamalar hakkında daha fazla bilgi [izleme ve tanılama](https://docs.microsoft.com/azure/architecture/best-practices/monitoring).
