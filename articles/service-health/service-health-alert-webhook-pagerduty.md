---
title: "Azure hizmet durumu uyarıları ile PagerDuty yapılandırma | Microsoft Docs"
description: "PagerDuty Örneğiniz için hizmet sistem durumu olayları hakkında Kişiselleştirilmiş bildirimler alın."
author: shawntabrizi
manager: scotthit
editor: 
services: service-health
documentationcenter: service-health
ms.assetid: 
ms.service: service-health
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: shtabriz
ms.openlocfilehash: 9edcb727b9f0af348cacd5533523c4f2e8214703
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="configure-service-health-alerts-with-pagerduty"></a>Hizmet durumu uyarıları PagerDuty ile yapılandırma

Bu makalede, Azure hizmet durumu bildirimlerine PagerDuty aracılığıyla ayarlamak bir Web kancası kullanarak gösterilmiştir. Kullanarak [PagerDuty](https://www.pagerduty.com/)ait özel Microsoft Azure tümleştirme türü, yeni veya var olan PagerDuty hizmetleriniz için hizmet durumu uyarıları harcamadan ekleyebilirsiniz.

## <a name="creating-a-service-health-integration-url-in-pagerduty"></a>Bir hizmeti sistem durumu tümleştirme URL'si PagerDuty içinde oluşturma
1.  Kaydolup ve içine imzalı olduğundan emin olun, [PagerDuty](https://www.pagerduty.com/) hesabı.

2.  Gidin **Hizmetleri** PagerDuty bölümünde.

    ![PagerDuty "Hizmetler" bölümünde](./media/webhook-alerts/pagerduty-services-section.png)

3.  Seçin **yeni hizmet Ekle** veya ayarladığınız var olan bir hizmetini açın.

4.  İçinde **Tümleştirme ayarlarını**, aşağıdakileri seçin:

    a. **Tümleştirme türü**: Microsoft Azure

    b. **Tümleştirme adı**: \<adı\>

    !["Tümleştirme ayarları" PagerDuty](./media/webhook-alerts/pagerduty-integration-settings.png)

5.  Tüm diğer gerekli alanları doldurun ve seçin **Ekle**.

6.  Bu yeni tümleştirme ve kopyalama açmak ve kaydetmek **tümleştirme URL**.

    !["Tümleştirme URL'de" PagerDuty](./media/webhook-alerts/pagerduty-integration-url.png)

## <a name="create-an-alert-using-pagerduty-in-the-azure-portal"></a>Azure portalında PagerDuty kullanarak bir uyarı oluşturabilir.
### <a name="for-a-new-action-group"></a>İçin yeni bir eylem grubu:
1. 1 ile 8 arasındaki adımları [Azure Portalı'nı kullanarak yeni bir eylem grubu için bir hizmet sistem durumu bildirim bir uyarı oluşturulup](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md).

2. Listesinde tanımlamak **Eylemler**:

    a. **Eylem türü:** *Web kancası*

    b. **Ayrıntılar:** PagerDuty **tümleştirme URL** , daha önce kaydedildi.

    c. **Ad:** Web kancası'nın adı, diğer ad veya tanımlayıcı.

3. Seçin **kaydetmek** uyarı oluşturmak için yapıldığında.

### <a name="for-an-existing-action-group"></a>Var olan bir eylem grubu için:
1. İçinde [Azure portal](https://portal.azure.com/)seçin **İzleyici**.

2. İçinde **ayarları** bölümünde, select **Eylem grupları**.

3. Bulmak ve düzenlemek istediğiniz eylem grubunu seçin.

4. Listesine ekleme **Eylemler**:

    a. **Eylem türü:** *Web kancası*

    b. **Ayrıntılar:** PagerDuty **tümleştirme URL** , daha önce kaydedildi.

    c. **Ad:** Web kancası'nın adı, diğer ad veya tanımlayıcı.

5. Seçin **kaydetmek** eylem grubunu güncelleştirmek için yapıldığında.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Bir HTTP POST isteği üzerinden, Web kancası tümleştirme sınaması
1. Göndermek istediğiniz hizmet durumu yükü oluşturun. Bir örnek hizmet durumu Web kancası yükü sırasında bulduğunuz [Azure etkinlik için Web kancası oturum uyarıları](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md).

2. Bir HTTP POST isteği gibi oluşturun:

    ```
    POST        https://events.pagerduty.com/integration/<IntegrationKey>/enqueue

    HEADERS     Content-Type: application/json

    BODY        <Service Health payload>
    ```
3. Alması gereken bir `202 Accepted` , "Olay Kimliği" içeren bir ileti ile

4. Git [PagerDuty](https://www.pagerduty.com/) tümleştirmenize başarıyla ayarlanmıştır onaylamak için.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [varolan sorunu yönetim sistemleri için Web kancası bildirimleri yapılandırmak](service-health-alert-webhook-guide.md).
- Gözden geçirme [etkinlik günlüğü uyarı Web kancası şeması](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md). 
- Hakkında bilgi edinin [hizmet durumu bildirimlerine](../monitoring-and-diagnostics/monitoring-service-notifications.md).
- Daha fazla bilgi edinmek [Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md).