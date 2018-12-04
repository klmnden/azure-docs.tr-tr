---
title: Azure portalında Eylem grupları oluşturma ve yönetme
description: Azure portalında Eylem grupları oluşturma ve yönetme hakkında bilgi edinin.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 99444e6b7ab71a531a95245b8e5056127446dbeb
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52847771"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Azure portalında Eylem grupları oluşturma ve yönetme
## <a name="overview"></a>Genel Bakış ##
Bir eylem grubu, Azure aboneliğinin sahibi tarafından tanımlanan bildirim tercihleri koleksiyonudur. Azure İzleyici ve hizmet Sistem Durumu Uyarıları Eylem grupları uyarı tetiklendi kullanıcılara bildirmek için kullanın. Çeşitli uyarılar aynı eylem grubu veya kullanıcının gereksinimlerine bağlı olarak farklı eylem grupları kullanabilir.

Ne zaman bir eylem bir kişiye bildirim e-posta ile yapılandırılmış veya SMS kişinin kendisi belirten bir onay alırsınız / Filiz eylem grubuna eklendi.

Bu makalede, Azure portalında Eylem grupları oluşturma ve yönetme işlemini göstermektedir.

Her eylem aşağıdaki özelliklerinden oluşur:

* **Ad**: eylem grubu içinde benzersiz bir tanımlayıcı.  
* **Eylem türü**: gerçekleştirilecek eylem. Bir ses araması, SMS, e-posta gönderme verilebilir; veya otomatik eylemler çeşitli türlerde tetikleniyor. Bu makalenin devamındaki türleri bakın. 
* **Ayrıntılar**: göre değişiklik gösteren ilgili ayrıntıları *eylem türü*. 

Eylem grupları yapılandırmak için Azure Resource Manager şablonlarını kullanma hakkında daha fazla bilgi için bkz: [eylem grubu Resource Manager şablonları](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Azure portalını kullanarak bir eylem grubu oluşturma ##
1. İçinde [portalı](https://portal.azure.com)seçin **İzleyici**. **İzleyici** dikey penceresinde, tüm izleme ayarlarınızı ve tek bir görünümde verileri birleştirir.

    !["İzleme" hizmeti](./media/monitoring-action-groups/home-monitor.png)
1. Seçin **uyarılar** seçip **Eylem grupları yönetme**.

    ![Eylem grupları düğmesi yönetme](./media/monitoring-action-groups/manage-action-groups.png)
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

## <a name="manage-your-action-groups"></a>Eylem grupları yönetme ##
Bir eylem grubu oluşturduktan sonra görünür **Eylem grupları** bölümünü **İzleyici** dikey penceresi. Yönetmek istediğiniz eylem grubu seçin:

* Ekleme, düzenleme veya eylemleri kaldırın.
* Eylem grubunu silin.

## <a name="action-specific-information"></a>Özel eylem bilgileri
**Azure uygulaması anında iletme** -en fazla 10 Azure uygulama eylemleri bir eylem grubu içinde olabilir. Şu anda Azure uygulama eylemi yalnızca ServiceHealth uyarıları da destekler. Başka bir uyarı zaman göz ardı edilir. Bkz: [hizmet durumu bildirimi gönderilen her uyarıları yapılandırma](monitoring-activity-log-alerts-on-service-notifications.md).

**E-posta** -aşağıdaki e-posta adreslerinden e-postalar gönderilecek. E-posta filtreleme uygun şekilde yapılandırıldığından emin olun
   - azure-noreply@microsoft.com
   - azureemail-noreply@microsoft.com
   - alerts-noreply@mail.windowsazure.com

Bir eylem grubu 1000 adede kadar e-posta eylemleri olabilir. Bkz: [bilgileri sınırlama oranı](./monitoring-alerts-rate-limiting.md) makale

**ITSM** -en fazla 10 olabilir bir eylem grubu ITSM eylem ITSM eylemleri bir ITSM bağlantısı gerektirir. Oluşturmayı bir [ITSM bağlantısı](../log-analytics/log-analytics-itsmc-overview.md).

**Mantıksal uygulama** -10 adede kadar mantıksal uygulama eylemleri bir eylem grubu içinde olabilir

**İşlev uygulaması** -işlevi eylemleri v2 işlev uygulamalarını'uygulama "files" için "AzureWebJobsSecretStorageType" ayarı yapılandırmak için şu anda gerektiren işlevleri API aracılığıyla okurken yapılandırılan uygulamalar için işlev tuşlarını görmek [ Anahtar Yönetimi işlevleri V2'de değişiklikleri]( https://aka.ms/funcsecrets) daha fazla bilgi için.

**Runbook** -bir eylem grubu başvuru için en fazla 10 Runbook eylemleri olabilir [Azure abonelik hizmeti limitleri](../azure-subscription-service-limits.md) sınırları üzerinde Runbook yükler

**SMS** -görebileceği bir eylem grubu en fazla 10 SMS eylemler olabilir [bilgileri sınırlama oranı](./monitoring-alerts-rate-limiting.md) bakın makalesi [SMS uyarısı davranışı](monitoring-sms-alert-behavior.md) makale

**Ses** -10 adede kadar ses eylemleri bir eylem grubu içinde olabilir</dd>
Bkz: [bilgileri sınırlama oranı](./monitoring-alerts-rate-limiting.md) makale</dd>

**Web kancası** -10 adede kadar Web kancası eylemleri bir eylem grubu içinde olabilir. Yanıt 10 saniyedir logic - zaman aşımı süresi yeniden deneyin. Web kancası çağrısı olacaktır, 2 katı şu HTTP durum kodları, döndürülen en fazla yeniden deneme: 408, 429, 503, 504 veya HTTP uç noktasına yanıt vermiyor. İlk yeniden deneme 10 saniye sonra gerçekleşir. İkinci ve son yeniden deneme 100 saniye sonra gerçekleşir.

Kaynak IP adresi aralıkları
    - 13.106.57.181
    - 13.106.54.3
    - 13.106.54.19
    - 13.106.38.142
    - 13.106.38.148
    - 13.106.57.196

Değişiklikler yapılandırdığınız öneririz bu IP adresleri için güncelleştirmeleri almak için bir [hizmet durumu Uyarısı](./monitoring-service-notifications.md) Eylem grupları hizmeti hakkında bilgi veren bildirimleri için izler.


## <a name="next-steps"></a>Sonraki adımlar ##
* Daha fazla bilgi edinin [SMS uyarısı davranışı](monitoring-sms-alert-behavior.md).  
* Geçirmesine bir [etkinlik günlüğü uyarısı Web kancası şeması anlama](monitoring-activity-log-alerts-webhook.md).  
* Daha fazla bilgi edinin [ITSM Bağlayıcısı](../log-analytics/log-analytics-itsmc-overview.md)
* Daha fazla bilgi edinin [hız sınırlaması](monitoring-alerts-rate-limiting.md) Uyarılardaki.
* Alma bir [etkinlik günlüğü uyarılarına genel bakış](monitoring-overview-alerts.md)ve uyarıları alma hakkında bilgi edinin.  
* Bilgi edinmek için nasıl [hizmet durumu bildirimi gönderilen her uyarıları yapılandırma](monitoring-activity-log-alerts-on-service-notifications.md).
