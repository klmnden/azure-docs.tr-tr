---
title: Azure hizmet durumu uyarıları ile OpsGenie yapılandırma | Microsoft Docs
description: OpsGenie Örneğiniz için hizmet sistem durumu olayları hakkında Kişiselleştirilmiş bildirimler alın.
author: shawntabrizi
manager: scotthit
editor: ''
services: service-health
documentationcenter: service-health
ms.assetid: ''
ms.service: service-health
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: shtabriz
ms.openlocfilehash: 6b8017f62dd895219f1d2cdac40f0efdf2db6c93
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30179353"
---
# <a name="configure-service-health-alerts-with-opsgenie"></a>Hizmet durumu uyarıları OpsGenie ile yapılandırma

Bu makalede bir Web kancası kullanarak OpsGenie ile Azure hizmet durumu uyarıları ayarlamak kullanmayı gösterir. Kullanarak [OpsGenie](https://www.opsgenie.com/)ait Azure hizmet durumu tümleştirmesi, Azure hizmet durumu uyarıları için OpsGenie iletebileceği. OpsGenie bildirmek için doğru kişilerin çağrısı zamanlamada dayalı olarak e-posta, metin iletisi (SMS), telefon görüşmeleri, iOS ve Android anında iletme bildirimleri kullanarak ve uyarı onaylanan veya kadar uyarıları yükselen belirleyebilirsiniz.

## <a name="creating-a-service-health-integration-url-in-opsgenie"></a>Bir hizmeti sistem durumu tümleştirme URL'si OpsGenie içinde oluşturma
1.  Kaydolup ve içine imzalı olduğundan emin olun, [OpsGenie](https://www.opsgenie.com/) hesabı.

2.  Gidin **tümleştirmeler** OpsGenie bölümünde.

    ![OpsGenie "Tümleştirmeler" bölümünde](./media/webhook-alerts/opsgenie-integrations-section.png)

3.  Seçin **Azure hizmet durumu** tümleştirme düğmesi.

    !["Azure hizmet durumu düğmesini" OpsGenie](./media/webhook-alerts/opsgenie-azureservicehealth-button.png)

4.  **Ad** , uyarı ve belirtin **ekibine atanan** alan.

5.  Gibi diğer alanları doldurun **alıcılar**, **etkin**, ve **Önle bildirimleri**.

6.  Kopyalayıp kaydedin **tümleştirme URL**, hangi zaten içermelidir, `apiKey` sonuna eklenir.

    !["Tümleştirme URL'de" OpsGenie](./media/webhook-alerts/opsgenie-integration-url.png)

7.  Seçin **tümleştirme Kaydet**

## <a name="create-an-alert-using-opsgenie-in-the-azure-portal"></a>Azure portalında OpsGenie kullanarak bir uyarı oluşturabilir.
### <a name="for-a-new-action-group"></a>İçin yeni bir eylem grubu:
1. 1 ile 8 arasındaki adımları [Azure Portalı'nı kullanarak yeni bir eylem grubu için bir hizmet sistem durumu bildirim bir uyarı oluşturulup](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md).

2. Listesinde tanımlamak **Eylemler**:

    a. **Eylem türü:** *Web kancası*

    b. **Ayrıntılar:** OpsGenie **tümleştirme URL** , daha önce kaydedildi.

    c. **Ad:** Web kancası'nın adı, diğer ad veya tanımlayıcı.

3. Seçin **kaydetmek** uyarı oluşturmak için yapıldığında.

### <a name="for-an-existing-action-group"></a>Var olan bir eylem grubu için:
1. İçinde [Azure portal](https://portal.azure.com/)seçin **İzleyici**.

2. İçinde **ayarları** bölümünde, select **Eylem grupları**.

3. Bulmak ve düzenlemek istediğiniz eylem grubunu seçin.

4. Listesine ekleme **Eylemler**:

    a. **Eylem türü:** *Web kancası*

    b. **Ayrıntılar:** OpsGenie **tümleştirme URL** , daha önce kaydedildi.

    c. **Ad:** Web kancası'nın adı, diğer ad veya tanımlayıcı.

5. Seçin **kaydetmek** eylem grubunu güncelleştirmek için yapıldığında.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Bir HTTP POST isteği üzerinden, Web kancası tümleştirme sınaması
1. Göndermek istediğiniz hizmet sistem durumu yükü oluşturun. Bir örnek hizmet sistem durumu Web kancası yükü sırasında bulduğunuz [Azure etkinlik için Web kancası oturum uyarıları](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md).

2. Bir HTTP POST isteği gibi oluşturun:

    ```
    POST        https://api.opsgenie.com/v1/json/azureservicehealth?apiKey=<APIKEY>

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
3. Alması gereken bir `200 OK` yanıt durumu "başarılı" iletisi

4. Git [OpsGenie](https://www.opsgenie.com/) tümleştirmenize başarıyla ayarlanmıştır onaylamak için.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [varolan sorunu yönetim sistemleri için Web kancası bildirimleri yapılandırmak](service-health-alert-webhook-guide.md).
- Gözden geçirme [etkinlik günlüğü uyarı Web kancası şeması](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md). 
- Hakkında bilgi edinin [hizmet durumu bildirimlerine](../monitoring-and-diagnostics/monitoring-service-notifications.md).
- Daha fazla bilgi edinmek [Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md).