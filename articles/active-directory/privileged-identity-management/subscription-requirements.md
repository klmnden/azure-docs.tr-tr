---
title: Privileged Identity Management abonelikler - Azure | Microsoft Docs
description: Abonelik ve lisans gereksinimleri için kiracınızda Azure AD Privileged Identity Management'ı kullanmaya ve yönetmeye açıklar
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 06/01/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 2fe80f01faae89256c96e23944025d3bd0c55811
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39617199"
---
# <a name="azure-active-directory-privileged-identity-management-subscription-requirements"></a>Azure Active Directory Privileged Identity Management abonelik gereksinimleri

Azure AD Privileged Identity Management kullanılabilir Azure AD Premium P2 sürümü bir parçası olarak. P2 ve nasıl Premium P1 karşılaştırır, diğer özellikler hakkında daha fazla bilgi için bkz. [Azure Active Directory sürümleri](../active-directory-editions.md).

>[!NOTE]
Azure Active Directory (Azure AD) Privileged Identity Management Önizleme aşamasında olan, hizmeti denemek bir kiracı için hiçbir lisans denetim vardı.  Azure AD Privileged Identity Management'a genel kullanılabilirlik ulaştı, deneme veya Ücretli aboneliği için Kiracı, Privileged Identity Management aralık 2016'dan sonra kullanmaya devam etmek mevcut olması gerekir.
  

## <a name="confirm-your-trial-or-paid-subscription"></a>Deneme veya Ücretli aboneliğinizi doğrulayın

Aboneliği satın aldığı veya kuruluşunuzun bir deneme sürümü olup emin değilseniz, bulunup bulunmadığını kiracınızdaki bir aboneliği Azure Active Directory modülü için Windows PowerShell V1'de bulunan komutları kullanarak kontrol edebilirsiniz. 
1. Bir PowerShell penceresi açın.
2. Girin `Connect-MsolService` kiracınızdaki bir kullanıcı olarak kimlik doğrulaması için.
3. Girin `Get-MsolSubscription | ft SkuPartNumber,IsTrial,Status`.

Bu komut, kiracınızdaki Aboneliklerin listesini alır. Döndürülen satır varsa, bir Azure AD Premium P2 aboneliği veya Azure AD Privileged Identity Management'ı kullanmak için EMS E5 aboneliği bir Azure AD Premium P2 deneme satın almak gerekir.  Bir deneme sürümü ve Azure AD Privileged Identity Management'ı kullanmaya başlayın almak için okuma [Azure AD Privileged Identity Management ile çalışmaya başlama](pim-getting-started.md).

Bu komut bir satır döndürürse hangi SkuPartNumber "AAD_PREMIUM_P2" veya "EMSPREMIUM" ve IsTrial "True" ise, bu kiracıda mevcut bir Azure AD Premium P2 deneme sürümü olduğunu gösterir.  Sonra abonelik durumu etkin değil ve bir Azure AD Premium P2 veya EMS E5 aboneliği satın almak zorunda kalmazsınız, bir Azure AD Premium P2 aboneliği veya Azure AD Privileged Identity Management'ı kullanmaya devam etmek için EMS E5 aboneliği satın almalısınız.

Azure AD Premium P2 aracılığıyla bir [Microsoft Kurumsal anlaşması](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), [açık Toplu Lisanslama programı](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx)ve [bulut çözüm sağlayıcıları program](https://partner.microsoft.com/en-US/cloud-solution-provider). Azure ve Office 365 aboneleri, Azure AD Premium P2 çevrimiçi da satın alabilir.  Azure AD Premium fiyatlandırma ve çevrimiçi sipariş et hakkında daha fazla bilgi şu adreste bulunabilir: [Azure Active Directory fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="azure-ad-privileged-identity-management-is-not-available-in-tenant"></a>Azure AD Privileged Identity Management kiracısında mevcut değil

Azure AD Privileged Identity Management artık kiracınızda kullanılabilir olacak varsa:
- Önizleme aşamasında olan ve Azure AD Premium P2 aboneliği veya EMS E5 aboneliği satın değil, kuruluşunuz Azure AD Privileged Identity Management kullanıyordu.
- Kuruluşunuzun bir Azure AD Premium P2 ya da EMS süresi E5 deneme sürümü vardı.
- Kuruluşunuz, süresi dolan satın alınan bir Aboneliğim vardı.

Bir Azure AD Premium P2 aboneliği veya EMS E5 aboneliği sona erdiğinde veya bir önizlemede Azure AD Privileged Identity Management kullanan kuruluş Azure AD Premium P2 veya EMS E5 aboneliği edinmek değil:

- Azure AD rollerine kalıcı rol atamaları etkilenmez.
- Bu Azure AD Privileged Identity Management uzantısı'nda Azure portalı, yanı sıra Graph API cmdlet'leri ve Azure AD Privileged Identity Management PowerShell arabirimleri kullanıcıların ayrıcalıklı rolleri etkinleştirmesine, ayrıcalıklı yönetmek için kullanılabilir erişim gözden geçirmeleri ayrıcalıklı rolleri gerçekleştirmek veya erişin.
- Bu kullanıcılar ayrıcalıklı rolleri etkinleştirmesine mümkün Azure AD rolleri, uygun rol atamaları kaldırılacak.
- Azure AD rolleri tüm devam eden bir erişim gözden geçirmelerini sona erecek ve Azure AD Privileged Identity Management yapılandırma ayarları kaldırılır.
- Azure AD Privileged Identity Management, e-postaları artık rol atama değişiklikleri gönderir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD Privileged Identity Management ile çalışmaya başlama](pim-getting-started.md)
- [Azure AD Privileged Identity Management içinde rol](pim-roles.md)
