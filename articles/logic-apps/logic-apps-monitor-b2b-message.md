---
title: B2B iletilerini izleme ve günlük kaydını - Azure Logic Apps ayarlama | Microsoft Docs
description: AS2, X 12 ve EDIFACT iletileri izleyin. Azure Logic Apps, tümleştirme hesabı için tanılama günlüğünü ayarlayın.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.date: 07/21/2017
ms.openlocfilehash: 63aa455851633d1e49fd1b26861aaac8a670ef15
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49404793"
---
# <a name="monitor-b2b-messages-and-set-up-logging-for-integration-accounts-in-azure-logic-apps"></a>B2B iletilerini izleme ve Azure Logic apps'te tümleştirme hesapları için günlük kaydını ayarlama

İkisi arasındaki B2B iletişim kurduktan sonra iş süreçlerine veya tümleştirme hesabınız üzerinden uygulamaları çalıştıran bu varlıkların birbirleriyle iletiler gönderip alabilir. Bu iletişim beklendiği gibi çalıştığından, AS2, X12, izlemeyi ayarlayabilirsiniz ve tümleştirme hesabınız üzerinden için tanılama günlüğünü birlikte EDIFACT iletileri onaylamak için [Azure Log Analytics](../log-analytics/log-analytics-overview.md) hizmeti. Bu hizmet, bulut izler ve şirket içi Ortamlarınızdaki, kullanılabilirliği ve performansı, tutmanıza yardımcı olur ve ayrıca çalışma zamanı Ayrıntılar ve daha zengin hata ayıklama olaylarını toplar. Ayrıca [tanılama verilerinizi diğer hizmetlerle kullanma](#extend-diagnostic-data), Azure depolama ve Azure Event Hubs gibi.

## <a name="requirements"></a>Gereksinimler

* Tanılama günlük kaydı ile ayarlanmış bir mantıksal uygulama. Bilgi [nasıl ayarlanacağı, mantıksal uygulama için günlüğe kaydetmeyi](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

  > [!NOTE]
  > Bu gereksinim karşılamanızın sonra bir Log Analytics çalışma alanı olmalıdır. Günlük tümleştirme hesabınız için ayarlarken aynı Log Analytics çalışma alanı kullanmanız gerekir. Bir Log Analytics çalışma alanınız yoksa, bilgi [bir Log Analytics çalışma alanı oluşturma](../log-analytics/log-analytics-quick-create-workspace.md).

* Mantıksal uygulamanıza bağlı olan tümleştirme hesabı. Bilgi [mantıksal uygulamanızın bağlantısını içeren bir tümleştirme hesabı oluşturma](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).

## <a name="turn-on-diagnostics-logging-for-your-integration-account"></a>Tanılama, tümleştirme hesabı için oturum açın

Doğrudan tümleştirme hesabınızdan ya da oturum açın veya [Azure İzleyici hizmeti aracılığıyla](#azure-monitor-service). Azure İzleyici, temel altyapı düzeyinde veriler ile izleme sağlar. Daha fazla bilgi edinin [Azure İzleyici](../azure-monitor/overview.md).

### <a name="turn-on-diagnostics-logging-directly-from-your-integration-account"></a>Tanılama doğrudan tümleştirme hesabından oturum açın

1. İçinde [Azure portalında](https://portal.azure.com)bulup tümleştirme hesabınızı seçin. Altında **izleme**, seçin **tanılama günlükleri** burada gösterildiği gibi:

   ![Bul ve tümleştirme hesabınızı seçin, "Tanılama günlükleri" seçin](media/logic-apps-monitor-b2b-message/integration-account-diagnostics.png)

2. Tümleştirme hesabı'nı seçtikten sonra aşağıdaki değerleri otomatik olarak seçilir. Bu değerler doğruysa seçin **tanılamayı Aç**. Aksi takdirde, istediğiniz değerleri seçin:

   1. Altında **abonelik**, tümleştirme hesabınızla kullandığınız Azure aboneliğini seçin.
   2. Altında **kaynak grubu**, tümleştirme hesabınızla kullandığınız kaynak grubunu seçin.
   3. Altında **kaynak türü**seçin **tümleştirme hesapları**. 
   4. Altında **kaynak**, tümleştirme hesabınızı seçin. 
   5. Seçin **tanılamayı Aç**.

   ![Tümleştirme hesabı için tanılamayı ayarlayın](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. Altında **tanılama ayarları**, ardından **durumu**, seçin **üzerinde**.

   ![Azure tanılamayı Aç](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Artık gösterildiği gibi günlük için kullanılacak veri ve Log Analytics çalışma alanı seçin:

   1. Seçin **Log Analytics'e gönderme**. 
   2. Altında **Log Analytics**, seçin **yapılandırma**. 
   3. Altında **OMS çalışma alanları**, günlüğe kaydetme için Log Analytics çalışma alanı seçin. 
   > [!NOTE]
   > OMS çalışma alanları, artık Log Analytics çalışma alanları da adlandırılır. 
   4. Altında **günlük**seçin **IntegrationAccountTrackingEvents** kategorisi.
   5. **Kaydet**'i seçin.

   ![Tanılama verilerini günlüğe gönderebilmek için Log Analytics'i ayarlama](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Artık [için Log analytics'te B2B iletilerinizi izleme ayarlama](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

<a name="azure-monitor-service"></a>

### <a name="turn-on-diagnostics-logging-through-azure-monitor"></a>Azure İzleyici aracılığıyla tanılama günlüğünü açma

1. İçinde [Azure portalında](https://portal.azure.com), ana Azure menüsünde **İzleyici**, **tanılama günlükleri**. Ardından burada gösterildiği gibi tümleştirme hesabı seçin:

   !["Tanılama günlükleri" seçin "İzle" tümleştirme hesabınızı seçin](media/logic-apps-monitor-b2b-message/monitor-service-diagnostics-logs.png)

2. Tümleştirme hesabı'nı seçtikten sonra aşağıdaki değerleri otomatik olarak seçilir. Bu değerler doğruysa seçin **tanılamayı Aç**. Aksi takdirde, istediğiniz değerleri seçin:

   1. Altında **abonelik**, tümleştirme hesabınızla kullandığınız Azure aboneliğini seçin.
   2. Altında **kaynak grubu**, tümleştirme hesabınızla kullandığınız kaynak grubunu seçin.
   3. Altında **kaynak türü**seçin **tümleştirme hesapları**.
   4. Altında **kaynak**, tümleştirme hesabınızı seçin.
   5. Seçin **tanılamayı Aç**.

   ![Tümleştirme hesabı için tanılamayı ayarlayın](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. Altında **tanılama ayarları**, seçin **üzerinde**.

   ![Azure tanılamayı Aç](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Artık gösterildiği günlüğe kaydetme için Log Analytics çalışma alanı ve olay kategorisi seçin:

   1. Seçin **Log Analytics'e gönderme**. 
   2. Altında **Log Analytics**, seçin **yapılandırma**. 
   3. Altında **OMS çalışma alanları**, günlüğe kaydetme için Log Analytics çalışma alanı seçin.
   > [!NOTE]
   > OMS çalışma alanları, artık Log Analytics çalışma alanları da adlandırılır.
   4. Altında **günlük**seçin **IntegrationAccountTrackingEvents** kategorisi.
   5. İşiniz bittiğinde **Kaydet**’i seçin.

   ![Tanılama verilerini günlüğe gönderebilmek için Log Analytics'i ayarlama](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Artık [için Log analytics'te B2B iletilerinizi izleme ayarlama](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Nasıl ve tanılama verilerini diğer hizmetleri ile kullandığınız genişletin

Azure Log Analytics ile birlikte mantıksal uygulamanızın tanılama verilerini diğer Azure hizmetleriyle örneğin kullanma genişletebilirsiniz: 

* [Azure depolama alanında Azure tanılama günlüklerini arşivleme](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [Azure Event hubs'a Stream Azure tanılama günlükleri](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

İzleme telemetri ve diğer hizmetlerden analytics kullanarak gerçek zamanlı Get ister sonra yapabilecekleriniz [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) ve [Power BI](../log-analytics/log-analytics-powerbi.md). Örneğin:

* [Event Hubs verilerini Stream Stream analytics'e](../stream-analytics/stream-analytics-define-inputs.md)
* [Stream Analytics ile akış verilerini analiz etme ve Power BI'da gerçek zamanlı analiz Pano oluşturma](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Ayarlamak istediğiniz seçenekleri bağlı olarak, emin olun, ilk [bir Azure depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md) veya [bir Azure olay hub'ı oluşturma](../event-hubs/event-hubs-create.md). Ardından, Tanılama verileri göndermek istediğiniz seçenekleri seçin:

![Azure depolama hesabına veya olay hub'ına veri Gönder](./media/logic-apps-monitor-b2b-message/storage-account-event-hubs.png)

> [!NOTE]
> Yalnızca bir depolama hesabı kullanmayı seçtiğinizde bekletme dönemleri uygulamak.

## <a name="supported-tracking-schemas"></a>Desteklenen izleme şemaları

Azure tüm şemalar dışında özel türü sabit şema türü izlemeyi destekler.

* [AS2 izleme şeması](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12 izleme şeması](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Özel izleme şeması](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Log analytics'te B2B iletilerini izleme](../logic-apps/logic-apps-track-b2b-messages-omsportal.md "Azure Log analytics'te izlemek B2B iletileri")
* [Enterprise Integration Pack hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")

