---
title: Azure Stack plan, teklif, kota ve abonelik genel bakış | Microsoft Docs
description: Bulut operatörü olarak, Azure Stack planlar, teklifler, kotalar ve abonelikleri anlamak istiyorum.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 10/12/2018
ms.openlocfilehash: 9d495087a0b3db5bba3b00fa9d0d40c91e5e9244
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58497866"
---
# <a name="plan-offer-quota-and-subscription-overview"></a>Plan, teklif, kota ve aboneliğe genel bakış

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

[Azure Stack](azure-stack-poc.md) , çok çeşitli iletmenizi sağlar, sanal makineler gibi SQL Server veritabanları, SharePoint, Exchange, hizmetleri ve hatta [Azure Market öğeleri](azure-stack-marketplace-azure-items.md). Azure Stack operatör olarak yapılandırın ve bu hizmetler, planlar, teklifler ve kotalar kullanarak Azure Stack'te sunun.

Bir veya daha fazla plan teklifleri içerir ve her planı bir veya daha fazla hizmet içerir. Planları oluşturmak ve bunları farklı bir teklif birleştiren yönetebilirsiniz:

- Kullanıcılarınız, hangi hizmet ve kaynaklara erişebilir.
- Kullanıcıların kullanabileceği kaynakları miktarı.
- Hangi bölgeler kaynaklara erişebilir.

Bir hizmeti sunmak, üst düzey adımları izleyin:

1. Kullanıcılarınıza sunmak istediğiniz bir hizmet ekleyin.
2. Bir veya daha fazla hizmet sahip bir plan oluşturun. Bir plan oluştururken seçin veya kaynak sınırları her hizmetin planında tanımlamak kotalar oluşturun.
3. Bir veya daha fazla plan içeren bir teklif oluşturun. Bu teklif, temel planlar ve isteğe bağlı eklenti planları dahil edebilirsiniz.

Teklif oluşturduktan sonra kullanıcılarınızın teklif sağlar Hizmetleri ve kaynaklarına erişmek için abone olabilirsiniz. Kullanıcılar istedikleri sayıda teklifler için abone olabilirsiniz. Aşağıdaki şekilde iki tekliflere abone bir kullanıcının basit bir örnek gösterilmektedir. Bir plan veya iki her bir teklifin vardır ve her plan bunları hizmetlerine erişmenizi sağlar.

![Kiracı abonelik teklifleri ve planlar](media/azure-stack-key-features/image4.png)

## <a name="plans"></a>Planlar

Planlar, bir veya birden fazla hizmetten oluşan gruplardır. Azure Stack operatör olarak, [planları oluşturun](azure-stack-create-plan.md) kullanıcılarınıza sunmak için. Böylece kullanıcılarınız tekliflerinize abone olarak, bu tekliflerin içerdiği planları ve hizmetleri kullanabilir. Planları oluştururken, kota ayarlamak, temel planlarınızı tanımlayabilirsiniz ve isteğe bağlı eklenti planları dahil etmeyi düşünün emin olun.

### <a name="quotas"></a>Kotalar

Bulut kapasitenizi yönetmenize yardımcı olmak için kullanabileceğiniz önceden yapılandırılmış *kotalar*, veya bir plandaki her hizmet için yeni bir kota oluşturun. Kotalar bir kullanıcı aboneliği sağlama veya tüketen üst kaynak sınırlarını tanımlayın. Örneğin, bir kota beş adede kadar sanal makineler (VM'ler) oluşturmak bir kullanıcı izin verebilir.

> [!IMPORTANT]
> Uygulamanın, Kullanıcı Portalı'nda veya değiştirilmiş bir kota uygulanmadan önce kullanılabilir olması için yeni kotalar iki saate kadar sürebilir.

Bölgeye göre kotalarını yapılandırabilirsiniz. Örneğin, bir bölge için bilgi işlem hizmetleri sağlayan bir plan kota iki VM olabilir.

>[!NOTE]
>Azure Stack geliştirme Seti'ni yalnızca tek bir bölge içinde (adlı *yerel*) kullanılabilir.

Daha fazla bilgi edinin [kota türleri Azure Stack'te](azure-stack-quota-types.md).

### <a name="base-plan"></a>Temel plan

Hizmet Yöneticisi, bir teklifi oluştururken, temel plan içerebilir. Bu temel plan, kullanıcı söz konusu teklife abone olduğunda varsayılan olarak dahil edilir. Bir kullanıcı abone olduğunda, bu temel planlar (ile ilgili kotalar) belirtilen tüm kaynak sağlayıcılarına erişim için sahiptirler.

### <a name="add-on-plans"></a>Eklenti planları

Eklenti planları için teklif ekleme isteğe bağlı planları oluşturulabilir. Eklenti planı varsayılan abonelik olarak dahil edilmez. Eklenti planları bir abone için aboneliklerini ekleyebileceğiniz bir teklifi bulunan ek planları (kotalar ile) oluşturulabilir. Örneğin, bir hizmeti benimsemek zorunda karar müşteriler deneme için sınırlı kaynaklarla temel plan ve daha önemli kaynaklara sahip bir eklenti planı sunabilir.

## <a name="offers"></a>Teklifler

Teklifler, oluşturduğunuz ve böylece kullanıcılar kendilerine abone olabilir bir veya daha fazla plan gruplarıdır. Örneğin, alfa teklif planı A, bilgi işlem hizmetleri kümesi sağlar ve depolama ve ağ hizmetleri sunmaktadır planlama B içerebilir.

Olduğunda, [teklif oluşturma](azure-stack-create-offer.md), en az bir temel plan eklemeniz gerekir, ancak bunların aboneliğe kullanıcı eklemek eklenti planları oluşturabilirsiniz.

## <a name="subscriptions"></a>Abonelikler

Kullanıcıların tekliflerinizi erişme bir aboneliktir. Azure Stack operatörü bir hizmet sağlayıcısı için varsa, kullanıcılarınızın (kiracılar) için tekliflerinize abone olarak hizmetlerinizi satın alın. Azure Stack operatörü bir kuruluşta kullanıyorsanız, kullanıcılarınızın (çalışan) ödeme yapmadan sunduğunuz hizmetler için abone olabilirsiniz.

Her bir teklif olan bir kullanıcı birleşimi benzersiz bir aboneliktir. Bir kullanıcı birden çok teklife abone olabilir, ancak her abonelik yalnızca bir teklif için geçerlidir. Planlar, teklifler ve kotalar yalnızca benzersiz bir abonelik için geçerlidir: abonelikler arasında paylaşılamaz. Bir kullanıcının oluşturduğu her kaynak bir abonelik ile ilişkilidir.

### <a name="default-provider-subscription"></a>Varsayılan sağlayıcı aboneliği

Azure Stack geliştirme Seti'ni dağıttığınızda varsayılan sağlayıcı aboneliği otomatik olarak oluşturulur. Bu abonelik, Azure Stack yönetmek, ek kaynak sağlayıcıları dağıtmak ve planlar ve teklifler için kullanıcı oluşturmak için kullanılabilir. Güvenlik ve lisans nedenleriyle, müşteri iş yüklerini ve uygulamaları çalıştırmak için kullanılmamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Planlar ve teklifler hakkında daha fazla bilgi için bkz: [bir plan oluşturmanız](azure-stack-create-plan.md).
