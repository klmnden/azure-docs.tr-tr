---
title: Portalı kullanarak Azure AD v2.0 uç noktası ile bir uygulamayı kaydetme | Microsoft Docs
description: Oturum açma etkinleştiriliyor ve v2.0 uç noktası'nı kullanarak Microsoft hizmetlerine erişmek için Microsoft ile bir uygulamayı kaydetme
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: celested
ms.custom: aaddev
ms.openlocfilehash: 8ab4e6b5b2813a216b6dd6f0fc108a09239ca9a6
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39506552"
---
# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>V2.0 uç noktası ile bir uygulamayı kaydetme
Hem kişisel Microsoft hesabı (MSA) ve iş kabul eden bir uygulama oluşturmak ya da Okul hesabı (Azure AD) oturum açma için önce Microsoft ile bir uygulamayı kaydetme gerekecektir. Şu anda, Azure AD'ye sahip olabileceğiniz tüm mevcut uygulamaları kullanmak mümkün olmayacaktır veya MSA - yeni bir tane oluşturmanız gerekir.

> [!NOTE]
> Tüm Azure Active Directory senaryolarını ve özelliklerini v2.0 uç noktası tarafından desteklenir. V2.0 uç noktası kullanıyorsanız belirlemek için aşağıdaki hakkında bilgi edinin: [v2.0 sınırlamaları](active-directory-v2-limitations.md).


## <a name="visit-the-microsoft-app-registration-portal"></a>Microsoft uygulama kayıt portalı ziyaret edin
İlk olarak, Microsoft uygulama kayıt portalında gidin [ https://apps.dev.microsoft.com/ ](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). 

Kişisel hesabıyla oturum açın, iş veya Okul Microsoft hesabı. Ya da yoksa, yeni bir kişisel hesap için kaydolun.

Bitti mi? Artık Microsoft uygulamaları listesi büyük olasılıkla boş olduğu aramanız. Değiştirelim.

Tıklayın **uygulama ekleme**ve bir ad verin. Portal, uygulamanızı daha sonra kodunuzda kullanacağınız genel benzersiz bir uygulama kimliği atar. Uygulamanız sunucu tarafı bileşeni içeriyorsa, erişim belirteçleri için arama API'leri gerekir (düşünün: Office, Azure veya kendi web API'si), oluşturmak istediğiniz bir **uygulama gizli anahtarı** de burada.

Ardından, ekleme **platformları** uygulamanızı kullanacak.

* Web tabanlı uygulamalar için sağlayan bir **yeniden yönlendirme URI'si** burada oturum açma ileti gönderilebilir.
* Mobil uygulamalarda, URI sizin için otomatik olarak oluşturulan varsayılan yeniden yönlendirme kaydedin.
* Web API'leri için Web API'sine erişmek için bir varsayılan kapsamı otomatik olarak sizin için oluşturulur. Ek kapsamlarla kullanarak eklemeyi seçebilir **kapsamı ekleme** düğmesi. Web API'si kullanarak kullanmak için önceden yetkilendirilmiş uygulamalarda da ekleyebilirsiniz **uygulamalar'önceden yetkilendirilmiş** formu. 

İsteğe bağlı olarak, oturum açma sayfanızda Görünüm ve yapısını özelleştirebilirsiniz **profili** bölümü. Tıkladığınızdan emin olun **Kaydet** geçmeden önce.

> [!NOTE]
> Kullanarak bir uygulama oluşturduğunuzda [ https://apps.dev.microsoft.com/ ](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), uygulamanın giriş kiracısında portalda oturum açmak için kullandığınız hesabın kayıtlı. Bu, bir uygulamayı kişisel bir Microsoft hesabı kullanarak Azure AD kiracınızda kaydedebileceğiniz değil, anlamına gelir. Açıkça bir uygulamayı belirli bir kiracıda kaydetmek istiyorsanız, ilk olarak bu kiracıda oluşturulan bir hesapla oturum açın.


## <a name="build-a-quickstart-app"></a>Bir hızlı başlangıç uygulaması oluşturun
Bir Microsoft uygulaması olduğuna göre v2.0 hızlı başlangıç öğreticileri birini tamamlayabilirsiniz. İşte birkaç öneri:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

