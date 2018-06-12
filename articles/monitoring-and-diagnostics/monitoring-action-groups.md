---
title: Azure portalında eylem gruplarını oluşturma ve yönetme
description: Azure portalında eylem gruplarını oluşturma ve yönetme öğrenin.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/1/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 63216d56fb3acbb954086fbf026441e69073621e
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263074"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Azure portalında eylem gruplarını oluşturma ve yönetme
## <a name="overview"></a>Genel Bakış ##
Bir eylem grubu, kullanıcı tarafından tanımlanan bildirimi tercihlerinizi koleksiyonudur. Azure İzleyici ve hizmet durumu uyarıları, uyarı tetiklendiğinde, belirli bir eylemi grubunu kullanmak üzere yapılandırılır. Çeşitli uyarılar aynı eylem grup veya kullanıcının gereksinimlere bağlı olarak farklı eylem grupları kullanabilir.

Bu makalede, Azure portalında Eylem grupları oluşturmak ve yönetmek nasıl gösterilmektedir.

Her eylem aşağıdaki özellikleri oluşur:

* **Ad**: eylem grubu içinde benzersiz bir tanımlayıcı.  
* **Eylem türü**: bir sesli arama veya SMS gönder, bir e-posta Gönder bir Web kancası çağrısı bir ITSM aracı veri Gönder mantıksal uygulamayı çağırmak Azure uygulaması anında iletme bildirimi göndermek veya bir Otomasyon runbook'u çalıştırmak.
* **Ayrıntılar**: karşılık gelen telefon numarası, e-posta adresi, Web kancası URI veya ITSM bağlantı ayrıntıları.

Eylem grupları yapılandırmak için Azure Resource Manager şablonları kullanma hakkında daha fazla bilgi için bkz: [eylem Grup Resource Manager şablonları](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Azure portalını kullanarak bir eylem grubu oluşturma ##
1. İçinde [portal](https://portal.azure.com)seçin **İzleyici**. **İzleyici** dikey penceresinde, izleme ayarları ve verileri tek bir görünümde birleştirir.

    !["İzleme" hizmeti](./media/monitoring-action-groups/home-monitor.png)
2. İçinde **ayarları** bölümünde, select **Eylem grupları**.

    !["Eylem grupları" sekmesi](./media/monitoring-action-groups/action-groups-blade.png)
3. Seçin **eylem Grup Ekle**ve alanları doldurun.

    !["Eylem Grup Ekle" komutu](./media/monitoring-action-groups/add-action-group.png)
4. Bir ad girin **eylem grup adı** kutu ve bir ad girin **kısa ad** kutusu. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.

      ![Eylem Grup Ekle"iletişim kutusu](./media/monitoring-action-groups/action-group-define.png)

5. **Abonelik** kutusuna geçerli aboneliğiniz ile autofills. Bu abonelik eylem grubunu kaydedildiği adrestir.

6. Seçin **kaynak grubu** eylem grubunu kaydedildiği içinde.

7. Eylemlerin bir listesini, her eylemin sağlayarak tanımlayın:

    a. **Ad**: Bu eylem için benzersiz bir tanımlayıcı girin.

    b. **Eylem türü**: e-posta/SMS/itme/ses, mantıksal uygulama, Web kancası, ITSM ya da Otomasyon Runbook'u seçin.

    c. **Ayrıntılar**: eylem türüne bağlı olarak, bir telefon numarası, e-posta adresi, Web kancası URI, Azure uygulaması, ITSM bağlantı ya da Otomasyon runbook'u girin. ITSM eylem için ayrıca belirtin **iş öğesi** ve diğer alanlar ITSM aracınızı gerektirir.

8. Seçin **Tamam** eylem grubu oluşturmak için.

## <a name="action-specific-information"></a>Eylem belirli bilgileri
<dl>
<dt>Azure uygulaması anında iletme</dt>
<dd>Bir eylem grubunda en fazla 10 Azure uygulaması eylemler olabilir.</dd>
<dd>Şu anda Azure uygulaması eylem yalnızca ServiceHealth uyarıları destekler. Başka bir uyarı zaman yoksayılacak. Bkz: [hizmeti sistem durumu bildirimi gönderilen her uyarıları yapılandırmak](monitoring-activity-log-alerts-on-service-notifications.md).</dd>

<dt>E-posta</dt>
<dd>E-postalar aşağıdaki e-posta adreslerini gönderilir. E-posta filtreleme uygun şekilde yapılandırıldığından emin olun

    - azure-noreply@microsoft.com
    - azureemail-noreply@microsoft.com
    - alerts-noreply@mail.windowsazure.com
    
</dd>
<dd>Bir eylem grubunda en fazla 1000 e-posta eylemler olabilir</dd>
<dd>Bkz: [bilgileri sınırlama oranı](./monitoring-alerts-rate-limiting.md) makale</dd>

<dt>ITSM</dt>
<dd>Bir eylem grubunda en fazla 10 ITSM eylemler olabilir</dd>
<dd>ITSM eylemi ITSM bağlantı gerektirir. Oluşturmayı öğrenin bir [ITSM bağlantı](../log-analytics/log-analytics-itsmc-overview.md).</dd>

<dt>Mantıksal uygulama</dt>
<dd>Bir eylem grubunda en fazla 10 mantıksal uygulama eylemler olabilir</dd>

<dt>runbook</dt>
<dd>Bir eylem grubunda en fazla 10 Runbook eylemler olabilir</dd>

<dt>SMS</dt>
<dd>Bir eylem grubunda en fazla 10 SMS eylemler olabilir</dd>
<dd>Bkz: [bilgileri sınırlama oranı](./monitoring-alerts-rate-limiting.md) makale</dd>
<dd>Bkz: [SMS uyarı davranışı](monitoring-sms-alert-behavior.md) makale</dd>

<dt>Ses</dt>
<dd>Bir eylem grubunda en fazla 10 sesli eylemler olabilir</dd>
<dd>Bkz: [bilgileri sınırlama oranı](./monitoring-alerts-rate-limiting.md) makale</dd>

<dt>Web kancası</dt>
<dd>Bir eylem grubunda en fazla 10 Web kancası eylemleri olabilir
<dd>Bir yanıt 10 saniye için mantığı - zaman aşımı süresi yeniden deneyin. Web kancası araması 2 katından zaman aşağıdaki HTTP durum kodları döndürülür, maksimum yeniden deneme işlemi: 408 429, 503 504 veya HTTP uç noktası yanıt vermiyor. İlk yeniden deneme 10 saniye sonra gerçekleşir. İkinci ve son yeniden deneme 100 saniye sonra gerçekleşir.</dd>
</dl>

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
