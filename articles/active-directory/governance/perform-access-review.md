---
title: Gruplara veya Azure AD erişim gözden geçirmeleri uygulamalarda erişimi gözden geçir | Microsoft Docs
description: Grup üyelerinin erişim veya uygulama erişimi Azure Active Directory erişim gözden geçirmeleri, gözden geçirmeyi öğrenin.
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
ms.date: 02/20/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 097d230e919e6d4b56e6c677364610bda6630f75
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56728395"
---
# <a name="review-access-to-groups-or-applications-in-azure-ad-access-reviews"></a>Erişim grupları ve uygulamaları Azure AD erişim gözden geçirmeleri, gözden geçirin

Azure Active Directory (Azure AD), kuruluşların Azure AD'de grupları ve uygulamalara erişimi yönetme basitleştirir ve başka bir özellik ile Microsoft Çevrimiçi Hizmetler Azure AD erişim gözden geçirmeleri çağrılır.

Bu makalede nasıl üyeleri bir grup veya uygulamaya erişimi olan kullanıcılar için erişim gözden geçirmesi belirlenen bir Gözden Geçiren gerçekleştirdiğini açıklar.

## <a name="open-the-access-review"></a>Erişim gözden geçirmesi açın

Erişim gözden geçirmesi gerçekleştirmek için ilk adım, erişim gözden geçirmesi açın sağlamaktır.

1. Erişim gözden geçirme ister Microsoft'tan bir e-posta için bakın. Bir grup erişimini gözden geçirmek için bir örnek e-posta aşağıda verilmiştir.

    ![Gözden geçirme'e-posta erişimi](./media/perform-access-review/access-review-email.png)

1. Tıklayın **Başlat gözden geçirme** erişim gözden geçirmesi açmaya yönelik bağlantı.

E-posta yoksa, aşağıdaki adımları izleyerek, beklemedeki erişim gözden geçirmeleri bulabilirsiniz.

1. MyApps portalında oturum açın [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

    ![MyApps portalında](./media/perform-access-review/myapps-access-panel.png)

1. Sayfanın sağ üst köşede, adını ve varsayılan kuruluşunuz görüntüleyen kullanıcı simgeyi tıklatın. Birden fazla kuruluş listeleniyorsa, erişim gözden geçirmesi istenen kuruluş seçin.

1. Sayfanın sağ tarafında tıklayın **erişim gözden geçirmeleriyle** beklemedeki erişim gözden geçirmeleri listesini görmek için kutucuğu.

    Kutucuk görünür değilse, bu kuruluşa ait gerçekleştirmek için hiç erişim gözden geçirmesi yok ve şu anda hiçbir eylem gerekmiyor.

    ![Erişim incelemeleri listesi](./media/perform-access-review/access-reviews-list.png)

1. Tıklayın **başlamak gözden geçirme** gerçekleştirmek istediğiniz erişim gözden geçirmesi için bağlantı.

## <a name="perform-the-access-review"></a>Erişim değerlendirmesi gerçekleştirme

Erişim gözden geçirmesi açtıktan sonra gözden geçirilmesi gereken kullanıcılar adlarını görürsünüz.

İstek, kendi erişim gözden geçirmek için ise sayfa farklı görünecektir. Daha fazla bilgi için [erişimi gözden geçir kendiniz grupları ve uygulamaları için](review-your-access.md).

![Erişim değerlendirmesi gerçekleştirme](./media/perform-access-review/perform-access-review.png)

Onaylama veya reddetme erişim iki yolu vardır:

- Onaylayın veya ayrı ayrı her bir isteği reddetme veya
- Kolay ve hızlı bir yolu olan sistem önerileri kabul edebilir.

### <a name="approve-or-deny-access-for-each-request"></a>Onaylayın veya reddedin her istek için erişim

1. Onaylayın veya reddedin sürekli erişimleri karar vermek için kullanıcıların listesini gözden geçirin.

1. Onaylayın veya reddedin her istek için satırın gerçekleştirilecek eylemi belirtmek için penceresini açmak için tıklayın.

1. Tıklayın **onaylama** veya **Reddet**. Emin değilseniz, tıklayabilirsiniz **bilmiyorum**. Bunun yapılması, kullanıcının kendi erişimi sürdürmek neden olur, ancak seçimi denetim günlüklerinde yansıtılır.

    ![Erişim değerlendirmesi gerçekleştirme](./media/perform-access-review/approve-deny.png)

    Erişim gözden geçirmesi Yöneticisi sürekli erişim veya grup üyeliği onaylama için bir neden sağlamanızı gerektirebilir.

1. Gerçekleştirilecek eylemi belirledikten sonra tıklayın **Kaydet**.

    Yanıtınız değiştirmek istiyorsanız, bir satırı seçin ve yanıt güncelleştirin. Örneğin, önceden reddedilen bir kullanıcı tarafından onaylanması veya önceden onaylanmış bir kullanıcıyı engelle. Erişim gözden geçirmesi sona erinceye kadar herhangi bir zamanda yanıtınızı değiştirebilirsiniz.

    Birden çok Gözden Geçiren varsa, son gönderilen yanıt kaydedilir. Burada yöneticinin iki Gözden Geçiren – Alice ve Bob atar örneği göz önünde bulundurun. Alice, erişim gözden geçirmesi ilk açılır ve erişim onaylar. Gözden geçirme sona ermeden önce Bob erişim gözden geçirmesi açılır ve erişimi engeller. Son reddetme yanıttır ne kaydedilir.

    > [!NOTE]
    > Bir kullanıcının erişimi reddedilirse, hemen kaldırılmaz. Bunlar, gözden geçirme sona erdiğinde veya yöneticinin gözden durduğunda kaldırılır.

### <a name="approve-or-deny-access-based-on-recommendations"></a>Onaylayın veya reddedin önerilerine göre erişim

Erişim gözden geçirmeleri daha kolay ve hızlı sizin yerinize yapmasını isteyin de kabul edebileceği önerileri tek bir tıklamayla sunuyoruz. Öneriler, kullanıcının oturum açma etkinliklere göre oluşturulur.

1. Sayfanın alt kısmındaki mavi çubuğunda **önerileri kabul et**.

    ![Önerileri kabul et](./media/perform-access-review/accept-recommendations.png)

    Önerilen eylemleri özetini görürsünüz.

    ![Özet önerileri kabul et](./media/perform-access-review/accept-recommendations-summary.png)

1. Tıklayın **Tamam** önerileri kabul etmek için.

## <a name="next-steps"></a>Sonraki adımlar

- [Grupları ve uygulamaları, erişim değerlendirmesi tamamlama](complete-access-review.md)
