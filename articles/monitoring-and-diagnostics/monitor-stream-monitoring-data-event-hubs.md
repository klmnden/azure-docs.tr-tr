---
title: Akış Azure Event hubs'a veri izleme | Microsoft Docs
description: Tüm Azure izleme verilerinizi bir iş ortağı SIEM verisine veya Analiz aracı almak için bir olay hub'ına akış öğrenin.
author: johnkemnetz
manager: robb
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/05/2018
ms.author: johnkem
ms.openlocfilehash: 35cdd157469556c071b03a0f25184df141057554
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34639070"
---
# <a name="stream-azure-monitoring-data-to-an-event-hub-for-consumption-by-an-external-tool"></a>Bir dış aracı tarafından izleme verileri tüketim için bir olay hub'ına akış Azure

Azure İzleyicisi, tüm Azure ortamınıza verilerden izleme, iş ortağı SIEM kolayca ayarlamanıza olanak sağlayarak ve bu verileri kullanmak üzere izleme araçları erişim almak için tek bir ardışık düzen sağlar. Bu makalede, bir tek olay hub'ları ad alanı veya olay hub'ına, burada bir dış aracı tarafından toplanabilecek gönderilmek üzere veri farklı katmandan Azure ortamınızdan ayarı aracılığıyla anlatılmaktadır.

## <a name="what-data-can-i-send-into-an-event-hub"></a>Hangi verilerin bir event hub'ına gönderebilirim? 

Azure ortamınızda 'izleme verilerinin birkaç katmanları' vardır ve her katmandan verilere yöntemi biraz değişir. Genellikle, bu katmanları olarak açıklanabilir:

- **Uygulama izleme verilerini:** yazmıştır ve Azure üzerinde çalışan kod işlevselliği ve performansı hakkındaki verileri. Performans izlemeleri, uygulama günlüklerini ve kullanıcı telemetri izleme verileri uygulama örneklerindendir. Uygulama izleme verileri aşağıdaki yollardan biriyle genellikle toplanır:
  - Kodunuzu bir SDK'sı gibi izleme tarafından [Application Insights SDK'sı](../application-insights/app-insights-overview.md).
  - Yeni uygulama makinede oturum için dinleyen bir izleme Aracısı çalıştırarak, uygulamanızın gibi çalışan [Windows Azure Tanılama Aracı](./azure-diagnostics.md) veya [Linux Azure Tanılama Aracı](../virtual-machines/linux/diagnostic-extension.md).
- **Konuk işletim sistemi izleme verilerini:** uygulamanızı çalıştıran işletim sistemi hakkındaki verileri. Konuk işletim sistemi izleme verilerini örnekleri Linux syslog veya Windows sistem olayları olacaktır. Bu tür veri toplamak için bir aracı gibi yüklemeniz gerekir [Windows Azure Tanılama Aracı](./azure-diagnostics.md) veya [Linux Azure Tanılama Aracı](../virtual-machines/linux/diagnostic-extension.md).
- **İzleme verilerini azure kaynağı:** bir Azure kaynağı işlemi hakkındaki verileri. Sanal makineler gibi bazı Azure kaynak türleri için var. bir konuk işletim sistemi ve uygulamaları içinde Azure bu hizmeti izlemek için Ağ güvenlik grupları gibi diğer Azure kaynakları için (olduğundan hiçbir konuk işletim sistemi veya uygulama bu kaynakları çalıştıran) izleme verileri kaynak yüksek katman veri kullanılabilir değil. Bu verileri kullanarak toplanabilir [kaynak tanılama ayarlarını](./monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).
- **İzleme verilerini azure platformu:** durumu ve Azure işlem hakkında veri yanı sıra, işlem ve bir Azure aboneliği veya Kiracı Yönetimi hakkında bilgileri kendisi. [Etkinlik günlüğü](./monitoring-overview-activity-logs.md), hizmet Sistem Durumu verilerini ve Active Directory dahil olmak üzere denetimleri izleme verilerini platform olarak gösterilebilir. Tanılama ayarları kullanılarak bu veriler toplanabilir.

