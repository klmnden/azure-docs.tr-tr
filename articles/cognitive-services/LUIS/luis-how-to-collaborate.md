---
title: LUIS uygulamalarda başkalarıyla işbirliği yapın
titleSuffix: Azure Cognitive Services
description: Uygulamanın sahibi ortak çalışanlar için uygulama ekleyebilirsiniz. Bu ortak çalışanlar modeli değiştirirseniz eğitmek ve uygulamayı yayımlayın.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 3ae31aaea76e5f4a34614088728269f69ac4a5cf
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45632611"
---
# <a name="how-to-manage-authors-and-collaborators"></a>Yazarlar ve ortak çalışanlar yönetme 

Uygulamanın sahibi ortak çalışanlar için uygulama ekleyebilirsiniz. Bu ortak çalışanlar modeli değiştirirseniz eğitmek ve uygulamayı yayımlayın. 

<a name="owner-and-collaborators"></a>

## <a name="add-collaborator"></a>Ortak çalışanı Ekle

Uygulama sahibi, tek bir yazar vardır ancak birçok ortak çalışanlar çıkarabilirsiniz. LUIS uygulamanızı düzenlemek ortak çalışanlar izin vermek için ortak çalışanlar listesini LUIS portala erişmek için kullandıkları e-posta eklemeniz gerekir. Bunlar eklendikten sonra uygulama kendi LUIS portalda görüntülenir.

1. Seçin **Yönet** seçip sağ üstteki menüden **ortak çalışanlar** soldaki menüde.

2. Seçin **ortak çalışanı Ekle** araç çubuğundan.

    [![](./media/luis-how-to-collaborate/add-collaborator.png "Ortak çalışanı Ekle")](./media/luis-how-to-collaborate/add-collaborator.png#lightbox)

3. Ortak çalışanı LUIS portalda oturum açmak için kullandığı e-posta adresi girin.

    ![Ortak çalışan kişinin e-posta adresi ekleyin](./media/luis-how-to-collaborate/add-collaborator-pop-up.png)

## <a name="transfer-of-ownership"></a>Sahipliğin aktarılması

LUIS sahipliğin aktarılması şu anda desteklemiyor, ancak uygulamanızı dışa aktarabilir ve LUIS kullanıcı başka bir uygulamayı içeri aktarabilirsiniz. LUIS puanları iki uygulama arasındaki küçük farklılıklar olabilir. 

## <a name="azure-active-directory-resources"></a>Azure Active Directory kaynakları

LUIS kullanmak istedikleri zaman LUIS, kuruluşunuzda Azure Active Directory (Azure AD) kullanırsanız, kullanıcılarınızla ilgili bilgilere erişmek için izne gerek duyar. LUIS gerektirdiği kaynakların düşüktür. 

Yönetici onayı sahip veya yönetici onayı gibi yönetici onayı gerektirmeyen bir hesapla oturum açmaya çalıştığında ayrıntılı açıklamasına bakın:

* Uygulamada Kurumsal hesabınızla oturum açın ve profilinizi okuyabilmesine sağlar. Temel şirket bilgilerini okumasına izin verir.
* Uygulamanın bakın ve bile uygulama kullanmakta olduğunuz değil, verilerinizin güncelleştirmesine izin verir.

LUIS, kullanıcı kimliği, e-posta, adı gibi temel profil verileri okuma izni olan ilk izin verir. İkinci izin kullanıcının erişim belirtecini yenilemek için gereklidir.

## <a name="azure-active-directory-tenant-user"></a>Azure Active Directory Kiracı Kullanıcı

LUIS standart Azure Active Directory (Azure AD) onay akışı kullanır. 

Kiracı yönetici, doğrudan Azure AD'de LUIS kullanmak için verilen erişmesi gereken kullanıcı ile çalışması gerekir. 

İlk olarak, kullanıcı HLUISimzalar ve yönetici onayı gerek açılan iletişim görür. Kullanıcı, Kiracı Yöneticisi devam etmeden önce bağlantı kurar. 

İkinci olarak, Kiracı yönetici HLUISimzalar ve bir onay akış açılan iletişim görür. Yönetici kullanıcı için izin vermek için gereken iletişim budur. Yönetici izni kabul ettiğinde, kullanıcı ile LUIS devam edebilirsiniz.

Kiracı yönetici LUIS için oturum açacak değil, yönetici erişebilir [onayı](https://account.activedirectory.windowsazure.com/r#/applications) LUIS için. 

![Uygulama Web sitesi tarafından Azure active directory izni](./media/luis-how-to-collaborate/tenant-permissions.png)

Kiracı yönetici yalnızca belirli kullanıcıların LUIS kullanmasını isterse, bunun için başvuruda [kimliği blogu](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/).

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Ortak Çalışanlar için birden çok e-posta ile kullanıcı hesapları

Bir LUIS uygulaması için ortak çalışanlar eklerseniz, bir ortak çalışanı LUIS ortak çalışan kullanması gereken tam e-posta adresi belirtiyorsunuz. LUIS, Azure Active Directory (Azure AD) tek bir kullanıcı kullanılabileceğinden, birden fazla e-posta hesabına sahip olmasını sağlar, ancak ortak çalışanı'nın listesinde belirtilen e-posta adresiyle oturum açmasını gerektirir.

