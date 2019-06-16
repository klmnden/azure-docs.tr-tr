---
title: Azure hizmet durumu uyarıları göndermek PagerDuty ile Web kancalarını kullanma
description: PagerDuty Örneğiniz için hizmet durumu olayları hakkında Kişiselleştirilmiş bildirimler alın.
author: stephbaron
ms.author: stbaron
ms.topic: article
ms.service: service-health
ms.date: 06/10/2019
ms.openlocfilehash: ab3bcffb6453b284c3c8bb0d0373c7155fe8ef23
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067145"
---
# <a name="send-azure-service-health-alerts-with-pagerduty-using-webhooks"></a>Azure hizmet durumu uyarıları göndermek PagerDuty ile Web kancalarını kullanma

Bu makalede, bir Web kancası kullanarak PagerDuty aracılığıyla Azure hizmet durumu bildirimlerini ayarlama işlemini göstermektedir. Kullanarak [PagerDuty](https://www.pagerduty.com/)ait özel Microsoft Azure tümleştirme türü, hizmet durumu uyarıları, yeni veya mevcut PagerDuty hizmetlerinizi kolayca ekleyebilirsiniz.

## <a name="creating-a-service-health-integration-url-in-pagerduty"></a>PagerDuty içinde bir hizmet sistem durumu tümleştirme URL'si oluşturma
1.  Kaydolup ve oturum açmış emin olun, [PagerDuty](https://www.pagerduty.com/) hesabı.

1.  Gidin **Hizmetleri** PagerDuty bölümünde.

    ![PagerDuty "Hizmetler" bölümünde](./media/webhook-alerts/pagerduty-services-section.png)

1.  Seçin **yeni hizmet Ekle** veya mevcut bir hizmet ayarladığınız açın.

1.  İçinde **Tümleştirme ayarlarını**, aşağıdakileri seçin:

    a. **Tümleştirme türü**: Microsoft Azure

    b. **Tümleştirme adı**: \<Ad\>

    ![PagerDuty "tümleştirme ayarları"](./media/webhook-alerts/pagerduty-integration-settings.png)

1.  Herhangi diğer gerekli alanları doldurun ve seçin **Ekle**.

1.  Bu yeni tümleştirme ve kopyalama açma ve kaydetmeye ilişkin **tümleştirme URL'sini**.

    ![PagerDuty "tümleştirme URL'yi"](./media/webhook-alerts/pagerduty-integration-url.png)

## <a name="create-an-alert-using-pagerduty-in-the-azure-portal"></a>Azure portalında PagerDuty kullanarak bir uyarı oluştur
### <a name="for-a-new-action-group"></a>Yeni bir eylem grubu için:
1. 1 ile 8 arasındaki adımları [Azure portalını kullanarak yeni bir eylem grubu için bir hizmet durumu bildirimi üzerinde uyarı oluşturma](../azure-monitor/platform/alerts-activity-log-service-notifications.md).

1. Listesinde tanımlamak **Eylemler**:

    a. **Eylem türü:** *Web kancası*

    b. **Ayrıntılar:** PagerDuty **tümleştirme URL'sini** , daha önce kaydedildi.

    c. **Adı:** Web kancası'nın adı, diğer adı veya tanımlayıcısı.

1. Seçin **Kaydet** uyarı oluşturma işlemi tamamlandığında.

### <a name="for-an-existing-action-group"></a>Var olan bir eylem grubu için:
1. İçinde [Azure portalında](https://portal.azure.com/)seçin **İzleyici**.

1. İçinde **ayarları** bölümünden **Eylem grupları**.

1. Bulmak ve düzenlemek istediğiniz eylem grubu seçin.

1. Listesine ekleme **Eylemler**:

    a. **Eylem türü:** *Web kancası*

    b. **Ayrıntılar:** PagerDuty **tümleştirme URL'sini** , daha önce kaydedildi.

    c. **Adı:** Web kancası'nın adı, diğer adı veya tanımlayıcısı.

1. Seçin **Kaydet** eylem grubunu güncelleştirmeye işiniz bittiğinde.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Bir HTTP POST isteği üzerinden, Web kancası tümleştirme testi
1. Göndermek istediğiniz hizmet sistem durumu yükü oluşturun. Bir örnek hizmet sistem durumu Web kancası yükü konumunda bulabilirsiniz [günlük uyarıları Azure etkinlik için Web kancaları](../azure-monitor/platform/activity-log-alerts-webhook.md).

1. Bir HTTP POST isteği şu şekilde oluşturun:

    ```
    POST        https://events.pagerduty.com/integration/<IntegrationKey>/enqueue

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
1. Alması gereken bir `202 Accepted` , "Olay Kimliği" içeren bir ileti ile

1. Git [PagerDuty](https://www.pagerduty.com/) tümleştirmenizi başarılı bir şekilde ayarlandığını doğrulamak için.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [mevcut sorun yönetim sistemleri için Web kancası bildirimleri yapılandırma](service-health-alert-webhook-guide.md).
- Gözden geçirme [etkinlik günlüğü uyarısı Web kancası şeması](../azure-monitor/platform/activity-log-alerts-webhook.md). 
- Hakkında bilgi edinin [hizmet durumu bildirimlerini](../azure-monitor/platform/service-notifications.md).
- Daha fazla bilgi edinin [Eylem grupları](../azure-monitor/platform/action-groups.md).