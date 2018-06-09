---
title: Azure yığınında hizmetleri sunan | Microsoft Docs
description: Bulut operatörü olarak, kullanıcılarınıza hizmet sunabilir.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 042e65cfe350cb61124ed8920ae3616502e6553d
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248852"
---
# <a name="overview-of-offering-services-in-azure-stack"></a>Azure yığınında hizmetleri sunan genel bakış

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

[Microsoft Azure yığın](azure-stack-poc.md) merkeziniz Hizmetleri sunmanıza olanak sağlayan bir karma bulut platformudur. Bir hizmet sağlayıcısı olarak, kiracılarınıza hizmet sunabilir. Bir iş veya devlet dairesi içinde çalışanlarınıza şirket içi hizmet sunabilir. Sağlayabileceğiniz Hizmetleri dahil ancak bunlarla sınırlı değildir:

- Bir hizmet (PaaS) Hizmetleri olarak platform uygulama hizmetleri, API Apps, API işlevleri, SQL ve MySQL ister.

Tümleştirme ve farklı kullanıcılar için karmaşık çözümleri oluşturmak için Hizmetleri bile birleştirebilirsiniz.

Kullanıcılarınız için bu hizmetleri sunmak için oluşturmalısınız [planları, teklifleri ve kotalar](azure-stack-plan-offer-quota-overview.md). Kullanıcılarınızın sonra hizmetleri kullanmaya Teklifleriniz için abone olabilirsiniz.

## <a name="plan-your-service-offers"></a>Hizmet teklifleri planlama

Teklifleriniz planlıyorsanız, aşağıdaki noktaları göz önünde bulundurun:

**Deneme teklifleri**: ek hizmetler sonra yükseltebilirsiniz yeni kullanıcılar çekmek için deneme teklifleri kullanabilirsiniz. Bir deneme teklifi oluşturmak için küçük bir oluşturun [temel plan](azure-stack-plan-offer-quota-overview.md#base-plan) isteğe bağlı büyük bir eklenti plan ile.

**Kapasite planlama**: kaynakları ve tüm kullanıcılar için sistem tıkamasını büyük miktarlarda yakalayın kullanıcıların endişe olabilir. Performans yardımcı olmak için şunları yapabilirsiniz [planlarınızı kotaları yapılandırın](azure-stack-plan-offer-quota-overview.md#plans) cap kullanım için.

**Temsilci sağlayıcıları**: başkalarının ortamınızdaki teklifler oluşturmanıza olanak verebilir. Örneğin, bir hizmet sağlayıcı barındırıyorsanız, yapabilecekleriniz [temsilci](azure-stack-delegated-provider.md) , yetkili satıcılar için bu özelliği. Veya, bir kuruluşun değilseniz, diğer bölümler/kuruluşlarının devredebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Bir teklifi Azure yığınında oluşturma](azure-stack-create-offer.md)
