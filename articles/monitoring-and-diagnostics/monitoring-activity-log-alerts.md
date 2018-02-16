---
title: "Etkinlik günlüğü uyarı oluşturma | Microsoft Docs"
description: "Etkinlik günlüğünde belirli olaylar meydana geldiğinde, SMS, Web kancası ve e-posta bildirilmesi."
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
ms.date: 08/03/2017
ms.author: johnkem
ms.openlocfilehash: c8a2ce3ca90895262e77c3895867d29c9d3530a2
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="create-activity-log-alerts"></a>Etkinlik günlüğü uyarı oluşturma

## <a name="overview"></a>Genel Bakış
Etkinlik günlüğü uyarıları uyarıda belirtilen koşullara uyan yeni bir etkinlik günlüğü olay ortaya çıktığında, etkinleştirme uyarılar. Bunlar Azure kaynaklarını; dolayısıyla bir Azure Resource Manager şablonu kullanılarak oluşturulabilir. Bunlar ayrıca oluşturulabilir, güncelleştirilmiş veya Azure portalında silindi. Bu makalede, etkinlik günlüğü uyarıları kavramları tanıtır. Bunu daha sonra etkinlik günlüğü olaylarını üzerinde bir uyarı ayarlamak için Azure Portalı'nı kullanmayı gösterir.

> [!NOTE]

>  [Uyarıları (Önizleme)](monitoring-overview-unified-alerts.md) şu anda oluşturma ve etkinliği günlüklerini yönetme Gelişmiş bir deneyim sunar.  [Daha fazla bilgi edinin](monitoring-activity-log-alerts-new-experience.md).

Genellikle, etkinlik bildirimleri almak için günlük uyarı oluşturma zaman:

* Belirli kaynaklar genellikle belirli kaynak grupları veya kaynakları için kapsamlı Azure aboneliğinizde değişiklikler. Örneğin, myProductionResourceGroup herhangi bir sanal makine silindiğinde bildirilmesini isteyebilirsiniz. Veya bir kullanıcıya, aboneliğinizdeki herhangi bir yeni rol atanmışsa bildirilmesini isteyebilirsiniz.
* Hizmet sistem durumu olayı oluşur. Hizmet sistem durumu olayları olaylar ve kaynakları uygulamak bakım olayları bildirimi içerir.

Her iki durumda da, uyarının oluşturulduğu abonelik olayları için yalnızca bir etkinlik günlüğü uyarı izler.

Herhangi bir üst düzey özelliği JSON nesnesinde bir etkinlik günlüğü olayı için temel bir etkinlik günlüğü uyarı yapılandırabilirsiniz. Ancak, portal en yaygın seçenekler gösterilmektedir:

