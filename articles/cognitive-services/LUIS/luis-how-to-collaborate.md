---
title: Başkalarıyla işbirliği yapın
titleSuffix: Language Understanding - Azure Cognitive Services
description: Uygulamanın sahibi ortak çalışanlar için uygulama ekleyebilirsiniz. Bu ortak çalışanlar modeli değiştirirseniz eğitmek ve uygulamayı yayımlayın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 02/12/2019
ms.author: diberry
ms.openlocfilehash: f23212a854fb37dda89fd2bf6b223cf0dc69526b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60198768"
---
# <a name="how-to-manage-authors-and-collaborators"></a>Yazarlar ve ortak çalışanlar yönetme 

Uygulamanın sahibi ortak çalışanlar için uygulama ekleyebilirsiniz. Bu ortak çalışanlar modeli değiştirirseniz eğitmek ve uygulamayı yayımlayın. 

<a name="owner-and-collaborators"></a>

## <a name="add-collaborator"></a>Ortak çalışanı Ekle

Uygulama sahibi, tek bir yazar vardır ancak birçok ortak çalışanlar çıkarabilirsiniz. LUIS uygulamanızı düzenlemek ortak çalışanlar izin vermek için ortak çalışanlar listesini LUIS portala erişmek için kullandıkları e-posta eklemeniz gerekir. Bunlar eklendikten sonra uygulama kendi LUIS portalda görüntülenir.

1. Seçin **Yönet** seçip sağ üstteki menüden **ortak çalışanlar** soldaki menüde.

2. Seçin **ortak çalışanı Ekle** araç çubuğundan.

    [![Ekle ortak çalışanı](./media/luis-how-to-collaborate/add-collaborator.png "Ekle ortak çalışanı")](./media/luis-how-to-collaborate/add-collaborator.png#lightbox)

3. Ortak çalışanı LUIS portalda oturum açmak için kullandığı e-posta adresi girin.

    ![Ortak çalışan kişinin e-posta adresi ekleyin](./media/luis-how-to-collaborate/add-collaborator-pop-up.png)

## <a name="transfer-of-ownership"></a>Sahipliğin aktarılması

LUIS sahipliğin aktarılması şu anda desteklemiyor, ancak uygulamanızı dışa aktarabilir ve LUIS kullanıcı başka bir uygulamayı içeri aktarabilirsiniz. LUIS puanları iki uygulama arasındaki küçük farklılıklar olabilir. 

## <a name="azure-active-directory-resources"></a>Azure Active Directory kaynakları

Kullanırsanız [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (Azure AD) LUIS kullanmak istediğinizde, kuruluşunuzda, Language Understanding (LUIS) kullanıcılarınızın erişim hakkında bilgi için izne ihtiyacı vardır. LUIS gerektirdiği kaynakların düşüktür. 

Yönetici onayı sahip veya yönetici onayı gibi yönetici onayı gerektirmeyen bir hesapla oturum açmaya çalıştığında ayrıntılı açıklamasına bakın:

* Uygulamada Kurumsal hesabınızla oturum açın ve profilinizi okuyabilmesine sağlar. Temel şirket bilgilerini okumasına izin verir. Bu LUIS kullanıcı kimliği, e-posta, adı gibi temel profil verileri okuma izni verir
* Uygulamanın bakın ve bile uygulama kullanmakta olduğunuz değil, verilerinizin güncelleştirmesine izin verir. İzin, kullanıcının erişim belirtecini yenilemek için gereklidir.


## <a name="azure-active-directory-tenant-user"></a>Azure Active Directory Kiracı Kullanıcı

LUIS standart Azure Active Directory (Azure AD) onay akışı kullanır. 

Kiracı yönetici, doğrudan Azure AD'de LUIS kullanmak için verilen erişmesi gereken kullanıcı ile çalışması gerekir. 

* İlk olarak, kullanıcı HLUISimzalar ve yönetici onayı gerek açılan iletişim görür. Kullanıcı, Kiracı Yöneticisi devam etmeden önce bağlantı kurar. 
* İkinci olarak, Kiracı yönetici HLUISimzalar ve bir onay akış açılan iletişim görür. Yönetici kullanıcı için izin vermek için gereken iletişim budur. Yönetici izni kabul ettiğinde, kullanıcı ile LUIS devam edebilirsiniz. Kiracı Yöneticisi için LUIS oturum açacak değil, yöneticinin erişebilir [onay](https://account.activedirectory.windowsazure.com/r#/applications) LUIS için aşağıdaki ekran görüntüsünde gösterilen. Listenin adı içeren öğeleri için filtre fark `LUIS`.

![Uygulama Web sitesi tarafından Azure active directory izni](./media/luis-how-to-collaborate/tenant-permissions.png)

Kiracı Yöneticisi yalnızca belirli kullanıcıların LUIS'i kullanmayı isterse, birkaç olası çözümü vardır:
* "Yönetici onayı" verme (onay için tüm kullanıcılar Azure ad), ancak ardından "Kuruluş uygulamanın özellikleri altında"kullanıcı ataması gerekli"Evet" olarak ve son Ata/yalnızca istenen kullanıcılar uygulamaya ekleyin. Bu yöntem ile yönetici uygulamaya yine de "yönetici onayı" veriyor, ancak erişebilecek kullanıcıları denetlenebiliyor mu.
* İkinci bir çözüm olan kullanarak [Azure AD Graph API'si](https://docs.microsoft.com/graph/azuread-identity-access-management-concept-overview) her belirli bir kullanıcıya izin sağlamak için. 

Azure active directory kullanıcıları ve onay hakkında daha fazla bilgi edinin: 
* [Uygulama kısıtlama](../../active-directory/develop/howto-restrict-your-app-to-a-set-of-users.md) kullanıcılara bir dizi

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Ortak Çalışanlar için birden çok e-posta ile kullanıcı hesapları

Bir LUIS uygulaması için ortak çalışanlar eklerseniz, bir ortak çalışanı LUIS ortak çalışan kullanması gereken tam e-posta adresi belirtiyorsunuz. LUIS, Azure Active Directory (Azure AD) tek bir kullanıcı kullanılabileceğinden, birden fazla e-posta hesabına sahip olmasını sağlar, ancak ortak çalışanı'nın listesinde belirtilen e-posta adresiyle oturum açmasını gerektirir.

