---
title: "Azure AD v2.0 uç Portalı'nı kullanarak bir uygulamayı kaydetme | Microsoft Docs"
description: "Bir uygulama oturum açma etkinleştirme ve v2.0 uç noktası kullanarak Microsoft hizmetlerine erişmek için Microsoft ile kaydetme"
services: active-directory
documentationcenter: 
author: lnalepa
manager: mtillman
editor: 
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: eba8ecd27542b23676c08b8ce072c91134d27fa5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Bir uygulamanın v2.0 uç noktası ile kaydetme
MSA & Azure AD kabul eden bir uygulama oluşturmak için oturum açma, önce bir uygulama Microsoft ile kaydetmeniz gerekir.  Şu anda, Azure AD ile olabilecek mevcut uygulamalardan kullanmanız mümkün olmayacaktır veya MSA - yepyeni bir tane oluşturmanız gerekir.

> [!NOTE]
> Tüm Azure Active Directory senaryolarını ve özelliklerini v2.0 uç noktası tarafından desteklenir.  V2.0 uç kullanmanızın gerekli olup olmadığını belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
> 
> 

## <a name="visit-the-microsoft-app-registration-portal"></a>Microsoft uygulama kayıt Portalı'nı ziyaret edin
İlk şey ilk - gidin [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Microsoft uygulamalarınızda yönetebileceği yeni bir uygulama kayıt portalı budur.

Oturum ya da kişisel oturum, iş veya Okul Microsoft hesabı.  Ya da yoksa, yeni bir kişisel hesap için kaydolun. Şimdi, uzun sürmez - biz burada beklemeniz.

Bitti mi? Şimdi Microsoft uygulamaları listesi büyük olasılıkla boş olduğu aramanız.  Değiştirelim.

Tıklatın **bir uygulama ekleyin**ve bir ad verin.  Portal uygulamanızı daha sonra kodunuzda kullanacağınız genel benzersiz bir uygulama kimliği atar.  Uygulamanızı bir sunucu tarafı bileşeni içeriyorsa, erişim belirteçleri için arama API'leri gerekir (düşünün: Office, Azure veya web API), oluşturmak istersiniz bir **uygulama gizli anahtarı** burada da.

Ardından, uygulamanızı kullanacağı platformları ekleyin.

* Web tabanlı uygulamalar için bir **yeniden yönlendirme URI'si** burada oturum açma iletiler gönderilemez.
* Mobil uygulamalarda, sizin için otomatik olarak oluşturulan varsayılan yeniden yönlendirme URI'si aşağı kopyalayın.

İsteğe bağlı olarak, oturum açma sayfanızda profil bölümü görünümünü özelleştirebilirsiniz.  Tıklattığınızdan emin olun **kaydetmek** geçmeden önce.

> [!NOTE]
> Kullanarak bir uygulama oluşturduğunuzda [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), uygulama giriş Kiracı Portalı'na imzalamak için kullandığınız hesabın içinde kayıtlı.  Bu, bir uygulamayı kişisel bir Microsoft hesabı kullanarak, Azure AD kiracınızda kaydedebilirsiniz değil, anlamına gelir.  Açıkça belirli bir kiracı içinde bir uygulamayı kaydetmek istiyorsanız, başlangıçta Kiracı içinde oluşturulan bir hesapla oturum açın.
> 
> 

## <a name="build-a-quick-start-app"></a>Hızlı Başlangıç uygulaması oluşturma
Bir Microsoft uygulaması sahip olduğunuza göre v2.0 hızlı başlangıç öğreticilerimizden birini tamamlayabilirsiniz.  İşte birkaç öneri:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

