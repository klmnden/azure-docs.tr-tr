---
title: Klasik etkinlik günlüğü uyarıları oluşturma
description: Etkinlik günlüğünde belirli olaylar meydana geldiğinde SMS, Web kancası ve e-posta bildirim.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/18/2017
ms.author: johnkem
ms.component: alerts
ms.openlocfilehash: 120fd3552ad36b3d19179f39ca95ce2b3ee2c2e6
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426627"
---
# <a name="create-activity-log-alerts-classic"></a>Etkinlik günlüğü uyarıları (Klasik) oluşturma

## <a name="overview"></a>Genel Bakış
Etkinlik günlüğü uyarıları uyarıda belirtilen koşulları karşılayan yeni bir etkinlik günlüğü olay meydana geldiğinde, etkinleştirme uyarılardır. Bir Azure Resource Manager şablonu kullanarak oluşturulabilir şekilde, Azure kaynaklarının değildirler. Bunlar ayrıca oluşturulabilir, güncelleştirilmiş veya Azure portalında silindi. Bu makale, etkinlik günlüğü uyarıları kavramları tanıtır. Bunu ardından, Azure portalında etkinlik günlüğü olayları hakkında bir uyarı ayarlamak için nasıl kullanılacağını gösterir.

> [!NOTE]

>  Yeni [uyarılar](monitoring-overview-unified-alerts.md) deneyimi yerine bu yordamı. Bu makalede, deneyiminiz için başvuru olarak sağlanır. [Daha fazla bilgi edinin](monitoring-activity-log-alerts-new-experience.md).

Genellikle, etkinlik günlüğü uyarıları, bildirimleri almak için oluşturma sırasında:

* Genellikle belirli bir kaynak grubu veya kaynakları için kapsamlı, Azure aboneliğinizdeki kaynaklar üzerinde belirli değişiklikleri oluşur. Örneğin, myProductionResourceGroup herhangi bir sanal makine silindiğinde, size bildirilmesini isteyebilirsiniz. Veya bir kullanıcıya, aboneliğinizdeki herhangi bir yeni rolü atanmışsa bildirilmesini isteyebilirsiniz.
* Hizmet sistem durumu olayı oluşur. Olaylar ve, aboneliğinizdeki kaynaklar uygulamak bakım olayları bildirimi hizmet durumu olayları içerir.

Her iki durumda da, olayların uyarının oluşturulduğu abonelik için yalnızca bir etkinlik günlüğü uyarısı izler.

Herhangi bir üst düzey özelliği için bir etkinlik günlüğü olayında JSON nesnesinde dayalı bir etkinlik günlüğü uyarısı yapılandırabilirsiniz. Ancak, portalda en yaygın seçenekleri gösterir:

