---
title: Uygun atamalar ve Azure Privileged Identity Management için kaynak görünürlüğü | Microsoft Docs
description: PIM kullanırken üyeleri kaynak rolleri için uygun olarak atamak açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 8089591708676073bcef84ad13b3690b39bdc653
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448212"
---
# <a name="eligible-assignments-and-resource-visibility-with-privileged-identity-management"></a>Uygun atamalar ve Privileged Identity Management ile kaynak görünürlüğü

Azure kaynak rolleri için Privileged Identity Management (PIM) kritik Azure kaynakları olan kuruluşlar için Gelişmiş güvenlik sağlar. Kaynak yöneticileri PIM üyeleri kaynak rolleri için uygun olarak atamak için kullanabilirsiniz. Aşağıdaki bölümlerde Azure kaynağı rolleri için atama durumu ve farklı atama türleri hakkında daha fazla bilgi edinin. 

## <a name="assignment-types"></a>Atama türleri

Azure kaynakları için PIM iki ayrı bir atama türü sağlar:

- Uygun
- Etkin

Rolünün üyesi rolü kullanmak için bir eylem gerçekleştirmek uygun atamalarını gerektirir. Çok faktörlü kimlik doğrulaması denetimi başarılı bir iş gerekçesi sağlamak ya da belirlenmiş onaylayanlar onay isteme işlemleri içerebilir.

Etkin atamalar üye rolü kullanmak için herhangi bir eylemi gerçekleştirmek gerekli değildir. Etkin olarak atanan üyeleri, her zaman rolüne atanan ayrıcalıklara sahiptir.

## <a name="assignment-duration"></a>Atama süresi

Bunlar bir rol için PIM ayarları yapılandırırken kaynak yöneticileri, her bir atama türü için iki seçenek arasından seçim yapabilirsiniz. Bu seçenekler, PIM rolüne bir üye atandığında varsayılan en uzun süresi olur. 

Yönetici bu atama türlerinden birini seçebilirsiniz:

- Kalıcı uygun atamaya izin ver
- Kalıcı etkin atamaya izin ver

Veya, bir yönetici bu atama türlerinden birini seçebilirsiniz:

- Uygun atamaların son geçerlilik tarihi
- Etkin atamaların son geçerlilik tarihi:

Bir kaynak yöneticisi seçerse **kalıcı uygun atamaya izin ver** veya **kalıcı etkin atamaya izin ver**, üyeleri için kaynak atadığınız tüm yöneticilerin kalıcı atayabilirsiniz. üyelikleri.

Bir kaynak yöneticisi seçerse **uygun atamaların son geçerlilik tarihi** veya **etkin atamaların son geçerlilik tarihi**, kaynak yöneticisi tarafından gerek atama yaşam döngüsü denetimleri tüm atama, belirtilen başlangıç ve bitiş tarihi yok.

> [!NOTE] 
> Kaynak yöneticileri tarafından belirtilen son tarihe sahip tüm atamaları yenilenebilir. Ayrıca, üyeleri Self Servis isteklerine başlatabilirsiniz [genişletin veya atamaları yenilemek](pim-resource-roles-renew-extend.md).


## <a name="assignment-states"></a>Atama durumları

Azure kaynakları için PIM görünen iki ayrı atama durumu olan **etkin rollerin** sekmesinde **rollerim**, **rolleri**, ve **üyeleri**PIM görünümlerini. Bu durumlar şunlardır:

- Atanan
- Etkin

Listelenen bir üyelik görüntülediğinizde **etkin rollerin**, değer kullanabileceğiniz **durumu** olan kullanıcıları ayırt etmek için sütun **atanan** etkin olarak ve Kullanıcılar, **etkinleştirildi** bir uygun atamayı ve bu şu anda etkin.

## <a name="next-steps"></a>Sonraki adımlar

[Ayrıcalıklı Kimlik Yöneticisi'nde Rolleri Ata](pim-resource-roles-assign-roles.md)
