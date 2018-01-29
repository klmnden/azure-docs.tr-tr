---
title: "Azure yığın tümleşik sistemleri bağlantı modelleri | Microsoft Docs"
description: "Dağıtım Planlama kararları çok düğümlü Azure yığınının belirler."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: ff56ced9f1d43780097cd65e58b492daaba9b61d
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="azure-stack-integrated-systems-connection-models"></a>Azure tümleşik yığını sistemleri bağlantı modelleri
Bir Azure tümleşik yığını sistemde düşünüyorsanız anlamanız gerekir [birçok veri merkezi tümleştirme konuları](azure-stack-datacenter-integration.md) sistem merkeziniz nasıl sığacak belirlemek Azure yığın dağıtımı için. Ayrıca, tam olarak nasıl, Azure yığın karma bulut ortamınıza tümleştirecek karar vermeniz gerekir. Bu makalede Azure bağlantısı, kimlik deposu dahil ve model kararları faturalama bu önemli kararlar genel bir bakış sağlar.

Tümleşik bir sistem satın almak karar verirseniz, daha ayrıntılı planlama işleminin büyük bölümü size kılavuzluk özgün donanım üreticisi (OEM) donanım satıcınıza yardımcı olur. Ayrıca gerçek dağıtım işlemini gerçekleştirecek.

## <a name="choose-an-azure-stack-deployment-connection-model"></a>Bir Azure yığın dağıtım bağlantı modeli seçin
Azure internet (ve Azure) bağlı veya bağlantısı kesilmiş yığını dağıtmayı seçebilirsiniz. Çoğu Azure Azure yığını ve Azure arasında karma senaryolar da dahil olmak üzere yığından elde etmek için dağıtmak için Azure bağlı istediğiniz. Bu seçenek, kimlik deposu (Azure Active Directory veya Active Directory Federasyon Hizmetleri) ve faturalama modeli için kullanılabilir olan seçenekleri tanımlar (, kullanım tabanlı ücret ödersiniz faturalama veya kapasite tabanlı fatura) Aşağıdaki diyagramda ve tabloda özetlenen: 

![Azure yığın dağıtımına ve senaryoları faturalama](media/azure-stack-deployment-decisions/azure-stack-scenarios.png)   
  
> [!IMPORTANT]
> Bu bir anahtar karar noktasıdır! Active Directory Federasyon Hizmetleri (AD FS) veya Azure Active Directory (Azure AD) seçerek dağıtım sırasında yapmanız gereken tek seferlik bir karardır. Bu daha sonra tüm sistemin yeniden dağıtmadan değiştiremezsiniz.  


|Seçenekler|Azure'a bağlı|Azure'dan bağlantısı kesildi|
|-----|-----|-----|
|Azure AD|![Desteklenen](media/azure-stack-deployment-decisions/check.png)| |
|AD FS|![Desteklenen](media/azure-stack-deployment-decisions/check.png)|![Desteklenen](media/azure-stack-deployment-decisions/check.png)|
|Tüketim tabanlı faturalama|![Desteklenen](media/azure-stack-deployment-decisions/check.png)| |
|Kapasite tabanlı faturalama|![Desteklenen](media/azure-stack-deployment-decisions/check.png)|![Desteklenen](media/azure-stack-deployment-decisions/check.png)|
|Güncelleştirme paketleri doğrudan Azure yığın indirin|![Desteklenen](media/azure-stack-deployment-decisions/check.png)|  |

Azure yığın dağıtımı için kullanılacak Azure bağlantı modelinde karar verdim sonra ek, bağlantı bağımlı kararları kimlik deposu ve fatura yöntemi için yapılması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [Azure bağlı Azure yığın dağıtım kararları](azure-stack-connected-deployment.md)
- Daha fazla bilgi edinmek [Azure Azure yığın dağıtım kararları bağlantısı kesildi](azure-stack-disconnected-deployment.md)
