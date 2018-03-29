---
title: Etkinlik günlüğü uyarıları Azure hizmeti bildirimleri alma | Microsoft Docs
description: Azure hizmet ortaya çıktığında, SMS, e-posta veya Web kancası aracılığıyla bilgi edinin.
author: johnkemnetz
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: johnkem
ms.openlocfilehash: b4c4fdeb825bbcab54f074c5224140282a24d196
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
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

4. Bir ad girin **etkinlik günlüğü uyarı adı** kutusuna ve sağlayan bir **açıklama**.

    !["Etkinlik günlüğü uyarı Ekle" iletişim kutusu](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group-sh.png)

5. **Abonelik** kutusuna geçerli aboneliğiniz ile autofills. Bu abonelik, etkinlik günlüğü uyarısı kaydetmek için kullanılır. Uyarı kaynağı bu aboneliğe dağıtılır ve etkinlik günlüğünde olayları izler.

6. Seçin **kaynak grubu** uyarı kaynak oluşturulduğu içinde. Bu uyarı tarafından izlenen kaynak grubu değil. Bunun yerine, uyarı kaynağın bulunduğu kaynak grubu değil.

7. **Olay kategorisi** kutusu otomatik olarak ayarlandığında **hizmet durumu**. İsteğe bağlı olarak, seçin **hizmet**, **bölge**, ve **türü** almak istediğiniz hizmet durumu bildirimlerine biri.

8. Altında **aracılığıyla uyarı**seçin **yeni** eylem Grup düğmesi. Bir ad girin **eylem grup adı** kutu ve bir ad girin **kısa ad** kutusu. Kısa ad bu uyarı oluşturulduğunda, gönderilen bildirimleri başvuruluyor.

9. Alıcıları listesini alıcının sağlayarak tanımlayın:

    a. **Ad**: alıcının adını, diğer ad veya tanımlayıcı girin.

    b. **Eylem türü**: SMS seçin, e-posta, Web kancası, Azure uygulaması ve daha fazla.

    c. **Ayrıntılar**: seçilen eylem türüne bağlı olarak, bir telefon numarası, e-posta adresi, Web kancası URI, vb. girin.

10. Seçin **Tamam** uyarı oluşturmak için.

Birkaç dakika içinde uyarı etkindir ve oluşturma sırasında belirttiğiniz koşullara göre tetiklemek başlar.

Bilgi edinmek için nasıl [varolan sorunu yönetim sistemleri için Web kancası bildirimleri yapılandırmak](../service-health/service-health-alert-webhook-guide.md). Etkinlik günlüğü uyarıları için Web kancası şeması hakkında daha fazla bilgi için bkz: [Azure etkinlik için Web kancası oturum uyarıları](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Bu adımlarda tanımlanan eylem grubu gelecekteki tüm uyarı tanımları için var olan bir eylem grubu olarak yeniden kullanılabilir.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a>Azure portalı kullanarak bir uyarı varolan bir eylem grup için bir hizmet sistem durumu bildirimi oluşturma

1. 1 ile 7 hizmeti sistem durumu bildirimi oluşturmak için önceki bölümdeki adımları izleyin. 

2. Altında **aracılığıyla uyarı**seçin **varolan** eylem Grup düğmesi. Uygun eylem grubunu seçin.

3. Seçin **Tamam** uyarı oluşturmak için.

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
