---
title: "Azure AD kullanarak erişim gözden erişim değerlendirmeleri | Microsoft Docs"
description: "Azure Active Directory erişimi incelemeler kullanarak erişim gözden geçirmek öğrenin."
services: active-directory
author: markwahl-msft
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: 98658a2b9fdd0ede98b3f6c10a19af527a8677cc
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="review-access-with-azure-ad-access-reviews"></a>Azure AD ile erişim gözden geçirme erişim gözden geçirme

Azure Active Directory (Azure AD) kuruluşların Azure AD'de uygulama ve grupların üyeleri erişimi yönetme ve diğer Microsoft Online Services erişimi adlı bir özelliği ile incelemeleri kolaylaştırır. Belki de, bir grubu üyeleri veya bir uygulamaya erişimi olan kullanıcılar için erişim gözden ister Microsoft'tan bir e-posta aldı. 

## <a name="open-an-access-review"></a>Bir erişim gözden geçirme açın

Bekleyen erişim incelemeler görmek için e-posta ile bağlantıyı seçin. E-posta yoksa, aşağıdaki adımları izleyerek erişim incelemeler bulabilirsiniz:

1. Oturum [Azure AD erişim paneli](https://myapps.microsoft.com).

2. Kullanıcı simgenin adını ve varsayılan kuruluşunuz görüntüler sayfanın sağ üst köşesinde seçin. Birden fazla organizasyon listeleniyorsa, erişim gözden geçirme istenen kuruluş seçin.

3. Bir kutucuk etiketli varsa **erişim incelemeler** sayfasında, sağ tarafta olduğu. Döşeme görünmüyorsa, bu kuruluş için gerçekleştirmek için erişim gözden geçirme vardır ve şu anda hiçbir eylem gerekmiyor.

## <a name="fill-out-an-access-review"></a>Bir erişim gözden geçirme doldurun

Listeden bir erişim gözden geçirme seçtiğinizde gözden geçirilmesi gereken kullanıcılar adları görürsünüz. Yalnızca bir adı--görebilirsiniz kendi--kendi access gözden geçirmek için istek olduğunda.

Listedeki her satır için onaylamak veya kullanıcının erişimini engellemek karar verebilirsiniz. Satırı seçin ve onaylamak veya reddetmek isteyip istemediğinizi seçin. (Kullanıcı bilmiyorsanız, çok belirtebilirsiniz.)

Gözden Geçiren sürekli erişim veya grup üyeliği onaylamak için bir düzeltme sağlamanızı gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bir kullanıcının reddedilen erişimi hemen kaldırılmaz. Gözden geçirme işlemi tamamlandığında veya bir yönetici gözden durduğunda kaldırılabilir. Yanıtınızı değiştirin ve önceden reddedilen bir kullanıcı tarafından onaylanması istiyorsanız veya önceden onaylanmış bir kullanıcıyı Reddet sırayı seçmek istiyorsanız sıfırlama yanıtı ve yeni bir yanıt seçin. Erişim gözden geçirme işlemi tamamlanana kadar bu adımı yapabilirsiniz.



