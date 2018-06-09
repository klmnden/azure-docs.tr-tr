---
title: Uygun atamaları ve Privileged Identity Management Azure'da kaynak görünürlük | Microsoft Docs
description: PIM kullanırken üye kaynak rolleri için uygun olarak atamak açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: c100cf5421539c985b8132bfd8af59ed0ac2e01c
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233433"
---
# <a name="eligible-assignments-and-resource-visibility-with-privileged-identity-management"></a>Uygun atamaları ve Privileged Identity Management ile kaynak görünürlüğü

Azure kaynak rolleri için ayrıcalıklı Kimlik Yönetimi (PIM) kritik Azure kaynaklarını olan kuruluşlar için Gelişmiş güvenlik sağlar. Kaynak yöneticileri PIM üye kaynak rolleri için uygun olarak atamak için kullanabilirsiniz. Aşağıdaki bölümlerde Azure kaynak roller atama durumlarını ve farklı atama türleri hakkında daha fazla bilgi edinin. 

## <a name="assignment-types"></a>Atama türleri

PIM Azure kaynakları için iki farklı atama türlerini sağlar:

- Uygun
- Etkin

Uygun atamaları rolünü kullanmak için bir eylem gerçekleştirmek üzere rolünün üyesi gerektirir. Eylemler, çok faktörlü kimlik doğrulama denetimi başarılı, bir iş gerekçesinin sağlanması veya belirlenen onaylayanlar onay isteyen içerebilir.

Etkin atamaları rolünü kullanmak için herhangi bir eylemi gerçekleştirmek üye olması gerekmez. Etkin olarak atanan üyelerin her zaman rolüne atanan ayrıcalıklarına sahiptir.

## <a name="assignment-duration"></a>Atama süresi

Bir rol için PIM ayarlarını yapılandırırken kaynak yöneticiler her atama türü için iki seçenek arasından seçim yapabilirsiniz. Bu seçenekler üyesi PIM rolüne atandığında varsayılan en uzun süre haline gelir. 

Bir yönetici bu atama türlerinden birini seçebilirsiniz:

- Kalıcı uygun atamaya izin ver
- Kalıcı etkin atamaya izin ver

Veya, bir yönetici bu atama türlerinden birini seçebilirsiniz:

- Uygun atamaların son geçerlilik tarihi
- Etkin atamaların son geçerlilik tarihi:

Bir kaynak yöneticisi seçerse **kalıcı uygun atamasına izin ver** veya **kalıcı etkin atama izin**, kaynağa üyeler atamak tüm yöneticiler kalıcı atayabilirsiniz üyelikleri.

Bir kaynak yöneticisi seçerse **uygun atamaları sonra süresi dolacak** veya **sonra etkin atamaları sona**, kaynak yöneticisi, gerektirerek atama yaşam döngüsü denetimleri tüm atama, belirtilen başlangıç ve bitiş tarihi yok.

> [!NOTE] 
> Belirtilen son tarihe sahip tüm atamaları kaynak yöneticileri tarafından yenilenebilir. Ayrıca, Self Servis isteklerine üyeleri başlatabilirsiniz [genişletmek veya atamaları yenileme](pim-resource-roles-renew-extend.md).


## <a name="assignment-states"></a>Atama durumları

PIM Azure kaynakları için görünür iki ayrı atama durumu olan **etkin rollerin** sekmesinde **My rolleri**, **rolleri**, ve **üyeleri**PIM görünümlerini. Bu durumlar şunlardır:

- Atanan
- Etkin

Listede bir üyelik görüntülediğinizde **etkin rollerin**, değer kullanabilirsiniz **durumu** olan kullanıcılar arasında ayırt etmek için sütun **atanan** etkin olarak ve Kullanıcılar, **etkinleştirildi** bir uygun atama ve bu şu anda etkin.

## <a name="next-steps"></a>Sonraki adımlar

[Ayrıcalıklı Kimlik Yöneticisi'nde Rolleri Ata](pim-resource-roles-assign-roles.md)
