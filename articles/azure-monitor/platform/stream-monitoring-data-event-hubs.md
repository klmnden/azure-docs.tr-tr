---
title: Stream Azure Event hubs'a veri izleme
description: Azure izleme verilerinizi analiz aracı ve bir iş ortağı SIEM verilerini almak için bir olay hub'ına akışı yapmayı öğrenin.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: johnkem
ms.subservice: ''
ms.openlocfilehash: ab439eb77113c53ab046256dd8d448a18b63f887
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58850078"
---
# <a name="stream-azure-monitoring-data-to-an-event-hub-for-consumption-by-an-external-tool"></a>Stream Azure harici bir aracı tarafından veri tüketimi için olay hub'ına izleme

Bu makalede, Azure ortamınızdan veri farklı katmandan ayarlama burada dış bir araç tarafından toplanabilir bir tek Event Hubs ad alanı veya olay hub'ına gönderilecek aracılığıyla gösterilmektedir.

> [!VIDEO https://www.youtube.com/embed/SPHxCgbcvSw]

## <a name="what-data-can-i-send-into-an-event-hub"></a>Hangi veri bir olay hub'ına gönderebilirim?

Azure ortamınızda 'izleme verilerinin çeşitli katmanları' vardır ve her bir katman veri erişimi yöntemi biraz farklılık gösterir. Genellikle, bu Katmanlar olarak açıklanabilir:

- **Uygulama izleme verileri:** Performansı ve işlevselliği yazmış ve Azure üzerinde çalışan kodun ilgili veriler. İzleme verileri uygulama performans izleme, uygulama günlükleri ve kullanıcı telemetrisi örneklerindendir. Uygulama izleme verileri, genellikle aşağıdaki yollardan biriyle toplanır:
  - Kodunuzu bir SDK'sı ile gibi işaretleyerek [Application Insights SDK'sı](../../azure-monitor/app/app-insights-overview.md).
  - Uygulamanızı, gibi çalıştıran makinede yeni bir uygulama günlüklerini için bekleyen bir izleme Aracısı'nı çalıştırarak [Windows Azure tanılama Aracısı](./../../azure-monitor/platform/diagnostics-extension-overview.md) veya [Linux Azure tanılama Aracısı](../../virtual-machines/extensions/diagnostics-linux.md).
- **Konuk işletim sistemi izleme verileri:** Uygulamanızın üzerinde çalıştığı işletim sistemiyle ilgili veriler. Konuk işletim sistemi izleme verileri örnekleri Linux syslog veya Windows Sistem olaylarını olacaktır. Bu tür veriler toplamak için aşağıdaki gibi bir aracı yüklemeniz gerekir [Windows Azure tanılama Aracısı](./../../azure-monitor/platform/diagnostics-extension-overview.md) veya [Linux Azure tanılama Aracısı](../../virtual-machines/extensions/diagnostics-linux.md).
- **Azure kaynak: izleme verileri** Bir Azure kaynağının çalışması hakkında veriler. Sanal makineler gibi bazı Azure kaynak türleri için var. bir konuk işletim sistemi ve uygulamaları içinde Azure hizmetini izlemek için (Olduğundan hiçbir konuk işletim sistemi veya uygulama kaynaklarla içinde çalışan), ağ güvenlik grupları gibi diğer Azure kaynakları için izleme verileri mevcut veri en yüksek katman kaynaktır. Bu veri kullanarak toplanabilir [kaynak tanılama ayarlarını](./../../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings).
- **Azure aboneliği: izleme verileri** Azure işlem ve sistem durumu hakkında veriler yanı sıra, işlem ve bir Azure aboneliğinin yönetim verileri kendisini. [Etkinlik günlüğü](./../../azure-monitor/platform/activity-logs-overview.md) izleme verileri, hizmet durumu olayları ve Azure Resource Manager denetimleri gibi çoğu abonelik içerir. Günlük profilini kullanarak bu verileri toplayabilir.
- **İzleme verilerini azure kiracısı:** Azure Active Directory gibi Azure hizmetlerinin Kiracı düzeyinde çalışması hakkında veriler. Azure Active Directory denetimlerimiz ve oturum açma işlemleri izleme verilerini Kiracı örnekleridir. Bu veriler, bir kiracı tanılama ayarını kullanarak toplanabilir.

