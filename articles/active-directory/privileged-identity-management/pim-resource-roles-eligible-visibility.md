---
title: Azure kaynakları için - uygun atamaları ve kaynak görünürlük Privileged Identity Management | Microsoft Docs
description: Kaynak rollere üye uygun olarak atamak açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 273b06c91d68a764fe814374c0eca6ed1698cc2e
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="eligible-assignments-and-resource-visibility"></a>Uygun atamaları ve Kaynak görünürlüğü

PIM Azure kaynak rolleri için kritik Azure resoruces olan kuruluşlar için Gelişmiş güvenlik sağlar. PIM Kaynak Yöneticileri üyeleri uygun kaynak rollere atama olanağı sağlar. Azure kaynak rolleri aşağıdaki durumlarını ve farklı atama türleri hakkında daha fazlasını okuyun. 

## <a name="assignment-types"></a>Atama türleri

PIM Azure kaynakları için iki farklı atama türlerini sağlar:

- Uygun
- Etkin

Uygun atamaları rolünü kullanmak için bir eylem gerçekleştirmek üzere rolünün üyesi gerektirir. Bu Eylemler, çok faktörlü kimlik doğrulama denetimi başarılı, bir gerekçe sağlama ve belirlenen onaylayanlar onay isteyen bulunabilir.

Etkin atamaları rolünü kullanmak için herhangi bir eylemi gerçekleştirmek üye gerektirmez. Etkin olarak atanan üyeler her zaman rol tarafından sağlanan ayrıcalıklarına sahiptir.

## <a name="assignment-duration"></a>Atama süresi

Kaynak yöneticileri her atama türü için iki seçenekten birini PIM ayarlarını bir rolü yapılandırırken seçebilirsiniz. Bu seçenekler üyesi PIM rolüne atandığında varsayılan en uzun süre haline gelir.

- Kalıcı uygun atamasına izin ver
- Kalıcı etkin atamasına izin ver

or

- Uygun atamaları sonra süresi dolacak
- Etkin atamaları sonra süresi dolacak

Bir kaynak yöneticisi "İzin kalıcı uygun atama" ve/veya "İzin kalıcı etkin atama", kaynağa üyeler atamak tüm yöneticiler seçerse kalıcı üyeliklerini atama olanağı sahip olur.

"Sonra süre sonu uygun atamaları" ve/veya "Süre sonu etkin atamaları sonra" seçme, belirtilen başlangıç ve bitiş tarihi tüm atamaları sahip gerektirerek atama yaşam döngüsü üzerinde denetim sağlar.

>[!NOTE] 
>Belirtilen son tarihe sahip tüm atamaları kaynak yöneticileri tarafından yenilenebilir ve üyeleri, Self Servis isteklerine başlatabilir [genişletmek veya atamaları yenileme](pim-resource-roles-renew-extend.md).


## <a name="assignment-states"></a>Atama durumları

PIM Azure kaynakları için My rolleri, rol ve üyeleri "Etkin rolleri" sekmesinde PIM görünümlerini görünür iki ayrı atama durumlar vardır. Bu durumlar şunlardır:

- Atanan
- Etkinleştirildi

"Active rollerinde" bir üyelik görüntüleme listelenen "Durum" sütununda, şu anda etkin olan kullanıcılar, "etkinleştirildi" uygun atama karşı etkin olarak "atanan" olan kullanıcılar arasında ayırt olanak sağlar.

## <a name="next-steps"></a>Sonraki adımlar

[PIM rolleri atama](pim-resource-roles-assign-roles.md)

