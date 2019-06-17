---
title: Azure İzleyici günlükleri - Azure Logic Apps ile B2B iletilerini izleme | Microsoft Docs
description: AS2, X 12 ve EDIFACT iletileri tümleştirme hesapları ve Azure Logic Apps için izleme ve tanılama günlüklerini Azure İzleyici günlükleri ile ayarlayın
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 10/23/2018
ms.openlocfilehash: 12799a308157c3c0e19de1f82c0fe3df44fad37e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62106309"
---
# <a name="monitor-b2b-messages-with-azure-monitor-logs-in-azure-logic-apps"></a>Azure İzleyici günlüklerine Azure Logic Apps ile B2B iletilerini izleme

Tümleştirme hesabı ticari ortaklar arasında B2B iletişim kurduktan sonra iş ortakları birbirleriyle iletiler gönderip alabilir. Bu iletişim beklediğiniz gibi çalışır, AS2, X12, izleyebilir ve EDIFACT iletileri ve tümleştirme hesabınız için günlüğe kaydetme tanılama ayarlama denetlemek için [Azure İzleyicisi](../log-analytics/log-analytics-overview.md). Bu hizmet, Bulut ve şirket içi ortamlarını, kullanılabilirlik ve performansı korumak ve çalışma zamanı Ayrıntılar ve daha zengin hata ayıklama olaylarını toplar izler. Bu veriler, Azure depolama ve Azure Event Hubs gibi diğer hizmetlerle de kullanabilirsiniz.

