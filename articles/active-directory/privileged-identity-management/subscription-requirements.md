---
title: Kimlik Yönetimi abonelikleri - Azure ayrıcalıklı | Microsoft Docs
description: Abonelik ve lisans yönetimi ve Azure AD Privileged Identity Management kiracınızda kullanma gereksinimleri açıklanır
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
ms.topic: article
ms.date: 06/01/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 69a27a2a75eb2a08a93b8b70648733673eac36db
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260055"
---
# <a name="azure-active-directory-privileged-identity-management-subscription-requirements"></a>Azure Active Directory Privileged Identity Management abonelik gereksinimleri

Azure AD Privileged Identity Management kullanılabilir Azure ad Premium P2 sürümünün bir parçası olarak. P2 ve nasıl Premium P1 karşılaştırır diğer özellikleri hakkında daha fazla bilgi için bkz: [Azure Active Directory sürümleri](../active-directory-editions.md).

>[!NOTE]
Azure Active Directory (Azure AD) Privileged Identity Management önizlemede olduğunda, hizmeti denemek bir kiracı için herhangi bir lisans denetim vardı.  Azure AD Privileged Identity Management genel kullanılabilirlik ulaştı, bir deneme aboneliği veya Ücretli abonelik aralık 2016 sonrasında Privileged Identity Management kullanmaya devam etmek Kiracı için mevcut olması gerekir.
  

## <a name="confirm-your-trial-or-paid-subscription"></a>Deneme aboneliği veya Ücretli aboneliğinizi onaylayın

Abonelik satın veya kuruluşunuzun bir deneme sürümü olup emin değilseniz, olup olmadığını kiracınızda bir abonelik Azure Active Directory modülü için Windows PowerShell V1 dahil komutlarını kullanarak kontrol edebilirsiniz. 
1. Bir PowerShell penceresi açın.
2. Girin `Connect-MsolService` kiracınızda bir kullanıcı olarak kimlik doğrulaması için.
3. Girin `Get-MsolSubscription | ft SkuPartNumber,IsTrial,Status`.

Bu komut kiracınızda Aboneliklerin listesini alır. Döndürülen satır varsa, Azure AD Premium P2 deneme, satın alma bir Azure AD Premium P2 aboneliği veya Azure AD Privileged Identity Management kullanmak için EMS E5 abonelik edinmeniz gerekir.  Bir deneme ve Azure AD Privileged Identity Management kullanarak başlangıç almak için okuma [Azure AD Privileged Identity Management ile çalışmaya başlama](../active-directory-privileged-identity-management-getting-started.md).

Bu komut bir satır döndürürse hangi SkuPartNumber "AAD_PREMIUM_P2" veya "EMSPREMIUM" ve IsTrial "True" ise, bu Azure AD Premium P2 deneme kiracısı'nda mevcut olduğunu gösterir.  Ardından abonelik durumunu etkin değil ve satın alma bir Azure AD Premium P2 ya da EMS E5 aboneliğiniz yoksa, bir Azure AD Premium P2 aboneliği veya Azure AD Privileged Identity Management kullanmaya devam etmek için EMS E5 abonelik satın almalısınız.

Azure AD Premium P2 aracılığıyla kullanılabilen bir [Microsoft Enterprise sözleşmesi](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), [açık toplu lisans programı](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx)ve [bulut çözüm sağlayıcıları program](https://partner.microsoft.com/en-US/cloud-solution-provider). Azure ve Office 365 aboneleri de Azure AD Premium P2 çevrimiçi satın alabilirsiniz.  Azure AD Premium fiyatlandırma ve çevrimiçi sipariş konusunda daha fazla bilgi bulunabilir [Azure Active Directory fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="azure-ad-privileged-identity-management-is-not-available-in-tenant"></a>Azure AD Privileged Identity Management kiracısında kullanılamıyor

Azure AD Privileged Identity Management artık kullanılabilir kiracınızda varsa:
- Önizlemede olan ve Azure AD Premium P2 abonelik ya da EMS E5 abonelik satın değil, kuruluşunuz Azure AD Privileged Identity Management kullanıyordu.
- Kuruluşunuz Azure AD Premium P2 ya da EMS süresi dolan E5 deneme vardı.
- Kuruluşunuz süresi satın alınan bir abonelik vardı.

Bir Azure AD Premium P2 abonelik ya da EMS E5 abonelik süresi dolduğunda veya bir Azure AD Privileged Identity Management önizlemede kullanarak kuruluş Azure AD Premium P2 ya da EMS E5 aboneliği edinmek değil:

- Azure AD rol kalıcı rol atamalarını etkilenmeyecek.
- Bu Azure AD Privileged Identity Management uzantısı'nda Azure portal yanı sıra grafik API'si cmdlet'leri ve Azure AD Privileged Identity Management PowerShell arabirimleri kullanıcıların ayrıcalıklı rolleri etkinleştirmesine, ayrıcalıklı yönetmek için kullanılabilir erişim veya ayrıcalıklı rolleri erişim incelenmesi gerçekleştirin.
- Bu kullanıcılar ayrıcalıklı rollerini etkinleştirebilir olarak Azure AD rollerin uygun rol atamalarını kaldırılacak.
- Tüm Azure AD rolleri devam eden erişim incelenmesi sona erer ve Azure AD Privileged Identity Management yapılandırma ayarları kaldırılır.
- Azure AD Privileged Identity Management, artık rol atama değişiklikleri e-posta gönderir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD Privileged Identity Management ile çalışmaya başlama](../active-directory-privileged-identity-management-getting-started.md)
- [Azure AD Privileged Identity Management rollerinde](../active-directory-privileged-identity-management-roles.md)
