---
title: Azure portalında eylem gruplarını oluşturma ve yönetme | Microsoft Docs
description: Azure portalında eylem gruplarını oluşturma ve yönetme öğrenin.
author: dkamstra
manager: chrad
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2018
ms.author: dukek
ms.openlocfilehash: e3185b8d8ce97ffd04188b2b49a457bd14d5c6c8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Azure portalında eylem gruplarını oluşturma ve yönetme
## <a name="overview"></a>Genel Bakış ##
Bu makalede, Azure portalında Eylem grupları oluşturmak ve yönetmek nasıl gösterilmektedir.

Eylem gruplarıyla eylemlerin bir listesini yapılandırabilirsiniz. Bu gruplar aynı eylemleri bir uyarı her tetiklenişinde alınır sağlama tanımlamak her uyarı tarafından kullanılabilir.

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
4. Bir ad girin **eylem grup adı** kutu ve bir ad girin **kısa ad** kutusu. Bu grubun kullanarak bildirimler gönderildiğinde kısa adı yerine bir tam eylem grup adı kullanılır.

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
<dd>Bir eylem grubunda en fazla 50 e-posta eylemler olabilir</dd>
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
<dd>Logic - yeniden deneme Web kancası Araması 3 kez zaman aşağıdaki HTTP durum kodları döndürülür, maksimum yeniden deneme işlemi: 408 429, 503, 504</dd>
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
