---
title: "Özel geliştirilmiş bir uygulama için izinlerin nasıl | Microsoft Docs"
description: "Azure AD portalı veya bir URL parametresini kullanarak özel geliştirilmiş uygulamanız için izinlerin nasıl"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 336b945929f80e1a566f7cf71b40fd799a98c12d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-grant-permissions-to-a-custom-developed-application"></a>Özel geliştirilmiş bir uygulama için izinlerin nasıl

Uygulamanızın üzerinde erken önlem izin vermek istediğiniz veya değil rıza bir hata bir uygulama çalıştırıyorsanız, aşağıdaki adımları deneyin.

## <a name="how-to-perform-admin-consent-for-your-application"></a>Uygulamanız için yönetici onayı gerçekleştirme

Bu, kuruluşunuzdaki tüm kullanıcılar için uygulama izin verme etkisi yoktur.

1. Gidin **uygulama kayıtlar** dikey olarak bir **genel yönetici**, uygulamayı seçin.

2. Seçin **gerekli izinler**ve son olarak isabet **izinler** dikey pencerenin üstündeki düğmesi.

Alternatif olarak, bir istek oluşturabileceğiniz *login.microsoftonline.com* uygulama yapılandırmaları ile ve üzerinde ilave *& komut istemi Yönetim =\_onayı*. Yönetici kimlik bilgileriyle oturum sonra uygulamayı tüm kullanıcılar için izin verildi.

## <a name="how-to-force-user-consent-for-your-application"></a>Uygulamanız için kullanıcı onayı zorlama

* Kimlik doğrulama istekleri ekleme *& sor onay =* son kullanıcıların kimlik doğrulaması her zaman onay gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

[İzin ve Azuread'i uygulamalara tümleştirme](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

[İzin ve adının Azuread'i v2.0 için uygulamalar Yakınsanan](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azuread'i StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
