---
title: Azure AD'yi kullanarak erişimi gözden geçir erişim gözden geçirmeleriyle | Microsoft Docs
description: Azure Active Directory erişim gözden geçirmelerini kullanarak erişim gözden geçirmeyi öğrenin.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 07/16/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: 09247a430d2ece44c307297838b7f4dc2819d56d
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45607874"
---
# <a name="review-access-with-azure-ad-access-reviews"></a>Azure AD ile erişimi gözden geçir erişim gözden geçirmeleri

Azure Active Directory (Azure AD) nasıl kuruluşlar, Azure AD'de uygulamaları ve grupların üyeleri erişim yönetmek ve diğer çevrimiçi Microsoft hizmetlerine erişim adlı bir özellik ile incelemeleri kolaylaştırır. Belki de üyeleri bir grup veya uygulamaya erişimi olan kullanıcılar için erişim gözden geçirme ister Microsoft'tan bir e-posta aldı. 

## <a name="open-an-access-review"></a>Erişim gözden geçirmesi açın

Beklemedeki erişim gözden geçirmeleri görmek için gözden geçirme erişim bağlantıya tıklayın. Ağustos 2018'den itibaren Azure AD rolleri için e-posta bildirimleri güncelleştirilmiş bir tasarım sahip. Bir kullanıcı bir Gözden Geçiren olarak davet, gönderilen örnek e-posta aşağıda gösterilmiştir. 

![Gözden geçirme'e-posta erişimi](./media/perform-access-review/new-ar-email.png)

E-posta yoksa, aşağıdaki adımları izleyerek erişim gözden geçirmeleri bulabilirsiniz:

1. Oturum [Azure AD erişim paneli](https://myapps.microsoft.com).

2. Kullanıcı sembol adını ve varsayılan kuruluşunuz görüntüleyen sayfanın sağ üst köşesinde bulunan seçin. Birden fazla kuruluş listeleniyorsa, erişim gözden geçirmesi istenen kuruluş seçin.

3. Bir kutucuk olarak etiketlenmiş, **erişim gözden geçirmeleriyle** seçin sayfasında, sağ tarafında olduğu. Kutucuk görünür değilse, bu kuruluşa ait gerçekleştirmek için hiç erişim gözden geçirmesi yok ve şu anda hiçbir eylem gerekmiyor.

## <a name="fill-out-an-access-review"></a>Erişim gözden geçirmesi doldurun

Erişim gözden geçirmesi listeden seçtiğiniz gözden geçirilmesi gereken kullanıcılar adlarını görürsünüz. Yalnızca bir adı--görebilirsiniz, kendi--kendi erişim gözden geçirmek için istek durdurulmuşsa.

Listedeki her bir satır için onaylamak veya kullanıcının erişimini engellemek karar verebilirsiniz. Satırı seçin ve onaylamak veya reddetmek isteyip istemediğinizi seçin. (Kullanıcı emin değilseniz, bu çok belirtebilirsiniz.)

Gözden Geçiren bir yaslama, sürekli erişimin ya da grup üyeliği onaylama sağladığınız gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcının reddedilen erişimi hemen kaldırılmaz. Gözden Geçirme tamamlandığında veya yöneticinin gözden durduğunda kaldırılabilir. Yanıtınızı değiştirin ve önceden reddedilen bir kullanıcı tarafından onaylanması istiyorsanız veya önceden onaylanmış bir kullanıcıyı Reddet satırı seçin, yanıt sıfırlama ve yeni bir yanıt seçin. Erişim gözden geçirme işlemi tamamlanana kadar bu adımı yapabilirsiniz.



