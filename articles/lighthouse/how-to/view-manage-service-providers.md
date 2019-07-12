---
title: Görüntüleme ve Azure portalında hizmet sağlayıcılarını Yönet
description: Müşteriler Azure portalında hizmet sağlayıcıları, hizmet sağlayıcısı teklifleri ve atanan kaynaklar hakkında bilgileri görüntülemek için hizmet sağlayıcıları sayfasını kullanabilirsiniz.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: a45458e7417bba058522fdc0dbc34fee04ad9af8
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809844"
---
# <a name="view-and-manage-service-providers"></a>Görüntüleme ve hizmet sağlayıcılarını Yönet

Müşteriler **hizmeti sağlayıcıları** sayfasını [Azure portalında](https://portal.azure.com) hizmet sağlayıcılarını ve hizmet sağlayıcı teklifleri hakkında daha fazla bilgi görüntülemek için temsilci aracılığıyla belirli kaynaklara [Azure Kaynak Yönetimi temsilcisi](../concepts/azure-delegated-resource-management.md)ve ek hizmet sağlayıcısı teklifleri satın alın. Hizmet sağlayıcıları ve burada müşterileri diyoruz, ancak birden çok kiracının yönetme kuruluşların yönetim deneyimlerinden birleştirmek için aynı işlem kullanabilirsiniz.

Erişmek için **hizmeti sağlayıcıları** müşterinin Azure portalında sayfası seçebilirsiniz **tüm hizmetleri**, ardından aramak **hizmeti sağlayıcıları** ve bu seçeneği belirleyin. Bunlar ayrıca, Azure portalının üst kısmındaki arama kutusuna "Hizmet sağlayıcıları" girerek bulabilirsiniz.

Aklınızda **hizmeti sağlayıcıları** sayfası, yalnızca müşterinin abonelik veya kaynak gruplarını temsil edilen Azure kaynak yönetimi aracılığıyla erişimi hizmet sağlayıcıları hakkında bilgi gösterir. Bir müşteri temsil edilen Azure kaynak yönetimi, müşterinin kaynaklara erişmeye kullanmayan diğer hizmet sağlayıcılar ile çalışıyorsa, bu hizmet sağlayıcıları hakkında daha fazla bilgi burada gösterilmez.

> [!NOTE]
> Hizmet sağlayıcıları, müşterilerine hakkında bilgi giderek görüntüleyebilirsiniz **müşterilerimin** Azure portalında. Daha fazla bilgi için bkz. [görünümü ve müşterileri ve temsilci kaynakları yönetme](view-manage-customers.md).

## <a name="view-service-provider-details"></a>Hizmet sağlayıcısı ayrıntılarını görüntüleme

Bir müşteri ile çalışan hizmet sağlayıcıları hakkında bilgi görüntülemek için bunlar seçebilirsiniz **sağlayıcısı sunar** sol tarafındaki **hizmeti sağlayıcıları** sayfası.

Her hizmet sağlayıcısı teklif için müşteri hizmet sağlayıcısının adını ve müşteri onboarding işlemi sırasında girilen adı ile birlikte onunla ilişkili teklif görürsünüz.

İçinde **temsilcileri** sütun, müşteri görür kaç abonelikleri ve/veya kaynak grupları, bu teklif için hizmet sağlayıcısı devredildi. Hizmet sağlayıcısı erişmek ve bu abonelik ve/veya teklifte belirtilen erişim düzeylerine göre kaynak gruplarını yönetmek mümkün olacaktır.

## <a name="delegate-resources"></a>Temsilci kaynakları

Bir hizmet sağlayıcısı erişmek ve bir müşterinin kaynaklarını yönetmek için önce bunlar temsilci gerekir. Bir müşteri, bir teklifi kabul etti ancak henüz herhangi bir kaynağa yetkilendirdiği değil, en üstündeki nota görürsünüz **sağlayıcısı sunar** bölümü. Bu, hizmet sağlayıcısı müşterinin kaynaklara erişebilmeniz için önce harekete geçmeye gerektiğini bildirin müşteri olanak tanır.

Abonelik veya kaynak grupları için temsilci seçmek için:

1. Hizmet sağlayıcısı, teklif ve adını içeren satır için kutuyu işaretleyin. Ardından **temsilci kaynakları** ekranın üstünde.
1. İçinde **Teklif Ayrıntıları** bölümünü **temsilci kaynakları** sayfasında, hizmet sağlayıcısı hakkındaki ayrıntıları gözden geçirin ve sunar. Teklif için rol atamalarını gözden geçirmesini seçin **teklifle ayrıntılarını görmek için buraya tıklayın**.
1. İçinde **temsilci** bölümünden **temsilci abonelikleri** veya **temsilci kaynak grupları**.
1. Abonelikleri ve/veya bu teklif için temsilci seçmek istediğiniz kaynak gruplarını seçin, ardından seçin **Ekle**.
1. Bu hizmet sağlayıcısı seçtikten sonra seçin kaynaklara erişim istediğinizi onaylamak için sayfanın altındaki onay kutusunu işaretleyin **temsilci**.

## <a name="add-or-remove-service-provider-offers"></a>Hizmet sağlayıcısı teklifleri Ekle Kaldır

Yeni bir hizmet sağlayıcısı tekliften müşteri ekleyebilirsiniz **sağlayıcısı sunar** seçerek sayfası **Ekle teklif**. Hizmet sağlayıcısı bu müşteri için bir teklif yayımlanan gerekir. Müşteri, daha sonra bu tekliften seçebilirsiniz **özel teklifler** ekran ve ardından **Oluştur**.

Müşterinin bir hizmet sağlayıcısı teklifi kaldırma isterse, bu teklif için sıradaki çöp kutusu simgesine seçebilirsiniz. Silme işlemini onayladıktan sonra hizmet sağlayıcısı artık bu teklif için önceden temsilci müşteri kaynaklara erişim gerekir.

## <a name="view-delegations"></a>Görünüm temsilciler

Temsilcileri, bir müşteri yetkilendirdiği kaynaklar için hizmet sağlayıcısı izinler rol atamaları temsil eder. Bu bilgileri görüntülemek için seçin **temsilcileri** sol tarafındaki **hizmeti sağlayıcıları** sayfası.

Sayfanın üstündeki filtreleri, sıralamak ve gruplandırmak temsilci bilgileri veya belirli müşterilerin, teklifleri veya anahtar sözcükleri filtre olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [Azure Deniz](../overview.md).
- Hizmet sağlayıcıları nasıl edinebilirsiniz [görüntüleyin ve yönetin müşteriler](view-manage-customers.md) giderek **müşterilerimin** Azure portalında.