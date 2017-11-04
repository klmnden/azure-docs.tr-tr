Verileri günlüğe ve Vm'lerinizi toplama, görüntüleme ve tanılama çözümlemenin izlemek için çok sayıda fırsatlar yararlanabilir. Basit yapmak için [izleme](../articles/monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) , VM Azure portalında VM için genel bakış ekranına kullanabilirsiniz. Kullanabileceğiniz [uzantıları](../articles/virtual-machines/windows/extensions-features.md) Vm'leriniz ek ölçüm verilerini toplamak için tanılama yapılandırmak için. Gibi daha gelişmiş izleme seçeneklerini kullanabilir [Application Insights](../articles/application-insights/app-insights-overview.md) ve [günlük analizi](../articles/log-analytics/log-analytics-overview.md).

## <a name="diagnostics-and-metrics"></a>Tanılama ve ölçümleri 

Ayarlama ve koleksiyonunu izlemek [tanılama verilerini](https://docs.microsoft.com/cli/azure/vm/diagnostics) kullanarak [ölçümleri](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md) Azure portalı, Azure CLI, Azure PowerShell ve programlama uygulama programlama arabirimleri (API). Örneğin, şunları yapabilirsiniz:

- **Temel ölçümleri, VM için inceleyin.** Azure portalına genel bakış ekranda gösterilen temel ölçümleri CPU kullanımı, ağ kullanımı, toplam disk bayt ve saniye başına disk işlemleri içerir.

- **Önyükleme tanılaması toplanmasını etkinleştirmek ve Azure portalını kullanarak görüntüleyin.** Azure veya hatta platform görüntülerden birini önyükleme için kendi görüntünüzü duruma getirilirken bir VM önyüklenebilir olmayan bir duruma neden alır birçok nedeni olabilir. Tıklayarak bir VM oluştururken önyükleme tanılaması kolayca etkinleştirebilirsiniz **etkin** için önyükleme tanılaması ayarları ekranının izleme bölümünde.

    Sanal makineleri önyükleme gibi önyükleme Tanılama Aracı önyükleme çıkış yakalar ve Azure depolama alanında depolar. Bu veriler, VM önyükleme sorunlarını gidermek için kullanılabilir. Komut satırı araçları'ndan bir VM oluştururken önyükleme tanılaması otomatik olarak etkin değildir. Önyükleme tanılaması etkinleştirmeden önce bir depolama hesabı önyükleme günlüklerini depolamak için oluşturulması gerekir. Azure portalında önyükleme tanılaması etkinleştirirseniz, bir depolama hesabı sizin için otomatik olarak oluşturulur.

    VM oluşturulduğunda önyükleme tanılaması etkinleştirirseniz alamadık, her zaman daha sonra kullanarak etkinleştirebilmeniz için [Azure CLI](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmbootdiagnostics), veya bir [Azure Resource Manager şablonu](../articles/virtual-machines/windows/extensions-diagnostics-template.md).

- **Konuk işletim sistemi Tanılama verileri koleksiyonu etkinleştirin.** Bir VM oluşturduğunuzda, konuk işletim sistemi Tanılama'yı etkinleştirmek için ayarlar ekranında olanağına sahip olursunuz. Tanılama veri koleksiyonunu etkinleştirdiğinizde [IaaSDiagnostics uzantısı Linux için](../articles/virtual-machines/linux/diagnostic-extension.md) veya [Windows IaaSDiagnostics uzantısı](../articles/virtual-machines/windows/ps-extensions-diagnostics.md) ek toplamanızı sağlar VM eklenir disk, CPU ve bellek verileri.

    Toplanan Tanılama verileri kullanarak VM'ler için otomatik ölçeklendirmeyi yapılandırabilirsiniz. Veri depolamak ve performansı oldukça sağ değilken, size bildirmek için uyarıları ayarlamak için günlükleri de yapılandırabilirsiniz.

## <a name="alerts"></a>Uyarılar

Oluşturabileceğiniz [uyarıları](../articles/monitoring-and-diagnostics/monitoring-overview-alerts.md) özel performans ölçümleri temelinde. Ortalama CPU kullanımını belirli bir Eşiği aşan veya kullanılabilir boş disk alanı belirli bir miktar düştüğünde, hakkında uyarı almak sorunları örneklerindendir. Uyarılar, içinde yapılandırılabilir [Azure portal](../articles/monitoring-and-diagnostics/insights-alerts-portal.md)kullanarak [Azure PowerShell](../articles/monitoring-and-diagnostics/insights-alerts-powershell.md), veya [Azure CLI](../articles/monitoring-and-diagnostics/insights-alerts-command-line-interface.md).

## <a name="azure-service-health"></a>Azure Hizmet Durumu

[Azure hizmet durumu](../articles/service-health/service-health-overview.md) sorunlar Azure hizmetlerinde etkiler ve yardımcı olur, hazırlamak için gelecek planlı bakım kişiselleştirilmiş rehberlik ve destek sağlar. Azure hizmet durumu ve hedeflenen ve esnektir bildirimlerini kullanarak ekipleriniz uyarır.

## <a name="azure-resource-health"></a>Azure Kaynak Durumu

[Azure kaynak durumu](../articles/service-health/resource-health-overview.md) tanılamanıza ve kaynaklarınızı Azure sorun etkilediğini destek edilirken yardımcı olur. Kaynaklarınızın güncel ve geçmiş durumu hakkında bilgiler sağlar ve sorunları azaltmaya yardımcı olur. Kaynak Durumu, Azure hizmet sorunları ile ilgili yardıma ihtiyacınız olduğunda teknik destek sağlar.

## <a name="logs"></a>Günlükler

[Azure etkinlik günlüğü](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md) , Azure'da oluşan abonelik düzeyinde olaylar hakkında bilgi sağlayan bir abonelik günlüktür. Günlük verileri, Azure Resource Manager işlemsel veri hizmeti sistem durumu olayları güncelleştirmeleri için bir aralığı içerir. VM için günlüğünü görüntülemek için Azure portalında etkinlik günlüğü tıklatın.

Etkinlik günlüğü ile yapabileceği şeylerden bazıları şunlardır:

- Oluşturma bir [etkinlik günlüğü olay uyarı](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md).
- [Olay Hub'ına akış](../articles/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md) bir üçüncü taraf hizmeti veya Powerbı gibi özel analiz çözümü tarafından alımı için.
- Powerbı kullanarak Analiz [Power BI içerik paketi](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
- [Bir depolama hesabına Kaydet](../articles/monitoring-and-diagnostics/monitoring-archive-activity-log.md) arşivleme veya el ile İnceleme için. Günlük profilini kullanarak bekletme süresi (gün cinsinden) belirtebilirsiniz.

Etkinlik günlüğü verilerini kullanarak da erişebilirsiniz [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights/), [Azure CLI](https://docs.microsoft.com/cli/azure/monitor), veya [İzleyici REST API'leri](https://docs.microsoft.com/rest/api/monitor/).

[Azure tanılama günlüklerini](../articles/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) olan, işlemi hakkında zengin, sık sık veri sağlayan VM tarafından gösterilen günlükleri. Tanılama günlükleri, VM içinden gerçekleştirilen işlemler hakkında görüş sağlayarak etkinlik günlüğünden farklılık gösterir.

Tanılama günlükleri ile yapabileceği şeylerden bazıları şunlardır:

- [Bir depolama hesabına kaydetmekte](../articles/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) denetim veya el ile İnceleme için. Kaynak tanılama ayarlarını kullanarak bekletme süresi (gün cinsinden) belirtebilirsiniz.
- [Event Hubs'a akış](../articles/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) bir üçüncü taraf hizmeti veya Powerbı gibi özel analiz çözümü tarafından alımı için.
- Bunları ile analiz [OMS günlük analizi](../articles/log-analytics/log-analytics-azure-storage.md).

## <a name="advanced-monitoring"></a>Gelişmiş izleme

- [Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/) bulut izleme, uyarma ve uyarı düzeltme özellikleri sağlar ve şirket içi varlıklar. Uzantı yükleyebileceğiniz bir [Linux VM](../articles/virtual-machines/linux/extensions-oms.md) veya [Windows VM](../articles/virtual-machines/windows/extensions-oms.md) , OMS Aracısı'nı yükler ve VM olan bir OMS çalışma kaydeder.

- [Günlük analizi](../articles/log-analytics/log-analytics-overview.md) bir bulut izler ve şirket içi kendi kullanılabilirliğini ve performansını korumak için ortamları OMS hizmetidir. Birden fazla kaynak arasında analiz sağlamak üzere bulut ve şirket içi ortamlarınızdaki kaynaklar ile diğer izleme araçları tarafından oluşturulan verileri toplar.

    Windows ve Linux VM'ler için günlükleri ve ölçümleri toplamak için önerilen günlük analizi Aracısı'nı yükleyerek yöntemdir. Bir VM günlük analizi Aracısı'nı yüklemek için kolay yolunu [Log Analytics VM uzantısı](../articles/log-analytics/log-analytics-azure-vm-extension.md). Uzantısını kullanarak yükleme işlemini basitleştirir ve belirttiğiniz için günlük analizi çalışma alanına veri göndermek için aracı otomatik olarak yapılandırır. Aracı ayrıca otomatik olarak en son özellikleri ve düzeltmeleri sahip olduktan yükseltilir.

- [Ağ İzleyicisi](../articles/network-watcher/network-watcher-monitoring-overview.md) bulundukları ağa kullanılabildiğinden, VM ve onun ilişkili kaynakları izlemenizi sağlar. Ağ İzleyicisi Aracısı uzantısı yükleyebileceğiniz bir [Linux VM](../articles/virtual-machines/linux/extensions-nwa.md) veya [Windows VM](../articles/virtual-machines/windows/extensions-nwa.md).

## <a name="next-steps"></a>Sonraki adımlar
- İzleyeceğiniz adımlarda size yol [Azure PowerShell ile Windows sanal makine izlemek](../articles/virtual-machines/windows/tutorial-monitoring.md) veya [Azure CLI ile Linux sanal makine izlemek](../articles/virtual-machines/linux/tutorial-monitoring.md).
- En iyi uygulamalar hakkında daha fazla bilgi [izleme ve tanılama](https://docs.microsoft.com/azure/architecture/best-practices/monitoring).
