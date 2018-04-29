---
title: Azure yığın planı, teklif, kota ve abonelik genel bakış | Microsoft Docs
description: Bulut operatörü Azure yığın planları, tekliflerini, kotalarını ve abonelikleri anlamak istiyorum.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/20/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: fcf19f486ebdc739f3d5c7b25215ba8726462a56
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="plan-offer-quota-and-subscription-overview"></a>Plan, teklif, kota ve aboneliğe genel bakış

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

[Azure yığın](azure-stack-poc.md) çok çeşitli teslim sağlar, SQL Server veritabanları, SharePoint, Exchange, sanal makineler gibi hizmetleri ve hatta [Azure Market öğesi](azure-stack-marketplace-azure-items.md). Bir Azure yığın işleç olarak yapılandırın ve planları, teklifleri ve kotaları kullanarak Azure yığınında gibi hizmetleri sunmak.

Bir veya daha fazla plan teklifleri içerir ve bir veya daha fazla hizmet her plan içerir. Planları oluşturma ve bunları farklı teklifleri birleştirilmesi denetim
- hangi hizmet ve kaynakları kullanıcılar erişebilir
- Kullanıcıların tüketebileceği kaynaklarla miktarı
- hangi bölgeleri kaynaklara erişimi

Bir hizmet sunmak, üst düzey adımları izleyin:

1. Kullanıcılarınız için teslim etmek istediğiniz bir hizmet ekleyin.
2. Bir veya daha fazla hizmet içeriyor bir plan oluşturun. Bir plan oluştururken seçin veya planda her hizmetin kaynak sınırlarını tanımlamak kotaları oluşturun.
3. (Temel planları ve isteğe bağlı eklenti planları dahil) bir veya daha fazla plan içeren bir teklif oluşturun.

Teklif oluşturduktan sonra kullanıcılarınızın Hizmetleri ve sağladığı kaynaklarına erişmek için abone olabilirsiniz. Kullanıcıların istedikleri gibi sayıda teklifleri için abone olabilirsiniz. Aşağıdaki diyagramda iki teklifleri abone bir kullanıcının basit bir örnek gösterilmektedir. Bir plan veya iki her teklif varsa ve her plan bunları erişim hizmetlerini sağlar.

![](media/azure-stack-key-features/image4.png)

## <a name="plans"></a>Planlar

Bir veya daha fazla hizmet gruplandırmaları planlarının. Bir Azure yığın operatör olarak, [planları oluşturma](azure-stack-create-plan.md) kullanıcılarınıza sunmak için. Buna karşılık, kullanıcılarınızın planları ve içerirler Hizmetleri'ni kullanmak için Teklifleriniz için abone olun. Planları oluştururken, kotalar ayarlamak için temel planlarınızı tanımlayın ve isteğe bağlı eklenti planları dahil etmeyi düşünün emin olun.

### <a name="quotas"></a>Kotalar

Bulut kapasitesi yönetmenize yardımcı olmak üzere seçin veya bir plandaki her hizmet için bir kota oluşturun. Kotalar bir kullanıcı abonelik sağlamak veya tüketmek üst kaynak sınırlarını tanımlayın. Örneğin, kota en fazla beş sanal makineler oluşturmak bir kullanıcı izin verebilir. Kotalar, sanal makineler, RAM ve CPU gibi kaynakları çeşitli sınırlandırabilir sınırlar.

Kotalar bölgeye göre yapılandırılabilir. Örneğin, A bölgesinden işlem hizmetleri içeren bir planı iki sanal makine, 4 GB RAM ve 10 CPU çekirdek kotası olabilir. Azure yığın Geliştirme Seti, yalnızca tek bir bölge içinde (adlı *yerel*) kullanılabilir.

Daha fazla bilgi edinmek [kota türleri Azure yığınında](azure-stack-quota-types.md). 

### <a name="base-plan"></a>Temel plan

Hizmet Yöneticisi, bir teklifi oluştururken, temel plan dahil edebilirsiniz. Bu teklif için bir kullanıcı abone olduğunda bu temel planları varsayılan olarak dahil edilir. Bir kullanıcı abone olduğunda, bu temel planlarıyla (karşılık gelen kotaları) içinde belirtilen tüm kaynak sağlayıcıları erişimi.

### <a name="add-on-plans"></a>Eklenti planları

Eklenti planları için bir teklif eklemek isteğe bağlı planlarının. Eklenti planları Abonelikteki varsayılan olarak dahil edilmez. Eklenti, ek planlarıyla (kotaları) abone, abonelik ekleyebileceğiniz teklif bulunan planlarının. Örneğin, hizmet benimsemeye karar müşteriler için bir deneme sürümü için sınırlı kaynaklarla temel plan ve daha önemli kaynağı olan bir eklenti plana sunabilir.

## <a name="offers"></a>Teklifler

Teklifler oluşturduğunuz ve böylece kullanıcılar bunlara abone olabilir bir veya daha fazla plan gruplarıdır. Örneğin, teklif alfa planı A içerebilir içeren bir dizi işlem Hizmetleri ve depolama ve ağ hizmetleri kümesini içeren B planlayın. 

Olduğunda, [teklifi oluşturmak](azure-stack-create-offer.md), en az bir temel plan içermelidir, ancak kendi aboneliğe kullanıcı eklemek eklenti planları oluşturabilirsiniz.


## <a name="subscriptions"></a>Abonelikler

Abonelik, kullanıcıların Teklifleriniz erişme ' dir. Bir hizmet sağlayıcısında bir Azure yığın işleç değilseniz, kullanıcılarınızın (kiracılar) Teklifleriniz için abone olarak hizmetlerinizin satın alın. Bir kuruluştaki bir Azure yığın işleç değilseniz, kullanıcılarınızın (çalışanlar) ödeme olmadan sunduğunuz hizmetler için abone olabilirsiniz. Her bir teklif sahip bir kullanıcı bir benzersiz abonelik birleşimidir. Bu nedenle, bir kullanıcı birden fazla teklifleri için abonelik olabilir, ancak her abonelik için yalnızca bir teklif geçerlidir. Planları, teklifleri ve kotaları yalnızca benzersiz her aboneliğiniz için geçerli – abonelikler arasında paylaşılamaz. Bir kullanıcının oluşturduğu her bir kaynağın bir abonelik ile ilişkilidir.


### <a name="default-provider-subscription"></a>Varsayılan sağlayıcı abonelik

Azure yığın Geliştirme Seti dağıttığınızda varsayılan sağlayıcı abonelik otomatik olarak oluşturulur. Bu abonelik Azure yığın yönetmek, daha fazla kaynak sağlayıcıları dağıtmak ve planları ve kullanıcılar için teklifleri oluşturmak için kullanılabilir. Güvenlik ve lisans nedenleriyle, müşteri iş yüklerini ve uygulamaları çalıştırmak için kullanılmamalıdır. 

## <a name="next-steps"></a>Sonraki adımlar

[Plan oluşturma](azure-stack-create-plan.md)
