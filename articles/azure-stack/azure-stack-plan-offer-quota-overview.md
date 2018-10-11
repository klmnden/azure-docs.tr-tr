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
ms.topic: get-started-article
ms.date: 06/07/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 70ed5d45701133434c708ad80aaafc58645297e8
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077128"
---
# <a name="plan-offer-quota-and-subscription-overview"></a>Plan, teklif, kota ve aboneliğe genel bakış

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

[Azure Stack](azure-stack-poc.md) çok çeşitli teslim sağlar, sanal makineler gibi SQL Server veritabanları, SharePoint, Exchange, hizmetleri ve hatta [Azure Market öğeleri](azure-stack-marketplace-azure-items.md). Azure Stack operatör olarak yapılandırın ve bu hizmetler, planlar, teklifler ve kotalar kullanarak Azure Stack'te sunun.

Bir veya daha fazla plan teklifleri içerir ve her planı bir veya daha fazla hizmet içerir. Planları oluşturmak ve bunları farklı bir teklif birleştiren yönetebilirsiniz:

- Kullanıcılarınız, hangi hizmet ve kaynaklara erişebilir.
- Kullanıcıların kullanabileceği kaynakları miktarı.
- Hangi bölgeler kaynaklara erişebilir.

Bir hizmeti sunmak, üst düzey adımları izleyin:

1. Kullanıcılarınıza sunmak istediğiniz bir hizmet ekleyin.
2. Bir veya daha fazla hizmet sahip bir plan oluşturun. Bir plan oluştururken seçin veya kaynak sınırları her hizmetin planında tanımlamak kotalar oluşturun.
3. Bir veya daha fazla plan içeren bir teklif oluşturun. Bu teklif, temel planlar ve isteğe bağlı eklenti planları dahil edebilirsiniz.

Teklif oluşturduktan sonra kullanıcılarınızın teklif sağlar Hizmetleri ve kaynaklarına erişmek için abone olabilirsiniz. Kullanıcılar istedikleri sayıda teklifler için abone olabilirsiniz. Aşağıdaki diyagramda iki tekliflere abone bir kullanıcının basit bir örnek gösterilmektedir. Bir plan veya iki her bir teklifin vardır ve her plan bunları hizmetlerine erişmenizi sağlar.

![Kiracı abonelik teklifleri ve planlar](media/azure-stack-key-features/image4.png)

## <a name="plans"></a>Planlar

Bir veya daha fazla hizmet gruplandırmalarını planları oluşturulabilir. Azure Stack operatör olarak, [planları oluşturun](azure-stack-create-plan.md) kullanıcılarınıza sunmak için. Buna karşılık, kullanıcılarınızın içerirler hizmetler ve planlar kullanılacak Teklifleriniz için abone olun. Planları oluştururken, kota ayarlamak, temel planlarınızı tanımlayabilirsiniz ve isteğe bağlı eklenti planları dahil etmeyi düşünün emin olun.

### <a name="quotas"></a>Kotalar

Bulut kapasitenizi yönetmenize yardımcı olmak için önceden yapılandırılmış kotalarını kullanın veya bir plandaki her hizmet için yeni bir kota oluşturun. Kotalar bir kullanıcı aboneliği sağlama veya tüketen üst kaynak sınırlarını tanımlayın. Örneğin, bir kota beş adede kadar sanal makineler (VM'ler) oluşturmak bir kullanıcı izin verebilir. Sanal makinelerde, RAM ve CPU çekirdeği gibi ek kotalar ayarlayın.

Bölgeye göre kotalarını yapılandırabilirsiniz. Örneğin, bir bölge için bilgi işlem hizmetleri sağlayan bir plan kota 4 GB RAM ve 8 CPU çekirdeği ile iki VM olabilir.

>[!NOTE]
>Azure Stack geliştirme Seti'ni yalnızca tek bir bölge içinde (adlı *yerel*) kullanılabilir.

Daha fazla bilgi edinin [kota türleri Azure Stack'te](azure-stack-quota-types.md).

### <a name="base-plan"></a>Temel plan

Hizmet Yöneticisi, bir teklifi oluştururken, temel plan içerebilir. Bu temel plan, kullanıcı söz konusu teklife abone olduğunda varsayılan olarak dahil edilir. Bir kullanıcı abone olduğunda, bu temel planlar (ilgili kotalar. ile) belirtilen tüm kaynak sağlayıcıları erişime sahip

### <a name="add-on-plans"></a>Eklenti planları

Eklenti planları için teklif ekleme isteğe bağlı planları oluşturulabilir. Eklenti planı varsayılan abonelik olarak dahil edilmez. Eklenti planları bir abone için aboneliklerini ekleyebileceğiniz bir teklifi bulunan ek planları (kotalar ile) oluşturulabilir. Örneğin, bir hizmeti benimsemek zorunda karar müşteriler deneme için sınırlı kaynaklarla temel plan ve daha önemli kaynaklara sahip bir eklenti planı sunabilir.

## <a name="offers"></a>Teklifler

Teklifler, oluşturduğunuz ve böylece kullanıcılar kendilerine abone olabilir bir veya daha fazla plan gruplarıdır. Örneğin, alfa teklif planı A, bilgi işlem hizmetleri kümesi sağlar ve depolama ve ağ hizmetleri sunmaktadır planlama B içerebilir.

Olduğunda, [teklif oluşturma](azure-stack-create-offer.md), en az bir temel plan eklemeniz gerekir, ancak bunların aboneliğe kullanıcı eklemek eklenti planları oluşturabilirsiniz.

## <a name="subscriptions"></a>Abonelikler

Kullanıcıların tekliflerinizi erişme bir aboneliktir. Azure Stack operatörü için bir hizmet sağlayıcısı iseniz, kullanıcılarınızın (kiracılar) için tekliflerinize abone olarak hizmetlerinizi satın. Azure Stack operatörü bir kuruluşta kullanıyorsanız, kullanıcılarınızın (çalışan) ödeme yapmadan sunduğunuz hizmetler için abone olabilirsiniz.

Her bir teklif olan bir kullanıcı birleşimi benzersiz bir aboneliktir. Bir kullanıcı birden çok teklife abone olabilir, ancak her abonelik yalnızca bir teklif için geçerlidir. Planlar, teklifler ve kotalar yalnızca benzersiz bir abonelik için geçerlidir: abonelikler arasında paylaşılamaz. Bir kullanıcının oluşturduğu her kaynak bir abonelik ile ilişkilidir.

### <a name="default-provider-subscription"></a>Varsayılan sağlayıcı aboneliği

Azure Stack geliştirme Seti'ni dağıttığınızda varsayılan sağlayıcı aboneliği otomatik olarak oluşturulur. Bu abonelik, Azure Stack yönetmek, ek kaynak sağlayıcıları dağıtmak ve planlar ve teklifler için kullanıcı oluşturmak için kullanılabilir. Güvenlik ve lisans nedenleriyle müşterinin iş yüklerini ve uygulamaları çalıştırmak için kullanılmamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Plan oluşturma](azure-stack-create-plan.md)
