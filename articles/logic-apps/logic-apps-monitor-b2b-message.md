---
title: B2B hareketlerini izlemek ve günlük - Azure Logic Apps ayarlama | Microsoft Docs
description: İzleyici AS2, X 12 ve EDIFACT iletileri tümleştirme hesabınız için tanılama günlüğünü Başlat
author: padmavc
manager: anneta
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 6afab12b9e2d6e8686ecbc95be9743afbe70d98c
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="monitor-and-set-up-diagnostics-logging-for-b2b-communication-in-integration-accounts"></a>İzleme ve tanılama günlüklerini tümleştirme hesaplarındaki B2B iletişimi için ayarlayın

İkisi arasındaki B2B iletişim kurduktan sonra iş süreçlerini veya tümleştirme hesabınızı aracılığıyla uygulamaları çalıştıran bu varlıkların birbirleriyle iletileri değiştirebilir. Bu iletişim beklendiği gibi çalıştığından, AS2, X12, izleme işlevini ayarlama ve tümleştirme hesabınız üzerinden için tanılama günlüğünü birlikte EDIFACT iletileri onaylamak için [Azure günlük analizi](../log-analytics/log-analytics-overview.md) hizmet. Bu hizmet, bulut izler ve şirket içi ortamları, bunların kullanılabilirlik ve performans, tutmanıza yardımcı olur ve ayrıca çalışma zamanı Ayrıntılar ve daha zengin hata ayıklama olaylarını toplar. Ayrıca [diğer hizmetlerle tanılama verilerinizi kullanma](#extend-diagnostic-data)Azure Storage ve Azure Event Hubs gibi.

## <a name="requirements"></a>Gereksinimler

* Tanılama Günlüğü ile ayarlanmış bir mantıksal uygulama. Bilgi [bu mantıksal uygulama için günlük kaydını ayarlamak nasıl](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

  > [!NOTE]
  > Bu gereksinim karşılamanızın sonra günlük analizi çalışma olması gerekir. Tümleştirme hesabınız için günlüğü ayarladığınızda, aynı günlük analizi çalışma alanı kullanmanız gerekir. Günlük analizi çalışma alanı yoksa, bilgi [günlük analizi çalışma alanı oluşturmak nasıl](../log-analytics/log-analytics-quick-create-workspace.md).

* Mantıksal uygulamanızı bağlantılı bir tümleştirme hesabı. Bilgi [mantıksal uygulamanızı bağlantı tümleştirme hesap oluşturmak nasıl](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).

## <a name="turn-on-diagnostics-logging-for-your-integration-account"></a>Tanılama tümleştirme hesabınızda oturum açın

Doğrudan tümleştirme hesabınızdan ya da günlük özelliğini açın veya [Azure İzleyicisi hizmeti aracılığıyla](#azure-monitor-service). Azure İzleyicisi temel altyapı düzeyinde verilerle izleme sağlar. Daha fazla bilgi edinmek [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md).

### <a name="turn-on-diagnostics-logging-directly-from-your-integration-account"></a>Tanılama tümleştirme hesabınızdan doğrudan oturum aç

1. İçinde [Azure portal](https://portal.azure.com), bulma ve tümleştirme hesabınızı seçin. Altında **izleme**, seçin **tanılama günlükleri** aşağıda gösterildiği gibi:

   ![Bulma ve tümleştirme hesabınızı seçin, "Tanılama günlükleri"](media/logic-apps-monitor-b2b-message/integration-account-diagnostics.png)

2. Tümleştirme hesabınızı seçtikten sonra aşağıdaki değerleri otomatik olarak seçilir. Bu değerler doğru olup olmadığını seçin **tanılamayı açın**. Aksi takdirde, istediğiniz değerleri seçin:

   1. Altında **abonelik**, tümleştirme hesabınızla kullandığınız Azure aboneliğini seçin.
   2. Altında **kaynak grubu**, tümleştirme hesabınızla kullandığınız kaynak grubu seçin.
   3. Altında **kaynak türü**seçin **tümleştirme hesapları**. 
   4. Altında **kaynak**, tümleştirme hesabınızı seçin. 
   5. Seçin **tanılamayı açın**.

   ![Tanılama tümleştirme hesabınız için ayarlama](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. Altında **tanılama ayarları**ve ardından **durum**, seçin **üzerinde**.

   ![Azure tanılamayı açın](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Şimdi gösterildiği gibi günlük için kullanılacak veri ve günlük analizi çalışma alanı seçin:

   1. Seçin **için günlük analizi Gönder**. 
   2. Altında **günlük analizi**, seçin **yapılandırma**. 
   3. Altında **OMS çalışma alanları**, günlük için kullanılacak günlük analizi çalışma alanını seçin.
   4. Altında **günlük**seçin **IntegrationAccountTrackingEvents** kategorisi.
   5. **Kaydet**'i seçin.

   ![Tanılama verilerini günlüğe gönderebilmek için günlük analizi ayarlayın](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Şimdi [B2B iletilerinizi günlük analizi için izlemeyi ayarlama](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

<a name="azure-monitor-service"></a>

### <a name="turn-on-diagnostics-logging-through-azure-monitor"></a>Tanılama günlükleri Azure İzleyicisi üzerinden Aç

1. İçinde [Azure portal](https://portal.azure.com), ana Azure menüsünde, **İzleyici**, **tanılama günlükleri**. Ardından aşağıda gösterildiği gibi tümleştirme hesabınızı seçin:

   !["İzleme", "Tanılama günlükleri" seçin tümleştirme hesabınızı seçin](media/logic-apps-monitor-b2b-message/monitor-service-diagnostics-logs.png)

2. Tümleştirme hesabınızı seçtikten sonra aşağıdaki değerleri otomatik olarak seçilir. Bu değerler doğru olup olmadığını seçin **tanılamayı açın**. Aksi takdirde, istediğiniz değerleri seçin:

   1. Altında **abonelik**, tümleştirme hesabınızla kullandığınız Azure aboneliğini seçin.
   2. Altında **kaynak grubu**, tümleştirme hesabınızla kullandığınız kaynak grubu seçin.
   3. Altında **kaynak türü**seçin **tümleştirme hesapları**.
   4. Altında **kaynak**, tümleştirme hesabınızı seçin.
   5. Seçin **tanılamayı açın**.

   ![Tanılama tümleştirme hesabınız için ayarlama](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. Altında **tanılama ayarları**, seçin **üzerinde**.

   ![Azure tanılamayı açın](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Şimdi gösterildiği gibi günlüğü için günlük analizi çalışma alanı ve olay kategorisi seçin:

   1. Seçin **için günlük analizi Gönder**. 
   2. Altında **günlük analizi**, seçin **yapılandırma**. 
   3. Altında **OMS çalışma alanları**, günlük için kullanılacak günlük analizi çalışma alanını seçin.
   4. Altında **günlük**seçin **IntegrationAccountTrackingEvents** kategorisi.
   5. İşiniz bittiğinde **Kaydet**’i seçin.

   ![Tanılama verilerini günlüğe gönderebilmek için günlük analizi ayarlayın](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Şimdi [B2B iletilerinizi günlük analizi için izlemeyi ayarlama](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Nasıl ve tanılama verilerini diğer hizmetleri ile kullandığınız genişletme

Azure günlük analizi ile birlikte nasıl mantığı uygulamanızın tanılama verilerini diğer Azure hizmetleriyle örneğin kullandığınız genişletebilirsiniz: 

* [Azure depolama alanında Azure tanılama günlüklerini arşiv](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [Azure Event hubs'a akış Azure tanılama günlükleri](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

Telemetri ve diğer hizmetlerden analizi kullanarak izleme Get gerçek zamanlı ister sonra şunları yapabilirsiniz [Azure akış analizi](../stream-analytics/stream-analytics-introduction.md) ve [Power BI](../log-analytics/log-analytics-powerbi.md). Örneğin:

* [Event Hubs akış verilerini akış analizi](../stream-analytics/stream-analytics-define-inputs.md)
* [Akış Analizi ile akış verilerini çözümleme ve gerçek zamanlı analiz Pano Power BI'da oluşturma](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Ayarlamak istediğiniz seçenekleri bağlı olarak, olduğundan emin olun, ilk [bir Azure depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md) veya [Azure olay hub'ı oluşturma](../event-hubs/event-hubs-create.md). Ardından Tanılama verileri gönder istediğiniz seçenekleri seçin:

![Verileri Azure depolama hesabı veya olay hub'ına Gönder](./media/logic-apps-monitor-b2b-message/storage-account-event-hubs.png)

> [!NOTE]
> Yalnızca bir depolama hesabı kullanmayı seçtiğinizde bekletme dönemleri uygulamak.

## <a name="supported-tracking-schemas"></a>Desteklenen izleme şemaları

Azure bunlar tüm özel türü dışında şemaları sabit şema türleri, izleme destekler.

* [AS2 izleme şeması](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12 izleme şeması](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Özel izleme şeması](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlük analizi B2B iletilerini izlemek](../logic-apps/logic-apps-track-b2b-messages-omsportal.md "OMS izleme B2B iletileri")
* [Enterprise Integration Pack hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")

