---
title: Etkinlik günlüğü uyarıları, Azure hizmet bildirimleri alma
description: SMS, e-posta veya Web kancası Azure hizmet meydana geldiğinde bildirim alın.
author: shawntabrizi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/09/2018
ms.author: shtabriz
ms.component: alerts
ms.openlocfilehash: 1cd82f7ffa9360dbc35f9c9d790df34355d9dd1a
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035722"
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Etkinlik günlüğü uyarıları hizmet bildirimlerinde oluşturma
## <a name="overview"></a>Genel Bakış
Bu makalede, Azure portalını kullanarak hizmet durumu bildirimlerine için etkinlik günlüğü uyarıları ayarlama işlemini göstermektedir.  

Azure, Azure aboneliğinizin hizmet durumu bildirimlerini gönderirken bir uyarı alabilirsiniz. Şuna bağlı olarak uyarıyı yapılandırabilirsiniz:

- Hizmet durumu bildirimi (hizmet sorunları, planlı bakımı, sistem durumu danışmanları) sınıfı.
- Etkilenen abonelik.
- Etkilenen hizmetler.
- Etkilenen olduğu bölgelerin.

> [!NOTE]
> Hizmet durumu bildirimlerine, sistem durumu olaylarını kaynakla ilgili bir uyarı göndermez.

Kimin uyarı göndermesi gerektiğini de yapılandırabilirsiniz:

- Var olan bir eylem grubu seçin.
- (Bu, gelecekteki uyarılar için kullanılabilir) yeni bir eylem grubu oluşturun.

Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](monitoring-action-groups.md).

Azure Resource Manager şablonları kullanarak hizmet durumu bildirimi uyarılarını yapılandırma hakkında daha fazla bilgi için bkz: [Resource Manager şablonları](monitoring-create-activity-log-alerts-with-resource-manager-template.md).

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a>Azure portalını kullanarak yeni bir eylem grubu için bir hizmet durumu bildirimi üzerinde uyarı oluşturma
1. İçinde [portalı](https://portal.azure.com)seçin **hizmet durumu**.

    !["Hizmet durumu" hizmeti](./media/monitoring-activity-log-alerts-on-service-notifications/home-servicehealth.png)

2. İçinde **uyarılar** bölümünden **sistem durumu uyarılarını**.

    !["Sistem Durumu Uyarıları" sekmesi](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades-sh.png)

3. Seçin **Oluştur hizmet durumu Uyarısı** ve alanları doldurun.

    !["Hizmet durumu uyarısı oluştur" komutu](./media/monitoring-activity-log-alerts-on-service-notifications/service-health-alert.png)

4. Seçin **abonelik**, **Hizmetleri**, ve **bölgeleri** için uyarı almak istediğiniz.

    !["Etkinlik günlüğü uyarısı Ekle" iletişim kutusu](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-new-ux.png)

> [!NOTE]
> Bu abonelik, etkinlik günlüğü uyarısı kaydetmek için kullanılır. Uyarı kaynağı bu aboneliğe dağıtılır ve etkinlik günlüğünde olayları izler.

5. Seçin **olay türleri** için uyarı almak istediğiniz: *hizmet sorunu*, *planlı Bakım*, ve *sistem durumu danışmanları* 

6. Girerek, uyarı ayrıntılarını tanımlama bir **uyarı kuralı adı** ve **açıklama**.

7. Seçin **kaynak grubu** kaydedilecek uyarı almak istediğiniz.

8. Yeni bir eylem grubu seçerek oluşturma **yeni eylem grubu**. Bir ad girin **eylem grubu adı** kutu ve bir ad girin **kısa ad** kutusu. Kısa ad bu uyarı tetiklendiğinde gönderilen bildirimleri başvuruluyor.

    ![Yeni bir eylem grubu oluştur](./media/monitoring-activity-log-alerts-on-service-notifications/action-group-creation.png)

9. Alıcıları listesi, alıcı sağlayarak tanımlayın:

    a. **Ad**: alıcının adını, diğer adı veya tanımlayıcısı girin.

    b. **Eylem türü**: seçin, SMS, e-posta, Web kancası, Azure uygulama ve daha fazla.

    c. **Ayrıntılar**: seçilen eylem türüne bağlı olarak, bir telefon numarası, e-posta adresi, Web kancası URI, vb. girin.

10. Seçin **Tamam** eylem grubu oluşturmaya ve ardından **uyarı kuralı oluştur** Uyarınız tamamlanması.

Birkaç dakika içinde uyarı etkin ve tetiklemek oluşturma sırasında belirttiğiniz koşullara göre başlar.

Bilgi edinmek için nasıl [mevcut sorun yönetim sistemleri için Web kancası bildirimleri yapılandırma](../service-health/service-health-alert-webhook-guide.md). Etkinlik günlüğü uyarıları için Web kancası şeması hakkında daha fazla bilgi için bkz: [günlük uyarıları Azure etkinlik için Web kancaları](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Bu adımlarda tanımlanan eylem grubu gelecekteki tüm uyarı tanımları için var olan bir eylem grubu olarak yeniden kullanılabilir.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a>Azure portalını kullanarak bir uyarı mevcut eylem grubu için bir hizmet durumu bildirimi oluşturun

1. 1 ile 7, hizmet durumu bildirimi oluşturmak için önceki bölümdeki adımları izleyin. 

2. Altında **tanımla eylem grubu**, tıklayın **seçme eylemini grubu** düğmesi. Uygun bir eylem grubu seçin.

3. Seçin **Ekle** eylem grubu eklemek ve ardından **uyarı kuralı oluştur** Uyarınız tamamlanması.

Birkaç dakika içinde uyarı etkin ve tetiklemek oluşturma sırasında belirttiğiniz koşullara göre başlar.

## <a name="manage-your-alerts"></a>Uyarılarınızı yönetme

Bir uyarı oluşturduktan sonra görünür **uyarılar** bölümünü **İzleyici**. Yönetmek istediğiniz uyarıyı seçin:

* Düzenleyin.
* Silin.
* Geçici olarak durdurmak veya uyarı bildirimleri almaya devam etmek istiyorsanız, etkinleştirmek veya devre dışı.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [mevcut sorun yönetim sistemleri için Web kancası bildirimleri yapılandırma](../service-health/service-health-alert-webhook-guide.md).
- Hakkında bilgi edinin [hizmet durumu bildirimlerini](monitoring-service-notifications.md).
- Hakkında bilgi edinin [bildirim hız sınırlaması](monitoring-alerts-rate-limiting.md).
- Gözden geçirme [etkinlik günlüğü uyarısı Web kancası şeması](monitoring-activity-log-alerts-webhook.md).
- Alma bir [etkinlik günlüğü uyarılarına genel bakış](monitoring-overview-alerts.md)ve uyarıları alma hakkında bilgi edinin. 
- Daha fazla bilgi edinin [Eylem grupları](monitoring-action-groups.md).
