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
ms.date: 06/07/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: d8aef778807d3a8a61cf9eedaae24abce84a19ab
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248767"
---
# <a name="plan-offer-quota-and-subscription-overview"></a>Plan, teklif, kota ve aboneliğe genel bakış

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

[Azure yığın](azure-stack-poc.md) çok çeşitli teslim sağlar, SQL Server veritabanları, SharePoint, Exchange, sanal makineler gibi hizmetleri ve hatta [Azure Market öğesi](azure-stack-marketplace-azure-items.md). Bir Azure yığın işleç olarak yapılandırın ve planları, teklifleri ve kotaları kullanarak Azure yığınında gibi hizmetleri sunmak.

Bir veya daha fazla plan teklifleri içerir ve bir veya daha fazla hizmet her plan içerir. Planları oluşturma ve bunları farklı teklifleri birleştirilmesi yönetebilirsiniz:

- Hangi hizmet ve kaynakları kullanıcılarınız erişebilir.
- Kullanıcıların tüketebileceği kaynakları miktarı.
- Hangi bölgeleri kaynaklarına erişimi vardır.

Bir hizmet sunmak, üst düzey adımları izleyin:

1. Kullanıcılarınız için teslim etmek istediğiniz bir hizmet ekleyin.
2. Bir veya daha fazla hizmet sahip bir plan oluşturun. Bir plan oluştururken seçin veya planda her hizmetin kaynak sınırlarını tanımlamak kotaları oluşturun.
3. Bir veya daha fazla plan içeren bir teklif oluşturun. Teklif temel planları ve isteğe bağlı eklenti planları dahil edebilirsiniz.

Teklif oluşturduktan sonra kullanıcılarınızın teklif sağlar Hizmetleri ve kaynaklarına erişmek için abone olabilirsiniz. Kullanıcıların istedikleri gibi sayıda teklifleri için abone olabilirsiniz. Aşağıdaki diyagramda iki teklifleri abone bir kullanıcının basit bir örnek gösterilmektedir. Bir plan veya iki her teklif varsa ve her plan bunları erişim hizmetlerini sağlar.

![Kiracı aboneliği sunar ve planları](media/azure-stack-key-features/image4.png)

## <a name="plans"></a>Planlar

Bir veya daha fazla hizmet gruplandırmaları planlarının. Bir Azure yığın operatör olarak, [planları oluşturma](azure-stack-create-plan.md) kullanıcılarınıza sunmak için. Buna karşılık, kullanıcılarınızın planları ve içerirler Hizmetleri'ni kullanmak için Teklifleriniz için abone olun. Planları oluştururken, kotalar ayarlamak için temel planlarınızı tanımlayın ve isteğe bağlı eklenti planları dahil etmeyi düşünün emin olun.

### <a name="quotas"></a>Kotalar

Bulut kapasitesi yönetmenize yardımcı olmak için önceden yapılandırılmış kotalarını kullanın veya bir plandaki her hizmet için yeni bir kota oluşturun. Kotalar bir kullanıcı abonelik sağlamak veya tüketmek üst kaynak sınırlarını tanımlayın. Örneğin, kota en fazla beş sanal makineler (VM'ler) oluşturmak bir kullanıcı izin verebilir. Sanal makinelerde RAM ve CPU çekirdekleri gibi ek kotalar ayarlayın.

Bölgeye göre kotalarını yapılandırabilirsiniz. Örneğin, bir bölge için işlem hizmetleri sağlayan bir plan ile 4 GB RAM ve 8 CPU çekirdeği iki VM kotası olabilir.

>[!NOTE]
>Azure yığın Geliştirme Seti, yalnızca tek bir bölge içinde (adlı *yerel*) kullanılabilir.

Daha fazla bilgi edinmek [kota türleri Azure yığınında](azure-stack-quota-types.md).

### <a name="base-plan"></a>Temel plan

Hizmet Yöneticisi, bir teklifi oluştururken, temel plan dahil edebilirsiniz. Bu teklif için bir kullanıcı abone olduğunda bu temel planları varsayılan olarak dahil edilir. Bir kullanıcı abone olduğunda, bu temel planlarıyla (ilgili kotalar.) belirtilen tüm kaynak sağlayıcıları erişimi

### <a name="add-on-plans"></a>Eklenti planları

Eklenti planları için bir teklif eklemek isteğe bağlı planlarının. Eklenti planları Abonelikteki varsayılan olarak dahil edilmez. Eklenti, ek planlarıyla (kotaları) abone, abonelik ekleyebileceğiniz teklif bulunan planlarının. Örneğin, hizmet benimsemeye karar müşteriler için bir deneme sürümü için sınırlı kaynaklarla temel plan ve daha önemli kaynağı olan bir eklenti plana sunabilir.

## <a name="offers"></a>Teklifler

Teklifler oluşturduğunuz ve böylece kullanıcılar bunlara abone olabilir bir veya daha fazla plan gruplarıdır. Örneğin, teklif alfa işlem hizmetleri kümesi sağlar planı A ve depolama ve ağ hizmetleri kümesi sağlar planlama B içerebilir.

Olduğunda, [teklifi oluşturmak](azure-stack-create-offer.md), en az bir temel plan içermelidir, ancak kendi aboneliğe kullanıcı eklemek eklenti planları oluşturabilirsiniz.

## <a name="subscriptions"></a>Abonelikler

Abonelik, kullanıcıların Teklifleriniz erişme ' dir. Bir hizmet sağlayıcısı için bir Azure yığın işleç değilseniz, kullanıcılarınızın (kiracılar) Teklifleriniz için abone olarak hizmetlerinizin satın alın. Bir kuruluştaki bir Azure yığın işleç değilseniz, kullanıcılarınızın (çalışanlar) ödeme olmadan sunduğunuz hizmetler için abone olabilirsiniz.

Her bir teklif sahip bir kullanıcı bir benzersiz abonelik birleşimidir. Bir kullanıcı birden fazla teklifleri için abonelik olabilir, ancak her abonelik yalnızca bir teklif için geçerlidir. Planları, teklifleri ve kotalar benzersiz bir aboneliğiniz için yalnızca geçerli – abonelikler arasında paylaşılamaz. Bir kullanıcının oluşturduğu her bir kaynağın bir abonelik ile ilişkilidir.

### <a name="default-provider-subscription"></a>Varsayılan sağlayıcı abonelik

Azure yığın Geliştirme Seti dağıttığınızda varsayılan sağlayıcı abonelik otomatik olarak oluşturulur. Bu abonelik Azure yığın yönetmek, ek kaynak sağlayıcıları dağıtmak ve planları ve kullanıcılar için teklifleri oluşturmak için kullanılabilir. Güvenlik ve lisans nedenleriyle, müşteri iş yüklerini ve uygulamaları çalıştırmak için kullanılmamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Plan oluşturma](azure-stack-create-plan.md)
