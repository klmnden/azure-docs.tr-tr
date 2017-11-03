---
title: "Etkinlik günlüğü uyarıları hizmet bildirimleri alma | Microsoft Docs"
description: "Azure hizmet ortaya çıktığında, SMS, e-posta veya Web kancası aracılığıyla bilgi edinin."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: bf6a98fd7e7e11764bef174f9efd0635fa7efe9a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Etkinlik günlüğü Uyarıları hizmeti bildirimlerinin oluşturun.
## <a name="overview"></a>Genel Bakış
Bu makalede Azure portalını kullanarak hizmet durumu bildirimlerine için etkinlik günlüğü uyarıları ayarlama gösterilmiştir.  

Azure hizmet durumu bildirimlerine Azure aboneliğinize gönderdiğinde bir uyarı alabilirsiniz. Temel uyarı yapılandırabilirsiniz:

- Hizmeti sistem durumu bildirimi (olay, bakım, bilgi, vb.) sınıfı.
- Etkilenen Hizmet (ler).
- Etkilenen bilgiler.
- (Karşılaştırması çözümlenen etkin) bildirim durumu.
- (Bilgi, uyarı, hata) bildirimleri düzeyi.

Uyarı gönderen de yapılandırabilirsiniz:

- Var olan bir eylem grubunu seçin.
- (Bu uyarılar için kullanılabilir) yeni bir eylem grubu oluşturun.

Eylem grupları hakkında daha fazla bilgi için bkz: [oluşturma ve eylem gruplarını yönetme](monitoring-action-groups.md).

Azure Resource Manager şablonları kullanarak hizmet sistem durumu bildirimi uyarıları yapılandırma hakkında daha fazla bilgi için bkz: [Resource Manager şablonları](monitoring-create-activity-log-alerts-with-resource-manager-template.md).

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a>Azure portalı kullanarak bir uyarı yeni bir eylem grubu için bir hizmet sistem durumu bildirimi oluşturma
1. İçinde [portal](https://portal.azure.com)seçin **İzleyici**.

    !["İzleme" hizmeti](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2. İçinde **etkinlik günlüğü** bölümünde, select **uyarıları**.

    !["Uyarılar" sekmesi](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

3. Seçin **etkinlik günlüğü uyarı Ekle**ve alanları doldurun.

    !["Etkinlik günlüğü uyarı Ekle" komutu](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

4. Bir ad girin **etkinlik günlüğü uyarı adı** kutusuna ve sağlayan bir **açıklama**.

    !["Etkinlik günlüğü uyarı Ekle" iletişim kutusu](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

5. **Abonelik** kutusuna geçerli aboneliğiniz ile autofills. Bu abonelik, etkinlik günlüğü uyarısı kaydetmek için kullanılır. Uyarı kaynağı bu aboneliğe dağıtılır ve etkinlik günlüğünde olayları izler.

6. Seçin **kaynak grubu** uyarı kaynak oluşturulduğu içinde. Bu uyarı tarafından izlenen kaynak grubu değil. Bunun yerine, uyarı kaynağın bulunduğu kaynak grubu değil.

7. İçinde **olay kategorisi** kutusunda **hizmet durumu**. İsteğe bağlı olarak, seçin **hizmet**, **bölge**, **türü**, **durum**, ve **düzeyi** hizmet sistem durumu almak istediğiniz bildirimleri.

8. Altında **aracılığıyla uyarı**seçin **yeni** eylem Grup düğmesi. Bir ad girin **eylem grup adı** kutu ve bir ad girin **kısa ad** kutusu. Kısa ad bu uyarı oluşturulduğunda, gönderilen bildirimleri başvuruluyor.

9. Alıcıları listesini alıcının sağlayarak tanımlayın:

    a. **Ad**: alıcının adını, diğer ad veya tanımlayıcı girin.

    b. **Eylem türü**: SMS seçin, e-posta veya Web kancası.

    c. **Ayrıntılar**: seçilen eylem türüne bağlı olarak, bir telefon numarası, e-posta adresi veya Web kancası URI girin.

10. Seçin **Tamam** uyarı oluşturmak için.

Birkaç dakika içinde uyarı etkindir ve oluşturma sırasında belirttiğiniz koşullara göre tetiklemek başlar.

Etkinlik günlüğü uyarıları için Web kancası şeması hakkında daha fazla bilgi için bkz: [Azure etkinlik için Web kancası oturum uyarıları](monitoring-activity-log-alerts-webhook.md).

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

Bir uyarı oluşturduktan sonra görünür **uyarıları** bölümünü **İzleyici** dikey. Yönetmek istediğiniz uyarıyı seçin:

* Düzenleyin.
* Dosyayı silin.
* Geçici olarak durdurmak veya uyarı bildirimleri almaya devam etmek istiyorsanız, etkinleştirmek veya devre dışı.

## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [hizmet durumu bildirimlerine](monitoring-service-notifications.md).
- Hakkında bilgi edinin [bildirim hız sınırlaması](monitoring-alerts-rate-limiting.md).
- Gözden geçirme [etkinlik günlüğü uyarı Web kancası şeması](monitoring-activity-log-alerts-webhook.md).
- Alma bir [etkinlik günlüğü uyarıları genel bakış](monitoring-overview-alerts.md)ve uyarıların nasıl alınacağını öğrenin. 
- Daha fazla bilgi edinmek [Eylem grupları](monitoring-action-groups.md).
