---
title: Azure hizmet durumu uyarıları göndermek OpsGenie ile Web kancalarını kullanma
description: OpsGenie Örneğiniz için hizmet durumu olayları hakkında Kişiselleştirilmiş bildirimler alın.
author: stephbaron
ms.author: stbaron
ms.topic: article
ms.service: service-health
ms.date: 06/10/2019
ms.openlocfilehash: fab99b7093ac3f18f6313273d21905e0a3ed7e5b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067158"
---
# <a name="send-azure-service-health-alerts-with-opsgenie-using-webhooks"></a>Azure hizmet durumu uyarıları göndermek OpsGenie ile Web kancalarını kullanma

Bu makalede bir Web kancası kullanarak OpsGenie ile Azure hizmet durumu uyarıları ayarlamak nasıl gösterir. Kullanarak [OpsGenie](https://www.opsgenie.com/)ait Azure hizmet durumu tümleştirmesi, ilettiğinizde Azure hizmet durumu uyarıları için OpsGenie. OpsGenie bildirmek için doğru kişilere nöbet zamanlamalarını temel alan e-posta, kısa mesaj (SMS), telefon görüşmeleri, iOS ve Android anında iletme bildirimleri kullanarak ve uyarı onaylanır veya kadar uyarılar Etkinleºmesini belirleyebilirsiniz.

## <a name="creating-a-service-health-integration-url-in-opsgenie"></a>Hizmet durumu tümleştirme URL'si OpsGenie içinde oluşturma
1.  Kaydolup ve oturum açmış emin olun, [OpsGenie](https://www.opsgenie.com/) hesabı.

1.  Gidin **tümleştirmeler** OpsGenie bölümünde.

    ![OpsGenie "Tümleştirmeler" bölümünde](./media/webhook-alerts/opsgenie-integrations-section.png)

1.  Seçin **Azure hizmet durumu** tümleştirme düğmesi.

    ![OpsGenie içinde "Azure hizmet durumu button"](./media/webhook-alerts/opsgenie-azureservicehealth-button.png)

1.  **Adı** , uyarı ve belirtin **takıma atanmış** alan.

1.  Diğer alanlar gibi doldurun **alıcılar**, **etkin**, ve **bildirimleri bastır**.

1.  Kopyalayıp kaydedin **tümleştirme URL'sini**, hangi zaten içermelidir, `apiKey` sonuna.

    ![OpsGenie "tümleştirme URL'yi"](./media/webhook-alerts/opsgenie-integration-url.png)

1.  Seçin **tümleştirme Kaydet**

## <a name="create-an-alert-using-opsgenie-in-the-azure-portal"></a>Azure portalında OpsGenie kullanarak bir uyarı oluştur
### <a name="for-a-new-action-group"></a>Yeni bir eylem grubu için:
1. 1 ile 8 arasındaki adımları [Azure portalını kullanarak yeni bir eylem grubu için bir hizmet durumu bildirimi üzerinde uyarı oluşturma](../azure-monitor/platform/alerts-activity-log-service-notifications.md).

1. Listesinde tanımlamak **Eylemler**:

    a. **Eylem türü:** *Web kancası*

    b. **Ayrıntılar:** OpsGenie **tümleştirme URL'sini** , daha önce kaydedildi.

    c. **Adı:** Web kancası'nın adı, diğer adı veya tanımlayıcısı.

1. Seçin **Kaydet** uyarı oluşturma işlemi tamamlandığında.

### <a name="for-an-existing-action-group"></a>Var olan bir eylem grubu için:
1. İçinde [Azure portalında](https://portal.azure.com/)seçin **İzleyici**.

1. İçinde **ayarları** bölümünden **Eylem grupları**.

1. Bulmak ve düzenlemek istediğiniz eylem grubu seçin.

1. Listesine ekleme **Eylemler**:

    a. **Eylem türü:** *Web kancası*

    b. **Ayrıntılar:** OpsGenie **tümleştirme URL'sini** , daha önce kaydedildi.

    c. **Adı:** Web kancası'nın adı, diğer adı veya tanımlayıcısı.

1. Seçin **Kaydet** eylem grubunu güncelleştirmeye işiniz bittiğinde.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Bir HTTP POST isteği üzerinden, Web kancası tümleştirme testi
1. Göndermek istediğiniz hizmet sistem durumu yükü oluşturun. Bir örnek hizmet sistem durumu Web kancası yükü konumunda bulabilirsiniz [günlük uyarıları Azure etkinlik için Web kancaları](../azure-monitor/platform/activity-log-alerts-webhook.md).

1. Bir HTTP POST isteği şu şekilde oluşturun:

    ```
    POST        https://api.opsgenie.com/v1/json/azureservicehealth?apiKey=<APIKEY>

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
1. Alması gereken bir `200 OK` durumu "başarılı" iletisi ile yanıt

1. Git [OpsGenie](https://www.opsgenie.com/) tümleştirmenizi başarılı bir şekilde ayarlandığını doğrulamak için.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [mevcut sorun yönetim sistemleri için Web kancası bildirimleri yapılandırma](service-health-alert-webhook-guide.md).
- Gözden geçirme [etkinlik günlüğü uyarısı Web kancası şeması](../azure-monitor/platform/activity-log-alerts-webhook.md). 
- Hakkında bilgi edinin [hizmet durumu bildirimlerini](../azure-monitor/platform/service-notifications.md).
- Daha fazla bilgi edinin [Eylem grupları](../azure-monitor/platform/action-groups.md).