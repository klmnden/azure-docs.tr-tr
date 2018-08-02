---
title: LUIS uygulamalarını azure'da başkalarıyla işbirliği yapma | Microsoft Docs
description: Bilgi Language Understanding (LUIS) uygulamalara başkalarıyla nasıl işbirliği yapılır.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/31/2018
ms.author: diberry
ms.openlocfilehash: 99f37cb6dc5e05fc5eb4bde09685435ee57fecc6
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397797"
---
# <a name="how-to-manage-authors-and-collaborators"></a>Yazarlar ve ortak çalışanlar yönetme 

Başkalarıyla LUIS uygulamanızı birlikte işbirliği yapabilir. 

## <a name="owner-and-collaborators"></a>Sahibi ve ortak çalışanlar

Uygulama sahibi, tek bir yazar vardır ancak birçok ortak çalışanlar çıkarabilirsiniz. 

## <a name="add-collaborator"></a>Ortak çalışanı Ekle

LUIS uygulamanızı düzenlemek ortak çalışanlar izin vermek için **ayarları** sayfa LUIS uygulamanızı tıklatın ve ortak çalışan e-posta girin **Ekle ortak çalışanı**. Ortak Çalışanlar, oturum açın ve uygulama üzerinde çalıştığınız aynı anda LUIS uygulamanızı düzenleyin.

![Ortak çalışanı Ekle](./media/luis-how-to-collaborate/add-collaborator.png)

## <a name="transfer-of-ownership"></a>Sahipliğin aktarılması

LUIS sahipliğin aktarılması şu anda desteklemiyor, ancak uygulamanızı dışa aktarabilir ve LUIS kullanıcı başka bir uygulamayı içeri aktarabilirsiniz. LUIS puanları iki uygulama arasındaki küçük farklılıklar olabilir. 

## <a name="azure-active-directory-tenant-user"></a>Azure Active Directory Kiracı Kullanıcı

LUIS standart Azure Active Directory (Azure AD) onay akışı kullanır. 

Kiracı yönetici, doğrudan Azure AD'de LUIS kullanmak için verilen erişmesi gereken kullanıcı ile çalışması gerekir. 

İlk olarak, kullanıcı HLUISimzalar ve yönetici onayı gerek açılan iletişim görür. Kullanıcı, Kiracı Yöneticisi devam etmeden önce bağlantı kurar. 

İkinci olarak, Kiracı yönetici HLUISimzalar ve bir onay akış açılan iletişim görür. Yönetici kullanıcı için izin vermek için gereken iletişim budur. Yönetici izni kabul ettiğinde, kullanıcı ile LUIS devam edebilirsiniz.

Kiracı yönetici LUIS için oturum açacak değil, yönetici erişebilir [onayı](https://account.activedirectory.windowsazure.com/r#/applications) LUIS için. 

![Uygulama Web sitesi tarafından Azure active directory izni](./media/luis-how-to-account-settings/tenant-permissions.png)

Kiracı yönetici yalnızca belirli kullanıcıların LUIS kullanmasını isterse, bunun için başvuruda [kimliği blogu](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/).

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Ortak Çalışanlar için birden çok e-posta ile kullanıcı hesapları

Bir LUIS uygulaması için ortak çalışanlar eklerseniz, bir ortak çalışanı LUIS ortak çalışan kullanması gereken tam e-posta adresi belirtiyorsunuz. LUIS, Azure Active Directory (Azure AD) tek bir kullanıcı kullanılabileceğinden, birden fazla e-posta hesabına sahip olmasını sağlar, ancak ortak çalışanı'nın listesinde belirtilen e-posta adresiyle oturum açmasını gerektirir.