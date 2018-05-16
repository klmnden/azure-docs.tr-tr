---
title: Azure AD v2.0 uç Portalı'nı kullanarak bir uygulamayı kaydetme | Microsoft Docs
description: Bir uygulama oturum açma etkinleştirme ve v2.0 uç noktası kullanarak Microsoft hizmetlerine erişmek için Microsoft ile kaydetme
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
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Bir uygulamanın v2.0 uç noktası ile kaydetme
Hem kişisel Microsoft hesabı (MSA) & iş kabul eden bir uygulama oluşturmak veya Okul hesabı (Azure AD) oturum açma için önce bir uygulama Microsoft ile kaydetmeniz gerekir. Şu anda, Azure AD ile olabilecek mevcut uygulamalardan kullanmanız mümkün olmayacaktır veya MSA - yepyeni bir tane oluşturmanız gerekir.

> [!NOTE]
> Tüm Azure Active Directory senaryolarını ve özelliklerini v2.0 uç noktası tarafından desteklenir. V2.0 uç kullanmanızın gerekli olup olmadığını belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).


## <a name="visit-the-microsoft-app-registration-portal"></a>Microsoft uygulama kayıt Portalı'nı ziyaret edin
İlk olarak, adresindeki Microsoft uygulama kayıt portalına gitmek [ https://apps.dev.microsoft.com/ ](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). 

Oturum ya da kişisel oturum, iş veya Okul Microsoft hesabı. Ya da yoksa, yeni bir kişisel hesap için kaydolun.

Bitti mi? Şimdi Microsoft uygulamaları listesi büyük olasılıkla boş olduğu aramanız. Değiştirelim.

Tıklatın **bir uygulama ekleyin**ve bir ad verin. Portal uygulamanızı daha sonra kodunuzda kullanacağınız genel benzersiz bir uygulama kimliği atar. Uygulamanızı bir sunucu tarafı bileşeni içeriyorsa, erişim belirteçleri için arama API'leri gerekir (düşünün: Office, Azure veya web API), oluşturmak istersiniz bir **uygulama gizli anahtarı** burada da.

Ardından, eklemek **platformları** uygulamanızı kullanacak.

* Web tabanlı uygulamalar için bir **yeniden yönlendirme URI'si** burada oturum açma iletiler gönderilemez.
* Mobil uygulamalarda, URI sizin için otomatik olarak oluşturulan varsayılan redirect aşağı kopyalayın.
* Web API'leri Web API erişmek için bir varsayılan kapsamı otomatik olarak oluşturulur. Ek kapsamlarla kullanarak eklemek için seçebileceğiniz **Add Scope** düğmesi. Web API kullanarak kullanmak için önceden yetkilendirilmesini herhangi bir uygulama da ekleyebilirsiniz **uygulamaları'önceden yetkili** formu. 

İsteğe bağlı olarak, oturum açma sayfanızda görünümünü özelleştirebilirsiniz **profil** bölümü. Tıklattığınızdan emin olun **kaydetmek** geçmeden önce.

> [!NOTE]
> Kullanarak bir uygulama oluşturduğunuzda [ https://apps.dev.microsoft.com/ ](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), uygulama giriş Kiracı Portalı'na imzalamak için kullandığınız hesabın içinde kayıtlı. Bu, bir uygulamayı kişisel bir Microsoft hesabı kullanarak, Azure AD kiracınızda kaydedebilirsiniz değil, anlamına gelir. Açıkça belirli bir kiracı içinde bir uygulamayı kaydetmek istiyorsanız, başlangıçta Kiracı içinde oluşturulan bir hesapla oturum açın.


## <a name="build-a-quickstart-app"></a>Hızlı Başlangıç uygulaması oluşturma
Bir Microsoft uygulaması sahip olduğunuza göre v2.0 hızlı başlangıç öğreticileri birini tamamlayabilirsiniz. İşte birkaç öneri:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

