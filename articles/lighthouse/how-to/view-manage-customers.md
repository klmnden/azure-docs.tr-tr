---
title: Müşteriler ve Azure portalında temsilci kaynakları görüntüleme ve yönetme
description: Azure'ı kullanarak bir hizmet sağlayıcısı kaynak yönetimi temsilcisi gibi tüm temsilci müşteri kaynaklar ve abonelikler müşterilerimin Azure portalında giderek görüntüleyebilirsiniz.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: acc90afa258fa7140cd7dfa8711dd64b554df45d
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809858"
---
# <a name="view-and-manage-customers-and-delegated-resources"></a>Müşteriler ve temsilci kaynakları görüntüleme ve yönetme

Hizmet sağlayıcıları kullanarak [Azure kaynak yönetimi temsilcisi](../concepts/azure-delegated-resource-management.md) kullanabilirsiniz **müşterilerimin** sayfasını [Azure portalında](https://portal.azure.com) temsilci müşteri kaynakları görüntülemek için ve Abonelikler. Hizmet sağlayıcıları ve burada müşterileri diyoruz, ancak birden çok kiracının yönetme kuruluşların yönetim deneyimlerinden birleştirmek için aynı işlem kullanabilirsiniz.

Erişim için **müşterilerimin** select Azure portalında sayfası **tüm hizmetleri**, öğesini arayın **müşterilerimin** ve bu seçeneği belirleyin. Bunu, "Müşterilerimin" arama kutusuna Azure portalının üst kısmındaki girerek da bulabilirsiniz.

Aklınızda **müşterilerimin** sayfası, yalnızca abonelik veya kaynak grupları temsilci müşterilerle ilgili bilgileri gösterir. Diğer müşteriler ile çalışıyorsanız (gibi aracılığıyla [bulut çözümü sağlayıcısı programı](https://docs.microsoft.com/partner-center/csp-overview), yerleşik kaynakları için temsilci sürece hakkında bilgi müşteriler burada göremezsiniz kaynak yönetimi.

> [!NOTE]
> Müşterilerinizin giderek hizmet sağlayıcıları hakkında daha fazla bilgi görüntüleyebilirsiniz **hizmeti sağlayıcıları** Azure portalında. Daha fazla bilgi için bkz. [görünümü ve hizmet sağlayıcılarını Yönet](view-manage-service-providers.md).

## <a name="view-and-manage-customer-details"></a>Görüntüleme ve yönetme Müşteri ayrıntıları

Müşteri ayrıntıları görüntülemek için seçin **müşteriler** sol tarafındaki **müşterilerimin** sayfası.

Her müşteri için müşterinin adı, Müşteri Kimliği (Kiracı kimliği) ve etkileşimini ilişkili teklif görürsünüz. İçinde **temsilcileri** sütun, temsilci olarak atanan abonelik sayısını ve/veya temsilci kaynak gruplarının sayısını görürsünüz.

Sayfanın üst kısmındaki filtre, sıralama ve grup müşteri bilgileri veya belirli müşterilerin, teklifleri veya anahtar sözcükleri göre filtrele olanak tanır.

Bu sayfadan aşağıdaki bilgileri görüntüleyebilirsiniz:

- Tüm abonelikleri görmek için sunar ve müşteriyle ilişkili temsilcileri müşterinin adı seçin.
- Bir teklif ve kendi temsilcileri hakkında daha fazla ayrıntı görmek için teklif adını seçin.
- Temsilci Abonelikleriniz veya kaynak grupları için acrolecess atamaları hakkında daha fazla ayrıntı görüntülemek için girişi seçin **temsilcileri** sütun.

## <a name="view-delegations"></a>Görünüm temsilciler

Temsilciler, kullanıcılar ve erişimi olan izinler birlikte devredildi abonelik/kaynak grubu gösterir. Bu bilgileri görüntülemek için seçin **temsilcileri** sol tarafındaki **müşterilerimin** sayfası.

Sayfanın üstündeki filtreleri, sıralama ve grup erişimi atama bilgileri veya belirli müşterilerin, teklifleri veya anahtar sözcükleri göre filtrele olanak tanır.

Kullanıcılar ve izinler her temsilci ile ilişkili görünür **rol atamaları** sütun. Kullanıcılara, gruplara veya abonelik veya kaynak grubuna erişim verilmiş olan hizmet sorumlularını tam listesini görüntülemek için her girişin seçebilirsiniz. Burada, belirli kullanıcı, Grup veya hizmet asıl adı daha ayrıntılı bilgi edinmek için seçebilirsiniz.

## <a name="work-in-the-context-of-a-delegated-subscription"></a>Bir temsilci abonelik bağlamında çalışır.

Doğrudan Azure portalındaki yetkilendirilmiş bir abonelik bağlamında, çalışmakta olduğunuz dizine geçiş olmadan çalışabilir. Bunu yapmak için:

1. Seçin **dizin + abonelik** Azure portalının üst kısmındaki simgesi.
2. İçinde **genel abonelik** filtrelemek için söz konusu yetkilendirilmiş abonelik seçilirse yalnızca kutunun emin olun. Kullanabileceğiniz **geçerli + temsilci dizinleri** yalnızca belirli bir dizin aboneliklerini göstermek için aşağı açılan kutusu. (Kullanmayın **dizini Değiştir** olduğu açtınız directory değişiklikler olduğundan seçeneği.)

Ardından destekleyen bir hizmet erişim [kiracılar arası yönetim deneyimleri](../concepts/cross-tenant-management-experience.md), hizmet seçtiğiniz temsilci abonelik bağlamına varsayılan olur. Yukarıdaki ve denetimi adımları izleyerek bu ayarı değiştirebilirsiniz **Tümünü Seç** kutusunu (veya bunun yerine iş için bir veya daha fazla abonelik seçme).

> [!NOTE]
> Bir veya daha fazla kaynak gruplarına erişimi yerine tüm bir aboneliğe erişim verilmiş, bu kaynak grubuna ait olduğu abonelik seçebilirsiniz. Bu abonelik bağlamında çalışmak, ancak yalnızca belirtilen kaynak gruplarını erişmek mümkün olacaktır.

Temsilci aboneliklerini ya da abonelik veya kaynak grubundan hizmet içinde seçerek kiracılar arası yönetim deneyimleri Destek Hizmetleri içindeki kaynak gruplarınızdaki ilgili işlevselliği de erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [kiracılar arası yönetim deneyimleri](../concepts/cross-tenant-management-experience.md).
- Müşterilerinize nasıl edinebilirsiniz [görüntüleyin ve hizmet sağlayıcılarını Yönet](view-manage-service-providers.md) giderek **hizmeti sağlayıcıları** Azure portalında.
