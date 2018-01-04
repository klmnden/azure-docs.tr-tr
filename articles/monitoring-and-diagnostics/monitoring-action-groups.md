---
title: "Azure portalında eylem gruplarını oluşturma ve yönetme | Microsoft Docs"
description: "Azure portalında eylem gruplarını oluşturma ve yönetme öğrenin."
author: anirudhcavale
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
ms.date: 05/15/2017
ms.author: ancav
ms.openlocfilehash: 05775415e210333cf63565e7b5b554d014f6ba23
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Azure portalında eylem gruplarını oluşturma ve yönetme
## <a name="overview"></a>Genel Bakış ##
Bu makalede, Azure portalında Eylem grupları oluşturmak ve yönetmek nasıl gösterilmektedir.

Eylem gruplarıyla eylemlerin bir listesini yapılandırabilirsiniz. Etkinlik günlüğü uyarıları tanımlarken bu gruplar daha sonra kullanılabilir. Bu gruplar daha sonra aynı eylemleri etkinlik günlüğü uyarısı her tetiklenişinde alınır sağlama tanımlamak her etkinlik günlüğü uyarı tarafından yeniden kullanılabilir.

Bir eylem grubu 10 her eylem türünde olabilir. Her eylem aşağıdaki özellikleri oluşur:

* **Ad**: eylem grubu içinde benzersiz bir tanımlayıcı.  
* **Eylem türü**: bir SMS gönder, bir e-posta Gönder, bir Web kancası çağrı, bir ITSM aracı veri göndermek, bir Azure uygulaması çağrısı veya bir Otomasyon runbook'u çalıştırmak.
* **Ayrıntılar**: karşılık gelen telefon numarası, e-posta adresi, Web kancası URI veya ITSM bağlantı ayrıntıları.

Eylem grupları yapılandırmak için Azure Resource Manager şablonları kullanma hakkında daha fazla bilgi için bkz: [eylem Grup Resource Manager şablonları](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Azure portalını kullanarak bir eylem grubu oluşturma ##
1. İçinde [portal](https://portal.azure.com)seçin **İzleyici**. **İzleyici** dikey penceresinde, izleme ayarları ve verileri tek bir görünümde birleştirir.

    !["İzleme" hizmeti](./media/monitoring-action-groups/home-monitor.png)
2. İçinde **ayarları** bölümünde, select **Eylem grupları**.

    !["Eylem grupları" sekmesi](./media/monitoring-action-groups/action-groups-blade.png)
3. Seçin **eylem Grup Ekle**ve alanları doldurun.

    !["Eylem Grup Ekle" komutu](./media/monitoring-action-groups/add-action-group.png)
4. Bir ad girin **eylem grup adı** kutu ve bir ad girin **kısa ad** kutusu. Bu grubun kullanarak bildirimler gönderildiğinde kısa adı yerine bir tam eylem grup adı kullanılır.

      ![Eylem Grup Ekle"iletişim kutusu](./media/monitoring-action-groups/action-group-define.png)

5. **Abonelik** kutusuna geçerli aboneliğiniz ile autofills. Bu abonelik eylem grubunu kaydedildiği adrestir.

6. Seçin **kaynak grubu** eylem grubunu kaydedildiği içinde.

7. Eylemlerin bir listesini, her eylemin sağlayarak tanımlayın:

    a. **Ad**: Bu eylem için benzersiz bir tanımlayıcı girin.

    b. **Eylem türü**: SMS seçin, e-posta, Web kancası, Azure uygulaması, ITSM veya Otomasyon Runbook.

    c. **Ayrıntılar**: eylem türüne bağlı olarak, bir telefon numarası, e-posta adresi, Web kancası URI, Azure uygulaması, ITSM bağlantı ya da Otomasyon runbook'u girin. ITSM eylem için ayrıca belirtin **iş öğesi** ve diğer alanlar ITSM aracınızı gerektirir. 

   > [!NOTE]
   > ITSM eylemi ITSM bağlantı gerektirir. Oluşturmayı öğrenin bir [ITSM bağlantı](../log-analytics/log-analytics-itsmc-overview.md). ITSM eylem şu anda yalnızca etkinlik günlüğü uyarıları için çalışır. Diğer uyarı türleri için bu şu anda Hayır op eylemdir.

8. Seçin **Tamam** eylem grubu oluşturmak için.

## <a name="manage-your-action-groups"></a>Eylem gruplarınızı yönetme ##
Bir eylem grubu oluşturduktan sonra görünür **Eylem grupları** bölümünü **İzleyici** dikey. Yönetmek istediğiniz eylem grubunu seçin:

* Ekleme, düzenleme veya Eylemler kaldırma.
* Eylem grubunu silin.

## <a name="next-steps"></a>Sonraki adımlar ##
* Daha fazla bilgi edinmek [SMS uyarı davranış](monitoring-sms-alert-behavior.md).  
* Geçirmesine bir [etkinlik günlüğü uyarı Web kancası şeması anlama](monitoring-activity-log-alerts-webhook.md).  
* Daha fazla bilgi edinmek [ITSM bağlayıcı](../log-analytics/log-analytics-itsmc-overview.md)
* Daha fazla bilgi edinmek [hız sınırlaması](monitoring-alerts-rate-limiting.md) uyarılar hakkında. 
* Alma bir [etkinlik günlüğü uyarıları genel bakış](monitoring-overview-alerts.md)ve uyarıların nasıl alınacağını öğrenin.  
* Bilgi edinmek için nasıl [hizmeti sistem durumu bildirimi gönderilen her uyarıları yapılandırmak](monitoring-activity-log-alerts-on-service-notifications.md).