- **Kategori**: yönetici, hizmet sistem durumu, otomatik ölçeklendirme ve öneri. Daha fazla bilgi için bkz: [Azure etkinlik günlüğü'ne genel bakış](./monitoring-overview-activity-logs.md#categories-in-the-activity-log). Hizmet sistem durumu olayları hakkında daha fazla bilgi için bkz: [etkinlik günlüğü Uyarıları hizmeti bildirimleri almak](./monitoring-activity-log-alerts-on-service-notifications.md).
- **Kaynak grubu**
- **Kaynak**
- **Kaynak türü**
- **İşlem adı**: The Resource Manager Role-Based erişim denetimi işlem adı.
- **Düzey**: (ayrıntılı bilgi, uyarı, hata veya kritik) olay önem derecesi.
- **Durum**: genellikle başlatıldı, başarısız veya başarılı oldu olayın durumu.
- **Olayı başlatan tarafından**: olarak da bilinen "çağırıcı." E-posta adresi veya işlemi gerçekleştiren kullanıcının Azure Active Directory tanıtıcısı.

> [!NOTE]
> Kategori "Yönetici" olduğunda, önceki ölçüt en az biri, Uyarınız belirtmeniz gerekir. Etkinlik günlüklerde her bir olay oluşturulduğunda etkinleştiren bir uyarı oluşturamazsınız.

Bir etkinlik günlüğü uyarı etkinleştirildiğinde, Eylemler veya bildirimleri oluşturmak için bir eylem grubu kullanır. Bir eylem grubu yeniden kullanılabilir e-posta adresleri gibi bildirim alıcıları Web kancası URL'leri ya da SMS telefon numaralarının kümesidir. Alıcılar merkezileştirmek ve bildirim kanallarını grup için birden çok uyarı başvurulabilir. Etkinlik günlüğü Uyarınız tanımlamak için iki seçeneğiniz vardır. Şunları yapabilirsiniz:

* Varolan bir eylem Grup etkinlik günlüğü Uyarınız kullanın.
* Yeni bir eylem grubu oluşturun.

Eylem grupları hakkında daha fazla bilgi için bkz: [oluşturma ve Eylem grupları Azure portalında yönetmek](monitoring-action-groups.md).

Hizmet durumu bildirimlerine hakkında daha fazla bilgi için bkz: [hizmet durumu bildirimlerine etkinlik günlüğü uyarılar alırsınız](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="create-an-alert-on-an-activity-log-event-with-a-new-action-group-by-using-the-azure-portal"></a>Azure Portalı'nı kullanarak yeni bir eylem grubu ile etkinlik günlüğü olay bir uyarı oluştur
1. İçinde [portal](https://portal.azure.com)seçin **İzleyici**.

    !["İzleme" hizmeti](./media/monitoring-activity-log-alerts/home-monitor.png)
2. İçinde **etkinlik günlüğü** bölümünde, select **uyarıları**.

    !["Uyarılar" sekmesi](./media/monitoring-activity-log-alerts/alerts-blades.png)
3. Seçin **etkinlik günlüğü uyarı Ekle**ve alanları doldurun.

4. Bir ad girin **etkinlik günlüğü uyarı adı** kutusunda ve seçin bir **açıklama**.

    !["Etkinlik günlüğü uyarı Ekle" komutu](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

5. **Abonelik** kutusuna geçerli aboneliğiniz ile autofills. Bu abonelik eylem grubunu kaydedildiği adrestir. Uyarı kaynağı bu aboneliğe dağıtılır ve etkinlik günlüğü olaylarını dışarı izler.

    !["Etkinlik günlüğü uyarı Ekle" iletişim kutusu](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

6. Seçin **kaynak grubu** uyarı kaynak oluşturulduğu içinde. Bu uyarı tarafından izlenen kaynak grubu değil. Bunun yerine, uyarı kaynağın bulunduğu kaynak grubu değil.

7. İsteğe bağlı olarak, seçin bir **olay kategorisi** gösterilen ek filtreler değiştirmek için. Yönetim olayları filtreler aşağıdakileri içerir **kaynak grubu**, **kaynak**, **kaynak türü**, **işlem adı**, **düzeyi**, **durum**, ve **olayı başlatan tarafından**. Bu değerleri bu uyarı izlemelidir hangi olayların tanımlayın.

    >[!NOTE]
    >Yukarıdaki ölçütlere en az biri, Uyarınız belirtmeniz gerekir. Etkinlik günlüklerde her bir olay oluşturulduğunda etkinleştiren bir uyarı oluşturamazsınız.
    >
    >

8. Bir ad girin **eylem grup adı** kutu ve bir ad girin **kısa ad** kutusu. Bu grubun kullanarak bildirimler gönderildiğinde kısa adı yerine bir tam eylem grup adı kullanılır.

9.  Eylemin sağlayarak eylemlerin bir listesini tanımlar:

    a. **Ad**: eylemin adı, diğer ad veya tanımlayıcı girin.

    b. **Eylem türü**: SMS seçin, e-posta veya Web kancası.

    c. **Ayrıntılar**: eylem türüne bağlı olarak, bir telefon numarası, e-posta adresi veya Web kancası URI girin.

10. Seçin **Tamam** uyarı oluşturmak için.

Uyarı tam olarak yaymak ve etkin hale birkaç dakika sürer. Yeni olaylar uyarının ölçütleri eşleştiğinde tetikler.

Daha fazla bilgi için bkz: [etkinlik günlüğü uyarıları kullanılan Web kancası şema anlamak](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Bu adımlarda tanımlanan eylem grubu gelecekteki tüm uyarı tanımları için var olan bir eylem grubu olarak yeniden kullanılabilir.
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-the-azure-portal"></a>Azure portalı kullanarak bir uyarı varolan bir eylem grup için bir etkinlik günlüğü olay oluşturma
1. 1 ile 7 etkinlik günlüğü uyarı oluşturmak için önceki bölümdeki adımları izleyin.

2. Altında **aracılığıyla bildir**seçin **varolan** eylem Grup düğmesi. Varolan bir eylem grubu listeden seçin.

3. Seçin **Tamam** uyarı oluşturmak için.

Uyarı tam olarak yaymak ve etkin hale birkaç dakika sürer. Yeni olaylar uyarının ölçütleri eşleştiğinde tetikler.

## <a name="manage-your-alerts"></a>Uyarılarınızı yönetme

Bir uyarı oluşturduktan sonra İzleyici dikey Uyarılar bölümünde görünür olur. Yönetmek istediğiniz uyarıyı seçin:

* Düzenleyin.
* Dosyayı silin.
* Geçici olarak durdurmak veya uyarı bildirimleri almaya devam etmek istiyorsanız, etkinleştirmek veya devre dışı.

## <a name="next-steps"></a>Sonraki adımlar
- Alma bir [uyarılar genel bakış](monitoring-overview-alerts.md).
- Hakkında bilgi edinin [bildirim hız sınırlaması](monitoring-alerts-rate-limiting.md).
- Gözden geçirme [etkinlik günlüğü uyarı Web kancası şeması](monitoring-activity-log-alerts-webhook.md).
- Daha fazla bilgi edinmek [Eylem grupları](monitoring-action-groups.md).  
- Hakkında bilgi edinin [hizmet durumu bildirimlerine](monitoring-service-notifications.md).
- Oluşturma bir [aboneliğinizi tüm otomatik ölçeklendirme motoru işlemleri izlemek için etkinlik günlüğü Uyarısı](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).
- Oluşturma bir [aboneliğinizi tüm başarısız otomatik ölçeklendirme ölçek/genişletme işlemleri izlemek için etkinlik günlüğü Uyarısı](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).
