---
title: Azure İzleyicisi'nde etkinlik günlüğü uyarıları
description: SMS, Web kancası, SMS, e-posta ve daha fazlasını belirli olaylar etkinlik günlüğünde gerçekleştiğinde aracılığıyla bildirilir.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 5d0819f71405b1bf1d4bef57a8b93d57bc879087
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244980"
---
# <a name="alerts-on-activity-log"></a>Etkinlik günlüğü uyarıları 

## <a name="overview"></a>Genel Bakış
Etkinlik günlüğü uyarıları uyarıda belirtilen koşulları karşılayan yeni bir etkinlik günlüğü olay meydana geldiğinde, etkinleştirme uyarılardır. Bir Azure Resource Manager şablonu kullanarak oluşturulabilir şekilde, Azure kaynaklarının değildirler. Bunlar ayrıca oluşturulabilir, güncelleştirilmiş veya Azure portalında silindi. Bu makale, etkinlik günlüğü uyarıları kavramları tanıtır. Bunu ardından, Azure portalında etkinlik günlüğü olayları hakkında bir uyarı ayarlamak için nasıl kullanılacağını gösterir. Kullanım hakkında daha fazla bilgi için bkz. [oluşturun ve etkinlik günlüğü Uyarıları yönetme](alerts-activity-log.md).

> [!NOTE]
> Uyarılar **olamaz** etkinlik günlüğü uyarısı kategorisinde oluşturulan olaylar için.

Genellikle, etkinlik günlüğü uyarıları, bildirimleri almak için oluşturma sırasında:

* Genellikle belirli bir kaynak grubu veya kaynakları için kapsamlı, Azure aboneliğinizdeki kaynaklar üzerinde belirli işlemler oluşur. Örneğin, myProductionResourceGroup herhangi bir sanal makine silindiğinde, size bildirilmesini isteyebilirsiniz. Veya bir kullanıcıya, aboneliğinizdeki herhangi bir yeni rolü atanmışsa bildirilmesini isteyebilirsiniz.
* Hizmet sistem durumu olayı oluşur. Olaylar ve, aboneliğinizdeki kaynaklar uygulamak bakım olayları bildirimi hizmet durumu olayları içerir.

Bir basit benzerleme, uyarı kuralları oluşturulabilir, etkinlik günlüğünde anlama koşullar için olan keşfedin veya aracılığıyla olayları filtrelemek için [Azure portalında etkinlik günlüğü](activity-log-view.md#azure-portal). Azure İzleyici - etkinlik günlüğü, bir filtre veya gerekli olay bulabilir ve ardından kullanarak bir uyarı oluşturma **etkinlik günlüğü uyarısı Ekle** düğmesi.

Her iki durumda da, olayların uyarının oluşturulduğu abonelik için yalnızca bir etkinlik günlüğü uyarısı izler.

Herhangi bir üst düzey özelliği için bir etkinlik günlüğü olayında JSON nesnesinde dayalı bir etkinlik günlüğü uyarısı yapılandırabilirsiniz. Daha fazla bilgi için [Azure etkinlik günlüğü'ne genel bakış](./activity-logs-overview.md#categories-in-the-activity-log). Hizmet durumu olayları hakkında daha fazla bilgi için bkz: [etkinlik günlüğü uyarıları hizmet bildirimleri almak](./alerts-activity-log-service-notifications.md). 

Etkinlik günlüğü uyarıları, birkaç ortak seçeneğiniz vardır:

- **Kategori**: Yönetici, hizmet sistem durumu, otomatik ölçeklendirme, güvenlik, ilke ve öneri. 
- **Kapsam**: Tek başına bir kaynak veya etkinlik günlüğü uyarısında tanımlandığı kaynaklar kümesi. Kapsam bir etkinlik günlüğü uyarısı için çeşitli düzeylerde tanımlanabilir:
    - Kaynak düzeyi: Örneğin, belirli bir sanal makine için
    - Kaynak grubu düzeyi: Örneğin, tüm sanal makinelerin belirli bir kaynak grubu
    - Abonelik düzeyi: Örneğin, tüm sanal makineleri bir aboneliği (veya) bir Abonelikteki tüm kaynaklar
- **Kaynak grubu**: Varsayılan olarak, uyarı kuralı kapsamında tanımlanan hedef aynı kaynak grubunda kaydedilir. Kullanıcı, uyarı kuralı nerede depolanması gereken kaynak grubunu da tanımlayabilirsiniz.
- **Kaynak türü**: Kaynak Yöneticisi uyarı hedef ad alanı tanımlı.

- **İşlem adı**: Resource Manager rol tabanlı erişim denetimi işlem adı.
- **Düzey**: (Ayrıntılı, bilgi, uyarı, hata veya kritik) olay önem derecesi.
- **Durum**: Olay, genellikle çalışmaya, başarısız veya başarılı durumu.
- **Olayı başlatan tarafından**: Olarak da bilinen "çağırana." E-posta adresi veya işlemi gerçekleştiren kullanıcının Azure Active Directory tanımlayıcısı.

> [!NOTE]
> Bir abonelikte en fazla 100 uyarı kuralları bir kapsamda ya da etkinliğin oluşturulabilir: tek bir kaynak, tüm kaynaklar kaynak grubuna (veya) tüm abonelik düzeyi.

Bir etkinlik günlüğü uyarısı etkinleştirildiğinde, Eylemler veya bildirimleri oluşturmak için bir eylem grubu kullanır. Bir eylem grubu yeniden kullanılabilir e-posta adresleri gibi bildirim alıcıları Web kancası URL'leri ya da SMS telefon numaralarını kümesidir. Alıcılar, merkezileştirme ve bildirim kanallarınızın grup için birden çok uyarı başvurulabilir. Etkinlik günlüğü uyarınızın tanımlamak için iki seçeneğiniz vardır. Şunları yapabilirsiniz:

* Etkinlik günlüğü uyarınızın var olan bir eylem grubu kullanın.
* Yeni bir eylem grubu oluşturun.

Eylem grupları hakkında daha fazla bilgi için bkz: [oluştur ve Eylem grupları Azure portalında yönetmek](action-groups.md).


## <a name="next-steps"></a>Sonraki adımlar
- Alma bir [uyarılara genel bakış](alerts-overview.md).
- Hakkında bilgi edinin [etkinlik günlüğü uyarıları oluşturup](alerts-activity-log.md).
- Gözden geçirme [etkinlik günlüğü uyarısı Web kancası şeması](activity-log-alerts-webhook.md).
- Hakkında bilgi edinin [hizmet durumu bildirimlerini](service-notifications.md).


