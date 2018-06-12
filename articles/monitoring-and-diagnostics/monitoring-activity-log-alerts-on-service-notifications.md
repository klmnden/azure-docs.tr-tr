---
title: Etkinlik günlüğü uyarıları Azure hizmeti bildirimleri alma
description: Azure hizmet ortaya çıktığında, SMS, e-posta veya Web kancası aracılığıyla bilgi edinin.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/09/2018
ms.author: johnkem
ms.component: alerts
ms.openlocfilehash: 01dc3a3c6489b694af26c78ae3b4756f3e8f00b7
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263125"
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Etkinlik günlüğü Uyarıları hizmeti bildirimlerinin oluşturun.
## <a name="overview"></a>Genel Bakış
Bu makalede Azure portalını kullanarak hizmet durumu bildirimlerine için etkinlik günlüğü uyarıları ayarlama gösterilmiştir.  

Azure hizmet durumu bildirimlerine Azure aboneliğinize gönderdiğinde bir uyarı alabilirsiniz. Temel uyarı yapılandırabilirsiniz:

- Hizmeti sistem durumu bildirimi (hizmet sorunları, planlı bakım, sistem durumu danışma) sınıfı.
- Etkilenen abonelik.
- Etkilenen Hizmet (ler).
- Etkilenen bilgiler.

Uyarı gönderen de yapılandırabilirsiniz:

- Var olan bir eylem grubunu seçin.
- (Bu uyarılar için kullanılabilir) yeni bir eylem grubu oluşturun.

Eylem grupları hakkında daha fazla bilgi için bkz: [oluşturma ve eylem gruplarını yönetme](monitoring-action-groups.md).

Azure Resource Manager şablonları kullanarak hizmet sistem durumu bildirimi uyarıları yapılandırma hakkında daha fazla bilgi için bkz: [Resource Manager şablonları](monitoring-create-activity-log-alerts-with-resource-manager-template.md).

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a>Azure portalı kullanarak bir uyarı yeni bir eylem grubu için bir hizmet sistem durumu bildirimi oluşturma
1. İçinde [portal](https://portal.azure.com)seçin **hizmet durumu**.

    !["Hizmet durumu" hizmeti](./media/monitoring-activity-log-alerts-on-service-notifications/home-servicehealth.png)

2. İçinde **uyarıları** bölümünde, select **sistem durumu uyarıları**.

    !["Sistem Durumu Uyarıları" sekmesi](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades-sh.png)

3. Seçin **oluşturma hizmeti sistem durumu Uyarısı** ve alanları doldurun.

    !["Oluştur hizmet Sistem Durumu Uyarısı" komutu](./media/monitoring-activity-log-alerts-on-service-notifications/service-health-alert.png)

4. Seçin **abonelik**, **Hizmetleri**, ve **bölgeleri** için uyarı almak istediğiniz.

    !["Etkinlik günlüğü uyarı Ekle" iletişim kutusu](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-new-ux.png)

> [!NOTE]
> Bu abonelik, etkinlik günlüğü uyarısı kaydetmek için kullanılır. Uyarı kaynağı bu aboneliğe dağıtılır ve etkinlik günlüğünde olayları izler.

5. Seçin **olay türleri** için uyarı almak istediğiniz: *hizmet sorunu*, *planlı Bakım*, ve *sistem durumu danışma* 

6. Uyarı ayrıntıları girerek tanımlamak bir **uyarı kuralı adı** ve **açıklama**.

7. Seçin **kaynak grubu** kaydedilmesi için uyarı almak istediğiniz.

8. Seçerek yeni bir eylem grubu oluşturmak **yeni eylem grubu**. Bir ad girin **eylem grup adı** kutu ve bir ad girin **kısa ad** kutusu. Kısa ad bu uyarı oluşturulduğunda, gönderilen bildirimleri başvuruluyor.

    ![Yeni bir eylem grubu oluşturun](./media/monitoring-activity-log-alerts-on-service-notifications/action-group-creation.png)

9. Alıcıları listesini alıcının sağlayarak tanımlayın:

    a. **Ad**: alıcının adını, diğer ad veya tanımlayıcı girin.

    b. **Eylem türü**: SMS seçin, e-posta, Web kancası, Azure uygulaması ve daha fazla.

    c. **Ayrıntılar**: seçilen eylem türüne bağlı olarak, bir telefon numarası, e-posta adresi, Web kancası URI, vb. girin.

10. Seçin **Tamam** eylem grubu oluşturmak ve ardından **oluşturma uyarı kuralı** Uyarınız tamamlamak için.

Birkaç dakika içinde uyarı etkindir ve oluşturma sırasında belirttiğiniz koşullara göre tetiklemek başlar.

Bilgi edinmek için nasıl [varolan sorunu yönetim sistemleri için Web kancası bildirimleri yapılandırmak](../service-health/service-health-alert-webhook-guide.md). Etkinlik günlüğü uyarıları için Web kancası şeması hakkında daha fazla bilgi için bkz: [Azure etkinlik için Web kancası oturum uyarıları](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Bu adımlarda tanımlanan eylem grubu gelecekteki tüm uyarı tanımları için var olan bir eylem grubu olarak yeniden kullanılabilir.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a>Azure portalı kullanarak bir uyarı varolan bir eylem grup için bir hizmet sistem durumu bildirimi oluşturma

1. 1 ile 7 hizmeti sistem durumu bildirimi oluşturmak için önceki bölümdeki adımları izleyin. 

2. Altında **tanımla eylem grubu**, tıklatın **seçme eylemi grup** düğmesi. Uygun eylem grubunu seçin.

3. Seçin **Ekle** eylem grubu eklemek için ve ardından **oluşturma uyarı kuralı** Uyarınız tamamlamak için.

Birkaç dakika içinde uyarı etkindir ve oluşturma sırasında belirttiğiniz koşullara göre tetiklemek başlar.

## <a name="manage-your-alerts"></a>Uyarılarınızı yönetme

Bir uyarı oluşturduktan sonra görünür **uyarıları** bölümünü **İzleyici**. Yönetmek istediğiniz uyarıyı seçin:

* Düzenleyin.
* Dosyayı silin.
* Geçici olarak durdurmak veya uyarı bildirimleri almaya devam etmek istiyorsanız, etkinleştirmek veya devre dışı.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [varolan sorunu yönetim sistemleri için Web kancası bildirimleri yapılandırmak](../service-health/service-health-alert-webhook-guide.md).
- Hakkında bilgi edinin [hizmet durumu bildirimlerine](monitoring-service-notifications.md).
- Hakkında bilgi edinin [bildirim hız sınırlaması](monitoring-alerts-rate-limiting.md).
- Gözden geçirme [etkinlik günlüğü uyarı Web kancası şeması](monitoring-activity-log-alerts-webhook.md).
- Alma bir [etkinlik günlüğü uyarıları genel bakış](monitoring-overview-alerts.md)ve uyarıların nasıl alınacağını öğrenin. 
- Daha fazla bilgi edinmek [Eylem grupları](monitoring-action-groups.md).