Herhangi bir katmanı verileri, bir olay hub'ına, burada bir iş ortağı aracına çekilebilir gönderilebilir. Bazı kaynakları başka bir işlem sırasında bir mantıksal uygulama gerekli verileri almak için gerekli olduğu gibi doğrudan bir olay hub'ına veri göndermek üzere yapılandırılabilir. Sonraki bölümlerde, verileri olay hub'ına akışla için her katmandan nasıl yapılandırılacağını açıklar. Adımları izlenmesi, o katmanın varlıklar zaten sahip olduğunuzu varsaymaktadır.

## <a name="set-up-an-event-hubs-namespace"></a>Bir Event Hubs ad alanı ayarlama

Başlamadan önce yapmanız [Event Hubs ad alanı ve olay hub'ı oluşturma](../../event-hubs/event-hubs-create.md). Bu ad alanı ve olay hub'ı tüm izleme verilerinizi için hedef olur. Aynı erişim ilkesi paylaşan event hubs'ı mantıksal bir gruplandırmasını bir Event Hubs ad alanı, çok gibi bir depolama hesabı depolama hesap dahilindeki tek tek bloblar sahiptir. Event hubs ad alanı ve oluşturduğunuz olay hub'ları birkaç ayrıntılarını lütfen unutmayın:
* Standart Event Hubs ad alanı kullanmanızı öneririz.
* Genellikle, yalnızca bir üretilen iş birimi gereklidir. Günlük kullanım arttıkça ölçeklendirilebilecek şekilde gerekiyorsa, her zaman el ile daha sonra ad alanı için işleme birimleri sayısını artırmak veya Otomatik enflasyon etkinleştirin.
* Üretilen iş birimlerinin sayısı, event hubs'ınız için aktarım hızı ölçeği artırmanıza olanak sağlar. Bölüm sayısı, birçok tüketicilere tüketim paralel hale getirmek sağlar. Tek bir bölüm 20MBps kadar veya yaklaşık yapabilirsiniz saniyede 20.000 iletileri. Verileri kullanan bir aracı bağlı olarak olabilir veya birden çok bölümdeki verileri kullanan desteklemiyor olabilir. Ayarlanacak bölüm sayısı hakkında emin değilseniz, dört bölüm ile başlamanızı öneririz.
* 7 gün için olay hub'ınızdaki ileti bekletme ayarlamanızı öneririz. Alıcı aracınız için bir günden kalırsa, bu aracın kaldığı yukarı seçebilir sağlar (olaylar için en fazla 7 gün).
* Olay hub'ınız için varsayılan bir tüketici grubu kullanmanızı öneririz. Diğer tüketici grubu oluşturun veya aynı olay hub'ı aynı verileri kullanan iki farklı araçları planlamıyorsanız bir ayrı bir tüketici grubu kullanmak için gerek yoktur.
* Azure etkinlik günlüğü için bir Event Hubs ad alanı seçin ve Azure İzleyici 'ınsights-günlükleri-operationallogs.' olarak adlandırılan bu ad alanı içinde bir olay hub'ı oluşturur. Diğer günlük türleri için (aynı ınsights günlükleri operationallogs olay hub'ı yeniden kullanmanıza olanak tanır) mevcut bir olay hub'ya da seçebilirsiniz veya Azure izleyici günlüğü Kategori başına bir olay hub'ı oluşturun.
* Genellikle, bağlantı noktası 5671 ve 5672 olay merkezinde verileri tüketme makinede açılması gerekir.

Ayrıca bkz [Azure Event Hubs SSS Sayfasındaki](../../event-hubs/event-hubs-faq.md).

## <a name="azure-tenant-monitoring-data"></a>İzleme verileri bir azure kiracısı

Azure Kiracı izleme verilerini şu anda yalnızca Azure Active Directory için kullanılabilir. Verilerden kullanabileceğiniz [Azure Active Directory raporlama](../../active-directory/reports-monitoring/overview-reports.md), oturum açma etkinlik ve denetim izi belirli bir kiracıda yapılan değişikliklerin geçmişini içerir.

### <a name="azure-active-directory-data"></a>Azure Active Directory veri

Bir Event Hubs ad alanına Azure Active Directory günlüğünden veri göndermek için bir kiracı tanılama ayarı AAD kiracınızda ayarlayın. [Bu kılavuzu izleyerek](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md) Kiracı tanılama ayarı oluşturmanız için.

