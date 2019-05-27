---
title: Erişimi gözden geçir kendiniz gruplara veya erişim gözden geçirmeleri - Azure Active Directory uygulamaları | Microsoft Docs
description: Kendi erişim grupları ve uygulamaları Azure Active Directory erişim gözden geçirmeleri, gözden geçirmeyi öğrenin.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/21/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6b38422b22885ad96c6681abe58b897155809521
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66113197"
---
# <a name="review-access-for-yourself-to-groups-or-applications-in-azure-ad-access-reviews"></a>Erişimi gözden geçir kendiniz gruplara veya Azure AD uygulama erişim gözden geçirmeleri

Azure Active Directory (Azure AD) nasıl kuruluşlar, Azure AD'de grupları veya uygulamalara erişim yönetmek ve diğer Microsoft Çevrimiçi Hizmetler Azure AD erişim adlı bir özellik ile incelemeleri kolaylaştırır.

Bu makalede, bir grup veya uygulamanın kendi erişimi gözden geçirmek açıklar.

## <a name="prerequisites"></a>Önkoşullar

- Azure AD Premium P2

Daha fazla bilgi için [hangi kullanıcıların lisansına sahip olması gerekir?](access-reviews-overview.md#which-users-must-have-licenses).

## <a name="open-the-access-review"></a>Erişim gözden geçirmesi açın

Erişim gözden geçirmesi gerçekleştirmek için ilk adım, erişim gözden geçirmesi açın sağlamaktır.

1. Erişim gözden geçirme ister Microsoft'tan bir e-posta için bakın. Bir grubu erişiminizi gözden geçirmek için bir örnek e-posta aşağıda verilmiştir.

    ![Gözden geçirme'e-posta erişimi](./media/review-your-access/access-review-email.png)

1. Tıklayın **erişimi gözden geçir** erişim gözden geçirmesi açmaya yönelik bağlantı.

E-posta yoksa, aşağıdaki adımları izleyerek, beklemedeki erişim gözden geçirmeleri bulabilirsiniz.

1. MyApps portalında oturum açın [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

    ![MyApps portalında](./media/review-your-access/myapps-access-panel.png)

1. Sayfanın sağ üst köşesinde yer alan ve adınızla varsayılan kuruluşunuzun gösterildiği kullanıcı simgesine tıklayın. Listede birden fazla kuruluş varsa erişim gözden geçirmesi isteğinde bulunan kuruluşu seçin.

1. Sayfanın sağ tarafında tıklayın **erişim gözden geçirmeleriyle** beklemedeki erişim gözden geçirmeleri listesini görmek için kutucuğu.

    Kutucuk yoksa ilgili kuruluş için bekleyen erişim gözden geçirmesi yoktur ve herhangi bir işlem yapmanız gerekmez.

    ![Erişim incelemeleri listesi](./media/review-your-access/access-reviews-list.png)

1. Tıklayın **başlamak gözden geçirme** gerçekleştirmek istediğiniz erişim gözden geçirmesi için bağlantı.

## <a name="perform-the-access-review"></a>Erişim değerlendirmesi gerçekleştirme

Erişim gözden geçirmesi açtıktan sonra erişiminizi görebilirsiniz.

1. Erişiminizi gözden geçirin ve erişim yine de ihtiyacınız olup olmadığına karar verebilirsiniz.

    İstek, diğerleri için erişim gözden geçirmek için ise sayfa farklı görünecektir. Daha fazla bilgi için [gruplar veya uygulamalar için erişimi gözden geçir](perform-access-review.md).

    ![Erişim değerlendirmesi gerçekleştirme](./media/review-your-access/perform-access-review.png)

1. Tıklayın **Evet** erişiminizi tutun veya **Hayır** erişiminizi kaldırmak için.

1. Tıklarsanız **Evet**, bir düzenlemede belirtmeniz gerekebilir **neden** kutusu.

    ![Erişim değerlendirmesi gerçekleştirme](./media/review-your-access/perform-access-review-submit.png)

1. **Gönder**'e tıklayın.

    Seçiminizi gönderilir ve MyApps portalında döndürdü.

    Yanıtınız değiştirmek istiyorsanız, erişim gözden geçirmeleri sayfasını yeniden açın ve yanıtınız güncelleştirin. Erişim gözden geçirmesi sona erinceye kadar herhangi bir zamanda yanıtınızı değiştirebilirsiniz.

    > [!NOTE]
    > Artık erişime ihtiyacınız belirtilmişse hemen kaldırılmaz. Gözden geçirme sona erdiğinde veya yöneticinin gözden durduğunda kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Grupları ve uygulamaları, erişim değerlendirmesi tamamlama](complete-access-review.md)
