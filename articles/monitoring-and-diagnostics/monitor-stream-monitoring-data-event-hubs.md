---
title: "Akış Azure Event hubs'a veri izleme | Microsoft Docs"
description: "Tüm Azure izleme verilerinizi bir iş ortağı SIEM verisine veya Analiz aracı almak için bir olay hub'ına akış öğrenin."
author: johnkemnetz
manager: robb
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2017
ms.author: johnkem
ms.openlocfilehash: 59f0cba66a5d8d2a528700861efff86967c1c748
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
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

Başlamadan önce şunları gerçekleştirmeniz [bir olay hub'ları ad alanı ve olay hub'ı oluşturma](../event-hubs/event-hubs-create.md). Bu ad alanı ve olay hub'ı İzleme verilerinizin tümünü hedefi değil.

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

[Linux Azure Tanılama Aracı](../virtual-machines/linux/diagnostic-extension.md) göndermek için kullanılan bir Linux makineden bir olay hub'ına veri izleme. Bu, LAD havuzunda olarak olay hub'ı ekleyerek yapılandırma dosyası korunan ayarları JSON yapabilirsiniz. [Bu makalede Linux Azure tanılama aracınızı olay hub'ı havuzu ekleme hakkında daha fazla bilgi için bkz:](../virtual-machines/linux/diagnostic-extension.md#protected-settings).

> [!NOTE]
> Konuk işletim sistemi izleme verileri portalda bir olay hub'ına akış olarak ayarlanamaz. Bunun yerine, el ile yapılandırma dosyasını düzenlemeniz gerekir.

### <a name="stream-windows-data-to-an-event-hub"></a>Bir olay hub'ına Windows veri akışı

[Windows Azure Tanılama Aracı](./azure-diagnostics.md) göndermek için kullanılan bir Windows makineden bir olay hub'ına veri izleme. Bu, privateConfig bölümünüzün WAD yapılandırma dosyasının bir havuz olarak olay hub'ı ekleyerek yapabilirsiniz. [Bu makalede Windows Azure tanılama aracınızı olay hub'ı havuzu ekleme hakkında daha fazla bilgi için bkz:](./azure-diagnostics-streaming-event-hubs.md).

> [!NOTE]
> Konuk işletim sistemi izleme verileri portalda bir olay hub'ına akış olarak ayarlanamaz. Bunun yerine, el ile yapılandırma dosyasını düzenlemeniz gerekir.

## <a name="how-do-i-set-up-application-monitoring-data-to-be-streamed-to-event-hub"></a>Olay hub'ına akışını uygulama izleme verilerini nasıl ayarlayabilirim?

Hiç şekilde yönlendirme uygulama verileri azure'da bir olay hub'ına izleme genel amaçlı bir çözüme kodunuzu bir SDK ile izlenmiş olan uygulama veri izleme gerektirir. Ancak, [Azure Application Insights](../application-insights/app-insights-overview.md) Azure uygulama düzeyi verilerini toplamak için kullanılan bir hizmettir. Application Insights kullanıyorsanız, aşağıdakileri yaparak bir olay hub'ına izleme veri akışı:

1. [Sürekli verme ayarlamak](../application-insights/app-insights-export-telemetry.md) bir depolama hesabı için Application Insights veri.

2. Zamanlayıcı tarafından tetiklenen mantığı uygulamasını ayarladığınızda, [blob depolama veri çeker](../connectors/connectors-create-api-azureblobstorage.md#use-an-action) ve [ileti olarak event hub'ına iter](../connectors/connectors-create-api-azure-event-hubs.md#send-events-to-your-event-hub-from-your-logic-app).

## <a name="what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub"></a>My event hub'ına gönderilen izleme verilerle ne yapabilirim?

Bir olay hub'ına Azure İzleyicisi ile İzleme verilerinizin yönlendirme, iş ortağı SIEM ve izleme araçları ile kolayca tümleştirmenize olanak sağlar. Çoğu araç olay hub bağlantı dizesine ve olay hub'ından veri okumak için Azure aboneliğiniz için belirli izinler gerektirir. Azure İzleyici tümleştirme araçlarıyla kapsamlı olmayan bir listesi aşağıda verilmiştir:

* **IBM QRadar** -QRadar Bağlayıcısı olay hub'ları için şu anda beta. Yakında IBM düzeltme merkezi veya otomatik olarak QRadar'ın otomatik güncelleştirme işlemi aracılığıyla el ile yüklemek kullanıma sunulacaktır.
* **Splunk** - [Splunk için Azure İzleyici eklentisi](https://splunkbase.splunk.com/app/3534/) Splunkbase ve açık kaynaklı proje bulunur. [Belgesidir burada](https://github.com/Microsoft/AzureMonitorAddonForSplunk/wiki/Azure-Monitor-Addon-For-Splunk).
* **SumoLogic** -SumoLogic verileri event hub'ındaki kullanmak üzere ayarlamak için yönergeler [kullanılabilir burada](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub)

## <a name="next-steps"></a>Sonraki Adımlar
* [Arşiv depolama hesabı etkinlik günlüğü](monitoring-archive-activity-log.md)
* [Azure etkinlik günlüğü bakış okuma](monitoring-overview-activity-logs.md)
* [Etkinlik günlüğü olaya dayanarak bir uyarı ayarlama](insights-auditlog-to-webhook-email.md)

