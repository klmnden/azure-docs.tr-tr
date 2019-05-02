---
title: Özel SKU'ları ve planları | Azure Market
description: Teklif bulunurluğu yönetmek için özel bir SKU'ları kullanma
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 6efdb1c28777d9230727066fdba03d2850be62b0
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935914"
---
<a name="private-skus-and-plans"></a>Özel SKU'ları ve planlar
============

Özel SKU'ları belirli müşterilere SKU kullanılabilirliğini sınırlamak etkinleştirin. Bir SKU özel olarak işaretlendiğinde, herhangi genel kataloğu dahil olmak üzere kullanılabilir değil [Azure Marketi](https://azuremarketplace.microsoft.com) ve [Azure portalında](https://portal.azure.com). Azure portalında, yalnızca SKU erişimi olan müşteriler, görebilirsiniz. Ayrıca, bunlar ayrıca özel teklifler için erişime sahip oldukları girmeniz istenirdi.

>[!NOTE]
>Özel SKU'ları yeni benzersiz SKU/Plan Genel SKU'larınız ile herhangi çakışmayı önlemek için kimlikleri olması gerekir.

Aşağıdaki senaryolarda işlemek için özel bir SKU'ları kullanabilirsiniz:

1.  Yalnızca belirli müşterilere genel olarak kullanılabilir ve genel olarak kullanılabilir değil istediğiniz yazılım yayımlayın.
2.  Özelleştirilmiş bir fiyat karşılığında Genel yazılım çeşitleri belirli müşteriler için yayımlayın.
3.  Özelleştirilmiş bir açıklama ve terimleri (yeni teklif) aracılığıyla Genel yazılım çeşitleri yayımlayın.

Yalnızca fiyat değiştirmek istiyorsanız, aynı Teklif başka bir SKU'da disklerden yeniden kullanabilirsiniz. Özel SKU'larıyla diskleri SKU'lar arasında yeniden gerekmez.

<a name="mark-a-sku-private"></a>Bir SKU özel olarak işaretle
---------------------

Bir SKU özel olarak işaretlemek için SKU özel olup olmadığını soran seçeneğini değiştirin:

![Bir SKU, özel olarak işaretleyin.](./media/cloud-partner-portal-publish-virtual-machine/markingskuprivate.png)

Başka bir SKU disklerin yeniden ve fiyatlandırma veya açıklamayı değiştirin. Diskleri yeniden kullanmak için işaretleyin **Evet** "Bu SKU yeniden kullanımı görüntüler genel bir SKU mu" yanıt olarak ister.

SKU özel olarak işaretlenmiş ve diğer SKU'lar reuseable disklerle teklif varsa SKU başka bir SKU disklerden kullanır belirtmek için gereklidir. Ayrıca özel SKU için hedef kitle belirtmek için gereklidir.

>[!NOTE]
>Yayımlandıktan sonra genel bir SKU özel yapılamaz.

<a name="select-an-image"></a>Resim seçme
------------------

Zaten başka bir SKU fiyatlandırması yalnızca değiştirme, veya açıklama içinde sağlanan aynı diskleri yeniden ya da özel SKU için yeni diskler sağlar. Diskleri yeniden kullanmak için işaretleyin **Evet** "Bu SKU görüntü yeniden kullanımı genel bir SKU yok" istemine yanıt olarak.

![Görüntüyü yeniden kullanımı gösterir](./media/cloud-partner-portal-publish-virtual-machine/selectimage1.png)

SKU başka bir SKU görüntüleri yeniden onayladıktan sonra kaynak görüntülerin SKU belirleyin.

Sonraki ekranda istemleri özel SKU'yu belirleme seçilen SKU görüntülerden yeniden show Yakala:

![Resim seçme](./media/cloud-partner-portal-publish-virtual-machine/selectimage2.png)

Teklif yayımladığınızda, seçilen SKU görüntülerden özel fiyatlar/koşullarına özel SKU kimliği altında kullanılabilir hale. Özel SKU yalnızca hedef kitleye görünür olur.

Görüntü güncelleştirmeler için yalnızca temel SKU'ları görüntüsünü güncelleştirmek için gerekli olacak. Arka planda görüntüsü özel SKU için de otomatik olarak güncelleştirilir. Temel SKU seçerek görüntüyü silin, benzer şekilde, görüntü ayrıca özel SKU kaldırılır.

<a name="restricting-the-audience"></a>Hedef kitle kısıtlama
------------------------

Özel teklifler bulunan ve yalnızca hedeflenen kullanıcılar tarafından dağıtılan.
Şu anda abonelik kimlikleri kullanarak kullanıcıları hedeflerken destekliyoruz.

Bu abonelikler bir el ile giriş formunu girilebilir **için 10 adede kadar abonelikleri**, veya sağlayan bir CSV dosyasını karşıya yükleyerek **20000 abonelikler için**.

Sınırlı bir kitle için el ile giriş:

![Hedef kitle el ile sınırla](./media/cloud-partner-portal-publish-virtual-machine/restrictaudience1.png)

CSV'yi karşıya yükle sınırlı bir kitle için:

![CSV hedef kitle kısıtlamak için kullanın](./media/cloud-partner-portal-publish-virtual-machine/restrictaudience2.png)

Örnek CSV dosyası içeriği:

            Type,Id,Description
            SubscriptionId,7738d703-3135-4e8d-8b81-1e70379abd9d,Private Customer

Değiştirdiğinizde CSV için el ile girişinde görünümü karşıya yükleyebilir veya el ile girişi CSV'den eski SKU erişimi olan abonelik kimlikleri listesini korunmaz. Bir uyarı görüntülenir ve listede yalnızca teklif kaydedildiğinde üzerine yazılır.

<a name="sync-private-subscriptions"></a>Eşitleme özel abonelikler
-------------------------

Abonelik özel SKU veya planı ile yayımlanan bir teklifi eklerken, İzleyici bilgi eklemek için teklif yeniden yayımlamanız gerekmez. Yalnızca İzleyici eklemek için bir Azure aboneliği kimliği (planları ve SKU'ları) veya Kiracı kimliği (yalnızca planları) kullanın.

<a name="previewing-private-offers"></a>Özel önizleme sunar.
-------------------------

Önizleme/hazırlama sırasında teklif düzeyi Önizleme abonelikleri SKU erişebilir yalnızca adım. Bu aynı zamanda hangi teklif doğrulamak için sınama aşaması, hedeflenen müşterilerinize gibi görünür ve yayımlama tüm türleri için standart.

Hazırlanmış teklifler erişim düzeyi Önizleme abonelikleri sunar:

![Önizleme abonelik kimlikleri](./media/cloud-partner-portal-publish-virtual-machine/previewoffer1.png)

Teklif Canlı olduktan sonra yalnızca sınırlı bir kitle abonelikleri (el ile giriş veya CSV girilen) görüntüleyebilir ve özel SKU'su dağıtmak olacaktır. Olmasını öneririz, **her zaman kendi aboneliklerini kısıtlı izleyiciye dahil** doğrulama amacıyla özel SKU.

>[!NOTE]
>Hata ayıklama amacıyla Microsoft desteği ve mühendislik ekipleri ayrıca özel bu teklifleri erişebilir.