## <a name="azure-subscription-monitoring-data"></a>İzleme verileri bir azure aboneliği

İzleme verileri bir azure aboneliği kullanılabilir [Azure etkinlik günlüğü](./../../azure-monitor/platform/activity-logs-overview.md). Bu oluşturma içerir, güncelleştirme ve silme işlemleri Kaynak Yöneticisi'nden yapılan değişiklikler [Azure hizmet durumu](../../service-health/service-health-overview.md) , aboneliğinizdeki kaynaklar etkileyebilir [kaynak durumu](../../service-health/resource-health-overview.md) durumu geçişleri ve diğer birçok abonelik düzeyindeki olayların türde. [Bu makalede, Azure etkinlik günlüğü'nde görüntülenen olaylar tüm kategorileri ayrıntılı](./../../azure-monitor/platform/activity-log-schema.md).

### <a name="activity-log-data"></a>Etkinlik günlüğü verileri

Bir Event Hubs ad alanına Azure etkinlik günlüğünde veri göndermek için aboneliğinizde bir günlük profili ayarlayın. [Bu kılavuzu izleyerek](./activity-logs-stream-event-hubs.md) aboneliğinizde bir günlük profili ayarlamak için. Bunu, izlemek istediğiniz abonelik başına bir kez yaparsınız.

> [!TIP]
> Günlük profilini şu anda yalnızca bir olay hub'ı adı 'insights-operational-logs.' oluşturulduğu bir Event Hubs ad alanı seçmenizi sağlar Henüz bir günlük profili kendi olay hub adı belirtmek mümkün değildir.

## <a name="azure-resource-metrics-and-diagnostics-logs"></a>Azure kaynak ölçümleri ve tanılama günlükleri

Azure kaynaklarını izleme verilerinin iki tür göstermiyor:
1. [Kaynak tanılama günlükleri](diagnostic-logs-overview.md)
2. [Ölçümler](data-platform.md)

Her iki tür veri kaynak tanılama ayarı kullanarak bir olay hub'ına gönderilir. [Bu kılavuzu izleyerek](diagnostic-logs-stream-event-hubs.md) kaynak tanılama ayarı belirli bir kaynak üzerinde ayarlamak için. Kaynak tanılama ayarı günlükleri toplamak istediğiniz her kaynaktan üzerinde ayarlayın.