> [!NOTE]
> Bu sayfa, başvurular için Microsoft Operations Management Suite (olan OMS), yine de olabilir [Ocak 2019 ' devre dışı bırakma](../azure-monitor/platform/oms-portal-transition.md), ancak bu adımlar, mümkün olduğunda Azure Log Analytics ile değiştirir. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Önkoşullar

* Tanılama günlük kaydı ile ayarlanmış bir mantıksal uygulama. Bilgi [bir mantıksal uygulama oluşturma işlemini](quickstart-create-first-logic-app-workflow.md) ve [nasıl ayarlanacağı, mantıksal uygulama için günlüğe kaydetmeyi](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Önceki gereksinimlerini sonra izleme ve Azure İzleyici günlüklerine aracılığıyla B2B iletişimi izlemek için kullandığınız bir Log Analytics çalışma alanı da gerekir. Bir Log Analytics çalışma alanınız yoksa, bilgi [bir Log Analytics çalışma alanı oluşturma](../azure-monitor/learn/quick-create-workspace.md).

* Mantıksal uygulamanıza bağlı olan tümleştirme hesabı. Bilgi [mantıksal uygulamanızın bağlantısını içeren bir tümleştirme hesabı oluşturma](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).

## <a name="turn-on-diagnostics-logging"></a>Tanılama günlüğünü etkinleştirme

Doğrudan tümleştirme hesabınızdan ya da oturum açın veya [Azure İzleyici hizmeti aracılığıyla](#azure-monitor-service). Azure İzleyici, temel altyapı düzeyinde veriler ile izleme sağlar. Daha fazla bilgi edinin [Azure İzleyici](../azure-monitor/overview.md).

### <a name="turn-on-logging-from-integration-account"></a>Tümleştirme hesabı oturum açma

1. İçinde [Azure portalında](https://portal.azure.com)bulup tümleştirme hesabınızı seçin. Altında **izleme**seçin **tanılama ayarları**.

   ![Bul ve tümleştirme hesabınızı seçin, "Tanılama ayarları" öğesini](media/logic-apps-monitor-b2b-message/find-integration-account.png)

1. Şimdi Bul ve tümleştirme hesabınızı seçin. Filtre listelerinde tümleştirme hesabınız için geçerli değerleri seçin.
İşiniz bittiğinde seçin **tanılama ayarı ekleme**.

   | Özellik | Değer | Açıklama | 
   |----------|-------|-------------|
   | **Abonelik** | <*Azure-subscription-name*> | Tümleştirme hesabınızla ilişkili Azure aboneliği | 
   | **Kaynak grubu** | <*Azure kaynak grubu adı*> | Tümleştirme hesabı için Azure kaynak grubu | 
   | **Kaynak türü** | **Tümleştirme hesaplarına genel bakış** | Günlüğü etkinleştirmek için istediğiniz bir Azure kaynak türü | 
   | **Kaynak** | <*Tümleştirme hesabı adı*> | Azure kaynağınıza, günlüğü etkinleştirmek için istediğiniz adı | 
   ||||  

   Örneğin:

   ![Tümleştirme hesabı için tanılamayı ayarlayın](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

1. Yeni bir tanılama ayarı için bir ad girin ve Log Analytics çalışma alanınızı ve günlüğe kaydetmek istediğiniz verileri seçin.

   1. Seçin **Log Analytics'e gönderme**. 

   1. Altında **Log Analytics**seçin **yapılandırma**. 

   1. Altında **OMS çalışma alanları**, günlük kaydı için kullanmak istediğiniz Log Analytics çalışma alanını seçin. 

      > [!NOTE]
      > OMS çalışma alanları Log Analytics çalışma alanları tarafından değiştirilir. 

   1. Altında **günlük**seçin **IntegrationAccountTrackingEvents** kategori seçip **Kaydet**.

   Örneğin: 

   ![Azure İzleyici günlüklerini tanılama verilerini günlüğe gönderebilmemiz ayarlayın](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

1. Artık [için Azure İzleyici günlüklerine B2B iletilerinizi izleme ayarlama](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

<a name="azure-monitor-service"></a>

### <a name="turn-on-logging-through-azure-monitor"></a>Azure İzleyici aracılığıyla günlüğü Aç

1. İçinde [Azure portalında](https://portal.azure.com), ana Azure menüsünde **İzleyici**. Altında **ayarları**seçin **tanılama ayarları**. 

   !["İzleme" seçin > "Tanılama ayarları" >, tümleştirme hesabı](media/logic-apps-monitor-b2b-message/monitor-diagnostics-settings.png)

1. Şimdi Bul ve tümleştirme hesabınızı seçin. Filtre listelerinde tümleştirme hesabınız için geçerli değerleri seçin.
İşiniz bittiğinde seçin **tanılama ayarı ekleme**.

   | Özellik | Değer | Açıklama | 
   |----------|-------|-------------|
   | **Abonelik** | <*Azure-subscription-name*> | Tümleştirme hesabınızla ilişkili Azure aboneliği | 
   | **Kaynak grubu** | <*Azure kaynak grubu adı*> | Tümleştirme hesabı için Azure kaynak grubu | 
   | **Kaynak türü** | **Tümleştirme hesaplarına genel bakış** | Günlüğü etkinleştirmek için istediğiniz bir Azure kaynak türü | 
   | **Kaynak** | <*Tümleştirme hesabı adı*> | Azure kaynağınıza, günlüğü etkinleştirmek için istediğiniz adı | 
   ||||  

   Örneğin:

   ![Tümleştirme hesabı için tanılamayı ayarlayın](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

1. Yeni bir tanılama ayarı için bir ad girin ve Log Analytics çalışma alanınızı ve günlüğe kaydetmek istediğiniz verileri seçin.

   1. Seçin **Log Analytics'e gönderme**. 

   1. Altında **Log Analytics**seçin **yapılandırma**. 

   1. Altında **OMS çalışma alanları**, günlük kaydı için kullanmak istediğiniz Log Analytics çalışma alanını seçin. 

      > [!NOTE]
      > OMS çalışma alanları Log Analytics çalışma alanları tarafından değiştirilir. 

   1. Altında **günlük**seçin **IntegrationAccountTrackingEvents** kategori seçip **Kaydet**.

   Örneğin: 

   ![Azure İzleyici günlüklerini tanılama verilerini günlüğe gönderebilmemiz ayarlayın](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

1. Artık [için Azure İzleyici günlüklerine B2B iletilerinizi izleme ayarlama](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

## <a name="use-diagnostic-data-with-other-services"></a>Tanılama verileri diğer hizmetleri ile kullanma

Azure İzleyici günlüklerine yanı sıra, mantıksal uygulamanızın tanılama verilerini diğer Azure hizmetleriyle örneğin kullanma genişletebilirsiniz: 

* [Azure depolama alanında Azure tanılama günlüklerini arşivleme](../azure-monitor/platform/archive-diagnostic-logs.md)
* [Azure Event hubs'a Stream Azure tanılama günlükleri](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md) 

İzleme telemetri ve diğer hizmetlerden analytics kullanarak gerçek zamanlı Get ister sonra yapabilecekleriniz [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) ve [Power BI](../azure-monitor/platform/powerbi.md). Örneğin:

* [Event Hubs verilerini Stream Stream analytics'e](../stream-analytics/stream-analytics-define-inputs.md)
* [Stream Analytics ile akış verilerini analiz etme ve Power BI'da gerçek zamanlı analiz Pano oluşturma](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Yedekleme kümesi istediğiniz seçenekleri bağlı olarak, emin olun, ilk [bir Azure depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md) veya [bir Azure olay hub'ı oluşturma](../event-hubs/event-hubs-create.md). Ardından, Tanılama verileri göndermek istediğiniz hedefleri seçebilirsiniz.
Yalnızca bir depolama hesabı kullanmayı seçtiğinizde bekletme dönemleri uygulamak.

![Azure depolama hesabına veya olay hub'ına veri Gönder](./media/logic-apps-monitor-b2b-message/diagnostics-storage-event-hub-log-analytics.png)

## <a name="supported-tracking-schemas"></a>Desteklenen izleme şemaları

Azure tüm şemalar dışında özel türü sabit şema türü izlemeyi destekler.

* [AS2 izleme şeması](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12 izleme şeması](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Özel izleme şeması](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici günlüklerine B2B iletilerini izleme](../logic-apps/logic-apps-track-b2b-messages-omsportal.md "Azure İzleyici günlüklerine izlemek B2B iletileri")
* [Enterprise Integration Pack hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")

