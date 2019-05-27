---
title: Azure veri Kataloğu'nda veri varlıklarını yönetme
description: Makalede, görünürlük ve Azure veri Kataloğu'nda kayıtlı veri varlıklarının sahipliğini denetleme vurgulanır.
services: data-catalog
author: JasonWHowell
ms.author: jasonh
ms.assetid: 623f5ed4-8da7-48f5-943a-448d0b7cba69
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 407e25b7bb1a2220448c9701bbef208195c50b63
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65953104"
---
# <a name="manage-data-assets-in-azure-data-catalog"></a>Azure veri Kataloğu'nda veri varlıklarını yönetme
## <a name="introduction"></a>Tanıtım
Böylece kolayca keşfedin ve analiz ve karar vermek için gereken veri kaynaklarını anlama, azure veri Kataloğu veri kaynağı bulma için tasarlanmıştır. Bu bulma özellikleri, sizin ve diğer kullanıcıların bulun ve çeşitli kullanılabilir veri kaynaklarını anlama en büyük etkiyi haline getirir. Bu öğeleri göz önünde görünür ve tüm katalog kullanıcıları tarafından bulunabilir olması tüm kayıtlı veri kaynakları veri Kataloğu'nın varsayılan davranışını içindir.

Veri Kataloğu, veri için erişim sağlamaz. Veri erişimini veri kaynağının sahibi tarafından denetlenir. Veri Kataloğu ile veri kaynaklarını bulmasına ve kataloğa kayıtlı kaynaklarla ilgili meta verileri görüntüleyebilirsiniz.

Burada veri kaynakları yalnızca belirli kullanıcılara veya belirli grupların üyelerine görünmesi durumlar olabilir. Kullanıcıların böyle senaryolarda katalog içindeki kayıtlı veri varlıklarının sahipliğini ve ardından oldukları varlıklarının görünürlüğünü denetleme.

> [!NOTE]
> Bu makalede açıklanan işlevselliği, yalnızca, Azure veri Kataloğu standart sürümü içinde kullanılabilir. Ücretsiz sürüm sahipliği ve kısıtlama veri varlığına görünürlük özelliklerini sağlamaz.
>
>

## <a name="manage-ownership-of-data-assets"></a>Veri varlıklarının sahipliğini yönetme
Varsayılan olarak, veri Kataloğu'nda kayıtlı veri varlıklarını sahipsiz. Kataloğu'na erişme izni olan herhangi bir kullanıcı bulabilir ve bu varlıklarına açıklama. Kullanıcılar sahipsiz veri varlıklarının sahipliğini alabilir ve sonra oldukları varlıklarının görünürlüğünü sınırlamak.

Veri Kataloğu'nda bir veri varlığına ait zaman sahipleri tarafından yetkili kullanıcıların varlık bulabilir ve meta verilerini görüntüleyin ve yalnızca sahiplerinin varlık Kataloğu'ndan silebilirsiniz.

> [!NOTE]
> Veri Kataloğu'nda sahipliği kataloğunda depolanan meta veriler etkiler. Sahiplik, temel alınan veri kaynağında herhangi bir izni confer değil.
>
>

### <a name="take-ownership"></a>Sahipliği üstlen
Kullanıcıların, seçerek veri varlıklarının sahipliğini yapabileceklerinizi **Sahipliği Al** veri Kataloğu portalında seçeneği. Sahipsiz veri varlığı sahipliğini almak için özel izin gerekir. Herhangi bir kullanıcı, bir sahipsiz veri varlığına sahipliğini alabilir.

### <a name="add-owners-and-co-owners"></a>Sahipler ve ikincil sahipler ekleyin
Bir veri varlığına zaten aitse, diğer kullanıcılar yalnızca sahipliği alamıyor. Bunlar mevcut bir sahibi tarafından ortak sahip olarak eklenmelidir. Herhangi bir sahip ek kullanıcılar veya güvenlik gruplarının ortak sahip olarak ekleyebilirsiniz.

> [!NOTE]
> Herhangi bir kişisel veri varlığı için sahipleri olarak en az iki kişiler için iyi bir uygulamadır.
>
>

### <a name="remove-owners"></a>Sahipleri kaldır
Herhangi bir varlık sahibi ikincil sahipler eklemeniz yeterlidir gibi tüm ikincil sahip herhangi bir varlık sahibi kaldırabilirsiniz.

Sahip olarak kendilerini kaldırır bir varlık sahibi artık varlık yönetebilirsiniz. Varlık sahibi kendilerini bir sahibi olarak kaldırır ve diğer ikincil sahipleri, varlık sahipsiz durumuna geri döner.

## <a name="control-visibility"></a>Denetim görünürlük
Veri varlığına sahip oldukları veri varlıklarının görünürlüğünü denetleyebilir. Burada tüm veri Kataloğu kullanıcıları bulmak ve veri varlığı görüntülemek, varsayılan olarak görünürlüğü kısıtlamak için varlık sahibi görünürlük ayarından geçiş yapabilirsiniz **herkes** için **sahipler ve bu kullanıcılar** içinde Varlık özellikleri. Sahipler, ardından belirli kullanıcılar ve güvenlik grupları ekleyebilirsiniz.

> [!NOTE]
> Mümkün olduğunda, varlık sahipliği ve görünürlük izinleri, güvenlik gruplarına ve bireysel kullanıcılara atanmalıdır.
>
>

## <a name="catalog-administrators"></a>Katalog yöneticileri
Veri Kataloğu yöneticileri, örtük olarak ikincil sahipler katalogdaki tüm varlıkların olur. Varlık sahipleri, yöneticileri görünürlük kaldıramazsınız ve yöneticiler sahipliği ve katalogdaki tüm veri varlıkları için görünürlük yönetebilir.

## <a name="summary"></a>Özet
Katkıda bulunan ve bulmak tüm katalog kullanıcıları meta verileri ve veri varlığı bulma için veri Kataloğu kitle kaynak modeli sağlar. , Veri Kataloğu standart sürümü, sahipliği ve yönetimi için görünürlük ve belirli veri varlıklarını kullanımını sınırlamak için tasarlanmıştır.