> [!TIP]
> Azure İlkesi belirli bir kapsamdaki tüm kaynakların her zaman bir tanılama ayarı ile kurulduğundan emin olmak için kullanabileceğiniz [Deployıfnotexists etkisi ilke kuralında kullanarak](../../governance/policy/concepts/definition-structure.md#policy-rule).

## <a name="guest-os-data"></a>Konuk işletim sistemi veri

Konuk işletim sistemi izleme verileri bir olay hub'ına göndermek için bir aracı yüklemeniz gerekir. Windows veya Linux için olay hub'ı ve bunun yanı sıra veri yapılandırma dosyası gönderilmesini ve bu yapılandırma dosyası sanal makinede çalışan Aracısı geçirmek olay hub'ı gönderilmesini istediğiniz verileri belirtin.

### <a name="linux-data"></a>Linux veri

[Linux Azure tanılama Aracısı](../../virtual-machines/extensions/diagnostics-linux.md) göndermek için kullanılan bir Linux makine verileri olay hub'ına izleme. Bu, LAD bir havuz olarak olay hub'ı ekleyerek yapılandırma dosyası korunan ayarları JSON yapabilirsiniz. [Bu makalede, Linux Azure tanılama aracısı için olay hub'ı havuzu ekleme hakkında daha fazla bilgi için bkz](../../virtual-machines/extensions/diagnostics-linux.md#protected-settings).

> [!NOTE]
> Konuk işletim sistemi izleme verileri portalda olay hub'ına akış ayarlanamaz. Bunun yerine, yapılandırma dosyasını el ile düzenlemeniz gerekir.

### <a name="windows-data"></a>Windows veri

[Windows Azure tanılama Aracısı](./../../azure-monitor/platform/diagnostics-extension-overview.md) göndermek için kullanılan bir Windows makineden veri bir olay hub'ına izleme. Bunu, privateConfig bölümüne WAD yapılandırma dosyasının bir havuz olarak olay hub'ı ekleyerek yapabilirsiniz. [Bu makalede, Windows Azure tanılama aracısı için olay hub'ı havuzu ekleme hakkında daha fazla bilgi için bkz](./../../azure-monitor/platform/diagnostics-extension-stream-event-hubs.md).

> [!NOTE]
> Konuk işletim sistemi izleme verileri portalda olay hub'ına akış ayarlanamaz. Bunun yerine, yapılandırma dosyasını el ile düzenlemeniz gerekir.

## <a name="application-monitoring-data"></a>Uygulama izleme verileri

İzleme verileri uygulama kodunuzu bulunmadığı için yönlendirme uygulama izleme verileri bir Azure olay hub'ına genel amaçlı bir çözüme bir SDK ile işaretlenmiş gerekir. Ancak, [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) Azure uygulama düzeyi verileri toplamak için kullanılan bir hizmettir. Application Insights kullanıyorsanız, aşağıdakileri yaparak izleme verilerini olay hub'ına akış:

1. [Sürekli dışarı aktarma kümesi](../../azure-monitor/app/export-telemetry.md) Application Insights verileri bir depolama hesabı.

2. Bir Zamanlayıcı ile tetiklenen mantıksal uygulaması ayarlama, [blob depolamadan/depolamaya veri çeker](../../connectors/connectors-create-api-azureblobstorage.md#add-action) ve [olay hub'ına ileti olarak gönderim](../../connectors/connectors-create-api-azure-event-hubs.md#add-action).

## <a name="what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub"></a>My olay hub'ına gönderilen izleme verilerini ile ne yapabilirim?

Bir olay hub'ına Azure İzleyici ile izleme verilerinizi yönlendirme, iş ortağı SIEM ve izleme araçları ile kolayca tümleştirmenize olanak sağlar. Çoğu araç, olay hub'ı bağlantı dizesi ve verileri olay hub'ından okumak için Azure aboneliğinize belirli izinler gerektirir. Azure İzleyici tümleştirmesine sahip araçların bir bölümü aşağıda listelenmiştir:

* **IBM QRadar** -Microsoft Azure DSM ve Microsoft Azure olay hub'ı Protokolü sitesinden indirilebilir [IBM Destek Web sitesi](https://www.ibm.com/support). [Azure ile tümleştirme hakkında daha fazla bilgiye buradan](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0) ulaşabilirsiniz.
* **Splunk** -Splunk kurulumunuza bağlı olarak, iki yaklaşım vardır:
    1. [Azure İzleyici eklenti Splunk için](https://splunkbase.splunk.com/app/3534/) Splunkbase ve açık kaynaklı proje kullanılabilir. [Belgeleri buradadır](https://github.com/Microsoft/AzureMonitorAddonForSplunk/wiki/Azure-Monitor-Addon-For-Splunk).
    2. Bir eklenti yükleyemiyorsanız Splunk Örneğinizde (örn.) varsa bir ara sunucu kullanıldığında veya Splunk bulutunda çalışan), bu olayları kullanarak Splunk HTTP Olay Toplayıcısı iletebilir [olay hub'ındaki yeni iletileri tarafından tetiklenen bu işlevin](https://github.com/Microsoft/AzureFunctionforSplunkVS).
* **SumoLogic** -bir olay hub'ından veri tüketmek SumoLogic ayarlamaya yönelik yönergeler [buradan kullanılabilir](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub)
* **ArcSight** -ArcSight Azure olay hub'ı akıllı bağlayıcı olarak kullanılabilir parçası [ArcSight akıllı bağlayıcı koleksiyonu](https://community.softwaregrp.com/t5/Discussions/Announcing-General-Availability-of-ArcSight-Smart-Connectors-7/m-p/1671852).
* **Syslog sunucusu** - stream Azure İzleyici verileri doğrudan bir syslog sunucusuna göz atabilirsiniz isterseniz [bu GitHub deposunu](https://github.com/miguelangelopereira/azuremonitor2syslog/).

## <a name="next-steps"></a>Sonraki Adımlar
* [Bir depolama hesabı için Etkinlik günlüğünü arşivleme](../../azure-monitor/platform/archive-activity-log.md)
* [Azure etkinlik günlüğüne genel bakış okuyun](../../azure-monitor/platform/activity-logs-overview.md)
* [Bir etkinlik günlüğü olayı temel alan bir uyarı ayarlama](../../azure-monitor/platform/alerts-log-webhook.md)


