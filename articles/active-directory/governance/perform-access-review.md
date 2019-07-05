---
title: Gruplar veya erişim gözden geçirmeleri - Azure Active Directory uygulamaları için erişimi gözden geçir | Microsoft Docs
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
ms.date: 05/21/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: b6f73d3bf5e502a758dd46561059c15a2970d9b6
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67471820"
---
# <a name="review-access-to-groups-or-applications-in-azure-ad-access-reviews"></a>Gruplara erişimi gözden geçirmek veya Azure AD uygulama erişim gözden geçirmeleri

Azure Active Directory (Azure AD) nasıl kuruluşlar, Azure AD'de gruplara ve uygulamalara erişimi yönetme ve diğer Microsoft Çevrimiçi Hizmetler Azure AD erişim adlı bir özellik ile inceler kolaylaştırır.

Bu makalede nasıl üyeleri bir grup veya uygulamaya erişimi olan kullanıcılar için erişim gözden geçirmesi belirlenen bir Gözden Geçiren gerçekleştirdiğini açıklar.

## <a name="prerequisites"></a>Önkoşullar

- Azure AD Premium P2

Daha fazla bilgi için [hangi kullanıcıların lisansına sahip olması gerekir?](access-reviews-overview.md#which-users-must-have-licenses).

## <a name="open-the-access-review"></a>Erişim gözden geçirmesi açın

Erişim gözden geçirmesi gerçekleştirmek için ilk adım, erişim gözden geçirmesi açın sağlamaktır.

1. Erişim gözden geçirme ister Microsoft'tan bir e-posta için bakın. Bir grup erişimini gözden geçirmek için bir örnek e-posta aşağıda verilmiştir.

    ![Örnek e-postanın bir gruba erişimi gözden geçirmek için Microsoft](./media/perform-access-review/access-review-email.png)

1. Tıklayın **Başlat gözden geçirme** erişim gözden geçirmesi açmaya yönelik bağlantı.

E-posta yoksa, aşağıdaki adımları izleyerek, beklemedeki erişim gözden geçirmeleri bulabilirsiniz.

1. MyApps portalında oturum açın [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

    ![MyApps portalında izinlerine sahip uygulamalar listesi](./media/perform-access-review/myapps-access-panel.png)

1. Sayfanın sağ üst köşesinde yer alan ve adınızla varsayılan kuruluşunuzun gösterildiği kullanıcı simgesine tıklayın. Listede birden fazla kuruluş varsa erişim gözden geçirmesi isteğinde bulunan kuruluşu seçin.

1. Tıklayın **erişim gözden geçirmeleriyle** beklemedeki erişim gözden geçirmeleri listesini görmek için kutucuğu.

    Kutucuk yoksa ilgili kuruluş için bekleyen erişim gözden geçirmesi yoktur ve herhangi bir işlem yapmanız gerekmez.

    ![Uygulamaları ve gruplar için beklemedeki erişim gözden geçirmeleri listesi](./media/perform-access-review/access-reviews-list.png)

1. Tıklayın **başlamak gözden geçirme** gerçekleştirmek istediğiniz erişim gözden geçirmesi için bağlantı.

## <a name="perform-the-access-review"></a>Erişim değerlendirmesi gerçekleştirme

Erişim gözden geçirmesi açtıktan sonra gözden geçirilmesi gereken kullanıcılar adlarını görürsünüz.

İstek, kendi erişim gözden geçirmek için ise sayfa farklı görünecektir. Daha fazla bilgi için [erişimi gözden geçir kendiniz grupları ve uygulamaları için](review-your-access.md).

![Açık erişim gözden geçirilmesi gereken kullanıcıları listeleme](./media/perform-access-review/perform-access-review.png)

Onaylama veya reddetme erişim iki yolu vardır:

- Onaylayın veya bir veya daha fazla kullanıcı erişimi reddetmek veya
- Kolay ve hızlı bir yolu olan sistem önerileri kabul edebilir.

### <a name="approve-or-deny-access-for-one-or-more-users"></a>Onaylamak veya reddetmek için bir veya daha fazla kullanıcı erişimi

1. Onaylayın veya reddedin sürekli erişimleri karar vermek için kullanıcıların listesini gözden geçirin.

1. Onaylayın veya tek bir kullanıcı erişimi reddetmek için satırın gerçekleştirilecek eylemi belirtmek için bir pencere açmak için tıklayın. Onaylayın veya reddedin birden çok kullanıcı için erişim için kullanıcıların yanında onay işareti ekleyin ve ardından **gözden geçirme X kullanıcı** gerçekleştirilecek eylemi belirtmek için bir pencere açmak için düğmeyi.

1. Tıklayın **onaylama** veya **Reddet**. Emin değilseniz, tıklayabilirsiniz **bilmiyorum**. Bunun yapılması, kullanıcının kendi erişimini koruma neden olur, ancak seçimi denetim günlüklerinde yansıtılır.

    ![Onayla, reddet, içeren eylem penceresi ve seçenekleri bilmiyorum](./media/perform-access-review/approve-deny.png)

1. Gerekirse, bir neden girin **neden** kutusu.

    Erişim gözden geçirmesi Yöneticisi sürekli erişim veya grup üyeliği onaylama için bir neden sağlamanızı gerektirebilir.

1. Gerçekleştirilecek eylemi belirledikten sonra tıklayın **Kaydet**.

    Yanıtınız değiştirmek istiyorsanız, bir satırı seçin ve yanıt güncelleştirin. Örneğin, önceden reddedilen bir kullanıcı tarafından onaylanması veya önceden onaylanmış bir kullanıcıyı engelle. Erişim gözden geçirmesi sona erinceye kadar herhangi bir zamanda yanıtınızı değiştirebilirsiniz.

    Birden çok Gözden Geçiren varsa, son gönderilen yanıt kaydedilir. Burada yöneticinin iki Gözden Geçiren – Alice ve Bob atar örneği göz önünde bulundurun. Alice, erişim gözden geçirmesi ilk açılır ve erişim onaylar. Gözden geçirme sona ermeden önce Bob erişim gözden geçirmesi açılır ve erişimi engeller. Son reddetme yanıttır ne kaydedilir.

    > [!NOTE]
    > Bir kullanıcının erişimi reddedilirse, hemen kaldırılmaz. Bunlar, gözden geçirme sona erdiğinde veya yöneticinin gözden durduğunda kaldırılır.

### <a name="approve-or-deny-access-based-on-recommendations"></a>Onaylayın veya reddedin önerilerine göre erişim

Erişim gözden geçirmeleri daha kolay ve hızlı sizin yerinize yapmasını isteyin de kabul edebileceği önerileri tek bir tıklamayla sunuyoruz. Öneriler, kullanıcının oturum açma etkinliklere göre oluşturulur.

1. Sayfanın alt kısmındaki mavi çubuğunda **önerileri kabul et**.

    ![Önerileri kabul et düğmesi gösteren listeleme açık erişim gözden geçirme](./media/perform-access-review/accept-recommendations.png)

    Önerilen eylemleri özetini görürsünüz.

    ![Önerilen eylemleri özetini görüntüler penceresi](./media/perform-access-review/accept-recommendations-summary.png)

1. Tıklayın **Tamam** önerileri kabul etmek için.

## <a name="next-steps"></a>Sonraki adımlar

- [Grupları ve uygulamaları, erişim değerlendirmesi tamamlama](complete-access-review.md)
