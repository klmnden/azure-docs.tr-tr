---
title: Azure portalında Eylem grupları oluşturma ve yönetme
description: Azure portalında Eylem grupları oluşturma ve yönetme hakkında bilgi edinin.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/1/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 091a097fc9fafd5bdc6a2521f4fa2a1b6b77ba4c
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39422562"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Azure portalında Eylem grupları oluşturma ve yönetme
## <a name="overview"></a>Genel Bakış ##
Bir eylem grubu kullanıcı tarafından tanımlanan bildirim tercihleri koleksiyonudur. Azure İzleyici ve hizmet Sistem Durumu Uyarıları, uyarı tetiklendiğinde, belirli bir eylem grubu kullanmak için yapılandırılır. Çeşitli uyarılar aynı eylem grubu veya kullanıcının gereksinimlerine bağlı olarak farklı eylem grupları kullanabilir.

Bu makalede, Azure portalında Eylem grupları oluşturma ve yönetme işlemini göstermektedir.

Her eylem aşağıdaki özelliklerinden oluşur:

* **Ad**: eylem grubu içinde benzersiz bir tanımlayıcı.  
* **Eylem türü**: bir sesli arama veya SMS gönderebilen, bir e-posta Gönder Web kancası çağırma bir ITSM aracına veri gönderme mantıksal uygulamayı çağırın Azure uygulamasına anında iletme bildirimi gönderin veya Otomasyon runbook'u çalıştırın.
* **Ayrıntılar**: karşılık gelen telefon numarası, e-posta adresi, Web kancası URI veya ITSM bağlantı ayrıntıları.

