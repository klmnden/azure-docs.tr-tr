---
title: Özel geliştirilmiş bir uygulama için izinlerin nasıl | Microsoft Docs
description: Azure AD portalı veya bir URL parametresini kullanarak özel geliştirilmiş uygulamanız için izinlerin nasıl
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 3310a08047700a577c5c6cbada90e575fcd12089
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333713"
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

[İzin ve Azuread'i uygulamalara tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

[İzin ve adının Azuread'i v2.0 için uygulamalar Yakınsanan](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azuread'i StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
