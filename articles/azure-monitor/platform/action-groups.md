---
title: Azure portalında Eylem grupları oluşturma ve yönetme
description: Azure portalında Eylem grupları oluşturma ve yönetme hakkında bilgi edinin.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 4/12/2019
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 3d06024b7fa4356d4ad0e8b52c45c2ead62ef784
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60778385"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Azure portalında Eylem grupları oluşturma ve yönetme
## <a name="overview"></a>Genel Bakış ##
Bir eylem grubu, Azure aboneliğinin sahibi tarafından tanımlanan bildirim tercihleri koleksiyonudur. Azure İzleyici ve hizmet Sistem Durumu Uyarıları Eylem grupları uyarı tetiklendi kullanıcılara bildirmek için kullanın. Çeşitli uyarılar aynı eylem grubu veya kullanıcının gereksinimlerine bağlı olarak farklı eylem grupları kullanabilir. Bir abonelikte en fazla 2.000 Eylem grupları yapılandırabilirsiniz.

Bir kişinin e-posta veya SMS, eylem grubuna eklenmiş olan belirten bir onay aldıkları bildirmek için bir eylem yapılandırırsınız.

Bu makalede, Azure portalında Eylem grupları oluşturma ve yönetme işlemini göstermektedir.

Her eylem aşağıdaki özelliklerinden oluşur:

* **Ad**: Eylem grubu içinde benzersiz bir tanımlayıcı.  
* **Eylem türü**: Gerçekleştirilen eylem. Bir ses araması, SMS, e-posta gönderme verilebilir; veya otomatik eylemler çeşitli türlerde tetikleniyor. Bu makalenin devamındaki türleri bakın. 
* **Ayrıntılar**: Göre farklılık gösteren ilgili ayrıntıları *eylem türü*. 

