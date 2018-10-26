---
title: Azure İzleyicisi'nde etkinlik günlüğü uyarıları
description: SMS, Web kancası, SMS, e-posta ve daha fazlasını belirli olaylar etkinlik günlüğünde gerçekleştiğinde aracılığıyla bildirilir.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 6f007ca3aacb338c14bf481ee58407596c8290ad
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50091632"
---
# <a name="alerts-on-activity-log"></a>Etkinlik günlüğü uyarıları 

## <a name="overview"></a>Genel Bakış
Etkinlik günlüğü uyarıları uyarıda belirtilen koşulları karşılayan yeni bir etkinlik günlüğü olay meydana geldiğinde, etkinleştirme uyarılardır. Bir Azure Resource Manager şablonu kullanarak oluşturulabilir şekilde, Azure kaynaklarının değildirler. Bunlar ayrıca oluşturulabilir, güncelleştirilmiş veya Azure portalında silindi. Bu makale, etkinlik günlüğü uyarıları kavramları tanıtır. Bunu ardından, Azure portalında etkinlik günlüğü olayları hakkında bir uyarı ayarlamak için nasıl kullanılacağını gösterir. Kullanım hakkında daha fazla bilgi için bkz. [oluşturun ve etkinlik günlüğü Uyarıları yönetme](alert-activity-log.md).

> [!NOTE]
> Uyarılar **başlatamaz** oluşturulur ve olaylar için etkinlik günlüğü uyarısı kategorisinde

Genellikle, etkinlik günlüğü uyarıları, bildirimleri almak için oluşturma sırasında:

* Genellikle belirli bir kaynak grubu veya kaynakları için kapsamlı, Azure aboneliğinizdeki kaynaklar üzerinde belirli işlemler oluşur. Örneğin, myProductionResourceGroup herhangi bir sanal makine silindiğinde, size bildirilmesini isteyebilirsiniz. Veya bir kullanıcıya, aboneliğinizdeki herhangi bir yeni rolü atanmışsa bildirilmesini isteyebilirsiniz.
* Hizmet sistem durumu olayı oluşur. Olaylar ve, aboneliğinizdeki kaynaklar uygulamak bakım olayları bildirimi hizmet durumu olayları içerir.

Bir basit benzerleme, uyarı kuralları oluşturulabilir, etkinlik günlüğünde anlama koşullar için olan keşfedin veya aracılığıyla olayları filtrelemek için [Azure Portal'da etkinlik günlüğü](monitoring-overview-activity-logs.md#query-the-activity-log-in-the-azure-portal). Azure İzleyici - etkinlik günlüğü, bir filtre veya gerekli olay bulabilir ve ardından kullanarak bir uyarı oluşturma **etkinlik günlüğü uyarısı Ekle** düğmesi.

Her iki durumda da, olayların uyarının oluşturulduğu abonelik için yalnızca bir etkinlik günlüğü uyarısı izler.

Herhangi bir üst düzey özelliği için bir etkinlik günlüğü olayında JSON nesnesinde dayalı bir etkinlik günlüğü uyarısı yapılandırabilirsiniz. Daha fazla bilgi için [Azure etkinlik günlüğü'ne genel bakış](./monitoring-overview-activity-logs.md#categories-in-the-activity-log). Hizmet durumu olayları hakkında daha fazla bilgi için bkz: [etkinlik günlüğü uyarıları hizmet bildirimleri almak](./monitoring-activity-log-alerts-on-service-notifications.md). 

Etkinlik günlüğü uyarıları, birkaç ortak seçeneğiniz vardır:

- **Kategori**: yönetici, hizmet sistem durumu, otomatik ölçeklendirme, güvenlik, ilke ve öneri. 
- **Kapsam**: tek başına bir kaynak veya etkinlik günlüğü uyarısında tanımlanır kaynaklar kümesi. Kapsam bir etkinlik günlüğü uyarısı için çeşitli düzeylerde tanımlanabilir:
    - Kaynak düzeyi: Örneğin, belirli bir sanal makine için
    - Kaynak grubu düzeyi: Örneğin, tüm sanal makinelerin belirli bir kaynak grubu
    - Abonelik düzeyi: Örneğin, tüm sanal makineleri bir aboneliği (veya) bir Abonelikteki tüm kaynaklar
- **Kaynak grubu**: varsayılan olarak, uyarı kuralı kapsamında tanımlanan hedef aynı kaynak grubunda kaydedilir. Kullanıcı, uyarı kuralı nerede depolanması gereken kaynak grubunu da tanımlayabilirsiniz.
- **Kaynak türü**: Resource Manager ad uyarı hedefi tanımlı.

- **İşlem adı**: işlem adı The Resource Manager Role-Based erişim denetimi.
- **Düzey**: (ayrıntılı, bilgi, uyarı, hata veya kritik) olay önem derecesi.
- **Durum**: olay, genellikle çalışmaya, başarısız veya başarılı durumu.
- **Olayı başlatan tarafından**: olarak da bilinen "çağırana." E-posta adresi veya işlemi gerçekleştiren kullanıcının Azure Active Directory tanımlayıcısı.

> [!NOTE]
> Bir abonelikte en fazla 100 uyarı kuralları bir kapsamda ya da etkinliğin oluşturulabilir: tek bir kaynak, tüm kaynaklar kaynak grubuna (veya) tüm abonelik düzeyi.

Bir etkinlik günlüğü uyarısı etkinleştirildiğinde, Eylemler veya bildirimleri oluşturmak için bir eylem grubu kullanır. Bir eylem grubu yeniden kullanılabilir e-posta adresleri gibi bildirim alıcıları Web kancası URL'leri ya da SMS telefon numaralarını kümesidir. Alıcılar, merkezileştirme ve bildirim kanallarınızın grup için birden çok uyarı başvurulabilir. Etkinlik günlüğü uyarınızın tanımlamak için iki seçeneğiniz vardır. Şunları yapabilirsiniz:

* Etkinlik günlüğü uyarınızın var olan bir eylem grubu kullanın.
* Yeni bir eylem grubu oluşturun.

Eylem grupları hakkında daha fazla bilgi için bkz: [oluştur ve Eylem grupları Azure portalında yönetmek](monitoring-action-groups.md).


## <a name="next-steps"></a>Sonraki adımlar
- Alma bir [uyarılara genel bakış](monitoring-overview-alerts.md).
- Hakkında bilgi edinin [etkinlik günlüğü uyarıları oluşturup](alert-activity-log.md).
- Gözden geçirme [etkinlik günlüğü uyarısı Web kancası şeması](monitoring-activity-log-alerts-webhook.md).
- Hakkında bilgi edinin [hizmet durumu bildirimlerini](monitoring-service-notifications.md).

