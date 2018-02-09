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
ms.date: 01/31/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: e6c94ef1172ea6380a94d5907c24069ed8c48ff5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="azure-stack-integrated-systems-connection-models"></a>Azure tümleşik yığını sistemleri bağlantı modelleri
Bir Azure tümleşik yığını sistemde düşünüyorsanız anlamanız gerekir [birçok veri merkezi tümleştirme konuları](azure-stack-datacenter-integration.md) sistem merkeziniz nasıl sığacak belirlemek Azure yığın dağıtımı için. Ayrıca, tam olarak nasıl, Azure yığın karma bulut ortamınıza tümleştirecek karar vermeniz gerekir. Bu makalede Azure bağlantısı, kimlik deposu dahil ve model kararları faturalama bu önemli kararlar genel bir bakış sağlar.

Tümleşik bir sistem satın almak karar verirseniz, daha ayrıntılı planlama işleminin büyük bölümü size kılavuzluk özgün donanım üreticisi (OEM) donanım satıcınıza yardımcı olur. Ayrıca gerçek dağıtım işlemini gerçekleştirecek.

## <a name="choose-an-azure-stack-deployment-connection-model"></a>Bir Azure yığın dağıtım bağlantı modeli seçin
Azure internet (ve Azure) bağlı veya bağlantısı kesilmiş yığını dağıtmayı seçebilirsiniz. Çoğu Azure Azure yığını ve Azure arasında karma senaryolar da dahil olmak üzere yığından elde etmek için dağıtmak için Azure bağlı istediğiniz. Bu seçenek, kimlik deposu (Azure Active Directory veya Active Directory Federasyon Hizmetleri) ve faturalama modeli için kullanılabilir olan seçenekleri tanımlar (, kullanım tabanlı ücret ödersiniz faturalama veya kapasite tabanlı fatura) Aşağıdaki diyagramda ve tabloda özetlenen: 

![Azure yığın dağıtımına ve senaryoları faturalama](media/azure-stack-connection-models/azure-stack-scenarios.png)  
  
> [!IMPORTANT]
> Bu bir anahtar karar noktasıdır! Active Directory Federasyon Hizmetleri (AD FS) veya Azure Active Directory (Azure AD) seçerek dağıtım sırasında yapmanız gereken tek seferlik bir karardır. Bu daha sonra tüm sistemin yeniden dağıtmadan değiştiremezsiniz.  


|Seçenekler|Azure'a bağlı|Azure'dan bağlantısı kesildi|
|-----|-----|-----|
|Azure AD|![Desteklenen](media/azure-stack-connection-models/check.png)| |
|AD FS|![Desteklenen](media/azure-stack-connection-models/check.png)|![Desteklenen](media/azure-stack-connection-models/check.png)|
|Tüketim tabanlı faturalama|![Desteklenen](media/azure-stack-connection-models/check.png)| |
|Kapasite tabanlı faturalama|![Desteklenen](media/azure-stack-connection-models/check.png)|![Desteklenen](media/azure-stack-connection-models/check.png)|
|Güncelleştirme paketleri doğrudan Azure yığın indirin|![Desteklenen](media/azure-stack-connection-models/check.png)|  |

Azure yığın dağıtımı için kullanılacak Azure bağlantı modelinde karar verdim sonra ek, bağlantı bağımlı kararları kimlik deposu ve fatura yöntemi için yapılması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

[Azure bağlı Azure yığın dağıtım kararları](azure-stack-connected-deployment.md)

[Azure Azure yığın dağıtım kararları bağlantısı kesildi](azure-stack-disconnected-deployment.md)