Eylem grupları yapılandırmak için Azure Resource Manager şablonlarını kullanma hakkında daha fazla bilgi için bkz: [eylem grubu Resource Manager şablonları](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Azure portalını kullanarak bir eylem grubu oluşturma ##
1. İçinde [portalı](https://portal.azure.com)seçin **İzleyici**. **İzleyici** bölmesinde, tüm izleme ayarlarınızı ve tek bir görünümde verileri birleştirir.

    !["İzleme" hizmeti](./media/action-groups/home-monitor.png)
1. Seçin **uyarılar** seçip **Eylem grupları yönetme**.

    ![Eylem grupları düğmesi yönetme](./media/action-groups/manage-action-groups.png)
1. Seçin **eylem grubu Ekle**ve alanları doldurun.

    !["Eylem Grup Ekle" komutu](./media/action-groups/add-action-group.png)
1. Bir ad girin **eylem grubu adı** kutu ve bir ad girin **kısa ad** kutusu. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.

      ![Eylem grubu Ekle"iletişim kutusu](./media/action-groups/action-group-define.png)

1. **Abonelik** kutusunda autofills geçerli aboneliğiniz ile. Eylem grubu kaydedildiği bir aboneliktir.

1. Seçin **kaynak grubu** eylem grubu kaydedildiği içinde.

1. Eylemlerin bir listesini tanımlar. Her eylem için aşağıdakileri sağlar:

    a. **Ad**: Bu eylem için benzersiz bir tanımlayıcı girin.

    b. **Eylem türü**: E-posta/SMS/anında iletme/ses, mantıksal uygulama, Web kancası, ITSM veya Otomasyon Runbook'u seçin.

    c. **Ayrıntılar**: Eylem türüne bağlı olarak, bir telefon numarası, e-posta adresi, Web kancası URI'si, Azure uygulaması, ITSM bağlantısı veya Otomasyon runbook'u girin. ITSM eylemleri için ayrıca belirtin **iş öğesi** ve ITSM aracınız için gereken diğer alanları.

1. Seçin **Tamam** eylem grubunu oluşturmak için.

## <a name="manage-your-action-groups"></a>Eylem grupları yönetme ##
Bir eylem grubu oluşturduktan sonra görünür **Eylem grupları** bölümünü **İzleyici** bölmesi. Yönetmek istediğiniz eylem grubu seçin:

* Ekleme, düzenleme veya eylemleri kaldırın.
* Eylem grubunu silin.

## <a name="action-specific-information"></a>Özel eylem bilgileri
> [!NOTE]
> Bkz: [izleme için abonelik hizmeti limitleri](https://docs.microsoft.com/azure/azure-subscription-service-limits#monitor-limits) sayısal sınırlar aşağıdaki öğelerin her biri için.  

**Azure uygulaması anında iletme** -sınırlı sayıda Azure uygulaması eylemleri bir eylem grubu içinde olabilir. Şu anda Azure uygulama eylemi yalnızca ServiceHealth uyarıları da destekler. Herhangi bir uyarı türünü göz ardı edilir. Bkz: [hizmet durumu bildirimi gönderilen her uyarıları yapılandırma](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).

**E-posta** -aşağıdaki e-posta adreslerinden e-postalar gönderilecek. E-posta filtreleme uygun şekilde yapılandırıldığından emin olun
- azure-noreply@microsoft.com
- azureemail-noreply@microsoft.com
- alerts-noreply@mail.windowsazure.com

E-posta eylemleri sınırlı sayıda bir eylem grubu içinde olabilir. Bkz: [bilgileri sınırlama oranı](./../../azure-monitor/platform/alerts-rate-limiting.md) makale

**ITSM** -ITSM eylemleri sayısı sınırlı sınırlı sayıda bir eylem grubu içinde olabilir. ITSM eylemi bir ITSM bağlantısı gerektirir. Oluşturmayı bir [ITSM bağlantısı](../../azure-monitor/platform/itsmc-overview.md).

**Mantıksal uygulama** -mantıksal uygulama eylemleri sınırlı sayıda bir eylem grubu içinde olabilir.

**İşlev uygulaması** -işlevi "dosyaları" işlevi eylemleri v2 işlev uygulamalarını'uygulama ayarı "AzureWebJobsSecretStorageType" yapılandırmak için şu anda gerektiren işlevleri API aracılığıyla okurken yapılandırılan uygulamalar için anahtarları. Daha fazla bilgi için [işlevler V2'de anahtar yönetimi değişiklikleri]( https://aka.ms/funcsecrets).

**Runbook** -Runbook eylemleri sınırlı sayıda bir eylem grubu içinde olabilir. Başvurmak [Azure abonelik hizmeti limitleri](../../azure-subscription-service-limits.md) sınırları üzerinde Runbook yükler.

**SMS** -SMS eylemleri sınırlı sayıda bir eylem grubu içinde olabilir. Ayrıca bkz: [bilgileri sınırlama oranı](./../../azure-monitor/platform/alerts-rate-limiting.md) ve [SMS uyarısı davranışı](../../azure-monitor/platform/alerts-sms-behavior.md) diğer önemli bilgiler için. 

**Ses** -ses Eylemler sınırlı sayıda bir eylem grubu içinde olabilir. Bkz: [bilgileri sınırlama oranı](./../../azure-monitor/platform/alerts-rate-limiting.md) makalesi.

**Web kancası** -Web kancası eylemleri sınırlı sayıda bir eylem grubu içinde olabilir. Web kancaları, aşağıdaki kurallar kullanılarak yeniden denenir. Web kancası çağrısı denenir, 2 katı şu HTTP durum kodları, döndürülen en fazla: 408, 429, 503, 504 veya HTTP uç noktası yanıt vermez. İlk yeniden deneme 10 saniye sonra yapılır. İkinci yeniden 100 saniye sonra gerçekleşir. İki hatasından sonra herhangi bir eylem grubu uç noktası 30 dakikalığına çağırır. 

Kaynak IP adresi aralıkları
 - 13.72.19.232
 - 13.106.57.181
 - 13.106.54.3
 - 13.106.54.19
 - 13.106.38.142
 - 13.106.38.148
 - 13.106.57.196
 - 52.244.68.117
 - 51.4.138.199
 - 51.5.148.86
 - 51.5.149.19

Önerilir, bu IP adreslerine yapılan değişiklikler hakkında güncelleştirmeler almak için [eylem grupları hizmeti hakkında bilgi veren bildirimleri için izleyen bir hizmet durumu Uyarısı,. yapılandırma


## <a name="next-steps"></a>Sonraki adımlar ##

* Daha fazla bilgi edinin [SMS uyarısı davranışı](../../azure-monitor/platform/alerts-sms-behavior.md).  
* Geçirmesine bir [etkinlik günlüğü uyarısı Web kancası şeması anlama](../../azure-monitor/platform/activity-log-alerts-webhook.md).  
* Daha fazla bilgi edinin [ITSM Bağlayıcısı](../../azure-monitor/platform/itsmc-overview.md)
* Daha fazla bilgi edinin [hız sınırlaması](../../azure-monitor/platform/alerts-rate-limiting.md) Uyarılardaki.
* Alma bir [etkinlik günlüğü uyarılarına genel bakış](../../azure-monitor/platform/alerts-overview.md)ve uyarıları alma hakkında bilgi edinin.  
* Bilgi edinmek için nasıl [hizmet durumu bildirimi gönderilen her uyarıları yapılandırma](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).