Herhangi bir katmanı verilerden bir event hub'ına, burada da bir iş ortağı aracına çekebilir gönderilebilir. Sonraki bölümlerde, her katman bir olay hub'ına akışı verileri nasıl yapılandırabileceğiniz açıklanmaktadır. Adımlarda varlıklar izlenmesi için bu katmana sahip olduğunuz varsayılır.

## <a name="set-up-an-event-hubs-namespace"></a>Bir olay hub'ları ad alanı ayarlama

Başlamadan önce şunları gerçekleştirmeniz [bir olay hub'ları ad alanı ve olay hub'ı oluşturma](../event-hubs/event-hubs-create.md). Bu ad alanı ve olay hub'ı İzleme verilerinizin tümünü hedefi değil. Bir olay hub'ları ad alanı aynı erişim ilkesi paylaşan olay hub'ları mantıksal bir gruplandırması, çok gibi bir depolama hesabı bu depolama hesabı içinde tek tek bloblar sahiptir. Lütfen olay hub'ları ad alanı ve oluşturduğunuz olay hub'ları hakkında birkaç ayrıntılarını not edin:
* Bir olay hub'ı standart ad kullanmanızı öneririz.
* Genellikle, yalnızca bir işleme birimi gereklidir. Günlük kullanım arttıkça ölçeği ihtiyacınız varsa, her zaman elle daha sonra ad alanı için işleme birimleri sayısını artırmak veya Otomatik enflasyon etkinleştirin.
* İşleme birimlerinin sayısı, olay hub'larınız işleme ölçeğini artırmanıza olanak sağlar. Bölüm sayısı tüketim arasında çok sayıda tüketiciye paralel hale sağlar. Tek bir bölüm 20MBps kadar veya yaklaşık yapabilirsiniz saniyede 20.000 ileti. Verileri tüketme aracın bağlı olarak bu olabilir veya birden fazla bölümdeki varlığa tüketen desteklemeyebilir. Ayarlamak için bölüm sayısı hakkında emin değilseniz, dört bölüm ile başlamanızı öneririz.
* İleti bekletme 7 gün için olay hub'ınızı ayarlamanızı öneririz. Kullanıcı aracınız birden fazla bir gün için devre dışı kalırsa, bu aracın kaldığı yerden yukarı seçebilirsiniz sağlar (olaylar için en fazla 7 gün).
* Olay hub'ınız için varsayılan bir tüketici grubu kullanmanızı öneririz. Aynı olay hub'ı aynı verilerden tüketen iki farklı araçlar sahip olmayı planladığınız sürece diğer tüketici grupları oluşturun veya ayrı bir tüketici grubundaki kullanmayı gerek yoktur.
* Azure etkinlik günlüğü için bir olay hub'ları ad alanını seçin ve bir event hub 'Öngörüler-günlükleri-operationallogs.' olarak adlandırılan bu ad alanı içindeki Azure İzleyici oluşturur Diğer günlük türleri için mevcut olay hub'ı (aynı Öngörüler günlükleri operationallogs olay hub'ı yeniden olanak) ya da seçebilirsiniz veya Azure günlük Kategori başına olay hub'ı oluşturma İzleyicisi.
* Genellikle, bağlantı noktası 5671 ve 5672 verileri olay hub'ı kullanma makinede açılması gerekir.

Ayrıca bkz [Azure olay hub'ları ile ilgili SSS](../event-hubs/event-hubs-faq.md).

## <a name="how-do-i-set-up-azure-platform-monitoring-data-to-be-streamed-to-an-event-hub"></a>Nasıl izleme verilerini Azure platformu olay hub'ına akışını ayarlarım?

İzleme verilerini azure platformu iki ana kaynaktan gelir:
1. [Azure etkinlik günlüğü](./monitoring-overview-activity-logs.md), oluşturma içerir, güncelleştirme ve silme işlemleri Kaynak Yöneticisi'nden değişiklikleri [Azure hizmet durumu](../service-health/service-health-overview.md) aboneliğinizdekaynaklaraetkisi[kaynak durumu](../service-health/resource-health-overview.md) durumu geçişleri ve diğer çeşitli abonelik düzeyi olay türlerini. [Bu makalede Azure etkinlik günlüğünde görünür olayların tüm kategorileri ayrıntıları](./monitoring-activity-log-schema.md).
2. [Azure Active Directory raporlama](../active-directory/active-directory-reporting-azure-portal.md), oturum açma etkinliği ve denetim izi belirli bir kiracı içinde yapılan değişikliklerin geçmişini içerir. Henüz bir event hub içine Azure Active Directory veri akışı için mümkün değildir.

### <a name="stream-azure-activity-log-data-into-an-event-hub"></a>Bir event hub içine Azure etkinlik günlüğü veri akışı

Azure etkinlik günlüğü bir olay hub'ları ad alanına veri göndermek için bir günlük profili, aboneliğinizde ayarlayın. [Bu kılavuzu uygulayın](./monitoring-stream-activity-logs-event-hubs.md) aboneliğinizi günlük profilinde ayarlamak için. İzlemek istediğiniz abonelik başına sonra bunu yapabilirsiniz.

> [!TIP]
> Bir günlük profil şu anda yalnızca bir event hub adı 'Öngörüler-işletimsel-günlükleriyle.' oluşturan bir olay hub'ları ad alanı seçmenize olanak sağlar Henüz bir günlük profilinde kendi olay hub'ı adı belirlemek mümkün değil.

## <a name="how-do-i-set-up-azure-resource-monitoring-data-to-be-streamed-to-an-event-hub"></a>Nasıl izleme verilerini Azure kaynağını olay hub'ına akışını ayarlarım?

Azure kaynaklarını iki tür izleme verileri göstermiyor:
1. [Kaynağın tanılama günlükleri](./monitoring-overview-of-diagnostic-logs.md)
2. [Ölçümler](monitoring-overview-metrics.md)

Her iki tür veri kaynağın tanılama ayarını kullanarak olay hub'ına gönderilir. [Bu kılavuzu uygulayın](./monitoring-stream-diagnostic-logs-to-event-hubs.md) kaynağın tanılama ayarını belirli bir kaynak üzerinde ayarlamak için. Günlükleri toplamak istediğiniz her kaynaktan üzerinde bir kaynağın tanılama ayarını yapın.

> [!TIP]
> Azure ilke belirli bir kapsamdaki her kaynak her zaman bir tanılama ayarıyla kurulduğundan emin olmak için kullanabileceğiniz [DeployIfNotExists etkisi ilke kuralı kullanarak](../azure-policy/policy-definition.md#policy-rule). Bugün DeployIfNotExists yalnızca yerleşik ilkeleri desteklenir.

## <a name="how-do-i-set-up-guest-os-monitoring-data-to-be-streamed-to-an-event-hub"></a>Nasıl konuk işletim sistemi izleme verileri olay hub'ına akışını ayarlarım?

Konuk işletim sistemi izleme verilerini bir event hub'ına göndermek için bir aracı yüklemeniz gerekir. Windows veya Linux için event hub'verileri yapılandırma dosyasındaki gönderilmesi gerektiğini ve VM'de çalıştırılan aracısı için yapılandırma dosyasının geçirin yanı sıra, olay hub'ı gönderilmesini istediğiniz verileri belirtin.

### <a name="stream-linux-data-to-an-event-hub"></a>Bir olay hub'ına Linux veri akışı

[Linux Azure Tanılama Aracı](../virtual-machines/extensions/diagnostics-linux.md) göndermek için kullanılan bir Linux makineden bir olay hub'ına veri izleme. Bu, LAD havuzunda olarak olay hub'ı ekleyerek yapılandırma dosyası korunan ayarları JSON yapabilirsiniz. [Bu makalede Linux Azure tanılama aracınızı olay hub'ı havuzu ekleme hakkında daha fazla bilgi için bkz:](../virtual-machines/extensions/diagnostics-linux.md#protected-settings).

> [!NOTE]
> Konuk işletim sistemi izleme verileri portalda bir olay hub'ına akış olarak ayarlanamaz. Bunun yerine, el ile yapılandırma dosyasını düzenlemeniz gerekir.

### <a name="stream-windows-data-to-an-event-hub"></a>Bir olay hub'ına Windows veri akışı

[Windows Azure Tanılama Aracı](./azure-diagnostics.md) göndermek için kullanılan bir Windows makineden bir olay hub'ına veri izleme. Bu, privateConfig bölümünüzün WAD yapılandırma dosyasının bir havuz olarak olay hub'ı ekleyerek yapabilirsiniz. [Bu makalede Windows Azure tanılama aracınızı olay hub'ı havuzu ekleme hakkında daha fazla bilgi için bkz:](./azure-diagnostics-streaming-event-hubs.md).

> [!NOTE]
> Konuk işletim sistemi izleme verileri portalda bir olay hub'ına akış olarak ayarlanamaz. Bunun yerine, el ile yapılandırma dosyasını düzenlemeniz gerekir.

## <a name="how-do-i-set-up-application-monitoring-data-to-be-streamed-to-event-hub"></a>Olay hub'ına akışını uygulama izleme verilerini nasıl ayarlayabilirim?

Hiç şekilde yönlendirme uygulama verileri azure'da bir olay hub'ına izleme genel amaçlı bir çözüme kodunuzu bir SDK ile izlenmiş olan uygulama veri izleme gerektirir. Ancak, [Azure Application Insights](../application-insights/app-insights-overview.md) Azure uygulama düzeyi verilerini toplamak için kullanılan bir hizmettir. Application Insights kullanıyorsanız, aşağıdakileri yaparak bir olay hub'ına izleme veri akışı:

1. [Sürekli verme ayarlamak](../application-insights/app-insights-export-telemetry.md) bir depolama hesabı için Application Insights veri.

2. Zamanlayıcı tarafından tetiklenen mantığı uygulamasını ayarladığınızda, [blob depolama veri çeker](../connectors/connectors-create-api-azureblobstorage.md#add-action) ve [ileti olarak event hub'ına iter](../connectors/connectors-create-api-azure-event-hubs.md#add-action).

## <a name="what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub"></a>My event hub'ına gönderilen izleme verilerle ne yapabilirim?

Bir olay hub'ına Azure İzleyicisi ile İzleme verilerinizin yönlendirme, iş ortağı SIEM ve izleme araçları ile kolayca tümleştirmenize olanak sağlar. Çoğu araç olay hub bağlantı dizesine ve olay hub'ından veri okumak için Azure aboneliğiniz için belirli izinler gerektirir. Azure İzleyici tümleştirme araçlarıyla kapsamlı olmayan bir listesi aşağıda verilmiştir:

* **IBM QRadar** -Merkezi'nden Microsoft Azure DSM ve Microsoft Azure olay hub'ı protokolü kullanılabilir [IBM Destek Web sitesi](http://www.ibm.com/support). Yapabilecekleriniz [Azure ile tümleştirme burada hakkında daha fazla bilgi](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0).
* **Splunk** -Splunk kurulumunuza bağlı olarak, iki yaklaşım vardır:
    1. [Azure İzleyici eklenti Splunk için](https://splunkbase.splunk.com/app/3534/) Splunkbase ve açık kaynaklı proje bulunur. [Belgesidir burada](https://github.com/Microsoft/AzureMonitorAddonForSplunk/wiki/Azure-Monitor-Addon-For-Splunk).
    2. Bir eklenti (örn., Splunk örneğinizin yükleyemiyorsanız bir ara sunucu kullanıldığında veya Splunk bulutta çalışan), Splunk HTTP Olay Toplayıcısı kullanarak bu olayları iletebilir [yeni iletiler event hub'ındaki tarafından tetiklenen bu işlevin](https://github.com/sebastus/AzureFunctionForSplunkVS).
* **SumoLogic** -SumoLogic verileri event hub'ındaki kullanmak üzere ayarlamak için yönergeler [kullanılabilir burada](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub)

## <a name="next-steps"></a>Sonraki Adımlar
* [Arşiv depolama hesabı etkinlik günlüğü](monitoring-archive-activity-log.md)
* [Azure etkinlik günlüğü bakış okuma](monitoring-overview-activity-logs.md)
* [Etkinlik günlüğü olaya dayanarak bir uyarı ayarlama](insights-auditlog-to-webhook-email.md)

