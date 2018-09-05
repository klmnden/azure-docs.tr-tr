---
title: Uygun atamalar ve kaynak görünürlük PIM - Azure | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure kaynak rolleri için uygun üyelere atamak açıklar.
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
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: fb52bc92c86261831d0e8d8e9e863a4863fe8fb9
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666898"
---
# <a name="eligible-assignments-and-resource-visibility-in-pim"></a>Uygun atamalar ve pım'de Kaynak görünürlüğü

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

## <a name="azure-resource-role-approval-workflow"></a>Azure Kaynak rolü onay iş akışı

Onay iş akışı ile PIM Azure kaynak rolleri için Yöneticiler daha fazla koruyabilir veya kritik kaynaklara erişimi kısıtlayın. Diğer bir deyişle, yöneticileri, rol atamalarını etkinleştirmek için onay isteyebilir.

Azure kaynak rolleri için benzersiz bir kaynak hiyerarşisinin kavramdır. Bu hiyerarşi üst kapsayıcı içindeki tüm alt kaynaklar için rol atamalarını aşağı üst kaynak nesneden devralınmasını sağlar. 

Örneğin: Bob, bir kaynak yöneticisi, uygun bir üye olarak Esra'nın Contoso abonelik sahibi rolüne atamak için PIM kullanır. Bu atama ile Esra'nın Contoso Abonelikteki tüm kaynak grubu kapsayıcıları bir uygun sahibidir. Gamze ayrıca bir uygun tüm kaynakları (sanal makineler gibi) abonelik her bir kaynak grubu içinde sahibidir.

Contoso abonelikte üç kaynak grubunuz yok varsayalım: Fabrikam Test, Fabrikam geliştirme ve Fabrikam ürün. Bu kaynak gruplarının her biri tek bir sanal makine içeriyor.

PIM ayarları, her bir kaynağın rolünü için yapılandırılır. Atamaları aksine bu ayarları yok devralınır ve kesinlikle Kaynak rolü için geçerlidir.

Örneğiyle devam etmesini: Bob PIM Contoso abonelik isteği onay sahibi roldeki tüm üyeleri etkinleştirilmesini istemek için kullanır. Fabrikam Prod kaynak grubundaki kaynakları korumak için Bob Ayrıca bu kaynağın sahip rolünün üyeleri için onay gerektirir. Fabrikam Test ve geliştirme Fabrikam sahip roller, etkinleştirme için onay gerekmez.

Alice Contoso abonelik için sahip rolünü etkinleştirme isteğinde bulunduğunda, bir onaylayan onaylayabilir veya o roldeki etkinleştirilmeden önce her bir isteği reddetmek gerekir. Alice karar verirse [her etkinleştirme kapsam](pim-resource-roles-activate-your-roles.md) Fabrikam Prod kaynak grubunu, bir onaylayan gerekir onaylayın veya çok bu isteği reddedin. Ancak, Esra'nın Fabrikam Test veya geliştirme Fabrikam biri veya her ikisi için her etkinleştirme kapsam karar verirse, onayı gerekli değildir.

Onay iş akışı, bir rolün tüm üyeleri için gerekli olmayabilir. Burada, kuruluşunuzun bir Azure aboneliğinde çalıştırılan uygulamanın geliştirilmesine yardımcı olmak için birkaç sözleşme ilişkilendirir hires bir senaryo düşünün. Bir kaynak yöneticisi, gerekli onay uygun erişim sağlamak için çalışanların istediğiniz, ancak sözleşme ilişkilendirir onay istemelisiniz. Onay iş akışı için yalnızca sözleşme ilişkilendirir yapılandırmak için atanmış çalışanlara rolle aynı izinlere sahip bir özel rol oluşturabilirsiniz. Bu özel rolü etkinleştirmek için onay gerektirebilir. [Özel roller hakkında daha fazla bilgi](pim-resource-roles-custom-role-policy.md).

## <a name="next-steps"></a>Sonraki adımlar

- [PIM Azure kaynak Rolleri Ata](pim-resource-roles-assign-roles.md)
