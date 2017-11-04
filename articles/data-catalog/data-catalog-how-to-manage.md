---
title: "Azure veri Kataloğu'nda veri varlıklarını yönetme | Microsoft Docs"
description: "Makaleyi görünürlük ve Azure veri Kataloğu'nda kayıtlı veri varlıklarının sahipliğini denetleme vurgular."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 623f5ed4-8da7-48f5-943a-448d0b7cba69
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 11/01/2017
ms.author: maroche
ms.openlocfilehash: 7e0d416c58dced89623a28038e804e8002f0341a
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="manage-data-assets-in-azure-data-catalog"></a>Azure veri Kataloğu'nda veri varlıklarını yönetme
## <a name="introduction"></a>Giriş
Böylece kolayca bulmak ve analiz gerçekleştirmek ve kararlar için gereken veri kaynaklarını anlamasına azure veri Kataloğu veri kaynağı bulma için tasarlanmıştır. Sizin ve diğer kullanıcıların bulmak ve kullanılabilir veri kaynakları çok çeşitli anlamak bulma yeteneklere büyük etkisi olun. Aklınızda bu öğeler ile görünür ve tüm katalog kullanıcıları tarafından bulunabilir olması için tüm kayıtlı veri kaynakları veri Kataloğu varsayılan davranışını içindir.

Veri Kataloğu, veri erişim sağlamaz. Veri erişimini veri kaynağının sahibi tarafından denetlenir. Veri Kataloğu ile veri kaynaklarını bulmasına ve kataloğa kayıtlı kaynaklarla ilgili meta verileri görüntüleyebilirsiniz.

Burada veri kaynaklarının yalnızca belirli kullanıcılara veya belirli grupların üyelerine görünmesi gereken durumlar olabilir. Kullanıcılar bu senaryolarda katalog içindeki kayıtlı veri varlıklarının sahipliğini ve sahip olduğunuz kaynakların güvenilirliğini denetlemek.

> [!NOTE]
> Bu makalede açıklanan işlevselliği, yalnızca, Azure veri Kataloğu standart sürümü kullanılabilir. Ücretsiz sürüm sahipliği ve kısıtlama veri varlığına görünürlük yeteneklerini sağlamaz.
>
>

## <a name="manage-ownership-of-data-assets"></a>Veri varlıklarının sahipliğini yönetme
Varsayılan olarak, veri Kataloğu'nda kayıtlı veri varlıklarını sahipsiz. Kataloğu'na erişmek için izne sahip herhangi bir kullanıcı bulmak ve bu varlıklarına açıklama. Kullanıcılar, sahipsiz veri varlıklarının sahipliğini ve sahip olduğunuz kaynakların güvenilirliğini sınırlayın.

Veri Kataloğu'ndaki bir veri varlığına ait olduğunda, yalnızca sahipleri tarafından yetkilendirilmiş kullanıcılar varlık bulmak ve meta verilerini görüntülemek ve yalnızca sahiplerinin varlık Kataloğu'ndan silebilirsiniz.

> [!NOTE]
> Veri Kataloğu sahipliği kataloğunda depolanan meta veriler etkiler. Sahipliği temel alınan veri kaynağında tüm izinleri confer değil.
>
>

### <a name="take-ownership"></a>Sahipliği alın
Kullanıcılar, seçerek veri varlıklarının sahipliğini alabilir **Sahipliği Al** veri Kataloğu portalında seçeneği. Özel izinler sahipsiz veri varlığına sahipliğini almak için gereklidir. Herhangi bir kullanıcı bir sahipsiz veri varlığı sahipliğini alabilir.

### <a name="add-owners-and-co-owners"></a>Sahipleri ve ikincil sahipler ekleme
Bir veri varlığına zaten sahip değilse, diğer kullanıcıların yalnızca Sahiplik alınamıyor. Bunlar birlikte sahipleri olarak var olan bir sahibi tarafından eklenmiş olması gerekir. Tüm sahibi ikincil sahip ek kullanıcı veya güvenlik grupları ekleyebilir.

> [!NOTE]
> Herhangi bir ait veri varlığını sahiplerini olarak en az iki kişiler için iyi bir uygulamadır.
>
>

### <a name="remove-owners"></a>Sahipleri Kaldır
Tüm varlık sahibi ikincil sahip eklemeniz yeterlidir gibi tüm varlık sahibi tüm ikincil sahip kaldırabilirsiniz.

Bir sahibi olarak kendisine veya kendisini kaldırır bir varlık sahibi artık varlık yönetebilirsiniz. Varlık sahibinin sahibi olarak kendisine veya kendisini kaldırır ve diğer ortak sahip yoktur, varlık sahipsiz durumuna geri döner.

## <a name="control-visibility"></a>Denetim görünürlüğü
Veri varlığına sahip oldukları veri varlıklarının görünürlüğünü kontrol edebilirsiniz. Burada tüm veri Kataloğu kullanıcıları bulmak ve veri varlığına görüntülemek, varsayılan olarak görünürlüğü kısıtlamak için varlık sahibinin görünürlük ayarından geçiş yapabilirsiniz **herkesin** için **sahipler ve bu kullanıcılar** içinde Varlık özellikleri. Sahipleri, belirli kullanıcılar ve güvenlik grupları daha sonra ekleyebilirsiniz.

> [!NOTE]
> Mümkün olduğunda, varlık sahipliği ve görünürlük izinleri güvenlik gruplarına bireysel kullanıcılara atanması gerekir.
>
>

## <a name="catalog-administrators"></a>Katalog yöneticileri
Veri Kataloğu yöneticileri örtük olarak katalogdaki tüm varlıkları ikincil sahip olur. Varlık sahipleri görünürlük yöneticileri kaldıramazsınız ve yöneticiler sahipliği ve tüm veri varlıklarını kataloğunda görünürlük yönetebilir.

## <a name="summary"></a>Özet
Meta veri ve veri varlık bulma için veri Kataloğu kitle kaynak modeli katkıda ve bulmak tüm katalog kullanıcıları sağlar. , Veri Kataloğu standart sürümü, görünürlük ve belirli veri varlıklarını kullanımını sınırlamak için sahipliği ve yönetimi için tasarlanmıştır.
