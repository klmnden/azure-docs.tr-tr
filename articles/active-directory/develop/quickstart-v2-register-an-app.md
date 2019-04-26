---
title: Azure AD v2.0 uç noktasıyla uygulama kaydetme | Microsoft Docs
description: Oturum açma ve Azure AD v2.0 uç noktasını kullanarak Microsoft hizmetlerine erişmeyi etkinleştirmek üzere bir uygulamayı kaydetmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/02/2018
ms.author: celested
ms.reviewer: lenalepa
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6221fb7575fc267030929b449cba48cff8563f27
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60298793"
---
# <a name="quickstart-register-an-app-with-the-azure-active-directory-v20-endpoint"></a>Hızlı Başlangıç: Azure Active Directory v2.0 uç noktası ile bir uygulamayı kaydetme

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Hem kişisel Microsoft hesabı (MSA) hem de iş veya okul hesabı (Azure AD) oturum açmalarını kabul eden bir uygulama geliştirmek için Azure Active Directory (Azure AD) v2.0 uç noktasıyla bir uygulamayı kaydetmeniz gerekmektedir. Şu anda Azure AD veya MSA ile sahip olduğunuz mevcut uygulamaları kullanamayacaksınız. Tamamen yeni bir uygulama oluşturmanız gerekir.

Bazı Azure AD senaryoları ve özellikleri v2.0 uç noktasında desteklenmez. v2.0 uç noktasını kullanmanızın gerekip gerekmediğini belirlemek için [v2.0 sınırlamaları](active-directory-v2-limitations.md) bölümünü okuyun.

> [!NOTE]
> Yeni bir uygulama mı kaydediyorsunuz? Azure portalında yeni **Uygulama kayıtları (Önizleme)** deneyimini kullanın. Başlamak için bkz. [Uygulama kaydetme (Önizleme)](quickstart-register-app.md).

## <a name="step-1-sign-in-to-the-microsoft-application-registration-portal"></a>1. Adım: Microsoft uygulama kayıt portalında oturum açın

1. [https://apps.dev.microsoft.com/](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) adresinde Microsoft uygulama kayıt portalına gidin.
1. Kişisel veya iş veya okul Microsoft hesabıyla oturum açın. Bunlardan herhangi birisi yoksa yeni bir kişisel hesap açın.
1. Açtınız mı? Şimdi muhtemelen boş olan bir Microsoft uygulamaları listenizi görüyor olmanız gerekir. Şimdi bunu değiştirelim.

## <a name="step-2-register-an-app"></a>2. Adım: Bir uygulamayı kaydetme

1. **Uygulama ekle**’yi seçin ve buna bir ad verin.
    Portal, uygulamanıza sonradan kodunuzda kullanacağınız genelde benzersiz bir Uygulama Kimliği atar. Uygulamanız sunucu tarafı bileşeni içeriyorsa, erişim belirteçleri için arama API'leri gerekir (düşünün: Office, Azure veya kendi web API'si), oluşturmak istediğiniz bir **uygulama gizli anahtarı** de burada.
1. Ardından uygulamanızın kullanacağı **Platformları** ekleyin.
    * Web tabanlı uygulamalar için oturum açma iletilerinin gönderilebildiği bir **Yönlendirme URI’si** sağlayın.
    * Mobil uygulamalar için, sizin için otomatik olarak oluşturulan varsayılan yeniden yönlendirme URI'sini kopyalayın.
    * Web API’leri için Web API’sine erişmeye yönelik bir varsayılan kapsam sizin için otomatik olarak oluşturulmuştur.
        **Kapsam Ekle** düğmesini kullanarak iki ek kapsam ekleyebilirsiniz. Ayrıca **Önceden yetkilendirilmiş uygulamalar** formunu kullanarak Web API’nizi kullanmak için önceden yetkilendirilmiş herhangi bir uygulama ekleyebilirsiniz.
1. İsteğe bağlı olarak **Profil** bölümünden oturum sayfanızın görünümünü özelleştirin. 
1. Devam etmeden önce değişikliklerinizi **kaydedin**.

> [!NOTE]
> Bir uygulamayı [https://apps.dev.microsoft.com/](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) adresini kullanarak kaydettiğinizde uygulama, portalda oturum açmak için kullandığınız hesabın ana kiracısına kaydedilir. Bu, bir uygulamayı kişisel bir Microsoft hesabı kullanarak Azure AD kiracınıza kaydedemeyeceğiniz anlamına gelir. Bir uygulamayı açık bir şekilde belirli bir kiracıya kaydetmek istiyorsanız ilk olarak o kiracıda oluşturulan bir hesapla oturum açın.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi bir Microsoft uygulamanız olduğuna göre v2.0 quickstart2’den birini tamamlayabilirsiniz. İşte birkaç öneri:

[!INCLUDE [active-directory-v2-quickstart-table](~/includes/active-directory-v2-quickstart-table.md)]