Eylem grupları yapılandırmak için Azure Resource Manager şablonlarını kullanma hakkında daha fazla bilgi için bkz: [eylem grubu Resource Manager şablonları](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Azure portalını kullanarak bir eylem grubu oluşturma ##
1. İçinde [portalı](https://portal.azure.com)seçin **İzleyici**. **İzleyici** dikey penceresinde, tüm izleme ayarlarınızı ve tek bir görünümde verileri birleştirir.

    !["İzleme" hizmeti](./media/monitoring-action-groups/home-monitor.png)
1. İçinde **ayarları** bölümünden **Eylem grupları**.

    !["Eylem grupları" sekmesi](./media/monitoring-action-groups/action-groups-blade.png)
1. Seçin **eylem grubu Ekle**ve alanları doldurun.

    !["Eylem Grup Ekle" komutu](./media/monitoring-action-groups/add-action-group.png)
1. Bir ad girin **eylem grubu adı** kutu ve bir ad girin **kısa ad** kutusu. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.

      ![Eylem grubu Ekle"iletişim kutusu](./media/monitoring-action-groups/action-group-define.png)

1. **Abonelik** kutusunda autofills geçerli aboneliğiniz ile. Eylem grubu kaydedildiği bir aboneliktir.

1. Seçin **kaynak grubu** eylem grubu kaydedildiği içinde.

1. Eylemlerin bir listesini, her eylemin sağlayarak tanımlayın:

    a. **Ad**: Bu eylem için benzersiz bir tanımlayıcı girin.

    b. **Eylem türü**: e-posta/SMS/anında iletme/ses, mantıksal uygulama, Web kancası, ITSM veya Otomasyon Runbook'u seçin.

    c. **Ayrıntılar**: eylem türüne bağlı olarak, bir telefon numarası, e-posta adresi, Web kancası URI'si, Azure uygulaması, ITSM bağlantısı veya Otomasyon runbook'u girin. ITSM eylemleri için ayrıca belirtin **iş öğesi** ve ITSM aracınız için gereken diğer alanları.

1. Seçin **Tamam** eylem grubunu oluşturmak için.

## <a name="action-specific-information"></a>Özel eylem bilgileri
<dl>
<dt>Azure uygulama iletimi</dt>
<dd>En fazla 10 Azure uygulama eylemleri bir eylem grubu içinde olabilir.</dd>
<dd>Şu anda Azure uygulama eylemi yalnızca ServiceHealth uyarıları da destekler. Başka bir uyarı zaman göz ardı edilir. Bkz: [hizmet durumu bildirimi gönderilen her uyarıları yapılandırma](monitoring-activity-log-alerts-on-service-notifications.md).</dd>

<dt>E-posta</dt>
<dd>Aşağıdaki e-posta adreslerinden e-postalar gönderilir. E-posta filtreleme uygun şekilde yapılandırıldığından emin olun

    - azure-noreply@microsoft.com
    - azureemail-noreply@microsoft.com
    - alerts-noreply@mail.windowsazure.com
    
</dd>
<dd>Bir eylem grubu 1000 adede kadar e-posta eylemleri olabilir</dd>
<dd>Bkz: [bilgileri sınırlama oranı](./monitoring-alerts-rate-limiting.md) makale</dd>

<dt>ITSM</dt>
<dd>Bir eylem grubunda en fazla 10 ITSM eylemleri olabilir</dd>
<dd>ITSM eylemi bir ITSM bağlantısı gerektirir. Oluşturmayı bir [ITSM bağlantısı](../log-analytics/log-analytics-itsmc-overview.md).</dd>

<dt>Mantıksal uygulama</dt>
<dd>10 adede kadar mantıksal uygulama eylemleri bir eylem grubu içinde olabilir</dd>

<dt>Runbook</dt>
<dd>Bir eylem grubunda en fazla 10 Runbook eylemler olabilir</dd>

<dt>SMS</dt>
<dd>Bir eylem grubunda en fazla 10 SMS eylemler olabilir</dd>
<dd>Bkz: [bilgileri sınırlama oranı](./monitoring-alerts-rate-limiting.md) makale</dd>
<dd>Bkz: [SMS uyarısı davranışı](monitoring-sms-alert-behavior.md) makale</dd>

<dt>Ses</dt>
<dd>Bir eylem grubu içinde 10 adede kadar ses eylemler olabilir</dd>
<dd>Bkz: [bilgileri sınırlama oranı](./monitoring-alerts-rate-limiting.md) makale</dd>

<dt>Web kancası</dt>
<dd>10 adede kadar Web kancası eylemleri bir eylem grubu içinde olabilir
<dd>Yanıt 10 saniyedir logic - zaman aşımı süresi yeniden deneyin. Web kancası çağrısı olacaktır, 2 katı şu HTTP durum kodları, döndürülen en fazla yeniden deneme: 408, 429, 503, 504 veya HTTP uç noktasına yanıt vermiyor. İlk yeniden deneme 10 saniye sonra gerçekleşir. İkinci ve son yeniden deneme 100 saniye sonra gerçekleşir.</dd>
</dl>

## <a name="manage-your-action-groups"></a>Eylem grupları yönetme ##
Bir eylem grubu oluşturduktan sonra görünür **Eylem grupları** bölümünü **İzleyici** dikey penceresi. Yönetmek istediğiniz eylem grubu seçin:

* Ekleme, düzenleme veya eylemleri kaldırın.
* Eylem grubunu silin.

## <a name="next-steps"></a>Sonraki adımlar ##
* Daha fazla bilgi edinin [SMS uyarısı davranışı](monitoring-sms-alert-behavior.md).  
* Geçirmesine bir [etkinlik günlüğü uyarısı Web kancası şeması anlama](monitoring-activity-log-alerts-webhook.md).  
* Daha fazla bilgi edinin [ITSM Bağlayıcısı](../log-analytics/log-analytics-itsmc-overview.md)
* Daha fazla bilgi edinin [hız sınırlaması](monitoring-alerts-rate-limiting.md) Uyarılardaki.
* Alma bir [etkinlik günlüğü uyarılarına genel bakış](monitoring-overview-alerts.md)ve uyarıları alma hakkında bilgi edinin.  
* Bilgi edinmek için nasıl [hizmet durumu bildirimi gönderilen her uyarıları yapılandırma](monitoring-activity-log-alerts-on-service-notifications.md).