- **Kategori**: yönetici, hizmet durumu, otomatik ölçeklendirme ve öneri. Daha fazla bilgi için [Azure etkinlik günlüğü'ne genel bakış](./monitoring-overview-activity-logs.md#categories-in-the-activity-log). Hizmet durumu olayları hakkında daha fazla bilgi için bkz: [etkinlik günlüğü uyarıları hizmet bildirimleri almak](./monitoring-activity-log-alerts-on-service-notifications.md).
- **Kaynak grubu**
- **Kaynak**
- **Kaynak türü**
- **İşlem adı**: işlem adı The Resource Manager Role-Based erişim denetimi.
- **Düzey**: (ayrıntılı, bilgi, uyarı, hata veya kritik) olay önem derecesi.
- **Durum**: olay, genellikle çalışmaya, başarısız veya başarılı durumu.
- **Olayı başlatan tarafından**: olarak da bilinen "çağırana." E-posta adresi veya işlemi gerçekleştiren kullanıcının Azure Active Directory tanımlayıcısı.

> [!NOTE]
> Kategori "Yönetici" olduğunda, uyarıyı yukarıdaki ölçütlerden en az bir belirtmeniz gerekir. Etkinlik günlüğünde her bir olay oluşturulduğunda etkinleştiren bir uyarı oluşturabilirsiniz.

Bir etkinlik günlüğü uyarısı etkinleştirildiğinde, Eylemler veya bildirimleri oluşturmak için bir eylem grubu kullanır. Bir eylem grubu yeniden kullanılabilir e-posta adresleri gibi bildirim alıcıları Web kancası URL'leri ya da SMS telefon numaralarını kümesidir. Alıcılar, merkezileştirme ve bildirim kanallarınızın grup için birden çok uyarı başvurulabilir. Etkinlik günlüğü uyarınızın tanımlamak için iki seçeneğiniz vardır. Şunları yapabilirsiniz:

* Etkinlik günlüğü uyarınızın var olan bir eylem grubu kullanın.
* Yeni bir eylem grubu oluşturun.

Eylem grupları hakkında daha fazla bilgi için bkz: [oluştur ve Eylem grupları Azure portalında yönetmek](monitoring-action-groups.md).

Hizmet durumu bildirimlerine hakkında daha fazla bilgi için bkz: [hizmet durumu bildirimlerini etkinlik günlük uyarılarını alırsınız](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="create-an-alert-classic-on-an-activity-log-event-with-a-new-action-group-by-using-the-azure-portal"></a>Bir uyarı (Klasik) yeni bir eylem grubu ile bir etkinlik günlüğü olayında Azure portalını kullanarak oluşturun
1. İçinde [portalı](https://portal.azure.com)seçin **İzleyici**.

    !["İzleme" hizmeti](./media/monitoring-activity-log-alerts/home-monitor.png)
1. İçinde **etkinlik günlüğü** bölümünden **uyarılar (Klasik)**.

    !["Uyarı" sekmesi](./media/monitoring-activity-log-alerts/alerts-blades.png)
1. Seçin **etkinlik günlüğü uyarısı Ekle**ve alanları doldurun.

1. Bir ad girin **etkinlik günlüğü uyarısı adı** kutusunda ve seçin bir **açıklama**.

    !["Etkinlik günlüğü uyarısı Ekle" komutu](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

1. **Abonelik** kutusunda autofills geçerli aboneliğiniz ile. Eylem grubu kaydedildiği bir aboneliktir. Uyarı kaynağı bu aboneliğe dağıtılır ve ondan etkinlik günlüğü olayları izler.

    !["Etkinlik günlüğü uyarısı Ekle" iletişim kutusu](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

1. Seçin **kaynak grubu** uyarı kaynağın oluşturulduğu. Bu uyarı tarafından izlenen bir kaynak grubu değil. Bunun yerine, Uyarı kaynağı bulunduğu kaynak grubunun olduğunu.

1. İsteğe bağlı olarak bir **olay kategorisi** gösterilen ek filtreler değiştirilecek. Yönetici olayları için ekleme filtreleri **kaynak grubu**, **kaynak**, **kaynak türü**, **işlem adı**, **Düzeyi**, **durumu**, ve **olayı başlatan tarafından**. Bu değerler, bu uyarının izlemesi gereken hangi olayların belirleyin.

    >[!NOTE]
    >Yukarıdaki ölçütlerden en az bir uyarıyı belirtmeniz gerekir. Etkinlik günlüğünde her bir olay oluşturulduğunda etkinleştiren bir uyarı oluşturabilirsiniz.
    >
    >

1. Bir ad girin **eylem grubu adı** kutu ve bir ad girin **kısa ad** kutusu. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.

1.  Eylemin sağlayarak eylemlerin bir listesini tanımlayın:

    a. **Ad**: eylemin adı, diğer adı veya tanımlayıcısı girin.

    b. **Eylem türü**: seçin, SMS, e-posta veya Web kancası.

    c. **Ayrıntılar**: eylem türüne bağlı olarak, bir telefon numarası, e-posta adresi veya Web kancası URI girin.

1.  Seçin **Tamam** uyarı oluşturmak için.

Uyarı tamamen yayılması ve ardından etkin hale gelmesi birkaç dakika sürer. Yeni olaylar uyarı ölçütü eşleştirdiğinizde tetikler.

Daha fazla bilgi için [etkinlik günlüğü uyarıları kullanılan Web kancası şeması anlamak](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Bu adımlarda tanımlanan eylem grubu gelecekteki tüm uyarı tanımları için var olan bir eylem grubu olarak yeniden kullanılabilir.
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-the-azure-portal"></a>Azure portalını kullanarak üzerinde var olan bir eylem grubu için bir etkinlik günlüğü olayında uyarı oluşturma
1. 1 ile 7, etkinlik günlüğü uyarısı oluşturmak için önceki bölümdeki adımları izleyin.

1. Altında **şu şekilde bildir**seçin **varolan** eylem grubu düğmesi. Var olan bir eylem grubu listesinden seçin.

1. Seçin **Tamam** uyarı oluşturmak için.

Uyarı tamamen yayılması ve ardından etkin hale gelmesi birkaç dakika sürer. Yeni olaylar uyarı ölçütü eşleştirdiğinizde tetikler.

## <a name="manage-your-alerts"></a>Uyarılarınızı yönetme

Bir uyarı oluşturduktan sonra İzleyici dikey penceresinin Uyarılar bölümünde görünür. Yönetmek istediğiniz uyarıyı seçin:

* Düzenleyin.
* Silin.
* Geçici olarak durdurmak veya uyarı bildirimleri almaya devam etmek istiyorsanız, etkinleştirmek veya devre dışı.

## <a name="next-steps"></a>Sonraki adımlar
- Alma bir [uyarılara genel bakış](monitoring-overview-alerts.md).
- Hakkında bilgi edinin [bildirim hız sınırlaması](monitoring-alerts-rate-limiting.md).
- Gözden geçirme [etkinlik günlüğü uyarısı Web kancası şeması](monitoring-activity-log-alerts-webhook.md).
- Daha fazla bilgi edinin [Eylem grupları](monitoring-action-groups.md).  
- Hakkında bilgi edinin [hizmet durumu bildirimlerini](monitoring-service-notifications.md).
- Oluşturma bir [aboneliğinizdeki tüm otomatik ölçeklendirme altyapısı işlemleri izlemek için etkinlik günlüğü Uyarısı](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).
- Oluşturma bir [aboneliğinizdeki tüm başarısız otomatik ölçeklendirme ölçek/ölçeğini işlemleri izlemek için etkinlik günlüğü Uyarısı](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).
