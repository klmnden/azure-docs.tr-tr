---
title: Azure Active Directory PoC Playbook malzemeleri | Microsoft Docs
description: "Keşfetmek ve hızlı bir şekilde kimlik ve erişim yönetimi senaryoları uygulayan"
services: active-directory
keywords: "Azure active directory, playbook, kavram kanıtı, PT"
documentationcenter: 
author: dstefanMSFT
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: dstefan
ms.openlocfilehash: ff4a8601b619837d835ec25c26b1f7e69b46ae85
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-ingredients"></a>Azure Active Directory Playbook malzemeleri kavram kanıtı 

## <a name="theme"></a>Tema
Azure AD kimlik ve erişim çözümleri birden fazla alanda kuruluş genelinde sağlar. Biz, aşağıdaki alanlarda senaryolarda sınıflandırmak: 

* [Birçok uygulama, bir kimlik](active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity) 
* [Güvenlik Artırma](active-directory-playbook-implementation.md#theme---increase-your-security) 
* [Self Servis ölçekli](active-directory-playbook-implementation.md#theme---scale-with-self-service) 

İş hedeflerini ile denenmemesi PoC yardımcı olan ilgili olarak odaklanmanız kare için bir tema tanımlama, görmemeleri kavram kanıtı ilgiyi Tetikleyicileri ilk başta olduğu. 

## <a name="environment"></a>Ortam

Burada PoC teslim eder ortamı ayrıntılarını belirlemek önemlidir. İdeal olarak PoC tamamlandıktan sonra onun üzerinde oluşturabilirsiniz. Hedef ortamı çok önemlidir ve olabildiğince gerçek ve sınırlamalar veya ek konular yükü yapmadan arasında doğru dengeyi bulmanız gerekir. Tipik ortamları kanıtları için şunlardır:
* **Üretim:** senaryolar Canlı ortamınızda uygulanan ve Microsoft Cloud services (üretim AD, Office 365, Azure AD Kiracı/SSO çözüm) zaten dağıtılmış. 
* **Kullanıcı kabul testi (UAT) / geliştirme ortamında:** test altyapısına sahip (AD ve potansiyel olarak Azure AD paralel Kiracı/SSO çözüm) test verilerle üretim benzer. Genellikle, test ortamı kuruluştaki birden çok projeler arasında paylaşılır.

Bu kılavuzdaki çoğu senaryolar yapısı eklenebilir. Sonuç olarak, bunlar üretim Kiracı PoC dışındaki kullanıcılar etkilemeden dağıtılabilir. Bu belge boyunca biz Kiracı genelinde etkisi hangi senaryolar yoktur çağırma. Bu durumda, bir üretim ortamı dışındaki düşünmek isteyebilirsiniz. 


## <a name="target-users"></a>Hedef Kullanıcılar

Özellikle ortam üretim ya da test olduğunda senaryoları çalışma kullanıcıları hedef belirlemek önemlidir. Hedef Kullanıcılar PoC için kategorileri şunlardır:
* **Pilot kullanıcıları:** çözümü kendi günlük iş işlevleri için kullandıkları hesabıyla kullanarak ortamında gerçek kullanıcıları
* **Test kullanıcılarını:** Test ortamında oluşturulan hesaplar 

Bu kılavuz Çoğu senaryoda, pilot kullanıcılar tarafından kullandı. Bu belge boyunca biz hedef kullanıcı konuları gerekirse sizi arayacaktır.


[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]